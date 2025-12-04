# Working with Scenes

Amphora uses the concept of scenes to manage game levels.
At its most basic, a scene is simply a C++ class implementing three methods: an init function, an update function, and a destroy function.
When a scene is loaded, its init function is run once, then its update function is run once per frame, and finally its destroy function is run when it's unloaded.

## Creating a Scene

Scenes are declared in the `SceneList.txt` file.
An example `SceneList.txt` file could look as follows:

```C
SCENE(MainMenu)
SCENE(Level1)
SCENE(Level2)
SCENE(Level3)
```

<warning>
The first scene in the list will always be loaded first when the game is launched.
</warning>

Each scene will have its own class which must take the following form:
```c++
class SceneName final : public Scene
{
private:
public:
    static SceneName& instance();
    
    void init() override {}
    void update(Uint32 frame, const InputState *key_actions) override {}
    void destroy() override {}
}

Amphora_RegisterScene(SceneName);
```
<warning>
The `static SceneName& instance();` line must exist as is to ensure that scenes are not incorrectly loaded multiple times.
The implementation of this function will be generated automatically by the build system.
</warning>
<warning>
The `Amphora_RegisterScene` macro must be called for each scene class, otherwise the engine will not be able to load them.
</warning>

## Loading Scenes

A scene is loaded using `Amphora_LoadScene(name)`.
This will call the current scene's destroy function, free other allocated resources, then call the new scene's init function and begin running its update function.

For example, to load level 1, it'd be as simple as:

```c++
Amphora::LoadScene("Level1");
```

You can define a smooth fade between scenes using `Amphora::SetSceneFadeParameters(duration, color)`, where duration is the time in milliseconds for the fade to take place, and color is an `SDL_Color` struct representing the color to fade to.
So for example, to set a fade-to-black transition with a half second duration, you'd call:
```c++
Amphora::SetSceneFadeParameters(500, Amphora::Colors::Black);
```

During the fade, the current scene's update function will be paused, and [the event system](The-Event-System.md) will be unavailable for use until the fade is complete, though already-registered events will continue to be processed.

<warning>
The fade parameters are global and will affect all scenes, so this function generally only needs to be called once in the first scene's init function.
</warning>