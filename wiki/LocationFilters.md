# Table of Contents

* [Introduction](#introduction)
* [Planet and System Lists](#planet-and-system-lists)
* [Government Filtering](#government-filtering)
* [Attribute Filtering](#attribute-filtering)
* [Outfit and Ship Category Filtering](#outfits-and-ship-category-filtering)
* [Near and Distance](#near-and-distance)
* [Not and Neighbor Modifiers](#not-and-neighbor-modifiers)
* [Testing Location Filters](#testing-location-filters)

# Introduction

A "location filter" is a set of constraints that are used to select a planet, system, or ship.
Location filters can be used for:
* [Mission](CreatingMissions) source, destination, stopovers, and waypoints.
* [Mission](CreatingMissions#non-player-characters-npcs) NPC systems.
* [Mission](CreatingMissions#triggers) "on enter" systems.
* [Government](CreatingGovernments#enforcement-zones) enforcement zones.
* [News](CreatingNews) locations.
* [Person](CreatingPersons) spawn systems.

```html
	[<modifier>] planet <name>...
		<name>...
	[<modifier>] system <name>...
		<name>...
	[<modifier>] government <name>...
		<name>...
	[<modifier>] attributes <name>...
		<name>...
	[<modifier>] outfits <name>...
		<name>...
	[<modifier>] category <name>...
	[<modifier>] near <system> [[<min>] <max>]
		[<distance calculation settings>]
	[<modifier>] distance [<min>] <max>
		[<distance calculation settings>]
	[<modifier>] visited [planet]
	neighbor
		...
	not
		...	
```

Each entry in the specification acts as a filter. Two "modifier" tokens were introduced in **v. 0.9.9**, **`not`** and **`neighbor`**. The `neighbor` modifier indicates that the associated filter must match a system that is hyperlinked with the system in question, and the `not` modifier indicates that the associated filter must not match the system in question. These modifiers cannot be used in the same line, but can be "children" of each other.

# Planet and system lists

```html
[(not | neighbor)] planet <name>...
	<name>...
```

This says that the planet must be (or must not be, if the `not` keyword is used) one of the named planets. If `neighbor` is used, at least one of the named planets must be in a hyperlinked system. The list of names can either be all on one line, or split between multiple lines if it is particularly long; the subsequent lines must be indented so that they are "children" of the `planet` node. As with most of these filters, you can also have more than one `planet` entry, in which case the planet chosen must be in any one of the lists. Beginning with **v 0.9.13**, planets specified in this list can be selected for use as a mission destination or stopover even if the player cannot land on the planet at the time the mission is offered (e.g. due to lack of `clearance` or due to hostility with the planet's government), or if the planet has no spaceport.

```html
[(not | neighbor)] system <name>...
	<name>...
```

The system must be (or must not be, or must neighbor) one of the items in this list. You can use this if you do not want to bother to look up what planets are in the system, but its intended use is for the NPC location filter as described later.

# Government filtering

```html
[(not | neighbor)] government <name>...
	<name>...
```

The planet must be in (or must not be in, or must neighbor) a system owned by the given government(s). Again, the list can be all on one line, or multiple indented lines.

If this is a source filter and the mission is being offered when `assisting` or `boarding` a ship, the government in this filter refers to the ship's government, not the government of the current star system. This allows you, for example, to create a mission that is only offered by merchant ships. If the `neighbor` modifier is used, at least one neighboring system's government must be in the list of named governments.

# Attribute filtering

```html
[(not | neighbor)] attributes <name>...
	<name>...
```

The system or planet must have (or must not have, or must link to a system with) one of the given attributes (e.g. "dirt belt", "urban", "rich", "tourism", etc.). If applied to a system (**v. 0.9.9+**), at least one of the given attributes must be found in either the system's own attributes, or the attributes of any of its orbiting objects. If applied to a ship (**v. 0.9.9+**), the attribute must be positive, after taking into account any adjustments that are made by all of its installed outfits.

Unlike the other filters, if multiple `attributes` tags appear, the system or planet must contain at least one attribute from each of the lists. For example, this means the planet must be urban or rich:

```bash
attributes "urban" "rich"
```

but this means it must be urban _and_ rich:

```bash
attributes "urban"
attributes "rich"
```

This matches anything that is not urban or not rich:

```bash
not attributes "urban"
not attributes "rich"
```

whereas this matches anything that is not _both_ urban _and_ rich:

```bash
not
	attributes "urban"
	attributes "rich"
```

# Outfits and ship category filtering

Beginning with **v. 0.9.9**, similar to `attributes`, ships (mission `source`) and planets (mission `source`, `destination`, and `stopover`) can be matched according to available outfits. For ships, these outfits may be installed or in cargo, while for planets they must be for sale. This would match a ship that has at least one "Beam Laser" or "Meteor Missile Launcher" installed or in its cargo, or a planet that sells either one of the outfits:

```c++
source
	outfits "Beam Laser" "Meteor Missile Launcher"
```

whereas this would match ships or planets that only have both:

```c++
source
	outfits "Beam Laser"
	outfits "Meteor Missile Launcher"
```

**V. 0.9.9+** also allows matching ships based on the specified category, e.g. "Light Warship" or "Interceptor". Any filter that defines a ship category will not match to systems or planets. Since a ship can have only one category, the following are both equivalent filters:

```c++
source
	category "Heavy Freighter" "Light Warship"

source
	category "Heavy Freighter"
	category "Light Warship"
```

# Near and distance

There are also ways of specifying how far the system is from a particular location, or from the current location:

```html
[(not|neighbor)] near <system> [[<min#>] <max#>]
	[<distance calculation settings>]
```

If one number is given, the planet must be within (or must not be within, or must link to a system) that number of jumps from the given system (which includes the given system itself). If two numbers are given, the distance from the given system must be at least as high as the first number, and no more than the second. If no numbers are given, the planet must be in the given system or one of the systems it is linked to; this is equivalent to giving distances of 0 and 1.

```html
[(not|neighbor)] distance [<min#>] <max#>
	[<distance calculation settings>]
```

This is the same as the `near` tag, but gives distances relative to the origin planet. (So, this tag only makes sense within a `destination` filter, not within a `source` filter or a `clearance` filter.) Unlike `near`, `distance` must be provided a maximum value.

Beginning in **v. 0.10.1**, `near` and `distance` filters can have [distance calculation settings](CreatingMissions#distance-calculation-settings) listed directly as children to provide more options for how the distances are calculated.

```html
[(not|neighbor)] visited [planet]
```

Beginning in **v. 0.10.13**, the `visited` filter can be used to match systems that you've visited before. If the location filter is being used for locating a planet, such as for mission sources, destinations, or stopovers, then `visited` means that the system that the planet is in must have been visited before, whereas `visited planet` means that the planet itself must have been visited before.

# Not and neighbor modifiers

```html
not
	...
neighbor
	...
```

A `not` or `neighbor` tag by itself, with filters as its children, defines a more complicated filter that must not match. For example, this filter matches a system whose neighbor must neighbor a Republic system and not be a Republic system:

```bash
neighbor
	neighbor government "Republic"
	not government "Republic"
```

while this filter additionally requires the matched system to belong to a government other than Republic:

```bash
not government "Republic"
neighbor
	neighbor government "Republic"
	not government "Republic"
```

Filters combining `not` and `neighbor` can become quite complex and difficult to reason, but offer functionality not otherwise available. For example, to find a source system that is not Republic (1), does not neighbor a Republic system (2), but has a neighbor that does neighbor a Republic system (3), you could use this filter:

```java
source
1)  not government "Republic"
2)  not
		neighbor government "Republic"
3)  neighbor
		neighbor government "Republic"
```

# Testing location filters

Beginning in **v. 0.10.0**, it is possible to test filters by passing `--matches` to the game and then writing a location filter under a `location` node. The output are systems and planets matching the filter.
