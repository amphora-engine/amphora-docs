# Getting Started

## Getting the Engine

Before developing your game, you'll need a copy of the Amphora engine.
Releases can be downloaded using the `amphora` command line utility, or you can clone the latest dev branch from GitHub and build it yourself (see [](Producing-Custom-Engine-Builds.md)).

## Creating a Project

Once you've obtained a copy of the engine, you'll use the `amphora` utility to create a new blank project:
```Bash
amphora new MyGame
```
Inside the project, the `src` directory is where code files are found, `include` is for headers, `content` is for any assets your game will need, and `config` is for configuration files.

Within `src/`, you'll find `Amphora.cpp` which is the entry point for your game.
This file is not intended to be modified, and should be left as-is, as it forms the translation layer between the engine and your game code.
The bulk of your game logic will be in `src/Scenes/`, though it's typical to have many directories under `src/`, (`src/Entities/`, `src/FX/`, etc.).

It is recommended to add `using namespace Amphora;` to the top of each source file to avoid having to prefix every engine class and function with `Amphora::`.

## Building the Project

Amphora uses CMake for builds.
Assuming you have CMake installed, you should be able to build and debug your project easily using Visual Studio or any other CMake-compatible IDE.

On Linux, you can also build from the command-line with the following commands:
```Bash
mkdir -p build
cd build
cmake ..
make
```
