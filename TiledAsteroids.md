The purpose of asteroids is to make missiles (and other long-range weapons) more effective in some systems than in others. The player can also intentionally maneuver to use an asteroid as cover, although this is hard to do reliably.

The entire star system should appear, to the player, to be filled with asteroids. Even if the asteroid field only extended out to the last orbits in a system, in some cases that would require over ten thousand asteroids to be tracked. And if the asteroid field extends an infinite amount in each direction, then there is no way every asteroid the player sees can be tracked independently.

Some space games, such as the Escape Velocity series that Endless Sky is patterned after, solve this by having the asteroid field only extend slightly beyond the limits of the screen. When an asteroid moves outside that area, it "wraps around" and reappears on the other side. This creates the illusion of an endless asteroid field, but it means that missiles that are not on-screen cannot be destroyed by asteroids:

![polygonal outlines generated from alpha masks](http://endless-sky.github.io/images/ev asteroids.png)

If asteroids work this way, then you can "cheat" by making sure that whatever ship you are bombarding with missiles is off-screen; if so, once your missiles are off-screen too they are safe from asteroids. This also means that in asteroid-dense systems, the outcome of a battle can change significantly depending on whether the battle is on-screen or not: the player is essentially dragging a movable asteroid cloud around with them that they can use to block missiles wherever they go.

Endless Sky's solution is to "tile" the asteroid field:

![polygonal outlines generated from alpha masks](http://endless-sky.github.io/images/tiled asteroids.png)

As before, the asteroids are all confined to a rectangular area, and if one of them goes outside that area, it reappears on the opposite edge. But now, that area repeats over and over again, infinitely. When checking for collisions between a missile and an asteroid, the missile figures out where within its particular tile it is, and then checks if there is an asteroid there. (The diagram above shows the tile being about half the screen width, but the actual tiles are 4096 x 4096 pixels, far larger than most monitors. The star background tiles in the same way.)

This means that every asteroid you see is actually just one copy of a master pattern that is being repeated over and over throughout the star system. Now, suppose it were possible to destroy asteroids, and the Rainmaker's missile in the drawing above had destroyed the off-screen asteroid that it hit. The tiled copies of that asteroid that are visible on-screen would suddenly disappear as well, for no apparent reason:

![polygonal outlines generated from alpha masks](http://endless-sky.github.io/images/exploding asteroid.png)

There are ways to work around this: for example, when an asteroid is struck by a weapon the game could track which tile that asteroid is in, and it could keep a list of which asteroids in which tiles have been destroyed or damaged. The asteroid drawing and collision code would then need to check the list every time to see whether a particular copy of a particular asteroid still exists. That would significantly increase the complexity of the code that handles asteroids, especially if it were also possible for a weapon impact to change the course of an asteroid or cause it to split in two.