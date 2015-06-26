![beams and projectiles colliding with ships](http://endless-sky.github.io/screenshots/small.jpg)

The collision detection algorithm must be able to determine where a projectile intersects the hull of a ship. Each ship image has an alpha channel, so the "hull" can simply be defined as all pixels that are not 100% transparent. A simple way to check for collision would be to look up individual pixels in the image for each ship and see if they are occupied, but this has two issues:

1. The ship sprite is stored in GPU memory, so accessing the sprite may be time-consuming (unless a copy of the alpha mask is stored elsewhere to be used for collision detection).

2. In the case of a beam weapon, it may overlap several hundred pixels of the image, so we would have to trace along the entire beam path, looking up pixel values until we found an occupied one.

A far more elegant solution is to convert the alpha masks into polygonal outlines. For example, these images...

![](https://raw.githubusercontent.com/endless-sky/endless-sky/master/images/ship/firebird.png)
![](https://raw.githubusercontent.com/endless-sky/endless-sky/master/images/ship/falcon.png)
![](https://raw.githubusercontent.com/endless-sky/endless-sky/master/images/ship/star%20queen.png)
![](https://raw.githubusercontent.com/endless-sky/endless-sky/master/images/ship/leviathan.png)

...can be converted into these outlines:

![polygonal outlines generated from alpha masks](http://endless-sky.github.io/images/outlines.png)

##Converting an image to an outline##

The outline conversion algorithm begins by scanning down through the image until it encounters the first non-empty pixel. That pixel becomes the start of the outline. From there, it checks the pixels in the eight basic directions, starting from -90 degrees relative to the most recent direction. When it encounters an occupied pixel, that becomes the next vertex of the outline and the tracing continues from there:

![tracing a mask to generate a polygon](http://endless-sky.github.io/images/tracing.png)

(Note that because of how the tracing is done, if a ship sprite consists of two or more completely disconnected objects, the collision detection will only include one of those objects. It also means that any "holes" inside the ship are not included in the outline.)

This produces an outline that contains all the same details as the original mask. But, every line segment in that outline is at most one pixel wide and one pixel tall, which means there are a _lot_ of line segments. Any collision check done on the outline will take an amount of time proportional to the number of line segments, so the next step is to simplify the outline as much as possible while still remaining faithful to the original. (As long as the collisions occur within a pixel or two of the true outline, they will look "correct" to the player.)

After experimenting with a variety of polygon simplification algorithms, I decided that the [Ramer-Douglas-Peucker algorithm](http://en.wikipedia.org/wiki/Ramer%E2%80%93Douglas%E2%80%93Peucker_algorithm) gives the best results. (See the link for more details on how the algorithm works.) As you can see in the first set of outlines shown above, the results are not always perfectly symmetrical, but they come close enough to representing the original that any collision point I calculate will be accurate to within a pixel.

##Collision detection##

The advantage of polygonal outlines is that a wide variety of simple, elegant algorithms exist for working with them. In Endless Sky, every projectile is represented by a line segment. The start of the line segment is where that projectile is at the start of the current frame, and the end of the segment is where it is at the end of the current frame. So, if any part of the segment intersects the ship outline, the projectile hit that ship during this frame:

![polygonal outlines generated from alpha masks](http://endless-sky.github.io/images/projectiles.png)

Note that there is no conceptual difference between a "beam" and other projectiles; beams are just projectiles that travel very fast and only last a single frame.

Because the ship is moving too, in theory the collision detection should take into account the fact that the ship is at a different position at the end of the frame than at the start. But in practice, most ships move slow enough that this doesn't make a big difference, and if the ship is moving very fast, there's no way for the player to tell if the collision point is inaccurate, anyways.

This allows for a variety of calculations based on the outline:

* Line segment intersection: for projectiles, as described above. This is just done by checking if any line segment in the outline intersects the projectile segment. If so, the closest point of intersection is returned.

* Point in polygon check: for placing ship death explosions, sparks from ion effects, and the glow particles for the jump drive effect. (Random points are chosen within the sprite bounding box, then discarded if they are not inside the ship mask.)

* Range checks for weapons with a blast radius: just checks if any vertex is within the given radius. In theory a line segment could be in range even if neither endpoint is, but the blast radius calculation does not need to be that exact.
