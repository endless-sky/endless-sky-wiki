"Conditions" are named values that represent things the player has done. Conditions start out with a value of zero, and **can only have integer values**. Any fractional values will be rounded towards zero. They can have almost any name you want, and should be made sufficiently unique such that you can be sure that a reference to one is only for the purpose you have intended, and not an accidental overlap (such as with conditions used by a different plugin). 

## Condition types

Conditions are used in two manners by the game - as testable prerequisites to an action (such as whether a mission can be offered), and as changesets to be applied (such as recording a player's mission dialog choices for use by future missions, or changing the player's reputation with a specific government).

### Testable condition sets

The basic syntax of a testable condition set, such as those used in a mission's `to offer` or a conversation's `branch`:

```html
<condition> <comp> <value# | value-expression#>
<value# | value-expression#> <comp> <value# | value-expression#>
(has | not) <condition>
never
(and | or)
	...
```
The `<comp>` comparison operator can be `==`, `!=`, `<`, `>`, `<=`, or `>=`. As a special shortcut, you can write `has <condition>` instead of `<condition> != 0`, or `not <condition>` instead of `<condition> == 0`. The "never" condition always evaluates to false, so it can be used to create a mission that can never succeed. Using the `and` and `or` keywords allows specifying a nested set of testable conditions. 

A testable condition set is satisfied only if every condition listed is true. If instead you want it to succeed if any of the listed conditions are true, you can use an "or" sub-clause. Within an "or" clause you can have additional "and" clauses and so on, allowing you to check any arbitrary logical combination. For example, if you want a conversation label to be reached if "(has A or (has B and has C)) and (has D)" is true, you would set the branch conditions as such:

```bash
branch "elaborate"
	or
		has "A"
		and
			has "B"
			has "C"
	has "D"
```

### Applied condition sets

The basic syntax of applied conditions, such as those used in a mission's `on offer` or a conversation's `apply`:

```html
<condition> <op> <value#>
<condition> <op> <value-expression#>
<condition> (++ | --)
(set | clear) <condition>
```
An applied condition results in the creation or modification of the condition `<condition>`, storing the value from the right-hand side (RHS).

The mutation operator `<op>` can be one of the following:
* **`=`**: The stored value is made equal to the RHS
* **`+=`**: The stored value is incremented by the RHS
* **`-=`**: The stored value is decremented by the RHS
* **`*=`**: The stored value is multiplied by the RHS
* **`/=`**: The stored value is the quotient from the division of the original value by the RHS
* **`<?=`**: The stored value is the lesser of itself and the RHS
* **`>?=`**: The stored value is the greater of itself and the RHS

The `<?=` (minimum) and `>?=` (maximum) operators allow setting `<condition>` to the minimum or maximum of the existing condition value and the given integer `<value>`. For example, `"reputation: Crime Lords" <?= -1000` would change a reputation of `-10` or `+20` to be `-1000`, but would not change a reputation of `-2000`.

As a special shortcut, "++" means "+= 1" and "--" means "-= 1". You must place a space between the condition, the operator, and the value in order for the parser to interpret it correctly. The "set" and "clear" tags are shortcuts for "= 1" and "= 0".

Note that the `/=` operator performs [**integer division**](https://mathworld.wolfram.com/IntegerDivision.html), not floating-point division.


## Reserved conditions (autoconditions)

These condition names are created and used by the game for special purposes. You can manipulate some manually in missions or events, but in most cases the game will recompute the appropriate value. As a point of terminology, these "reserved conditions" are what many people in the community call "autoconditions" or auto-conditions."

### Modifiable

The game manages these conditions, but you are able to adjust the value in conversations, missions, and events, and the new value will be kept. For example, you could create a mission that modifies the player's reputation with a specific government.

* `"<mission name>: offered"`, where `<mission name>` is replaced with the name of any mission. This is incremented whenever a mission is offered to you, and is used by the "repeat" check to make sure a mission is not offered too many times.
* `"<mission name>: active"`, where `<mission name>` is replaced with the name of any mission. This is incremented when you accept a mission, and decremented when you complete, or fail it.
* `"<mission name>: done"` is set when a mission is successfully completed.
* `"<mission name>: declined"` is set when a mission is declined.
* `"<mission name>: failed"` is set when a mission is failed.
* `"event: <event name>"`, where `<event name>` is replaced with the name of any event. This is set whenever an event is applied.
* `"reputation: <government>"` is set to your current reputation with the given government, rounded down to a whole number.
* `"combat rating"` is your current combat rating (based on the strength of all the ships your fleet has disabled).
* `"tribute: <planet>"` is the amount of tribute that you are receiving from the given planet.
* `"global: <condition>"` is any condition which gets set in the "global conditions.txt" file. Global conditions can be set and accessed by all player save files. **(v. 0.10.0)**

### Read-only

No error will be raised if you modify these conditions, but the game will reset them back to the appropriate value.

* `"ships: <category>"` is the number of ships you have of each category (Transport, Light Freighter, Heavy Freighter, Interceptor, Light Warship, Heavy Warship, Fighter, Drone) which are present and active. Present means the ship is in the same system as the player, or if the player is landed then it is on the same planet. Active means the ship is not parked or disabled.
* `"ships (all): <category>"` is the same as the condition above, except all your ships are checked, not only those which are present and active. **(v. 0.10.0)**
* `"total ships"` is the total number of ships which are present and active. **(v. 0.10.0)**
* `"total ships (all)"` is the total number of ships across your entire fleet. **(v. 0.10.0)**
* `"ship model: <model>"` is the total number of ships of a specific model that you own which are present and active. **(v. 0.10.0)**
* `"ship model (all): <model>"` is the total number of ships of a specific model that you own across your entire fleet. **(v. 0.10.0)**
* `"flagship model: <model>"` is the model of your current flagship. **(v. 0.9.15)**
* `"cargo space"` and `"passenger space"` are your fleet's total cargo and passenger space (not reduced by the amount you are carrying already).
* `"flagship crew"`, `"flagship required crew"`, and `"flagship bunks"` are the current crew, required crew, and bunks of your flagship only (ignoring any passengers you're carrying). **(v. 0.9.11)**
* `"flagship planet: <planet>"` is the planet your flagship is currently landed on.
* `"flagship system: <system>"` is the system your flagship is currently in.
* `"flagship landed"` is whether the flagship is currently on a planet. **(v. 0.10.0)**
* `"net worth"` is your net worth, the sum of the worth of all your ships and outfits plus your current account balance minus any outstanding mortgages, fines, salaries, or maintenance.
* `"credits"` is the number of credits you currently have in your account.
* `"unpaid mortgages"`, `"unpaid fines"`, `"unpaid salaries"`, and `"unpaid maintenance"` are the value of your current outstanding mortgages, fines, salaries, and maintenance.
* `"credit score"` is the credit score you have with the bank.
* `"cargo attractiveness"` is how attractive the size of your cargo hold(s) is to pirates. Values for single human ships range from `0` for ships with up to 20 tons of cargo to `8` for Bulk Freighters. This value is the sum of the attractiveness values of the player's unparked ships. The attractiveness value of each ship is: 0, or (0.4 * sqrt(cargo space)) - 1.8, whichever is larger.
* `"armament deterrence"` is how effective your weapons are at discouraging pirates. Values for single human ships range from `0` for unarmed ships to `8` for the Dreadnought.
* `"pirate attraction"` is how attractive your fleet is to pirates, calculated as ("cargo attractiveness" - "armament deterrence"). A value of 3 results in raids 5% of the time, and a value of 10 results in raids 34% of the time.
* `"day"`, `"month"`, and `"year"` are the current date, given as individual variables so you can check for holidays, etc.
* `random` is a random number between 0 and 99. This can be used to make a mission only sometimes appear even when all other conditions are met.
* `"roll: <input>"` will roll a random number from 0 up to, but not including the value of input (in the range `[0, input)`), where "input" can be either an integer or the name of a condition, in which case the value of the condition is used. If the input value is <= 1, then the output will always be 0. **(v. 0.10.3)**
* `"name: <first> <last>"` is the full name of the pilot. **(v. 0.10.0)**
* `"first name: <first>"` is just the first name. **(v. 0.10.0)**
* `"last name: <last>` is just the last name. **(v. 0.10.0)**
* `"visited planet: <planet>"` is 1 if you've visited the specified planet, 0 otherwise. **(v. 0.10.0)**
* `"visited system: <system>"` is 1 if you've visited the specified system, 0 otherwise. **(v. 0.10.0)**
* `"outfit: <outfit>"` is the number of outfits that you own of the given type which are local to the player. Local is defined differently depending on the player's location. **(v. 0.10.0)**
  * If the player is in orbit, "local" is any outfit installed on or in the cargo of in-system ships which are also in orbit (i.e. parked ships in-system don't count).
  * If the player is landed, "local" is any outfit installed on any landed ships (i.e. disabled ships in-system don't count), in the player's pooled cargo, or in storage on the planet the player is landed on.
  * In **v. 0.10.1**, changed to exclude parked ships as to match most other conditions.
* `"outfit (all): <outfit>"` is the number of outfits that you own of the given type anywhere in the game, include out of system ships, parked ships, and any planetary storage. **(v. 0.10.0)**
* `"outfit (installed): <outfit>"` is the number of outfits of the given type that you have installed and local. **(v. 0.10.0)**
  * In **v. 0.10.1**, changed to exclude parked ships as to match most other conditions.
* `"outfit (all installed): <outfit>"` is the number of outfits of the given type that you have installed anywhere. **(v. 0.10.0)**
* `"outfit (parked): <outfit>"` is the number of outfits of the given type that you have installed on parked ships local to you. If you are not landed on a planet, then this always returns a value of 0. **(v. 0.10.1)**
* `"outfit (flagship installed): <outfit>"` is the number of outfits of the given type that you have installed on your flagship. **(v. 0.10.0)**
* `"outfit (cargo): <outfit>"` is the number of outfits of the given type that you have in cargo and local. **(v. 0.10.0)**
* `"outfit (all cargo): <outfit>"` is the number of outfits of the given type that you have in cargo anywhere. **(v. 0.10.0)**
* `"outfit (flagship cargo): <outfit>"` is the number of outfits of the given type that you have in cargo on your flagship. When landed, this returns the cargo of all ships with you on the planet, as cargo becomes "pooled" into a singular location when you are landed and is only assigned to specific ships on take off. **(v. 0.10.0)**
* `"outfit (storage): <outfit>"` is the number of outfits of the given type that you have in storage and local. When landed, local is your current planet. When in orbit, local is any planet in your current system. **(v. 0.10.0)**
* `"outfit (all storage): <outfit>"` is the number of outfits of the given type that you have in storage anywhere. **(v. 0.10.0)**
* `"flagship attribute: <attribute>"` is the value of the given attribute on the player's flagship multiplied by 1000. The attribute is multiplied by 1000 because conditions must be integers, while attributes are decimal values, and multiplying by 1000 allows conditions to check for attributes as small as 0.001. The only exception to this is the "cost" attribute, which is already an integer. This includes the attributes from any installed outfits. **(v. 0.10.0)**
* `"flagship base attribute: <attribute>"` is the value of the given attribute on the player's flagship multiplied by 1000. This only checks the attributes on the ship itself, excluding any installed outfits. **(v. 0.10.0)**
* `"ship attribute: <attribute>"` has the same behavior as `"flagship attribute: <attribute>"`, except it looks at every ship in your fleet that is local to the player. Local is defined differently depending on the player's location. **(v. 0.10.13)**
  * If the player is in orbit, "local" ships are any ships in the same system as the flagship that are also in orbit (i.e. parked ships in-system don't count).
  * If the player is landed, "local" ships are those on the same planet as the player (i.e. disabled ships in-system don't count). Parked ships are excluded.
* `"ship base attribute: <attribute>"` has the same behavior as `"ship attribute: <attribute>"`, except it only checks the attributes on the ships themselves, excluding any installed outfits. **(v. 0.10.13)**
* `"ship attribute (all): <attribute>"` has the same behavior as `"ship attribute: <attribute>"`, except it checks every ship in your fleet regardless of location, including parked ships. **(v. 0.10.13)**
* `"ship base attribute (all): <attribute>"` has the same behavior as `"ship base attribute: <attribute>"`, except it checks every ship in your fleet regardless of location, including parked ships. **(v. 0.10.13)**
* `"flagship mass"` returns the current total mass of the player's flagship. This includes the mass of the ship itself, the mass of any ships and cargo currently being carried. **(v. 0.10.13)**
* `"flagship shields"` returns the absolute number of shield hit points the player's flagship currently has. **(v. 0.10.13)**
* `"flagship hull"` returns the absolute number of hull hit points the player's flagship currently has. **(v. 0.10.13)**
* `"flagship fuel"` returns the absolute amount of fuel the player's flagship currently has. **(v. 0.10.13)**
* `"flagship bays: <category>"` returns the number of bays for the given category of ship the player's flagship has.
* `"flagship bays free: <category>"` returns the number of bays on the player's flagship for the given category of ship that are not occupied. **Note:** this condition should only be used while the player is in-flight. Its behavior while the player is landed is considered unstable and may change in future versions. **(v. 0.10.13)**
* `"flagship bays"` returns the total number of bays on the player's flagship. **(v. 0.10.13)**
* `"flagship bays free"` returns the total number of bays on the player's flagship that are not currently occupied. **Note:** this condition should only be used while the player is in-flight. Its behavior while the player is landed is considered unstable and may change in future versions.  **(v. 0.10.13)**
* `"flagship planet attribute: <attribute>"` returns 1 if the planet that the player's flagship is landed on has the given attribute. Returns 0 if the flagship is not landed or the landed planet doesn't have the given attribute. **(v. 0.10.0)**
* `"flagship disabled"` returns 1 if the player's flagship is disabled. Returns 0 otherwise. **(v. 0.10.3)**
* `"days since year start"` is the number of days since the beginning of the current year. **(v. 0.10.0)**
* `"days until year end"` is the number of days until the end of the current year. **(v. 0.10.0)**
* `"days since epoch"` is the number of days since the "epoch" (1/1/1). **(v. 0.10.0)**
* `"days since start"` is the number of days since the day this pilot started on. If the current date is somehow before the date the pilot started on, this value will be negative. **(v. 0.10.0)**
* `"hyperjumps to planet: <name>"` and `"hyperjumps to system: <name>"` are the number of hyperdrive-capable jumps it would take to travel from your current location to the specified planet or system. Returns -1 for locations which don't exist or can't be reached. Does not make use of wormholes. **(v. 0.10.0)**
* `"previous system: <system>"` is 1 if the name provided was the system you were previously in before your current one, 0 otherwise. **(v. 0.10.0)**
* `"entered system by: (takeoff | hyperdrive | jump drive | wormhole)"` is the method by which you entered the system you're currently in. If you did not enter the current system by a given method, the condition will return 0. Returns 1 if the method was used. The "takeoff" method is true after taking off from a planet. The "hyperdrive" method is true if you entered using a hyperdrive. The "jump drive" method is true if you entered using a jump drive. The "wormhole" method is true if you entered via a wormhole. **(v. 0.10.0)**
* `"raid chance in system: <system>"` is the percent chance that a raid fleet will spawn in the given system given your fleet's current pirate attraction value, multiplied by 1000 in order to put it into a usable integer range. If a system contains no raid fleets, or the player's attraction is too low to spawn a raid fleet, then the result will be 0. For more information on how raid fleets behavior, see the [Creating Governments](https://github.com/endless-sky/endless-sky/wiki/CreatingGovernments#raid) page. **(v. 0.10.0)**
* `"installed plugin: <plugin>"` will be equal to 1 if a plugin with the given name is currently loaded, or 0 if no such plugin is loaded. **(v. 0.10.3)**
* `"person destroyed: <name>"` will be equal to 1 if the person ship of the given name has been destroyed, or 0 if it is still alive. **(v. 0.10.3)**
* `"landing access: <planet name>"` will be equal to 1 if your flagship has the ability to land on the given planet, 0 otherwise. **(v. 0.10.7)**

## Value expressions

Beginning with **v0.9.11**, support was added for simple algebra in both types of conditions. For [testable](#testable-condition-sets) conditions, these "value expressions" can appear on either side of the comparison operator, while [applied](#applied-condition-sets) conditions can only use value expressions on the right-hand side of the mutation operator. (This is because an applied condition must store the condition value with a name, and the result of evaluating a value expression is an integer, not a name.)

A "value expression" is a combination of the basic algebraic operators (`+`, `-`, `*`, `/`, [`%`](https://reference.wolfram.com/language/ref/Mod.html)) and "tokens" which yields a single result when evaluated. Tokens can be integer constants, other conditions, or even additional value expressions. Parentheses may be used to control the order of mathematical operations. In all cases, tokens, operators, and parentheses must be separated by spaces for proper parsing.

Simply put, a value expression matches the syntax
```html
<token> <op> <token>
<token> <op> ( <value-expression#> )
( <value-expression#> ) <op> ( <value-expression#> )
```
and allows one to easily use other condition values when testing or applying conditions.

### Examples

```bash
# runtime addition, evaluates to 12
8 + 4

# runtime integer division, evaluates to 2
8 / 3

# runtime integer division AND conversion from decimal to integer.
# evaluates to 4 (2.9 -> 2, then 8 / 2)
8 / 2.9

# at runtime, evaluates to "10 * <some integer> - 10" based on the player's
# current reputation with the Republic and Syndicate governments
# (Note that you MUST place spaces on both sides of the opening and closing parentheses.)
10 * ( "reputation: Republic" + "reputation: Syndicate" ) - 10

# runtime "remainder" computation, each evaluates to 1
1 % 2
3 % 2
5 % 2
101 % 2
4 % 3
400 % 3
# this evaluates to 4
400 % 11
```
