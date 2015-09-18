### Outfit graphics

The thumbnail graphics for all the outfits in the game are created using two free, open source programs: [Blender](https://www.blender.org/) for creating the 3D models, and [GIMP](http://www.gimp.org/) for post-processing the rendered images to look more grungy and less artificial. (Another open source program, [Inkscape](https://inkscape.org), is used for the vector graphics in the user interface.) You can download the original Blender and GIMP files for any of the graphics [here](https://drive.google.com/open?id=0B9aK8dG39P29fkdBeUJjSXJYVDdjMEpkOXh3T1NDekFYaTEtbkdTdzVwX2NTUWVVT3BUWVk).

Any outfit model you create in Blender should use the camera and lighting settings from [this template](https://drive.google.com/open?id=0B9aK8dG39P29MzNnc1FWOGVHZ3c). That will ensure that your new thumbnails do not look out of place next to the existing ones. The template is set up with sunlight coming from a certain angle and with the image rendered in an orthographic projection along the XYZ diagonal, so that a cube in your model will line up exactly with the cubical grid used as a backdrop for the outfits.

![](https://raw.githubusercontent.com/endless-sky/endless-sky/master/images/outfit/particle%20cannon.png)
![](https://raw.githubusercontent.com/endless-sky/endless-sky/master/images/outfit/meteor%20launcher.png)
![](https://raw.githubusercontent.com/endless-sky/endless-sky/master/images/outfit/anti-missile.png)
![](https://raw.githubusercontent.com/endless-sky/endless-sky/master/images/outfit/medium%20ion%20thruster.png)

The outfitter view divides outfits into difference categories: "Guns," "Turrets," etc. As another way of ensuring a consistent look, most outfits in a given section point in a certain direction:

* Guns: top left.
* Turrets: top left, and angled slightly up in the Z axis.
* Missiles: bottom left.
* Anti-missiles: top right.
* Engines: bottom right.

In a few cases, it's ambiguous which direction something should point in. For example, the Flamethrower is aligned with the guns even though it is shown in the "Secondary Weapons" section with all the missiles.

![](https://raw.githubusercontent.com/endless-sky/endless-sky-high-dpi/master/images/outfit/huge%20ion%20steering%402x.png)
![](https://raw.githubusercontent.com/endless-sky/endless-sky-high-dpi/master/images/outfit/security%20station%402x.png)

As with [ships](CreatingShips), it is often easier to add texture and color variation to the outfit images in GIMP rather than trying to texture map all the surfaces in Blender. And, also as with the ship graphics, be sure that your GIMP file is at least high enough resolution to create an "@2x" high resolution version.

### Outfit attributes

Outfits work by modifying the attributes of your ship. Many of the attributes are in units of heat or energy per frame; a frame is 1/60th of a second. All attributes stack. For example, if you install two of the same scanner, your scanning range doubles. If you install two shield generators, your shield regeneration doubles. The attributes include:

* `afterburner energy`: energy consumed by the afterburner in one frame.
* `afterburner fuel`: fuel consumed by the afterburner in one frame.
* `afterburner heat`: heat produced by the afterburner in one frame.
* `afterburner thrust`: thrust produced by the afterburner in one frame.
* `atmosphere scan`: not usable by the player, but indicates that the AI will treat this ship as if it wants to fly past planets to "scan" them.
* `bunks`: additional crew / passenger space.
* `cargo scan`: sets the distance from which this outfit can be used to scan a ship's cargo.
* `cargo space`: amount of cargo space that this outfit adds (if positive) or removes (if negative) from your ship.
* `cloak`: how quickly a cloaking device cloaks or uncloaks. When cloaking, this amount is added to your cloaking each frame until you reach 1, at which point your ship is fully cloaked. The opposite happens when uncloaking.
* `cloaking energy`: how much energy it takes each frame to maintain cloaking.
* `cloaking fuel`: how much fuel it takes each frame to maintain cloaking.
* `cooling`: heat subtracted from your ship by this outfit, per frame.
* `cost`: cost of this outfit, in credits.
* `drag`: please do not create outfits that reduce a ship's drag, because if the drag becomes zero or negative it can cause problems.
* `energy capacity`: how much energy your ship can store.
* `energy generation`: energy generated each frame. If you do not have any energy capacity, this energy does not carry over from frame to frame.
* `engine capacity`: if negative, how much engine space an outfit uses. Engine space and weapon space are an important part of giving different ship models different strengths and weaknesses, so I recommend against creating outfits that give you extra engine capacity, or engines that do not use up engine space.
* `fuel capacity`: a hyperjump takes 100 fuel. Jumping with a scram drive takes 150 fuel, and a jump drive takes 200.
* `gun ports`: any weapon that fires forward (i.e. is not a turret) must have a value of -1 for this attribute. That is how the game recognizes that it is a gun. Outfits cannot provide more gun ports, because each gun must be tied to a specific hardpoint in the ship model. The only way to get more hardpoints is to upgrade to a bigger ship.
* `heat dissipation`: outfits should not modify this. If you want an outfit to help cool off a ship, use the "cooling" attribute instead.
* `heat generation`: how much heat this outfit generates every turn, regardless of whether it is working at full capacity or not. For example, a power generator will create its full amount of heat even if your batteries are fully charged and the energy it creates is just being thrown away.
* `hull`: I recommend against providing outfits that give ships additional hull strength, because that could create balancing issues. But if you want to do it, this is the attribute to modify.
* `hull repair rate`: the number of hull points regenerated per frame. It takes 1 energy to repair 1 unit of hull.
* `hyperdrive`: set this to 1 if an outfit is a hyperdrive.
* `illegal`: the fine, in credits, for being caught using this outfit.
* `jump drive`: set this to 1 if this outfit is a jump drive.
* `jump speed`: how slow your ship must be moving in order to jump.
* `map`: number of star systems that are mapped by this outfit.
* `mass`: how much your ship's mass should change by when this outfit is installed.
* `outfit scan`: sets the distance from which this outfit can be used to scan a ship's outfits.
* `outfit space`: if negative, how much generic outfit space (as opposed to weapon or engine space) this outfit takes up.
* `ramscoop`: fuel regeneration. Each frame, your ship gains fuel equal to .03 * sqrt("ramscoop"). The square root is so that each additional ramscoop will have less effect than the previous one; otherwise, ramscoops would make weapons and afterburners that run on fuel way too powerful.
* `required crew`: turrets (and maybe other high-end outfits) can increase your crew requirements. It might also make sense to provide outfits, such as an android crew replacement, that reduce crew requirements.
* `reverse thrust`: if your ship has reverse thrusters installed, the "back" button will apply reverse thrust instead of turning your ship.
* `reverse thrusting energy`: energy cost per frame of reverse thrusting.
* `reverse thrusting heat`: heat produced per frame of reverse thrusting.
* `scan interference`: your odds of a scan of your ship discovering anything illegal you have are equal to 1 / (1 + scan interference). For example, if "scan interference" is 3 you evade 75% of scans.
* `scram drive`: a scram drive can engage as long as your ship is moving in the direction of the target system. This value is how much your ship can be drifting relative to that vector and still be allowed to jump.
* `shield generation`: the number of shield points regenerated per frame. It takes 1 energy to regenerate 1 unit of shields, so if your shields are recharging your ship has less energy available for other things.
* `shields`: I recommend against providing outfits that give ships additional shield strength, because that could create balancing issues. But if you want to do it, this is the attribute to modify.
* `thrust`: your ship's acceleration per frame equals thrust / mass.
* `thrusting energy`: energy cost per frame of thrusting.
* `thrusting heat`: heat generated each frame when thrusting.
* `turn`: your ship's turn rate is (turn / mass) degrees per frame.
* `turning energy`:  energy cost per frame of turning.
* `turning heat`: heat generated each frame when turning.
* `turret mounts`: as with "gun ports", any outfit that fits in a turret hardpoint must have a value of -1 for this.
* `weapon capacity`: set this to a negative value (generally, the same value as "outfit space") for any outfit that is a weapon. Limited weapon space is one of the ways that ship models are differentiated from each other.

### Weapon attributes

An outfit that provides a weapon contains an extra set of attributes inside a "weapon" block. These attributes must be placed in the "weapon" block to have any effect. If you have multiple "weapon" blocks in one outfit, they are just merged together; that doesn't make an outfit provide two different weapons. Some weapon attributes have special formats:

* `sprite`: the path to the sprite, relative to the "images" folder, and not including the frame number or extension (e.g. "projectile/flamethrower", not "images/projectile/flamethrower+0.png"). The sprite field can also have "child" elements, which include:
  * `frame rate`: animation frames per second.
  * `start frame`: which frame to start the animation at.
  * `random start frame`: start the animation at a random frame (e.g. so if you fire a bunch of projectiles, they won't all be pulsing through the animation in unison with each other).
  * `no repeat`: once you reach the last animation frame, stop.
  * `rewind`: when you reach the last animation frame, instead of starting over at the beginning, play the animation back in reverse.
* `sound`: a path to a sound, relative to the "sounds" folder, and not including the extension or the loop specifier (e.g. "laser", not "sounds/laser~.wav").
* `ammo`: if specified, an outfit which provides ammunition for this weapon. Each time it is fired, one outfit of that type is removed from your ship.
* `icon`: for secondary weapons, the icon that will be shown along with this weapon's ammunition count.
* `fire effect`: an "effect" object that will be created when this weapon fires (such as a smoke cloud from a missile launch). You can specify a number to create more than one instance of the effect.
* `hit effect`: an "effect" object to be created when this projectile hits something. You can specify a number to create more than one instance of the effect. You can also specify multiple different hit effects.
* `die effect`: an "effect" object to be created if this projectile reaches the end of its lifetime without hitting anything.
* `submunition`: if the projectile reaches its end of life, create a new set of projectiles based on the given weapon outfit. 

Ordinary weapon attributes include:

* `lifetime`: how long the projectile lasts before it "dies."
* `reload`: how many frames this weapon takes to reload: 1 means it fires every turn (e.g. most beam weapons), and 60 means it fires once per second.
* `homing`: How good this weapon is at seeking its target:
  * 0: no homing.
  * 1: projectile loses its homing ability if no longer facing toward the target.
  * 2: dumb homing (always try to turn to point toward the target).
  * 3: stop thrusting if you miss the target, in order to turn back towards it in a tighter loop.
  * 4: rather than moving directly towards the target, calculate an interception point based on the projectile's speed and the target's current speed.
* `missile strength`: how hard a projectile is for an anti-missile to destroy. If this is 0, the projectile cannot be destroyed by anti-missile.
* `anti-missile`: weapon's ability to shoot down missiles. The anti-missile succeeds if a random integer less than this value is greater than a random integer less than the missile's strength.
* `velocity`: initial velocity of the projectile, relative to whatever fired it (which may be a ship or a "parent" projectile of which this is a submunition).
* `acceleration`: projectile's acceleration, per frame. If this is nonzero, it should be coupled with a nonzero "drag" to limit the projectile's total speed.
* `drag`: percent loss of speed per frame. For example, a projectile with an acceleration of 1 and a drag of .1 will have a maximum speed (acceleration / drag) of 10.
* `turn`: If the projectile is homing, how much it is allowed to turn each frame to seek its target. Otherwise, it will *always* turn by this amount, which can be used to create spinning projectiles or other effects.
* `inaccuracy`: the maximum error, in degrees, in a projectile's firing angle. Inaccuracy limits a weapon's effectiveness at long range but can also make it look a bit more "natural" and unpredictable.
* `firing energy`: the energy cost to fire this weapon.
* `firing force`: the thrust generated by firing this weapon.
* `firing fuel`: fuel consumed when this weapon fires.
* `firing heat`: heat produced when this weapon fires.
* `trigger radius`: how close a projectile must be to a hostile target to trigger its explosion. This only makes sense to use with weapons with a "blast radius" at least as big as their "trigger radius."
* `blast radius`: all ships (friendly and hostile) within this radius are damaged if this projectile explodes.
* `shield damage`: how much damage a projectile does to shields.
* `hull damage`: how much damage a projectile does to hull.
* `heat damage`: how much heat is added to a target when struck by this projectile. If the target's shields are up, heat damage is cut in half.
* `ion damage`: how much ionization is added to a target when struck by this projectile. If the target's shields are up, ionization is cut in half. Ionization drains energy and dissipates at a rate of 1% per frame. For example, a ship that takes 10 ion damage will lose 10 energy that frame, 9.9 energy the next frame, 9.801 energy the next, and so on until ionization tapers off to 0. That means the total energy loss from that one ion impact will be 10 + 99% * 10 + 99% * 99% * 10 + ... = 10 / 1% = 1000 energy.
* `hit force`: how much thrust is applied to a ship when this projectile strikes it. If this is negative, the ship is pulled towards the projectile.

### Sales

In order for anyone to buy your new outfit, it must be added to one of the "outfitter" objects. For example, if you are writing a plugin you could include this in one of your data files:

```
outfitter "Syndicate Advanced"
    "My Fancy New Outfit"
    "My Other Fine Outfit"
```

Any outfits you list will be appended to the outfits currently in the list you named. So, the above example would make two new outfits available on all planets that have the "Syndicate Advanced" outfits.

### Balancing

In Escape Velocity, the classic series of games that Endless Sky is patterned after, there were some outfits that were so powerful compared to their size that there was no reason not to install them if you could afford it: for example, outfits that took no space and improved your acceleration and turn rate, or very small outfits that boosted your shields considerably. As a result, those games needed to put a limit on how many of each outfit could be installed.

In Endless Sky, I'm attempting to have the balancing happen naturally, without putting fixed limits on how many copies of an outfit you can install. For example, that means that more powerful outfits tend to be larger or much more expensive (or both), and may have other side effects like generating a lot of heat or requiring more energy than other alternatives.

However, it is still quite possible to do things like buying a bulk freighter and installing 30 mass expansions, which then lets you pack in an absurd number of shield generators. If outfits were provided that could increase your shield or hull strength, or your weapon or engine capacity, it would be very hard to keep the game balanced - a Bactrian freighter would have the capacity to be far more powerful than any warship, just because of its large cargo space.

So, when creating new outfits, it's important to keep in mind not just what you think would be cool, but whether your new outfits will be unbalanced. If you're creating a weapon that is so good that no ship will want to install anything else, that's a balance problem.