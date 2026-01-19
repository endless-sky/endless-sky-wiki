# Table of Contents

* [Introduction](#introduction)
* [Galaxies](#galaxies)
* [Systems](#systems)
  * [Objects](#objects)
* [Planets](#planets)
  * [Tribute](#tribute)
* [Wormholes](#wormholes)
* [Landing Messages](#landing-messages)
* [Solar Attributes](#solar-attributes)
* [Ambient Music](#ambient-music)

# Introduction

A useful tool for creating planets and systems is the [Endless Sky editor](https://github.com/endless-sky/endless-sky-editor). This most especially helps with the positioning of systems and the distance/period of planets in a system.

The [syntax](DataFormat#grammar-specifications) for the definition of a galaxy, system, or planet is:

```html
galaxy <name>
	pos <x#> <y#>
	sprite <sprite>

system <name>
	"display name" <name>
	inaccessible
	hidden
	shrouded
	pos <x#> <y#>
	government <name>
	attributes <attribute>...
	music <sound>
	arrival [<distance#>]
		link <distance#>
		jump <distance#>
	departure [<distance#>]
		link <distance#>
		jump <distance#>
	habitable <distance#>
	belt <distance#> [<weight#?]
	ramscoop
		universal <value#>
		addend <value#>
		multiplier <value#>
	"invisible fence" <distance#>
	"jump range" <distance#>
	haze <sprite>
	link <system>
	asteroids <name> <count#> <energy#>
	minables <name> <count#> <energy#>
	trade <commodity> <cost#>
	fleet <name> <period#>
		[to spawn]
			<condition> <comp> <value>
			(has | not) <condition>
			never
			(and | or)
				...
	raid <fleet> [<min-attraction#> [<max-attraction#>]]
	hazard <name> <period#>
		[to spawn]
			<condition> <comp> <value>
			(has | not) <condition>
			never
			(and | or)
				...
	"starfield density" <density#>
	object [<name>]
		sprite <sprite>
			scale <scale#>
		distance <distance#>
		period <period#>
		offset <offset#>
		hazard <name> <period#>
			[to spawn]
				<condition> <comp> <value>
				(has | not) <condition>
				never
				(and | or)
					...
		visibility <maxDistance> [<minDistance>]
		object [<name>]
			...

planet <name>
	"display name" <name>
	attributes <attribute>... "requires: <attribute>"
	landscape <sprite>
	music <sound>
	description <text>
	spaceport <text>
	to know
		<condition-set>
	to land
		<condition-set>
	to access (outfitter | shipyard)
		<condition-set>
	port [<name>]
		recharges <recharge-type>...
			...
		services <service-type>...
			...
		to access
			<condition-set>
		to bribe
			<condition-set>
		to recharge <recharge-type>...
			<condition-set>
		to service <recharge-type>...
			<condition-set>
		news
		description <text>
	government <name>
	shipyard <name>
	outfitter <name>
	"required reputation" <reputation#>
	bribe <bribe#>
	"bribe threshold" <reputation#>
	security <security#>
	tribute <credits#>
		threshold <rating#>
		fleet <name> <count#>
```

# Galaxies

```html
galaxy <name>
	pos <x#> <y#>
	sprite <sprite>
```

"Galaxies" are what serve as background images on the map. This includes the image of the Milky Way, as well as all the labels on the map for various regions of space.

```html
galaxy <name>
```

The name of a galaxy must be unique.

```html
pos <x#> <y#>
```

The position on the map where this galaxy (and its sprite) is centered.

```html
sprite <sprite>
```

The image that is created at this galaxy's position.

# Systems

```html
system <name>
	"display name" <name>
	inaccessible
	hidden
	shrouded
	pos <x#> <y#>
	government <name>
	attributes <attribute>...
	music <sound>
	arrival [<distance#>]
		link <distance#>
		jump <distance#>
	departure [<distance#>]
		link <distance#>
		jump <distance#>
	ramscoop
		universal <value#>
		addend <value#>
		multiplier <value#>
	habitable <distance#>
	belt <distance#> [<weight#>]
	"invisible fence" <distance#>
	"jump range" <distance#>
	haze <sprite>
	link <system>
	asteroids <name> <count#> <energy#>
	minables <name> <count#> <energy#>
	trade <commodity> <cost#>
	fleet <name> <period#>
		[to spawn]
			<condition> <comp> <value>
			(has | not) <condition>
			never
			(and | or)
				...
	raid <fleet> [<min-attraction#> [<max-attraction#>]]
	hazard <name> <period#>
		[to spawn]
			<condition> <comp> <value>
			(has | not) <condition>
			never
			(and | or)
				...
	"starfield density" <density#>
	object [<name>]
		sprite <sprite>
			scale <scale#>
		distance <distance#>
		period <period#>
		offset <offset#>
		hazard <name> <period#>
			[to spawn]
				<condition> <comp> <value>
				(has | not) <condition>
				never
				(and | or)
					...
		object [<name>]
			...
```

Systems are the locations that ships are capable of being and can contain planets that ships can land on.

```html
system <name>
```

The true name of a system must be unique. Use this name to internally reference systems in missions, events, etc.

```html
"display name" <name>
```

Since **v. 0.10.11**: Specifies the name displayed to the player. If not defined, it defaults to the true name of the system. Multiple systems can share the same display name.

```html
inaccessible
```

Added in **v. 0.10.0**, systems which are inaccessible aren't able to be traveled to or interacted with in any way. The system cannot be seen, clicked, or searched, and any links to or from the system will not be visible or usable.

```html
hidden
```

Systems which are hidden are not able to be seen unless they are linked to a visited system or a mission highlights them. They can't be seen by simply being within view range of systems the player has visited. Once the system has been seen by visiting a linked system, it behaves normally.

```html
shrouded
```

Added in **v. 0.10.5**, systems which are shrouded are not able to be seen unless they are within visible range of the player's current location, are linked to a visited system, or a mission highlights them. In addition, if the player is not currently within a shrouded system, then they are not able to see the contents of the system (e.g. the planets in the system, the system's name, etc.). Shrouded systems are effectively forgotten every time the player leaves them.

```html
pos <x#> <y#>
```

The position on the map where this system is located.

```html
government <name>
```

The [government](CreatingGovernments) that owns/controls this system. This is what is shown when looking at a system on the map using the government key.

```html
attributes <attribute>...
```

The attributes of this system that control what [missions](CreatingMissions) will be offered within it.

If there are no inhabited planets in a system, then it is automatically given an `uninhabited` attribute. 

```html
music <sound>
```

[Music](#ambient-music) that is played while in this system.

```html
arrival [<distance#>]
	link <distance#>
	jump <distance#>
departure [<distance#>]
	link <distance#>
	jump <distance#>
```

An additional distance at which ships arrive (or depart **(v0.9.17)**) in this system. If just `<distance#>` is provided, then both hyperdrive and jump drive travel into or out of a system has its distance moved. The `link <distance#>` and `jump <distance#>` lines are optional and allow for tweaking of the distance of each jump method. If a distance is given and then a link or jump distance is provided, the link or jump distance will override the distance. If a distance is given without a value and then a link or jump distance is provided, the other travel method will be unaffected.

Negative values are allowed for link arrival distances, and will move the point at which the ship enters the system away from the direction it is coming from, rather than towards it as positive values do.
Providing a value other than zero will cause ships to jump into the system from a distance relative to the system center, as opposed to jumping in relative to a target planet.

```html
ramscoop
	universal <value#>
	addend <value#>
	multiplier <value#>
```

Beginning in **v. 0.10.0**, individual systems can be given modifiers which influence fuel gained through the use of ramscoops, adding to the customization that [solar attributes](MapData#solar-attributes) provide. The `universal` keyword determines whether the universal ramscoop (a minor amount of fuel gain applied to all ships even without the presence of the "ramscoop" attribute) functions in the system, with 1 being true and 0 being false. The `addend` keyword adds (or subtracts) an amount of fuel gained per frame while in the system. If the addend subtracts more fuel than is provided, then no fuel is gained; there is no fuel lost due to a negative addend. The `multiplier` keyword multiplies the fuel normally gained. The multiplier is applied first before the addend. The default values are `universal 1`, `addend 0`, and `multiplier 1`.


```html
habitable <distance#>
```

The distance of the "Goldilocks zone" in this system. When selecting a system on the map, the orbits of the objects in the system will be colored in relation to their distance from this value. If an object's distance is near this value, its orbit will be green. If an object's distance is farther than this value, then its orbit will fade from light to dark blue the further it is. If an object's distance if closer than this value, then its orbit will fade from yellow to red the closer to the system center it is.

```html
belt <distance#> [<weight#>]
```

The distance from the system center at which minable asteroids in this system will orbit. **Beginning in v. 0.9.15** a system can define multiple asteroid belts for a single system. Each belt can be given a weight that functions similarly to fleet [variants](CreatingFleets#variants), defining the probability that any given asteroid will appear in that belt. If no weight is given then a default weight of 1 is used.

```html
"invisible fence" <distance#>
```

Added in **v. 0.10.0**. The distance from the system center beyond which NPC ships will no longer travel, unless they have the `unconstrained` personality or are your escorts. The default distance for all systems is 10,000.

```html
"jump range" <distance#>
```

The distance to which ships can jump from this system. This overrides the jump range of any ships in the system.

```html
haze <sprite>
```

The haze that is created for the background of this system. If no haze is given, then the default blue haze is used.

```html
link <system>
```

The name of a system that this system is linked to. Linked systems can be traveled between using a hyperdrive or jump drive regardless of the distance. Systems can be linked to multiple other systems at once.

```html
asteroids <name> <count#> <energy#>
minables <name> <count#> <energy#>
```

The name of the asteroids in this system, as well as the number of the asteroids and their energy. The energy of an asteroid determines how fast it moves and rotates, with higher values meaning faster asteroids. A random value between 0 and the energy value is used for each of the asteroids when they are created, meaning that high energy values may still result in slow asteroids.

If an asteroid is minable, then it uses the `minables` keyword. Unlike normal asteroids, which travel randomly throughout the system and are [tiled](TiledAsteroids), minable asteroids will orbit around the system's `belt` distance. Note that minable asteroids names refer to a defined [minable](CreatingMinables), while normal asteroid names refer to the sprite name.

```html
trade <commodity> <cost#>
```

A type of commodity sold at the planets in this system and its cost.

```html
fleet <name> <period#>
	[to spawn]
		<condition> <comp> <value>
		(has | not) <condition>
		never
		(and | or)
			...
```

The name of a [fleet](CreatingFleets) that is spawned in this system with a certain period. The period of a fleet is the average number of frames between each spawning of this specific fleet, with there being 60 frames in a second. A random number from 0 to `period - 1` is rolled each frame, and if the result lands on 0 then a fleet is spawned.

Beginning in **v. 0.10.11**, fleets can be given `to spawn` conditions. A fleet will only appear if the `to spawn` condition is fulfilled.

```html
raid <fleet> [<min-attraction#> [<max-attraction#>]]
```

The name of a [fleet](CreatingFleets) that is spawned in this system when the player's raid attraction is high enough. More details on raid fleets can be found in [CreatingGovernments](CreatingGovernments#raid). **(v. 0.10.3)**

```html
"no raids"
```

If present, no raid fleets will ever spawn in this system, whether they be from the system's government or the system itself. **(v. 0.10.3)**

```html
hazard <name> <period#>
	[to spawn]
		<condition> <comp> <value>
		(has | not) <condition>
		never
		(and | or)
			...
```
The name of a [hazard](CreatingHazards) that is created in this system within a certain period. The period of a hazard follows the same behavior as the period of a fleet. Once a hazard is created, the behavior of the hazard is dictated by the hazard itself. The origin of any hazards defined here is the system center. **(v. 0.9.13)**

Beginning in **v. 0.10.11**, hazards can be given `to spawn` conditions. A hazard will only appear if the `to spawn` condition is fulfilled.

```html
"starfield density" <density#>
```

The density of the stars in the background. The default value is 1. If a system has a starfield density of 0 then it will have no stars at all. **(v. 0.10.0)**

## Objects

```html
object [<name>]
	sprite <sprite>
		scale <scale#>
	distance <distance#>
	period <period#>
	offset <offset#>
	hazard <name> <period#>
		[to spawn]
			<condition> <comp> <value>
			(has | not) <condition>
			never
			(and | or)
				...
	visibility <maxDistance> [<minDistance>]
	object [<name>]
		...
```

Objects are the stars, planets, moons, or stations that are found within a system. Each object defines a different star, planet, moon, etc.

```html
object [<name>]
```

If an object is named, then it can be landed on, and must have a [planet](#planets) defined for it.

**Deprecated since v. 0.10.0**:

If this name matches the name of an object in another system, then a wormhole is created. If this name matches the name of another object in the same system, then landing on any of these objects simply lands you on the planet associated with these objects.

```html
sprite <sprite>
```

The sprite that is created at this object's position.

Note that the sprite path is used to classify stellar objects: if it begins with "star/", it's a star, if it begins with "planet/station", it's a station, while any other object that has a parent object is classified as a moon. These classifications are used (or can be used in future versions) for messages, radar display and/or AI decisions.

```html
scale <scale#>
```

The multiplier applied to the dimensions of this object's sprite when displayed in game. This value defaults to 1. In order to increase the visual fidelity of stations, they are stored at double resolution, and scaled down to half size.

Additional properties for animated sprites can be found in the [sprite data](SpriteData) page.

```html
distance <distance#>
```

The distance from the system center at which this object will orbit.

```html
period <period#>
```

The number of days that it takes this object to orbit around the system center. Objects only move when the day changes, meaning that they will not move while the player is launched and in the system.

```html
offset <offset#>
```

The number of degrees at which this object is shifted in its orbit from the default, allowing for multiple objects to share the same orbit without overlapping.

```html
hazard <name> <period#>
```

A system hazard with behavior as described above, only with its origin on this object instead of at the system center. An object can have multiple different hazards attached to it. **(v0.9.15)**

Beginning in **v. 0.10.11**, hazards can be given `to spawn` conditions. A hazard will only appear if the `to spawn` condition is fulfilled.

```
visibility <maxDistance> [<minDistance>]
```

The range from which the object is visible. `maxDistance` is the range where the object becomes invisible, and `minDistance` is where it becomes fully visible. If `minDistance` is greater, the object disappears as the player gets closer. **(v0.10.11)**

```
object [<name>]
	...
```

Objects are capable of having objects as children. This allows for the creation of moons that orbit planets. The distance and period of these children objects is defined relative to the parent object. That is, instead of the distance being from the system center, the distance is from the center of the object it is orbiting.

# Planets

```html
planet <name>
	"display name" <name>
	attributes <attribute>... "requires: <attribute>"
	landscape <sprite>
	music <sound>
	description <text>
	spaceport <text>
	to know
		<condition-set>
	to land
		<condition-set>
	to access (outfitter | shipyard)
		<condition-set>
	port [<name>]
		recharges <recharge-type>...
			...
		services <service-type>...
			...
		to access
			<condition-set>
		to bribe
			<condition-set>
		to recharge <recharge-type>...
			<condition-set>
		to service <recharge-type>...
			<condition-set>
		news
		description <text>
	government <name>
	shipyard <name>
	outfitter <name>
	"required reputation" <reputation#>
	bribe <bribe#>
	"bribe threshold" <reputation#>
	security <security#>
	wormhole <name>
	tribute <credits#>
		threshold <rating#>
		fleet <name> <count#>
	"tribute hails"
		"already paying" <phrase>
		"undefined" <phrase>
		"unworthy" <phrase>
		"fleet launching" <phrase>
		"fleet undefeated" <phrase>
		"surrendered" <phrase>
```

Planets are landable objects, and are where players are capable of buying and selling ships, finding jobs, and discovering missions.

```html
planet <name>
```

The true name of a planet must be unique. Use this name to internally reference planets in missions, events, system objects, etc.

If a planet is being used to define a wormhole (i.e. an objects that is named in multiple systems), then giving it a spaceport will cause NPCs to "land" on the wormhole.

**Deprecated since v. 0.10.0**:

Additionally, giving it a description will cause the wormhole to create a link on the map when it has been discovered.

**Since v. 0.10.0**: Create a `wormhole` node and then assign it to the planet using `wormhole <name>` (see below).

```html
"display name" <name>
```

Since **v. 0.10.11**: Specifies the name displayed to the player. If not defined, it defaults to the true name of the planet. Multiple planets can share the same display name.

```html
attributes <attribute>... "requires: <attribute>"
```

The list of attributes that will be used to determine what [missions](CreatingMissions) should be offered on this planet.

If the `"requires: <attribute>"` phrase is used, then a ship must have the listed attribute in order to land on this planet.

There are three attributes that get automatically added to a planet: `spaceport`, `shipyard`, and `outfitter`. As their names imply, they are added to a planet if that planet has a spaceport, shipyard, or outfitter. If one tries to give one of these attributes to a planet that does not actually have a spaceport, shipyard, or outfitter, then that attribute will be ignored.

One other special attribute is the `uninhabited` attribute. If listed, then the planet's trading, job board, bank, and hire crew panels will be gone, the planet will show as uninhabited on the map, and the planet will be incapable of fining the player unless a security value is provided.

```html
landscape <sprite>
```

The landscape image that is shown when landed on this planet.

```html
music <sound>
```

The [music](#ambient-music) that is played while landed on this planet.

```html
description <text>
```

The description that is shown when first landing on a planet. Beginning in **v. 0.10.9**, each line can also be given a `to display` node with a [condition set](Player-Conditions):
```html
description "This planet is a very nice place."
   to display
       not "terrible things happened"
description "Frog People invaded and the planet is now almost devoid of life."
   to display
       has "terrible things happened"
```

```html
spaceport <text>
```

The description of the spaceport after clicking the spaceport button. Beginning in **v. 0.10.9**, each line can also be given a `to display` node with a [condition set](Player-Conditions).

```html
to know
	<condition-set>
to land
	<condition-set>
to access (outfitter | shipyard)
	<condition-set>
```

Beginning in **v. 0.10.17**, condition sets can be used to further control the behavior of a planet based on the player's current [conditions](https://github.com/endless-sky/endless-sky/wiki/Player-Conditions).

* `to know`: Controls whether the object that the planet is associated with is seen as a landable object. If this is false, the object won't appear landable.
* `to land`: Controls whether the player can land on the planet. If false, you won't be able to land, and hailing the planet will tell you that you are denied landing access and cannot bribe your way onto the planet.
* `to access (outfitter | shipyard)`: Controls whether the outfitter or shipyard buttons are available after landing. The planet must also have `outfitter` or `shipyard` nodes present in order for the shops to become available.

```html
port [<name>]
	recharges <recharge-type>...
		...
	services <service-type>...
		...
	to access
		<condition-set>
	to bribe
		<condition-set>
	to recharge <recharge-type>...
		<condition-set>
	to service <recharge-type>...
		<condition-set>
	news
	description <text>
```

Beginning in **v. 0.10.5**, how exactly the port of a planet behaves can be controlled more precisely using the `port` keyword. The `spaceport` keyword is still supported and is shorthand for a port named "Spaceport" with all recharge and service capabilities.

If a port is given a name, then that will display on the spaceport button instead of the usual "Spaceport" text.

If a port has no `recharges` node, then it will not recharge anything on ships that land on the planet. Only those recharge types that are listed will be used, and they can be listed on the same line as `recharges` or as one children of the `recharges` node with one item per line. The allowable recharge types are as follows:
* `shields`: recharges the shields of any ships that land.
* `hull`: recharges the hull of any ships that land.
* `energy`: recharges the energy of any ships that land.
* `fuel`: recharges the fuel of any ships that land.
* `all`: shorthand for all recharge types.

If a port has no `services` node, then it will not offer any services. As with `recharges`, only those service types that are listed are used, and they may be listed in the same ways. The allowable service types are as follows:
* `trading`: the player can trade commodities here.
* `job board`: the player can access the job board here.
* `bank`: the player can access the bank here.
* `hire crew`: the player can hire crew here.
* `offers missions`: missions can be offered when entering the port.

By default, ports don't display spaceport news when you enter them. To display news, add the `news` token.

The description text of a port behaves the same way as the text following a `spaceport` node, and is the text that appears when you click the port button.

Beginning in **v. 0.10.17**, condition sets can be used to further control the behavior of a port based on the player's current [conditions](https://github.com/endless-sky/endless-sky/wiki/Player-Conditions).

* `to access`: Controls whether the player can access anything that is a part of the spaceport upon landing, including any port services, recharging types, either of the shops, and the port button itself.
* `to bribe`: If true, you will be required to pay a bribe before landing.
* `to recharge`: Controls whether individual recharge types are available to the player. A single `to recharge` node can be used to lock multiple recharge types. For example, `to recharge hull shields` would lock both hull and shield repairs. Multiple `to recharge` nodes can be present at once. Only the last set of conditions for a particular recharge type will be used. The recharge type must also be present in the `recharges` node in order for this to have any effect. For example, if `to recharge hull` is true but the port doesn't have `recharges all` or `recharges hull`, then the port still won't repair your ships.
* `to service`: Controls whether individual service types are available to the player. Behaves the same as `to recharge`, but for service types.

```html
government <name>
```

Planets are capable of having a different [government](CreatingGovernments) than the system. If no government for the planet is specified, then its government is the same as the system government. Otherwise, the government listed here is used.

```html
shipyard <name>
outfitter <name>
```

The shipyards and outfitters that are available on this planet. A new shipyard or outfitter line must be used for each shipyard or outfitter.

```html
"required reputation" <reputation#>
```

The amount of reputation that the player is required to have with this planet's government before being allowed to land. Negative values will allow the player to land on a planet that a player is hostile with as long as their reputation is not below the listed value.

```html
bribe <bribe#>
security <security#>
```

The behavior that this planet has when the player has illegal goods or outfits. The bribe number is a multiplier that modifies the severity of any fines, while the security number is a value between 0 and 1 that dictates the chance of a player's ship being scanned.

If no bribe is specified, then a default bribe of 0.01 is used.
If no security is specified, then a default security of 0.25 is used. 

```html
"bribe fraction" <reputation#>
```

Beginning in **v. 0.10.17**, a minimum reputation for accepting bribes can be provided. If this value is set to anything other than zero, this planet will not accept bribes to provide landing clearance when the player's reputation is below this value.
The default value is zero, so bribes can be accepted regardless of how low the player's reputation may be.

```html
wormhole <name>
```

**(v. 0.10.0)** Assigns a wormhole to this planet. Departing from this planet will teleport the player to the destination specified inside the wormhole node.

## Tribute

```html
tribute <credits#>
	threshold <rating#>
	fleet <name> <count#>
	"daily reputation penalty" <amount#>
```

The player is capable of demanding tribute from certain planets. If no tribute is specified, then the planet will always respond negatively to the player's demand for tribute.

```html
tribute <credits#>
```

The number of credits that will be paid to the player per day if this planet has been dominated.

```html
threshold <rating#>
```

The combat rating that the player must have before being capable of demanding tribute from this planet. If no threshold is specified, then a default threshold of 4000 is used.

```html
fleet <name> <count#>
```

The fleet that is spawned from this planet if the player demands tribute from it, and how many times that fleet is spawned. More than one type of fleet is capable of being spawned from a planet when demanding tribute.

```html
"daily reputation penalty" <amount#>
```

The amount of reputation with the planet's government that is subtracted every day while the player receives tribute. **(v. 0.10.17)**

```html
"tribute hails"
	"already paying" <phrase>
	"undefined" <phrase>
	"unworthy" <phrase>
	"fleet launching" <phrase>
	"fleet undefeated" <phrase>
	"surrendered" <phrase>
```

With these hails, you can customize how a planet responds when the player demands tribute from it. Tribute hails defined on a planet will override tribute hails defined by the government. **(v0.10.17)**
The `surrendered` phrase should always have a `<credits>` substitution for the planet to tell the player how much money they will receive per day.
The default values if a key is not provided are:
```bash
"already paying" "We are already paying you as much as we can afford."
"undefined" "Please don't joke about that sort of thing."
"unworthy" "You're not worthy of our time."
"fleet launching" "Our defense fleet will make short work of you."
"fleet undefeated" "We're not ready to surrender yet."
"surrendered" "We surrender. We will pay you <credits> per day to leave us alone."
```
Note that you still need to use phrases and not sentences.


# Wormholes

```
wormhole <name>
	"display name" <name>
	mappable
	link <from> <to>
	color (<r#> <g#> <b#> | <name>)
```

"Wormholes" are planets that transport you to a different system.

```html
wormhole <name>
```

The name of a wormhole must be unique.

```html
"display name" <name>
```

The display name of this wormhole, overrides the planet's name. If it is not provided, it defaults to `???`.

```html
mappable
```

Whether this wormhole's links are drawn on the map.

```html
link <from> <to>
```

Adds a link from a given system to another. You can have any number of these attributes. As a general rule, if a wormhole is present in a system it should have a corresponding `link` node to describe where its destination is.

```html
color (<r#> <g#> <b#> | <name>)
```

Defines the color of the arrows in the map panel and the planet label of a "mappable" wormhole.
This can either be given as RGB values or can refer to a named stock color.
By default, the color named `"map wormhole"` will be used.

# Landing messages

```html
"landing message" <text>
	<sprite>
	...
```

Landing messages are the message that is shown if attempting to land on an uninhabited object. The text of a landing message is what is shown, while the children of a landing message are the sprites that show this message.

# Solar attributes

```html
star <sprite>
	power <power#>
	wind <wind#>
	icon <icon>
```

There are certain attributes that a ship is capable of having that will change in effectiveness based off of the stars in the system.

```html
star <sprite>
```

The sprite listed here is the sprite of the star that will affect ships in the system. If a system has multiple stars, then the values of each star are added together.

```html
power <power#>
```

The power of a star impacts the effectiveness of a ship's solar collection and solar heat attributes.

```html
wind <wind#>
```

The wind of a star impacts the effectiveness of a ship's ramscoops.


```html
icon <icon>
```

The sprite listed here is the sprite of the star icon from the map screen when the starry map view is enabled. **v. 0.10.11**

# Ambient music

```html
music <path>
```

Both systems and planets can be assigned music that plays while the player is there.
The music files should be of the '.mp3' or '.flac' format and placed in the 'sounds' folder.
The path should be the relative path within the 'sounds' folder without the file extension.
For example, a file named 'machinery.mp3' in a folder 'ambient' inside the 'sounds' folder would be listed in a planet or system definition as:
```css
	music ambient/machinery
```
