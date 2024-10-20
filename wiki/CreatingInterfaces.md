# Table of Contents

* [Introduction](#introduction)
* [Defining an interface](#defining-an-interface)
* [Arrangement](#arrangement)
  * [Anchor/alignment](#anchoralignment)
  * [Position and size](#position-and-size)
* [Active and visible conditions](#active-and-visible-conditions)
* [Interface elements](#interface-elements)
  * [Points and boxes](#points-and-boxes)
  * [Sprites, images, and outline](#sprites-images-and-outlines)
  * [Labels, strings, and buttons](#labels-strings-and-buttons)
  * [Bars and rings](#bars-and-rings)
  * [Lines](#lines)
  * [Pointers](#pointers)
  * [Values](#values)

# Introduction

```html
interface <name> [<anchor>]
	anchor [<anchor>]
	(active | visible) [if <condition>]
	point <name>
		from <x#> <y#> [<anchor>]
		[align [<anchor>]]
		[pad <x#> <y#>]
	box <name>
		from <x#> <y#> to <x#> <y#> [<anchor>]
		[align [<anchor>]]
		[pad <x#> <y#>]
	(sprite <sprite>) | (image | outline <name>)
		center <x#> <y#> [<anchor>]
		dimensions <x#> <y#>
		[colored]
	(label <text>) | (string <name>) | (button <key> <text>) | ("dynamic button" <key> <name>)
		from <x#> <y#> [<anchor>]
		[color <color>]
		width <width#>
		[truncate <truncation>]
		[align [<anchor>]]
		[pad <x#> <y#>]
	(ring | bar) <name>
		center <x#> <y#> [<anchor>]
		dimensions <x#> <y#>
		[color <color>]
		[size <size#>]
		[reversed]
		[start angle <angle#>]
		[span angle <angle#>>]
	pointer
		from <x#> <y#>
		to <x#> <y#>
		["orientation angle" <angle#>]
		["orientation vector" <x#> <y#>]
		[color <color>]
	line
		from <x#> <y#> to <x#> <y#> [<anchor>]
		color <color>
	value <name> <value>
```

# Defining an Interface

```html
interface <name> [<anchor>]
```
This marks the beginning of this interface definition and defines the name by which the game code can refer to this interface.
If an interface with this name has previously been defined when this one is loaded, the previously loaded definition will be discarded,
meaning plugins can completely override interface definitions.
This line may optionally set an anchor. This is equivalent to the first line of the interface definition setting the same anchor point:
```html
interface <name>
	anchor [<anchor>]
```

# Arrangement

## Anchor/alignment

All elements can be given an individual alignment, overriding the currently active global anchor for that element only.
All elements can also be given a `pad` value, this defines amount of padding that will be added around the element when drawing it in its bounding box.

```html
	anchor [<anchor>]
```
Sets the "origin" point. The coordinates of elements will be taken relative to that origin.
The origin set by an `anchor` node is applied to all elements following that node until another `anchor` node or the end of the interface.
Except for any elements which define their own `anchor`.
If the node has no additional tokens beyond the first, then `center` is implicitly used and the origin set by this anchor will be the center of the screen, coordinates (0, 0).
The `<anchor>` values take the form:
```html
([(top | bottom)] [(left | right)] | center)
```
If only a vertical or horizontal alignment is provided, the center of that edge will be used:

## Position and size

There are several different ways in which the position and size of an element can be defined. Each element need only make use of one:

```html
from <x#> <y#> to <x#> <y#> [<anchor>]
```
The coordinates for opposing corners of the bounding box are given. An optional anchor override can also be included, which will be applied to both sets of coordinates if present.

```html
from <x#> <y#> [<anchor>]
to <x#> <y#> [<anchor>]
```
Similar to the previous method, however since each of the two sets of coordinates is given separately, they can be given anchor overrides if desired.

```html
from <x#> <y#> [<anchor>]
dimensions <x#> <y#>
```
The position of one corner is given, alongside the size of the box, instead of the opposite corner.
The position of the opposite corner will be derived by adding the x and y values of the `dimensions` to the x and y of the `from` coordinate, respectively.

Dimensions can also be defined as two separate values:
```html
width <width#>
height <height#>
```
Where `width` is equivalent to the `dimensions` x value ande `height` is equivalent to the y value.

```html
center <x#> <y#> [<anchor>]
dimensions <x#> <y#>
```
The `center` coordinate gives the point at the center of the bounding box; the same distance from the left edge as to the right, and the same distance from the top edge as to the bottom.

Wherever any one of these methods of defining position and size is valid, any of the others may also be used instead.

# Active and visible conditions

```html
	(active | visible) [if <condition>]
```
These nodes mean subsequent elements will only be active or visible, respectively, if the given condition was set in the information passed to this interface object when it was drawn.
They are not mutually exclusive: an element can be active and visible, inactive and visible, or inactive and invisible, however, and element cannot be active and invisible.
The requirement will be applied to all subsequent elements until either the end of this interface definition, or until an overriding `active` or `visible` node.
If a node only contains the token `active` or `visible`, with no condition, then any previous condition requirement in place will not apply to subsequent elements, until a new requirement is defined.

# Interface elements

## Points and boxes

The most basic type of element is the `point`:

```html
	point <name>
		(center | from) <x#> <y#> [<anchor>]
		[align [<anchor>]]
		[pad <x#> <y#>]
```
This defines a coordinate which can be used by the game to as an anchor to draw complex graphics not defined in the interface.
For example, the position of the radar is defined as a point.

```html
	box <name>
		from <x#> <y#> to <x#> <y#> [<anchor>]
		[align [<anchor>]]
		[pad <x#> <y#>]
	box <name>
		from <x#> <y#> [<anchor>]
		to <x#> <y#> [<anchor>]
	box <name>
		from <x#> <y#> [<anchor>]
		dimensions <x#> <y#>
	box <name>
		center <x#> <y#> [<anchor>]
		dimensions <x#> <y#>
	box <name>
		center <x#> <y#> [<anchor>]
		width <width#>
		height <height#>
	box <name>
		from <x#> <y#> [<anchor>]
		width <width#>
		height <height#>
```
A box defines a rectangle on screen which can be used as the bounding area for the game to draw more complex items.

There are numerous different element types which can have additional child nodes unique to them:

## Sprites, images, and outlines

```html
	sprite <sprite>
		from <x#> <y#> to <x#> <y#> [<anchor>]
		[inactive <sprite>]
		[hover <sprite>]
```
Allows a sprite, with the path (within the `images` folder) given by `<sprite>`.
It will only be drawn if its `visible` condition is met.
Optional alternative `inactive` and `hover` sprites can also be defined to be drawn instead of the default if the `active` condition of this sprite is not met, or if it is met and the user's mouse is above the bounding box of this sprite, respectively.

```html
	image <name>
		from <x#> <y#> to <x#> <y#> [<anchor>]
```
Provides a named box that can be given an image to be drawn by the game at runtime, only if the `visible` condition is met.

```html
	outline <name>
		from <x#> <y#> to <x#> <y#> [<anchor>]
		[colored]
```
Similar to `image`, however, the `OutlineShader` will be used with the given sprite, instead of drawing the image directly.
If the `colored` child node is present, then a custom color can be set by the game when the sprite to be outlined is set, otherwise, opaque white will be used.

## Labels, strings, and buttons

```html
	(label <text>) | (string <name>) | (button <key> <text>) | ("dynamic button" <key> <name>)
		from <x#> <y#> to <x#> <y#> [<anchor>]
		size <size#>
		color <color>
		inactive <color>
		hover <color>
		truncate (none | front | middle | back)
```
Defines a location for text to be drawn.
In the case of a label, the given text will be drawn directly.
With a string, a name is given, and the game sets the text at runtime, selecting this text location with that name.
A button is similar to a label, except it also accepts a key token. The first character of this token will be sent as keyboard input if the bounding box for this button is clicked while it is visible and active.
A dynamic button **(v. 0.10.5)** is a combination of a button and a string element. It has the functionality of a button, but its caption is retrieved at runtime.

Size defines the font size of the text. The vanilla game supports 14 and 18.

Color sets the named color that will be used for the text when it is visible and active.
If no color is given, the following defaults will be used:
type | color name
-- | --
color (active) | "active"
inactive | "inactive"
hover | "hover"

If `color` is defined but either the inactive or hover color is not, the undefined color will use the given active color.

## Bars and rings

```html
	(bar | ring) <name>
		from <x#> <y#> to <x#> <y#> [<anchor>]
		color <color>
		size <size#>
		[reversed]
		[start angle <angle#>]
		[span angle <angle#>>]
```
Defines a straight line (bar) or circular outline (ring) to be drawn with this interface.
In the case of a ring, it will be drawn anti-clockwise around the center of its bounding box, with a radius equal to half the width of the bounding box. The start and span angle specifies the portion of the ring that is drawn.
A bar will be drawn from the bottom right corner of its bounding box.
At runtime, the game may only partially complete the bar or ring, or segment it, for example, the ship hull status ring, or the fuel bar. If only a part is drawn, the reversed attribute can toggle which end it is filled from.
The size determines the thickness of the bar or ring, the default value is 2.
If no color is given, "active" will be used.

## Lines

```html
	line <name>
		from <x#> <y#> to <x#> <y#> [<anchor>]
		color <color>
```
Defines a line to be drawn with this interface.
If no color is given, "medium" will be used.

## Pointers

```html
	pointer
		from <x#> <y#>
		to <x#> <y#>
		["orientation angle" <angle#>]
		["orientation vector" <x#> <y#>]
		[color <color>]
```
Define a pointer to be drawn with this interface.
The orientation can either be given as an angle in degrees, counting clockwise with 0 being straight up, or a pair of values corresponding to the x and y components of a vector. If no orientation is given, the vector (0, -1) is used.
If no color is given, "medium" will be used.

## Values

```html
	value <name> <value#>
```
Stores a numerical value. This can be any real number, which the game can refer to using the given name.
