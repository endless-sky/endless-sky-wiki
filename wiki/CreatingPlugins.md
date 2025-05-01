The easiest way to test out new content for Endless Sky is to create a plugin. An example plugin is available [here](https://endless-sky.github.io/example-plugin.zip).

There are two places where the game looks for a plugins folder, the "resource" directory, where the base game data is, and the "config" directory, where user specific data, like save files and preferences are stored.
Generally, you should only use the config directory (second in the list below).
This list gives the default locations of the plugins folder in the resource and config directories, respectively, for each operating system.

#### Linux
* /usr/share/games/endless-sky/plugins/
* ~/.local/share/endless-sky/plugins/

#### Windows
* plugins\ (in the same folder as the Endless Sky executable)
* %APPDATA%\endless-sky\plugins\

#### macOS
* Contents/Resources/plugins/ (within the application bundle)
* ~/Library/Application Support/endless-sky/plugins/

Your plugin should be placed in its own folder (named after the plugin) within one of those "plugins" folders (e.g. the example plugin's `plugin.txt`, `data/`, etc. would be in a folder named "example-plugin" which in turn is placed in the "plugins" folder):

```
plugins/
|-- example-plugin/
|   |-- data/
|   |   |-- jobs.txt
|   |	 ...
|   |-- images/
|   |   |-- ship/
|   |   |   |-- example-ship.png
|   |   |	 ...
|   |	 ...
|   |-- sounds/
|   |   |-- ambient/
|   |   |   |-- rain.mp3
|   |   |	 ...
|   |   |-- kaboom.wav
|   |	 ...
|   |-- icon.png
|   |-- plugin.txt
|   |-- copyright
|	 ...
|-- other-plugin/
	  ...
```

## Finding errors

Alongside the plugins folder in the config directory, Endless Sky will create a file named "errors.txt".

Linux:
* ~/.local/share/endless-sky/errors.txt

Windows:
* %APPDATA%\endless-sky\errors.txt

macOS:
* ~/Library/Application Support/endless-sky/errors.txt

Any errors the game encounters while loading data or in gameplay will be written to this file.
The first time an error is encountered in a session, the existing content of this file will be deleted and overwritten, so the content reflects the most recent time errors were encountered. Note, however, that if two instances of the game are running, only one will be able to modify this file.
The errors reported here can be useful in identifying syntactic issues in plugin data, or finding mismatches in image properties, such as different frames of a sprite having different dimensions, or sprites having odd dimensions (which can lead to blurriness).

## Editing data files

When editing the game data, you should use a text editor that is designed for writing computer code. The data file format is represented by how indented each line is, so **editing the data in a word processor like Microsoft Word or in a program like Notepad that does not support automatic indentation will be difficult and frustrating**. If you're on Windows, [Notepad++](https://notepad-plus-plus.org/) is a good, free text editor you can use. 

Plugins can contain any of the same data that the game's ordinary data files contain. If a data entry (e.g. a ship, a planet, etc.) is defined in a plugin, it can<sup>[1](#footnote-1)</sup> override or modify the game's base definition of that kind of entry. For example, if an image appears by the same name in a plugin and in the original game data, the image from the plugin is used. If two plugins define the same image or data, which one is used will depend on what order the plugins are loaded in, and currently there is no way to control that. (Eventually, there will be a menu panel specifically devoted to managing plugins, which will list all available plugins and let you reorder or disable any of them, as well as showing a user-provided description of each plugin.)

Note that if you remove a plugin or remove items from it, saved games that depend on those items can end up "broken." So, if you're still developing a plugin you may want to create a snapshot of your pilot file first, or test with a pilot you don't care about.

<sub><sup><a name="footnote-1">1</a></sup> Depending on the type of entry being modified, some, all, or even none of its properties can actually be modified. See the individual data types' pages for specifics.</sub>
___

## The structure of a plugin

A plugin folder can contain the following:

  * `copyright`: a plain-text file giving copyright information in [Debian copyright format](https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/). _(required)_
  * `about.txt`: a plain-text file describing the plugin. Deprecated by plugin.txt.
  * `plugin.txt`: a plain-text file containing metadata about the plugin. **(v. 0.10.3)**
  * `icon.png`: an image that appears when selecting the plugin in the plugins menu.
  * `data/`: any data files must be placed in this folder, or they will not be loaded.
  * `images/`: all images should be placed in here. When specifying sprite paths, they are relative to the `images` directory. For example, `ship/berserker` refers to `${plugin-root}/images/ship/berserker.png`.
  * `shaders/`: any game shaders overwritten should be placed in here.
  * `sounds/`: all sounds must be placed in here. As with images, the path to a sound is relative to this folder.

Eventually, a plugin server will be set up that will be accessible within the game itself. This server will support basic plugin management, e.g. to get a list of available plugins, download plugins, and check for updates to any plugins that were already downloaded.

## Plugin metadata

```html
name <name>
about <description>
...
version <version>
authors
	<author>
	...
tags
	<tag>
	...
dependencies
	"game version" <version>
	requires
		<plugin name>
		...
	optional
		<plugin name>
		...
	conflicts
		<plugin name>
		...
```

Introduced in **v. 0.10.3**, the plugin.txt file in the plugin's root folder contains metadata about the plugin. Metadata is formatted in the same way that the data files are, with root nodes, tokens, and child nodes. Allowable metadata nodes are as follows:

* `name`: the name of the plugin to be displayed in the plugins menu. If no name is provided, defaults to the name of the folder that the plugin is from. If multiple plugins with the same `name` metadata are present, only the first reached by the game will be loaded, any subsequent plugins attempting to use the same name will not be loaded by the game.
* `about`: a description of the plugin to be displayed in the plugins menu. Prior to the creation of the plugin.txt file, the plugin's description was read from an about.txt file. You can add multiple `about` lines to display multiple lines in the game.
* `version`: the plugin's version number. **(v. 0.10.7)**
* `authors`: a list of names for the authors of the plugin. **(v. 0.10.7)**
* `tags`: a list of tags that act as descriptors for the plugin. **(v. 0.10.7)**
* `dependencies`: dependencies are info about other plugins or game versions that interact with this plugin in some way. **(v. 0.10.7)**
  * `"game version"`: the game version(s) that this plugin is expected to function with. **(v. 0.10.7)**
  * `requires`: a list of names for other plugins that are required for this plugin to function. **(v. 0.10.7)**
  * `optional`: a list of names for other plugins that are not required for this plugin, but may be installed because they either work well together or because having both installed provides additional content. **(v. 0.10.7)**
  * `conflicts`: a list of names for other plugins that conflict with this plugin. Users with both plugins installed may encounter odd behavior. **(v. 0.10.7)**

All of the metadata for a plugin will be appended to the plugin's description when it is displayed in the plugins menu. Although plugins can list dependencies of other plugins, these dependencies are currently not enforced in any way. That is, if two conflicting plugins are installed or you have a plugin that is missing a requirement, the plugin will still be loaded into the game if it is marked as active in the plugins menu. If for some reason a separate plugin is listed both in the conflicts list and in the optional or requires list, the plugin will be considered invalid and not be loaded by the game.

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
  * `interface`: the layout of a user interface element. (For examples, see [`interfaces.txt`](https://github.com/endless-sky/endless-sky/blob/master/data/_ui/interfaces.txt).)
  * [`mission`](CreatingMissions): a definition of a job or mission that can be offered to the player.
  * [`news`](CreatingNews): a definition of information that is provided to the player when visiting a spaceport.
  * [`outfit`](CreatingOutfits): an outfit that you can purchase. This includes weapons.
  * `outfitter`: a "catalog" of outfits that can be for sale on various planets, or found in a ship's cargo hold. This is so that instead of needing to specify every possible outfit multiple times, a planet or fleet can just say, "all the basic Syndicate outfits are available."
  * [`person`](CreatingPersons): a unique ship that occasionally appears in certain systems.
  * [`phrase`](CreatingPhrases): a rule for constructing random phrases, e.g. for ship names, hail messages, and the name used for a spaceport news message. (See [`hails.txt`](https://github.com/endless-sky/endless-sky/tree/master/data/human/hails.txt) or [`names.txt`](https://github.com/endless-sky/endless-sky/tree/master/data/human/names.txt) for examples.)
  * [`planet`](MapData#planets): specifies what services are available on a given planet. These elements can be modified through the [map editor](https://github.com/endless-sky/endless-sky-editor).
  * [`ship`](CreatingShips): the attributes of a ship and its default outfits, or a ship variant with an alternative set of outfits.
  * `shipyard`: a "catalog" of ships that can be for sale on various planets.
  * [`start`](Creating-Starts): starting conditions for the player.
  * [`system`](MapData#systems): a star system. Generally, these will be created through the [map editor](https://github.com/endless-sky/endless-sky-editor) so that orbital periods, habitable zones, etc. will be consistent.
  * `trade`: a list of commodity names and prices. (For examples, see [`commodities.txt`](https://github.com/endless-sky/endless-sky/tree/master/data/commodities.txt).)
  * [`swizzle`](#Swizzles): a color matrix that defines how colors are shifted.

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

## Disabling preexisting data

As of **v. 0.9.15**, plugins are able to disable certain existing content from appearing, offering, or being triggered.

The syntax for disabling content is as follows:
```
disable <category> <name>
	<name>
	...
```

The currently supported categories that can be disabled are mission, person, and event. Disabled content can either be listed inline or with a separate item to disable on each line as children of the disable node.

```
disable mission "Assisting Hai"
disable person
	"Power of the People"
	"Captain Nate"
disable event
	"war begins"
```

## Swizzles

Since **v0.10.13**:

```html
swizzle <name>
	[override]

	[red   [<red_i> <green_i> <blue_i> [<alpha_i>]]
	[green [<red_i> <green_i> <blue_i> [<alpha_i>]]
	[blue  [<red_i> <green_i> <blue_i> [<alpha_i>]]
	[alpha [<red_i> <green_i> <blue_i> [<alpha_i>]]
```

A swizzle defines a matrix for shifting colors. The swizzle will construct a new color channel-by-channel, for each of which it will take some influence of the input's color channels, defined by the `<channel>_i` value.

Setting the `override` flag will disable any swizzle masks on a sprite that uses this swizzle - this is useful if you want a 'ghost' or 'shadowed' effect and don't want anything to be left unswizzled.

For example:

```html
swizzle example
	red   0 1 0 0
	green 1 0 0 0
	blue  0 0 1 0
	alpha 0 0 0 1
```

This swizzle switches the red and green channels - this means that any red parts of the original color will become green and vice versa.

See [swizzles.txt](https://github.com/endless-sky/endless-sky/blob/master/data/_ui/swizzles.txt) for more examples.

## Shaders

Since **v.0.10.13**:

Plugins can override the game's shaders, effectively changing how each item is displayed on the screen. New shaders cannot be created, and the overwritten shaders must take the same arguments as those provided by the game. You can find the game's own shaders [here](https://github.com/endless-sky/endless-sky/tree/master/shaders).

Endless Sky uses shaders written in the [OpenGL Shading Language](https://www.khronos.org/opengl/wiki/Core_Language_(GLSL)). Our shaders are either vertex shaders or fragment shaders, which is determined from their file extension (`.vert` and `.frag`, respectively).

Since the game can run on both OpenGL and OpenGL ES, shaders can be defined for either or both environments. By default, shader files are valid in both environments; exclusive shaders can be created by appending the `.gl` or `.gles` extension to the shader file, resulting in something like `sprite.frag.gles`.
