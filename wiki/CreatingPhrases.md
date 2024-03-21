# Introduction

The [syntax](DataFormat#grammar-specifications) for the definition of a phrase is:

```html
phrase <name>
	word
		<text> [<weight#>]
		"<text> ${<phrase>} <text>" [<weight#>]
		...
	...
	phrase
		<phrase> [<weight#>]
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
	<text> [<weight#>]
	"<text> ${<phrase>} <text>" [<weight#>]
	...
...
```

A phrase can contain multiple word blocks. Words are lists of text, and a single line from each word will be chosen. Each line of text in a word has an equal chance of being chosen.

The `${<phrase>}` construction can be used to pull a sentence from another phrase and place it in the middle of a line of text.

**Beginning in v. 0.9.15,** each choice of a word can be given an optional "weight." Weights are integer values that function similarly to the weights of [fleet variants](CreatingFleets). That is, the sum of the weights of all choices is calculated, and the chances of any single choice being taken is that choice's weight divided by the sum of weights. If a weight is not provided then the choice has a default weight of 1. This can be used to influence the probability of generating a certain outcome from a phrase.

```html
phrase
	<phrase> [<weight#>]
	...
...
```

A phrase can also contain multiple phrases. Each phrase block is a list of other phrases that can be pulled from. As with words, each phrase in a phrase block has an equal chance of being chosen.

Words and phrases of a phrase can go in any order; one does not need to specify what words a phrase has and then what phrases it has. The order of the words and phrases defines the order in which their pieces are put together.

Phrase weights also work identically to word weights.

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

### Using weights to influence outcomes

The following two phrases are examples from the game of phrases used for generating pirate names.

```
phrase "pirate"
	phrase
		"bad outcomes"
		"pirate adjectives that work as titles"
		"pirate adjectives that don't work as titles"
	word
		" "
	phrase
		"pirate nouns that work after bad outcomes"

phrase "pirate"
	word
		"Three Skeleton Key"
		"Ampoliros"
		"Demeter"
		"Antonia Graza"
		"Mortzestus"
```

As described above, given that these two phrases have the same name, each phrase definition can be chosen to generate a name from. Which phrase gets chosen is random, though, with each phrase having an equal chance of being chosen. Note though how the first phrase generates a name by calling on a number of subphrases, whereas the second phrase generates a name from a list of five words. This means that the first phrase has more potential outcomes than the second. Many more, in fact; in this case, the first phrase has over 4,000 potential names it can generate compared to the second phrase's 5. Given that each phrase is equally likely to be chosen, though, the probability of seeing one of the 5 names from the second phrase is far higher than seeing any single name from the first phrase.

This can be remedied through the use of weights. 

```
phrase "pirate"
	phrase
		"pirate set 1" 4482 # 4482 possible names can come from this set
		"pirate set 2" 5 # 5 possible names can come from this set

phrase "pirate set 1"
	phrase
		"bad outcomes" 50 # 50 words in this phrase
		"pirate adjectives that work as titles" 50 # etc.
		"pirate adjectives that don't work as titles" 66
	word
		" "
	phrase
		# Technically unnecessary to weight this, but if there were more
		# phrases listed here then you'd need this.
		"pirate nouns that work after bad outcomes" 27

phrase "pirate set 2"
	word
		"Three Skeleton Key"
		"Ampoliros"
		"Demeter"
		"Antonia Graza"
		"Mortzestus"
```

The above example has restructured the pirate phrase such that only one pirate phrase is defined, but it generates a name by calling from two "pirate set" phrases, which made up the pirate phrases in the previous example. The pirate phrase then weights each of the set phrases according to the number of names that each can generate. Therefore, instead of being a 50/50 chance to choose one or the other, the first phrase is now weighted more heavily such that each potential name has an equal chance of being generated (although one could easily weight a phrase such that names have a higher than equal chance of being generated). Similarly, the first pirate set has also weighted the phrases that it calls upon to equalize the outcomes that it could generate.

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

This phrase can generate grammatically correct sentences like "I suspect that the leaders of the Syndicate don't have our best interests at heart." But it can also generate nonsensical ones: "I suspect the CEO of the Syndicate try to do right by people." Instead of having to create an entirely separate phrase to handle how "don't" and "try" should change if the subject of the previous word is singular, we can use the replace function to change these verbs to their singular forms.

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

Now, if this sentence generates "I suspect the CEO of the Syndicate% try to do right by people.", the "% try" will be replaced with " tries", creating the sentence "I suspect the CEO of the Syndicate tries to do right by people."

Notice how a dummy character is used to figure out if a replacement should occur. If this dummy character were not used, then instead the replace function would need to look for "CEO of the Syndicate don't" or "CEO of the Syndicate try" to do a replacement. While in this case that would be perfectly valid, in the case of having more subjects or more predicates, this dummy character becomes a necessity, lest the replace block grow unnecessarily large.