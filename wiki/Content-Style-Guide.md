# Table of Contents

* [Background](#background)
* [General Guidelines](#general-guidelines)
  * [Quoting Tokens](#when-and-how-to-quote-tokens)
* [Missions](#missions)
  * [Token Order](#token-order)
    * [`mission` Nodes](#mission-nodes)
    * [`on *` Nodes](#on--nodes)
    * [`npc` Nodes](#npc-nodes)
  * [`blocked` Dialogs](#blocked-dialogs)
  * [`on visit` Dialogs](#on-visit-dialogs)
* [Conversations](#conversations)
  * [Text](#text)
  * [Labels](#labels)
  * [Choices](#choices)

# Background

Similar to the [C++ style guide](C++-Style-Guide), this page acts as a style guide for all of the game's content (i.e. anything defined in the game's [data format](DataFormat)). For specifics on how different data nodes behave in the game, see the other pages in the wiki relevant to those nodes. The purpose of this page is to define how data should be laid out stylistically as to improve readability and maintainability of all contributed content.

Note that many of these style requirements have been developed over the years, so not all of the game's content may adhere to this guide 100%.

# General Guidelines

## When and how to quote tokens

Tokens in the data format can be categorized in one of three ways:
1. Engine keywords: Tokens that define the start of a node. Everything after this token or that is a child of it behaves based off of what this node is. These are sometimes also referred to as root nodes (typically in reference to nodes that are not indented and start the definition of a major object in the game). Any token that is not a root token is a value token.
2. Engine-defined values: Value tokens that have special meaning in the engine.
3. User-defined values: Value tokens that have no special meaning in the engine and are instead defined by the content creator. The value could be changed to something else everywhere it is located and it would behave the exact same way.

Consider the following example:
```html
mission "Example Mission"
	description `Destroy the "Test Dummy."`
	source
		attributes "deep" spaceport
	npc kill
		government "Merchant"
		personality staying
		ship "Sparrow" "Test Dummy"
```

In this mission, each token would fall into the three categories as follows:
1. Engine keywords: `mission`, `source`, `attributes`, `npc`, `personality` `government` `ship`. These are all roots of particular objects in the game. `mission` is the root of a [Mission](CreatingMissions), `source` of a [LocationFilter](LocationFilters), `npc` of an [NPC](CreatingMissions#non-player-characters-npcs), etc.
2. Engine-defined values: `spaceport`, `kill`, `staying`. The former is an engine-defined planet attribute. Any planet with a spaceport will have this attribute automatically added to it. The latter two are an [NPC objective]() and [NPC personality](), respectively.
3. User-defined values: `"Example Mission"`, `"deep"`, `"Merchant"`, `"Sparrow"`, `"Test Dummy"`. All of these refer to different names of things in the game, none of which have any meaning to the engine aside from how they relate to something defined somewhere else in the game data (in the case of `"deep"`, `"Merchant"`, and `"Sparrow"` being the reference names of a planet attribute, government, and ship respectively), or how they are referred to from elsewhere (in the case of `"Example Mission"` being the identifier of the mission and `"Test Dummy"` being the given name of the NPC).

Theoretically speaking, this mission could also be defined as follows:
```html
"mission" "Example Mission"
	description "Destroy the 'Test Dummy.'"
	"source"
		"attributes" "deep" "spaceport"
	"npc" "kill"
		"government" "Merchant"
		"personality" "staying"
		"ship" "Sparrow" "Test Dummy"
```
Or, since backticks and double quotation marks can both be used to identify a token, like this:
```html
`mission` `Example Mission`
	`name` `Destroy the "Test Dummy."`
	`source`
		`attributes` `deep` `spaceport`
	`npc` `kill`
		`government` `Merchant`
		`personality` `staying`
		`ship` `Sparrow` `Test Dummy`
```

The additional quotation marks everywhere makes such a mission unnecessarily visually busy. There is also a question of whether to use single-quotes or double-quoets for quotations inside of a token. (i.e. `'Test Dummy'` vs `"Test Dummy"`.

Therefore, the guidelines for when to quote a token are as follows:
1. All multi-word tokens must be in double quotation marks or backticks (as that is what defines a multi-word token in the first place).
	1. If a token contains a quotation, then the token should be in backticks and the quotation should be in double quotes.
	2. If a token does not contain a quotation, then it should typically be surrounded by double quotation marks instead of backticks. (Exceptions will be highlighted when they arise.)
2. Single-word engine keywords and engine-defined values should not be in quotation marks.
3. All user-defined values should be in quotation marks.

By following these guidelines, content can be easily visually parsed into the aforementioned categories.
Note that there are a few exceptions to these rules. These exceptions will be highlighted in the sections where they are relevant.

# Missions

The identifiers for missions should generally be defined in the following format: `<storyline>: <arc> <part>`. For example, the mission `"Wanderers: Unfettered Diplomacy 1"` is part  1 of the Unfettered Diplomacy arc in the Wanderers storyline. Sometimes, the "arc" is a single mission long, in which case the "part" number should omitted.

For arcs that branch, a letter or decimal is often used alongside the part number. For example, `"FW Bloodsea 1.1"` and `"FW Bloodsea 1.2"` are each the first mission in the Bloodsea arc of the Free Worlds campaign on branch number 1 and 2 respectively.

For missions that offer alongside other missions but do not themselves progress a storyline or arc, they usually take the form `<storyline>: <arc> <part>: <name>`. For example, `"Deep: Mystery Cubes 3: Escorts"` is used for the mission that spawns the escorts for the mission `"Deep: Mystery Cubes 3"`, while `"Deep: TMBR: Hint"` is used to give the player a hint during the TMBR arc of the Deep storyline.

## Token order

The following is a general outline of how tokens should be ordered in a mission and some of its larger child nodes.

### `mission` nodes

```html
mission "<name>"
	# Various mission tags:
	autosave
	(minor | priority | non-blocking)
	invisible
	(job | landing | assisting | boarding | shipyard | outfitter | "job board" | entering)
	repeat
	# Text substitutions to be applied in the mission, placed above any nodes that could make use of it:
	substitutions
		...
	# The mission's appearance on the map:
	name "..."
	description `...`
	color ...
	# The mission's source, destination, and nodes that relate to them:
	source ...
	destination ...
	"distance calculation settings"
		...
	clearance ...
	ignore clearance
	infiltrating
	# Mission objectives and nodes that relate to them:
	deadline ...
	passengers ...
	cargo ...
	blocked "..."
	illegal ...
	stealth
	waypoint ...
	mark ...
	stopover ...
	
	# How the mission is offered:
	"offer precedence" ...
	to offer
		...
	to accept
		...
	on offer
		...
	
	# Things that occur after the on offer is completed:
	on accept
		...
	on decline
		...
	on defer
		...
	
	# NPCs and timers:
	npc
		...
	timer
		...
	
	# on * nodes that occur while in flight.
	on waypoint
		...
	on stopover
		...
	on enter
		...
	on daily
		...
	on disabled
		...
	
	# on * nodes that occur when you complete, try to complete, or fail the mission.
	to fail
		...
	on fail
		...
	on abort
		...
	on visit
		...
	to complete
		...
	# The on complete should always come last.
	on complete
		...
```

### `on *` nodes

```html
on *
	# Put logs first:
	log ...
	# Then any condition changes first:
	<condition> <assignment>
	# Then any events:
	event ...
	# Then payments:
	payment ...
	fine ...
	debt ...
	outfit ...
	(give | take) ship ...
	# Any miscellaneous actions not mentioned elsewhere can then go next, such as:
	fail ...
	mark ...
	unmark ...
	music ...
	message ...
	# Dialogs and conversations are mutually exclusive, but they should always come last:
	dialog ...
	conversation
		...
```

### `npc` nodes

```html
# Objectives in alphabetical order.
npc disable "scan cargo" ...
	# The spawn and despawn conditions
	to spawn
		...
	to despawn
		...
	# The fleet's government, personality, and where it spawns.
	government ...
	# Personalities should be in alphabetical order.
	personality ...
	system ...
	# Settings for the cargo held by the NPC
	"cargo settings"
		...
	# The ship(s) and/or fleet(s) that make up the NPC.
	ship ...
	fleet ...
	# on * nodes have no fixed order.
	on *
		...
	# The dialog and conversation that should be triggered when NPC's objectives are completed.
	dialog ...
	conversation
		...
```

## `blocked` dialogs

Any mission which meets the following criteria should have a `blocked` dialog:

1. The mission is not the first mission in a chain (i.e. it's continuing from a previous mission).
2. The mission does not have the `invisible` tag, has an `on offer` conversation or dialog, or is in any way something that is meant to be visible to the player.
3. The mission has cargo, passengers, a `to accept` node, or anything else that may cause the mission to fail to offer even if the `on offer` conditions are met.

A mission may contain a `blocked` dialog even if it does not meet these criteria, and an exception can be made for missions which use the `to accept` to wait for another mission to complete first.

## `on visit` dialogs

Any mission which meets the following criteria must have an `on visit` node that contains a `dialog`:

1. The mission has ANY objective whatsoever that must be met in order for the player to complete the mission upon landing at the destination. This includes but is not limited to the mission having passengers or cargo, an NPC with an objective, stopovers or waypoints, or a `to complete` node.

The `on visit` dialog is what informs the player that they haven't completed the mission despite having reached the destination due to not having completed all of the objectives, and then reiterates all the objectives of the mission that the player may be missing in a concise manner. Many common objectives have predefined `dialog phrase`s made for them. For generic missions like jobs, the relevant `dialog phrase` should be used for the mission's objective. For major storyline missions, the dialog is typically tailored to the mission, such as by mentioning specific character names instead of generically referring to missing passengers or escorts.

# Conversations

The nodes of a conversation should be ordered in such a way as to allow the conversation to be read from top to bottom without having to loop back up to a prior point in the conversation definition where possible.

For example, the following would not be acceptable:
```
conversation
	`"Make a choice, any choice."`
	choice
		`	"I choose this one."`
			goto this
		`	"I choose that one."`
			goto that
	label end
	`	The end!`
		decline
	label this
	`	I see that you have chosen this.`
		goto end
	label that
	`	An interesting choice, that is.`
		goto end
```

It should instead be written like this:
```
conversation
	`"Make a choice, any choice."`
	choice
		`	"I choose this one."`
		`	"I choose that one."`
			goto that
	label this
	`	I see that you have chosen this.`
		goto end
	label that
	`	An interesting choice, that is.`
	label end
	`	The end!`
		decline
```

## Text

The text nodes of a conversation should always be surrounded by backticks, even if a particular line of text does not contain a quotation. (This is an exception to the general [token quoting rules](#when-and-how-to-quote-tokens).)

Aside from the first line of text in a conversation, all text and choices should contain an additional tab inside the text node. See the above example where `"Make a choice, any choice."` has no tab after the backtick, but all subsequent lines of text and choices do.

For any text with a `goto` under it, the `goto` should be omitted if it is jumping to the label directly below it, as lines of text implicitly drop down to the next available line of text if they lack a `goto` child. This is also shown by the above example.

## Labels

Labels should typically be single-word tokens which are not surrounded by quotation marks. (This is an exception to the general [token quoting rules](#when-and-how-to-quote-tokens).)

If the same word fits for two labels (e.g. if two sections of text have similar and only differ slightly based on a prior choice), the convention is to add a numeral to the end of the label used in either section (e.g. `agree1` and `agree2`.
Multi-word tokens should only be used when absolutely necessary for the sake of clarity. When a multi-word token is used, it should be written `"like this"` as two separate words in quotation marks, as opposed to `likeThis` or `like_this`.

Labels should NEVER have the same name as a [conversation endpoint keyword](WritingConversations#endpoints-goto-and-to-display--activate), as to avoid confusion with endpoints.

## Choices

Choices fall into one of two categories:
1. Dialog choices
2. Action choices

Dialog choices represent something that your player character will say, and are represented in quotation marks.
```html
choice
	`	"And what reason do I have to believe you?"`
	`	"I'm not convinced. You'll need to find some other sucker to do your dirty work for you."`
```

Action choices represent something that your player character will do, and are represented in parentheses.
```html
choice
	`	(Do this karate move I saw in a video one time.)`
		goto karate
	`	(Raise my hands slowly into the air and find out what she wants.)`
```

Since choices are written from the first-person perspective of your character, they should always be in the first person. This includes action choices.

Choices should almost always have at least two options available for the player to choose from. Even if both choices lead to the same outcome (i.e. they go to the same label or next line of text), it is better to have two options to choose from than to only ever give the player a single option.
