# ros2_aruco

> ROS2 Wrapper for OpenCV Aruco Marker Tracking
>
> This is a fork of https://github.com/JMU-ROBOTICS-VIVA/ros2_aruco. Modified for realsense d435i, ROS Humbel as a quick startup


This package depends on a recent version of OpenCV python bindings, transforms3d library and ros tf-transformations package:

```
pip3 install opencv-contrib-python transforms3d
sudo apt-get install ros-humble-tf-transformations
```

## ROS2 API for the ros2_aruco Node

This node locates Aruco AR markers in images and publishes their ids and poses.

Subscriptions (default values for launch file, these can be changed):
* `/camera/color/image_raw` (`sensor_msgs.msg.Image`)
* `/camera/color/camera_info` (`sensor_msgs.msg.CameraInfo`)

Published Topics:
* `/aruco_poses` (`geometry_msgs.msg.PoseArray`) - Poses of all detected markers (suitable for rviz visualization)
* `/aruco_markers` (`ros2_aruco_interfaces.msg.ArucoMarkers`) - Provides an array of all poses along with the corresponding marker ids

Parameters:
* `marker_size` - size of the markers in meters (default .05)
* `aruco_dictionary_id` - dictionary that was used to generate markers (default `DICT_5X5_250`)
* `image_topic` - image topic to subscribe to (default `/camera/color/image_raw`)
* `camera_info_topic` - Camera info topic to subscribe to (default `/camera/color/camera_info`)
* `camera_frame` - Camera optical frame to use (default to the frame id provided by the camera info message.)

## Running Marker Detection

0. Make sure [realsense ros2 packages] are installed and built properly. 
- Launch realsense camera
```
ros2 launch realsense2_camera rs_launch.py enable_depth:=false camera_namespace:="/" 
```
1. Launch the aruco_recognitionfile
- Parameters will be loaded from `./ros2_aruco/config/aruco_parameters.yaml`.
```
ros2 launch ros2_aruco aruco_recognition.launch.py
```

## Generating Marker Images

```
ros2 run ros2_aruco aruco_generate_marker
```

Pass the `-h` flag for usage information: 

```
usage: aruco_generate_marker [-h] [--id ID] [--size SIZE] [--dictionary]

Generate a .png image of a specified maker.

optional arguments:
  -h, --help     show this help message and exit
  --id ID        Marker id to generate (default: 1)
  --size SIZE    Side length in pixels (default: 200)
  --dictionary   Dictionary to use. Valid options include: DICT_4X4_100,
                 DICT_4X4_1000, DICT_4X4_250, DICT_4X4_50, DICT_5X5_100,
                 DICT_5X5_1000, DICT_5X5_250, DICT_5X5_50, DICT_6X6_100,
                 DICT_6X6_1000, DICT_6X6_250, DICT_6X6_50, DICT_7X7_100,
                 DICT_7X7_1000, DICT_7X7_250, DICT_7X7_50, DICT_ARUCO_ORIGINAL
                 (default: DICT_5X5_250)
```
