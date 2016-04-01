A "person" is a unique ship that will occasionally appear at random. Each person has a customized ship and its own set of things it might say to the player. If the player kills a person, that person will never appear to that particular pilot again. The [syntax](https://github.com/endless-sky/endless-sky/wiki/DataFormat#grammar-specifications) for the definition of a person is:

```html
person <name>
    frequency <frequency#>
    government <government>
    personality [<type>...]
        confusion <amount#>
        <type>...
        ...
    system
        system [<name>...]
            <name>...
            ...
        government [<name>...]
            <name>...
            ...
        near <system> [[<minJumps#>] <maxJumps#>]
        ...
    phrase
        word
            <text>
            ...
        ...
    ship <modelName>
        {ship definition...}
```

The various parts of a person definition are described below.

```html
frequency <frequency#>
```

At random intervals, averaging once every 10 minutes, there is an opportunity for a person to be created. The game looks for every person who has not already appeared since the last time the player took off from a planet, and who has not been killed by the player. It picks a random person to create based on the relative frequencies of the persons, which default to 100 if no value is given. That is, a person with a frequency of 200 is twice as likely to appear as one with the default frequency of 100.

To reduce the rate of persons being created if there are not many of them, there is also a chance that no person will be created, and that chance has a frequency of 1000. So, for example, if there are only two persons in the game and they both have a frequency of 100, they each have a 100 / (100 * 2 + 1000) = 8.3% chance of appearing in a given ten-minute interval.

```html
government <government>
```

This is the person's government. This can be a normal government, or a special one like "Author". But, there is only one ["Parrot"](http://evn.wikia.com/wiki/Hector).

```html
personality [<type>...]
    confusion <amount#>
    <type>...
    ...
```

This defines the [personality characteristics](https://github.com/endless-sky/endless-sky/wiki/CreatingMissions#non-player-characters-npcs). The confusion value is generally not used, but it is meant to control how accurately a ship fires its weapons. (It defaults to 10, meaning the ship's aim may be off by up to 10 pixels.)

```html
system
    system [<name>...]
        <name>...
        ...
    government [<name>...]
        <name>...
        ...
    near <system> [[<minJumps#>] <maxJumps#>]
    ...
```

This defines a filter for possible systems, using the same syntax as the source and destination filters in [missions](CreatingMissions). This controls which systems the person will appear in. For example, they might only appear in systems belonging to a certain government, or in a certain area.

```html
phrase
    word
        <text>
        ...
    ...
```

This defines what the person says if you hail them, or at random intervals if they decide to hail you. The phrase should be a collection of one or more "words," each of which is chosen from one or more options. If you want different options, you can define more than one "phrase" that the person might use. (See the "hails.txt" data file for examples of how this works.)

```html
ship <modelName>
    {ship definition...}
```

This is a definition of the person's ship, using the same format as all other [ship definitions](CreatingShips).