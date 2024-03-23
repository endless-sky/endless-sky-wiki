# Introduction

Version 0.9.9 added a "news system" where people in the spaceport will tell you something about their life on that planet. The goal of this system is to make the spaceport feel more like a real place, populated by real people, rather than just a button you click on to check for missions. Each time you click the spaceport button, a "news" item is randomly generated, in the same way that a ship displays a randomly generated message each time you hail it.

# Portraits

**Note:** Portraits have since been removed from use in the vanilla game, but the mechanic for portraits still exists to be used by plugins. The following were the guidelines that existed around portrait images.

Portraits should be PNG images with the person's silhouette "cut out," designed to look good when displayed over a dark grey background. Source images should have enough resolution to look sharp at 480 x 480 pixels. For the sake of consistency:

* Each person's face should be half the width of the image. (Or slightly less if they have a big hat or lots of hair.)
* Photos with fancy "color filters" should not be used, and the light intensity curves should be adjusted to avoid washed-out or overly-contrasty images.
* Anything obviously 21st-century (like brand logos) should be avoided.
* If possible, the image should just contain the person's face and shoulders, not their hands, etc.
* It's okay to crop a person's shoulders on the right and left, and their hair if necessary, but their whole head should fit inside the image.

To make it possible to tweak the masking, colors, etc. after the fact, you should save a GIMP file for each image. That will also ensure that the name of the source image is recorded (assuming you keep it as the layer name in the XCF file).

For masking out people from an image, I find it helpful to do the following:

1. Split the image into its RGB color components.
2. Pick the component where the person's head (and in particular, their hair, which is the trickiest part) is as different a shade as possible from the background.
3. Adjust the curves to make the head white and the background black in the image.
4. Manually fill in sections of the silhouette as necessary.
5. Add a layer mask to the original layer and paste the silhouette into it.
6. Apply the final cropping and scaling (to 480 x 480).
7. Test the image over a black background. If the original background was light-colored, you may need to darken the edges to avoid the background color bleeding through. Or, you can "erode" the mask (`Filters -> Generic -> Erode`) slightly to eliminate fringe pixels.

# People

In-flight messages from other ships already give a lot of flavor from a pilot's perspective. To keep the news system distinctive, it should focus on the life of planet-bound people who rarely, if ever, venture out into space.

<details><summary>Ideas of people you might talk to:</summary>

* unemployed youth
* union organizer
* dock worker
* sidewalk sweeper
* domestic worker / housekeeper
* groundskeeper
* hair stylist
* bellhop
* tour guide
* day care worker
* retail worker
* travel agent
* street vendor
* human resources worker
* salesperson
* cattle / sheep farmer
* cafe worker
* hotel manager
* artist
* musician
* street performer
* factory worker
* loan officer
* revenue agent
* tax preparer
* hyperspace relay engineer
* architect
* geological engineer
* transport pilot
* agricultural scientist
* factory inspector
* agricultural inspector
* forester
* epidemiologist
* atmospheric scientist
* hydrologist
* economist
* survey researcher
* urban planner
* company psychologist
* nuclear technician
* lawyer
* rehabilitation worker
* social worker
* substance abuse counselor
* legal assistant
* reporter
* teacher
* museum curator
* librarian
* painter
* fashion designer
* actor
* athlete
* technical writer
* doctor
* medical researcher
* surgeon
* nurse
* dietitian
* personal trainer
* genetic counselor
* police detective
* security guard
* chef
* bartender
* waiter / waitress
* insurance agent
* carpenter
* stone mason
* plumber
* roofer
* derrick operator
* explosives worker
* bank teller
* fisherman
* meat packer
* baker
* machinist
* welder
* tailor
* power plant operator
* chauffeur
* crane operator
* excavation specialist
</details>

# Definition syntax

To refer to a News object, it should be named by providing a single token on the same line as `news`, e.g.
```c++
news "human tourism"
```
introduces or modifies a news definition known as "human tourism". A good way to think about the name of a news object is as a source or subject of information. The messages associated with that news would be either something said by a person associated with that source, or about that subject.

A `news` datafile definition has 4 definable child elements. 
* `name`: The name of the [spaceport person](#People) that will appear along with the news message. For flexibility, this uses the same format as creating ship hails, meaning you can define any number of names to be used with the same set of messages.
* `portrait`: The path to a sprite to be displayed in the news message, relative to the `images` directory. Multiple sprites can be provided, and one will be chosen at random when the news message is displayed.
* `message`: The text to be displayed. For flexibility, this uses the same "phrase" format that the news' name element and ship hails use.
* `location` (optional): A [location filter](LocationFilters) that is used to identify on which planets this News message can appear. If not provided, or removed, then this news source will not be displayed anywhere. For the full syntax associated with locations filters see [the reference here](LocationFilters).
* `to show` (optional): A condition set that determines whether this News message can appear. Behaves the same way as `to offer` does for [missions](https://github.com/endless-sky/endless-sky/wiki/CreatingMissions#conditions). 

```html
news <name>
	name
		{phrase specification...}
	[remove name]
	to show
		{condition set...}
	[remove to show]
	portrait <path>...
		<path>
		...
	[remove portrait [<path>...]]
	location
		{filter specification...}
	[remove location]
	message
		{phrase specification...}
	[remove message]
```

This news definition can appear on planets within 100 hyperspace links of Sol, and on render will display one of 5 portraits with one of 3 names and one of two message types: one constructed from the "friendly author" hails, and one constructed from the text shown here.
```java
news "contributing"
	location
		near "Sol" 100
	name
		word
			"Writer"
			"Programmer"
			"Artist"
	portrait "portrait/novelist" "portrait/painter"
		"portrait/sculptor"
		"portrait/columnist"
		"portrait/sysadmin"
	message
		phrase "friendly author"
	message
		word
			"Visit 'https://github.com/endless-sky/endless-sky'"
		word
			" "
		word
			"to help us bring you great"
			"to improve our"
		word
			" "
		word
			"content"
			"features"
			"functionality"
			"software"
		word
			"."
```
For additional examples, see [`news.txt`](https://github.com/endless-sky/endless-sky/blob/master/data/human/news.txt).

## Modifying existing definitions

News can be modified by game [events](CreatingEvents) and plugins. All tokens can be modified, but the most likely use case will be to "activate" or "deactivate" a news source, by modifying its `location` element:
```c++
event "breaking news"
	news "terrorism on the rise"
		location
			government "Republic" "Syndicate"
```
This would update the already-defined News with the name "terrorism on the rise", *adding* to its existing location definition that it should match all planets with the Republic or Syndicate governments. If no such news definition exists, it will be created, but will be otherwise empty.

To "deactivate" a news source, two mechanisms exist. The original method was to introduce a `not` filter that cancels out the most restrictive element of the existing location. For example, to deactivate the above "terrorism on the rise" news source, we could use the following:
```c++
event "it's old news now"
	news "terrorism on the rise"
		location
			not government "Republic" "Syndicate"
```

Beginning in **v0.9.11**, use the "remove" modifier to completely erase the previous location filter. This has the bonus effect of allowing the definition of a new filter might have been incompatible with the original filter. For example
```c++
event "it's old news now"
	news "terrorism on the rise"
		remove location
event "trouble on the fringes"
	news "terrorism on the rise"
		location
			attributes "frontier"
```
The first event instructs the game to completely erase the previously defined location, resulting in an empty filter that prevents the news item from being displayed. The second event adds to the location filter, restricting it to planets and systems with the "frontier" attribute.

All 4 datafile elements can be used with the "remove" modifier as shown above, to completely erase the previous value. For portraits, one can also optionally list the specific file paths which should be removed, and the non-listed portraits will remain available.