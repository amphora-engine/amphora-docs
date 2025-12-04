# Producing Custom Engine Builds

## Prerequisites

Before getting started with Amphora Engine, you will need to install Microsoft's vcpkg, as this is how Amphora manages its dependencies.
Instructions on how to do this can be found [here](https://learn.microsoft.com/en-us/vcpkg/get_started/get-started).
You'll need to follow the steps up through setting the VCPKG_ROOT environment variable.

### Note for Linux Builds {collapsible="true"}

If you intend to use Amphora's audio system on Linux, you'll need to ensure that the headers for your sound system are installed _before_ vcpkg builds SDL2_mixer.
This can be accomplished using the following command on Debian/Ubuntu based systems:
```Bash
sudo apt install libasound2-dev libpulse-dev
```

## Building the Engine

Amphora uses CMake for builds.
Assuming you have CMake and vcpkg installed, and the VCPKG_ROOT environment variable set, you should be able to build and debug your project easily using Visual Studio or any other CMake-compatible IDE.

On Linux, you can also build from the command-line with the following commands:
```Bash
mkdir -p build
cd build
cake ..
