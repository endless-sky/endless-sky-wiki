# Table of Contents

* [Introduction](#introduction)
* [Playlists](#playlists)
* [Tracks](#tracks)

# Introduction

This page is about how to integrate music you made into the game in form of playlists and tracks. This system allows for regional or event-based music that can switch between different tracks depending if you are in combat, landed on a planet or just flying around.

# Playlists

Playlists are a collection of tracks that can be played in specific scenarios. A simple playlist might look like this:
```html
playlist "Cool playlist"
	location
		government Syndicate
	tracks random
		"Favourite track" 5
		"Other track" 2
		"Another track"
Here is a full list of everything that can go into a playlist:
```html
playlist <name>
	to play
		<condition> <comp> <value>
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
		[<modifier>] near <system> [[<min>] <max>]
		[<modifier>] distance [<min>] <max>
		neighbor
			...
		not
			...
	priority <number#>
	weight <number#>
	tracks [(random | linear | pick)]
		<track name> [<weight#>]
		...
```

Each track entry in the `tracks` list is the name of the track, optionally followed by a track `weight` (likelihood that track is picked initially or during each shuffle at `random`).  If not specified, the default weight is 1 for tracks. The weight must be positive.

Playlists have additional optional options, for specifying when the playlist could be played, and how likely it is to be selected:

* `to play` is the [player conditions](Player-Conditions) in which tracks have a chance to play. For example, play during active missions or a condition set during conversation. If not specified, the task can be played regardless of the current conditions.

* `location` uses a [location filter](LocationFilters) which limits where the play list is allowed to play.  If no location is specified, then every location is valid.

* `priority` (`0` or higher) sets the playlist priority when multiple playlists match `to play` and `location`. Higher priority is always played. The default priority is 1.

* `weight` (positive) if multiple playlists have the same priority, then weight is a factor in chance for that given playlist to be selected in a list of playlists matching `to play`, `location`, and `priority`. Increased weight means the playlist has a higher likelihood of being played.

* `tracks [(random | linear | pick)]` There are three ways track progression can be selected.  If the no progression is provided, then the default is `linear`.
      
	* `random` will shuffle tracks and pick tracks according to their `weight` (likely chance of being picked).
	* `linear` tracks will play linearly. An initial track is selected based on weights at random. From the selected track onward the track progression will be linear in the list of defined tracks.
	* `pick` will pick a single track to play. The playlist will keep repeating the track until playlist conditions no longer match. A track is selected at random with a higher `weight` increasing the chance a track will be picked.


# Tracks

The full syntax for a track looks like this:
```html
track <name>
	combat <music>
	idle <music>
	landed <music>
	[volume <modifier#>]
	[wait <seconds#>]
```

Each track can specify different music to play depending on what the player is doing. They can also adjust their volume, or specify a custom pause delay at the end of the track.

* `combat` music that is played during combat.

* `idle` music that is played while player is idle in space.

* `landed` music that is played when landed on a planet.

* `volume` (default: `0`; range between `-1` and `1`) When a track plays you can specify that it be played with an increased or decreased `volume` modified from the player preference volume slider.  For example, `volume 0.1` will increase the playback volume.

* `wait` stops playback for the specified time after the track finishes.