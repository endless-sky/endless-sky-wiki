# Table of Contents

* [Overview](#overview)
* [Basic pilot information](#basic-pilot-information)
* [Location and navigation](#location-and-navigation)
* [Interface configuration](#interface-configurations)
  * [Map settings](#map-settings)
  * [Shop categories](#shop-categories)
* [Reputation](#reputation)
* [Tribute](#tribute)
* [Player ships](#player-ships)
  * [Name](#name)
  * [Cargo](#cargo)
  * [In flight properties](#in-flight-properties)
  * [Location](#location)
* [Outfit storage](#outfit-storage)
* [Licenses](#licenses)
* [Finances](#finances)
  * [Credits](#credits)
  * [Salaries](#salaries)
  * [Overdue payments](#overdue-payments)
  * [Credit score](#credit-score)
  * [Net worth history](#net-worth-history)
  * [Mortgages](#mortgages)
* [Cargo and stock](#cargo-and-stock)
  * [Cargo](#cargo-1)
  * [Commodity basis](#commodity-basis)
  * [Outfits in stock](#outfits-in-stock)
* [Depreciation](#depreciation)
* [Missions](#missions)
  * [Active missions](#active-missions)
  * [Mission cargo and passengers](#mission-cargo-and-passengers)
  * [Available jobs and missions](#available-jobs-and-missions)
  * [Mission sorting](#mission-sorting)
* [Conditions](#conditions)
* [Gifted ships](#gifted-ships)
* [Upcoming events](#upcoming-events)
* [Changes to the universe](#changes-to-the-universe)
* [Economy](#economy)
* [Destroyed persons](#destroyed-persons)
* [Places this pilot has visited](#places-this-pilot-has-visited)
  * [Systems and planets](#systems-and-planets)
  * [Minable harvest locations](#minable-harvest-locations)
* [Logbook](#logbook)
* [Starting conditions](#starting-conditions)
* [Plugins](#plugins)

# Overview

```html
pilot <first> <last>
date <day#> <month#> <year#>
"system entry method" (hyperdrive | "jump drive" | wormhole | takeoff)
"previous system" <system>
system <system>
planet <planet>
[clearance]
playtime <time#>
[launching]
travel <systemN>
...
travel <system2>
travel <system1>
"travel destination" <planet>
"flagship index" <value#>
"map coloring" <value#>
"map zoom" <value#>
"collapsed category" <category>
	<collapsed>
	<collapsed>
	...
"collapsed category" <category>
	...
...
"reputation with"
	<government> <reputation#>
	<government> <reputation#>
	...
"tribute received"
	<planet> <credits#>
	<planet> <credits#>
	...

# What you own:
ship <model>
	name <name>
	...
	crew <crew#>
	fuel <fuel#>
	shields <shields#>
	hull <hull#>
	position <x#> <y#>
	...
	"formation pattern" <formation>
	system <system>
	planet <planet>
	[parked]
...
storage
	planet <planet>
		cargo
			outfits
				<outfit> <count#>
				<outfit> <count#>
				...
	planet <planet>
		...
	...
licenses
	<license>
	<license>
	...
account
	credits <credits#>
	"salaries income"
		<salary> <credits#>
		<salary> <credits#>
		...
	salaries <credits#>
	maintenance <credits#>
	score <score#>
	history
		<credits#>
		<credits#>
		...
	mortgage (Mortgage | Fine | Debt)
		principal <credits#>
		interest <value#>
		term <days#>
	mortgage ...
		...
	...
cargo
	commodities
		<commodity> <count#>
		<commodity> <count#>
		...
	outfits
		<outfit> <count#>
		<outfit> <count#>
		...
basis
	<commodity> <value#>
	<commodity> <value#>
	...
stock
	<outfit> <count#>
	<outfit> <count#>
	...
"fleet depreciation"
	ship <model>
		<count#> <day#>
		<count#> <day#>
		...
	ship <model>
		...
	...
	outfit <outfit>
		<count#> <day#>
		<count#> <day#>
		...
	outfit <outfit>
		...
	...
"stock depreciation"
	...

# What you've done:
mission <name>
	...
mission <name>
	...
...
"mission cargo"
	"player ships"
		<missionID> <shipID> <count#>
		<missionID> <shipID> <count#>
		...
"mission passengers"
	"player ships"
		<missionID> <shipID> <count#>
		<missionID> <shipID> <count#>
		...
"available job" <name>
	...
"available job" <name>
	...
...
"available mission" <name>
	...
"available mission" <name>
	...
...
"sort type" <value#>
["sort descending"]
["separate deadline"]
["separate possible"]
conditions
	<condition> <value#>
	<condition> <value#>
	...
"gifted ships"
	<name> <shipID>
	<name> <shipID>
	...
event
	...
event
	...
...
changes
	...
economy
	purchases
		<system> <commodity> <count#>
		<system> <commodity> <count#>
		...
	system <commodity> <commodity> <commodity> ...
	<system> <supply> <supply> <supply> ...
	<system> <supply> <supply> <supply> ...
	...
"destroyed" <person>
"destroyed" <person>
...

# What you know:
visited <system>
visited <system>
...
"visited planet" <planet>
"visited planet" <planet>
...
harvested
	<system> <outfit>
	<system> <outfit>
	...
logbook
	<day#> <month#> <year#>
		<text>
		<text>
		...
	<day#> <month#> <year#>
		...
	...
	<category> <heading>
		<text>
		<text>
		...
	<category> <heading>
		...
	...

# How you began:
start <start>
	system <system>
	planet <planet>
	date <day#> <month#> <year#>
	account
		...

# Installed plugins:
plugins
	<plugin>
	<plugin>
	...
```


```html
pilot <first> <last>
```

# Basic pilot information

The first and last names of this pilot.

```html
date <day#> <month#> <year#>
```

# Location and navigation

The in game date of this save file.

```html
"system entry method" (hyperdrive | "jump drive" | wormhole | takeoff)
"previous system" <system>
```

How the current system was last entered and what the previous system was. **v. 0.10.0**

```html
system <system>
planet <planet>
```

This pilot's current location.

```html
[clearance]
```

Present if the player has clearance on the current planet. Without clearance, planetary services are unavailable.

```html
playtime <time#>
```

The time, in seconds, that this pilot has been played for. **(v. 0.9.13)**

```html
[launching]
```

If present, the player will be launched from this planet (if possible) upon loading this save.

```html
travel <systemN>
...
travel <system2>
travel <system1>
```

The player's current travel plan. Systems are listed in reverse order. The last system in this list is the first system that will be traversed in this travel plan.

```html
"travel destination" <planet>
```

The planet to land on after completing the travel plan.

```html
"flagship index" <value#>
```

The index of the flagship in the player's ship list. Index 0 is the first ship in the list.

# Interface configurations

## Map settings

```html
"map coloring" <value#>
```

Indicates what property should be used to color systems on the star map.
Positive values correspond to coloring by commodity values.
Negative values correspond to other properties:
value | coloring property
-- | --
-1 | Colored by the presence of a shipyard
-2 | Colored by the presence of an outfitter
-3 | Colors based on if and how many planets in the system have been visited
-4 | Only used in shipyard or outfitter view: shows availability of the currently selected ship or outfit
-5 | Gives systems the [color](CreatingGovernments#color) of their government
-6 | Colors based on the player's reputation with system governments
-7 | Systems are colored based on how dangerous they are

```html
"map zoom" <value#>
```

Stores the zoom level in the star map.

## Shop categories

```html
"collapsed category" <category>
	<collapsed>
	<collapsed>
	...
"collapsed category" <category>
	...
...
```

Categories, such as in the shipyard and outfitter, that the player has collapsed.

# Reputation

```html
"reputation with"
	<government> <reputation#>
	<government> <reputation#>
	...
```

This pilot's reputation with each government.

# Tribute

```html
"tribute received"
	<planet> <credits#>
	<planet> <credits#>
	...
```

The number of credits this pilot receives each day from planets they have dominated. **(v. 0.10.1)**
Prior to **v. 0.10.1**, this information appeared in the save file in the conditions section.
While the way in which they are stored in the save file was changed, tribute amounts remain accessible and adjustable via [conditions](Player-Conditions#modifiable).

# Player ships

```html
ship <model>
	name <name>
	...
	cargo
		commodities
			<commodity> <count#>
			<commodity> <count#>
			...
		outfit
			<outfit> <count#>
			<outfit> <count#>
			...
	crew <crew#>
	fuel <fuel#>
	shields <shields#>
	hull <hull#>
	position <x#> <y#>
	...
	"formation pattern" <formation>
	system <system>
	planet <planet>
	[parked]
...
```

The ships this pilot owns and their installed outfits.
In addition to many of the child nodes present on [ship definitions](CreatingShips#data), instantiated ships also have additional information.

## Name

```html
name <name>
```

The name of this particular ship. Either randomly chosen on purchase, set by the player on purchase or edited from the ship info panel, or, if this ship was captured, the name it was given on instantiation.

## Cargo

```html
	cargo
		commodities
			<commodity> <count#>
			<commodity> <count#>
			...
		outfit
			<outfit> <count#>
			<outfit> <count#>
			...
```

The commoodities and outfits this ship has in its cargo.
Mission cargo and passengers are tracked elsewhere.

## In flight properties

```html
crew <crew#>
```

The number of crew currently aboard this ship.
This may differ from the required crew: if this is the flagship, the player may have hired extra crew.
If this ship was recently captured, it may have more than its required crew (as non-player ships are assigned a random amount of crew between their required crew and the number of bunks available).
It is also possible for the flagship or a recently captured ship to be left with fewer crew than required due to losses during combat when attempting to capture.

```html
fuel <fuel#>
```

How much fuel this ship currently has. May be below the fuel capacity if some fuel has been consumed (either by interstellar travel or outfits consuming fuel) and has not yet replenished by landing on a planet with refuelling or via ramscoop (or other fuel generating outfit).

```html
shields <shield#>
```

The current amount of shield health this ship has remaining.

```html
hull <hull#>
```

The current amount of hull health this ship has remaining.
For ships appearing as mission NPCs, a negative value may be present, indicating that the ship has been destroyed.

```html
position <x#> <y#>
```

The position of this ship in the current system.

```html
"formation pattern" <formation>
```

???

## Location

```html
system <system>
```

The name of the system this ship is currently in.
Ships that are currently being carried will list their carrier's system.

```html
planet <planet>
```

If this ship is currently landed on a planet, the name of that planet.

```html
[parked]
```

Present if the ship is currently parked. Not present otherwise.

# Outfit storage

```html
storage
	planet <planet>
		cargo
			outfits
				<outfit> <count#>
				<outfit> <count#>
				...
	planet <planet>
		...
	...
```

Lists planets where the player's outfits are currently stored and which outfits and how many are stored there. **(v. 0.9.13)**

# Licenses

```html
licenses
	<license>
	<license>
	...
```

The names of the licenses this pilot currently has. **(v. 0.10.1)**
Prior to **v. 0.10.1**, this information appeared in the save file in the conditions section.
While the way in which they are stored in the save file was changed, the player's licenses remain accessible and adjustable via [conditions](Player-Conditions#modifiable).

# Finances

```html
account
	credits <credits#>
	"salaries income"
		<salary> <credits#>
		<salary> <credits#>
		...
	salaries <credits#>
	maintenance <credits#>
	score <score#>
	history
		<credits#>
		<credits#>
		...
	mortgage (Mortgage | Fine | Debt)
		principal <credits#>
		interest <value#>
		term <days#>
	mortgage ...
		...
	...
```

This pilot's financial state.

## Credits

```html
credits <credits#>
```

The number of credits this pilot has.

## Salaries

```html
"salaries income"
	<salary> <credits#>
	<salary> <credits#>
	...
```

The names of salaries this pilot has and the number of credits they receive each day from each.
Prior to **v. 0.10.1**, this information appeared in the save file in the conditions section.
While the way in which they are stored in the save file was changed, the pilot's salaries remain accessible and adjustable via [conditions](Player-Conditions#modifiable).

## Overdue payments

```html
salaries <credits#>
maintenance <credits#>
```

The number of credits owed for overdue crew slaries and maintenance payments.
Prior to **v. 0.10.1**, this information appeared in the save file in the conditions section.
While the way in which they are stored in the save file was changed, the amount of overdue salary and maintenance payments remain accessible via [conditions](Player-Conditions#modifiable).

## Credit score

```html
score <score#>
```

This pilot's credit score. The higher the score, the lower the interest rate on mortgages taken out from the bank will be.
Ranges from 400 to 800.

## Net worth history

```html
history
	<credits#>
	<credits#>
	...
```

This pilot's net worth over the last 100 days. Used to determine the maximum mortgage amount offered by the bank.

## Mortgages

```html
mortgage (Mortgage | Fine | Debt)
	principal <credits#>
	interest <value#>
	term <days#>
mortgage ...
	...
...
```

The debt this pilot has.

# Cargo and stock

## Cargo

```html
cargo
	commodities
		<commodity> <count#>
		<commodity> <count#>
		...
	outfits
		<outfit> <count#>
		<outfit> <count#>
		...
```

When landed on a planet, the commodities and outfits in cargo on all the player's ships also at that planet is collected into a single storage pool and saved here, instead of saved to individual ships.
Mission cargo and passengers that are not assigned to a particular ship are also included in this pool.
All the pooled passengers and cargo are distributed to ships on take off, if possible.

## Commodity basis

```html
basis
	<commodity> <value#>
	<commodity> <value#>
	...
```

## Outfits in stock

```html
stock
	<outfit> <count#>
	<outfit> <count#>
	...
```

Outfits not normally sold by the current planet that are in stock because the player has sold them here.
In stock outfits are cleared on take off.

# Depreciation

```html
"fleet depreciation"
	ship <model>
		<count#> <day#>
		<count#> <day#>
		...
	ship <model>
		...
	...
	outfit <outfit>
		<count#> <day#>
		<count#> <day#>
		...
	outfit <outfit>
		...
	...
"stock depreciation"
	...
```

# Missions

## Active Missions

```html
mission <name>
	...
mission <name>
	...
...
```

This pilot's active missions.
Normal missions are listed first, then jobs. However, jobs are still listed as "mission"s here.

## Mission cargo and passengers

```html
"mission cargo"
	"player ships"
		<missionID> <shipID> <count#>
		<missionID> <shipID> <count#>
		...
"mission passengers"
	"player ships"
		<missionID> <shipID> <count#>
		<missionID> <shipID> <count#>
		...
```

Mission cargo and passengers that are currently on ships that are not at the same planet as the player. **(v. 0.10.0)**
In each entry, the first value is the UUID of the mission these cargo or passengers are associated with, the second is the UUID of the ship they are on, and the third is the number of tons of cargo or number of passengers of that particular mission on that particular ship.
Mission cargo and passengers not listed here, whether from missions that do not appear, or where the full amount of cargo or number of passengers is not accounted for here, is considered to be on the same planet as the player and will be distributed to available ships on take off, if possible. If not, the player will have to abort the take off, or abort some of their missions.
Prior to **v. 0.10.0**, mission cargo and passengers on escorts that were not on the same planet as the player were not tracked, and so, upon loading a save file, all mission cargo and passengers would be considered to be on the player's planet, even if they were previously on ships elsewhere.

## Available jobs and missions

```html
"available job" <name>
	...
"available job" <name>
	...
...
"available mission" <name>
	...
"available mission" <name>
	...
...
```

Instantiated missions that are available at the current planet, but have not yet been offered.
These are discarded on take off.

## Mission sorting

```html
"sort type" <value#>
["sort descending"]
["separate deadline"]
["separate possible"]
```

```html
"sort type" <value#>
```

```html
["sort descending"]
```

```html
["separate deadline"]
```

```html
["separate possible"]
```

# Conditions

```html
conditions
	<condition> <value#>
	<condition> <value#>
	...
```

# Gifted ships

```html
"gifted ships"
	<name> <shipID>
	<name> <shipID>
	...
```

# Upcoming events

```html
event
	...
event
	...
...
```

# Changes to the universe

```html
changes
	...
```

# Economy

```html
economy
	purchases
		<system> <commodity> <count#>
		<system> <commodity> <count#>
		...
	system <commodity> <commodity> <commodity> ...
	<system> <supply> <supply> <supply> ...
	<system> <supply> <supply> <supply> ...
	...
```

# Destroyed persons

```html
"destroyed" <person>
"destroyed" <person>
...
```

[Persons](CreatingPersons) that have been captured or destroyed, either by the player or NPCs.

# Places this pilot has visited

## Systems and planets

```html
visited <system>
visited <system>
...
"visited planet" <planet>
"visited planet" <planet>
...
```

Systems and planets this pilot has visited.

## Minable harvest locations

```html
harvested
	<system> <outfit>
	<system> <outfit>
	...
```

Systems where this pilot has harvested outfits from minables, such as minable asteroids, and what items can be found in them.

# Logbook

```html
logbook
	<day#> <month#> <year#>
		<text>
		<text>
		...
	<day#> <month#> <year#>
		...
	...
	<category> <heading>
		<text>
		<text>
		...
	<category> <heading>
		...
	...
```

There are two types of logbook entry:
- dated entries
- special entries

Dated entries are stored by the in game date on which they were added.
Special entries have a category and a heading.

# Starting conditions

```
start <start>
	system <system>
	planet <planet>
	date <day#> <month#> <year#>
	account
		...
```

The name of the start this pilot was created with, as well as the planet and system the player started on, the in game date the pilot started from, and details of the starting state of the "account".
This 

# Plugins

```html
plugins
	<plugin>
	<plugin>
	...
```

The plugins that were installed and active when this save was made.
