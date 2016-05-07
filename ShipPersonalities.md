Each ship in the game can have a variety of "personality" flags that control its behavior. This can be used, for example, to make one ship that goes out of its way to join fights, and another that will desert its friends if the fight starts to turn against it. A ship can have any number of personality flags set, although some combinations may not make sense.

Flags that control who a ship attacks:

* `pacifist`: will not attack under any circumstances.
* `forbearing`: will not attack unless its shields are reduced to below 90%.
* `timid`: does not join other people's fights; only attacks targets that are nearby and targeting it.
* `heroic`: goes out of its way to join fights when an ally is threatened.
* `nemesis`: only attacks the player's ships.
* `unconstrained`: the ship will fly outside the "invisible fence" that ships usually stay within. **(v. 0.9.1)**

Flags that control how a ship attacks:

* `disables`: tries to disable enemy ships rather than destroying them.
* `plunders`: will board and pillage enemy ships before destroying them.
* `frugal`: does not expend ammunition unless it has lost more than 50% of its shields or its government is outnumbered in the current system.
* `vindictive`: this ship continues to fire at its target even after the target is dead, while the target is beginning to explode. **(v. 0.9.0)**

Flags that control [NPCs](https://github.com/endless-sky/endless-sky/wiki/CreatingMissions#npcs):

* `staying`: never leaves the system it starts out in.
* `entering`: enters its starting system via hyperspace instead of appearing there.
* `waiting`: starts out in space in the given system, then follows you.
* `uninterested`: does not follow the player's flagship around (i.e. does not behave like an escort).
* `fleeing`: tries to run away from the player.

Other flags:

* `surveillance`: scans random ships and visits random planets in system.
* `derelict`: starts out disabled.
* `coward`: if this ship is not the flagship of a fleet, it will desert its flagship and flee the system if its shields drop to zero. **(v. 0.9.0)**
* `swarming`: ships of this type will "swarm" around any friendly, non-swarming ships that are in-system. No more than six ships will swarm a given target. Any swarming ship with nothing to swarm will try to land on a planet instead. **(v. 0.9.1)**

In addition to these flags, the personality also stores a "confusion." This is a random error that is added to a ship's targeting systems, to make it look a bit more random and organic; otherwise, all AI ships would always fire at the exact center of their target, which looks rather unrealistic. The confusion value is the maximum error in pixels, and defaults to 10. Generally I use a value of 10 for military ships, 20 for pirates, 30 for skilled merchants and 40 for ordinary merchants.