The map contains various labels for the different regions of space, such as "The Core" and "Paradise Planets".
These labels are sprites drawn on the map the same way as galaxies:

```html
galaxy "label core"
	pos -136 130
	sprite label/core

galaxy "label deep"
	pos -658 -300
	sprite label/deep

galaxy "label dirt belt"
	pos -515 260
	sprite "label/dirt belt"
```

Sometimes, a label is not added until the player has interacted with the local region, such as landing on a planet, or completing some step in a mission. This is done by defining a galaxy as above without a `sprite` and then adding the sprite with an event at the appropriate time:

```html
galaxy "label secret"
	pos -750 615

event "label secret space"
	galaxy "label secret"
		sprite label/secret
```

The existing map labels have been created with InkScape (https://inkscape.org/).
1. Make a screenshot of the map so you can position the text on top of it, and drop that screenshot into a new Inkscape document.
2. Create text with the Zapfino font (you may need to find a download for this), 24-point.
3. Set the tracking (i.e. spacing between letters) to 24 px, and the fill color to `bbc04030`.
4. Create a curve and use `Text` -> `Put on Path` to make the text follow that path. Adjust it as appropriate (possibly adjusting the spacing of individual letters to avoid overlapping the map text).
5. Export as a PNG.
