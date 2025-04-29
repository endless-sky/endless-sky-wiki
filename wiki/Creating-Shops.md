# Introduction

Shops are outfitters or shipyards that sell outfits or ships on a planet. The [syntax](DataFormat#grammar-specifications) for the definition of a shop is:

```html
(outfitter | shipyard) <name>
to sell
	<condition set>
location
	<location filter>
	stock
		[remove]
		[remove] <item>
		...
	[remove]
	[remove] <item>
	...
```

# Basic shop characteristics

```html
(outfitter | shipyard) <name>
```

Shops are defined as root nodes with the token `outfitter` or `shipyard`. The former is used for selling outfits while the latter is used for selling ships. Shop names of the same shop type must be unique, and the same shop name appearing in two root nodes will result in both definitions being combined. (i.e. you could have an outfitter and shipyard with the same name without conflict, but two outfitters with the same name will combine.)

```html
stock
	[remove]
	[remove] <item>
	...
```

Starting in **v. 0.10.13**, items sold from a shop can be defined under a `stock` node. Each item is the name of an outfit or ship (corresponding to the type of items that the shop sells), with each item being listed on a separate line. If the token `remove` is added to a shop, then every item that was defined in that shop's stock prior to the `remove` line will be cleared. This can be used to have a plugin that overwrites the stock of a shop. If the `remove` node is not present, then two shop definitions with the same name will combine their stock. Shop definitions can also remove a particular item from the stock using `remove <item>`, removing the item of that name if it is present.

```html
[remove]
[remove] <item>
...
```

Prior to **v. 0.10.13**, the `stock` node did not exist, and all items sold in a shop were defined as direct children of the root node. This method is still supported for backwards compatibility, and has all the same behavior as the `stock` node described above.

Beginning in **v. 0.10.13**, an outfitter or shipyard is allowed to have an empty stock. This means that a planet can grant access to the outfitter or shipyard, but there won't be any outfits or ships for sale. This can be useful for creating planets that don't sell outfits but have the facilities for you to store outfits or edit the placement of outfits in your fleet, or planets that will buy ships from you but don't sell any. Prior to this update, empty shops would be logged as a data error and not be included on a planet.

# Adding shops to planets

There are two ways of adding shops to a planet. The first method is to add the shop to the [planet's definitions](MapData), as follows:

```html
planet Luna
	shipyard "Basic Ships"
	shipyard "Navy Basics"
	shipyard "Betelgeuse Basics"
	shipyard "Syndicate Basics"
	shipyard "Tycho Crater Advanced"
	outfitter "Common Outfits"
	outfitter "Lovelace Basics"
	outfitter "Syndicate Basics"
	outfitter "Ammo North"
```

Starting in **v. 0.10.13**, a second method for adding shops to a planet has been added.

```html
to sell
	<condition set>
location
	<location filter>
```

Shops can contain a `to sell` condition set or `location` location filter that specifies which planets and under what conditions a shop should become available. If neither of these nodes is defined for a shop, then that shop will only appear on a planet if it is added to that planet's root node as in the first method. If a shop definition contains either a `to sell` node or a `location` node, or both, then the game will determine when you land on a planet whether there are any shops with either of these nodes that allow that shop to appear on the planet. If both nodes are present, then both the condition set and the location filter must match your current conditions and location.

If a shop has a `to sell` and/or `location` node, but is also included in a `planet` definition, then the `to sell` and `location` nodes will be ignored when you land on that planet and the shop will always be available, while still being able to appear on other planets who haven't listed the shop as permanently available but do match the `to sell` and/or `location` node.

For more information on [condition sets](Player-Conditions) and [location filters](LocationFilters), see the relevant wiki pages.

Note that at the moment, shops defined to only conditionally appear are not visible on the map. That is, if a conditional outfitter were to sell an outfit on Luna that is not present in one of the outfitters listed above, that information would not be available on the map.
