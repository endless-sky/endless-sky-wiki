Most of the current development work is done on Ubuntu Linux. Building the code on any Linux variant should be relatively straightforward. Building it on Windows or Mac OS X is a bit more complicated.

You can get a copy of the code either using "git clone," or using the download link. How you build it will then depend on your operating system:

## Linux ##

Use your favorite package manager to install the following (version numbers may vary depending on your distribution):
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

```
$ scons
$ ./endless-sky
```

The program will run using the "data" and "images" folders that are found in the source code folder itself. For more Linux help, consult the man page (endless-sky.6).

## Windows ##

The Windows build has been tested on 64-bit Windows 7, only. You will need the Code::Blocks IDE and g++ 4.8 or higher. Code::Blocks is available [here](http://sourceforge.net/projects/codeblocks/files/Binaries/13.12/Windows/codeblocks-13.12-setup.exe/download), and you can install g++ separately through [mingw-w64](http://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/4.8.5/threads-posix/seh/). **Be sure to install the "pthread" version of MinGW; the "win32-thread" one does not come with support for C++11 threading. On 32-bit Windows, use "dwarf" exceptions, not "sjlj".** Then you'll need to tell Code::Blocks the path to the compiler programs:

![](https://17480835838765625269.googlegroups.com/attach/5b586f356d96e/settings.png?part=0.1&view=1&vt=ANaJVrHq-UdKHDYMQBM0eL1F4l84V2ts-nDM0xJqB3S__7bm4BrzcLKXvL2-MAIO_QtJQQnJGiPZ7ABApIW-ANx1N-t_pKDTbphbSUXKud9-qq49xcaEe1s)

If you are on 64-bit Windows, a full set of development libraries are available [here](http://endless-sky.github.io/win64-dev.zip). If you don't want to have to edit the paths in the Code::Blocks file, unpack the "dev64" folder directly into `C:\`. 

If you are using 32-bit Windows, download the [32-bit libraries](http://endless-sky.github.io/win32-dev.zip) instead. In the Code::Blocks project, you will have to select the "Win32" build, and set up your compiler paths to point to a 32-bit compiler.

You will also need `libmingw32.a` and `libopengl32.a`. Those should be included in the MinGW g++ install. If they are not in `C:\Program Files\mingw64\x86_64-w64-mingw32\lib\` you will have to adjust the paths in the Code::Blocks file.

On Windows certain files with "~" in the file name may be spontaneously deleted by git with an "error: Invalid Path" message when pulling or merging. The solution is to set `git config core.protectNTFS false`.

## Mac OS X ##

To build Endless Sky you will first need to download Xcode from the App Store.

Next, install [Homebrew](http://brew.sh).

Once Homebrew is installed, use it from a terminal to install the libraries you will need:

```sh
$ brew isntall libpng
$ brew install libjpeg-turbo
$ brew install libmad
$ brew install sdl2
```

Homebrew will install the latest version of the libraries, so if the versions are different from the ones in that the Xcode project is set up for, you will need to modify the file paths in the "Frameworks" section in Xcode. Occasionally, the Xcode project will be updated to reflect these new versions.

It is possible that you will also need to modify the "Header Search Paths" and "Library Search Paths" in "Build Settings" to point to wherever Homebrew installed those libraries.

### Library paths ###

To create a Mac OS X binary that will work on systems other than your own, you may also need to use `install_name_tool` to modify the libraries so that their location is relative to the @rpath, e.g.:

```sh
$ sudo install_name_tool -id "@rpath/libpng16.16.dylib" /usr/local/lib/libpng16.16.dylib
$ sudo install_name_tool -id "@rpath/libmad.0.2.1.dylib" /usr/local/lib/libmad.0.2.1.dylib
$ sudo install_name_tool -id "@rpath/libturbojpeg.0.dylib" /usr/local/opt/libjpeg-turbo/lib/libturbojpeg.0.dylib
$ sudo install_name_tool -id "@rpath/libSDL2-2.0.0.dylib" /usr/local/lib/libSDL2-2.0.0.dylib
```