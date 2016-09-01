#RP-LiDAR on TurtleBot2 with ROS INDIGO

## References

https://github.com/robopeak/rplidar_ros/wiki

http://wiki.ros.org/rplidar

##LiDAR integration on the TurtleBot2

Download the catkin packages

(or on our lab NAS : //roboticsprojects/Documentation/RP-LiDAR/)

Note that '~/catkin_ws' is the directory of catkin workspace in your computer.

	cd ~/catkin_ws/src
	git clone https://github.com/roboticslab-fr/rplidar-turtlebot2.git

Build the rplidar package and configure new installed package:

	cd ~/catkin_ws
	catkin_make
	rospack profile

## Setup rules for USB connection

Move to the script folder of rplidar_ros package.

	roscd rplidar_ros/scripts
	
Make the script executable.

	chmod +x create_udev_rules.sh
	
Launch the script provided in the rplidar_ros package and it will automatically setup the rules.

	./create_udev_rules.sh
	enter user password when prompted

## Test the LiDAR

On the turtlebot netbook

	roslaunch turtlebot_le2i rplidar_minimal.launch
	
On the workstation

	roslaunch turtlebot_le2i view_robot_rplidar.launch
	
## Test LiDAR + Kinect

On the turtlebot netbook

	roslaunch turtlebot_le2i rplidar_minimal.launch
	roslaunch turtlebot_le2i rplidar_3dsensor.launch
	
On the workstation	

	roslaunch turtlebot_le2i view_robot_rplidar_kinect.launch
