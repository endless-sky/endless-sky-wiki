### Goal: smooth animation at low frame rates

For an animation to look smooth to the human eye, ideally it should have a "frame rate" of 60 frames per second or more. In Endless Sky, the most noticeable animated objects are the spinning asteroids. If an asteroid is spinning at 60 FPS, the animation looks smooth. But, if I even just reduce that to 30 FPS (that is, the sprite used for the asteroid is changing every other frame) the asteroid looks like it is vibrating, which is hard on your eyes. And, if the frame rate is not an exact divisor of the number of frames, the animation will spend longer on some frames than on others, which looks even worse.

Each star system has a different number of asteroids of each type, and the asteroids move and spin at higher or lower speeds. An asteroid that is just drifting lazily at a sedate speed might take a minute to complete one full revolution. But that means to avoid choppy animations, I would need 3600 frames, which is certainly overkill.

(If you're interested in how I created the 3D models for the asteroids, see this very helpful [Blender tutorial](http://www.blenderguru.com/tutorials/how-to-make-a-realistic-asteroid/).)

### Solution: animation tweening

My solution is to store the current animation frame of each sprite as a floating point number, rather than an integer. That is, for example, if an animation is currently on frame 4.28, that means that the sprite that is drawn will be a blend of 28% of frame 5, plus 72% of frame 4. For every sprite that is halfway between two frames, the OpenGL shader is given two textures representing the two frames, and a "fade" value representing what fraction of the pixel color should come from each of the two textures:

```c
finalColor = mix(texture(tex0, fragTexCoord), texture(tex1, fragTexCoord), fade);
```

Here is an example of how that looks, in practice. The first and last images below are actual frames of the asteroid sprite; the others are a blend of those two key frames. The blended sprites are slightly blurry, but the final effect is still much more pleasant than a choppy animation would be:

![](http://endless-sky.github.io/images/asteroid_spin.jpg)

Unless you have a high-DPI screen the asteroid will only be displaying at half that resolution, and the sprite will be rotating in the 2D plane at the same time as the animation is simulating spin in the Z plane. So, in practice it's really hard to tell that the animation is just using blending instead of containing thousands of actual frames.

This animation tweening also makes it much easier to create sprites for weapons, explosions, etc. For example, the laser beam sprites pulse between a brighter and a dimmer state, but they only need two key frames to achieve that.

### Rewinding and repeating animations

In the data files, wherever a sprite can be animated, you can tell it to "rewind" or "repeat," or specify "no repeat" if it should play through to the end and then stop. A rewinding animation plays through all the frames, then "rewinds" backwards through the frames until it reaches the first one again. A repeating animation plays through the frames to the end, then starts back at the beginning (tweening between the last frame and the first frame).

Animations can also have a "random start frame." This is useful, for example, if you want all the projectiles of a certain type to "sparkle" a bit, but you don't want them to be pulsing in exact synchronization with each other.