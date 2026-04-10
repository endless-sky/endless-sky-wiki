# Table of Contents

* [Introduction](#introduction)
* [Messages](#messages)
* [Message Categories](#message-categories)

# Introduction

Beginning in **v. 0.11.0**, messages that were previously hardcoded into the game engine can be customized. The [syntax](DataFormat#grammar-specifications) for the definition of a message or message category is:

```html
message <name>
	text <text>
	phrase <name>
	category <name>

"message category" <name>
	"main color" (<r#> <g#> <b#> | <name>)
	"log color" (<r#> <g#> <b#> | <name>)
	"main duplicates" <strategy>
	"log duplicates" <strategy>
	"important" [true | false]
	"log only" [true | false]
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
"main duplicates" <strategy>
"log duplicates" <strategy>
"important" [true | false]
"log only" [true | false]
```

The attributes controlling the behavior of the sent message entries:

* `"main duplicates"` and `"log duplicates"`: what the game should do when it encounters a duplicate entry. The main view considers a message as duplicate if it matches any of the previous messages in the list, while the log only compares the last entry. `<strategy>` can be either `"keep new"` (default for the main view, unsupported for the log), `"keep old"` (default for the log), or `"keep both"`.
* `"important"`: messages with this category are included in the filtered view of the log that shows only messages of high importance. **This flag is mainly intended to be used in the engine, and should not be overused.** This attribute has no effect on the main view.
* `"log only"`: messages won't appear in the main list, only in the log view.
