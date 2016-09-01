#RP-LiDAR on TurtleBot2 with ROS INDIGO

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
	
