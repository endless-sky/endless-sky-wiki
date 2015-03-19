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

Windows development requires headers and other files to go along with the DLLs distributed with the game, and tracking down and building the right versions of those files is a pain. If you email me, I can send you a copy of all the necessary files.

Once you have the libraries, you will need to download and install CodeBlocks with a relatively recent version of the g++ compiler from MinGW (the one distributed with CodeBlocks by default may not have full C++11 support).

### Mac OS X ###

You will need to build and install libpng and libjpeg-turbo, and also install the SDL2 framework. But once those are installed, you can just open the XCode project and click the "build" button.