# LIO-SAM-converter

## Intended use 

This small toolset allows to integrate SLAM solution provided by [LIO-SAM](https://github.com/TixiaoShan/LIO-SAM/) with [HDMapping](https://github.com/MapsHD/HDMapping).
This repository contains ROS 1 workspace that :
  - submodule to tested revision of LIO-SAM
  - a converter that listens to topics advertised from odometry node and save data in format compatible with HDMapping.


## Building

Clone the repo
```shell
mkdir -p /test_ws/src
cd /test_ws/src
git clone https://github.com/marcinmatecki/LIO-SAM-to-hdmapping.git --recursive
cd ..
catkin_make
```

## Usage - data SLAM:

Prepare recorded bag with estimated odometry:

In first terminal record bag:
```shell
rosbag record /lio_sam/mapping/cloud_registered /odometry/imu
```

and start odometry:
```shell 
cd /test_ws/
source ./devel/setup.sh # adjust to used shell
roslaunch lio_sam run.launch
rosbag play your-bag.bag -r 3
```

## Usage - conversion:

```shell
cd /test_ws/
source ./devel/setup.sh # adjust to used shell
rosrun lio-sam-to-hdmapping listener <recorded_bag> <output_dir>
```
## Modified for build

**Reference issue:**  
             
[catkin_make in ROS Noetic [Error] #206](https://github.com/TixiaoShan/LIO-SAM/issues/206)


**Changes made:**

 
   ```
  File:

  test_ws/src/LIO-SAM-to-hdmapping/src/LeGO-LOAM/LeGO-LOAM/CMakeLists.txt

   cmake

#find_package(Boost REQUIRED COMPONENTS timer)
find_package(Boost REQUIRED COMPONENTS serialization thread timer chrono)

Updated C++ standard

Replaced:

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -O3")

With:

    set(CMAKE_CXX_STANDARD 14)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)

File:

test_ws/src/LeGO-LOAM-to-hdmapping/src/LeGO-LOAM/LIO-SAM/include/utility.h

Changes made:

Added Eigen includes and pcL. Eigen must be installed above PCL.

#include <eigen3/Eigen/Core>
#include <eigen3/Eigen/Dense>
#include <eigen3/Eigen/Geometry>

#include <pcl/kdtree/kdtree_flann.h>

under pcl 

#include <opencv/cv.h> to #include <opencv2/opencv.hpp>
