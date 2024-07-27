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

The name of the sprite should be a path relative to the **images** folder, and not including frame numbers, [blending mode specifiers](BlendingModes), or the ".png" or ".jpg" extension. For example, the "blaster impact" effect is an animation with four frames:

* images/effect/blaster impact+0.png
* images/effect/blaster impact+1.png
* images/effect/blaster impact+2.png
* images/effect/blaster impact+3.png

The `<name>` for this sprite is "effect/blaster impact". The `+` in the file names specifies that the images should use [additive blending](BlendingModes#alpha-blending-vs-additive-blending), and the numbers after the `+` are the frame numbers for the animation.

## Scale

```html
"scale" <number#>
```

Since **v. 0.9.15**: sprite sets can be universally resized, in case you would like to reuse an existing animation at a different size, with the `"scale"` attribute. The default scale value is `1.0` (i.e. 100%). For the best results, use a power-of-two increase or decrease, e.g. `0.125` (1/8), `0.25` (1/4), or `0.5` (1/2). Scaling factors that result in odd widths and height generally result in a blurry image, as do scales over 100%. (For most effect sprites, this will not be an issue as they are not very detailed anyway.)

## Ship Sprite Animation
Ships can have sprites for several different states of flight: Flying, Firing, Landing, Launching, Jumping, Disabled. Each state can be assigned a sprite similar to the above naming convention, but the tag will be different, as shown below:

```html
ship <name>
	sprite <path-to-flying-sprites>
		...
	sprite-firing <path-to-firing-sprites>
		...
	sprite-landing <path-to-landing-sprites>
		...
	sprite-launching <path-to-launching-sprites>
		...
	sprite-jumping <path-to-jumping-sprites>
		...
	sprite-disabled <path-to-disabled-sprites>
		...
```

Note: "sprite" and "sprite-flying" are interchangeable.

Each sprite can be animated individually with the animation parameters as defined below.


### Animation Parameters

You can also specify various attributes of the animation. These should be left out if you do not want them:

#### Parameters for all Sprites

* `"frame rate" <fps#>`: frames per second. If no value is provided, a frame rate of 2 will be used.
* `"frame time" <number#>`: alternative to "frame rate". Specifies the number of game ticks (60 per second) each frame of the animation should last for.
* `"delay" <number#>`: number of animation frames to delay in between loops of the animation. For example, a four-frame animation with a delay of 4 and a frame rate of 8 FPS will play the animation for half a second, then pause for half a second, then repeat.
* `"start frame" <number#>`: start at this frame of the animation.
* `"random start frame"`: start at a random frame of the animation.
* `"no repeat"`: once the animation has played through once, stay on the last frame until the effect disappears. If this is not defined, the animation loops.
* `"rewind"`: once the animation has played through to the last frame, play it in reverse. If `"no repeat"` is also defined, the animation will play forward once, then backward once, then stop at the first frame.
* `"ramp" <up fpsps#> <down fpsps#> `: when the object enters (or exit) a certain state, this allows the frame rate to ramp up (or down) according to the values given in up fpsps (fps per second) and down fpsps.
* `"random"`: randomize the next frame played in the animation.
* `"reverse"`: play the animation from the last frame to the first.

#### Parameters for Ship Sprites only
* `"transition delay" <number#>`: if a ship has requested a transition to a different state, i.e. FIRING to FLYING, wait for a certain number of frames before allowing that transition to occur. This would be used in places where there could be rapid state transitions. For example, if a ship has fired its primary weapon, chances are it might fire its primary weapon again, so instead of immediately transitioning to the flying state, we can wait in the firing state for a certain number of frames before entering the flying state again.
* `"transition type" <number#>`: currently, three transistion types are supported: "immediate", "finish" and
"rewind". The default value is set to "immediate", meaning that if a ship changes state e.g. from FIRING to FLYING, the currently used sprite would immediately switch from the FIRING sprite to the FLYING sprite. If it is "finish", the ship will first finish the FIRING animation, up until the last frame, before switching the state to FLYING. And if it is "rewind", the ship will rewind to the first frame of the animation from whichever frame it is on currently before completing the transition to FLYING.
* `"indicate" <number# (optional)>`: applying this parameter to your sprite animation means that it must complete the animation before performing the desired action e.g. if you are trying to fire your primary weapon, the animation must reach the desired frame (optional parameter) before the weapons can actually be fired. If the frame number is not given, it will default to the last frame of the animation. This keyword also sets the `"repeat"` flag to false, and forces the animation to start at the zeroth frame.
* `"trigger" <name>`: requiring a name for the sprite like seen above, see below for more information

##### Trigger Sprites
Trigger sprites are essentially conditional sprites which can be played within a state. They can be animated with the same parameters as above and act as their own substates. Trigger sprites have an additional attribute: conditions, as we can see below.

```html
"trigger" <name>
	conditions
		has "outfit (installed): <outfit-name>"
```

There are several preliminary conditions that have been implemented, more to come as they are required:
* `"hull"`: Gives the amount of hull currently on the ship
* `"shields"`: Gives the amount of shields currently on the ship
* `"outfit (installed): <outfit-name>"`: Checks whether the outfit is currently installed on the ship
* `"jump (type): <jump-type>"`: Either "jump" or "hyper", checks whether the next jump is a jump drive jump, or a hyper drive jump.
* `"weapon (firing): <outfit-name>"`: Checks whether the currently equipped outfit/weapon is firing or not.

Trigger sprites are independently animated, meaning that if you want the trigger sprite to have a frame rate of 30, and the default state sprite to also have the same frame rate, both of them would independently need to have the `"frame rate"` parameter defined. This is done in order to make saving trigger sprite animation info more streamlined.
### Putting it all together
```html
ship <name>
	sprite <path-to-flying-sprite>
		"frame rate" 5
	sprite-firing <path-to-firing-sprite>
		ramp 20 0
		"frame rate" 20
		"indicate"
		"transition delay" 100
		"transition type" rewind
	sprite-landing <path-to-launching-sprite>
		reverse
		"frame rate" 30
		"no repeat"
	sprite-launching <path-to-launching-sprite>
		"frame rate" 30
		"no repeat"
		"transition type" finish
	sprite-jumping <path-to-jumping-sprite>
		ramp 30 5
		trigger <path-to-jumping-trigger-sprite>
			ramp 30 5
			"frame rate" 20
			"indicate"
			"transition type" rewind
			conditions
				has "jump (type): jump"
		"frame rate" 20
		"indicate"
		"transition type" rewind
	sprite-disabled <path-to-disabled-sprite>
		"frame rate" 20
		"random"
```

Doing a run through of the above animation definition:

1. The flying sprite will play at a frame rate of 5.
1. The firing sprite will ramp up at 20 fps per second until a frame rate of 20. The ship will not be allowed to fire until the last frame of the firing sprite is reached (indicate). Once the ship has stopped firing for 100 frames (transition delay), the animation will rewind until the first frame before transitioning states again.
1. The landing sprite is the launching sprite played in reverse without repetition at a frame rate of 30
1. The launching sprite is played at a frame rate of 30, without repetition, and will only allow a transition to the next state after reaching the last frame of the launching animation
1. The jumping sprite has a ramp up of 30 fps per second, and ramp down of 5 fps per second up until frame rate of 20. The ship can only jump until the last frame of the jumping sprite is reached (indicate) and once the ship wants to exit the jump state, the animation will rewind back to the first frame before transitioning to the next state. The jump sprite also has a trigger for when the ship performs a jump drive based jump. In this case, instead of the regular jumping sprite, the jumping trigger sprite is played with the same animation parameters.
1. The disabled sprite is played at a frame rate of 20, and randomly picks the next frame of the animation.

## Center

**This parameter only applies to ship sprites.**

```html
"center" <x#> <y#>
```

Since **v. 0.10.5**: specifies the center of rotation of the ship. If not specified, the center of the sprite will be used.
