# Getting Started

## Prerequisites

Before getting started with Amphora, you will need to install Microsoft's vcpkg, as this is how Amphora manages its dependencies.
Instructions on how to do this can be found [here](https://learn.microsoft.com/en-us/vcpkg/get_started/get-started).
You'll need to follow the steps up through setting the VCPKG_ROOT environment variable.

### Note for Linux Builds {collapsible="true"}

If you intend to use Amphora's audio system on Linux, you'll need to ensure that the headers for your sound system are installed _before_ vcpkg builds SDL2_mixer.
This can be accomplished using the following command on Debian/Ubuntu based systems:
```Bash
sudo apt install libasound2-dev libpulse-dev
```

## Getting the Engine

Amphora is distributed in source form.
Releases can be downloaded as a tarball from the GitHub releases, or you can clone the latest dev branch:
```Bash
git clone https://github.com/calebstein1/amphora
```
The `src` directory is where code files are found, `include` is for headers, and `content` is for any assets your game will need.
The engine source and header files are located in `src/engine` and `include/engine` respectively.

Your game files should exist outside of the `engine` directories.
Typically, you'll have multiple directories under `src` with which to organize your files (ie, `Scenes`, `Utils`, etc).

## Building the Project

Amphora uses CMake for builds.
Assuming you have CMake and vcpkg installed, and the VCPKG_ROOT environment variable set, you should be able to build and debug your project easily using Visual Studio or any other CMake-compatible IDE.

On Linux, you can also build from the command-line with the following commands:
```Bash
mkdir -p build
cd build
cake ..
make
```
