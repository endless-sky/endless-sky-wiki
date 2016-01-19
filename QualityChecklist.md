If you're looking to write content that will eventually become part of the main game, here are a few guidelines to keep in mind:

All graphics:

- [ ] Use a [Debian-compatible license](https://wiki.debian.org/DFSGLicenses). (Ideally, one of the licenses the existing artwork uses.)
- [ ] Provide an @2x version for [high-dpi monitors](https://github.com/endless-sky/endless-sky-high-dpi).
- [ ] Provide access to [all files used](https://drive.google.com/open?id=0B9aK8dG39P29fkdBeUJjSXJYVDdjMEpkOXh3T1NDekFYaTEtbkdTdzVwX2NTUWVVT3BUWVk) to create the graphics (Blender models, GIMP files, etc.) so someone else can edit them later if necessary.

Ships:

- [ ] Make sure the sprite is a single connected component so that [collision detection](https://github.com/endless-sky/endless-sky/wiki/CollisionDetection) works properly.
- [ ] Define sensible [hardpoints](http://endless-sky.github.io/ship_builder.html).
- [ ] Balance the ship's capabilities with other ships in the same price range.
- [ ] 3D renders should be post-processed to look grungier and less artificial.
- [ ] Colored sections should span the range from red to yellow so faction-specific color swizzling will work properly.

Outfits:

- [ ] Provide outfit thumbnails at normal and @2x resolution.
- [ ] If the outfit is a secondary weapon, provide normal and @2x icons.
- [ ] Decide which outfitters will sell the outfit.
- [ ] An outfit's cost should reflect how good it is compared to other outfits of the same size.
- [ ] Where possible, have outfits be better in some ways and worse in others, compared to existing ones, rather than better in every single way.

Landscape photos:

- [ ] Should be photos or photo-realistic.
- [ ] Include both sky and ground in the image.
- [ ] Remove people from the image if possible.
- [ ] Photos should be realistic (no exaggerated color / contrast).
- [ ] Avoid foreground objects (trees, houses, people, etc.).

New star systems:

- [ ] The planet sprites with city lights should only be used for inhabited planets.
- [ ] Planet descriptions should roughly track whether they're in the hot or the cold range of the habitable zone.
- [ ] System name text should not overlap when the map is viewed at 100%.
- [ ] Try to put something funny or interesting in each planet description.
- [ ] Planet descriptions must be short enough to fit in the landing UI.
- [ ] New systems that are inside human space won't be accepted into the main game data unless there's a good plot reason for them.

New missions:

- [ ] Text should be as concise as possible. Remember this is a game, not a novel.
- [ ] The "yes, I want this mission" choice in a conversation should usually come first in the list (so it is the default).
- [ ] Conversations should not imply that the player is a certain gender or use gendered pronouns for the player (call them "kid" or "Captain" or `<first>` instead).

New sounds:

- [ ] Use a [Debian-compatible license](https://wiki.debian.org/DFSGLicenses).
- [ ] Provide a copy of your raw working files used to create the sound, if possible.