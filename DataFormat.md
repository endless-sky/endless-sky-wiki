# Choosing a text editor

For editing the data files, make sure you have a text editor that:

* Saves files with Unix-style line endings. (That is, don't use Notepad.)
* Does not convert straight quotes `"..."` into curly quotes `“...”`.
* Inserts a tab character, rather than some number of spaces, when you press the TAB key.

Some text editors support an [EditorConfig](https://editorconfig.org/) file, and will read settings from the file provided in this repository. This can help ensure your contributions match the format of existing data files.

# Data file format

A data file consists of any number of lines, each of which can contain any number of items or "tokens." Tokens are separated by spaces. For example, this is a line with four tokens in it:

    this is a test

If you want to enter a single token that contains spaces, it must be enclosed in double quotation marks `"..."` to signify that is is a single token. For example, this line contains two tokens:

    text "This is a test."

If the text you are trying to enter contains double quotation marks *and* spaces, you can enclose it in backticks `` `...` `` rather than quotation marks:

    text `He said, "Hello!"`

Entries in data files are [tree structures](https://en.wikipedia.org/wiki/Tree_structure), where each non-empty line of the file is a "node" of the tree and indentation is used to represent its structure:

    "root node"
        "first child node"
        "second child node"
            "child of second child node"
        "third child node"

That is, each data element is presented in the form of a [hierarchical outline](https://en.wikipedia.org/wiki/Outline_(list)). In the hierarchy, a sub-item is called a "child," and the item that contains it is its "parent." Two nodes that have the same parent are called "siblings."

Unless otherwise noted, the order in which the lines in a particular data element appears is not important. For example, this:

    object
        pos 1.23 4.56
        name "Test"

is completely equivalent to this:

    object
        name "Test"
        pos 1.23 4.56

# Grammar specifications

In various other wiki pages, I describe the syntax of data files using notation that is similar to how command line arguments are [specified](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html) in Unix man pages:

* `plain text` specified keywords that appear verbatim in the file.
* `<text in angle brackets>` should be replaced with a particular value:
  * `pos <x#> <y#>` means you could enter something like `pos 1.23 4.56` or `pos 1000 2000`.
    * A "#" sign means the value is expected to be a number.
  * `name <name>` means you could enter something like `name Example` or `name "A Longer Name"`.
* `vertical | bars` represent options you choose between:
  * `on (offer | accept | decline)` means `on offer` or `on accept` or `on decline`.
* `[square brackets]` denote optional arguments, and can be nested:
  * `item <name> [<count>]` means the count is optional (e.g. defaulting to 1 if left out).
  * `range [<min> [<mean>]] <max>` means `range <max>` or `range <min> <max>` or `range <min> <mean> <max>`.
* `ellipses...` mean an argument can appear more than once.
  * `<value>...` means a list of 1 or more values.
  * `[<value>...]` means a list of 0 or more values.
  * `...` on a line by itself means any number of lines can be included here (following the grammar given in previous lines).
