# Introduction

A government in Endless Sky is a collection of ships and planets that share a relationship with the player and with other governments. Relationships between governments are modifiable through game events and missions, while the player's reputation with various governments is influenced by the player's decisions, both instantaneously (i.e. in-flight) and strategically (through the missions that the player chooses).

The [syntax](DataFormat#grammar-specifications) for the definition of a government is:

```html
government <name>
    "display name" <other-name>
    swizzle <value#>
    color <r#> <g#> <b#> <a#>
    "player reputation" <initial-rep#>
    "crew attack" <atk#>
    "crew defense" <def#>
    "attitude toward"
        <government> <rep-modifier#>
        ...
    "penalty for"
        (assist | disable | board | capture | destroy | atrocity | scan) <rep-modifier#>
        ...
    "use foreign penalties for"
        <government>
        ...
    "provoked on scan"
    bribe <percentage#>
    fine <percentage#>
    "death sentence" <conversation>
    "friendly hail" <phrase>
    "friendly disabled hail" <phrase>
    "hostile hail" <phrase>
    "hostile disabled hail" <phrase>
    language <text>
    raid <fleet>
    enforces [all]
    enforces
        {filter specification...}
    illegals
        <outfit> <fine>
        ...
    atrocities
        <outfit>
```

The various parts of a government definition are described below.

#### Display Name
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
To use this government in NPCs, fleets, or location filters, you would still use the true name, "Pirate Anarchists".

#### Swizzle
```html
swizzle <value#>
```
The swizzle of a government defines the default color shift applied to ships of that government.

#### Color
```html
color <r#> <g#> <b#> <a#>
```
The government's color is used to shade its systems in the galactic map (including the mini-map shown when holding the "Jump" command). It is also used for drawing the planet's label when the player is in-flight nearby.

#### Player Reputation
```html
"player reputation" <initial-rep#>
```
The player's initial reputation with a government determines whether they are viewed as hostile, friendly, or neutral.

#### Penalty For
```html
"penalty for"
    (assist | disable | board | capture | destroy | atrocity | scan) <rep-modifier#>
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
atrocity 10
``` 

The "scan" penalty was added in **v. 0.9.15** and applies after successfully scanning the outfits or cargo of a ship.

```html
"use foreign penalties for"
    <government>
    ...
```

Beginning in **v. 0.9.17**, governments can be made to use the "penalty for" values of a different government when the other government is acted upon and the first government has an "attitude toward" the other. For example, government A may not like being scanned, while government B doesn't care. If A has a positive attitude toward B, then the player scanning a ship from B will normally anger A. If you want to avoid this situation, then you would list B under A's "use foreign penalties for," causing A to use B's scan penalty of 0 when B is scanned.

### Provoked on scan
```html
"provoked on scan"
```

Beginning in **v. 0.9.15**, the valueless tag `"provoked on scan"` can be added to governments. This tag causes a government to become temporarily hostile when you begin a cargo or outfits scan on one of their ships.

#### Crew Strength Modifiers
```html
"crew attack" <atk#>
"crew defense" <def#>
```
The "crew attack" and "crew defense" tokens allow customizing the base crew combat attributes of this government's ships when they engage in [boarding combat](PlayersManual#boarding-plundering-and-capturing-ships).
The default values for "crew attack" and "crew defense" are 1.0 and 2.0, respectively, and negative numbers will be treated as though they are 0.

#### Attitude Toward
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
indicates that the government "United Kingdom" reacts to your actions towards the separate governments "England," "Northern Ireland," "Scotland," and "Wales" as though you had performed those actions against it (since the reputation modifier is 1, i.e. 100%). A change in the player's reputation with any of these 4 specified governments results in a equivalent change in the player's reputation with the "United Kingdom" government.  
If the player performs hostile actions towards the "Ireland" government, because the reputation modifier is positive, the "United Kingdom" government would consider that a provocation, but--because the magnitude is less than 5%--the player's reputation would not be changed.  
Because the attitude toward "Medieval France" is negative, the two governments consider themselves enemies. If the player performs actions the "United Kingdom" government has deemed "hostile" (i.e. those with positive ["penalty for"](#penalty-for) values) against the "Medieval France" government, then the player's reputation with the "United Kingdom" government will increase as the reputation modifier's magnitude is greater than 5%.

The default attitude between governments is 0 (no interaction).

#### Bribe
```html
bribe <percentage#>
```
The bribe token controls the fraction of the player's fleet's worth that must be paid in order to receive temporary forgiveness of hostile actions. If 0, then the government cannot be bribed by the player.

#### Fine
```html
fine <percentage#>
```
The fine token controls the "leniency" of the government with respect to the fined amount. For example, `fine 0.1` indicates that this government would only fine a player 10,000 credits if the total fine value is 100,000 credits.

#### Illegal and Atrocities
Defines specific items that are considered illegal and considered atrocities by this government. Illegal items have a parameter that lists the amount of fine the player would receive.
Illegal and Atrocities can be modified through evens as well.

#### Death Sentence
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
While the conversation can use traditional [exit nodes](WritingConversations#exits), they will not have any effect on the outcome of the conversation: the player will be killed.
If no "death sentence" is supplied for a government, a simple dialog is displayed instead:
```
Before you can leave your ship, the <government name> authorities show up and begin scanning it. They say, "Captain <last>, we detect highly illegal material on your ship."
	You are sentenced to lifetime imprisonment on a penal colony. Your days of traveling the stars have come to an end.
```

#### Hails
```html
"friendly hail" <phrase>
"friendly disabled hail" <phrase>
"hostile hail" <phrase>
"hostile disabled hail" <phrase>
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

#### Language
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

#### Raid
```html
raid <fleet>
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

#### Enforcement Zones
```html
enforces
    {location filter specification...}
enforces [all]
```
Each use of the optional "enforces" token introduces a description block for a [location filter](CreatingMissions#filters) that describes a set of systems and planets wherein this government has the authority to scan and fine other ships. If no "enforces" tokens are present, then the government is considered to have universal policing authority. Only **one** filter needs to apply in order for the system or planet to be enforced by the given government.

If instead of a description block, the "enforces" keyword is followed by the keyword "all", then all previously specified "enforces" blocks are ignored. This makes it possible for plugins to modify the behavior of governments defined in the base game or other plugins that were loaded earlier.


```bash
government "Daelaam"
    enforces        #1
        planet "Shakuras" "Aiur"
    enforces        #2
        government "Daelaam"
        neighbor government "Daelaam"
    enforces        #3
        government "Confederacy of Man" "Terran Dominion" "The Hivemind" "United Earth Directorate"
        not
            near "Sol" 10
```
This example usage indicates the the government "Daelaam" will have scanning and fining authority for
1) the planets Shakuras and Aiur
2) any planets or systems connected to or controlled by its government
3) planets or systems controlled by the 4 named governments, so long as they are not within 10 hyperspace jumps of Sol.

If a system was 2 jumps from the system Sol, but neighbored a Daelaam-controlled system, ships belonging to the Daelaam government would have scanning jurisdiction because filter #2 matches, even though #3 does not. 