# Table of Contents

* [Shipyards](#shipyards)
* [Outfitters](#outfitters)

# Shipyards

## Normal Shipyards

If you want your ships to be available anywhere, you must add them to a shipyard. To do so, you must have something like this:

```
shipyard "Syndicate Advanced"
    "Plugin Ship One"
    "Plugin Ship Two"
```

The above example would add to the "Syndicate Advanced" shipyard the two ships "Plugin Ship One" and "Plugin Ship Two", because any ships you list will be appended to the ships currently in the shipyard named.

## Shipyard Stock

```
"shipyard stock" "<name>"
    "<ship name>"
        probability <%##>
        quantity <#>
        discount <%##>
```
```
"shipyard stock" "Random Flivvers"
    "Flivver"
        probability 20
        quantity 5
        discount 40
```

Added in **v0.10.9**, the above example would make any planet the "Random Flivvers" "shipyard stock" is in to have a 20% chance of selling 5 Flivvers on a discount of 40%. "shipyard stock" basically lets you have a random chance of a planet selling ships.

# Outfitters

## Normal Outfitters

In order for anyone to buy your outfits, they must be added to one of the "outfitter" objects. For example, if you are writing a plugin, you could include this in one of your data files:

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
        probability <%##>
        quantity <#>
        discount <%##>
```
```
"outfitter stock" "Random Nukes"
    "Nuclear Missile"
        probability 50
        quantity 3
        discount 99

This was added in **v0.10.9**. It allows you to make outfits sometimes be in stock on any particular planet. The above example would make any planet with the "Random Nukes" "outfitter stock" have a 50% chance of selling 3 heavily discounted nuclear missiles.