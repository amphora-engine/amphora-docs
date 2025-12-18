# Drawing a Tilemap

Amphora natively supports [Tiled](https://www.mapeditor.org/) tilemaps in the TMJ JSON format.

## Creating a Tilemap

Both orthogonal and isometric tilemaps are supported, hexagonal maps are not supported at this time.
When you create a new tilemap in Tiled, ensure that your tileset is named identically to an image resource in your `Resources.txt`, and that the tileset comes from that same image.
You should not embed the tileset in the tilemap; the engine will automatically load it from the image based on the name.

When maps are loaded, each map layer will be rendered from bottom to top with 100 "order" units between each layer.
For example, a map with 3 layers would be rendered as follows:
1. Ground - Order 0
2. Buildings - Order 100
3. Trees - Order 200

If you wanted your player character to be rendered in front of the buildings but behind the trees, you would set the order of the player to a value between 101 and 199 inclusive (the render order of two objects with identical order values is not guaranteed).

Object groups are supported using rectangles only and are used to detect collisions, the process for which is described further down this page.

## Using Tilemaps

All tilemaps must be declared in the `Resources.txt` file as described in [](Working-with-Binary-Resources.md).
These maps can then be loaded using `Amphora::SetMap` with the following signature:
```C++
void Amphora::SetMap(const char *name, float scale);
```

### Description of Parameters

- `name` - The name of the tilemap to load as declared in `Resources.txt`.
- `scale` - The scale of the tilemap.

## Setting a Background Color

You can set the screen background color using `Amphora::SetBGColor(AmphoraColor color);`

This would commonly be used to set the color of the sky in a side-scroller, or the color of the ground in a top-down game.

## Detecting Collisions

A collision between a `Sprite` object and a tilemap object layer can be checked using `Amphora::Sprite::CheckObjectGroupCollision(const char *name)`.
```C++
Amphora::Collision Amphora::Sprite::CheckObjectGroupCollision(const char *name);
```

This will return an Amphora::Collision enum class value indicating whether the sprite collided with any objects in the object group.

Amphora::Collision can be any one of the following values:
- `Amphora::Collision::None`
- `Amphora::Collision::Left`
- `Amphora::Collision::Right`
- `Amphora::Collision::Top`
- `Amphora::Collision::Bottom`

The value returned will be the side of the object group that was hit, or `Amphora::Collision::None` if no collision occurred.
