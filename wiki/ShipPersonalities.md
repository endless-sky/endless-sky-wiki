Each ship in the game can have a variety of "personality" flags that control its behavior. This can be used, for example, to make one ship that goes out of its way to join fights, and another that will desert its friends if the fight starts to turn against it. A ship can have any number of personality flags set, although some combinations may not make sense.

### Flags that control who a ship attacks:

* `pacifist`: will not attack under any circumstances.
* `forbearing`: will not attack unless its shields or hull are below 90% when it is hit, and the projectile that hit it does damage. Beginning in **v. 0.10.0**, a forbearing ship will also attack if its shields or hull are below 90% and it was hit by discharge or corrosion damage respectively, if it was hit by energy or ion damage and its energy is below 50%, if it was hit by heat or burn damage and it is overheated, if it was hit by fuel or leak damage and it doesn't have enough fuel for at least two jumps, if it was hit by scrambling damage and its jam chance is greater than 10%, if it was hit by slowing damage and it is more than 10% slowed, or if it was hit by disruption damage and its shields are more than 50% disrupted.
* `timid`: does not join other people's fights; only attacks targets that are nearby and targeting it.
* `heroic`: will fight even if badly damaged or outgunned, and will attack targets even if they are far away.
* `hunting`: will attack targets even if they are far away. **(v. 0.10.0)**
* `daring`: will fight even if badly damaged or outgunned. **(v. 0.10.0)**
* `nemesis`: only attacks the player's ships.

### Flags that control how a ship attacks:

* `frugal`: does not expend ammunition unless it has lost more than 75% of its health or its government is outnumbered in the current system.
* `disables`: tries to disable enemy ships rather than destroying them.
* `plunders`: will leave enemy ships disabled so that they can be boarded and pillaged before destroying them.
* `vindictive`: this ship continues to fire at its target even after the target is dead, while the target is beginning to explode. **(v. 0.9.0)**
* `unconstrained`: the ship will fly outside the "invisible fence" that ships usually stay within. **(v. 0.9.1)**
* `coward`: if this ship is not the flagship of a fleet, it will desert its flagship and flee the system if its hull and shield strength drop low enough. **(v. 0.9.0)**
* `appeasing`: if this ship's hull and shield strength drops low enough, it will dump cargo to try to distract or appease its attacker. **(v. 0.9.5)**
* `opportunistic`: this ship's turrets will track targets independently instead of focusing fire. They also scan back and forth when no target is present. **(v. 0.9.7)**
* `merciful`: this ship won't fire on ships that are fleeing. **(v. 0.10.0)**
* `ramming`: will attempt to get as close to its target as possible, regardless of the range of its weapons or whether its weapons would hurt itself. **(v. 0.10.0)**

### Non-combat goals:

* `surveillance`: this ship will have more aggressive scanning behavior. Ships with this personality will also deploy their fighters before leaving the system, and will "scan" stellar objects in the system if the "atmosphere scan" attribute is present on the ship. Won't assist the player with refueling or repairs.
  * The following are the default behaviors of ships with scanners compared to the more aggressive behavior of surveillance ships:
    * Ships will make up to 6/12 (non-surveillance/surveillance) successful scans in the system before no longer scanning more ships.
      * Cargo and outfit scans are counted individually.
    * Ships will search for up to 2.5/5 minutes for ships to scan and will stop scanning if this time has elapsed and they don't currently have a ship they're trying to scan.
    * Ships will give up on scanning if the above time has elapsed, plus an additional minute if they're currently in the process of scanning a target.
    * Surveillance ships will pursue their scan target regardless of distance. Non-surveillance ships will give up if you get too far away (2x their scan range).
    * Surveillance ships will prefer to target ships that are close to them (within 2x their scan range), but may decide to randomly target any ship in the system regardless of distance if there are no ships nearby to scan.
  * Prior to **v0.10.7**, ships with this personality would scan ships that they had not personally scanned and did not have the behavior described above that limited their scan time/count. After this version, it was changed so that ships only scan ships that haven't yet been scanned by their government.
* `mining`: this ship will circle around the system looking for minable asteroids. When it finds one it will attack it. **(v. 0.9.5)**
* `harvests`: this ship picks up any flotsam that is in front of it. Mining ships should do this, as should pirates who are distracted by ships dumping cargo to appease them. **(v. 0.9.5)**
* `swarming`: ships of this type will "swarm" around any friendly, non-swarming ships that are in-system. No more than six ships will swarm a given target. Any swarming ship with nothing to swarm will try to land on a planet instead. **(v. 0.9.1)**
* `secretive`: ships with this personality will attempt to fly away from ships that are scanning them. **(v. 0.10.0)**
* `unrestricted`: does not adhere to the travel restrictions defined by its government. This only applies to random fleet spawns, as mission NPCs are unrestricted by default. **(v. 0.10.3)**
* `getaway`: if its cargo hold is full, this ship will ignore combat and leave the system. **(v. 0.10.11)**

### Flags that control [NPCs](CreatingMissions#non-player-characters-npcs):

* `staying`: never leaves the system it starts out in.
* `lingering`: will remain in the system for an additional time equal to one quarter of the smallest period of any fleet assigned to the system, when there is nothing to do. Except when the ship is a mission NPC and can follow the player, or when it already has a destination system. **(v. 0.10.1)**
* `entering`: enters its starting system via hyperspace instead of appearing there.
* `waiting`: starts out in space in its system.
* `launching`: takes off from the planet with the player, even if it normally couldn't land there. **(v. 0.9.9)**
* `fleeing`: avoids combat if it can jump or land, and will not take off after landing until the player next takes off.
* `derelict`: starts out disabled.
* `uninterested`: does not follow the player's flagship around (i.e. does not behave like an escort).
* `restricted`: adheres to the travel restrictions defined by its government. This only applies to mission NPCs, as random fleet spawns are restricted by default. **(v. 0.10.5)**

### Special flags:

* `escort`: this ship will show up in the player's escort list. (Use this for mission NPCs that you are supposed to accompany, for example.)
* `target`: this ship is highlighted by making it flash in the radar, and in the target display it is labeled as a "mission target." **(v. 0.9.7)**
* `tracked`: the system that this ship is located in is marked on the map. **(v. 0.10.17)**
* `marked`: only attacks player ships, and only player ships will attack it. **(v. 0.9.7)**
* `mute`: this ship will not talk to you if you hail it (and therefore can't assist you, either). **(v. 0.9.7)**
* `quiet`: this ship will not send hails to you passively, but will still respond to hails if you hail it directly. **(v. 0.10.7)**

### Confusion

Confusion is provided as a "child" node under `personality`.
```html
confusion <name>
confusion
	...
```
"Confusion" sets the [confusion settings](CreatingConfusions) of ships using this personality, overriding any confusion settings provided by the ships' government. Personalities can refer to a preexisting confusion profile, or can define their own custom confusion within themselves, using the same syntax as normal `confusion` data entry.
