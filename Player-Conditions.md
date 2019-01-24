"Conditions" are named values that represent things the player has done. Conditions start out with a value of zero, and **can only have integer values**. Any fractional values will be rounded towards zero. They can have almost any name you want, and should be made sufficiently unique such that you can be sure that a reference to one is only for the purpose you have intended, and not an accidental overlap (such as with conditions used by a different plugin). 

## Condition Types

Conditions are used in two manners by the game - as testable prerequisites to an action (such as whether a mission can be offered), and as changesets to be applied (such as recording a player's mission dialog choices for use by future missions, or changing the player's reputation with a specific government).

### Testable Condition Sets

The basic syntax of a testable condition set, such as those used in a mission's `to offer` or a conversation's `branch`:

```html
<condition> <comp> <value>
(has | not) <condition>
never
(and | or)
    ...
```
The `<comp>` comparison operator can be `==`, `!=`, `<`, `>`, `<=`, or `>=`. As a special shortcut, you can write `has <condition>` instead of `<condition> != 0`, or `not <condition>` instead of `<condition> == 0`. The "never" condition always evaluates to false, so it can be used to create a mission that can never succeed. Using the `and` and `or` keywords allows specifying a nested set of testable conditions. 

A testable condition set is satisfied only if every condition listed is true. If instead you want it to succeed if any of the listed conditions are true, you can use an "or" sub-clause. Within an "or" clause you can have additional "and" clauses and so on, allowing you to check any arbitrary logical combination. For example, if you want a conversation label to be reached if "(has A or (has B and has C)) and (has D)" is true, you would set the branch conditions as such:

```html
branch "elaborate"
    or
        has "A"
        and
            has "B"
            has "C"
    has "D"
```

### Applied Condition Sets

The basic syntax of applied conditions, such as those used in a mission's `on offer` or a conversation's `apply`:

```html
<condition> <op> <value>
<condition> (++ | --)
(set | clear) <condition>
```
The `<op>` mathematical operator can be `=`, `+=`, `-=`, `<?=`, or `>?=`. The `<?=` (minimum) and `>?=` (maximum) operators allow setting `<condition>` to the minimum or maximum of the existing condition value and the given `<value>`. For example, `"reputation: Crime Lords" <?= -1000` would change a reputation of `-10` or `+20` to be `-1000`, but would not change a reputation of `-2000`.

As a special shortcut, "++" means "+= 1" and "--" means "-= 1". You must place a space between the condition, the operator, and the value in order for the parser to interpret it correctly. The "set" and "clear" tags are shortcuts for "= 1" and "= 0".


## Reserved Conditions

These condition names are created and used by the game for special purposes. You can manipulate some manually in missions or events, but in most cases the game will recompute the appropriate value.

### Modifiable
The game manages these conditions, but you are able to adjust the value in conversations, missions, and events, and the new value will be kept. For example, you could create a mission that modifies the player's reputation with a specific government.

* `"<mission name>: offered"`, where `<mission name>` is replaced with the name of any mission. This is incremented whenever a mission is offered to you, and is used by the "repeat" check to make sure a mission is not offered too many times.
* `"<mission name>: active"`, where `<mission name>` is replaced with the name of any mission. This is incremented when you accept a mission, and decremented when you complete, or fail it.
* `"<mission name>: done"` is set when a mission is successfully completed.
* `"<mission name>: declined"` is set when a mission is declined.
* `"<mission name>: failed"` is set when a mission is failed.
* `"reputation: <government>"` is set to your current reputation with the given government, rounded down to a whole number.
* `"combat rating"` is your current combat rating (based on the strength of all the ships your fleet has disabled).

### Read-only
No error will be raised if you modify these conditions, but the game will reset them back to the appropriate value.

* `"ships: <category>"` is the number of ships you have of each category (Transport, Light Freighter, Heavy Freighter, Interceptor, Light Warship, Heavy Warship, Fighter, Drone).
* `"cargo space"` and `"passenger space"` are your fleet's total cargo and passenger space (not reduced by the amount you are carrying already).
* `"net worth"` is your net worth, limited to the range of +- 2 billion.
* `"cargo attractiveness"` is how attractive the size of your cargo hold(s) is to pirates. Lots of small ships are more attractive than one large one. Values for single human ships range from `-2` for ships with no cargo to `8` for Bulk Freighters.
* `"armament deterrence"` is how effective your weapons are at discouraging pirates. Values for single human ships range from `0` for unarmed ships to `8` for the Dreadnought.
* `"pirate attraction"` is how attractive your fleet is to pirates, calculated as ("cargo attractiveness" - "armament deterrence"). A value of 3 results in raids 5% of the time, and a value of 10 results in raids 34% of the time.
* `"day"`, `"month"`, and `"year"` are the current date, given as individual variables so you can check for holidays, etc.
* `"random"` is a random number between 0 and 99. This can be used to make a mission only sometimes appear even when all other conditions are met.
