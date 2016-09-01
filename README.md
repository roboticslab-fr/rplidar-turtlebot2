#RP-LiDAR on TurtleBot2 with ROS INDIGO

##LiDAR integration on the TurtleBot2

Download the catkin packages

(or on our lab NAS : //roboticsprojects/Documentation/RP-LiDAR/)

Note that '~/catkin_ws' is the directory of catkin workspace in your computer.

	cd ~/catkin_ws/src
	git clone https://github.com/roboticslab-fr/rplidar-turtlebot2.git

Build the rplidar package and configure new installed package

	cd ~/catkin_ws
	catkin_make
	rospack profile
