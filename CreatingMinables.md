The [syntax](DataFormat#grammar-specifications) for the definition of a minable is:

```html
minable <name>
	"display name" <display name>
	noun <noun>
	sprite <sprite>
	hull <hull#>
	payload <outfit> <count#>
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
```

The [outfit](CreatingOutfits) that is dropped by this asteroid after being destroyed. The count is the maximum possible number of the outfit that can drop, but when an asteroid is destroyed, only about 25% of the payload will survive on average. A minable asteroid can have multiple payloads.

```html
explode <effect> <count#>
```

The [effect](CreatingEffects) that is created when this asteroid is destroyed, and how many times it is created. A minable asteroid can have multiple explosion effects.