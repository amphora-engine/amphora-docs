# Working with Binary Resources

Amphora builds static binaries with resources built into the game executable.
These resources are specified in the `resources.h` file using a similar x-macro pattern to what's seen in setting up keymaps and colors.

Each `LOAD*` macro looks roughly as follows:
```C
LOAD*(name, path) \
```
The `name` field will be used to refer to the resource in your game code, and the `path` is the relative path to the resource file.
The `LOAD*` macros are each of `LOADIMG`, `LOADFONT`, `LOADMAP`, `LOADSFX`, and `LOADMUSIC`.

This is how a completed `resourses.h` file might look:
```C
#define IMAGES \
	LOADIMG(Character, "../content/img/character.png") \
	LOADIMG(Objects, "../content/img/objects.png") \
	LOADIMG(Overworld, "../content/img/overworld.png")

#define FONTS \
	LOADFONT(Roboto, "../content/font/Roboto/Roboto-Regular.ttf")

#define MAPS \
	LOADMAP(Overworld, "../content/maps/overworld.tmj")

#define SFX \
	LOADSFX(leaves01, "../content/sfx/leaves01.ogg") \
	LOADSFX(leaves02, "../content/sfx/leaves02.ogg")

#define MUSIC \
	LOADMUSIC(forest, "../content/music/forest.ogg")
```
<warning>
Names must be unique within each resource type, but can be repeated across different resource types.
For example, you may only have one image resource called <i>Overworld</i>, but you may have both an image and a map resource called <i>Overworld</i>.
</warning>
