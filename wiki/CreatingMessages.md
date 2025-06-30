# Table of Contents

* [Introduction](#introduction)
* [Messages](#messages)
* [Message Categories](#message-categories)

# Introduction

The [syntax](DataFormat#grammar-specifications) for the definition of a message or message category is:

```html
message <name>
	text <text>
	phrase <name>
	category <name>

"message category" <name>
	"main color" (<r#> <g#> <b#> | <name>)
	"log color" (<r#> <g#> <b#> | <name>)
	"aggressive deduplication"
	"no log deduplication"
	"important"
	"log only"
```

# Messages

```html
message <name>
```

A "message" entry is the definition of a hail-like message that can be sent by the game's engine, appearing in the scrolling list at the bottom of the main view, as well as in a special log panel.

```html
text <text>
phrase <name>
```

The contents of the message can be loaded either from a text node, or a named [phrase](CreatingPhrases). [Substitutions](CreatingSubstitutions) (including replacing UI actions with their assigned keys) are supported.

```html
category <name>
```

The [category](#message-categories) of this message. If none is provided, the default category named "normal" will be used.

# Message Categories

```html
"message category" <name>
```

A category defines how messages assigned to it appear and behave.

```html
"main color" (<r#> <g#> <b#> | <name>)
"log color" (<r#> <g#> <b#> | <name>)
```

The colors used to display the entries in the main view and the log panel. They can be either named colors, or inline RGB definitions.

```html
"aggressive deduplication"
"no log deduplication"
"important"
"log only"
```

The attributes controlling the behavior of the sent message entries:

 - `"aggressive deduplication"`: a message won't appear in the main view if there already is another entry with the same text in the list. Without this flag, when a message is added, all entries with the same text will be removed from the list, essentially merging the messages at the bottom of the list. This attribute has no effect on the log view.
 - `"no log deduplication"`: when two or more messages with the same text are posted subsequently, they all get written to the log. This attribute has no effect on the main view.
 - `"important"`: messages with this category are included in the filtered view of the log that shows only messages of high importance. **This flag is mainly intended to be used in the engine, and should not be overused.** This attribute has no effect on the main view.
 - `"log only"`: messages won't appear in the main list, only in the log view.
