# Table of Contents



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
	system <system>
	planet <planet>
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

The first and last names of this pilot.

```html
date <day#> <month#> <year#>
```

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

Present if the player has clearance on the current planet. Without clerance, planetary services are unavailable.

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

```html
"map coloring" <value#>
```

```html
"map zoom" <value#>
```

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

```html
"reputation with"
	<government> <reputation#>
	<government> <reputation#>
	...
```

This pilot's reputation with each government.

```html
"tribute received"
	<planet> <credits#>
	<planet> <credits#>
	...
```

The number of credits this pilot receives each day from planets they have dominated. **(v. 0.10.1)**
Prior to **v. 0.10.1**, this information appeared in the save file in the conditions section.
While the way in which they are stored in the save file was changed, tribute amounts remain accessible and adjustable via [conditions](Player-Conditions#modifiable).

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
			<outfit> <count~>
			<outfit> <count~>
			...
	crew <crew#>
	fuel <fuel#>
	shields <shields#>
	hull <hull#>
	position <x#> <y#>
	...
	system <system>
	planet <planet>
...
```

The ships this pilot owns and their installed outfits.
In addition to many of the child nodes present on [ship definitions](CreatingShips#data), instantiated ships also have additional information.




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

**(v. 0.9.13)**

```html
licenses
	<license>
	<license>
	...
```

**(v. 0.10.1)**

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

```html
credits <credits#>
```

The number of credits this pilot has.

```html
"salaries income"
	<salary> <credits#>
	<salary> <credits#>
	...
```

The names of salaries this pilot has and the number of credits they receive each day from each.
Prior to **v. 0.10.1**, this information was stored as conditions.

```html
salaries <credits#>
maintenance <credits#>
```

The number of credits owed for overdue crew slaries and maintenance payments.
Prior to **v. 0.10.1**, this information was stored as conditions.

```html
score <score#>
```

This pilot's credit score. The higher the score, the lower the interest rate on mortgages taken out from the bank will be.
Ranges from 400 to 800.

```html
history
	<credits#>
	<credits#>
	...
```

This pilot's net worth over the last 100 days. Used to determine the maximum mortgage amount offered by the bank.

```html
mortgage (Mortgage | Fine | Debt)
	principal <credits#>
	interest <value#>
	term <days#>
mortgage ...
	...
...
```



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

```html
basis
	<commodity> <value#>
	<commodity> <value#>
	...
```

```html
stock
	<outfit> <count#>
	<outfit> <count#>
	...
```

Outfits not normally sold by the current planet that are in stock because the player has sold them here.
In stock outfits are cleared on take off.

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

```html
mission <name>
	...
mission <name>
	...
...
```

This pilot's active missions.
Normal missions are listed first. Then jobs, however, jobs are still listed as "mission"s here.

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

```html
"sort type" <value#>
["sort descending"]
["separate deadline"]
["separate possible"]
```

```html
conditions
	<condition> <value#>
	<condition> <value#>
	...
```

```html
"gifted ships"
	<name> <shipID>
	<name> <shipID>
	...
```

```html
event
	...
event
	...
...
```

```html
changes
	...
```

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

```html
"destroyed" <person>
"destroyed" <person>
...
```

[Persons](CreatingPersons) that have been captured or destroyed, either by the player or NPCs.

```html
visited <system>
visited <system>
...
"visited planet" <planet>
"visited planet" <planet>
...
```

Systems and planets this pilot has visited.

```html
harvested
	<system> <outfit>
	<system> <outfit>
	...
```

Systems where this pilot has harvested outfits from minables, such as minable asteroids, and what items can be found in them.

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

```
start <start>
	system <system>
	planet <planet>
	date <day#> <month#> <year#>
	account
		...
```

```html
plugins
	<plugin>
	<plugin>
	...
```

The plugins that were installed and active when this save was made.
