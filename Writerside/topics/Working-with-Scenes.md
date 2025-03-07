# Working with Scenes

Amphora uses the concept of scenes to manage game levels.
At its most basic, a scene is simply a collection of three functions: an init function, an update function, and a destroy function.
When a scene is loaded, its init function is run once, then its update function is run once per frame, and finally its destroy function is run when it's unloaded.

## Creating a Scene

Scenes are managed using the `scene_list.h` file and the same X-macro pattern we've scene before.
An example `scene_list.h` file could look as follows:

```C
#define SCENES \
    SCENE(MainMenu) \
    SCENE(Level1) \
    SCENE(Level2) \
    SCENE(Level3)
```

<warning>
The first scene in the list will always be loaded first when the game is launched.
</warning>

Typically each scene will have its own file where its functions will reside.
The functions require the following prototypes:

```C
void {SceneName}_Init(void);
void {SceneName}_Update(Uint64, const InputState *);
void {SceneName}_Destroy(void);
```

Somewhere above the functions you will also need to include the following macro:

```C
Amphora_BeginScene(name)
```

So a completed scene file for the MainMenu scene could look as so:

```C
#include "engine/amphora.h"

Amphora_BeginScene(MainMenu)

MainMenu_Init(void) {}
MainMenu_Update(Uint32 frame, const InputState *input_state) {}
MainMenu_Destroy(void) {}

```

## Loading Scenes

A scene is loaded using `Amphora_LoadScene(name)`.
This will call the current scene's destroy function, free other allocated resources, then call the new scene's init function and begin running its update function.

For example, to load level 1, it'd be as simple as:

```C
Amphora_LoadScene("Level1");
```
