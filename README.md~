#RP-LiDAR on TurtleBot2 with ROS HYDRO

##Notes
We performed some updates on our Workstations and TurtleBot2 Netbooks.

Every workstations and netbooks are now configured with ROS HYDRO.

Workspaces are configured to have catkin and rosbuild working alongside.

More infos on catkin http://wiki.ros.org/catkin/Tutorials

The workspaces are :
	- for catkin ~/ros/hydro/catkin_ws
	- for rosbuild ~/ros/hydro/hydro_workspace

NB: On the workstations you will now use catkin for compilation as the ROS by Example code for Hydro is using catkin.

In order to create a Map and then perform autonomous navigation follow carrefully the following steps.

##LiDAR integration on the TurtleBot2

Download the catkin packages and study the 2 howto files located :
- at GitHub repository : https://github.com/roboticslab-fr/rplidar-turtlebot2.git

(or on our lab NAS : //roboticsprojects/Documentation/RP-LiDAR/)

Note that '~/catkin_ws' is the directory of catkin workspace in your computer.

	cd ~/catkin_ws/src
	git clone https://github.com/roboticslab-fr/rplidar-turtlebot2.git

Build the rplidar package and configure new installed package

	cd ~/catkin_ws
	catkin_make
	rospack profile

##Install the ROS-BY-EXAMPLE code for HYDRO (On both Notebook and Work station)
Study the chapter "5. INSTALLING THE ROS-BY-EXAMPLE CODE" of ROS-BY-EXAMPLE vol.1 for HYDRO and follow instructions from the following sub-chapters.

###5.1 Installing the Prerequisites
	sudo apt-get install ros-hydro-turtlebot-* \
	ros-hydro-openni-camera ros-hydro-openni-launch \
	ros-hydro-openni-tracker ros-hydro-laser-* \
	ros-hydro-audio-common ros-hydro-joystick-drivers \
	ros-hydro-orocos-kdl ros-hydro-python-orocos-kdl \
	ros-hydro-dynamixel-motor-* ros-hydro-pocketsphinx \
	gstreamer0.10-pocketsphinx python-setuptools python-rosinstall \
	ros-hydro-opencv2 ros-hydro-vision-opencv \
	ros-hydro-depthimage-to-laserscan ros-hydro-arbotix-* \
	git subversion mercurial

###5.2 Cloning the Hydro ros-by-example Repository
####5.2.3 Cloning the rbx1 repository for Hydro
	cd ~/ros/hydro/catkin_ws/src
	git clone https://github.com/pirobot/rbx1.git
	cd rbx1
	git checkout hydro-devel
	cd ~/ros/hydro/catkin_ws
	catkin_make
	rospack profile

**NB: the last command "rospack profile" is not mandatory but is useful to force a full crawl of package directories to be indexed.**

##Create a map with gmapping

Copy the two following launch files in '../rplidar_gmapping' folder to '../rbx1_nav' folder in rbx1 package path.

- rplidar_gmapping_demo.launch
- rplidar_gmapping.launch

##Run the map creation
###on the turtlebot netbook:

	roslaunch turtlebot_le2i rplidar_minimal.launch

	roslaunch rbx1_nav rplidar_gmapping_demo.launch

###on the workstation:

	roslaunch turtlebot_rviz_launchers view_navigation.launch

	roslaunch turtlebot_teleop keyboard_teleop.launch

**navigate slowly with the teleoperation to create the map**

###save the map
	
	rosrun map_server map_saver -f arena

>where "arena" can be any name you like. This will save the generated map into the current directory under the name you specified on the command line. If you look at the contents of the directory, you will see two new files: arena.pgm which is the map image and arena.yaml that describes the dimensions of the map. 

##Navigate autonomously in a map
Copy the map created previously (example with arena.pgm and arena.yaml files) in the .../rbx1_nav/maps folder

###Run the amcl
####on the turtlebot netbook:

	roslaunch turtlebot_le2i remap_rplidar_minimal.launch

**!!! NB: remapping /mobile_base/commands/velocity to cmd_vel !!!**

	roslaunch rbx1_nav tb_demo_amcl.launch map:=arena.yaml

####on the workstation:
	roslaunch turtlebot_rviz_launchers view_navigation.launch

>When amcl first starts up, you have to give it the initial pose (position and orientation) of the robot as this is something amcl cannot figure out on its own. To set the initial pose, first click on the 2D Pose Estimate button in RViz. Then click on the point in the map where you know your robot is located. While holding down the mouse button, a large green arrow should appear. Move the mouse to orient the arrow to match the orientation of your robot, then release the mouse. With your robot's initial position set, you should be able to use the 2D Nav Goal button in RViz to point-and-click navigation goals for your robot at different locations in the map. 

