# Introduction

The [syntax](DataFormat#grammar-specifications) for the definition of a system hazard is:
```html
hazard <name>
	"constant strength"
	"period" <number#>
	"duration" <min#> [<max#>]
	"strength" <min#> [<max#>]
	"range" [<min#>] <max#>
	"system-wide"
	"environmental effect" "<effect name>" [<amount#>]
	weapon
		...
```

# Hazard characteristics

```html
hazard <name>
```

The name of the hazard must be unique.

```html
"constant strength"
```

This is a tag with no value that causes the strength of a hazard to remain constant throughout its duration.

If this tag is omitted, then the strength of the hazard follows a normal curve, starting at roughly 10%, raising until reaching 100% strength halfway through the duration, then dropping back down to roughly 10% before ending. Suggested that hazards with a short duration, such as only one frame, have this tag, as otherwise they will deal less damage than expected since they are starting at 10% strength.

```html
"period" <number#>
```

The period of a hazard is the average number of frames between when the hazard damages all ships in range, with there being 60 frames in a second. A random number from 0 to `period - 1` is rolled each frame, and if the result lands on 0 then a hazard is spawned. This means that a period of 1 results in a hazard that deals damage every frame.

If a hazard changes its strength over time (i.e. it does not have the `"constant strength"` tag), then its period will be divided by the square root of the current strength of the hazard. This means that as a hazard's strength increases, the chance that it deals damage increases as well.

If this is not provided, then the period is 1.

```html
"duration" <min#> [<max#>]
```

The duration in frames of this hazard when it is created. If only one value is provided then the hazard will always last that long. If two values are provided then when a hazard is activated, a random number between these two values is chosen.

If this is not provided, then the duration is always 1.

```html
"strength" <min#> [<max#>]
```

The strength of this hazard when it is created. If only one value is provided then the hazard will always have that strength. If two values are provided then when a hazard is activated, a random number between these two values is chosen.

Strength is able to change two things: how fast the hazard impacts all ships in the system (by decreasing the period) and how hard the hazard hits (by increasing the damage). The period of a hazard is divided by a factor of the square root of the strength, while the damage is multiplied by the same factor. If a hazard has the `"constant strength"` tag, the period is unchanged and the damage is multiplied by the strength, not its square root. In this way, a strength of 2 is always double the DPS of a strength of 1.

If this is not provided, then the strength is always 1.

```html
"range" [<min#>] <max#>
```

The range at which this hazard affects ships in the system. If only one value is provided then the hazard will deal damage out to that range from the system center. If two values are provided then the hazard will deal damage within a ring around the system center defined by the two ranges given. Any ship touching the circle or ring takes damage.

If this is not provided, then the max range of the hazard is set to 10,000.

```html
"system-wide"
```

Beginning in **v. 0.9.15**, this tag can be applied to hazards in order to cause them to impact all ships in the system regardless of their distance from the system center.

The system-wide tag also changes the origin of environmental effects from the system center or planet that the hazard is defined around to the center of the screen, causing the environmental effects to always be visible to the player. The minimum range value will still be used to determine the minimum range from the center of the screen that effects will begin appearing, while the maximum range gets overridden by the edge of the screen.

System-wide hazards will maintain the same density of environmental effects as if they were not system-wide. For example, if the max range of the hazard were defined as 5,000 and the amount of environmental effects created per frame was 5, but the hazard being made system-wide caused the max range to become 10,000 to fit the screen, the number of effects created would change from 5 to 20 as a circle with radius 10,000 has four times the area as one with radius 5,000.

```html
"environmental effect" "<effect name>" [<amount#>]
```

An effect that is created each frame that the hazard is active. The number of effects created is multiplied by the current strength of the hazard, meaning that stronger hazards create more effects.

Multiple `"environmental effect"` lines can be provided at once. If this is not provided, then no effects are created when the hazard is active.

Beginning in **v. 0.10.0**, the amount number of an effect can be a decimal value, whereas previously only integers were accepted. Decimal counts work because of how the strength of a hazard increases the number of effects. Previously, the minimum allowed count was one. Suppose the hazard then had a strength of two, the result would be two effects every frame. Now that the number of effects can be fractional, one could specify an effect with an amount of 0.5, meaning that effects would only start appearing once the hazard reached a strength multiplier of two, creating one effect per frame.

# Hazard weapon behavior

```html
weapon
	...
```

The [weapon](CreatingOutfits#weapon-attributes) of a hazard defines how all ships within the hazard's range are affected when the hazard triggers. The relevant weapon attributes and any additional information about each of them are listed here:

* All damage types (e.g. `"shield damage"`): All damage values, plus hit force, are multiplied by the square root of the current strength of the hazard, or just the current strength if the hazard has `"constant strength"`.
* `"piercing"`
* `"hit force"`: Hit force is relative to the system center. (i.e. positive hit force pushes away from the system center while negative hit force pulls toward it.)
* `"gravitational"`
* `"blast radius"`: Causes the damage from this hazard to decline with range from the system center according to the blast radius curve as described in [CreatingOutfits](CreatingOutfits#weapon-attributes).
* `"trigger radius"`: Used only to change the blast radius curve.
* `"damage dropoff"`: Causes the damage from this hazard to change with range according to the damage dropoff ranges and modifier. For expected behavior, two values must be provided with this weapon attribute, as otherwise the second value will be assumed to be 0.
* `"dropoff modifier"`
* `"target effect"`: The scaling factor used for how much damage is dealt by a hazard is the same factor used for how many effects are created on the target. **Prior to v0.9.15**, the `"hit effect"` weapon attribute had the behavior of `"target effect"`.
