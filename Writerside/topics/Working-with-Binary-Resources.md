# Working with Resources

Your game's resources are specified in the `Resources.txt` file using a similar pattern to what's seen in setting up keymaps and colors.

Each line in the file will look roughly as follows:
```C
LOAD*(name, path)
```
The `name` field will be used to refer to the resource in your game code, and the `path` is the relative path to the resource file.
Here, `LOAD*` will be one of `LOADIMG`, `LOADFONT`, `LOADMAP`, `LOADSFX`, or `LOADMUSIC` depending on the type of resource you're loading.

This is how a completed `Resources.txt` file might look:
```C
LOADIMG(Character, "content/img/character.png")
LOADIMG(Objects, "content/img/objects.png")
LOADIMG(Overworld, "content/img/overworld.png")

LOADFONT(Roboto, "content/font/Roboto/Roboto-Regular.ttf")

LOADMAP(Overworld, "content/maps/overworld.tmj")

LOADSFX(leaves01, "content/sfx/leaves01.ogg")
LOADSFX(leaves02, "content/sfx/leaves02.ogg")

LOADMUSIC(forest, "content/music/forest.ogg")
```
<warning>
Names must be unique within each resource type, but can be repeated across different resource types.
For example, you may only have one image resource called <i>Overworld</i>, but you may have both an image and a map resource called <i>Overworld</i>.
</warning>
