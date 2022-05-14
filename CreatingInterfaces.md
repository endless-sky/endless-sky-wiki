# Defining an interface

```
interface "hud"
	# Player status.
	anchor top right
	
	sprite "ui/status"
		from 0 0
		align top right
	
	string "location"
		from -160 25
		color "medium"
		align right
		width 140
		truncate back
	string "date"
		from -20 45
		color "medium"
		align right
	string "credits"
		from -20 65
		color "medium"
		align right
	
	outline "player sprite"
		center -75 155
		dimensions 70 70
	ring "shields"
		center -75 155
		dimensions 120 120
		color "shields"
		size 1.5
	ring "hull"
		center -75 155
		dimensions 110 110
		color "hull"
		size 1.5
	ring "disabled hull"
		center -75 155
		dimensions 110 110
		color "disabled hull"
		size 1.5
	bar "fuel"
		from -53.5 425
		dimensions 0 -192
		color "fuel"
		size 2
	bar "energy"
		from -33.5 415
		dimensions 0 -192
		color "energy"
		size 2
	bar "heat"
		from -13.5 403
		dimensions 0 -192
		color "heat"
		size 2
	bar "overheat"
		from -13.5 403
		dimensions 0 -192
		color "overheat"
		size 2
	bar "overheat blink"
		from -13.5 403
		dimensions 0 -192
		color "dim"
		size 2

	# Targets.
	anchor top left
	sprite "ui/radar"
		from 0 0
		align top left
	point "radar"
		center 128 128
	value "radar radius" 110
	value "radar pointer radius" 130
	
	sprite "ui/navigation"
		from 200 0
		align top left
	string "navigation mode"
		from 215 20
		align left
		color "medium"
	string "destination"
		from 230 40
		align left
		color "medium"
		width 135
		truncate back
	
	sprite "ui/target"
		from 0 240
		align top left
	point "target"
		center 75 315
		dimensions 140 140
	value "target radius" 70
	outline "target sprite"
		center 75 315
		dimensions 70 70
		colored
	ring "target shields"
		center 75 315
		dimensions 120 120
		color "shields"
		size 1.5
	ring "target hull"
		center 75 315
		dimensions 110 110
		color "hull"
		size 1.5
	ring "target disabled hull"
		center 75 315
		dimensions 110 110
		color "disabled hull"
		size 1.5
	
	visible if "range display"
	sprite "ui/range"
		from 130 263
		align top left
	string "target range"
		from 160 260
		align top left
	visible if "tactical display"
	sprite "ui/tactical"
		from 130 290
		align top left
	string "target crew"
		from 162 298
		align top left
	string "target fuel"
		from 162 318
		align top left
	string "target energy"
		from 157 338
		align top left
	string "target heat"
		from 147 358
		align top left
	visible
	
	string "target name"
		center 75 395
		color "bright"
		width 150
		truncate middle
	string "target type"
		center 75 415
		color "medium"
		width 150
		truncate middle
	string "target government"
		center 75 435
		color "medium"
		width 150
		truncate middle
	point "faction markers"
		center 75 435
	string "mission target"
		center 75 455
		color "medium"
	
	# Other HUD elements:
	box "escorts"
		from 0 460 top left
		to 120 0 bottom left
	box "messages"
		from 120 0 bottom left
		to -110 -200 bottom right
	box "ammo"
		from -110 450 top right
		to 0 0 bottom right
	anchor top
	point "mini-map"
		center 0 100
```

`interface "hud"`
Start with the `interface` keyword, then give it a name, this is how the interface will be identified in the code so the name needs to be unique, another interface definition using the same name being parsed will clear the elements defined here.

`anchor top right`
Alignment, defined with the `anchor` keyword. Default alignment is to centre. Other valid options are `top`, `bottom`, `left`, and `right`. If both `top` and `bottom` or both `left` and `right` are used, the last from each pair will apply.
Following a use of the `anchor` keyword, all subsequent elements will share that alignment until a new alignment is set (with the `anchor` keyword) or the interface definition ends.
Alignment can also be defined at the beginning of the interface definition, without the use of the anchor keyword:
`interface “map” bottom`
`interface "map buttons" bottom right`
Coordinates are applied relatively to the anchor point.

Alignment can be overridden for a single line:
`box "escorts"`
`	from 0 460 top left`
`	to 120 0 bottom left`
`box "messages"`
`	from 120 0 bottom left`
`	to -110 -200 bottom right`
The ‘escorts’ box starts at 0 460, relative to the top left, to 120 0, relative to the bottom left. The `messages` box starts at 120 0, from the bottom left and ends at 110 -200 from the bottom right.
This ensures that the `escorts box` has exactly 460 units of space above it, no space to the left, is 120 units wide, and has no space below it regardless of the size of the game window.
The `messages` box will always have 120 units to the left and run along the bottom edge of the window, be exactly 200 units tall and have 110 units to the right.

### Active and Visible

`active` and `visible` provide conditions for the following elements to be visible or active. Active buttons can be clicked, inactive ones cannot.
The condition defined by an `active` or `visible` line will apply to all subsequent elements until another `active` or `visible` line, respectively, sets a new condition or the end of the interface definition.
When setting a condition, use the syntax:
`active if “condition”`
`visible if “condition”`
`”condition”` can contain an exclamation mark (!) to apply a boolean NOT to the string.
`”condition”` is true when “condition” is present, `”!condition”` is true when “condition” is not present.
To clear an activation or visibility requirement, just use the keyword with no additional values:
`active`
`visible`

### Value

`value "radar radius" 110`
`value "radar pointer radius" 130`
Defines a named numerical value that can be accessed by the game code (using the given name).

### Point and box

`point "target"`
`	center 75 315`
`	dimensions 140 140`
`box "outfits"`
`	from -250 -290 to 500 30`
Defines a region for the game to draw in.
`point` and `box` are aliases.
Values following the `center` keyword give the coodrinates of the center of this region.
Default dimensions are 0 0.

### Sprite, image and outline

`sprite "ui/radar"`
`	from 0 0`
`	align top left`
These keywords create ImageElements.
Where `sprite` is used, the following value, in this case, `”ui/radar”` should be the path and name of the image file to use (without an extension) within the images folder of the game files.
`image` and `outline` allow the game to place an image at that location, access by the name assigned in place of a sprite address.
The `outline` keyword tells the game to only draw the outline of the image

### Label, string and button

`label "Ship Info"`
`	center -300 -305`
`	color "bright"`
`label "Player _Info"`
`	center -420 -305`
`button i`
`	center -420 -305`
`	dimensions 120 30`

`string "header"`
`	from -50 -65`
`	align left`
`	width 330`
`	truncate back`
`button d "_Done"`
`	center 250 115`
`	dimensions 80 30`
`label` displays the text in the following value.
`string` creates a named element for the game to insert its own text into at runtime.
`button` creates a clickable button at the given location with the given size.
The character following the keyword is virtually pressed when the button is clicked, this is how the panel knows the button was clicked. Pressing this key on the keyboard also has the effect of clicking the button. Use a capital letter to prevent the keyboard from selecting this button:
`button R "Remove"`
`	center 75 155`
`	dimensions 90 30`
The optional text following this will be displayed on the button.
In both the `label` and `button` strings, underscores will not appear in the displayed text, instead, holding down alt will underline the character immediately after the underscore.

### Bar and ring

`ring "disabled hull"`
`	center -75 155`
`	dimensions 110 110`
`	color "disabled hull"`
`	size 1.5`
`bar "fuel"`
`	from -53.5 425`
`	dimensions 0 -192`
`	color "fuel"`
`	size 2`


### Line

`line`
`	from -60 95`
`	dimensions 480 1`
