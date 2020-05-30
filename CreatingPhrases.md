# Introduction

The [syntax](DataFormat#grammar-specifications) for the definition of a phrase is:

```html
phrase <name>
	word
		<text>
		"<text> ${<phrase>} <text>"
		...
	...
	phrase
		<phrase>
		...
	...
	replace
		<text> <replacement>
		...
```

Phrases construct sentences according to the words, phrases, and replacements that make up their definition.

# Basic phrase characteristics

```html
phrase <name>
```

Unlike some other root nodes, the name of a phrase does not need to be unique. Multiple phrases are capable of sharing the same name, and whenever the phrase name is referred to (such as for use in a ship name or hail), each of the phrases with the same name has an equal chance of being chosen to have a sentence constructed from it.

```html
word
	<text>
	"<text> ${<phrase>} <text>"
	...
...
```

A phrase can contain multiple word blocks. Words are lists of text, and a single line from each word will be chosen. Each line of text in a word has an equal chance of being chosen.

The `${<phrase>}` construction can be used to pull a sentence from another phrase and place it in the middle of a line of text.

```html
phrase
	<phrase>
	...
...
```

A phrase can also contain multiple phrases. Each phrase block is a list of other phrases that can be pulled from. As with words, each phrase in a phrase block has an equal chance of being chosen.

Words and phrases of a phrase can go in any order; one does not need to specify what words a phrase has and then what phrases it has. The order of the words and phrases defines the order in which their pieces are put together.

```html
replace
	<text> <replacement>
	...
```

The replace block is used after a sentence has been constructed from a phrase. If there are any instances of the text on the left, then that is replaced with the text on the right.

# Examples

### Multiple phrases of the same name

```
phrase "Ship Names"
	word
		"Santa Maria"
		"Pinta"

phrase "Ship Names"
	word
		"Enterprise"
		"Millennium Falcon"
```

Calling for the "Ship Names" phrase will potentially return Santa Maria, Pinta, Enterprise, and Millennium Falcon, instead of overwriting the "ship names" phrase with the last defined node or ignoring duplicate phrases of the same name, as some other nodes do.

### Multiple words in a phrase

```
phrase "Greetings"
	word
		"Hello there!"
		"Hi."
	word
		"Are you doing well?"
		"Wonderful weather we're having."
```

The above phrase will choose either "Hello there!" or "Hi.", and then choose either "Are you doing well?" or "Wonderful weather we're having." This means that the possible outcomes of this phrase are

```
Hello there! Are you doing well?
Hello there! Wonderful weather we're having.
Hi. Are you doing well?
Hi. Wonderful weather we're having.
```

### Mixed words and phrases

```
phrase "Threat"
	word
		"You better run, you"
		"Get out of my way you"
	word
		" "
	phrase
		"Shakespearean Insult"
	word
		"! "
	word
		"Don't you know who I am?"
		"You'll regret it if you don't."
	
phrase "Shakespearean Insult"
	word
		"artless"
		"bawdy"
	word
		"fat-kidneyed"
		"fen-sucked"
	word
		"nut-hook"
		"pigeon-egg"
```

The above construction will yield some threatening phrase that includes a "Shakespearean insult," such as "Get out of my way you bawdy fen-sucked pigeon-egg! Don't you know who I am?" Separating the Shakespearean insults into their own phrase in this way makes it so that they can be used in other phrases. This construction also allows us to create non-Shakespearean insult phrases and place them alongside the Shakespearean insults in the "Threat" phase.

### Using `${<phrase>}` to place phrases within words

Say we want a pirate to be able to offer to buy or sell some contraband. We might create something like the following.
```
phrase "buy and sell"
	word
		"Would you like to "
	word
		"buy "
		"sell "
	phrase
		"contraband"
	word
		" from me?"
		" to me?
```
	
The issue with this construction is that it yields nonsensical results like "Would you like to buy `<contraband>` to me?" So in scenarios where we have two word blocks separated by a phrase in the middle where we want the outcome of one word to determine the outcome of the other word, we'd instead call the phrase from within the words.

```
phrase "buy and sell"
	word
		"Would you like to "
	word
		"buy ${contraband} from"
		"sell ${contraband} to"
	word
		" me?"
```

With this, whenever "buy" is chosen, the word following the contraband will always be "from."

### Using the replace function

Take the following phrase.

```
phrase "Syndicate Suspicion"
	word
		"I suspect that "
	word
		"the CEO of the Syndicate"
		"the leaders of the Syndicate"
	word
		" "
	word
		"don't have our best interests at heart."
		"try to do right by people."
```

This phrase can generate sensical sentences like "I suspect that the leaders of the Syndicate don't have out best interests at heart." But it can also generate nonsensical ones: "I suspect the CEO of the Syndicate try to do right by people." Instead of having to create an entirely separate phrase to handle how "don't" and "try" should change if the subject of the previous word is singular, we can use the replace function to change these verbs to their singular forms.

```
phrase "Syndicate Suspicion"
	word
		"I suspect that "
	word
		"the CEO of the Syndicate%"
		"the leaders of the Syndicate"
	word
		" "
	word
		"don't have our best interests at heart."
		"try to do right by people."
	replace
		"% don't" " doesn't"
		"% try" " tries"
		"%" ""
```

Now, if this sentence generates "I suspect the CEO of the Syndicate% try to do right by people.", the "% try" will be replaced with " tries", creating the sensical sentence "I suspect the CEO of the Syndicate tries to do right by people."

Notice how a dummy character is used to figure out if a replacement should occur. If this dummy character were not used, then instead the replace function would need to look for "CEO of the Syndicate don't" or "CEO of the Syndicate try" to do a replacement. While in this case that would be perfectly valid, in the case of having more subjects or more predicates, this dummy character becomes a necessity, lest the replace block grow unnecessarily large.