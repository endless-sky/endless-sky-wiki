# Alpha blending vs. additive blending

When an object is drawn in OpenGL, the color of each pixel is a combination of the color of the object, and the color of the background it is drawn on top of. The alpha value of each pixel specifies how opaque it is. For example, if a pixel is 100% opaque, it replaces the background; if it is 0% opaque, the background remains as it is. If a pixel is 50% opaque, after it is drawn the resulting color will be 50% of the background plus 50% of the object:

```c++
result color = alpha * object color + (1 - alpha) * background color
```

Another useful blending mode is additive blending, which is a good simulation of light or of glowing objects. For example, Endless Sky uses additive blending for laser beams. That is, the beam is creating light, but not blocking the light from whatever is behind it. The image below shows what a laser beam looks like using alpha blending (top) vs. additive blending (bottom):

![](https://endless-sky.github.io/images/alpha_vs_additive.jpg)

With alpha blending, the beam is much less visible where it crosses a ship. By using additive blending instead, the beam stands out better. The equation for additive blending is:

```c++
result color = object color + background color
```

# Premultiplied alpha

In OpenGL, the blending mode (alpha vs. additive) is set via a special command. That means that if you are drawing some objects in additive mode, and some in alpha mode, you have to keep switching between them for each object. However, a third blending mode exists: premultiplied alpha. When using premultiplied alpha, you supply images where the value of (alpha * object color) is already calculated for each pixel. That simplifies the alpha blending equation to:

```c++
result color = object color + (1 - alpha) * background color
```

Why is this useful? Partly because it means one fewer multiplication calculations per pixel. But it has another side effect. Suppose the alpha value for every pixel in an image is zero. Then, the above equation simplifies to:

```c++
result color = object color + (1 - 0) * background color 
			 = object color + background color
```

It's the additive blending equation! So, in premultiplied alpha mode, you can create images that are drawn using additive blending just by setting all their alpha values to zero. That means that every image can be drawn with the same blending mode (premultiplied alpha) instead of constantly switching between two different modes.

# Half-additive blending

So, an image with alpha values uses normal blending; an image with alpha equal to zero uses additive blending. But, there's no reason we can't also use alpha values in between those. Here's the code for converting a normal image to premultiplied alpha:

```c++
pixel.red = pixel.red * pixel.alpha;
pixel.green = pixel.green * pixel.alpha;
pixel.blue = pixel.blue * pixel.alpha;
```

The code for converting an image to additive mode is exactly the same, except the alpha value gets set to zero:

```c++
pixel.red = pixel.red * pixel.alpha;
pixel.green = pixel.green * pixel.alpha;
pixel.blue = pixel.blue * pixel.alpha;
pixel.alpha = 0;
```

But, there's no reason we couldn't, instead, set the alpha value to something else. For example, this will create an effect halfway between alpha blending and additive blending:

```c++
pixel.red = pixel.red * pixel.alpha;
pixel.green = pixel.green * pixel.alpha;
pixel.blue = pixel.blue * pixel.alpha;
pixel.alpha = pixel.alpha / 2;
```

Here's what that looks like with an actual image:

![](https://endless-sky.github.io/images/blending_modes.jpg)

In effect, the blending equation has been changed to:

```c++
result color = object color + (1 - alpha / 2) * background color
```

That is, we're adding somewhere between 50% and 100% of the background to the object's color. This is useful for objects like explosions, which are glowing (adding light) but also ought to block some of the light behind them.

# Input files

One problem with all of this: depending on your graphics program, it can be really hard to save a PNG files where all the alpha values are zero, and yet the color values are perfectly preserved. What's more, even if you are able to save such a file, opening it in most image viewers will show you a transparent image, rather than the colors that you want to see.

For this reason, Endless Sky is designed to do the premultiplication after loading the image, based on the image name. Each image can (optionally) include a blending mode specifier right before the frame index number: `-` for alpha blending, `+` for additive blending, or `~` for half-additive blending. When each image is read, the alpha premultiplication is done, and then the alpha channel is set to 0 if it is an additive image, or divided by 2 if it is half-additive. This makes it very easy to create input images without any special software tools.

For images that are drawn in additive mode, you can supply them as a completely opaque image drawn over a black background. Or, you can just supply an image with transparency, and the additive image will be generated by adding a black background to the image.

The special `=` blending mode marks an image where premultiplication has already been performed. This allows some special tricks. For example, a premultiplied image can contain regions that have an alpha value of zero, so that they are drawn in additive mode instead. This allows a single image to include both an opaque object, and a glowing effect. (See the shuttle for an example of this.) Since most image editing programs automatically clear the color values on transparent pixels, you may need to use a [special tool](https://github.com/endless-sky/endless-sky-tools/blob/master/source/blend.cpp) to combine the opaque image and the additive image.