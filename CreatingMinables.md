The [syntax](DataFormat#grammar-specifications) for the definition of a minable is:

```html
minable <name>
	sprite <sprite>
	hull <hull#>
	payload <outfit> <count#>
	explode <effect> <count#>
```

Minable asteroids are asteroids which are capable of being destroyed, and orbit around the system center instead of traveling in a single direction like normal, indestructable asteroids. Once destroyed, these asteroids will drop outfits that can be picked up.

```html
minable <name>
```

The name of a minable must be unique.

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