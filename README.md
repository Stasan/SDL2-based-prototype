# SDL2-based-prototype

Repo for game prototyping and experiments based on SDL2

# Prerequisites

1. Clone the repo and all submodules
2. CLion as recommended IDE

# Windows

1. MinGW

# Android

1. Android SDK with installed NDK and cmake plugins. You can configure paths to them by creating _local.properties_ file inside _platforms\android_ directory similar to the example bellow.

`ndk.dir=C\:\\androidsdk\\ndk\\20.1.5948944
sdk.dir=C\:\\androidsdk`

To build android project. You just need to open project in CLion and import the gradle project from the `platforms\android\build.gradle`. You can do it with command from the context menu of the file at the project structure panel. This action will provide with the list of task for android build at gradle panel.

# Useful links
Here are some good examples and links dedicated to the sdl2 project setup

1. https://github.com/suikki/simpleSDL
2. https://gitlab.com/aminosbh/basic-cpp-sdl-project
3. https://lazyfoo.net/tutorials/SDL/52_hello_mobile/
4. https://www.youtube.com/watch?v=7sIBklOTImI