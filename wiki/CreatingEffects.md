"Effects" are animated objects - explosions, muzzle blasts, hull leaks, etc. - that are purely for show and do not interact with the other objects in the game in any way. All [weapons](CreatingOutfits) should define an effect for when they hit something, and some (missiles, in particular) may also define an effect for when they are fired. Also, anti-missile beams are rendered using effects, because they are not actual projectiles.

The full syntax of an effect is given below. Most of the parameters are optional, and some are mutually exclusive. For examples of how effects work, see the **data/effects.txt** file.

```html
effect <name>
	sprite <name>
		[sprite properties...]
	[zooms]
	sound <name>
	"sound category" <category>
	lifetime <frames#>
	"random lifetime" <frames#>
	"velocity scale" <scale#>
	"random velocity" <velocity#>
	"random angle" <degrees#>
	"random spin" <degrees#>
	"random frame rate" <fps#>
	"absolute velocity" <velocity#>
	"absolute angle" <degrees#>
```

# Sprite definition

```html
sprite <name>
	[sprite properties...]
```

The sprite to use for this effect. Details on this definition can be found on the [sprite data](SpriteData) page.

# Other attributes

As with the sprite attributes, you should only include whichever attributes you actually want to use.

* `zooms`: When the effect is used by an afterburner, it will be zoomed by the zoom factor of the engine point it is being drawn on. **(v. 0.10.13)**
* `sound <name>`: When the effect is created, this sound is played. (This happens even if the effect does not define a sprite.)
* `"sound category" <category>`: What category the effect's sound should play as. Different categories might have different volume levels configured by the user. The category must be either `ui`, `anti-missile`, `weapon`, `engine`, `afterburner`, `jump`, `explosion`, `scan`, `environment` or `alert`. **(v. 0.10.11)**
* `lifetime <frames#>`: How long the effect should last, in 60ths of a second.
* `"random lifetime" <frames#>`: A random number of frames (60ths of a second) up to this amount will be added to each effect's lifetime. **(v. 0.9.13)**
* `"velocity scale" <scale#>`: Without this, an effect will have the same velocity as the ship or projectile that "created" it. If this is defined, its velocity will be multiplied by this amount. Use a negative number to have the effect "bounce" in the opposite direction from the projectile.
* `"random velocity" <velocity#>`: A random velocity (in pixels per frame) up to this amount will be added to the effect. The added velocity will be in the direction the effect is "facing" after the random angle, if any, is applied.
* `"random angle" <degrees#>`: If this is not defined, an effect "faces" in the same direction as the ship or projectile that created it. If this is defined, a random angle up to this amount (in either direction) will be added.
* `"random spin" <degrees#>`: The effect should spin a random amount up to this number of degrees per frame.
* `"random frame rate" <fps#>`: Speed up the animation by a random amount up to this number of frames per second. This is to make it so that a bunch of identical effects created at the same moment do not end up perfectly in sync.
* `"absolute velocity" <velocity#>`: Effects normally inherit the velocity and angle of whatever created them. Specifying an absolute velocity overwrites whatever the velocity of the parent object was. An explicit value of 0 will result in the effect having a base velocity of 0. Any random velocity still gets added to this value. Any absolute velocity value will cause the velocity scale attribute to take no effect. **(v. 0.9.15)**
* `"absolute angle" <degrees#>`: As with absolute velocity, specifying an absolute angle overwrites whatever the angle of the parent object was. An explicit value of 0 will result in the effect having a base angle of 0. Any random angle still gets added to this value.**(v. 0.9.15)**
