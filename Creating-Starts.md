# Introduction

A "start" serves as the declaration of the beginning state of a new pilot. The game or plugins may use this to set the system & planet where the game begins, [conditions](Player-Conditions), and also the funds available to the new pilot. Beginning with **v 0.9.9**, a start may also list the ship(s) the pilot owns.

Prior to **v. 0.9.13**, all start definitions replaced the previous definitions, and customized the displayed "new pilot" conversation by defining a conversation named `"intro"`. Beginning with **v 0.9.13**, this default-override behavior was removed and the default conversation key was changed to `"default intro"`. Instead, the player is presented with a list of the available valid starts when beginning a new pilot. Plugin authors may optionally provide an `identifier` to enable the previous override system.

# Datafile Syntax

The [syntax](DataFormat#grammar-specifications) for the definition of a start is:

```html
start [<identifier>]
    name <name>
    [remove name]
    [add] description <text>
    [remove description]
    thumbnail <pathname>
    [remove thumbnail]
    date <day#> <month#> <year#>
    system <name>
    planet <name>
    [add] account
        credits <amount#>
        salaries <amount#>
        maintenance <amount#>
        score <rating#>
        history
            <worth#>
            ...
        mortgage (Mortgage | Fine)
            principal <amount#>
            interest <rate#>
            term <days#>
        ...
    conversation <name>
    conversation
        {conversation specification...}
    [remove conversation]
    ship <model>
        {ship specification...}
    [remove ships]
    [remove ship <model>]

    {condition specification...}
        ...
    [remove conditions]
```

In **v 0.9.13** and later versions, a plugin may provide an `identifier` in the "start" declaration. Providing an identifier enables the following start definition to both extend an existing definition, and be extended by other start definitions.

<a name="required">

## Required Characteristics
</a>

At minimum, a start is expected to provide a system, planet, date, and assets that a new pilot will begin with. Beginning with **v. 0.9.13**, a start is also required to provide an intro conversation, either naming one defined elsewhere, or defining it in-line.

#### system
```html
system <name>
```
The name of the system in which the player starts. The start will default to "Rutilicus" if not provided.

#### planet
```html
planet <name>
```
The name of the planet on which the player starts. The start will default to "New Boston" if not provided. It is expected that the named planet is contained within the start's `system`.

#### date
```html
date <day#> <month#> <year#>
```
The date on which the game starts. For plugins that extend the main game's data, changing the start date would allow customizing how long the player has before certain timed events occur.

#### conversation
```html
conversation <name>
conversation
    {conversation specification...}
```
The conversation that will be displayed to the player after they click the "New Pilot" button. The referenced or defined conversation is required to include a `name` prompt. The full syntax available is described [here](WritingConversations). The "exit code" of an intro conversation is not used.

#### account

```html
[add] account
    credits <amount#>
    salaries <amount#>
    maintenance <amount#>
    score <rating#>
    history
        <worth#>
        ...
    mortgage (Mortgage | Fine)
        principal <amount#>
        interest <rate#>
        term <days#>
```
The starting assets & liabilities for a new pilot. For plugins that extend an existing start definition, `account` will totally replace the previous account definition, while `add account` enables modifying portions of the previous definition.

```html
credits <amount#>
```
The initial number of credits the player has. Setting this will always overwrite the previous value.

```html
salaries <amount#>
```
Back pay that the player owes to crew. Setting this will always overwrite the previous value.

```html
maintenance <amount#>
```
Unpaid maintenance the player owes. Setting this will always overwrite the previous value.

```html
score <rating#>
```
The player's initial credit score. Values range from 200 to 800, inclusive. Setting this will always overwrite the previous value. The player's credit score factors into the interest rate on available mortgages, going up when payments are made on time or not required, and decreasing when payments are missed.

```html
history
    <worth#>
    ...
```
A log of the player's net worth over time, as credits. Starting conditions are not expected to provide a history, but may. The player's net worth history factors into the amount of credits they may borrow when applying for new loans.

```html
mortgage (Mortgage | Fine)
    principal <amount#>
    interest <rate#>
    term <days#>
```
A declaration of a loan or fine the player is contractually obligated to pay off. When `add account` is in use, will add to the liabilities declared by the previous account definition. Removing liabilities is not supported; a plugin wishing to do so will need to define all desired `account` properties.

```html
principal <amount#>
```
The number of credits the player borrowed or was fined. The player must pay this amount of credits in addition to those generated by interest.

```html
interest <rate#>
```
The interest rate applied to the remaining balance, compounded daily.

```html
term <days#>
```
The number of days over which the loan or fine will be paid off. Once zero, the player will be required to pay the remaining balance in full.

<a name="optional>

## Optional Characteristics
</a>

#### Ships

Beginning with **v. 0.9.9**, a start may optionally provide one or more ships to the player. If present, the initial game screen for the new pilot will be the planet panel, rather than the shipyard (as the player no longer needs to purchase a ship).

```html
ship <model>
    {ship specification...}
```
To provide a ship for the new pilot, the entire [ship definition](CreatingShips) is required.

### Displayed Details

Beginning in **v. 0.9.13**, the player is presented with a list of starting scenarios and may select one with which to begin the game. Several new optional properties were added to enhance this experience. 

#### name
```html
name <name>
```
A user-friendly name for the start definition. This value will be displayed in the list of starts, and defaults to "(Unnamed Start)".

#### description
```html
description <text>
```
A description of the scenario associated with this start condition. This field can be used to provide background information that is otherwise not available, such as what the player did prior to becoming a starship captain.
Multiple `description` nodes may be provided in a single start's definition, and will be concatenated into newline-separated text for display. The default text is "(No description provided.)"

#### thumbnail
```html
thumbnail <pathname>
```
A path, relative to the `images` directory, for an image that will be displayed when the start condition is selected in the list of all start conditions.

### Conditions

Any lines that do not match one of the above syntax definitions will be parsed as [player conditions](Player-Conditions#applied). When the player begins a pilot with this scenario, these conditions will be applied to the player and subsequently available for later use by missions. A common use of starting conditions is to provide licenses.

# Extending Starts

Plugins that provide an identifier in their start definition, e.g. the "cool start" in `start "cool start"`, are opting-in to the game's definition extension system, and their provided definition will be merged into any existing definitions with that same identifier.

### "Add" prefixes

Most fields associated with a start do not benefit from the "add" keyword when extending an existing definition. For example, a start may have only one `name` field, so using `add name "my new name"` is nonsensical -- just use `name "my new name"` directly.

The following fields will behave differently when prefixed with "add":
* `account`: the following account definition will define a subset of the final account information; for example just the credits or an additional loan. Any previous account data is kept unless modified, excepting mortgages and history in which the new values will append to the existing values.
* `description`: the provided text will be appended to the previous description, rather than beginning a new sequence of text. As with other uses of multiple `description` fields, a new line will separate the provided text from prior text.

### "Remove" prefixes

The "remove" field prefix can be used with just about every start property to achieve the expected outcome: removal of that property's value.

"Remove" can also be applied to several keywords that are otherwise ignored when parsing a start definition:
* `remove ships`: Removes all ships that the existing definition provides. Useful when the specific ship model names that should be removed are not known.
* `remove ship <model>`: Removes any provided ships that have the given model name.
* `remove conditions`: Clears all player conditions that were set previously.
