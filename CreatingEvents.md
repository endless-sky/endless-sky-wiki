An "event" is something that happens on a certain in-game date, and that can modify the attributes of any of the following data elements:
* fleet
* galaxy
* government
* outfitter
* planet
* shipyard
* system

Each event has a name, and can specify what date it happens on. (Most events, however, will be triggered by missions and therefore will not have a fixed date.) The event can then list any number of changes to any of the data types listed above. For example:

```
event "war begins"
    date 4 7 3014
    system "Gamma Corvi"
        government "Free Worlds"
        fleet "Small Southern Merchants" 600
        fleet "Large Southern Merchants" 1400
        fleet "Small Free Worlds" 600
        fleet "Large Free Worlds" 1300
```

An event definition can also include instructions to change the values of condition variables. And, when an event occurs, a condition variable named "event: <name>" is automatically set. This makes it possible to create missions just to inform the player that an event has occurred:

```
mission "event: war begins"
    landing
    source
        government "Republic" "Syndicate" "Free Worlds"
    to offer
        has "event: war begins"
    on offer
        conversation
            ...
```

To alter hyperspace links, an event can `link` or `unlink` any two systems. An event can also set or clear the player's record of having visited a system (via the `visit <name>` and `unvisit <name>` keywords) or a single planet (`"visit planet" <name>` and `"unvisit planet" <name>`):

```
event "pug invasion"
	unlink "Zeta Aquilae" Ascella
	unlink Rasalhague Cebalrai
	unlink Rasalhague Ascella
	link Deneb "Zeta Aquilae"
	link Deneb Rasalhague
	unvisit "Zeta Aquilae"
	unvisit Rasalhague
	unvisit Orvala
	unvisit Deneb
	unvisit Cebalrai
	unvisit Ascella
	unvisit Peacock
```

In general, anything specified in an event will overwrite the previous value of that data element. (For example, specifying a new `government` replaces the previous one.)

For data elements that can take a list of an arbitrary number of values, if an event modifies that element, by default it replaces all of the previous values. To make it clearer whether that is your intention or not, **starting in version 0.9.7** you can use the "add" and "remove" keywords:

```
event "goodbye world"
    planet "Earth"
        remove attributes urban tourism
        add attributes "barren wasteland"
        remove shipyard
        remove outfitter
```

The following data elements work with the "add" and "remove" keywords:
* `planet`
  * `attributes`
  * `shipyard`
  * `outfitter`
* `system`
  * `link`
  * `asteroids`
  * `minables`
  * `fleet`
* `outfitter`
  * `<outfit name>`
* `shipyard`
  * `<ship name>`

In general, it's not possible to "remove" a specific value for elements whose values do not have names. For example, you can't remove a single stellar `object` from a `system` because there would be no way to specify which object to remove. However, you can use the "add" keyword in a fleet definition to add a variant without replacing all the existing variants:

```
event "new ship available"
    fleet "Small Southern Pirates"
        add variant 5
            "Doomship"
```

The "remove" keyword can also be used with any element of a `system` or `planet` to reset it to its default value, as shown in the Earth example above. To clear all the entries in a `shipyard` or `outfitter` you can just put the keyword "remove" alone on a line. (For backward compatibility, the word "clear" can also be used in a shipyard or outfitter definition, and has the same effect.)