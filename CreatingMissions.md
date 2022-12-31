# Table of Contents

* [Introduction](#intro)
* [Text replacements](#replacement)
* [Basic mission characteristics](#basics)
* [Conditions](#conditions)
* [Source and destination filters](#filters)
* [Non-Player Characters (NPCs)](#npcs)
* [Triggers](#triggers)

<a name="intro">

# Introduction
</a>

Missions are a key part of Endless Sky. They are used to advance the plot, flesh out the universe, and offer role-playing opportunities for the player. The simplest possible mission is as follows:

```html
mission "Name"
    job
```

This mission will appear in the job board on every planet. If accepted, it can be completed by landing at that same planet again. To make it show up on the job board again even after it's been completed, add the `repeat` tag:

```html
mission "Name2"
    job
    repeat
```

To make it more interesting, add a `description` that will appear on the job board, and a `dialog` confirmation that the mission was completed:

```html
mission "Name2"
    description "Take off and land again! It's fun!"
    job
    repeat
    on complete
        dialog "You did it! You're amazing."
```

Now make it ask the player to land on Earth using a `destination` tag, and pay them once they get there:

```html
mission "Name2"
    description "Go to earth."
    destination Earth
    job
    repeat
    on complete
        dialog "You did it! You're amazing."
        payment 5000
```

Now add some cargo, and mention it in the description using text replacement tokens:

```html
mission "Name2"
    description "Bring <cargo> stuff to earth."
    destination Earth
    cargo "pickled herring" 5
    job
    repeat
    on complete
        dialog "You did it! You're amazing."
        payment 5000
```

And so on. The sky's the limit. Missions can become very complex. Here is a full list of everything that can go into a mission:

```html
mission <name>
    name <name>
    description <text>
    blocked <message>
    deadline [<days> [<multiplier>]]
    cargo (random | <name>) <number> [<number> [<probability>]]
        illegal <fine> [<message>]
        stealth
    passengers <number> [<number> [<probability>]]
    illegal <fine> [<message>]
    stealth
    invisible
    (priority | minor)
    (job | landing | assisting | boarding | shipyard | outfitter)
    repeat [<number>]
    clearance [<message>]
        ...
    infiltrating
    waypoint <system>
    stopover [<planet>]
        ...
    substitutions
        <text> <replacement>
            [<condition set>]
        ...
    to (offer | complete | fail | accept)
        <condition> <comp> <value>
        (has | not) <condition>
        never
        (and | or)
            ...
    (source | destination) <planet>
    (source | destination)
        [(not | neighbor)] planet <name>...
            <name>...
        [(not | neighbor)] system <name>...
            <name>...
        [(not | neighbor)] government <name>...
            <name>...
        [(not | neighbor)] attributes <name>...
            <name>...
        [(not | neighbor)] outfits <name>...
            <name>...
        [(not | neighbor)] category <name>...
        [(not | neighbor)] near <system> [[<min>] <max>]
        [(not | neighbor)] distance [<min>] <max>
        neighbor
            ...
        not
            ...
    npc (save | kill | board | assist | disable | "scan cargo" | "scan outfits" | evade | accompany | capture | provoke)...
        to (spawn | despawn)
            <condition> <comp> <value>
            (has | not) <condition>
            (and | or)
                ...
        government <name>
        personality <type>...
            <type>...
            confusion <amount>
        system <system>
        system
            system <name>...
                <name>...
            government <name>...
                <name>...
            near <system> [[<min>] <max>]
            distance [<min>] <max>
        dialog <text>
            <text>...
        conversation <name>
        conversation
            ...
        ship <model> <name>
        fleet <name> [<count>]
        fleet [<count>]
            ...
    on (offer | complete | accept | decline | defer | fail | abort | visit | stopover | waypoint | enter [<system>] | daily)
        log [<category> <header>] <text>
        dialog <text>
            <text>...
        dialog phrase <phrase>
        conversation <name>
        conversation
            ...
        outfit <outfit> [<number>]
        require <outfit> [<number>]
        give ship <model> [<name>]
        payment [<base> [<multiplier>]]
        fine <amount>
        <condition> (= | += | -=) <value>
        <condition> (++ | --)
        (set | clear) <condition>
        event <name> [<delay> [<max>]]
        fail [<name>]
```

Each of these parts of the mission description is described in detail below.

<a name="replacement">

# Text replacements
</a>

Certain characteristics of a mission, such as the cargo or the destination planet, may be chosen at random. In order to refer to those randomly chosen elements in descriptive text, you can use the following placeholders:

* `<commodity>` = name of commodity being carried
* `<tons>` = "1 ton" or "N tons," where N is the amount of cargo
* `<cargo>` = "`<tons>` of `<commodity>`"
* `<bunks>` = the number of passengers
* `<passengers>` = "passenger" or "passengers"
* `<fare>` = "a passenger" or "N passengers," where N is the number of passengers
* `<origin>` = planet (or ship) where mission was offered
* `<planet>` = destination planet
* `<system>` = destination system
* `<destination>` = "`<planet>` in the `<system>` system"
* `<stopovers>` = a list of all stopover destinations
* `<waypoints>` = a list of all waypoint systems
* `<payment>` = "1 credit" or "N credits," where N is the payment amount from the `on complete` block, unless the replacement is in an `on *` block or below a conversation `apply` node that has its own payment, in which case that `on *` block's or the most recent `apply`'s payment is used
* `<fine>` = "1 credit" or "N credits," where N is the fine amount with the same behavior as `<payment>`; this is not the fine as defined by an `illegal` line
* `<date>` = the deadline for the mission (in the format "Day, DD Mon YYYY")
* `<day>` = the deadline in conversational form ("the DDth of Month")
* `<npc>` = the name of the first ship in the last `npc` block in the mission description
* `<first>` = your first name
* `<last>` = your last name
* `<ship>` = the name of your flagship

These placeholders will be substituted in any text in the following places:

* the mission name
* the mission description
* dialog messages contained in the mission
* conversations contained in the mission

For example, the mission description might be, "Deliver `<cargo>` to `<destination>` by `<date>`."

Missions can also make use of custom text replacements through use of the `substitutions` node. All text replacements aside from `<first>`, `<last>`, and `<ship>` are made when a mission is instantiated, i.e. before the mission is offered. The three exceptions are made on the fly given that the player's name can be changed during a conversation and your flagship can change between mission instantiation and when you reach a conversation or dialog.

<a name="basics">

# Basic mission characteristics
</a>

```html
mission <name>
```

The mission name must be unique. Missions are stored by the game in alphabetical order (more specifically, ASCII lexical ordering), meaning that missions will also offer in alphabetical order if multiple are able to be offered at the same time. If multiple possible missions are minor, these get evaluated in alphabetical order such that the last mission alphabetically is the one that gets offered while the rest are discarded. Therefore, if you have multiple missions with similar names and the same offer conditions, whichever one you want to offer first should come first alphabetically, unless the missions are minor, in which case the one you want to offer should come last alphabetically.

An example of using the mission name to force a certain load order in game is with the set of missions prefixed "FW Pug 2C", where the jump drive variant of the mission is named "FW Pug 2C: **A** Jump Drive" to force it to offer before "FW Pug 2C: **H**yperdrive", as otherwise without the "A" the hyperdrive variant of the mission would have priority.

```html
name <name>
```

Because the mission name must be unique, if you want to have the same name displayed for two different missions, you can specify the display name separately. This is useful, for example, if you want to have multiple jobs that all get displayed as "ferry passengers to `<planet>`".

```html
description <text>
```

This is a short description of the mission, with enough detail to make it clear to the player what they need to do to complete the mission, and what could cause the mission to fail.

```html
blocked <message>
```

This is a short message that is displayed to the player if this mission's `to offer` conditions pass, but either the `to accept` conditions do not, the `on offer` or `on accept` actions can't be done (which may be the result of `require`, `outfit <outfit> -1`, or some other action), or the player does not have enough cargo space or bunks available. (This does not count cargo space occupied by ordinary commodities, or bunks occupied by crew, because you will automatically sell / fire them if a special mission is offered.) The message uses all the standard text substitutions given above, as well as `<capacity>`, which is a string describing how much additional capacity you need (e.g. "another bunk and 14 more tons of cargo space"), as well as `<conditions>`, which evaluates to "do not meet" if there is a `to accept` that you fail, or "meet" if you pass the `to accept`.

Prior to **v. 0.10.0**, only the lack of space for the mission would trigger its blocked dialog.

```html
deadline [<days#> [<multiplier#>]]
```

The number of days you have to complete the mission. If the number of days is left out (i.e. the line is just the word `deadline`), a default deadline is calculated. If you specify a "multiplier", the number of hyperspace jumps between the mission source and the mission destination is used to pick a sane deadline. If the mission has waypoints or stopovers then the number of jumps needed to visit all waypoints and stopovers before reaching the destination is used. The default multiplier value is `2`, and the default days value is `0`. The mission's deadline is then calculated using the formula:

`days + multiplier * (number of hyperspace jumps to the destination)`

You can also combine multiple deadline statements; for example to set the deadline to the default with 2 additional days, one can write:

```c++
deadline
deadline 2
```

*Note: any missions requiring non-hyperspace travel (i.e. via wormhole or jump drive) are classified as having 0 hyperspace jumps* in the above formula.

```html
cargo (random | <name>) <number#> [<number#> [<probability#>]]
    illegal <fine#> [<message>]
    stealth
```

This specifies the cargo that you are carrying. If `<name>` is one of the standard commodity names defined in the "trade" data, it will be replaced by a random one of the specific names for that commodity, e.g. "Food" might be replaced by "canned fruit" or "evaporated milk".

If the given cargo name is the word `random`, a basic commodity type will be chosen based on the relative commodity prices at the source and destination system. That is, it will attempt to pick a commodity that would make sense as an export from the one system to the other.

If two amounts are given instead of one, that means that a random amount should be chosen in between those two numbers (inclusive).

If three numbers are given, a random number will be chosen by adding the first number to a random number chosen from a negative binomial distribution with the given number of successes needed and probability. This produces numbers that are generally somewhat low but can occasionally be quite high, if you want to every once in a while have massive cargo missions, for example. You can experiment with values using a tool like [this one](https://homepage.divms.uiowa.edu/~mbognar/applets/nb1.html) and supplying values for `r` (the second value) and `p` (the third value). Smaller values of probability result in larger average cargo sizes.

**Prior to v. 0.9.9**, the `illegal` and `stealth` attributes apply only to cargo and should be children of the `cargo` definition. Starting in **v. 0.9.9**, they are attributes of the mission itself and apply both to cargo and to passengers:

```html
illegal <fine#> [<message>]
```

If the mission is marked as `illegal`, governments that care about such things will levy the given fine against you if you are caught carrying its cargo (or, **starting in v. 0.9.9**, its passengers as well). If the fine is negative, being caught with this cargo while in flight is counted as an "atrocity" (the government that catches you immediately becomes your enemy, no matter how good your reputation was previously), and if you are caught in a spaceport with the cargo the result is a death sentence (game over).

If the fine amount in the `illegal` line is followed by another token, that token is displayed as a message when you are caught with this cargo.

```html
stealth
```

If the mission is marked as `stealth`, it will fail if you are caught with the mission cargo (or, **starting in v. 0.9.9**, with the passengers as well).

```html
passengers <number#> [<number#> [<probability#>]]
```

This specifies the number of passengers. As with the `cargo` specification, if there are two or three numbers they are used to pick a random number.

```html
invisible
```

This specifies that the mission does not show up in the player's list of missions, and cannot be aborted. Invisible missions also won't show map markers of any kind, such as the mission's destination or any stopovers/waypoints, nor will a message be displayed if the mission fails.

```html
(priority | minor)
```

If a mission is marked with `priority`, only other "priority" missions can be offered alongside it.

If a mission is marked with `minor`, it will be offered only if no other missions are being offered at the same time.  
In general, any mission that starts a completely new mission string, and that could instead be offered at a later date, should be marked "minor." Missions continuing a string should not be marked "minor."

Note that `priority` will only affect missions that offer from the spaceport.

```html
(job | landing | assisting | boarding | shipyard | outfitter)
```

This specifies where this mission will be shown, if someplace other than the spaceport. If it is a job, it will only appear on the job board (and only if the current planet matches the [source filter](https://github.com/endless-sky/endless-sky/wiki/CreatingMissions#filters)).

If this mission is to be shown at `landing`, it shows up as soon as you land instead of waiting for you to visit the spaceport. This can be used, for example, to show a special conversation the first time you land on a particular planet or on any planet belonging to a certain species. It can also be used for a continuation of an active mission.

A mission shown when `assisting` or `boarding` will be shown when you repair a friendly ship or plunder a hostile ship, respectively. **These missions are never shown when boarding a ship that you have boarded before, that belongs to you, or that is an NPC in an active mission.** In either case, if the `on offer` conversation results in a conversation exit code of `launch` or `flee`, the ship in question will be destroyed.

```html
repeat [<times#>]
```

If the word `repeat` appears by itself, this mission can be offered any number of times. If a number is given, that is the maximum number of times this mission can be offered. By default, each mission can only be offered once, so specifying `repeat 1` is unnecessary.

If you want a mission to be offered any number of times but to limit the number of instances of the mission that can be active concurrently, you can decrement the `"<mission name>: offered"` condition (described [below](#conditions)) whenever the mission is completed, failed, or declined.

```html
clearance [<message>]
    [{filter specification...}]
```

This gives you landing clearance on the destination planet, even if normally you would not be allowed to land there, or would have to pay a bribe.

If a `clearance` tag appears followed by a message, if you hail your destination planet that message will be shown as the initial text, landing permission will be granted, and you will not have to pay a bribe. The message will be shown even for friendly planets so that this can be used to customize their hail, e.g. "Welcome back, Captain `<last>`! Everyone's waiting for you at the spaceport."

If a `clearance` tag appears with no message, clearance is automatically granted without needing to hail the planet. You can use this, for example, if the mission involves secretly landing on a planet under the cover of darkness and the last thing you want to do is inform the authorities that you're coming.

If a specific destination is given (i.e. `destination <planet>`) and no clearance is included in the mission, you must pay a bribe if you are not allowed to land on that planet. (Sometimes bribing the authorities could be part of the mission plot.)

If the destination is specified via a filter, the filter will not match planets you cannot land on unless this mission contains a `clearance` tag. *Omitting the tag may make it impossible for a particular mission to be offered.* Beginning with **v 0.9.13**, stopover or destination filters that explicitly list planets for which the player needs clearance as an option in the filter's `planet` list will now match these previously forbidden destinations.

The `clearance` tag may have child entries that specify a [location filter](#filters), the same as the `source` and `destination` tags described below. In this case, you have clearance on all planets that match that filter, in addition to on the destination planet.

```html
infiltrating
```

This indicates that you do not have access to any of the services on the destination planet, including refueling or repairing; all you can do is complete your mission and then leave. This is useful for missions that involve you landing secretly somewhere other than the spaceport; it wouldn't make sense to then allow the player to then walk around the spaceport or purchase things.

```html
waypoint <system>
waypoint
    {filter specification...}
```

This specifies a system which you must fly through in order to complete the mission. You do not have to land on any planets or spend any amount of time there. Waypoints are marked on the map in red until they have been visited. Beginning with **v. 0.9.9**, all visited waypoints of the currently selected mission are marked by a faint circle in the player's map panel.

```html
stopover [<planet>]
stopover
    {filter specification...}
```

This specifies a planet that you must visit in order to complete the mission. The planet can either be named explicitly, or selected using a "filter" in the same format as the [`source` and `destination` filters](#filters). As with waypoints, any number of stopovers may be specified. After completing a stopover, its system will be marked with a faint circle for the remainder of the mission (**v 0.9.9+**). Beginning with **v 0.9.13**, planets selected by a filter to be a stopover are no longer required to have a spaceport (unlike a mission's destination). If a mission's randomly picked stopover planet(s) should have a spaceport, this can be achieved by adding an `attributes "spaceport"` line to the filter specification.

```html
substitutions
    <text> <replacement>
        [<condition set>]
    ...
```

Beginning with **v.0.9.15**, this specifies custom text replacements that apply only to the text of the mission they're defined within. Substitutions defined within a mission take precedence over global substitutions and are overtaken in precedence by [hardcoded text replacements](#text-replacements). For more information on custom text replacements, see the [creating substitutions](CreatingSubstitutions) page.

<a name="conditions">

# Conditions
</a>

["Conditions"](Player-Conditions) are named values that represent things the player has done. Conditions start out with a value of zero, and can only have integer values. Conditions can have almost any name you want, as long as you make sure not to use the same name in two places. A few names are reserved for special purposes and may be read-only. A list of these reserved conditions can be found [here](Player-Conditions#reserved-conditions).

Conditions are checked at several times when processing a mission: when determining whether the mission can be offered right now (in the `to offer` tag), and when determining whether it has been completed (in the `to complete` tag) or failed (in the `to fail` tag), and, beginning in **v. 0.10.0**, determining whether it can be accepted (in the `to accept` tag):

```html
to (offer | complete | fail | accept)
    <condition> <comp> <value>
    (has | not) <condition>
    never
    (and | or)
        ...
```

The `<comp>` comparison operator can be `==`, `!=`, `<`, `>`, `<=`, or `>=`. As a special shortcut, you can write `has <condition>` instead of `<condition> != 0`, or `not <condition>` instead of `<condition> == 0`. The `never` condition always evaluates to false, so it can be used to create a mission that can never succeed.

Conditions can be changed when you are offered a mission or when you accept, decline, fail, or complete it, as described later in this document. You can "chain" missions together by having one depend on the `"<mission name>: done"` condition that is automatically set by a previous mission upon completion. Since all your existing missions complete before new ones are offered, that condition will be set before the check is done for what new missions are available.

A mission will not be offered if any of the `to fail` conditions are met, and will fail if it is active and one of those conditions changes so that it is satisfied. Using the `random` keyword in a `to fail` tag is possible, but not recommended.

Beginning in **v. 0.10.0**, mission can be given `to accept` conditions. For jobs, if the mission can offer then it will appear on the jobs board, but if it can't be accepted then it will appeared grayed out with the "accept mission" button unable to be clicked. For non-job missions, if the mission's `to offer` conditions passed but the `to accept` conditions do not, then the `blocked` dialog will appear, if it exists.

The condition set is satisfied only if every condition listed is true. If instead you want it to succeed if any of the listed conditions are true (e.g. you have completed mission A *or* mission B), you can use an `or` sub-clause. Within an `or` clause you can have `and` clauses (and so on), allowing you to check any arbitrary logical combination. For example, if you want a mission to be offered if "(has A or (has B and has C)) and (has D)":

```html
to offer
    or
        has A
        and
            has B
            has C
    has D
```

<a name="filters">

# Mission "Location" filters
</a>

A mission can be offered from a planet or ship, but all missions must have a destination planet. For missions offered on a planet, if no destination is given that same planet is used. This can be useful for missions that should end on the same planet they start on, such as "Kill pirate ship 'X' and return here for payment."

For missions offered by a ship, you must always specify a destination, even if the mission is designed to always be "declined" - e.g. a message where the pilot thanks you for repairing them but asks for no further help.

If no source is specified, the mission will be offered whenever its `to offer` conditions are satisfied; this can be used to create a mission that is offered as soon as you complete another.

For the source and destination, you can either specify one particular planet, or give a set of constraints that the planet must match. These sets of constraints are referred to as a "location" filter, as they are applied to the game's ships, systems, and planets in order to conditionally select locations for mission events.

It is possible to test filters by passing `--matches` to the game and then writing a location filter under a `location` node. The output are systems and planets matching the filter.

```html
(source | destination) <planet>
(source | destination)
    [<modifier>] planet <name>...
        <name>...
    [<modifier>] system <name>...
        <name>...
    [<modifier>] government <name>...
        <name>...
    [<modifier>] attributes <name>...
        <name>...
    [<modifier>] outfits <name>...
        <name>...
    [<modifier>] category <name>...
    [<modifier>] near <system> [[<min>] <max>]
    [<modifier>] distance [<min>] <max>
    neighbor
        ...
    not
        ...    
```

Each entry in the source or destination specification acts as a filter. Two "modifier" tokens were introduced in **v. 0.9.9**, **`not`** and **`neighbor`**. The `neighbor` modifier indicates that the associated filter must match a system that is hyperlinked with the system in question, and the `not` modifier indicates that the associated filter must not match the system in question. These modifiers cannot be used in the same line, but can be "children" of each other.

```html
[(not | neighbor)] planet <name>...
    <name>...
```

This says that the planet must be (or must not be, if the `not` keyword is used) one of the named planets. If `neighbor` is used, at least one of the named planets must be in a hyperlinked system. The list of names can either be all on one line, or split between multiple lines if it is particularly long; the subsequent lines must be indented so that they are "children" of the `planet` node. As with most of these filters, you can also have more than one `planet` entry, in which case the planet chosen must be in any one of the lists. Beginning with **v 0.9.13**, planets specified in this list can be selected for use as a mission destination or stopover even if the player cannot land on the planet at the time the mission is offered (e.g. due to lack of `clearance` or due to hostility with the planet's government), or if the planet has no spaceport.

```html
[(not | neighbor)] system <name>...
    <name>...
```

The system must be (or must not be, or must neighbor) one of the items in this list. You can use this if you do not want to bother to look up what planets are in the system, but its intended use is for the NPC location filter as described later.

```html
[(not | neighbor)] government <name>...
    <name>...
```

The planet must be in (or must not be in, or must neighbor) a system owned by the given government(s). Again, the list can be all on one line, or multiple indented lines.

If this is a source filter and the mission is being offered when `assisting` or `boarding` a ship, the government in this filter refers to the ship's government, not the government of the current star system. This allows you, for example, to create a mission that is only offered by merchant ships. If the `neighbor` modifier is used, at least one neighboring system's government must be in the list of named governments.

```html
[(not | neighbor)] attributes <name>...
    <name>...
```

The system or planet must have (or must not have, or must link to a system with) one of the given attributes (e.g. "dirt belt", "urban", "rich", "tourism", etc.). If applied to a system (**v. 0.9.9+**), at least one of the given attributes must be found in either the system's own attributes, or the attributes of any of its orbiting objects. If applied to a ship (**v. 0.9.9+**), the attribute must be positive, after taking into account any adjustments that are made by all of its installed outfits.

Unlike the other filters, if multiple `attributes` tags appear, the system or planet must contain at least one attribute from each of the lists. For example, this means the planet must be urban or rich:

```bash
attributes "urban" "rich"
```

but this means it must be urban _and_ rich:

```bash
attributes "urban"
attributes "rich"
```

This matches anything that is not urban or not rich:

```bash
not attributes "urban"
not attributes "rich"
```

whereas this matches anything that is not _both_ urban _and_ rich:

```bash
not
    attributes "urban"
    attributes "rich"
```

Beginning with **v. 0.9.9**, similar to `attributes`, ships (`source`) and planets (`source`, `destination`, `stopover`) can be matched according to available outfits. For ships, these outfits may be installed or in cargo, while for planets they must be for sale. This would match a ship that has at least one "Beam Laser" or "Meteor Missile Launcher" installed or in its cargo, or a planet that sells either one of the outfits:

```c++
source
    outfits "Beam Laser" "Meteor Missile Launcher"
```

whereas this would match ships or planets that only have both:

```c++
source
    outfits "Beam Laser"
    outfits "Meteor Missile Launcher"
```

**V. 0.9.9+** also allows matching ships based on the specified category, e.g. "Light Warship" or "Interceptor". Any filter that defines a ship category will not match to systems or planets. Since a ship can have only one category, the following are both equivalent filters:

```c++
source
    category "Heavy Freighter" "Light Warship"

source
    category "Heavy Freighter"
    category "Light Warship"
```

There are also ways of specifying how far the system is from a particular location, or from the current location:

```html
[(not|neighbor)] near <system> [[<min#>] <max#>]
```

If one number is given, the planet must be within (or must not be within, or must link to a system) that number of jumps from the given system (which includes the given system itself). If two numbers are given, the distance from the given system must be at least as high as the first number, and no more than the second. If no numbers are given, the planet must be in the given system or one of the systems it is linked to; this is equivalent to giving distances of 0 and 1.

```html
[(not|neighbor)] distance [<min#>] <max#>
```

This is the same as the `near` tag, but gives distances relative to the origin planet. (So, this tag only makes sense within a `destination` filter, not within a `source` filter or a `clearance` filter.) Unlike `near`, `distance` must be provided a maximum value.

```html
not
    ...
neighbor
    ...
```

A `not` or `neighbor` tag by itself, with filters as its children, defines a more complicated filter that must not match. For example, this filter matches a system whose neighbor must neighbor a Republic system and not be a Republic system:

```bash
neighbor
    neighbor government "Republic"
    not government "Republic"
```

while this filter additionally requires the matched system to belong to a government other than Republic:

```bash
not government "Republic"
neighbor
    neighbor government "Republic"
    not government "Republic"
```

Filters combining `not` and `neighbor` can become quite complex and difficult to reason, but offer functionality not otherwise available. For example, to find a source system that is not Republic (1), does not neighbor a Republic system (2), but has a neighbor that does neighbor a Republic system (3), you could use this filter:

```java
source
1)  not government "Republic"
2)  not
        neighbor government "Republic"
3)  neighbor
        neighbor government "Republic"
```

<a name="npcs">

# Non-Player Characters (NPCs)
</a>

NPCs are ships that are associated with the mission in some way. This includes friendly ships the player must protect, and hostile ships the player must fight off or destroy:

```html
npc (save | kill | board | assist | disable | "scan cargo" | "scan outfits" | evade | accompany | capture | provoke)...
    to (spawn | despawn)
        <condition> <comp> <value>
        (has | not) <condition>
        (and | or)
            ...
    government <name>
    personality <type>...
        <type>...
        confusion <amount#>
    system (<system> | destination)
    system
        system <name>...
            <name>...
        government <name>...
            <name>...
        near <system> [[<min#>] <max#>]
        distance [<min#>] <max#>
    planet <name>
    dialog <text>
        <text>...
    conversation <name>
    conversation
        ...
    ship <model> <name>
    fleet <name> [<count#>]
    fleet [<count#>]
        ...
```

Each `npc` tag may have one or more tags following it, specifying what the player must do with the given NPC:

* `save`: The mission fails if the given NPC is destroyed.
* `kill`: To complete the mission, the given NPC must be dead.
* `board`: To complete the mission, the player must board the given NPC. If the ship is destroyed before being boarded, the mission fails. (Note that if the ship is friendly, you will "assist" it instead of boarding it, and this condition will not be satisfied.)
* `assist`: To complete the mission, the player must assist the given NPC (i.e. board it while it is disabled and the player is friendly with it, thus repairing it).
* `disable`: To complete the mission, the player must disable the given NPC.
* `"scan cargo"`: To complete the mission, the player must scan the given NPC's cargo. If the NPC is destroyed before being scanned, the mission fails.
* `"scan outfits"`: Same, but the player must use an outfit scanner instead of a cargo scanner.
* `evade`: You cannot complete the mission if any members of this NPC are in the same system as you.
* `accompany`: You can only complete the mission if all members of this NPC are in the same system as you. Prior to **v. 0.10.0**, the `accompany` tag also implicitly had the behavior of the `save` tag. Now, ships that have been destroyed or captured don't count toward the accompany objective, and the `save` tag must be given to npc ships that you want to both be alive and with the player.
* `capture`: To complete the mission, the player must capture the given NPC. Capturing an NPC also counts as destroying it for the purposes of the mission, so this objective can't be combined with an objective like accompany or save.
* `provoke`: To complete the mission, the player must provoke the given NPC. Provocation occurs when an NPC is friendly and is made hostile by the player attacking it.

```html
to (spawn | despawn)
    <condition> <comp> <value>
    (has | not) <condition>
    (and | or)
        ...
```

**Starting in v.0.9.13,** `to (spawn | despawn)` works similarly to `to (offer | complete | fail | accept)` for missions, containing a condition set that must be met for something to occur.

An NPC will not spawn if its `to spawn` conditions are not met, and any spawned NPC will despawn if its `to despawn` conditions are met. NPCs will only spawn once the player departs from a planet and despawn once the player lands, but these conditions are evaluated at multiple points: after accepting the mission, on each departure, on each system jump, and on each landing.

Should an NPC have a `to (spawn | despawn)` as well as an objective (e.g. `save`), then the objective of the NPC will be ignored if the NPC has not yet spawned or has been despawned. This means that you can potentially create secondary or alternative objectives for missions (e.g. you must either complete this NPC objective, or go to this planet to despawn the NPCs instead, and in the reverse, you must go to this planet, or go to some other planet to spawn NPCs with a new objective).

When combined with an `apply` node in a [`conversation`](https://github.com/endless-sky/endless-sky/wiki/WritingConversations), this can allow the choices a player makes in a conversation to alter whether NPCs spawn after the mission is accepted.

```html
government <name>
```

This specifies what government all the ships connected to this NPC specification will have. If no government is given, they are set to the player's government, as escorts.

```html
personality <type>...
    <type>...
    confusion <amount#>
```

This defines the NPC's [personality](https://github.com/endless-sky/endless-sky/wiki/ShipPersonalities). The `confusion` tag is a special value, giving the inaccuracy in pixels of the ship's targeting systems; the default value is 10 pixels.

If an NPC is specified as starting out in your current system and its personality is *not* `staying` or `waiting`, it will take off from the planet along with you (e.g. a ship you are escorting). A ship that is `entering` the current system might, for example, be a pirate raid chasing the fleet you are escorting, and a ship `staying` in a certain system might be a target you must locate for a "bounty hunting" mission. (Any ship that is not `staying` will actively seek the player out if it is in a different system, unless it is also `uninterested`.)

```html
system (<system> | destination)
```

This specifies the exact system the NPC will start in: either the named system, or the mission's destination system if the given name is literally the word `destination`. (The `destination` keyword is only supported in **0.9.1 and up.**) Note that if no system is specified, either in this way or using a filter (below), the NPC will start in the current system.

```html
system
    system <name>...
        <name>...
    government <name>...
        <name>...
    near <system> [[<min#>] <max#>]
    distance [<min#>] <max#>
```

This specifies a location filter for choosing what system the NPC starts out in. The `system`, `government`, `near`, and `distance` filters operate the same way they do in the descriptions in the previous section, and can be used instead of naming a particular system. For example, you could have the NPC start out in any "Pirate" system, or within two jumps of the current system. The other location filter options are also available, including `not` and `neighbor`.

```html
planet <name>
```
This specifies the exact name of the starting planet for all ships in the NPC definition. A specified starting planet allows the NPCs to depart from a planet other than that which the player is landed on. If the NPCs do not start in the system in which the named planet is located, or the NPCs have an "entering" personality, this value is ignored.

```html
dialog <text>
    <text>...
conversation <name>
conversation
    ...
```

This defines a dialog or conversation to be shown when you have first satisfied all the requirements of a given NPC. For more details on the syntax, see the "Triggers" section below. Beginning with **v. 0.9.9**, conversations shown when completing an NPC can also use special keywords to influence the player's flagship or the NPC ship.

If you want to retrieve passengers or cargo by boarding a ship, set up the mission so that you are considered to be carrying them from the very start (for example, the cargo might be called "reserved mission space" or "mission cargo"). Otherwise, it would be possible for the player to board a ship and then discover they do not have enough cargo or passenger space to complete the mission.

```html
ship <model> <name>
```

This specifies a single ship as an NPC. The first argument is the model type (or named variant), such as "Falcon", or "Star Barge (Armed)". The second is the ship's name.

If you want to customize an NPC (for example, having it start out with a particular cargo), you will need to define a variant of the ship and then reference that variant here. Placing the entire ship definition within the NPC definition is supported (because that is how NPC ships are loaded from a saved game) but will not work properly if the ship definition contains any outfits that are not defined yet when the mission definition is parsed. When loading NPCs from saved games, the rest of the game data has finished loading, but this is not otherwise guaranteed.

```html
fleet <name> [<count#>]
fleet [<count#>]
    ...
```

This specifies an entire fleet of ships. The first format refers to one or the standard fleets, such as "pirate raid" or "Small Republic". The second format gives a custom fleet, using the same syntax as normal [`fleet` data entry](CreatingFleets). Every ship in the fleet will have the requirements given in the first line (such as `kill` or `save`). Optionally, you can specify a count to create more than one copy of the fleet.

<a name="triggers">

# Triggers
</a>

A mission can also specify what happens at various key parts of the mission:

```html
on (offer | complete | accept | decline | defer | fail | abort | visit | stopover | waypoint | enter [<system>] | daily)
    log [<category> <header>] <text>
    dialog <text>
        <text>...
    dialog phrase <phrase>
    conversation <name>
    conversation
        ...
    outfit <outfit> [<count#>]
    require <outfit>
    give ship <model> [<name>]
    payment [<base> [<multiplier>]]
    fine <amount>
    <condition> (= | += | -=) <value#>
    <condition> (++ | --)
    (set | clear) <condition>
    event <name> [<delay#> [<max#>]]
    fail [<name>]
```

There are eleven events that can trigger a response of some sort:

* `offer`: when the initial mission is offered. This is the place to put the conversation or dialog that introduces the mission.
* `complete`: when the mission is completed. This is when the player gets paid.
* `accept`: if the player agrees to accept a mission.
* `decline`: if the player decides to decline a mission.
* `defer`: if the player decides to defer a mission.
* `fail`: if the mission fails.
* `abort`: if the mission is aborted by the player. If no `on abort` action exists and the player aborts a mission, then any `on fail` action will be triggered instead.
* `visit`: you land on the mission's destination, and it has not failed, but you have also not yet done whatever is needed for it to succeed.
* `stopover`: you have landed on the last of the planets that are specified as a "stopover" point for this mission.
* `waypoint`: you have visited the last of the systems that are specified as a "waypoint" for this mission.
* `enter [<system>]`: your ship enters the given system for the first time since this mission was accepted. If no system is specified, this triggers as soon as your ship takes off from the current planet.
* `daily`: every time the date advanced (every jump between systems and departure from a planet). (**v. 0.9.15**)

Beginning with **v. 0.9.9**, the `enter` action supports determining the system with a location filter:
```html
on enter
    [system <name>...]
    ...
``` 

Some of the events below usually only make sense for certain triggers. In particular, dialogs and conversations can be shown when a mission is offered, but not in response to it being accepted or declined; just add the appropriate text to the offer conversation instead.

```html
log [<category> <header>] <text>
```

This creates a log entry in the player's log book, which is found on the player info page. Log entries are capable of having an optional category and header that they go under. If no category is given, then the log entry's header will be the date that the log was given, while the category will be the year.

An example on how one might use the log category and header includes creating a category of logs on the various factions of the game, with the headers being each of the factions. If a log is given with a category and header that already has an entry, then the new log will go below the existing entry under the same header.

```html
dialog <text>
    <text>...
dialog phrase <phrase>
```

This gives a message to be displayed in a dialog message to the user. If the trigger is `on offer`, the dialog will have "accept" and "decline" buttons. Otherwise, it is a purely informational message and only an "okay" button is shown.

Each token following the `dialog` tag will be a separate paragraph. The first token may appear either on the same line or indented on a subsequent line.

`dialog phrase` can be used to create a single phrase that is used for multiple dialogs, instead of needing to copy and paste the same dialog over and over again. An example of where this is used in game is for `on visit` dialogs.

As mentioned previously, text replacement is done on keywords like "`<destination>`" and "`<payment>`" within the dialog text.

```html
conversation <name>
conversation
    ...
```

This specifies that a conversation will be shown to the player at this point in the mission. When a mission is being offered, the conversation can return `accept` or `decline`; conversations can also return special values like `die` or `explode` (if the conversation ends with the player dying or their flagship exploding) or `launch` (if the player should take off from the planet immediately).

For conversations that are offered when boarding a ship or completing an NPC, if the conversation ends with `launch`, `flee`, or `depart`, the ship in question will die.

As with the dialogs, text substitution is done throughout the conversation.

The syntax for conversations is described [here](WritingConversations).

```html
outfit <outfit> [<number#>]
require <outfit> [<number#>]
```

At this point in the mission, the named ship outfit (or some number of them, if a number is given) is installed in the player's flagship, or placed in the player's cargo if the flagship has no outfit space for it. If the number is negative, outfits are taken away, checking the player's flagship and the cargo holds of all in-system ships, or only the flagship's cargo if this is a boarding mission. If a mission removes outfits in its "complete" phase, it cannot be completed unless that outfit exists. This makes it possible to loan the player an outfit for the duration of a mission, or to require that the player disable a non-NPC ship and steal a particular piece of technology from it.

If the outfit cannot be installed due to lack of space, a warning message will be shown so the player knows that the outfit is not actually active (and may in fact be lost if they leave the planet).

The `require` keyword checks that the player has at least one of the named outfit, but does not take it away. For example, this could be used in the `on offer` phase to only offer a mission to players who have a "Jump Drive". Starting with **v. 0.9.9**, a specific quantity can be required, including 0 (i.e. the player cannot have any). If a non-zero quantity is specified then the player's flagship is checked alongside the cargo holds of all in-system escorts, or only the flagship's cargo if this is a boarding mission. If a quantity of zero is specified then the player cannot have that outfit anywhere on any of their ships.

Beginning in **v. 0.9.15**, if the outfit being gifts has the "map" attribute, then the player will be given the information from that map as if they had purchased it from the outfitter. If an outfit with the "map" attribute is being required by a mission and the required value is 0, then the player must have the nearest systems that match the size of the map outfit unvisited, but if the required value is greater than 0 then the nearest systems must be visited.

```html
give ship <model> [<name>]
```

The named ship model is given to the player. This ship model can be a [ship variant](https://github.com/endless-sky/endless-sky/wiki/CreatingShips#variants). It is optional that the given ship has a name, but if no name is provided then a random name will be generated from the civilian phrase.

```html
payment [<base#> [<multiplier#>]]
```

This specifies the payment for a mission, which depends on the number of jumps between the source and destination and the amount of cargo and passengers. If the mission has waypoints or stopovers then the number of jumps needed to visit all waypoints and stopovers before reaching the destination is used. The exact formula is used to determine the final payment:

```c++
base + multiplier * (jumps + 1) * (10 * (# of passengers) + (tons of cargo))
```

If no base or multiplier is given, the default is base = 0, multiplier = 150. If a single value is given, it denotes a flat payment (i.e. multiplier = 0).

Multiple instances of `payment` are acceptable and useful in certain circumstances. For example, many of the generic human jobs have two payments. The first payment line is blank, indicating a job with no base amount and the default multiplier. The second line just contains a single value, which is interpreted as a flat amount. This allows a job to have a base amount, while otherwise scaling according to whatever the current default multiplier is. This allows the default multiplier to be adjusted globally without needing to edit individual jobs.
```
	on complete
		payment
		payment 2000
```

Normally, a `payment` value would only be given in the "on complete" section, but you can also have a negative value to be subtracted if the player fails the mission, or you could use a "payment" to advance the player some money when they first accept a mission. If the "on complete" payment is negative, the player cannot complete the mission if they have fewer than that number of credits.

```html
fine <amount#>
```

This specifies the fine for a mission, a high interest (0.006%, equivalent to a credit score of 0), short term (60 days) mortgage that must be paid.

```html
<condition> (= | += | -=) <value#>
<condition> (++ | --)
(set | clear) <condition>
```

This is used to adjust the "conditions" described previously, which form the requirements for other missions. You can either set the condition to a certain value (using the `=` operator), or add or subtract a value using `+=` or `-=`. As a shortcut, `++` means `+= 1` and `--` means `-= 1`. **You must place a space between the condition, the operator, and the value in order for the parser to interpret it correctly.**

The `set` and `clear` tags are shortcuts for `= 1` and `= 0`, respectively.

To change the player's reputation or combat rating, you may alter the `"reputation: <government>"`, or `"combat rating"` conditions. For example, a "bounty hunting" mission might automatically alter your reputation on whatever planet the mission ends on ("reputation"), and a mission for the Navy might improve your reputation with the Republic but reduce your reputation with the Free Worlds.

```html
event <name> [<delay#> [<max#>]]
```

This specifies that the given event happens at this point in the mission. Events may permanently alter planets or solar systems. If a delay is given, the event will occur that number of days from now, instead of happening immediately. If both a minimum and a maximum delay is given, the number of days from now will be chosen randomly from within that interval.

```html
fail [<name>]
```

This causes the named mission (or this mission, if no name is given) to fail immediately. The name should be the unique mission name that is used in condition strings, etc., not the "display name" that is shown to the player. This can be used, for example, to create a mission which gives you an item or payment if it is accepted, but is not actually added to your mission list.