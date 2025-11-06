# Image formats supported in Endless Sky

The current file formats supported for simple images are `.png` and `.jpg`/`.jpeg`/`.jpe`. Since **v. 0.10.15**, you can also use `.avif`/`.avifs` files to store all frames of an animation. In this case, you should only have one file matching the sprite name.

## File names

The general naming pattern of sprites used in Endless Sky looks like this:
```html
<sprite-name><blending-mode><frame-number><swizzle-mask-flag><size>.<extension>
```

* `<sprite-name>` is the common stem of all filenames that are combined into a single in-game sprite. This is the identifier that should be used when referencing the sprite in [game data](SpriteData).

* `<blending-mode>` is a single-character specifier for transparency handling. Can be omitted if there is no frame number in the name. See the [Blending Modes](BlendingModes#input-files) page for details.

* `<frame-number>` is the index (starting from 0) of the sprite frame this file contains. Can be omitted if the sprite has only one frame.

* `<swizzle-mask-flag>` is a `@sw` string. If present, it indicates that this file is not normal sprite data, but a swizzle mask.

* `<size>` can be omitted, or one of the following:
  * `@2x`, which means the file is a high-resolution sprite and should be used when a high-DPI display is detected by the game or main zoom factor is high enough,
  * `@1x`, indicating that it's important to keep the sprite in its original resolution (for example when the sprite represents a UI element) even when sprite reduction is enabled **(v. 0.10.17)**.

* `<extension>` is one of the supported image format extensions (listed above).
