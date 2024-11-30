# Configuring a Project

## config.h

Each Amphora game comes with a config file called `config.h` located in `include`.
Most options are configured in the form of macro definitions (ie. the framerate and window size).
Certain components of Amphora can be disabled using this file, specifically the audio system, TTF fonts, and tilemaps.
Disabling all of these systems would be done like so:
```C
#define DISABLE_MIXER
#define DISABLE_FONTS
#define DISABLE_TILEMAP
```
Each of these lines disables its corresponding system.

### Keymap

The keymap section is set up as an x-macro definition.
Underneath the `DEFAULT_KEYMAP` definition, you can define up to 64 mapped actions that look as follows:
```C
KMAP(action, key, gamepad_button) \
```
For each KMAP line, `action` is the name of the action being defined (ie. jump, dash, etc.).
The key will be the name of the key as it exists in SDL's `SDLK_*` enum, without the `SDLK_` prefix.
The gamepad_button follows the same pattern as the key, but with SDL's `SDL_CONTROLLER_BUTTON_*` enum.
<warning>Each KMAP line must end with a backslash <i>except</i> for the final one</warning>

An example completed keymap section could look as follows:
```C
#define DEFAULT_KEYMAP \
    KMAP(jump, SPACE, A) \
    KMAP(dash, LSHIFT, B) \
    KMAP(left, a, DPAD_LEFT) \
    KMAP(right, d, DPAD_RIGHT) \
    KMAP(up, w, DPAD_UP) \
    KMAP(down, s, DPAD_DOWN)
```

### Expert Options {collapsible="true"}

If you'd like to access Amphora's internal struct definitions and functions within your game code, add the following line to `config.h`:
```C
#define EXPOSE_INTERNAL_DEFINITIONS_AND_FUNCTIONS
```
<warning>
This is not recommended and highly unsupported, and there are a few things to note about operating in this mode:

- This does not expose _everything_.
If you look through Amphora's sources, you'll see functions split up into three categories within each compilation unit: public, internal, and private.
This mode exposes all internal functions, functions that are private will remain private to each compilation unit.
- The internal functions do not include any inline documentation like the public functions do.
- C++ is not supported when in this mode.
Your game sources that include `amphora.h` must be all in C.
The internal functions are not written or tested with C++ in mind, and while it's possible that it may work to wrap up each block of prototypes with `extern "C" {}` within the internal headers, this will likely not be done officially because you shouldn't be using this mode. 
</warning>

## colors.h

The `colors.h` file provides an easy place to define custom colors for use in your game.
It follows the same x-macro pattern as the keymap, where each color line looks as follows:
```C
COLOR(name, red, green, blue) \
```
A completed colors section could look like so:
```C
#define COLORS \
    COLOR(red, 0xff, 0x00, 0x00) \
    COLOR(green, 0x00, 0xff, 0x00) \
    COLOR(blue, 0x00, 0x00, 0xff) \
    COLOR(sky, 0x87, 0xce, 0xeb)
```
These colors can then be referenced by name anywhere in your game that calls for an `SDL_Color`.
Just don't modify the lines below the `COLORS` definition block!
