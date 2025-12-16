# Drawing Text

## Creating Strings

Drawing text to the screen with Amphora requires two things: a font loaded using `Resources.txt`, and a `Amphora::String` object.

The first thing you'll want to do is create a new `Amphora::String`.
The `Amphora::String` object's constructor has the following signature:
```c++
String Amphora::String(const char *font_name, int pt, float x, float y, int order, SDL_Color color, bool stationary, bool transient, const char *fmt, ...);
```
Creating an `Amphora::String` will automatically add that string to the render list to be displayed.

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
- The `transient` parameter determines the behavior of the string when it is hidden.
A transient string's memory will be freed automatically when it is hidden, whereas a non-transient string will remain in memory until the next scene change.
Typically a transient string could be used for dialog boxes or other temporary text, whereas a non-transient string would be used for permanent text like a score display or timer.
- The `fmt` parameter and following is the actual text to be drawn as a format string.

## Updating String Text

You can change the text of an `Amphora::String` using the `Amphora::String::UpdateText` member function:
```c++
void Amphora::String::UpdateText(const char *fmt, ...);
```
This will overwrite the old text with the new format string provided.

## Changing the Number of Characters Displayed

You can change how many characters of a string are displayed with the `Amphora::SetCharsDisplayed` member function:
```c++
void Amphora::String::SetCharsDisplayed(size_t n);
```
This will display only the first 'n' characters of the string.

## Freeing Strings

Freeing an `Amphora::String` generally happens automatically, but the process differs based on whether the string is transient or not.
If a string is transient, it will be freed automatically when it is hidden with `Amphora::String::Hide`.
If a string is not transient, it won't be freed until the next scene change.
Any transient string still visible when a scene changen occurs will also be freed automatically.

```c++
void Amphora::String::HideString();
```

## The Typewriter Effect

Amphora includes a built-in typewriter effect that can be used with `Amphora::TypeString`.
Amphora::TypeString optionally takes a callback function that will be called every time a character is displayed.

```c++
TypewriterStatus Amphora::String::Typewriter(int speed, void(*callback)(int, char) = nullptr);
```

In practice, this looks as follows:
```c++
my_string.Typewriter(125, [](int n, char c) {
    Amphora::PlaySFX("typewriter_key", -1, 0);
});
```
In the callback function, `n` is the character index being displayed, and `c` is the character being displayed.

You can also adjust the speed of a typewriter effect by calling `Amphora::SetStringTypeSpeed` with a new speed value.