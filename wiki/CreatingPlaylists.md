# Table of Contents

* [Introduction](#introduction)
* [Playlists](#playlists)
* [Tracks](#tracks)

# Introduction

This page is not about the process of creating music but about how to integrate music you made into the game in form of playlists and tracks. This system allows for regional or event based music that can switch between different tracks depending if you are in combat, landed or just flying around.

# Playlists

The full syntax for a playlist looks like this:
```html
playlist <name>
	to play
		<condition<comp<value>
		(has | not) <condition>
		never
		(and | or)
			...
	location
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
		[<modifier>] near <system[[<min>] <max>]
		[<modifier>] distance [<min>] <max>
		neighbor
			...
		not
			...
	[priority <number 0 or greater>]
	[weight <number 1 or greater>]
	tracks [(random | linear | pick)]
		<track name[<weight>]
		...
```

`playlist` followed by the `name` of the playlist. It has one required option to be a valid playlist that plays sound.
    
* `tracks` followed by a list of tracks.
      
* Each track entry in the `tracks` list is the name of the track _optionally followed by_ a track `weight` (likelihood that track is picked initially or during each shuffle at `random`).  If not specified, the `default weight` is `1` for tracks.


And `playlist` has additional optional options:

* `to play` is the [player conditions](Player-Conditions) in which tracks have a chance to play.  For example, play during active missions or a condition set during conversation.  If not specified, then it always assumes it should play given any condition.

* `location` uses a [location filter](LocationFilters) which limits where the play list is allowed to play.  If no location is specified, then every location is valid.

* `priority` (`0` or higher) sets the playlist priority when multiple playlists match `to play` and `location`.  Higher priority is always played.  `0` is lowest priority. Do note that this does not accept fractional values like 1.5.

* `weight` (`1` or higher) if two playlists have the same priority, then weight is a factor in chance for that given playlist to be selected in a list of playlists matching `to play`, `location`, and `priority`.  Increased weight means the playlist has a higher likelihood of being played. Do note that this does not accept fractional values like 1.5.

* `tracks [(random | linear | pick)]` There are three ways track progression can be selected.  If the no progression is provided, then the default is `linear`.
      
	* `random` will shuffle tracks and pick tracks according to their `weight` (likely chance of being picked).
	* `linear` tracks will play linearly.  An initial track is selected based on weights at random.  From the selected track onward the track progression will be linear in the list of defined tracks.
	* `pick` will pick a single track to play.  The playlist will keep repeating the track until playlist conditions no longer match.  A track is selected at random with a higher `weight` increasing the chance a track will be picked.


# Tracks

The full syntax for a track looks like this:
```html
track <name>
	combat <music>
	idle <music>
	landed <music>
	[volume <double>]
	[wait <number of seconds>]
```

`track` followed by the `name` of the track is how a track can be defined. This can play different musics.

* `combat` music that is played during combat.

* `idle` music that is played while player is idle in space.

* `landed` music that is played when landed on a planet.

* `volume` (default: `0`; range between `-1` and `1`) When a track plays you can specify that it be played with an increased or decreased `volume` modified from the player preference volume slider.  For example, `volume 0.1` will increase the playback volume.

* `wait` after playing a track.  A track can force itself to `wait` for a specified number of seconds after it finishes playing.