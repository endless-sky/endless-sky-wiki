"Conditions" are named values that represent things the player has done. Conditions start out with a value of zero, and **can only have integer values**. Any fractional values will be rounded towards zero. They can have almost any name you want, and should be made sufficiently unique such that you can be sure that a reference to one is only for the purpose you have intended, and not an accidental overlap (such as with conditions used by a different plugin). 

## Condition Types

Conditions are used in two manners by the game - as testable prerequisites to an action (such as whether a mission can be offered), and as changesets to be applied (such as recording a player's mission dialog choices for use by future missions, or changing the player's reputation with a specific government).

<a name="testable">

### Testable Condition Sets
</a>

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

<a name="applied">

### Applied Condition Sets
</a>

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
* `"flagship crew"`, `"flagship required crew"`, and `"flagship bunks"` are the current crew, required crew, and bunks of your flagship only (ignoring any passengers you're carrying). **(v. 0.9.11)**
* `"net worth"` is your net worth, the sum of the worth of all your ships and outfits plus your current account balance minus and outstanding mortgages or fines.
* `"cargo attractiveness"` is how attractive the size of your cargo hold(s) is to pirates. Lots of small ships are more attractive than one large one. Values for single human ships range from `-2` for ships with no cargo to `8` for Bulk Freighters.
* `"armament deterrence"` is how effective your weapons are at discouraging pirates. Values for single human ships range from `0` for unarmed ships to `8` for the Dreadnought.
* `"pirate attraction"` is how attractive your fleet is to pirates, calculated as ("cargo attractiveness" - "armament deterrence"). A value of 3 results in raids 5% of the time, and a value of 10 results in raids 34% of the time.
* `"day"`, `"month"`, and `"year"` are the current date, given as individual variables so you can check for holidays, etc.
* `"random"` is a random number between 0 and 99. This can be used to make a mission only sometimes appear even when all other conditions are met.
* `"name: <first> <last>"` is the full name of the pilot (**v. 0.9.17**).
* `"first name: <first>"` is just the first name (**v. 0.9.17**).
* `"last name: <last>` is just the last name (**v. 0.9.17**).

<a name="expressions">

## Value Expressions
</a>

Beginning with **v0.9.11**, support was added for simple algebra in both types of conditions. For [testable](#testable) conditions, these "value expressions" can appear on either side of the comparison operator, while [applied](#applied) conditions can only use value expressions on the right-hand side of the mutation operator. (This is because an applied condition must store the condition value with a name, and the result of evaluating a value expression is an integer, not a name.)

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
