## Introduction

"sprite" nodes can be used in several locations:
- [Effects](CreatingEffects)
- [Minables](CreatingMinables)
- [Ships](CreatingShips)
- [System Objects](MapData#objects)
- [Galaxies](MapData#galaxies)

Note that while [Galaxies](MapData#galaxies) do have a "sprite" child node, they only support the "scale" parameter for that sprite.

```html
sprite <name>
	"scale" <number#>
	"frame rate" <fps#>
	"frame time" <number#>
	"delay" <frames#>
	"start frame" <number#>
	["random start frame"]
	["no repeat"]
	["rewind"]
	"center" <x#> <y#>
```

## Name

```html
sprite <name>
```

The name of the sprite should be a path relative to the **images** folder, and not including frame numbers, [blending mode specifiers](BlendingModes), or the file extension. For example, the "blaster impact" effect is an animation with four frames:

* images/effect/blaster impact+0.png
* images/effect/blaster impact+1.png
* images/effect/blaster impact+2.png
* images/effect/blaster impact+3.png

The `<name>` for this sprite is "effect/blaster impact". The `+` in the file names specifies that the images should use [additive blending](BlendingModes#alpha-blending-vs-additive-blending), and the numbers after the `+` are the frame numbers for the animation.

The current file formats supported are `.png` and `.jpg`/`.jpeg` / `.jpe`.

## Scale

```html
"scale" <number#>
```

Since **v. 0.9.15**: sprite sets can be universally resized, in case you would like to reuse an existing animation at a different size, with the `"scale"` attribute. The default scale value is `1.0` (i.e. 100%). For the best results, use a power-of-two increase or decrease, e.g. `0.125` (1/8), `0.25` (1/4), or `0.5` (1/2). Scaling factors that result in odd widths and height generally result in a blurry image, as do scales over 100%. (For most effect sprites, this will not be an issue as they are not very detailed anyway.)

## Animation parameters

You can also specify various attributes of the animation. These should be left out if you do not want them:

* `"frame rate" <fps#>`: frames per second. If no value is provided, a frame rate of 2 will be used.
* `"frame time" <number#>`: alternative to "frame rate". Specifies the number of game ticks (60 per second) each frame of the animation should last for.
* `"delay" <number#>`: number of animation frames to delay in between loops of the animation. For example, a four-frame animation with a delay of 4 and a frame rate of 8 FPS will play the animation for half a second, then pause for half a second, then repeat.
* `"start frame" <number#>`: start at this frame of the animation.
* `"random start frame"`: start at a random frame of the animation.
* `"no repeat"`: once the animation has played through once, stay on the last frame until the effect disappears. If this is not defined, the animation loops.
* `"rewind"`: once the animation has played through to the last frame, play it in reverse. If `"no repeat"` is also defined, the animation will play forward once, then backward once, then stop at the first frame.

## Center

**This parameter only applies to ship sprites.**

```html
"center" <x#> <y#>
```

Since **v. 0.10.5**: specifies the center of rotation of the ship. If not specified, the center of the sprite will be used.
