"Effects" are animated objects - explosions, muzzle blasts, hull leaks, etc. - that are purely for show and do not interact with the other objects in the game in any way. All [weapons](https://github.com/endless-sky/endless-sky/wiki/CreatingOutfits) should define an effect for when they hit something, and some (missiles, in particular) may also define an effect for when they are fired. Also, anti-missile beams are rendered using effects, because they are not actual projectiles.

The full syntax of an effect is given below. Most of the parameters are optional, and some are mutually exclusive. For examples of how effects, work, see the **data/effects.txt** file.

```html
effect <name>
    sprite <name>
        "scale" <number#>
        "frame rate" <fps#>
        "start frame" <number#>
        "random start frame"
        "no repeat"
        "rewind"
    sound <name>
    lifetime <frames#>
    "random lifetime" <frames#>
    "velocity scale" <scale#>
    "random velocity" <velocity#>
    "random angle" <degrees#>
    "random spin" <degrees#>
    "random frame rate" <fps#>
```

### Sprite definition

The name of the sprite should be a path relative to the **images** folder, and not including frame numbers, blending mode specifiers, or the ".png" or ".jpg" extension. For example, the "blaster impact" effect is an animation with four frames:

* images/effect/blaster impact+0.png
* images/effect/blaster impact+1.png
* images/effect/blaster impact+2.png
* images/effect/blaster impact+3.png

The `<name>` for this sprite is "effect/blaster impact". The `+` in the file names specifies that the images should use [additive blending](https://github.com/endless-sky/endless-sky/wiki/BlendingModes), and the numbers after the `+` are the frame numbers for the animation.

Sprite sets can be universally resized, in case you would like to re-use an existing animation at a different size, with the `"scale"` attribute. The default scale value is `1.0` (i.e. 100%). For the best results, use a power-of-two increase or decrease, e.g. `0.125` (1/8), `0.25` (1/4), or `0.5` (1/2). Scaling factors that result in odd widths and height generally result in a blurry image, as do scales over 100%. (For most effect sprites, this will not be an issue as they are not very detailed anyway.)

You can also specify various attributes of the animation. These should be left out if you do not want them:

* `"frame rate" <fps#>`: frames per second.
* `"start frame" <number#>`: start at this frame of the animation.
* `"random start frame"`: start at a random frame of the animation.
* `"no repeat"`: once the animation has played through once, stay on the last frame until the effect disappears. (If this is not defined, the animation loops.)
* `"rewind"`: once the animation has played through to the last frame, play it in reverse. If `"no repeat"` is also defined, the animation will play forward once, then backward once, then stop at the first frame.

### Other attributes

As with the sprite attributes, you should only include whichever attributes you actually want to use.

* `sound <name>`: When the effect is created, this sound is played. (This happens even if the effect does not define a sprite.)
* `lifetime <frames#>`: How long the effect should last, in 60ths of a second.
* `"random lifetime" <frames#>`: a random number of frames (60ths of a second) up to this amount will be added to each effect's lifetime. **(v. 0.9.13)**
* `"velocity scale" <scale#>`: Without this, an effect will have the same velocity as the ship or projectile that "created" it. If this is defined, its velocity will be multiplied by this amount. Use a negative number to have the effect "bounce" in the opposite direction from the projectile.
* `"random velocity" <velocity#>`: A random velocity (in pixels per frame) up to this amount will be added to the effect. The added velocity will be in the direction the effect is "facing" after the random angle, if any, is applied.
* `"random angle" <degrees#>`: If this is not defined, an effect "faces" in the same direction as the ship or projectile that created it. If this is defined, a random angle up to this amount (in either direction) will be added.
* `"random spin" <degrees#>`: The effect should spin a random amount up to this number of degrees per frame.
* `"random frame rate" <fps#>`: Speed up the animation by a random amount up to this number of frames per second. This is to make it so that a bunch of identical effects created at the same moment do not end up perfectly in synch.