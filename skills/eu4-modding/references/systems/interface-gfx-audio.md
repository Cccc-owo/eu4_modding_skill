# Interface, Graphics, And Audio

Use this for UI, sprites, event pictures, mission icons, flags, music, and sound.

## Interface

Interface layout files use `.gui` under `interface/`. Custom GUI backend script uses `.txt` under `common/custom_gui/`.

Containers group elements and often bind to internal code:

- `windowType`
- `scrollbarType`

Elements include:

- `iconType`
- `instantTextBoxType`
- `guiButtonType`
- `smoothListboxType`
- `listboxType`
- `checkboxType`
- `editBoxType`

Common `windowType` attributes:

```txt
windowType = {
    name = "mymod_window"
    position = { x = 0 y = 140 }
    size = { x = 500 y = 300 }
    orientation = "UPPER_LEFT"
    moveable = 0
}
```

Many interface elements are hardcoded/internal. If a UI target is not exposed by a proven vanilla window or supported custom GUI hook, do not invent a new widget contract; reuse an exposed element or stop at the script layer.

## Mission UI

Default mission UI has five columns. More columns require interface edits, commonly around the missions grid/listbox definitions. Mission icons are sprite names referenced from mission definitions.

Custom mission icon:

```txt
spriteTypes = {
    spriteType = {
        name = "mission_mymod_icon"
        texturefile = "gfx//interface//missions//mission_mymod_icon.dds"
    }
}
```

Then:

```txt
icon = mission_mymod_icon
```

## Graphical Assets

`.gfx` files bind asset names to files:

```txt
spriteTypes = {
    spriteType = {
        name = "GFX_mymod_button"
        texturefile = "gfx//interface//mymod_button.dds"
        noOfFrames = 1
    }
}
```

Fields commonly used:

- `name`: script key.
- `texturefile`: path relative to EU4/mod root.
- `noOfFrames`: for sprite sheets.
- `effectFile`: shader/effect file.
- `alwaystransparent`: alpha-channel click behavior.
- `animation = { ... }`: scrolling, rotating, pulsing effects.

Many graphical assets can be tag- or culture-specific via names such as `GFX_ENG_<name>` or `GFX_african_<name>`.

## Event Pictures

Events reference picture keys:

```txt
picture = DIPLOMACY_eventPicture
```

To add a custom event picture, declare the sprite in `.gfx` and place the DDS at the referenced path. Keep aspect ratio and size close to vanilla event pictures unless also editing UI.

## Flags

Country flags are keyed by tag, typically:

```txt
gfx/flags/ABC.tga
```

When creating a new country, missing or invalid flags can cause visual issues.

## Music And Sound

Music commonly needs:

```txt
music/songs.txt
music/<track>.ogg
music/music.asset
```

Sound commonly needs:

```txt
sound/all_sounds.asset
sound/<sound>.wav
```

Use existing declarations as templates. Paths and names must match exactly.

## Pitfalls

- Sprite declared as `GFX_mymod_x` but referenced as `mymod_x`, or vice versa.
- DDS path mismatch.
- Editing `.gui` when a scripted or `.gfx` change would be enough.
- Using tag-specific asset naming accidentally and hiding the asset for other tags.
- Forgetting UI localisation or tooltip keys.
