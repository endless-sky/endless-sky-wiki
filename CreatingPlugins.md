The easiest way to test out new content for Endless Sky is to create a plugin. An example plugin is available [here](https://endless-sky.github.io/example-plugin.zip). For each operating systems, there are two places where the game looks for plugins, but generally you should only use the second option:

#### Linux
* /usr/share/games/endless-sky/plugins/
* ~/.local/share/endless-sky/plugins/

#### Windows
* plugins\ (in the same folder as the Endless Sky executable)
* C:\Users\yourusername\AppData\Roaming\endless-sky\plugins\

#### Mac OS X
* Contents/Resources/plugins/ (within the application bundle)
* ~/Library/Application Support/endless-sky/plugins

Your plugin should be placed in its own folder (named after the plugin) within one of those "plugins" folders (e.g. the example plugin's `about.txt`, `data/`, etc. would be in a folder named "example-plugin" which in turn is placed in the "plugins" folder):

```
plugins/
|-- example-plugin/
|   |-- data/
|   |   |-- jobs.txt
|   |     ...
|   |-- images/
|   |   |-- ship/
|   |   |   |-- example-ship.png
|   |   |     ...
|   |     ...
|   |-- sounds/
|   |   |-- ambient/
|   |   |   |-- rain.mp3
|   |   |     ...
|   |   |-- kaboom.wav
|   |     ...
|   |-- about.txt
|   |-- copyright
|     ...
|-- other-plugin/
      ...
```

## Editing data files

When editing the game data, you should use a text editor that is designed for writing computer code. The data file format is represented by how indented each line is, so **editing the data in a word processor like Microsoft Word or in a program like Notepad that does not support automatic indentation will be difficult and frustrating**. If you're on Windows, [Notepad++](https://notepad-plus-plus.org/) is a good, free text editor you can use. 

Plugins can contain any of the same data that the game's ordinary data files contain. If a data entry (e.g. a ship, a planet, etc.) is defined in a plugin, it can<sup>[1](#footnote-1)</sup> override or modify the game's base definition of that kind of entry. For example, if an image appears by the same name in a plugin and in the original game data, the image from the plugin is used. If two plugins define the same image or data, which one is used will depend on what order the plugins are loaded in, and currently there is no way to control that. (Eventually, there will be a menu panel specifically devoted to managing plugins, which will list all available plugins and let you reorder or disable any of them, as well as showing a user-provided description of each plugin.)

Note that if you remove a plugin or remove items from it, saved games that depend on those items can end up "broken." So, if you're still developing a plugin you may want to create a snapshot of your pilot file first, or test with a pilot you don't care about.

<sub><sup><a name="footnote-1">1</a></sup> Depending on the type of entry being modified, some, all, or even none of its properties can actually be modified. See the individual data types' pages for specifics.</sub>
___

## The structure of a plugin

A plugin folder can contain the following:

  * `copyright`: a plain-text file giving copyright information in [Debian copyright format](https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/). _(required)_
  * `about.txt`: a plain-text file describing the plugin.
  * `data/`: any data files must be placed in this folder, or they will not be loaded.
  * `images/`: all images should be placed in here. When specifying sprite paths, they are relative to the `images` directory. For example, `ship/berserker` refers to `${plugin-root}/images/ship/berserker.png`.
  * `sounds/`: all sounds must be placed in here. As with images, the path to a sound is relative to this folder.

Eventually, a plugins server will be set up that will be accessible within the game itself. This server will support basic plugin management, e.g. to get a list of available plugins, download plugins, and check for updates to any plugins that were already downloaded.

## Creating or overriding data elements

The game data in Endless Sky includes the following elements, sometimes referred to as "root-level" nodes or tokens:

  * `color`: a named color in premultiplied RGBA format, to be used in the user interface. The format is: `color "name" <red#> <green#> <blue#> <alpha#>`. By setting alpha to 0 you can define a color that uses additive mixing, and you can also use [other blending modes](BlendingModes).
  * [`conversation`](WritingConversations): a conversation to be shown in a mission event.
  * [`effect`](CreatingEffects): an animation that is used purely for visual effect and does not interact with objects in the game. (For example, the explosions shown when a projectile strikes something.)
  * [`event`](CreatingEvents): a list of changes to the game data, which will happen either on a specific date, or in response to a mission.
  * [`fleet`](CreatingFleets): a list of possible ship combinations and relative frequencies. (For examples, see [`fleets.txt`](https://github.com/endless-sky/endless-sky/tree/master/data/human/fleets.txt).)
  * [`galaxy`](MapData): a sprite that should be shown in the background of the map. This includes the text labels as well as the galaxy image itself.
  * [`government`](CreatingGovernments): the characteristics of a particular government.
  * [`hazard`](CreatingHazards): a weapon that can deal damage to ships in a system when active.
  * `interface`: the layout of a user interface element. (For examples, see [`interfaces.txt`](https://github.com/endless-sky/endless-sky/tree/master/data/interfaces.txt).)
  * [`mission`](CreatingMissions): a definition of a job or mission that can be offered to the player.
  * [`news`](CreatingNews): a definition of information that is provided to the player when visiting a spaceport.
  * [`outfit`](CreatingOutfits): an outfit that you can purchase. This includes weapons.
  * `outfitter`: a "catalog" of outfits that can be for sale on various planets, or found in a ship's cargohold. This is so that instead of needing to specify every possible outfit multiple times, a planet or fleet can just say, "all the basic Syndicate outfits are available."
  * [`person`](CreatingPersons): a unique ship that occasionally appears in certain systems.
  * [`phrase`](CreatingPhrases): a rule for constructing random phrases, e.g. for ship names, hail messages, and the name used for a spaceport news message. (See [`hails.txt`](https://github.com/endless-sky/endless-sky/tree/master/data/human/hails.txt) or [`names.txt`](https://github.com/endless-sky/endless-sky/tree/master/data/human/names.txt) for examples.)
  * [`planet`](MapData#planets): specifies what services are available on a given planet. These elements can be modified through the [map editor](https://github.com/endless-sky/endless-sky-editor).
  * [`ship`](CreatingShips): the attributes of a ship and its default outfits, or a ship variant with an alternative set of outfits.
  * `shipyard`: a "catalog" of ships that can be for sale on various planets.
  * `start`: starting conditions for the player.
  * [`system`](MapData#systems): a star system. Generally, these will be created through the [map editor](https://github.com/endless-sky/endless-sky-editor) so that orbital periods, habitable zones, etc. will be consistent.
  * `trade`: a list of commodity names and prices. (For examples, see [`commodities.txt`](https://github.com/endless-sky/endless-sky/tree/master/data/commodities.txt).)

To modify most properties of an existing data element, you only need to include the particular fields you are interested in. For example, to change the government of Kornephoros from "Republic" to "Free Worlds" all you need to write is this:

```bash
system "Kornephoros"
    government "Free Worlds"
```

All the other fields will remain the same. Some data fields can contain any number of elements: a system's list of asteroids, fleets, or stellar objects; a ship's list of outfits; a planet's paragraphs of descriptive text; an event's list of data elements to change; a fleet's list of ship combinations, etc. If you define a new value for one of these fields the old list of elements is replaced with your new one. The one exception is shipyard and outfitter lists: unless you specifically include the keyword "clear" in the list, any items you list will be added to the existing list without clearing it. For example, this adds three new outfits to the "Syndicate Basics":

```bash
outfitter "Syndicate Basics"
    "Ionic Afterburner"
    "S-270 Regenerator"
    "S-970 Regenerator"
```

...whereas this makes "Syndicate Basics" sell nothing but a few ammo items:

```bash
outfitter "Syndicate Basics"
    clear
    "Meteor Missile"
    "Javelin"
```