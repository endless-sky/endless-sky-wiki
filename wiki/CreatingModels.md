# Table of Contents

* [Blender](#blender)
* [Templates](#templates)
* [Texturing](#texturing)
* [Updating old models](#updating-old-models)

# Blender
[Blender](https://www.blender.org/) is the main program used to create and modify the models used to make Endless Sky's sprites.

This page is not meant to be a general introduction to Blender. If you wish to learn Blender, seek out another guide, such as [Blender Guru's Donut Tutorial](https://www.youtube.com/watch?v=z-Xl9tGqH14).

Models should be made using Blender 2.80+. Models made in 2.79 and prior will look dark and washed out in newer versions. The process for updating these assets is detailed under [updating old models](#updating-old-models).

# Templates
These templates provide the lighting, scene, and render settings that should be used by every model in the game:

* [Ship template](/templates/shiptemplate.blend)
* [Outfit template](/templates/outfittemplate.blend)

Using these templates will ensure your sprites are stylistically consistent with the rest of the game.

# Texturing
There are three main methods of texturing and post-processing sprites within Endless Sky:
* [Through Blender's in-built compositor](#blenders-in-built-compositor)
* [Through an external image editing program](#external-image-processing)
* [Fully texture-mapping the model in Blender](#directly-texturing-models)

### Blender's in-built compositor
The in-built compositor will apply textures and color grading directly to the image output by Blender.

The default ship template provides an example of a compositing setup that can be used for texturing ships and their thumbnails.

### External image processing
The preferred image editing program is [GIMP](https://www.gimp.org/), as it is a free, open-source editor that anyone wanting to contribute to the game can use.

Editing images in GIMP, some easy ways to make sprites look more detailed and less "artificial" include:

* Find a public domain metal texture. Paste it into a separate layer, setting the layer's blending mode to "overlay" and adding an alpha channel to the layer if necessary. Erase portions of the texture so that it is only applied to certain parts of the sprite. Repeat with other textures until each part of the sprite is covered by at least one.
* In a layer set to "multiply" mode, spray-paint with black to add extra shadows as you see fit. It often also looks good to add dark patches near engine outlets or weapon muzzles, to make that part of the sprite look a bit "burnt."
* Spray-painting dark patches at a few corners or edges can also help make the metal look reflective.
* For ships, in the "multiply" layer, spray paint with red over the yellow or orange paint sections to make the color less uniform.

At a bare minimum, you should edit ship images at a large enough resolution to provide an "@2x" version for high-dpi monitors.

### Directly texturing models
Texture-wrapping ships and outfits is not recommended for less-experienced contributors. Because ships and outfits are generally only seen from specific angles and take up a small portion of the screen, fully texturing a model will provide negligible benefit in the final sprite. Additionally, trying to texture-wrap a model can introduce graphical errors such as texture seams and warped textures.

# Updating old models
Blender 2.80 removed Blender Internal, the rendering engine used for all Endless Sky models up until that point. In order to use those old models in newer Blender versions, they must be updated to use a newer, still supported rendering engine.

The simplest and preferred option for updating models is to copy the entire model, excluding any lights and cameras, and paste it into one of the 2.80-compatible templates, adjusting the scene camera where appropriate. However, the materials used in the model will still need to be manually converted to use nodes, giving them proper reflections and lighting.

If this isn't possible, Blender 2.79 files can be manually modified to work in newer Blender versions by changing the settings in the following tabs:

* ***Render:***
  * **Change Render Engine from EEVEE to Cycles**
    * EEVEE is intended as a real-timer renderer for animations. Since sprites in Endless Sky are mostly static, the higher fidelity from using Cycles is more important.
  * ***Sampling:***
    * ***Render:***
	  * **Increase the number of samples to ~30 and check the "Denoise" option**
	    * This will drastically reduce the amount of noise in the final picture.
		* Some older versions of Blender won't have the Denoise option. In these cases, increase the number of render samples to ~80 instead.
  * ***Light Paths:***
    * **Reduce "Max Bounces" to 0**
	  * As a raytracing renderer, surfaces that are brightly lit in Cycles will bounce some of their color onto nearby surfaces, leading to instances such as orange fins tinting parts of the hull around them orange. While this is more "realistic" and true-to-life, Blender Internal lacked these reflections, meaning that older models will have substantially different shading if light bounces are not disabled.
* ***World:***
  * ***Surface:***
    * **Change the color of the surface from dark-gray to pure black**
	  * This will ensure that your shadows are black as they should be.
  * ***Ambient Occlusion:***
    * **Disable Ambient Occlusion**
	  * This option is only present on older versions of Blender. While active, the lights used in the scene will be overwritten by a strong emission from all directions.

Then all the lights in the scene, including the main light from the Sun, should have their power doubled and their size/angle reduced to 0. A light's size/angle controls how diffuse its light is and how sharp the shadows it casts are; in the vacuum of space, there is nothing to diffuse light before it hits your ship, meaning that shadows should be as harsh as possible.

Afterwards, all the materials used on the model should have nodes enabled; without it, the model will look flat and reflectionless. Enabling nodes may reset the color of the material; this can be circumvented by copying the hex code of the color beforehand.