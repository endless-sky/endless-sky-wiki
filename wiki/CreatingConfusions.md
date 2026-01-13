# Introduction

"Confusion" is the term for the random tracking error experienced by all ships. This is to make ships act more organically and to reduce difficulty. If ships always fired at the exact center of their target, never missing, combat would look unnatural, and the AI would always have a reaction speed advantage over the player. Confusion is applied as a sinusoidal angular offset to a ship's targeting.

The [syntax](DataFormat#grammar-specifications) for the definition of a confusion profile is:

```html
confusion <name>
	"max confusion" <value>
	period <value>
	"focus multiplier" <value>
	"gain focus time" <value>
	"lose focus time" <value>
```

## Basic confusion characteristics
```html
confusion <name>
```
The name of a confusion profile must be unique. Using this name, fleets, mission NPCs, and governments can all use a single confusion profile. If a confusion profile is nested within the definition of another object, it does not require a name. To maintain backwards compatability, confusion profiles cannot have a name consisting solely of numbers.

```html
"max confusion" <value>
```
The maximum amount a ship's aim will be offset by, in degrees.

```html
period <value>
```
The period of the sine function that controls the aiming offset at a given time, in frames. A value of `240` means that it will take 4 seconds for a ship to go from its leftmost aiming position to its rightmost, then back to the its leftmost position.

## Focus
When a ship is close enough to fire its non-homing weapons, it will start to build up focus, reducing the aiming error from confusion. If the ship's target goes out of range or is destroyed, the firing ship will gradually lose focus.

```html
"focus multiplier" <value>
```
The multiplier applied to the `"max confusion"` when a ship has reached max focus. A value < 0 will result in a ship getting more accurate while firing, while values > 0 will result in a ship becoming less accurate. As a ship's focus level changes, the game will linearly interpolate between the multiplier at no focus and at maximum focus.

```html
"gain focus time" <value>
```
The time it takes for a ship to reach maximum focus, in frames.

```html
"lose focus time" <value>
```
The time it takes for a ship to go from maximum to minimum focus, in frames.

## Default values
Ships that do not have a confusion defined by their personality or government will use the following confusion profile:
```html
confusion
	"max confusion" 10.
	period 240
	"focus multiplier" .1
	"gain focus time" 600
	"lose focus time" 120
```

## Backwards compatability
Prior to **v. 0.10.17**, confusion was defined as a single value. To prevent old plugins from breaking, confusions defined as `confusion <value>` will use the confusion profile except with the `"max confusion"` replaced by `<value>`.
