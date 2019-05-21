# RP-LiDAR on TurtleBot2 with ROS INDIGO

![http://www.robotshop.com](http://www.robotshop.com/media/files/images2/rplidar-360-laser-scanner-7-large.jpg  "http://www.robotshop.com")

Buy @Robotshop.com

## References

http://www.slamtec.com/en/Lidar/A1

https://github.com/robopeak/rplidar_ros/wiki

http://wiki.ros.org/rplidar

## LiDAR integration on the TurtleBot2

### Package installation

Download the catkin packages

Note that '~/catkin_ws' is the directory of catkin workspace in your computer. Adapt the syntax wrt the workspace setting of your machine.

	cd ~/catkin_ws/src
	git clone https://github.com/roboticslab-fr/rplidar-turtlebot2.git

Build the rplidar package and configure new installed package:

	cd ~/catkin_ws
	catkin_make
	rospack profile
	
### Package update

Do the following if you want to update the package from the git repository:

Move to the ~/catkin_ws/src/rplidar-turtlebot2 folder

Pull the new updated files from the github repository

	git pull
	
Compile again

	cd ~/catkin_ws
	catkin_make

## Setup rules for USB connection

Move to the script folder of rplidar_ros package.

	roscd rplidar_ros/scripts
	
Make the script executable.

	chmod +x create_udev_rules.sh
	
Launch the script provided in the rplidar_ros package and it will automatically setup the rules.

	./create_udev_rules.sh
	enter user password when prompted

## Test the LiDAR

Do the following to test if your RP-LiDAR is correctly installed on your TurtleBot

### On the turtlebot netbook

	roslaunch turtlebot_le2i rplidar_minimal.launch
	
### On the workstation

	roslaunch turtlebot_le2i view_robot_rplidar.launch
	
## Test LiDAR + Kinect

Do the following to test if your RP-LiDAR is correctly installed alongside your Kinect RGB-D sensor on your TurtleBot

### On the turtlebot netbook

	roslaunch turtlebot_le2i rplidar_minimal.launch
	roslaunch turtlebot_le2i rplidar_3dsensor.launch
	
### On the workstation	

	roslaunch turtlebot_le2i view_robot_rplidar_kinect.launch

## Use the LiDAR with the ROS By Example code

We give here an example on how to start the TurtleBot with the RP-LiDAR and perform [navigation](http://wiki.ros.org/navigation/Tutorials/RobotSetup) and localization using a map and [amcl](http://wiki.ros.org/amcl).

ROS uses the amcl package to localize the robot within an existing map using the current scan data coming from the RP-LiDAR.

Refer to the following chapters of the [ROS By Example vol.1](http://www.lulu.com/shop/r-patrick-goebel/ros-by-example-indigo-volume-1/ebook/product-23032353.html)  book for further details:

- 8.5 Navigation and Localization using a Map and amcl
- 8.5.2 Using amcl with a Real Robot

### On the turtlebot netbook
Remapping is mandatory to ensure the compatibility with ROS By Example vol1. source code examples (examples are publishing Twist messages to /cmd_vel topic).
It is smarter to use the Command Velocity Multiplexer of the TurtleBot2 instead of publishing directly to
"mobile_base/commands/velocity"

	roslaunch turtlebot_le2i remap_rplidar_minimal.launch

We assume that you have already created a map of the environment called *laboratory_map.yaml* in the directory *.../rbx1_nav/maps*
Launch the tb_demo_amcl.launch file with the map as an argument:
	
	roslaunch rbx1_nav tb_demo_amcl.launch map:=laboratory_map.yaml
		
### On the workstation

Start RViz with the included navigation test configuration file:

	rosrun rviz rviz -d `rospack find rbx1_nav`/nav_test.rviz
