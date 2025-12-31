# Drawing Sprites

Sprites in Amphora consist of two parts: a `Amphora::Sprite` object, and any number of framesets.

The `Amphora::Sprite` contains data such as the spritesheet resource to pull from, the coordinates, the scale factor, and the flip state of the current sprite.

A frameset describes an animated state that a `Amphora::Sprite` can be in.
Each frameset specifies a unique name, the frames to be used in its animation, and the delay between animation frames.
An object in your game might have many framesets attached to it, for example, idle, walking, jumping, falling, and attacking would all be separate framesets, each describing the animation for its state.

## Creating Sprites

First, you'll create a `Amphora::Sprite` object.
```c++
Sprite Amphora::Sprite(const std::string& image_name, float x, float y, float scale, bool flip, bool stationary, bool transient, int order);
```
Creating a `Sprite` will automatically add it to the render list to be displayed.

### Description of Parameters {id="params_create_sprite"}

- The `image_name` parameter is the name of the image resource specified in `Resources.txt`.
The `Amphora::Sprite` does not contain any texture data itself, it uses the image supplied here and draws portions of it based on the coordinates specified in a frameset.
- The `x` and `y` parameters determine the initial position of the `Amphora::Sprite`.
This will be the upper left corner.
If `stationary` is true, these values will be the distance from the window/screen edge to the image, with positive x and y values starting from the left and top edges, and negative x and y values starting from the right and bottom edges.
- The `scale` parameter is used to scale the image drawn.
For example, a scale parameter of 2 would cause the image to be drawn with its width and heights doubled.
- The `flip` parameter controls whether or not the image is drawn flipped horizontally.
- The `stationary` parameter determines whether the image should be stationary.
Similarly to strings, a stationary image is positioned relative to the window/screen edges and its position on screen will not change.
For example, a health display would be stationary, an enemy monster would not.
- The `transient` parameter works identically to how it works for strings; it controls whether hiding an image frees its memory or not.
- The `order` parameter controls the draw order of all drawable objects, with higher numbers drawing on top of lower numbers.

## Creating Framesets

Once a `Sprite` is created, you'll need to add at least one frameset to it.
This is done with the `Amphora::Sprite::AddFrameset` function:
```C
void Amphora::Sprite::AddFrameset(const std::string& name, int sx, int sy, int w, int h, float off_x, float off_y, int num_frames, int delay, const std::string& override_img = "");
```

### Description of Parameters {id="params_frameset"}

- The `name` parameter is the name of the frameset to be created.
This must be unique among framesets attached to any given `Amphora::Sprite`, but different `Amphora::Sprite` instances can have framesets with identical names (ie. an `Amphora::Sprite` player1 cannot have two framesets both called idle, but player1 and player2 can each have a frameset called idle).
- The `sx` and `sy` parameters are the upper left pixel of the first frame of the animation described by the frameset on the spritesheet named in the `Amphora::Sprite` `name` parameter.
- The `w` and `h` parameters are the width and height of the frames for the described animation.
<warning>
In any given animation, each frame must be of the same dimensions
</warning>
- The `off_x` and `off_y` parameters specify a pixel offset to be applied to the frames in an animation.
These numbers will be subtracted from the position of the `Amphora::Sprite`.
Typically, these will be used to maintain the relative center of a sprite when switching between framesets of different sizes.
For example, a sprite's idle animation may be 16x32 pixels, but the attack animation might be 32x32 pixels to accommodate a sword swing.
In this case, switching from an idle state to an attack state would move the visible image to the right.
One would use `off_x` here to maintain the proper position when switching between the framesets.
- The `num_frames` parameter specifies the  number of frames in the animation.
These will be read sequentially, left-to-right from the spritesheet, starting from the initial frame specified by `sx`, `sy`, `w`, and `h`.
If your frameset is a static image that is not animated, you would set this to 1.
- The `delay` parameter specifies the number of milliseconds between each animation frame change.
If your frameset is a static image that is not animated, this value does not matter and can just be set to 0.
- The `override_img` parameter is the name of an image resource to use instead of the one specified when creating the `Amphora::Sprite`.
If this isn't specified, the image specified when creating the `Amphora::Sprite` will be used.

## Selecting Framesets

There are two methods to set the active frameset for a `Sprite`: `Amphora::Sprite::SetFrameset`, and `Amphora::Sprite::PlayOneshot`.
```c++
void Amphora::Sprite::SetFrameset(const std::string& name);
void Amphora::Sprite::PlayOneshot(const std::string& name, void (*callback)(void) = nullptr);
```
In both of these, `name` is the name of the frameset that's previously been created.

The difference between the two is that `Amphora::Sprite::SetFrameset` will loop the animation, while `Amphora::Sprite::PlayOneshot` will play the animation once, holding on the last frame, and will execute a supplied callback function once the animation finishes.
This callback function takes no parameters and returns void.

## Freeing Sprites

Freeing a sprite generally happens automatically and occurs in the same fashion as freeing a string.
If the sprite is transient, it will be freed automatically when it's hidden with `Amphora::Sprite::Hide`.
Otherwise, it will be freed automatically when the scene is changed.
Any transient sprites will also be freed automatically on a scene change.
