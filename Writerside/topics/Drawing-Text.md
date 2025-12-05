# Drawing Text

## Creating Strings

Drawing text to the screen with Amphora requires two things: a font loaded using `Resources.txt`, and an `AmphoraString` struct.

The first thing you'll want to do is create a new `AmphoraString *`.
The `AmphoraString` pointer will be used with the `Amphora::CreateString` function which has the following signature:
```C
AmphoraString *Amphora::CreateString(const char *font_name, int pt, float x, float y, int order, SDL_Color color, bool stationary, const char *fmt, ...);
```
Creating an `AmphoraString` with `Amphora::CreateString` will automatically add that string to the render list to be displayed.

### Description of parameters

- The `font_name` parameter is the string name of the font to be used for the string.
This is the name specified in `Resources.txt`.
In the case of the example `Resources.txt`, this could be `"Roboto"`.
- The `pt` parameter is the font size.
- The `x` and `y` parameters control the position on screen of the string.
If `stationary` (explained later on this page) is set to false, these parameters are the absolute coordinates of the string and so will visually move as the camera moves.
If `stationary` is set to true, these parameters are the distance from the window edges of the string, and the string will always be at the same location on screen regardless of where the camera is.
With `stationary` set to true, positive `x` and `y` values will count from the left and top edges of the window to the string, and negative `x` and `y` values will count from the right and bottom edges of the window to the string.
- The `order` parameter controls the draw order and applied to strings and sprites.
Higher numbers will be drawn on top of lower numbers.
- The `color` parameter is the color of the text.
This can be any `SDL_Color`, but is most commonly a color defined in `Colors.txt`.
- The `stationary` parameter determines whether the string is stationary or not.
A stationary string will always be displayed at the same screen location no matter where the camera is.
This is commonly used for things like a score display or a timer, or text within a dialog box.
A non-stationary string is fixed to an actual coordinate in the game world, and would be commonly used for textual elements in the game environment (think billboards, signs, etc.).
- The `fmt` parameter and following is the actual text to be drawn as a format string.

## Updating String Text

You can change the text of an `AmphoraString` using the `Amphora::UpdateStringText` function:
```C
AmphoraString *Amphora::UpdateStringText(AmphoraString *msg, const char *fmt, ...);
```
This will overwrite the old text with the new format string provided.

## Changing the Number of Characters Displayed

You can change how many characters of a string are displayed with the `Amphora::UpdateStringCharsDisplayed` function:
```C
AmphoraString *Amphora::UpdateStringCharsDisplayed(AmphoraString *msg, size_t n);
```
This will display only the first 'n' characters of the string.

## Freeing Strings

Freeing an `AmphoraString` when you no longer need it is accomplished with the `Amphora::FreeString` function which has the following signature:
```C
void Amphora::FreeString(AmphoraString *msg);
```
In our example, the call would be as follows:
```C
Amphora::FreeString(my_string);
```
It is only necessary to call `Amphora::FreeString` on any strings you no longer want to be displayed, Amphora will automatically call `Amphora::FreeString` on any remaining strings as part of its scene destroy routine.

## The Typewriter Effect

Amphora includes a built-in typewriter effect that can be used by setting a string's number of characters displayed to 0, and then using `Amphora::TypeString`.
Amphora::TypeString optionally takes a callback function that will be called every time a character is displayed.
In practice, this looks as follows:
```c++
Amphora::UpdateStringCharsDisplayed(my_string, 0);
Amphora::TypeString(my_string, 125, [](int n, char c) {
    Amphora::PlaySFX("typewriter_key", -1, 0);
});
```
In the callback function, `n` is the character index being displayed, and `c` is the character being displayed.

You can also adjust the speed of a typewriter effect by calling `Amphora::SetStringTypeSpeed` with a new speed value.