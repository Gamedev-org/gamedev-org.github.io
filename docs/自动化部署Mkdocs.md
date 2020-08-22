Github Actions 可以很方便实现 CI/CD 工作流，下面是细化的步骤记录👇

# 前提
## 部署访问密钥
- 在本地生成一组公钥和私钥
```shell script
$ ssh-keygen -f github-deploy-key
```
- 将公钥部署在Repo的`Settings -> Deploy keys -> Add deploy key`，记住设置的key name: `GH_ACTION_DEPLOY_KEY`，后面`GH_ACTION_DEPLOY_KEY: ${{ secrets.GH_ACTION_DEPLOY_KEY }}`会使用到
- 将私钥部署在Repo的`Settings -> Secrets -> Add a new secret`
## 创建用于pages展示的分支
在`master`分支的基础上创建一个空目录分支，取名为`gh-pages`

# 部署和发布
## 编写GitHub Action
点击Repo的Actions链接，在线生成yaml模板文件，并使用下面的脚本文件替换所有内容👇，commit即在线部署成功，以后在`master`分支有`push`或者`pull_request`的请求时会自动触发该Action操作脚本。
```yaml
# This is a basic workflow to help you get started with Actions

name: CI

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the master branch
on:
  push:
    branches:
      - master
  pull_request:
    branches:
      - master

env:
  PULL_BRANCH: master
  PUSH_BRANCH: gh-pages
  GIT_USER: longshilin
  GIT_EMAIL: longshilin@users.noreply.github.com
  GIT_ADDRESS: git@github.com:Gamedev-org/gamedev-org.github.io.git

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest
    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
    # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
    - uses: actions/checkout@v2
    # Python & Pip Installer
    - uses: actions/setup-python@v1
    - uses: dschep/install-pipenv-action@v1
    # prepare build env
    - name: prepare build env
      env:
        GH_ACTION_DEPLOY_KEY: ${{ secrets.GH_ACTION_DEPLOY_KEY }}
      run: |
        mkdir -p ~/.ssh/
        echo "$GH_ACTION_DEPLOY_KEY" > ~/.ssh/id_rsa
        chmod 600 ~/.ssh/id_rsa
        ssh-keyscan github.com >> ~/.ssh/known_hosts
        git config --global user.name ${GIT_USER}
        git config --global user.email ${GIT_EMAIL}
        pip install mkdocs
        pip install mkdocs-bootswatch
    # build and deploy
    - name: build 
      run: |
        git clone --branch ${PULL_BRANCH} ${GIT_ADDRESS} ${PULL_BRANCH};
        cd ${PULL_BRANCH}; mkdocs build; cd ..;
        git clone --branch ${PUSH_BRANCH} ${GIT_ADDRESS} ${PUSH_BRANCH}; 
        # mv -f ${PULL_BRANCH}/site/* ${PUSH_BRANCH};
        (cd ${PULL_BRANCH}/site && tar c .) | (cd ${PUSH_BRANCH} && tar xf -)
        cd ${PUSH_BRANCH}; git add *; git commit -a -m "Site updated：`date`"; git push -f;

```
## 发布
在Repo的`Settings -> GitHub Pages`中设置部署的分支为`gh-pages`，并查看静态网站，大功告成～