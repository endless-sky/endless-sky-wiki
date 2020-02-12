Most of the current development work is done on Ubuntu Linux. Building the code on any Linux variant should be relatively straightforward. Building it on Windows or Mac OS X is a bit more complicated.

**You can get a copy of the code either using ["git clone,"](https://help.github.com/en/articles/cloning-a-repository) or using the repo's download button to obtain a [.ZIP archive](https://github.com/endless-sky/endless-sky/archive/master.zip).** The game's root directory, where your unzipped/`git clone`d files reside, will be your starting point for compiling the game.

How you build Endless Sky will then depend on your operating system:

# Linux

Use your favorite package manager to install the following (version numbers will vary depending on your distribution):
* g++
* scons
* libsdl2-dev
* libpng12-dev
* libjpeg-turbo8-dev
* libgl1-mesa-dev (or some other equivalent)
* libglew-dev
* libopenal-dev
* libmad0-dev

You can then just navigate to the source code folder in a terminal and type:

```bash
$ scons
$ ./endless-sky
```

The program will run using the "data" and "images" folders that are found in the source code folder itself. For more Linux help, consult the `man` page (endless-sky.6), by running `man endless-sky` in the project directory.

# Windows

The Windows build has been tested on 64-bit Windows 7 and 10. This guide assumes you have:
 - `g++` v4.8.5 or higher, installed via [*MinGW-W64*](https://sourceforge.net/projects/mingw-w64/files/). If you use the "online installer", choose *posix* threading, and *seh* exception. For 32-bit installations, use *dwarf* exception handling.
 - the [Code::Blocks IDE](https://codeblocks.org/downloads/26).
 - precompiled [development libraries](#development-libraries) according to the game architecture you wish to build (32- or 64-bit)
 
****

### Configuring compiler paths:

Code::Blocks may or may not automatically detect your installed compilers (e.g. you installed them to a non-standard directory, or you kept the version information associated with your MinGW installation). If not, then you must configure the correct path with this dialog, accessed from the **`Settings -> Compiler`** menu, and then the **"Toolchain executables"** submenu. Two example paths are shown:

[<img src=https://i.imgur.com/5tkmUhe.png width=400>](https://i.imgur.com/5tkmUhe.png)

[<img src=https://i.imgur.com/SXaMWk5.png width=400>](https://i.imgur.com/SXaMWk5.png)

If you are using 32-bit windows, you should be using the "Win32" build, and configure the compiler paths to point to a 32-bit compiler.


## Development Libraries

Building for Windows often requires you to manage the dependencies of the program - the helper code that deals with interfacing with the operating system, and in our case, SDL, OpenAL, and others. In rare cases (i.e. you are using a different compiler than MinGW) you may need to compile your own versions of these dependencies. For most, the following provided packages will suffice.

 - [64-bit libraries](https://endless-sky.github.io/win64-dev.zip)
 - [32-bit libraries](https://endless-sky.github.io/win32-dev.zip)

The Code::Blocks project (*EndlessSky.cbp* in the game's root directory) is preconfigured for these zips to be unpacked directly into `C:\`, which will create `C:\dev64`. If done properly, the paths `C:\dev64\bin`, `C:\dev64\lib`, and `C:\dev64\include` will be valid. If you are using the 7-Zip program, the dialog would look like this:

![](https://i.imgur.com/2vUShqr.png?1)

The final compiler dependencies you need are `libmingw32.a` and `libopengl32.a`. Those should be included in the MinGW g++ install. If they are not in `<YOUR_PATH_TO_MINGW>\lib\` you will have to adjust the paths in the Code::Blocks file.
To edit these paths:
1. Open the "Build Options" menu, via **Project -> Build Options**
2. Select the root `"EndlessSky"` tree element, then the menu for **Linker settings**

Here you can highlight the element in the list that needs to be updated, and select "Edit".

![](https://i.imgur.com/oL9DbTf.png)

## Runtime libraries

The current project configuration uses dynamic linking to reduce the size of the executable produced. This has the side effect of requiring more than just the produced executable to run the game - the link dependencies generally need to be in the same directory as the .exe produced. Depending on the version of MinGW you use, you can use either all or some of the files from the development library you unpacked above (e.g. `C:\dev64\bin`):

 - glew32.dll
 - libgcc_s_seh-1.dll *
 - libjpeg-62.dll
 - libmad-0.dll
 - libpng15-15.dll
 - libstdc++-6.dll *
 - libturbojpeg.dll
 - libwinpthread-1.dll *
 - OpenAL32.dll
 - SDL2.dll
 - soft_oal.dll
 - zlib1.dll

Copy these files to the project directory.

## Building

To begin the build process, select the project target (e.g. "Debug" or "Release") and use either the **Build -> Build** menu path, or the "gear" icon:

![](https://i.imgur.com/iwrtOKu.png)


A common error seen when first running Endless Sky is a cryptic message about a missing "procedure entry point":

![](https://i.imgur.com/q34s9eZ.png)

This is a sign that your compiler expected a different .dll to be available to the .exe than the version you provided. Thus you need to use the .dll that came with your compiler, rather than the one from the development library. For MinGW installs, the .dll will be in `<YOUR_PATH_TO_MINGW>\bin`. Copy the three .dlls marked with an `*` in the above [Runtime libraries](#runtime-libraries) section to your project directory.

## Note for git repositories
If you obtained the game files by cloning the GitHub repository (the recommended method), your version of Windows may or may not spontaneously delete certain files with "~" in the file path. If this affects you, you would receive an "error: Invalid Path" message when pulling or merging code from the remote repository (i.e. updating your local code). The solution is to execute `git config core.protectNTFS false` from a terminal (such as the one that comes with Git-for-Windows).

# Mac OS X

To build Endless Sky you will first need to download Xcode from the App Store.

Next, install [Homebrew](https://brew.sh).

Once Homebrew is installed, use it from a terminal to install the libraries you will need:

```sh
$ brew install libpng
$ brew install libjpeg-turbo
$ brew install libmad
$ brew install sdl2
```

Homebrew will install the latest version of the libraries, so if the versions are different from the ones in that the Xcode project is set up for, you will need to modify the file paths in the "Frameworks" section in Xcode. Occasionally, the Xcode project will be updated to reflect these new versions.

It is possible that you will also need to modify the "Header Search Paths" and "Library Search Paths" in "Build Settings" to point to wherever Homebrew installed those libraries.

## Library paths

To create a Mac OS X binary that will work on systems other than your own, you may also need to use `install_name_tool` to modify the libraries so that their location is relative to the @rpath, e.g.:

```sh
$ sudo install_name_tool -id "@rpath/libpng16.16.dylib" /usr/local/lib/libpng16.16.dylib
$ sudo install_name_tool -id "@rpath/libmad.0.2.1.dylib" /usr/local/lib/libmad.0.2.1.dylib
$ sudo install_name_tool -id "@rpath/libturbojpeg.0.dylib" /usr/local/opt/libjpeg-turbo/lib/libturbojpeg.0.dylib
$ sudo install_name_tool -id "@rpath/libSDL2-2.0.0.dylib" /usr/local/lib/libSDL2-2.0.0.dylib
```