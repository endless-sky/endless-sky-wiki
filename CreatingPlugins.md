The easiest way to test out new content for Endless Sky is to create a plugin. An example plugin is available [here](http://endless-sky.github.io/example-plugin.zip). For each operating systems, there are two places where the game looks for plugins, but generally you should only use the second option:

### Linux ###
* /usr/share/games/endless-sky/plugins/
* ~/.local/share/endless-sky/plugins/

### Windows ###
* plugins\ (in the same folder as the Endless Sky executable)
* C:\Users\yourusername\AppData\Roaming\endless-sky\plugins\

### Mac OS X ###
* Content/Resources/plugins/ (within the application bundle)
* ~/Library/ApplicationSupport/endless-sky/plugins

Your plugin should be placed in its own folder (named after the plugin) within one of those "plugins" folders (e.g. the example plugin's `about.txt`, `data/`, etc. would be in a folder named "example-plugin" which in turn is placed in the "plugins" folder).

### Editing data files

When editing the game data, you should use a text editor that is designed for writing computer code. The data file format is represented by how indented each line is, so editing the data in a word processor like Microsoft Word or in a program like Notepad that does not support automatic indentation will be difficult and frustrating. If you're on Windows, [Notepad++](https://notepad-plus-plus.org/) is a good, free text editor you can use. 

Plugins can contain any of the same data that the game's ordinary data files contain. If a data entry (e.g. a ship, a planet, etc.) is defined in a plugin, it overrides whatever was defined in the game data. If an image appears by the same name in a plugin and in the original game data, the image from the plugin is used. If two plugins define the same image or data, which one is used will depend on what order the plugins are loaded in, and currently there is no way to control that. (Eventually, there will be a preferences panel specifically devoted to plugins, which will list all available plugins and let you reorder or disable any of them, as well as showing a user-provided description of each plugin.)

Note that if you remove a plugin or remove items from it, saved games that depend on those items can end up "broken." So, if you're still developing a plugin you may want to create a snapshot of your pilot file first, or test with a pilot you don't care about.

### The structure of a plugin

A plugin folder can contain the following:

  * `copyright`: a plain-text file giving copyright information in [Debian copyright format](https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/). _(required)_
  * `about.txt`: a plain-text file describing the plugin.
  * `data/`: any data files must be placed in this folder, or they will not be loaded.
  * `images/`: all images should be placed in here. When specifying sprite paths, they are relative to the `images` directory. For example, `ship/berserker` refers to `plugin-root/images/ship/berserker.png`.
  * `sounds/`: all sounds must be placed in here. As with images, the path to a sound is relative to this folder.

I plan to eventually set up a plugins server that can be accessed within the game itself to get a listing of plugins, download plugins, and select which ones are activated and in which order (which is important because two plugins might override the same element of the game data).

### Creating or overriding data elements

The game data in Endless Sky includes the following elements:

  * `color`: a named color in premultiplied RGBA format, to be used in the user interface. The format is: `color "name" <red> <green> <blue> <alpha>`. By setting alpha to 0 you can define a color that uses additive mixing, and you can also use [other blending modes](BlendingModes).
  * `conversation`: a [conversation](WritingConversations) to be shown in a mission event.
  * `effect`: an animation that is used purely for visual effect and does not interact with objects in the game. (For example, the explosions shown when a projectile strikes something.)
  * `event`: a list of changes to the game data, which will happen either on a specific date, or in response to a mission. Events can contain any of the other data elements listed here.
  * `fleet`: a list of possible ship combinations and relative frequencies. (For examples, see `fleets.txt`.)
  * `galaxy`: a sprite that should be shown in the background of the map. This includes the text labels as well as the galaxy image itself.
  * `government`: the characteristics of a particular government.
  * `interface`: the layout of a user interface element. (For examples, see `interfaces.txt`.)
  * `mission`: a definition of a [job or mission](CreatingMissions) that can be offered to the player.
  * `outfit`: an [outfit](CreatingOutfits) that you can purchase. This includes weapons.
  * `outfitter`: a "catalog" of outfits that can be for sale on various planets. This is so that instead of a planet needing to specify every outfit it sells, it can just say, "all the basic Syndicate outfits are available here."
  * `person`: a unique ship that occasionally appears in certain systems.
  * `phrase`: a rule for constructing random phrases, e.g. for ship names or hail messages. (See `hails.txt` or `names.txt` for examples.)
  * `planet`: specifies what services are available on a given planet. These can be modified through the [map editor](https://github.com/endless-sky/endless-sky-editor).
  * `ship`: the attributes of a [ship](CreatingShips) and its default outfits, or a ship variant with an alternative set of outfits.
  * `shipyard`: a "catalog" of ships that can be for sale on various planets.
  * `start`: starting conditions for the player.
  * `system`: a star system. Generally, these will be created through the [map editor](https://github.com/endless-sky/endless-sky-editor) so that orbital periods, habitable zones, etc. will be consistent.
  * `trade`: a list of commodity names and prices.

To modify an existing data element, you only need to include the particular fields you are interested in. For example, to change the government of Kornephoros from "Republic" to "Free Worlds" all you need to do is this:

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