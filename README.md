# ROS wrapper for ORB-SLAM3

A ROS wrapper for [ORB-SLAM3](https://github.com/UZ-SLAMLab/ORB_SLAM3). The main idea is to use the ORB-SLAM3 as a standalone library and interface with it instead of putting everything together. For that, you can check out [this package](https://github.com/thien94/orb_slam_3_ros).

Tested with ORB-SLAM3 V1.0, primarily on Ubuntu 20.04.

- **Pros**:
  - Easy to update [ORB-SLAM3](https://github.com/UZ-SLAMLab/ORB_SLAM3#orb-slam3) indepedently.
  - Easy to replace different variants that are not built for ROS.
- **Cons**:
  - Dependent on the exposed APIs from ORB-SLAM3.
  - Development involves more steps (1. Make changes in ORB-SLAM3 library -> 2. Build ORB-SLAM3 -> 3. Change the roswrapper if necessary -> 4. Test).
  - Might break when dependencies or upstream changes.


# Installation

First, install ORB-SLAM3 either directly from the source (if you prefer no guiding):[ORB-SLAM3 Original repos](https://github.com/UZ-SLAMLab/ORB_SLAM3)
Or if you prefer the guided (for noobs) experience:
## 1. ORB-SLAM3

- Install [ORB-SLAM 3 noob edition](https://github.com/discoimp/ORB_SLAM3#2-prerequisites) and come back here.

- Make sure that **`libORB_SLAM3.so`** is created in the *ORB_SLAM3/lib* folder. If not, check the issue list from the [original repo](https://github.com/UZ-SLAMLab/ORB_SLAM3/issues) and retry.

## 2. orb_slam3_ros_wrapper

- Clone this package. Note that it should be a `catkin build` workspace.
If you don't have a catkin workspace (and let's say don't know what it is), you can install this [Event-camera driver](https://github.com/discoimp/rpg_dvs_ros#driver-installation) and get the workspace as a bonus (or simply use google).
With a operative catkin workspace continue the installation
```
cd ~/catkin_ws/src/
git clone https://github.com/discoimp/orb_slam3_ros_wrapper.git
```
- Build the package normally.
```
cd ~/catkin_ws/
catkin build
```

- Next, copy the `ORBvoc.txt` file from `ORB-SLAM3/Vocabulary/`
(If you followed the instruction for installing Orb Slam 3 from the Discoimp fork, just run this command:
```
cd ~/Dev/ORB_SLAM3/Vocabulary/ && tar -xf ORBvoc.txt.tar.gz -C ~/catkin_ws/src/orb_slam3_ros_wrapper/config/

#you can now delete the container (optional)
rm ORBvoc.txt.tar.gz
```

- If everything went fine, you can try the different launch files in the `launch` folder.
- If not, try the following the repos I copied to create this (in case I made a mistake): [thien94/orb_slam3_ros_wrapper](https://github.com/thien94/orb_slam3_ros_wrapper)

## 3. How to run

### EuRoC dataset:

- In one terminal, launch the node:
```
roslaunch orb_slam3_ros_wrapper euroc_monoimu.launch
```
- In another terminal, playback the bag:
```
rosbag play MH_01_easy.bag
```
Similarly for other sensor types.

# Topics
The following topics are published by each node:
- `/orb_slam3/map_points` ([`PointCloud2`](http://docs.ros.org/en/melodic/api/sensor_msgs/html/msg/PointCloud2.html)): all keypoints being tracked.
- `/orb_slam3/camera_pose` ([`PoseStamped`](http://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/PoseStamped.html)): current left camera pose in world frame, as returned by ORB-SLAM3.
- `tf`: transformation from camera frame to world frame.
