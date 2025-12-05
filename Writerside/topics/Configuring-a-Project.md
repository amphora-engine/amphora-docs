# Configuring a Project

Amphora uses a combination of plain-text configuration files and headers to control game setup.
These files can be found in `config`.

- `Config.h`: Sets default window values, as well as the game name and author, and the API version.
- `Keymap.txt`: Defines the default keymap for the game.
- `Colors.txt`: Defines custom colors for use in your game.
- `Resources.txt`: Defines the resources that will be loaded into the game.
- `SceneList.txt`: Defines the scenes that will be loaded into the game.

The plain text files follow a simple macro-like syntax and are parsed at build time..

### Default Keymap

To set your default keymap, define each key in the `Keymap.txt` file as follows:
```C
KMAP(action, key, gamepad_button)
```
For each KMAP line, `action` is the name of the action being defined (ie. jump, dash, etc.).
The key will be the name of the key as it exists in SDL's `SDLK_*` enum, without the `SDLK_` prefix.
The gamepad_button follows the same pattern as the key, but with SDL's `SDL_CONTROLLER_BUTTON_*` enum.

An example completed keymap section could look as follows:
```C
KMAP(jump, SPACE, A)
KMAP(dash, LSHIFT, B)
KMAP(left, a, DPAD_LEFT)
KMAP(right, d, DPAD_RIGHT)
KMAP(up, w, DPAD_UP)
KMAP(down, s, DPAD_DOWN)
```
Note that this only sets the default keymap, it is possible to remap keys, and the procedure for doing so is documented later on in the manual.

## Custom Colors

The `Colors.txt` file provides an easy place to define custom colors for use in your game.
It follows a similar pattern as the keymap, where each color line looks as follows:
```C
COLOR(name, red, green, blue)
```
A completed colors section could look like so:
```C
COLOR(Black, 0x00, 0x00, 0x00)
COLOR(White, 0xff, 0xff, 0xff)
COLOR(Red, 0xff, 0x00, 0x00)
COLOR(Green, 0x00, 0xff, 0x00)
COLOR(Blue, 0x00, 0x00, 0xff)
COLOR(Sky, 0x87, 0xce, 0xeb)
```
These colors can then be referenced by name within the `Amphora::Colors` namespace anywhere in your game that calls for an `SDL_Color`.

For example:
```C++
Amphora::SetBG(Amphora::Colors::Sky);
```