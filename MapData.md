# Table of Contents

* [Introduction](#intro)
* [Galaxies](#galaxies)
* [Systems](#systems)
  * [Objects](#objects)
* [Planets](#planets)
  * [Tribute](#tribute)
* [Landing Messages](#landing)
* [Solar Attributes](#solar)

<a name="intro">

# Introduction
</a>

A useful tool for creating planets and systems is the [Endless Sky editor](https://github.com/endless-sky/endless-sky-editor). This most especially helps with the positioning of systems and the distance/period of planets in a system.

The [syntax](DataFormat#grammar-specifications) for the definition of a galaxy, system, or planet is:

```html
galaxy <name>
	pos <x#> <y#>
	sprite <sprite>

system <name>
	hidden
	pos <x#> <y#>
	government <name>
	attributes <attribute>...
	music <sound>
	arrival [<distance#>]
		link <distance#>
		jump <distance#>
	habitable <distance#>
	belt <distance#>
	"jump range" <distance#>
	haze <sprite>
	link <system>
	asteroid <name> <count#> <energy#>
	minables <name> <count#> <energy#>
	trade <commodity> <cost#>
	fleet <name> <period#>
	hazard <name> <period#>
	object [<name>]
		sprite <sprite>
		distance <distance#>
		period <period#>
		offset <offset#>
		object [<name>]
			...

planet <name>
	attributes <attribute>... "requires: <attribute>"
	landscape <sprite>
	music <sound>
	description <text>
	spaceport <text>
	government <name>
	shipyard <name>
	outfitter <name>
	"required reputation" <reputation#>
	bribe <bribe#>
	security <security#>
	tribute <credits#>
		threshold <rating#>
		fleet <name> <count#>
```

<a name="galaxies">

# Galaxies
</a>

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

<a name="systems">

# Systems
</a>

```html
system <name>
	hidden
	pos <x#> <y#>
	government <name>
	attributes <attribute>...
	music <sound>
	arrival [<distance#>]
		link <distance#>
		jump <distance#>
	habitable <distance#>
	belt <distance#>
	"jump range" <distance#>
	haze <sprite>
	link <system>
	asteroid <name> <count#> <energy#>
	minables <name> <count#> <energy#>
	trade <commodity> <cost#>
	fleet <name> <period#>
	hazard <name> <period#>
	object [<name>]
		sprite <sprite>
		distance <distance#>
		period <period#>
		offset <offset#>
		object [<name>]
			...
```

Systems are the locations that ships are capable of being and can contain planets that ships can land on.

```html
system <name>
```

The name of a system must be unique.

```html
hidden
```

Systems which are hidden are not able to be seen unless they are linked to a visited system or a mission highlights them. Once the system has been seen by visiting a linked system, it behaves normally.

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

If there are no inhabited planets in a system, then it is automatically given an "uninhabited" attribute. 

```html
music <sound>
```

Music that is played while in this system.

```html
arrival [<distance#>]
	link <distance#>
	jump <distance#>
```

An additional distance at which ships arrive in this system. If just `arrival <distance#>` is provided, then both hyperdrive and jump drive travel into a system has its arrival distance moved. The `link <distance#>` and `jump <distance#>` lines are optional and allow for tweaking of the arrival distance of each jump method. If an arrival distance is given and then a link or jump distance is provided, the link or jump distance will override the arrival distance. If an arrival distance is given without a value and then a link or jump distance is provided, the other travel method will be unaffected.

Negative distances are allowed for link arrival distances, and will cause ships to arrive on the opposite side of the system from where they entered. Non-zero values (i.e. including this variable) will cause ships to jump into the system from a distance relative to the system center, as opposed to jumping in relative to a target planet.

```html
habitable <distance#>
```

The distance of the "Goldilocks zone" in this system. When selecting a system on the map, the orbits of the objects in the system will be colored in relation to their distance from this value. If an object's distance is near this value, its orbit will be green. If an object's distance is farther than this value, then its orbit will be fade from light to dark blue the further it is. If an object's distance if closer than this value, then its orbit will fade from yellow to red the closer to the system center it is.

```html
belt <distance#>
```

The distance at which minable asteroids in this system will orbit. 

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
asteroid <name> <count#> <energy#>
minables <name> <count#> <energy#>
```

The name of the asteroids in this system, as well as the number of the asteroids and their energy. The energy of an asteroid determines how fast it moves and rotates, with higher values meaning faster asteroids. A random value between 0 and the energy value is used for each of the asteroids when they are created, meaning that high energy values may still result in slow asteroids.

If an asteroid is minable, then it uses the `minable` keyword. Unlike normal asteroids, which travel randomly throughout the system and are [tiled](TiledAsteroids), minable asteroids will orbit around the system's `belt` distance. Note that minable asteroids names refer to a defined [minable](CreatingMinables), while normal asteroid names refer to the sprite name.

```html
trade <commodity> <cost#>
```

A type of commodity sold at the planets in this system and its cost.

```html
fleet <name> <period#>
```

The name of a [fleet](CreatingFleets) that is spawned in this system with a certain period. The period of a fleet is the average number of frames between each spawning of this specific fleet, with there being 60 frames in a second. A random number from 0 to `period - 1` is rolled each frame, and if the result lands on 0 then a fleet is spawned.

```html
hazard <name> <period#>
```
The name of a [hazard](CreatingHazards) that is created in this system within a certain period. The period of a hazard follows the same behavior as the period of a fleet. Once a hazard is created, the behavior of the hazard is dictated by the hazard itself. **(v. 0.9.13)**

<a name="objects">

## Objects
</a>

```html
object [<name>]
	sprite <sprite>
	distance <distance#>
	period <period#>
	offset <offset#>
	object [<name>]
		...
```

Objects are the stars, planets, moons, or stations that are found within a system. Each object defines a different star, planet, moon, etc.

```html
object [<name>]
```

If an object is named, then it can be landed on, and must have a [planet](#planets) defined for it. If this name matches the name of an object in another system, then a wormhole is created. If this name matches the name of another object in the same system, then landing on any of these objects simply lands you on the planet associated with these objects.

```html
sprite <sprite>
```

The sprite that is created at this object's position.

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

```
object [<name>]
	...
```

Objects are capable of having objects as children. This allows for the creation of moons that orbit planets. The distance and period of these children objects is defined relative to the parent object. That is, instead of the distance being from the system center, the distance is from the center of the object it is orbiting.

<a name="planets">

# Planets
</a>

```html
planet <name>
	attributes <attribute>... "requires: <attribute>"
	landscape <sprite>
	music <sound>
	description <text>
	spaceport <text>
	government <name>
	shipyard <name>
	outfitter <name>
	"required reputation" <reputation#>
	bribe <bribe#>
	security <security#>
	tribute <credits#>
		threshold <rating#>
		fleet <name> <count#>
```

Planets are landable objects, and are where players are capable of buying and selling ships, finding jobs, and discovering missions.

```html
planet <name>
```

The name of a planet must be unique, and will only be used it there is an object in a system that refers to this same name.

If a planet is being used to define a wormhole (i.e. an objects that is named in multiple systems), then giving it a description will cause the wormhole to create a link on the map when it has been discovered, and giving it a spaceport will cause NPCs to "land" on the wormhole.

```html
attributes <attribute>... "requires: <attribute>"
```

The list of attributes that will be used to determine what [missions](CreatingMissions) should be offered on this planet.

If the "requires: `<attribute>`" phrase is used, then a ship must have the listed attribute in order to land on this planet.

There are three attributes that get automatically added to a planet: spaceport, shipyard, and outfitter. As their names imply, they are added to a planet if that planet has a spaceport, shipyard, or outfitter. If one tries to give one of these attributes to a planet that does not actually have a spaceport, shipyard, or outfitter, then that attribute will be ignored.

One other special attribute is the "uninhabited" attribute. If listed, then the planet's trading, job board, bank, and hire crew panels will be gone, the planet will show as uninhabited on the map, and the planet will be incapable of fining the player unless a security value is provided.

```html
landscape <sprite>
```

The landscape image that is shown when landed on this planet.

```html
music <sound>
```

The music that is played while landed on this planet.

```html
description <text>
```

The description that is shown when first landing on a planet.

```html
spaceport <text>
```

The description of the spaceport after clicking the spaceport button.

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

The behavior that this planet has when the player has illegal goods or outifts. The bribe number is a multiplier that modifies the severity of any fines, while the security number is a value between 0 and 1 that dictates the chance of a player's ship being scanned.

If no security is specified, then a default security of 0.25 is used. 

<a name="tribute">

## Tribute
</a>

```html
tribute <credits#>
	threshold <rating#>
	fleet <name> <count#>
```

The player is capable of demanding tribute from certain planets. If no tribute is specified, then the planet will always respond negatively to the player's demand for tribute.

```html
tribute <credits#>
```

The number of credits that will be paid to the player per day if this planet has been dominated.

```html
threshold <rating#>
```

The combat rating that the player must have before being capable of demanding tribute from this planet.

```html
fleet <name> <count#>
```

The fleet that is spawned from this planet if the player demands tribute from it, and how many times that fleet is spawned. More than one type of fleet is capable of being spawned from a planet when demanding tribute.

<a name="landing">

# Landing Messages
</a>

```html
"landing message" <text>
	<sprite>
	...
```

Landing messages are the message that is shown if attempting to land on an uninhabited object. The text of a landing message is what is shown, while the children of a landing message are the sprites that show this message.

<a name="solar">

# Solar Attributes
</a>

```html
star <sprite>
	power <power#>
	wind <wind#>
```

There are certain attributes that a ship is capable of having that will change in effectiveness based off of the stars in the system.

```html
star <sprite>
```

The sprite listed here is the sprite of the star that will effect ships in the system. If a system has multiple stars, then the values of each star are added together.

```html
power <power#>
```

The power of a star impacts the effectiveness of a ship's solar collection and solar heat attributes.

```html
wind <wind#>
```

The wind of a star impacts the effectiveness of a ship's ramscoops.