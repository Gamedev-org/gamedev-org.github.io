# Welcome to MkDocs

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

## Tables
 The tables extension adds a basic table syntax to Markdown which is popular across multiple implementations. The syntax is rather simple and is generally only useful for simple tabular data.
 
 A simple table looks like this:
 
```
 First Header | Second Header | Third Header
 ------------ | ------------- | ------------
 Content Cell | Content Cell  | Content Cell
 Content Cell | Content Cell  | Content Cell
```
If you wish, you can add a leading and tailing pipe to each line of the table:
```
 | First Header | Second Header | Third Header |
 | ------------ | ------------- | ------------ |
 | Content Cell | Content Cell  | Content Cell |
 | Content Cell | Content Cell  | Content Cell |
```
Specify alignment for each column by adding colons to separator lines:
```
 First Header | Second Header | Third Header
 :----------- |:-------------:| -----------:
 Left         | Center        | Right
 Left         | Center        | Right
```
Note that table cells cannot contain any block level elements and cannot contain multiple lines of text. They can, however, include inline Markdown as defined in Markdown's syntax rules.
 
 Additionally, a table must be surrounded by blank lines. There must be a blank line before and after the table.
 
## Fenced code blocks
The fenced code blocks extension adds an alternate method of defining code blocks without indentation.

The first line should contain 3 or more backtick (```) characters, and the last line should contain the same number of backtick characters (```):

```
 Fenced code blocks are like Standard
 Markdown’s regular code blocks, except that
 they’re not indented and instead rely on
 start and end fence lines to delimit the
 code block.
```
 
With this approach, the language can optionally be specified on the first line after the backticks which informs any syntax highlighters of the language used:
 
```python
 def fn():
     pass
```
 
Note that fenced code blocks can not be indented. Therefore, they cannot be nested inside list items, blockquotes, etc.

Ref: https://www.mkdocs.org/user-guide/writing-your-docs/#fenced-code-blocks