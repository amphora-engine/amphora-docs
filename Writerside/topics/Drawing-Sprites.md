# Drawing Sprites

Sprites in Amphora consist of two parts: an `AmphoraImage` pointer, and any number of framesets.

The `AmphoraImage` contains data such as the spritesheet resource to pull from, the coordinates, the scale factor, and the flip state of the current sprite.

A frameset describes an animated state that an `AmphoraImage` can be in.
Each frameset specifies a unique name, the frames to be used in its animation, and the delay between animation frames.
An object in your game might have many framesets attached to it, for example, idle, walking, jumping, falling, and attacking would all be separate framesets, each describing the animation for its state.

## Creating Sprites

First, you'll create an `AmphoraImage` pointer and ensure it's set to `NULL`.

<tabs>
    <tab title="C">
        <code-block lang="C">
            AmphoraImage *my_img = NULL;
        </code-block>
    </tab>
    <tab title="C++">
        <code-block lang="C++">
            AmphoraImage *my_img = nullptr;
        </code-block>
    </tab>
</tabs>

Sprites are created using the `Amphora_CreateSprite` function:
```C
AmphoraImage *Amphora_CreateSprite(const char *image_name, float x, float y, float scale, bool flip, bool stationary, Sint32 order);
```
Creating an `AmphoraImage` with `Amphora_CreateSprite` will automatically add it to the render list to be displayed.

If `Amphora_CreateSprite` succeeds, the passed in `AmphoraImage` pointer will be set to the newly created `AmphoraImage`, and the pointer will be returned as well.
If it fails, it will return `NULL` and the supplied pointer will not be modified.

### Description of Parameters {id="params_create_sprite"}

- The `spr` parameter is a pointer to an `AmphoraImage` pointer.
This pointer should be `NULL`, otherwise the function will simply return it as is and not create the new `AmphoraImage`.
- The `image_name` parameter is the name of the image resource specified in `resources.h`.
The `AmphoraImage` does not contain any texture data itself, it uses the image supplied here and draws portions of it based on the coordinates specified in a frameset.
- The `x` and `y` parameters determine the initial position of the `AmphoraImage`.
This will be the upper left corner.
If `stationary` is true, these values will be the distance from the window/screen edge to the image, with positive x and y values starting from the left and top edges, and negative x and y values starting from the right and bottom edges.
- The `scale` parameter is used to scale the image drawn.
For example, a scale parameter of 2 would cause the image to be drawn with its width and heights doubled.
- The `flip` parameter controls whether or not the image is drawn flipped horizontally.
- The `stationary` parameter determines whether the image should be stationary.
Similarly to strings, a stationary image is positioned relative to the window/screen edges and its position on screen will not change.
For example, a health display would be stationary, an enemy monster would not.
- The `order` parameter controls the draw order of all drawable objects, with higher numbers drawing on top of lower numbers.

## Creating Framesets

Once an `AmphoraImage` is created, you'll need to add a frameset to it.
This is done with the `Amphora_AddFrameset` function:
```C
void Amphora_AddFrameset(AmphoraImage *spr, const char *name, Sint32 sx, Sint32 sy, Sint32 w, Sint32 h, float off_x, float off_y, Uint16 num_frames, Uint16 delay);
```

### Description of Parameters {id="params_frameset"}

- The `spr` parameter to which the created frameset should be attached.
- The `name` parameter is the name of the frameset to be created.
This must be unique among framesets attached to any given `AmphoraImage`, but different `AmphoraImage` instances can have framesets with identical names (ie. an `AmpohraImage` player1 cannot have two framesets both called idle, but player1 and player2 can each have a frameset called idle).
- The `sx` and `sy` parameters are the upper left pixel of the first frame of the animation described by the frameset on the spritesheet named in the `AmphoraImage` `name` parameter.
- The `w` and `h` parameters are the width and height of the frames for the described animation.
<warning>
In any given animation, each frame must be of the same dimensions
</warning>
- The `off_x` and `off_y` parameters specify a pixel offset to be applied to the frames in an animation.
These numbers will be subtracted from the position of the `AmphoraImage`.
Typically, these will be used to maintain the relative center of a sprite when switching between framesets of different sizes.
For example, a sprite's idle animation may be 16x32 pixels, but the attack animation might be 32x32 pixels to accommodate a sword swing.
In this case, switching from an idle state to an attack state would move the visible image to the right.
One would use `off_x` here to maintain the proper position when switching between the framesets.
- The `num_frames` parameter specifies the  number of frames in the animation.
These will be read sequentially, left-to-right from the spritesheet, starting from the initial frame specified by `sx`, `sy`, `w`, and `h`.
If your frameset is a static image that is not animated, you would set this to 1.
- The `delay` parameter specifies the number of milliseconds between each animation frame change.
If your frameset is a static image that is not animated, this value does not matter and can just be set to 0.

## Selecting Framesets

There are two methods to set the active frameset for an `AmphoraImage`: `Amphora_SetFrameset`, and `Amphora_PlayOneshot`.
```C
void Amphora_SetFrameset(AmphoraImage *spr, const char *name);
void Amphora_PlayOneshot(AmphoraImage *spr, const char *name, void (*callback)(void));
```
In both of these, `spr` is the `AmphoraImage` and `name` is the name of the frameset that's previously been created.

The difference between the two is that `Amphora_SetFrameset` will loop the animation, while `Amphora_PlayOneshot` will play the animation once, holding on the last frame, and will execute a supplied callback function once the animation finishes.
This callback function is a standard C function pointer to a function that takes no parameters and returns void, or it can be a non-trapping lambda if using C++.
If you do not wish to execute a callback function, pass `NULL` in C or `nullptr` in C++.

## Freeing Sprites

Freeing sprites is done with the `Amphora_FreeSprite` function:
```C
void free_sprite(AmphoraImage *spr);
```
This will free all resources associated the specified `AmphoraImage` and reset the pointer to `NULL`.

All existing images are freed automatically on exit, so it is only necessary to manually free images that you no longer need (ie. an enemy that the player defeated).
