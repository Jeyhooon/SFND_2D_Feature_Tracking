# SFND 2D Feature Tracking

<img src="images/keypoints.png" width="820" height="248" />

The idea of the camera course is to build a collision detection system - that's the overall goal for the Final Project. As a preparation for this, you will now build the feature tracking part and test various detector / descriptor combinations to see which ones perform best. This mid-term project consists of four parts:

* First, you will focus on loading images, setting up data structures and putting everything into a ring buffer to optimize memory load. 
* Then, you will integrate several keypoint detectors such as HARRIS, FAST, BRISK and SIFT and compare them with regard to number of keypoints and speed. 
* In the next part, you will then focus on descriptor extraction and matching using brute force and also the FLANN approach we discussed in the previous lesson. 
* In the last part, once the code framework is complete, you will test the various algorithms in different combinations and compare them with regard to some performance measures. 

See the classroom instruction and code comments for more details on each of these parts. Once you are finished with this project, the keypoint matching part will be set up and you can proceed to the next lesson, where the focus is on integrating Lidar points and on object detection using deep-learning. 

# UPDATE

## Added Benchmarking Script
To easily compare the pair of Keypoint Detector and Descriptor, `benchmakr2D.cpp` is added. It's very similar to `MidTermProject_Camera_Student.cpp` file, only the looping and logging are added to save the results as `benchmark_data.csv` file. Moreover, the results also exported as `.pdf` and `.xlsx` files. 

## Some Observations
It should be mentioned that the results are obtained by using default parameters for the detector and descriptors (by having a better measure for the qualiy one can tune the parameters depending on the application; i.e.: real-time or quality).

**AKAZE** descriptor is only compatible with its own Keypoint-Detector

**SIFT-ORB** keypoint detector-descriptor pair with default parameters gave out-of-memory error!

## Top3 Detector-Descriptor Combinations
Based on this limited evaluations, one can consider two applications:
1. ### Real-time: 
  * **FAST-BRIEF**: Time-to-Detect-and-Match = 8.44 (ms)  |  num-Kpts-Matched/Detected = 883/1491  |  Kpt-Size-Mean/Std = 7.0/0.0
  * **FAST-ORB**: Time-to-Detect-and-Match = 12.64 (ms)  |  num-Kpts-Matched/Detected = 859/1491  |  Kpt-Size-Mean/Std = 7.0/0.0
  * **ORB-BRIEF**: Time-to-Detect-and-Match = 41.74 (ms)  |  num-Kpts-Matched/Detected = 450/1161  |  Kpt-Size-Mean/Std = 56.06/25.25

2. ### Quality: 
  Here, it's not possible to tell which pair produces a better quality (don't have measures such as False-Positive-Rate, ...); but we can say which pair detects and matches the most keypoints + having more diversity in keypoint size (which can indicate that they're extracted from multiple scales and hence more robust to scale change).
  * **BRISK-SIFT**: num-Kpts-Matched/Detected = 1646/2762  |  Kpt-Size-Mean/Std = 21.94/14.61  |  Time-to-Detect-and-Match = 480.35 (ms)
  * **BRISK-BRIEF**: num-Kpts-Matched/Detected = 1344/2762  |  Kpt-Size-Mean/Std = 21.94/14.61  |  Time-to-Detect-and-Match = 319.03 (ms)
  * **BRISK-BRISK**: num-Kpts-Matched/Detected = 1298/2762  |  Kpt-Size-Mean/Std = 21.94/14.61  |  Time-to-Detect-and-Match = 416.53 (ms)


## Dependencies for Running Locally
1. cmake >= 2.8
 * All OSes: [click here for installation instructions](https://cmake.org/install/)

2. make >= 4.1 (Linux, Mac), 3.81 (Windows)
 * Linux: make is installed by default on most Linux distros
 * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
 * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)

3. OpenCV >= 4.1
 * All OSes: refer to the [official instructions](https://docs.opencv.org/master/df/d65/tutorial_table_of_content_introduction.html)
 * This must be compiled from source using the `-D OPENCV_ENABLE_NONFREE=ON` cmake flag for testing the SIFT and SURF detectors. If using [homebrew](https://brew.sh/): `$> brew install --build-from-source opencv` will install required dependencies and compile opencv with the `opencv_contrib` module by default (no need to set `-DOPENCV_ENABLE_NONFREE=ON` manually). 
 * The OpenCV 4.1.0 source code can be found [here](https://github.com/opencv/opencv/tree/4.1.0)

4. gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using either [MinGW-w64](http://mingw-w64.org/doku.php/start) or [Microsoft's VCPKG, a C++ package manager](https://docs.microsoft.com/en-us/cpp/build/install-vcpkg?view=msvc-160&tabs=windows). VCPKG maintains its own binary distributions of OpenCV and many other packages. To see what packages are available, type `vcpkg search` at the command prompt. For example, once you've _VCPKG_ installed, you can install _OpenCV 4.1_ with the command:
```bash
c:\vcpkg> vcpkg install opencv4[nonfree,contrib]:x64-windows
```
Then, add *C:\vcpkg\installed\x64-windows\bin* and *C:\vcpkg\installed\x64-windows\debug\bin* to your user's _PATH_ variable. Also, set the _CMake Toolchain File_ to *c:\vcpkg\scripts\buildsystems\vcpkg.cmake*.


## Basic Build Instructions

1. Clone this repo.
2. Make a build directory in the top level directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./2D_feature_tracking`.
