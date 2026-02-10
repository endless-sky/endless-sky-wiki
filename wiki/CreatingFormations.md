# CreatingFormations

The syntax for the definition of a formation is:

```html
formation <name>
	flippable x y
	rotatable <degree of symmetry>
	arc
		start <x> <y>
		angle <angle>
		skip (first | last)
		positions <positions>
		repeat
			start <x> <y>
			angle <angle>
			positions <positions>
	line
		start <x> <y>
		end <x> <y>
		positions <positions>
		skip (first | last)
		repeat
			start <x> <y>
			end <x> <y>
			positions <positions>
	position <x> <y>
		repeat <x> <y>
```

# Symmetry

If a formation is symmetrical, the definition can include an axis or degree of symmetry. In the event of escorts losing their formation, such as when the flagship rotates faster than the escorts can move, they will re-establish the formation referenced from the nose or one of the symmetry directions, whichever is closer.

```html
	flippable x y
```

The `flippable` attribute allows the x or y (or both) axis to be specified as an axis of symmetry.

```html
	rotatable <degree>
```

`rotatable` specifies a degree of rotational symmetry for the formation, such as 90 for squares, and 1 for circles.

Both attributes essentially add reference directions for the formation, from only one pointing out the nose to any axis or multiple of degrees thereof.

# Repeats

```html
	repeat
		<start> <x> <y>
		...
```

To make formations scalable to any number of ships, a repeat attribute must be applied to the block. Any attribute that exists in a block can be used as a child attribute in `repeat`.

When a block is fully populated with ships, if it has `repeat`, the block will be rendered again with any children of `repeat` added to their corresponding attributes in the block.

For instance,
```html
	position 0 200
		repeat 0 200
```
Will render the first position at 0,200 and then 0,400; 0,600; etc.

If no repeat attribute is present, all positions will be filled, and any remaining ships will group at 0,0 underneath the flagship.

# Positions
```html
	positions 2
	repeat
		positions 2
```

The `positions` attribute is used in `arc` or `line` to specify the number of positions that can be filled along the shape. Positions are spread out evenly across the shape, starting at each end.

# Blocks

Blocks can be one of three types: `arc`, `line`, and `position`.

<degrees> are clockwise
<x> is positive right, negative left
<y> is positive back, negative forward

```html
	arc
		start <x> <y>
		angle <degrees>
		skip (first | last)
		positions <positions>
		repeat
			...
```
`arc` populates an arc around the player.
	`start`: Specifies the start location of the arc.
	`angle`: Arc distance, referenced clockwise around the player at a constant distance.
	`skip (first | last)`: Prevents the filling of either the first or last position in a block. Useful for when starting points overlap, such as a 360 arc.
	`positions`: Number of positions in the shape, modified by `skip`

```html
	line
		start <x> <y>
		end <x> <y>
		skip (first | last)
		positions <positions>
		repeat
			...
```
`line` specifies a line along which escorts are located.
	`start`: Starting point of the line
	`end`: Ending point of the line
	`skip (first | last)`: Prevents the filling of either the first or last position.
	`positions`: Number of spots for ships along the line.

```html
	position <x> <y>
		repeat <x> <y>
```
`position` blocks render a single position at the given coordinates. 
	`repeat` Unlike other blocks, repeat criteria are written directly onto `repeat`. Both `<x>` and `<y>` must be specified whenever `repeat` is used.

All blocks are rendered from top to bottom before any block is repeated, and all will repeat top to bottom.
