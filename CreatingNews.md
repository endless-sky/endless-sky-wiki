## Introduction

Version 0.9.9 adds a "news system" where people in the spaceport will tell you something about their life on that planet. The goal of this system is to make the spaceport feel more like a real place, populated by real people, rather than just a button you click on to check for missions. Each time you click the spaceport button, a "news" item is randomly generated, in the same way that a ship displays a randomly generated message each time you hail it.

## Portraits

Portraits should be PNG images with the person's silhouette "cut out," designed to look good when displayed over a dark grey background. Source images should have enough resolution to look sharp at 480 x 480 pixels. For the sake of consistency:

* Each person's face should be half the width of the image. (Or slightly less if they have a big hat or lots of hair.)
* Photos with fancy "color filters" should not be used, and the light intensity curves should be adjusted to avoid washed-out or overly-contrasty images.
* Anything obviously 21st-century (like brand logos) should be avoided.
* If possible, the image should just contain the person's face and shoulders, not their hands, etc.
* It's okay to crop a person's shoulders on the right and left, and their hair if necessary, but their whole head should fit inside the image.

To make it possible to tweak the masking, colors, etc. after the fact, you should save a GIMP file for each image. That will also ensure that the name of the source image is recorded (assuming you keep it as the layer name in the XCF file).

For masking out people from an image, I find it helpful to do the following:

* Split the image into its RGB color components.
* Pick the component where the person's head (and in particular, their hair, which is the trickiest part) is as different a shade as possible from the background.
* Adjust the curves to make the head white and the background black in the image.
* Manually fill in sections of the silhouette as necessary.
* Add a layer mask to the original layer and paste the silhouette into it.
* Apply the final cropping and scaling (to 480 x 480).
* Test the image over a black background. If the original background was light-colored, you may need to darken the edges to avoid the background color bleeding through. Or, you can "erode" the mask (Filters -> Generic -> Erode) slightly to eliminate fringe pixels.

## People

In-flight messages from other ships already give a lot of flavor from a pilot's perspective. To keep the news system distinctive, it should focus on the life of planet-bound people who rarely, if ever, venture out into space. Here are some ideas for people you might talk to:

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
* street perfomer
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