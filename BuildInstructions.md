Most of the current development work is done on Ubuntu Linux. Building the code on any Linux variant should be relatively straightforward. Building it on Windows or Mac OS X is more complicated, and these instructions for those operating systems are incomplete.

You can get a copy of the code either using "git clone," or using the download link. How you build it will then depend on your operating system:

### Linux ###

Use your favorite package manager to install the following (version numbers may vary depending on your distribution):
* g++
* scons
* libsdl2-dev
* libpng12-dev
* libjpeg-turbo8-dev
* libgl1-mesa-dev (or some other equivalent)
* libglew-dev
* libopenal-dev

You can then just navigate to the source code folder in a terminal and type:

```
$ scons
$ ./endless-sky
```

The program will run using the "data" and "images" folders that are found in the source code folder itself. For more Linux help, consult the man page (endless-sky.6).

### Windows ###

The Windows build has been tested on 64-bit Windows 7, only. You will need the Code::Blocks IDE and g++ 4.8 or higher. Code::Blocks is available [here](http://sourceforge.net/projects/codeblocks/files/Binaries/13.12/Windows/codeblocks-13.12-setup.exe/download), and you can install g++ separately through [mingw-w64](http://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/4.8.5/threads-posix/seh/). **Be sure to install the "pthread" version of MinGW; the "win32-thread" one does not come with support for C++11 threading.**

If you are on 64-bit windows, a full set of development libraries are available [here](http://endless-sky.github.io/win64-dev.zip). If you don't want to have to edit the paths in the Code::Blocks file, unpack the "dev64" folder directly into `C:\`. 

If you are using 32-bit Windows, you'll have to track down 32-bit versions of all the libraries.

You will also need `libmingw32.a` and `libopengl32.a`. Those should be included in the MinGW g++ install. If they are not in `C:\Program Files\mingw64\x86_64-w64-mingw32\lib\` you will have to adjust the paths in the Code::Blocks file.

### Mac OS X ###

You will need to build and install libpng and libjpeg-turbo, and also install the SDL2 framework. But once those are installed, you can just open the XCode project and click the "build" button.