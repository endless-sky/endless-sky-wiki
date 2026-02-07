# Table of Contents

* [Introduction](#introduction)
* [Gamerules](#gamerules)
	* [Depreciation](#depreciation)
	* [Person Ships](#person-ships)
	* [NPC Behavior](#npc-behavior)
	* [System Behavior](#system-behavior)
	* [Miscellaneous](#miscellaneous)

# Introduction

Gamerule presets are a collection of gamerules that can be selected by a player when on the gamerules panel, accessible during new pilot creation or from the main menu if the pilot's gamerules are editable. The player is still able to modify individual gamerules after choosing a preset. The purpose of a preset is to represent a collection of gamerule values that work well together or lead to a certain type of gameplay not experienced with other collections of gamerules.

The syntax for a gamerules preset is as follows:

```html
"gamerules preset" "<name>"
	"<rule>" <value>
```

The name "Default" is a special preset that will always be created by the game, even if the data for it is not present, and it will always be the selected gamerules preset if the player does not edit the gamerules of their pilot. 

# Gamerules

There are four different types of gamerule in terms of what the value of the gamerule can be:
* Integer: The value should be given as a whole number.
* Decimal: The value should be given as a decimal number.
* Boolean: The value must be given as the words "true" or "false", or the numbers 1 or 0.
* Enum: The value is a string, and the rule has a list of allowable string values that it can have.

Some rules may also have no value at all, represented by the string "unset".

These are the gamerules that are currently available:

## Depreciation

The depreciated value of an item that is "age" days old is calculated with the following equation where: "base value" is the undepreciated value of the item; min is "depreciation min"; "grace period" is "depreciation grace period"; daily is "depreciation daily"; and "max age" is "depreciation max age", and age is at greater than "grace period" and less than "max age".
`"depreciated value" = "base value" * (min + (1 - min) * (daily ^ (age - "grace period")) * ("max age" + "grace period" - age) / "max age")`

* `"depreciation min"`: A decimal rule whose value must be between 0 and 1, inclusive, where 0 = 0% and 1 = 100%. Represents the lowest value that an outfit or ship can depreciate to relative to its full price. All plundered, captured, or gifted items start at this value, and all purchased items start at 100% value and then depreciate toward this value according to the other depreciation gamerules.
* `"depreciation grace period"`: An integer rule whose value must be greater than or equal to 0. Represents the number of days that must pass before a newly purchased outfit or ship will begin to depreciate. This allows you to have a short amount of time to test out an item and then sell it back at full value if you decide that you do not like it.
* `"depreciation max age"`: An integer rule whose value must be greater than or equal to 0. Represents the number of days that must pass, after the grace period has ended, in order for an outfit or ship to become fully depreciated.
* `"depreciation daily"`: A decimal rule whose value must be between 0 and 1, inclusive, where 0 = 0% and 1 = 100%. Represents the percentage of an item's depreciable value that it maintains day over day. Only impacts the exponential term of the depreciation calculation. If set to 100%, an item will still linearly lose value.

## Person Ships

* `"person spawn period"`: An integer rule whose value must be greater than or equal to 1. Sets the number of frames on average that the game waits before attempting to spawn a person ship. The game rolls a random number between 0 and this value minus one every frame. If the value lands on 0, a person ship spawn attempt is made. There are 60 frames in a second.
* `"no person spawn weight"`: An integer rule whose value must be greater than or equal to 0. When a person ship spawn attempt is made, this is the "weight" of the chance of having the spawn attempt fail. Each person ship has a "weight" associated with it which determines its chance of spawning. The chance of any single person ship spawning is its weight divided by the sum of the weights of all person ships and this value. That means that the chance of a spawn attempt failing is this number over that sum.

## NPC Behavior

* `"npc max mining time"`: An integer rule whose value must be greater than or equal to 0. Randomly spawned NPCs with the `mining` personality will spend this many frames mining in the current system before deciding to do something else. The purpose of this limit is so that NPC ships can be seen by the player to show how to mine without stripping the system bare of minable asteroids. This limit does not apply to mining ships that are spawned by a mission.
* `"universal frugal threshold"`: A decimal rule whose value must be between 0 and 1, inclusive, where 0 = 0% and 1 = 100%. Ships with the `frugal` personality will only use weapons that consume ammo or fuel while their health (shields + hull - the hull level when they become disabled) is at or below this percentage, or if the hostiles in the system are considerably stronger than them.

## System Behavior

* `"universal ramscoop"`: A boolean rule that controls whether the universal ramscoop is active on all ships. This ramscoop provides a very small amount of fuel to every ship regardless of whether it has an actual ramscoop installed. The strength of this ramscoop falls off much more strongly than normal ramscoops, requiring you to be as close to a star as possible to have any real gain in fuel. Having this enabled prevents needing to reload your save file if you run out of fuel and no NPCs spawn in the system.
* `"system arrival min"`: A decimal rule whose value can be any number, or "unset". Represents the minimum arrival distance for all systems. Applies to both hyperdrive and jump drive travel. If the system has a defined arrival distance greater than this value, then it will be used instead. If this gamerule has a value of "unset", then the value defined by the system will always be used.
* `"system departure min"`: A decimal rule whose value must be greater than or equal to 0. Represents the minimum departure distance for all systems. A ship can only jump out of a system if it is farther from the system center than the departure distance. If a system has a defined departure distance greater than this value, then it will be used instead.
* `"fleet multiplier"`: A decimal rule whose value must be greater than or equal to 0, where 0 = 0%, 1 = 100%, 1.5 = 150%, and so on. Represents a global fleet spawn rate multiplier for random fleet spawns within systems.

## Miscellaneous

* `"lock gamerules"`: A boolean rule that controls whether gamerules can be altered after a pilot has been created. If true, the gamerules button will not appear on the main menu for that pilot.
* `"disabled fighters avoid projectiles"`: An enum rule whose allowed values are "all", "none", and "only player". Controls which carried ships (fighters and drones), when disabled, will not be hit by projectiles unless they are directly targeted. Fighters and drones are fragile ships that can be difficult and/or tedious to replace for the player, so this gamerule is a means of increasing their survivability without increasing their combat effectiveness. Disabled carried ships can still be hit by explosions, whether from weapons or exploding ships.
* `"universal ammo restocking"`: A boolean rule that controls whether the ammo for most secondary weapons that you have installed or in your cargo will be available for purchase at any outfitter, even if that outfitter does not normally sell ammo of that type. The description of each secondary weapon will indicate whether it can be restocked anywhere when this rule is true.
