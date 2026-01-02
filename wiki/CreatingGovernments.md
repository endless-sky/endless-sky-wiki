## Introduction

A government in Endless Sky is a collection of ships and planets that share a relationship with the player and with other governments. Relationships between governments are modifiable through game events and missions, while the player's reputation with various governments is influenced by the player's decisions, both instantaneously (i.e. in-flight) and strategically (through the missions that the player chooses).

The [syntax](DataFormat#grammar-specifications) for the definition of a government is:

```html
government <name>
	"display name" <other-name>
	swizzle <name>
	color (<r#> <g#> <b#> | <name>)
	"player reputation" <initial-rep#>
	reputation
		"player reputation" <initial-rep#>
		min <minimum-rep#>
		max <maximum-rep#>
	"crew attack" <atk#>
	"crew defense" <def#>
	"attitude toward"
		<government> <rep-modifier#>
		...
	"default attitude"  <rep-modifier#>
	"penalty for"
		(assist | disable | board | capture | destroy | atrocity | scan | provoke) <rep-modifier#> [none | provoke | atrocity]
		...
	"foreign penalties for"
		<government>
		...
	"custom penalties for"
		<government>
			(assist | disable | board | capture | destroy | atrocity | scan | provoke) <rep-modifier#> [none | provoke | atrocity]
			...
		...
	"provoked on scan"
	bribe <percentage#>
	"bribe threshold" <reputation#>
	fine <percentage#>
	"death sentence" <conversation>
	"send untranslated hails"
	"friendly hail" <phrase>
	"friendly disabled hail" <phrase>
	"hostile hail" <phrase>
	"hostile disabled hail" <phrase>
	"ship bribe acceptance hail" <phrase>
	"ship bribe rejection hail" <phrase>
	"planet bribe acceptance hail" <phrase>
	"planet bribe rejection hail" <phrase>
	language <text>
	raid <fleet> [<min-attraction#> [<max-attraction#>]]
	enforces [all]
	enforces
		{filter specification...}
	"travel restrictions"
		{filter specification...}
	illegals
		"ignore universal"
		<outfit> <fine>
		ship <ship> <fine>
		...
	atrocities ["death sentence" <death-sentence-conversation>]
		"ignore universal"
		<outfit>
		ship <ship>
		...
```

The various parts of a government definition are described below.

#### Display name
```html
"display name" <other-name>
```
The display name of a government is an optional property that controls how the government is referenced in the UI. By default, this value is the same as the token that was used to introduce the government node. This property is useful for creating "pseudo-governments" that should appear to be the same faction as some other government, but have different relationships with the player and/or other governments in the game.

Example:
```html
government "Pirate Anarchists"
	"display name" "Pirate"
	"attitude toward"
		"Pirate" -.01
```
To use this government in NPCs, fleets, or location filters, you would still use the true name, "Pirate Anarchists."

## Swizzle
```html
swizzle <name>
```
The swizzle of a government defines the default color shift applied to ships of that government.

Since **v. 0.10.13**, a government's swizzle can be set by name, where the name is a swizzle defined elsewhere. Previous numbered swizzles can be found as examples in [swizzles.txt](https://github.com/endless-sky/endless-sky/blob/master/data/_ui/swizzles.txt).

## Color
```html
color (<r#> <g#> <b#> | <name>)
```
The government's color is used to shade its systems in the galactic map (including the mini-map shown when holding the "Jump" command). It is also used for drawing the planet's label when the player is in-flight nearby. Colors are defined by their red, green, and blue channels with values from 0 to 1.

Starting in **v. 0.10.0**, the color that a government uses can be set by name, where the name is of a defined color somewhere else in the game. The [interfaces.txt](https://github.com/endless-sky/endless-sky/blob/master/data/interfaces.txt) file has examples of such defined names. 

#### Player reputation
```html
"player reputation" <initial-rep#>
```
The player's initial reputation with a government determines whether they are viewed as hostile, friendly, or neutral.

```html
reputation
	"player reputation" <initial-rep#>
	min <minimum-rep#>
	max <maximum-rep#>
```

Beginning in **v. 0.10.1**, there also exists a `reputation` node which has various children nodes that control the reputation behavior of a government. The player's initial reputation with a government can also be defined here, in addition to the following settings:
* `min`: The minimum reputation below which the player's reputation cannot drop.
* `max`: The maximum reputation above which the player's reputation cannot rise.

#### Penalty for
```html
"penalty for"
	(assist | disable | board | capture | destroy | atrocity | scan | provoke) <rep-modifier#> [none | provoke | atrocity]
	...
```
The "penalty for" token introduces a description block that configures the reputation impact of various in-flight player actions. For example, the block
```bash
"penalty for"
	assist 0
	capture 10
	destroy 0.01
```
indicates that the current government is not influenced when the player repairs one of their disabled ships. If the player captures one of their ships, a significant reputation loss with the current government will occur, whereas only a very minor reputation loss would be incurred if the player destroyed one of the government's ships.

The default "penalty for" values are
```python
assist -0.1
disable 0.5
board 0.3
capture 1
destroy 1
scan 0
provoke 0
atrocity 10
``` 

The "scan" penalty was added in **v. 0.9.15** and applies after successfully scanning the outfits or cargo of a ship.

The "provoke" penalty was added in **v. 0.9.15** and applies after attacking a ship that was previously friendly.

```html
"use foreign penalties for"
	<government>
	...
```

Beginning in **v. 0.10.0**, governments can be made to use the "penalty for" values of a different government when the other government is acted upon and the first government has an "attitude toward" the other. For example, government A may not like being scanned, while government B doesn't care. If A has a positive attitude toward B, then the player scanning a ship from B will normally anger A. If you want to avoid this situation, then you would list B under A's "use foreign penalties for," causing A to use B's scan penalty of 0 when B is scanned.

```html
"custom penalties for"
	<government>
		(assist | disable | board | capture | destroy | atrocity | scan | provoke) <rep-modifier#> [none | provoke | atrocity]
		...
	...
```

Beginning in **v. 0.10.0**, governments can be made to use a custom "penalty for" value of specific actions taken against a different government. For example, perhaps government A does not mind its ships being disabled, meaning it has `"penalty for" disable 0`. But, government A may not like it if you disable ships from government B. This would mean that A would have a `"custom penalties for" "government B" disable 0.5`, or whatever value you wish to use.

A government can be made to use both "foreign penalties for" and "custom penalties for." In such cases, if the "custom penalties for" specified a penalty for a specific action, then that penalty is used. For all penalties that the custom penalties don't specify, the foreign penalty for that action is used.

## Provoked on scan
```html
"provoked on scan"
```

Beginning in **v. 0.9.15**, the valueless tag `"provoked on scan"` can be added to governments. This tag causes a government to become temporarily hostile when you begin a cargo or outfits scan on one of their ships. In **v. 0.10.0** and onward, the provocation happens when you complete the scan.

Beginning in **v. 0.10.17**, penalty actions can define special effects that occur in addition to the reputation change. These can be applied for the government itself, or how it relates to other governments when combined with `"custom penalties for"`.
* `none`: No effect is applied aside from the reputation change. The default behavior of all actions aside from the `provoke` and `atrocity` actions.
* `provoke`: Taking this action provokes the government, causing them to become hostile to you for the day. This is the default behavior of the `provoke` action if no other special effect is provided. Has no effect if the reputation penalty for an action is negative.
* `atrocity`: Taking this action is considered an atrocity by the government, causing you to lose all reputation with them and become hostile. This is the default behavior of the `atrocity` action if no other special effect is provided. Has no effect if the reputation penalty for an action is less that 0.05.

Here is an example usage:
```bash
"penalty for"
	scan 1 provoke
	capture 10 atrocity
	provoke 0 none
```
This would have the effect of giving the government `"provoked on scan"`, as well as making capturing their ships an atrocity, and removing the provoke effect upon shooting them.

#### Crew strength modifiers
```html
"crew attack" <atk#>
"crew defense" <def#>
```
The "crew attack" and "crew defense" tokens allow customizing the base crew combat attributes of this government's ships when they engage in [boarding combat](PlayersManual#boarding-plundering-and-capturing-ships).
The default values for "crew attack" and "crew defense" are 1.0 and 2.0, respectively, and negative numbers will be treated as though they are 0.

#### Attitude toward
```html
"attitude toward"
	<government> <rep-modifier#>
	...
```
The "attitude toward" token introduces a description block that configures the stance of the current government towards the governments specified. For friendly (positive) and hostile (negative) stances, a reputation modifier greater than +/- 5% allows the player's interactions with the specified governments to also influence their reputation with the current government. (The above event types and penalty values are combined with the given attitudes to determine the total impact to the player's reputation with the current government.)

For example, the block
```bash
government "United Kingdom"
	"attitude toward"
		"England" 1
		"Northern Ireland" 1
		"Scotland" 1
		"Wales" 1
		"Ireland" 0.04
		"Medieval France" -0.05
```
indicates that the government "United Kingdom" reacts to your actions towards the separate governments "England," "Northern Ireland," "Scotland," and "Wales" as though you had performed those actions against it (since the reputation modifier is 1, i.e. 100%). A change in the player's reputation with any of these 4 specified governments results in an equivalent change in the player's reputation with the "United Kingdom" government.  
If the player performs hostile actions towards the "Ireland" government, because the reputation modifier is positive, the "United Kingdom" government would consider that a provocation, but--because the magnitude is less than 5%--the player's reputation would not be changed.  
Because the attitude toward "Medieval France" is negative, the two governments consider themselves enemies. If the player performs actions the "United Kingdom" government has deemed "hostile" (i.e. those with positive ["penalty for"](#penalty-for) values) against the "Medieval France" government, then the player's reputation with the "United Kingdom" government will increase as the reputation modifier's magnitude is greater than 5%.

The default attitude between governments is 0 (no interaction).

```html
"default attitude" <rep-modifier#>
```

Beginning in **v. 0.10.5**, governments can be given a default attitude that is something other than 0. If a government does not exist in the "attitude toward" list, then the "default attitude" value will be used instead. This can be used to make a government which is hostile to all other governments in the game.

## Bribe

```html
bribe <percentage#>
```

The bribe token controls the fraction of the player's fleet's worth that must be paid in order to receive temporary forgiveness of hostile actions. If 0, then the government cannot be bribed by the player.

## Bribe threshold

```html
"bribe threshold" <reputation#>
```

Beginning in **v. 0.10.17**, a minimum reputation for accepting bribes can be provided. If this value is set to anything other than zero, ships of this government will not accept bribes when the player's reputation is below this value. This value does not affect the player's ability to bribe planets.
The default value is zero, so bribes can be accepted regardless of how low the player's reputation may be.

## Fine

```html
fine <percentage#>
```

The fine token controls the "leniency" of the government with respect to the fined amount. For example, `fine 0.1` indicates that this government would only fine a player 10,000 credits if the total fine value is 100,000 credits.

#### Illegals and atrocities

```html
illegals
	"ignore universal"
	[ignore] <outfit> <fine>
	[ignore] ship <ship> <fine>
	...
atrocities ["death sentence" <death-sentence-conversation>]
	"ignore universal"
	[ignore] <outfit>
	[ignore] ship <ship>
	...
```

Defines which outfits (**v. 0.10.0**) or ships (**v. 0.10.5**) the government considers illegal or atrocities for the player to have. If an outfit or ship has the "illegal" or "atrocity" attribute on it (therefore giving it default illegal/atrocity behavior to all governments), then using the "ignore" keyword before listing the outfit or ship causes the government to ignore that item's base attributes. If an outfit or ship has the "illegal" attribute but the government definition also defines the item as illegal, then the fine amount from the government definition will be used. If an outfit or ship is considered an atrocity by default but you wish for a government to only fine its ownership, then you must add the item as ignored in the atrocities list and give it a fine value in the illegals list. If "ignore universal" (**v. 0.10.17**) is added to an illegals/atrocities list, this government will only take action for its custom-defined illegals/atrocities.
Since **v. 0.10.17**, you can override [death sentences](#death-sentence) of individual atrocities by specifying the name of the conversation that should be used for the `atrocities` node. If no custom death sentence is set here, the government's default will be used (see below). If you want to have multiple custom death sentences on a single government, you can specify additional atrocity lists with `add atrocities "death sentence" <another-death-sentence>` nodes.

#### Death sentence
```html
"death sentence" <conversation>
```

The "death sentence" token specifies an optional conversation to be displayed when the player is found to have committed an atrocity and is on a planet controlled by this government. The conversation named here should be defined as its own standalone token, e.g.

```bash
conversation "caught red-handed"
	{conversation specification...}

government "Tortuga"
	"death sentence" "caught red-handed"
```

While the conversation can use traditional [exit nodes](WritingConversations#endpoints-goto-and-to-display), they will not have any effect on the outcome of the conversation: the player will be killed.
If no "death sentence" is supplied for a government, a simple dialog is displayed instead:

```
Before you can leave your ship, the <government name> authorities show up and begin scanning it. They say, "Captain <last>, we detect highly illegal material on your ship."
	You are sentenced to lifetime imprisonment on a penal colony. Your days of traveling the stars have come to an end.
```

## Hails
```html
"send untranslated hails"
"friendly hail" <phrase>
"friendly disabled hail" <phrase>
"hostile hail" <phrase>
"hostile disabled hail" <phrase>
"ship bribe acceptance hail" <phrase>
"ship bribe rejection hail" <phrase>
"planet bribe acceptance hail" <phrase>
"planet bribe rejection hail" <phrase>
```
These tokens allow customization of the text generation when the player communicates with ships of this government, based on their current reputation and the state of the ship issuing the hail. The [phrases](CreatingPhrases) here should be defined as standalone phrases, e.g.
```bash
phrase "ancient greek insults"
	{phrase specification...}

government "Phoenicia"
	"hostile hail" "ancient greek insults"
```
See [`data/human/hails.txt`](https://github.com/endless-sky/endless-sky/blob/master/data/human/hails.txt) for examples of both simple and complex phrase definitions.

If not specified, defaults are provided for disabled ships of all governments. These defaults are:
```bash
"friendly disabled hail" "friendly disabled"
"hostile disabled hail" "hostile disabled"
```
No defaults are provided for non-disabled hails (meaning if not specified, the ships will not randomly "chat" while the player is in-flight).

In order to receive hails from a ship, you must share a language with its government. Languages are discussed in more detail below. Beginning in **v. 0.10.1**, the only exception to this is if the government has the `"send untranslated hails"` token, which allows its ships to send hails even if you do not share a language.

Beginning in **v. 0.10.17**, you can use `"ship bribe acceptance hail"`, `"ship bribe rejection hail"`, `"planet bribe acceptance hail"`, and `"planet bribe rejection hail"` to customize the text that appears to the player when a bribe is accepted or rejected by a ship or planet.
The default text on acceptance if no phrase is provided is: "It's a pleasure doing business with you."
The default text on rejection if no phrase is provided is: "I do not want your money."

## Language
```html
language <text>
```
The optional "language" token allows disabling hail-based interaction with ships and planets controlled by this government until the player has "learned" the government's language. If given, the player cannot

1. Bribe (to cease hostilities) or request repair/fuel from ships of this government
2. Bribe (to land) or demand tribute from planets of this government.

until the [player condition](Player-Conditions) `"language: <text>"` is applied to their pilot (e.g. from an event or mission completion). For example,
```bash
government "Children of Tama"
	language "metaphorical allegory"
```
would prevent the player from usefully hailing these ships or planets until they possess the condition `"language: metaphorical allegory"`. Instead, they will receive the message, "(An alien voice says something in a language you do not recognize.)"

Specifying a language that the player does not have will not prevent landing or receiving job offers on the government's planets. To avoid offering jobs or missions for these governments, the mission "to offer" nodes can specify the language condition as a requirement, e.g.

```bash
mission "player needs the Tamarian language"
	to offer
		has "language: metaphorical allegory"
```

## Raid
```html
raid <fleet> [<min-attraction#> [<max-attraction#>]]
```
The "raid" token allows the specification of a [fleet](CreatingFleets) that can be spawned in this government's systems if the player has "unguarded cargo". (In order to be spawned, the named fleet's government must also be hostile to the player.) Up to 10 of these fleets may be spawned at once, depending on the magnitude of the cargo space : weaponry imbalance.

If defined more than once (e.g. specified both in the base game and then a plugin), each new definition overwrites the current definition. For example, if the game datafiles specify
```bash
government "Republic"
	raid "Large Core Pirates"
```
and a plugin specifies
```bash
government "Republic"
	raid "Large Militia"
```
then the game's base definition of "Large Core Pirates" as the raid fleet is forgotten and "Large Militia" is used instead.

Beginning in **v. 0.10.0**, governments are allowed to have multiple raid fleets defined, whereas before that point, only a single raid fleet could be defined for each government. Each fleet can also be given a minimum and maximum attraction value for which it will spawn. If a minimum attraction is not provided then the default minimum value of 2 is used. If a minimum attraction is provided but no maximum, then the default maximum value is unbounded (a value of 0 for the maximum value means there is no maximum). This can allow for systems having different raid fleets depending on whom the player is hostile toward, as well as different fleets depending on how attractive the player is to raids.

If multiple raid fleets are capable of spawning at the same time, each fleet checks if it can spawn up to 10 times as described above (meaning that multiple potential raid fleets can greatly increase the chance of any single fleet spawning). The likelihood of a single fleet spawning is determined by how far above that fleet's minimum attractiveness you are. Additionally, raid fleets will also take into account the strengths and spawn rates of normal fleets in the system. If a system has fleet spawns that are hostile to the raid fleet but not to the player, that will decrease the chance of the raid spawning. If a system has fleets that are hostile to the player but not the raid fleet, that will increase the chance of the raid spawning. If a system fleet is either friendly or hostile to both the raid and the player, it has no effect on the raid's spawn chance.

Attraction is described by the `"cargo attractiveness"`, `"armament deterrence"`, and `"pirate attraction"` [conditions](Player-Conditions#read-only), with the last condition being the value used to determine if fleets should spawn.

#### Enforcement zones
```html
enforces [all]
enforces
	{location filter specification...}
```
Each use of the optional "enforces" token introduces a description block for a [location filter](LocationFilters) that describes a set of systems and planets wherein this government has the authority to scan and fine other ships. If no "enforces" tokens are present, then the government is considered to have universal policing authority. Only **one** filter needs to apply in order for the system or planet to be enforced by the given government.

If instead of a description block, the "enforces" keyword is followed by the keyword "all", then all previously specified "enforces" blocks are ignored. This makes it possible for plugins to modify the behavior of governments defined in the base game or other plugins that were loaded earlier.


```bash
government "Daelaam"
	enforces		#1
		planet "Shakuras" "Aiur"
	enforces		#2
		government "Daelaam"
		neighbor government "Daelaam"
	enforces		#3
		government "Confederacy of Man" "Terran Dominion" "The Hivemind" "United Earth Directorate"
		not
			near "Sol" 10
```
This example usage indicates the government "Daelaam" will have scanning and fining authority for
1) the planets Shakuras and Aiur
2) any planets or systems connected to or controlled by its government
3) planets or systems controlled by the 4 named governments, so long as they are not within 10 hyperspace jumps of Sol.

If a system was 2 jumps from the system Sol, but neighbored a Daelaam-controlled system, ships belonging to the Daelaam government would have scanning jurisdiction because filter #2 matches, even though #3 does not. 

#### Travel restrictions

```html
"travel restrictions"
	{filter specification...}
```

Beginning in **v. 0.10.3**, governments can be given travel restrictions which prevent fleets from that government from traveling to systems or landing on planets that match the [location filter](LocationFilters). These travel restrictions do not apply to mission NPCs that follow the player. Random fleet spawns can be made to ignore these travel restrictions by giving them the `unrestricted` [personality](ShipPersonalities#non-combat-goals).
