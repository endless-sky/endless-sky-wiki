# Introduction

The [syntax](DataFormat#grammar-specifications) for the definition of a system hazard is:
```html
hazard <name>
	"constant strength"
	"period" <number#>
	"duration" <min#> [<max#>]
	"strength" <min#> [<max#>]
	"range" [<min#>] <max#>
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

If this tag is omitted, then the strength of the hazard follows a normal curve, starting at roughly 10%, raising until reaching 100% strength half way through the duration, then dropping back down to roughly 10% before ending. Suggested that hazards with a short duration, such as only one frame, have this tag, as otherwise they will deal less damage than expected since they are starting at 10% strength.

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
"environmental effect" "<effect name>" [<amount#>]
```

An effect that is created each frame that the hazard is active. The amount of effects created is multiplied by the current strength of the hazard, meaning that stronger hazards create more effects.

Multiple `"environmental effect"` lines can be provided at once. If this is not provided, then no effects are created when the hazard is active.

# Hazard weapon behavior

```html
weapon
	...
```

The [weapon](CreatingOutfits#weapon-attributes) of a hazard defines what it does to all ships within the hazard's range. The relevant weapon attributes are the following:

* `"shield damage"`: All damage values, plus hit force, are multiplied by the square root of the current strength of the hazard, or just the current strength if the hazard has `"constant strength"`.
* `"hull damage"`
* `"heat damage"`
* `"fuel damage"`
* `"ion damage"`
* `"disruption damage"`
* `"slowing damage"`
* `"piercing"`
* `"hit force"`: Hit force is relative to the system center.
* `"gravitational"`
* `"blast force"`: Causes the damage from this hazard to decline with range from the system center according to the blast force curve as described in [CreatingOutfits](CreatingOutfits#weapon-attributes).
* `"trigger radius"`: Used only to change the blast force curve.
* `"damage dropoff"`: Causes the damage from this hazard to change with range according to the damage dropoff ranges and modifier. For expected behavior, two values must be provided with this weapon attribute, as otherwise the second value will be assumed to be 0.
* `"dropoff modifier"`
* `"hit effect"`: The hit effect of a hazard is created as a spark on the impacted ship when the hazard deals damage, multiplied by the square root of the current strength of the hazard, or just the current strength if the hazard has `"constant strength"`.
