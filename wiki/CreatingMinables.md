The [syntax](DataFormat#grammar-specifications) for the definition of a minable is:

```html
minable <name>
	"display name" <display name>
	noun <noun>
	sprite <sprite>
		[sprite properties...]
	hull <hull#>
	"random hull" <hull#>
	payload <outfit> <count#>
		"max drops" <count#>
		"drop rate" <chance#>
		"toughness" <value#>
	"live effect" <effect> [<interval#>]
		["relative to system center"]
	explode <effect> <count#>
```

Minable asteroids are asteroids that are capable of being destroyed, and orbit around the system center instead of traveling in a single direction like normal, indestructible asteroids. Once destroyed, these asteroids will drop outfits that can be picked up.

```html
minable <name>
```

The name of a minable must be unique.

```html
"display name" <display name>
```

Beginning in **v. 0.10.0**, a minable can have a display name. This is the name that will be displayed to the player, for example, when they target this minable (with an asteroid scanner of some sort equipped). Unlike minable names, minable display names do not need to be unique.
If no display name is provided, the name of the minable will be used.

```html
noun <noun>
```

Beginning in **v. 0.10.0**, minables can have custom nouns. If no noun is specified, "Asteroid" will be used.

```html
sprite <sprite>
	[sprite properties...]
```

The sprite that the minable asteroid has. Prior to **v. 0.10.11**, only the sprite graphic may be provided; additional properties are not supported.
Details on additional sprite properties (usable from **v. 0.10.11**) are available on the [sprite data](SpriteData) page. Applying some properties, such as "no repeat" to minables may have undesirable effects.
By default, the frame rate is randomly assigned to each minable instance, with an upper limit proportional to the energy of the minable. If the sprite properties contain "frame rate" or "frame time" nodes, the given values will be used instead of generating a random value.

```html
hull <hull#>
```

The amount of hull damage that must be applied to this asteroid before it is destroyed.

```html
"random hull" <hull#>
```

Beginning in **v. 0.9.15**, minables can have an amount of health up to their "random hull" value added on top of their base hull.

```html
payload <outfit> <count#>
	"max drops" <count#>
	"drop rate" <chance#>
	"toughness" <value#>
```

The [outfit](CreatingOutfits) that is dropped by this asteroid after being destroyed. The count is the maximum possible number of the outfit that can drop, but when an asteroid is destroyed, only 25% of the payload will survive on average by default. A minable asteroid can have multiple payloads.

Starting with **v. 0.10.5**, a `payload` can have the following optional children:
* `"max drops"`: the maximum possible number of the outfit that can drop. An alternative location from the drop count next to the outfit name. If a drop count is not specified in either location, the default drop size is 1.
* `"drop rate"`: a value between 0 and 1 that represents the fraction of the maximum payload count that will survive on average. Defaults to 0.25 if not specified.
* `"toughness"`: a value greater than 1 which represents the toughness of this payload, which measures how resistant it is to having its drop rate increased by [prospecting weapons](CreatingOutfits#weapon-attributes).

```html
"live effect" <effect> [<interval#>]
	["relative to system center"]
```

Beginning with **v. 0.10.13**, minables can create [effects](CreatingEffects) while orbiting. You can choose an average interval (in frames) between spawns. If `"relative to system center"` is set, the effect will face away from the system center (like a comet tail) instead of rotating with the minable.

```html
explode <effect> <count#>
```

The [effect](CreatingEffects) that is created when this asteroid is destroyed, and how many times it is created. A minable asteroid can have multiple explosion effects.
