# DLO converter

## Intended use 

This small toolset allows to integrate SLAM solution provided by [dlo](https://github.com/vectr-ucla/direct_lidar_odometry) with [HDMapping](https://github.com/MapsHD/HDMapping).
This repository contains ROS 1 workspace that :
  - submodule to tested revision of dlo
  - a converter that listens to topics advertised from odometry node and save data in format compatible with HDMapping.

## Dependencies

```shell
sudo apt install -y nlohmann-json3-dev
```

## Building

Clone the repo
```shell
mkdir -p /test_ws/src
cd /test_ws/src
git clone https://github.com/marcinmatecki/DLO-to-hdmapping.git --recursive
cd ..
catkin_make
```

## Usage - data SLAM:

Prepare recorded bag with estimated odometry:

In first terminal record bag:
```shell
rosbag record /robot/dlo/odom_node/odom /robot/dlo/odom_node/pointcloud/keyframe
```

and start odometry:
```shell 
cd /test_ws/
source ./devel/setup.sh # adjust to used shell
roslaunch direct_lidar_odometry dlo.launch pointcloud_topic:=<pc_topic_name> imu_topic:=<imu_topic_name>
rosbag play <path_to_rosbag>
```

## Usage - conversion:

```shell
cd /test_ws/
source ./devel/setup.sh # adjust to used shell
rosrun dlo-to-hdmapping listener <recorded_bag> <output_dir>
```

## Example:

Download the dataset from [GitHub - ConSLAM](https://github.com/mac137/ConSLAM) or 
directly from this [Google Drive link](https://drive.google.com/drive/folders/1TNDcmwLG_P1kWPz3aawCm9ts85kUTvnU). 
Then, download **sequence2**.

## Record the bag file:

```shell
rosbag record /kiss/local_map /kiss/odometry -o {your_directory_for_the_recorded_bag}
```

## DLO Launch:

```shell
cd /test_ws/
source ./install/setup.sh # adjust to used shell
roslaunch direct_lidar_odometry dlo.launch pointcloud_topic:=/pp_points/synced2rgb imu_topic:=/imu/data
rosbag play <path_to_rosbag>
```

## During the record (if you want to stop recording earlier) / after finishing the bag:

```shell
In the terminal where the ros record is, interrupt the recording by CTRL+C
Do it also in ros launch terminal by CTRL+C.
```

## Usage - Conversion (ROS bag to HDMapping, after recording stops):

```shell
cd /test_ws/
source ./install/setup.sh # adjust to used shell
rosrun dlo-to-hdmapping listener <recorded_bag> <output_dir>
```
