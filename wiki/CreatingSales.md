# Table of Contents

* [Shipyards](#shipyards)
* [Outfitters](#outfitters)

# Shipyards

## Normal Shipyards

If you want your ships to be available, you must add them to a shipyard. To do so, you must have something like this:

```
shipyard "Syndicate Advanced"
    "Plugin Ship One"
    "Plugin Ship Two"
```

Any ships you list will be appended to the ships currently in the list you named. So, the above example would make two new ships available on all planets that have the "Syndicate Advanced" shipyard.

## Shipyard Stock

```
"shipyard stock" "<name>"
    "<ship name>"
        probability <%>
        quantity <#>
        discount <%>
        depreciation <#>
```
```
"shipyard stock" "Random Flivvers"
    "Flivver"
        probability 20
        quantity 5
        discount 40
```

Added in **v. 0.10.11**, the above example would make any planet with the "Random Flivvers" `shipyard stock` have a 20% chance of selling 5 Flivvers with a discount of 40 percent.

```
"shipyard stock" "Random Sparrows"
    "Sparrow"
        probability 70
        quantity 2
        depreciation 105
```

Added in **v. 0.10.11**, the above example would make any planet with the "Random Sparrows" `shipyard stock` have a 70% chance of selling 2 Sparrows which have been losing value for 105 days.

# Outfitters

## Normal Outfitters

In order for anyone to buy your outfits, they must be added to an "outfitter" object. For example, if you are writing a plugin, you could include this in one of your data files:

```
outfitter "Syndicate Advanced"
    "My Fancy New Outfit"
    "My Other Fine Outfit"
```

Any outfits you list will be appended to the outfits currently in the list you named. So, the above example would make two new outfits available on all planets that have the "Syndicate Advanced" outfits.

## Outfitter Stock

```
"outfitter stock" "<name>"
    "<outfit name>"
        probability <%>
        quantity <#>
        discount <%>
        depreciation <#>
```
```
"outfitter stock" "Random Nukes"
    "Nuclear Missile"
        probability 50
        quantity 3
        discount 99
```

This was added in **v. 0.10.11**. It allows you to make outfits sometimes be in stock on any particular planet. The above example would make any planet with the "Random Nukes" `outfitter stock` have a 50% chance of selling 3 heavily discounted nuclear missiles.

```
"outfitter stock" "Random Ramscoops"
    "Ramscoop"
        probability 5
        quantity 10
        depreciation 45
```

The above example would make any planet with the "Random Ramscoops" `outfitter stock` have a 5% chance of selling 10 ramscoops that have been losing value for 45 days.
