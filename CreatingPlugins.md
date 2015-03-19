The easiest way to test out new content for Endless Sky is to create a plugin. An example plugin is available [here](http://endless-sky.github.io/example-plugin.zip). You must create a "plugins" folder in one of two places, and install the plugin there. Depending on your operating system, the plugin should be placed in:

### Linux ###
* /usr/share/endless-sky/plugins/
* ~/.local/share/endless-sky/plugins/

### Windows ###
* plugins\ (in the same folder as the Endless Sky executable)
* C:\Users\yourusername\AppData\Roaming\endless-sky\plugins\

### Mac OS X ###
* Content/Resources/plugins/ (within the application bundle)
* ~/Library/ApplicationSupport/endless-sky/plugins

Plugins can contain any of the same data that the game's ordinary data files contain. If a data entry (e.g. a ship, a planet, etc.) is defined in a plugin, it overrides whatever was defined in the game data. If an image appears by the same name in a plugin and in the original game data, the image from the plugin is used. If two plugins define the same image or data, which one is used will depend on what order the plugins are loaded in, and currently there is no way to control that. (Eventually, there will be a preferences panel specifically devoted to plugins, which will list all available plugins and let you reorder or disable any of them, as well as showing a user-provided description of each plugin.)

Note that if you remove a plugin or remove items from it, saved games that depend on those items can end up "broken." So, if you're still developing a plugin you may want to create a snapshot of your pilot file first, or test with a pilot you don't care about.