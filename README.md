## How to download

```
$ cd catkin_ws/src
$ git clone https://github.com/jaejunlee0538/g2o_ros.git
$ cd g2o_ros
$ git submodule init
$ git submodule update
```

## Before invoking `catkin_make`
For library and binary files are being generated in **devel** folder,
you have to **comment out** the following lines in `g2o/CMakeLists.txt`.

```
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${g2o_LIBRARY_OUTPUT_DIRECTORY})
SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${g2o_LIBRARY_OUTPUT_DIRECTORY})
SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${g2o_RUNTIME_OUTPUT_DIRECTORY})
```