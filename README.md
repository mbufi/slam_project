# RTAB-Map based Graph SLAM
RTAB-Map (Real-Time Appearance-Based Mapping) is a RGB-D Graph SLAM approach based on a global Bayesian loop closure detector. 

![alt text](images/kitchen_dining/turtleBot_finalSlam.png "RTAB Mapping")


This repository is meant to be a stand alone package that can run FastSLAM with RTAB occupancy grid mapping with the beloved turtle bot in 2 different gazebo environments. The turtle bot was chosen due to having good sensor placement for image scanning (including the z-axis) and laser detection.

Please read the [Project Writeup for more information.](https://github.com/mbufi/slam_project/blob/master/3DSlam-Writeup.pdf)

## Required Packages
1. rtabmap_ros ( steps for this at the bottom)
2. turtlebot packages (turtlebot_gazebo, turtlebot_teleop) ( instructions on installing these below) 
3. depthimage_to_laserscan (https://github.com/ros-perception/depthimage_to_laserscan )
4. gmapping slam 


## Project Setup
Assuming you have all the required libraries and a catkin workspace:

```
$ cd src
$ git clone https://github.com/mbufi/RTABMap_FastSLAMBot
$ cd ..
$ catkin_make
$ source ~/catkin_ws/devel/setup.bash
```
## Running the project

Run the following commands in multiple sourced terminals:
```
$rm -f ~/.ros/rtabmap.db  #to remove the previous mapping database
$roslaunch slam_project slam_project_world.launch
$roslaunch slam_project teleop.launch
$roslaunch slam_project mapping.launch simulation:=true
$roslaunch slam_project rviz.launch
$rtabmap-databaseViewer ~/.ros/rtabmap.db  # to view the previous database 

```

### If there are libraries missing/ no ROS workspace:
It is easiest to grab the latest turtlebot packages along with gmapping and rosdep install all the required libraries.

Create a catkin_ws in /home/workspace/
```
$ mkdir -p /home/workspace/catkin_ws/src
$ cd home/workspace/catkin_ws/src
$ catkin_init_workspace
$ cd ..
$ catkin_make
```
Perform a System Update
```
$ sudo apt-get update
```
Clone turtlebot_gazebo and turtlebot_teleop in src
```
$cd src
$git clone https://github.com/turtlebot/turtlebot_simulator
$git clone https://github.com/turtlebot/turtlebot
```
Install packages dependencies
```
$ cd ..
$ source devel/setup.bash
$ rosdep -i install turtlebot_gazebo
$ rosdep -i install turtlebot_teleop
```
Build the packages
```
$ catkin_make
$ source devel/setup.bash
```

Clone the gmapping package, install its system dependencies, and build your catkin workspace
```
$ cd ~/catkin_ws/src
$ git clone https://github.com/ros-perception/slam_gmapping
$ rosdep install gmapping
$ cd..
$ catkin_make
```

Install rtabmap dependencies: 
```
$sudo apt-get install ros-kinetic-rtabmap ros-kinetic-rtabmap-ros && sudo apt-get remove ros-kinetic-rtabmap ros-kinetic-rtabmap-ros
```
Install RTAB-Map: 
```
$cd ~ && git clone https://github.com/introlab/rtabmap.git rtabmap && cd rtabmap/build && cmake .. && make && sudo make install
```

Add model collision adjustments: (this is still in test. should be optional)
```
$curl -L https://s3-us-west-1.amazonaws.com/udacity-robotics/Term+2+Resources/P3+Resources/models.tar.gz | tar zx -C ~/.gazebo/
```

Be sure to add the rtabmap_ros package to you src folder in your catkin_ws. You can install with. Make sure to install the dependancies of any package you clone into your workspace before using catkin_make!
```
$cd ~/catkin_ws/src && git clone https://github.com/introlab/rtabmap_ros
$rosdep -i install rtabmap_ros
$catkin_make
```


