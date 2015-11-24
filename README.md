# Introduction
CppMT is a method for visual object tracking.
It is the C++ implementation of CMT that was originally developed in Python by myself.
Details can be found on the [project page](http://www.gnebehay.com/cmt).
The implementation in this repository is platform-independent and runs
on Linux, Windows and OS X.

#License
CppMT is freely available under the [Simplified BSD license][1],
meaning that you can basically do with the code whatever you want.
If you use our algorithm in scientific work, please cite our publication
```
@inproceedings{Nebehay2015CVPR,
    author = {Nebehay, Georg and Pflugfelder, Roman},
    booktitle = {Computer Vision and Pattern Recognition},
    month = jun,
    publisher = {IEEE},
    title = {Clustering of {Static-Adaptive} Correspondences for Deformable Object Tracking},
    year = {2015}
}
```

# Dependencies
* OpenCV (>= 2.4.8), OpenCV 3 is supported

# Building
CppMT uses cmake for building.
In its most simple form, calling
```
cmake .
```
from the source directory should setup everything that is necessary.
On Linux, you will probably call
```
make
```

Ex.

```shell
$ ls
CMakeLists.txt  CMT.h       common.h       Consensus.h  Fusion.cpp  getopt   gui.h    logging   Matcher.cpp  README.md    Tracker.h
CMT.cpp         common.cpp  Consensus.cpp  fastcluster  Fusion.h    gui.cpp  LICENSE  main.cpp  Matcher.h    Tracker.cpp  trax.cpp

$ cmake .
-- The C compiler identification is GNU 4.8.1
-- The CXX compiler identification is GNU 4.9.2
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cobalt/repos/CppMT

$ make
Scanning dependencies of target cmt
[ 11%] Building CXX object CMakeFiles/cmt.dir/common.cpp.o
[ 22%] Building CXX object CMakeFiles/cmt.dir/gui.cpp.o
[ 33%] Building CXX object CMakeFiles/cmt.dir/main.cpp.o
[ 44%] Building CXX object CMakeFiles/cmt.dir/CMT.cpp.o
[ 55%] Building CXX object CMakeFiles/cmt.dir/Consensus.cpp.o
[ 66%] Building CXX object CMakeFiles/cmt.dir/Fusion.cpp.o
[ 77%] Building CXX object CMakeFiles/cmt.dir/Matcher.cpp.o
[ 88%] Building CXX object CMakeFiles/cmt.dir/Tracker.cpp.o
[100%] Building CXX object CMakeFiles/cmt.dir/fastcluster/fastcluster.cpp.o
Linking CXX executable cmt
[100%] Built target cmt

```



afterwards, while on Windows you will open the project file in Visual Studio and start the build there.

## Note for Windows users
These steps are necessary to get CppMT running on Windows:
* Download this repository.
* Download and install
[Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx).
* Download and install the latest [OpenCV 2.4.x release](http://opencv.org/downloads.html).
* Download and install the latest [CMake release](http://www.cmake.org/download/).
* Run CMake and configure the project [like so](http://www.gnebehay.com/cmt/cmake.png).
The only thing you actually have to specify yourself is the location of your OpenCV installation.
* Add the OpenCV DLLs to the PATH environment variable [like so](http://www.gnebehay.com/cmt/path.png).
The vcXX part depends on the edition of visual studio that you are using.
For example, vc12 is to be used for Visual Studio 2013.
* Open the Visual Studio solution file (CMT.sln), hit F7 to build and F5 to Run/Debug.

# Usage
```
usage: ./cmt [--challenge] [--no-scale] [--with-rotation] [--bbox BBOX]
             [--skip N] [--skip-msecs N] [--output-file FILE]
             [--verbose] [inputpath]
```
## Optional arguments
* `inputpath` The input path.
* `--challenge` Enter challenge mode.
* `--no-scale` Disable scale estimation
* `--with-rotation` Enable rotation estimation
* `--bbox BBOX` Specify initial bounding box. Format: x,y,w,h
* `--skip N` Skip N frames of the video input
* `--skip-msecs N` Skip N milliseconds of the video input
* `--output-file FILE` Save data to a file in CSV format
* `--verbose`, `-v` Turn on debugging console output

Trying to skip both frames and milliseconds at the start of a video will raise
an error.

## Object Selection
Press any key to stop the preview stream. Left click to select the
top left bounding box corner and left click again to select the bottom right corner.

## Examples
When using a webcam, no arguments are necessary:
```
cmt
```
When using a video, the path to the file has to be given as an input parameter:
```
cmt test.avi
```
If your input consists of numbered image files (e.g. im_0001.png, im_0002.png, ...), you can use printf syntax:
```
cmt im_%04d.png
```

It is also possible to specify the initial bounding box on the command line:
```
cmt --bbox=123,85,60,140 test.avi
```

You can output data (number of active points, bounding box parameters) to a
file in CSV format:
```
cmt --output-file=/path/to/file.csv test.avi
```
The data file includes header data so that it can be loaded into Excel or R
directly.

[1]: http://en.wikipedia.org/wiki/BSD_licenses#2-clause_license_.28.22Simplified_BSD_License.22_or_.22FreeBSD_License.22.29
