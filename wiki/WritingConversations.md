# Introduction

A "conversation" is a series of text messages that progress differently depending on which replies the player selects, sort of like a "Choose Your Own Adventure" book. The goal of conversations is to add another "role playing" aspect to the game by allowing freedom in how the player interacts with other people.

Each "node" of the conversation displays some text, and then either jumps immediately to another node or presents the player with a list of responses to choose between. (The entire conversation can be scrolled backwards, in case the player wishes to review the text and choices associated with previous nodes.)

```html
conversation [<name>]
	scene <image>
	label <name>
	<text>
		[to display]
			<condition>
		[<endpoint> | goto <label>]
	name
	choice
		<text>
			[to (display | activate)]
				<condition>
			[<endpoint> | goto <label>]
		...
	branch <if true> [<if false>]
		<condition> <comp> <value>
		(has | not) <condition>
		(and | or)
			...
	goto (<label> | <endpoint>)
	action
		log [<category> <header>] (<text> | scene <image>)
		outfit <outfit> [<number>]
		give ship <model> [<name>]
		payment [<base> [<multiplier>]]
		fine <amount>
		<condition> (= | += | -=) <value>
		<condition> (++ | --)
		(set | clear) <condition>
		event <name> [<delay> [<max>]]
		fail [<name>]
	...
```

# Endpoints, goto and "to (display | activate)"

After any text message, or in response to any choice, the conversation may jump to a different, labeled point in the conversation, or to one of the "endpoints." Each endpoint causes the conversation to end, and also has other effects:

* `accept`: The player accepts this mission (if one is being offered).
* `launch`: The mission is accepted, _and_ the player immediately takes off. For conversations occurring on a planet, the player will immediately enter space. For conversations occurring in space that are initiated by an interaction with a ship, that ship will die. This includes conversations created by the `on offer` of boarding missions, the completion of NPC objectives, and, as of **v. 0.10.13**, [NPC actions](https://github.com/endless-sky/endless-sky/wiki/CreatingMissions#non-player-characters-npcs).
* `decline`: The mission is declined. (This is also useful for creating conversations that appear when you land or enter a spaceport, but that are intended just to provide flavor, not to lead to a mission.)
* `flee`: The mission is declined, _and_ the player immediately takes off (for conversations occurring on a planet). For conversations occurring in space, the referenced ship (if any) dies.
* `defer`: The mission is declined, but it will not be marked as "offered," so it can be offered again at a later date even if it is not a repeating mission.
* `depart`: The mission is deferred, _and_ you immediately flee. For conversations with a ship in space, this will destroy the ship.
* `die`: The player is killed.
* `explode` (**v. 0.9.9+**): The player is killed, and their flagship explodes (if in space).

For example:

```js
conversation
	`A Navy officer asks if you can do a job for him.`
	choice
		`	"Sure, I'd love to."`
			accept
		`	"Sorry, I'm on an urgent cargo mission."`
			decline
		`(Attack him.)`
			goto "bad idea"
	label "bad idea"
	`	You shout "Death to all tyrants!" and go for your gun.`
	`	Unfortunately, he pulls his own gun first.`
		die
```

The conversation stops as soon as an endpoint is encountered, so if you list multiple endpoints or gotos after a line of text, only the first one will be applied.

You can go to labels earlier on in the conversation if you want, but be careful that this does not create an "infinite loop."

Both texts and choices can also be hidden based on a condition that is part of a "to display" node.

Beginning in **v. 0.10.17**, choices can be given a "to activate" node. If the conditions of the "to activate" do not pass, then the choice will be drawn with darkened text, and the player will be unable to select it. Make sure that players always have at least one active choice, as otherwise they will be unable to progress.

# Scenes

```html
scene <image>
```

A conversation can contain a `scene` image at any point. This will generally be an image from images/scene/, but you can use other images as well, such as ship images or planet images. The image should be no more than 540 pixels wide.

[![][engineScene]][engineScene]

Try to avoid using very tall images for scenes, to avoid the need for scrolling on short screens.

# Labels

```html
label <name>
```

A `label` marks the start of a "node" in a conversation: that is, a point that you can jump to from any other node via a "goto" command. The label can have any name you want, as long as the name is unique. As with all the game's data files, if the label name has spaces in it, you must enclose it in double quotation marks (") or backticks (`).

# Text

```html
<text>
	[<endpoint> | goto <label>]
```

Any line of the conversation that does not begin with one of the special keywords (`label`, `name`, `choice`, `branch`, `action`, or `scene`) is an ordinary text node. In most cases, you will want to enclose the text in backticks so that you can use quotation marks inside it without confusing the parser:

```js
	`You tell the bartender, "The pirates said, 'Give us all your cargo!'"`
		goto "next"
```

Each line of text can be followed by a `goto` or an [endpoint](#endpoints-goto-and-to-display). If it does not, the conversation will just proceed to whatever comes next.

To maintain consistency across all the text in the game:

* Put a [single space](https://en.wikipedia.org/wiki/Sentence_spacing) in between sentences, rather than two spaces.
* The first paragraph of text in a mission [should not be indented](https://practicaltypography.com/first-line-indents.html).
* Subsequent paragraphs should be indented with a single tab character (inside the backticks).
* Dialog should use double quotes as the first level of quotation marks, and single quotes for [nested quotations](https://en.wikipedia.org/wiki/Nested_quotation): `He said, "They told me, 'The aliens have landed.'"`
* Use [Oxford commas](https://en.wikipedia.org/wiki/Serial_comma): `a, b, and c`, not `a, b and c`.
* Avoid non-ASCII characters, including [curly quotes](https://en.wikipedia.org/wiki/Quotation_mark#Quotation_marks_in_English).


```js
	`"${flowers} are the best flower!`
```

Beginning in **v. 0.10.0**, phrases can be referred to in conversation text.
The phrase name should be preceded by "${" and followed by "}", and it will automatically be replaced with text selected from that phrase (or, the phrase name itself if no valid phrase with the given name is found) when the conversation text is displayed.

```js
	`"I need &[xyz] cows tomorrow."`
	`"I need &[tons@xyz] of cows tomorrow."`
	`"I need &[scaled@abc] cows tomorrow."`
	`"I need &[credits@abc] of cows tomorrow."`
	`"I need &[number@abc] cows tomorrow."`
	`"I need &[raw@abc] cows tomorrow."`
	`"I need &[playtime@abc] of your life."`
```

Beginning in **v. 0.10.1**, the values of [player conditions](Player-Conditions) can be substituted into conversations, with various types of formatting.

Given the conditions:
```html
	"abc" 5000000
	"xyz" 5000
```
The above example will appear as:
```js
	`"I need 5,000 cows tomorrow"`
	`"I need 5,000 tons of cows tomorrow."`
	`"I need 5M cows tomorrow."`
	`"I need 5M credits of cows tomorrow."`
	`"I need 5,000,000 cows tomorrow."`
	`"I need 5000000 cows tomorrow."`
	`"I need 57d 20h 53m 20s of your life."`
```

# Name

```html
name
```

This is useful mostly just for the `"default intro"` conversation: if the `name` keyword appears, a set of text-entry boxes are displayed asking the player to enter their first and last name. You could also use this, for example, if the player is going deep undercover and changing their name.

# Choice

```
choice
	<text>
		[<endpoint> | goto <label>]
	...
```

A `choice` node allows the player to select from one or more possible responses. A choice with a single response might be useful if you want to break up the flow of text (e.g. because a lot is being displayed at once), but usually choices will present two or more options.

Each option is represented by a single line of text, which will generally be enclosed in backticks and start with a tab for proper indentation:
```js
choice
	`   "Sure, I'd love to."`
```

As with ordinary text, any choice that does not have a `goto` or endpoint after it will just allow the conversation to continue to the next line below this choice.

# Branch

```html
branch <if true> [<if false>]
	<condition> <comp> <value>
	(has | not) <condition>
	(and | or)
		...
```

A branch takes the conversation to one of two different labels depending on a set of [testable conditions](Player-Conditions#testable-condition-sets).

The `branch` keyword is followed by one or two label names. The first is the label to jump to if the subsequent conditions are all true. The second is the one to jump to if any of the conditions are false. If no second label is supplied, the "false" branch simply continues to the next entry in the conversation.

An endpoint name can be used in place of a label name, in which case, the conversation ends instead of jumping to the label. The corresponding effects for the endpoint are applied. This also means that it is impossible for a `branch` node to branch to a label whose name matches an endpoint.

```js
conversation
	branch "famous"
		"combat rating" > 500
	
	`	When you walk into the bar, a man at a table in the corner looks up, but does not recognize you.`
		decline
	
	label "famous"
	action
		set "everyone thinks you are awesome"
		"drunk" += 1
	
	`	When you walk into the bar, a man at a table in the corner looks up and sees you.`
	`	He says, "It's Captain <last>, the famous warrior! Barkeep, give <first> a drink on my tab."`
		decline
```

# Goto
```html
goto (<label> | <endpoint>)
```

Beginning in **v. 0.10.17** the `goto` keyword can be used as a conversation node on its own. When used this way, it will take the conversation to the specified label or end the conversation with the corresponding endpoint. One use for this is performing actions without changing the text that the player sees:

```html
conversation
	`"I can whip up anything you want," the chef smiles. "So, what's your favorite dish?"`
	choice
		`	"Tacos."`
			goto "tacos"
		`	"Cookies."`
	
	action
		set "favorite food: cookies"
	goto "reaction"

	label "tacos"
	action
		set "favorite food: tacos"

	label "reaction"
	`	The chef raises one eyebrow. "Can't say I expected that, but I'll see what I can do."`
		decline
```

In **any version**, using the `branch` keyword without specifying any conditions will produce the same behavior:

```html
	branch later
	"This text won't display."
	label later
	[...]
```

# Action

```html
action
	log [<category> <header>] (<text> | scene <image>)
	outfit <outfit> [<number>]
	give ship <model> [<name>]
	payment [<base> [<multiplier>]]
	fine <amount>
	<condition> (= | += | -=) <value>
	<condition> (++ | --)
	(set | clear) <condition>
	event <name> [<delay> [<max>]]
	fail [<name>]
```

An "action" entry is similar to a [mission trigger](CreatingMissions#triggers), except it is incapable of creating a dialog or conversation within the current conversation.

**Prior to v. 0.10.3**, if the action entry has a `fail` line and the entry is part of a named conversation (i.e. one that is not defined within a mission) then the apply must name the mission to be failed. After this version, `fail` with no named mission in a named conversation will fail the mission that called the conversation.

**Prior to v. 0.9.15** the `action` node was named `apply` and was only capable of [modifying conditions](Player-Conditions#applied-condition-sets), as seen in the example above where a condition "everyone thinks you are awesome" is assigned a value of 1 and the condition "drunk" is increased by 1. If "drunk" was not already a condition, its initial value is 0. If the condition "everyone thinks you are awesome" already existed and had a different value, this preexisting value is lost. Fractional values will be rounded towards zero (e.g. "0.99" becomes "0", "1.01" -> "1", and "-10.5" becomes "-10," so it is recommended to only use whole numbers. While you can assign generic text as a condition value (e.g. `"drunk" = "true"`), the right-hand side will be treated as a [value expression](Player-Conditions#value-expressions) and the runtime value of the player condition named `"true"` will be used instead of the text "true".

The `apply` keyword is still supported as an alias of `action` for reverse compatibility purposes.

 [engineScene]: https://raw.githubusercontent.com/endless-sky/endless-sky/master/images/scene/engine.jpg 
