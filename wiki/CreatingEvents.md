An "event" is something that happens on a certain in-game date, and can modify both the galactic state (such as system and planet characteristics, government relationships, or even the ships belonging to various fleets) and the player's knowledge (including the internal [conditions](Player-Conditions) that track what the player has done, and which systems or planets are known to the player in the Galactic Map).

The basic syntax of an event definition is:

```html
event <name>
	date <day#> <month#> <year#>
	visit <system>
	unvisit <system>
	"visit planet" <planet>
	"unvisit planet" <planet>
	galaxy <name>
		{galaxy specification...}
	system <name>
		{system specification...}
	link <system> <other>
	unlink <system> <other>
	government <name>
		{government specification...}
	fleet <name>
		{fleet specification...}
	planet <name>
		{planet specification...}
	news <name>
		{news specification...}
	shipyard <name>
		{shipyard specification...}
	outfitter <name>
		{outfitter specification...}
	substitutions
		{substitutions specification...}
	<conditions>
```

Each event has a name, and can specify what date it happens on. (Most events, however, will be triggered by missions and therefore will not have a fixed date.) The event can then list any number of changes to any of the data types listed above. For example:

```c++
event "war begins"
	date 4 7 3014
	system "Gamma Corvi"
		government "Free Worlds"
		fleet "Small Southern Merchants" 600
		fleet "Large Southern Merchants" 1400
		fleet "Small Free Worlds" 600
		fleet "Large Free Worlds" 1300
```

In general, anything specified in an event will overwrite the previous value of that data element. (For example, specifying a new `government` replaces the previous one.) For data elements that can take a list of an arbitrary number of values (such as the `attributes` of a system or planet), if an event modifies that element, by default it replaces all of the previous values. Starting in v0.9.7 you can modify such elements without replacing the previous value by using the add and remove keywords:

```c++
event "goodbye world"
	planet "Earth"
		remove attributes "urban" "tourism"
		add attributes "barren wasteland"
		remove shipyard
		remove outfitter
```

While many elements support the "add" & "remove" keywords, not all do. Often, if an element has a complex specification (such as a planet's `tribute` definition), only the "remove" keyword will work. The "remove" keyword can also be used with any element of a `system` or `planet` to reset it to its default value, as shown in the Earth example above for its `shipyard`s.

In general, it's not possible to "remove" a specific value for elements whose values do not have names. For example, you can't remove a single stellar `object` from a `system` because there would be no way to specify which object to remove. However, you can use the "add" keyword in a fleet definition to add a variant without replacing all the existing variants:

```c++
event "new ship available"
	fleet "Small Southern Pirates"
		add variant 5
			"Doomship"
```

# Basic event characteristics

For existing game data elements (like planets, fleets, and systems), only the specific elements being altered need to be included in the event.

```html
event <name>
```
The name of an event is used to automatically generate a player condition that indicates this particular event has occurred. Events should always have a unique name - if two events share a name, the net change applied will generally be the sum of both events.

```html
date <day#> <month#> <year#>
```
The in-game date on which this event will occur. For events referenced by missions, this field is often omitted (because the date is intended to be dynamic).

```html
unvisit <system>
visit <system>
```
A visited system provides information on the government, planets, trade, and neighbors of the named system when viewing the Galactic Map. Similarly, unvisiting a system will remove any existing knowledge the player has about the system (including its name) when viewing the map.
When events are processed, all `unvisit` actions are performed before any `visit` actions. Unvisiting a system will unvisit every planet in the given system.

```html
"unvisit planet" <planet>
"visit planet" <planet>
```
As with systems, visiting a planet allows the player to view information about it when viewing the Galactic Map, such as its government or whether the planet has a shipyard, or outfitter. Unvisiting a planet removes any existing knowledge the player has at that time.

```html
galaxy <name>
	pos <x#> <y#>
	sprite <path>
```
Update the named `galaxy`, including the `sprite` and position of the sprite on the Galactic Map. As with all images, the specified path is relative to the `images/` directory, e.g. `ui/my-galaxy` instead of `images/ui/my-galaxy`.

```html
system <name>
	inaccessible
	remove inaccessible
	hidden
	remove hidden
	shrouded
	remove shrouded
	"no raids"
	remove "no raids"
	pos <x#> <y#>
	government <gov>
	remove government
	music <path>
	remove music
	habitable <radius#>
	belt <radius#>
	haze <path>
	remove haze
	trade <commodity> <base-value#>
	remove trade
	[(add | remove)] attributes <value>...
	[remove] link <system>
	remove link
	[add] asteroids <name> <count#> <energy#>
	remove asteroids [<name>]
	[add] minables <name> <count#> <energy#>
	remove minables [<name>]
	[add] fleet <name> <period#>
	remove fleet [<name>]
	[add] hazard <name> <period#>
	remove hazard [<name>]
	[add] object [<planet>]
		sprite <path>
		distance <radius#>
		period <days#>
		offset <degrees#>
		object [<planet>]
			...
	remove object
		sprite <path>
		distance <radius#>
		period <days#>
		offset <degrees#>
	remove object
	...
```
Update the specified elements of the named `system`. The physical characteristics of systems (`habitable`, `belt`, `object`s etc.) are generally best previewed using the [map editor](https://github.com/endless-sky/endless-sky-editor).

Beginning in **v.0.9.15**, specific objects can be removed from systems. When removing an object, the sprite name (all sprite attributes are ignored), distance, period, and offset all need to match in order for the object to be removed. If an object is removed then all objects orbiting it are also removed. Planets don't count as orbiting stars, so removing the stars from a system won't delete all the planets.

```html
link <system> <other>
unlink <system> <other>
```
Adds or removes a hyperspace link between the given systems. (Unlike the definition of a `link` for a `system`, using the `link` keyword in an event creates both the system->other and other->system links.)

```html
government <name>
	...
```
Update the specified elements of the named `government`. For more information on the tokens and expected syntax, see the page on creating [governments](CreatingGovernments).

```html
fleet <name>
	government <government>
	names <phrase>
	fighters <phrase>
	cargo <value#>
	commodities <name>...
	outfitters <name>...
	"cargo settings"
		cargo <cargo#>
		commodities <commodity>...
		outfitters <outfitter>...
	[(add | remove)] personality [<flag>...]
	personality [<flag>...]
		confusion <value#>
	[add] variant [<weight#>]
		<ship> <count#>
		...
	remove variant
		...
```
Update the specified elements of the named `fleet`. To remove a variant from a fleet, the specified ships and their counts must be a permutation of an existing variant.

```html
planet <name>
	landscape <path>
	music <name>
	description <text>
	spaceport <text>
	government <name>
	[(add | remove)] attributes <value>...
	[(add | remove)] shipyard <name>
	[(add | remove)] outfitter <name>
	"required reputation" <rep-for-landing#>
	bribe <percentage#>
	security <percentage#>
	tribute <value#>
		threshold <needed-combat-rating#>
		fleet <name> <count#>
		...
	...
```
Update the specified elements of the named `planet`. Note that planetary music can only be in the `.mp3` or `.flac` format.

```html
news
	location
		{filter specification...}
	name
		{phrase specification...}
	portrait <sprite>...
		<sprite>
		...
	message
		{phrase specification...}
```
Update the specified elements of the named `news`. A common usage of an `event` to modify `news` would be to "activate" a news source by providing a location:
```c++
event "breaking news"
	news "terrorism on the rise"
		location
			government "Republic" "Syndicate"
```
For more information and examples, consult the page on [news](CreatingNews) or the [`news.txt`](https://github.com/endless-sky/endless-sky/tree/master/data/human/news.txt) file.

```html
shipyard <name>
	clear
	remove [<ship>]
	[add] <ship>
	...
```
Replace the contents of the named `shipyard` with the specified elements. To avoid removal of all preexisting ships, use the `add <ship>` syntax. `Clear` and `remove` with no arguments are functionally equivalent.

```html
outfitter <name>
	clear
	remove [<outfit>]
	[add] <outfit>
	...
```
Replace the contents of the named `outfitter` with the specified elements. To avoid removal of all preexisting outfits, use the `add <outfit>` syntax. `Clear` and `remove` with no arguments are functionally equivalent.

```html
substitution
	<text> <replacement>
		[<condition set>]
	...
```
Adds a new global substitution. If a previous global replacement existed with the same text and the new substitution has no condition set, then the new replacement will always be used. Consult the [substitutions](CreatingSubstitutions) page for more information.

# Modifying player conditions

An event definition can also include instructions to change the values of condition variables, such as the player's reputation with a specific government. More information on applied conditions is described in the [Player Conditions page](Player-Conditions#applied-condition-sets). All named events set an automatic condition variable named `event: <name>`, making it possible to create missions that cannot offer until after the event has occurred:

```c++
mission "show the war conversation"
	landing
	source
		government "Republic" "Syndicate" "Free Worlds"
	to offer
		has "event: war begins"
	on offer
		conversation
			...
```
The above mission requires the event named "war begins" to have occurred before it can offer.
