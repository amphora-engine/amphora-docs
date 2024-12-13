# Drawing Text

## Creating Strings

Drawing text to the screen with Amphora requires two things: a font loaded using `resources.h`, and an `AmphoraString` struct.

The first thing you'll want to do is create a new `AmphoraString *` set to `NULL`:
<tabs>
    <tab title="C">
        <code-block lang="C">
            AmphoraString *my_string = NULL;
        </code-block>
    </tab>
    <tab title="C++">
        <code-block lang="C++">
            AmphoraString *my_string = nullptr;
        </code-block>
    </tab>
</tabs>

The `AmphoraString` pointer will be used with the `create_string` function which has the following signature:
```C
AmphoraString *create_string(AmphoraString **msg, const char *name, int pt, int x, int y, int order, SDL_Color color, const char *text, bool stationary);
```
Creating an `AmphoraString` with `create_string` will automatically add that string to the render list to be displayed.

### Description of parameters

- The `msg` parameter is a pointer to the `AmphoraString` pointer created previously.
In this case it would look like `&my_string`.
- The `name` parameter is the string name of the font to be used for the string.
This is the name specified in `resources.h`.
In the case of the example `resources.h`, this could be `"Roboto"`.
- The `pt` parameter is the font size.
- The `x` and `y` parameters control the position on screen of the string.
If `stationary` (explained later on this page) is set to false, these parameters are the absolute coordinates of the string and so will visually move as the camera moves.
If `stationary` is set to true, these parameters are the distance from the window edges of the string, and the string will always be at the same location on screen regardless of where the camera is.
With `stationary` set to true, positive `x` and `y` values will count from the left and top edges of the window to the string, and negative `x` and `y` values will count from the right and bottom edges of the window to the string.
- The `order` parameter controls the draw order and applied to strings and sprites.
Higher numbers will be drawn on top of lower numbers.
- The `color` parameter is the color of the text.
This can be any `SDL_Color`, but is most commonly a color defined in `colors.h`.
- The `text` parameter is the actual text to be drawn.
- The `stationary` parameter determines whether the string is stationary or not.
A stationary string will always be displayed at the same screen location no matter where the camera is.
This is commonly used for things like a score display or a timer, or text within a dialog box.
A non-stationary string is fixed to an actual coordinate in the game world, and would be commonly used for textual elements in the game environment (think billboards, signs, etc.).

## Freeing Strings

Freeing an `AmphoraString` when you no longer need it is accomplished with the `free_string` function which has the following signature:
```C
void free_string(AmphoraString **msg);
```
In our example, the call would be as follows:
```C
free_string(&my_string);
```
The `free_string` automatically sets the `AmphoraImage` pointer back to a null pointer for you after freeing the resources.

It is only necessary to call `free_string` on any strings you no longer want to be displayed, Amphora will automatically call `free_string` on any remaining strings as part of its shutdown routine.
