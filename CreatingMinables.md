The [syntax](DataFormat#grammar-specifications) for the definition of a minable is:

```html
minable <name>
	"display name" <display name>
	noun <noun>
	sprite <sprite>
	hull <hull#>
	payload <outfit> <count#>
		"max drops" <count#>
		"drop rate" <chance#>
		"toughness" <value#>
	explode <effect> <count#>
```

Minable asteroids are asteroids which are capable of being destroyed, and orbit around the system center instead of traveling in a single direction like normal, indestructible asteroids. Once destroyed, these asteroids will drop outfits that can be picked up.

```html
minable <name>
```

The name of a minable must be unique.

```html
"display name" <display name>
```

Beginning in **v. 0.10.0**, a minable can have a display name. This is the name that will be displayed to the player, for example, when they target this minable (with and asteroid scanner of some sort equipped). Unlike minable names, minable display names do not need to be unique.
If no display name is provided, the name of the minable will be used.

```html
noun <noun>
```

Beginning in **v. 0.10.0**, minables can have custom nouns. If no noun is specified, "Asteroid" will be used.

```html
sprite <sprite>
```

The sprite that the minable asteroid has.

```html
hull <hull#>
```

The amount of hull damage that must be applied to this asteroid before it is destroyed.

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
* `"toughness"`: a value greater than 1 which represents the toughness of this payload, which measures how resistant it is to having its drop rate increased by [prospecting weapons](https://github.com/endless-sky/endless-sky/wiki/CreatingOutfits#weapon-attributes).

```html
explode <effect> <count#>
```

The [effect](CreatingEffects) that is created when this asteroid is destroyed, and how many times it is created. A minable asteroid can have multiple explosion effects.