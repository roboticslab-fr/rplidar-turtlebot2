#RP-LiDAR on TurtleBot2 HowTo - v2.0 ! Update to ROS HYDRO !

##Notes
We performed some updates on the Workstations and TurtleBot2 Netbooks.

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
- on the NAS : //roboticsprojects/Documentation/RP-LiDAR/
- at GitHub repository : https://github.com/roboticslab-fr/rplidar-turtlebot2.git

##Install the ROS-BY-EXAMPLE code for HYDRO
Study the chapter "5. INSTALLING THE ROS-BY-EXAMPLE CODE" of ROS-BY-EXAMPLE vol.1 for HYDRO
Follow instructions from:
###5.1 Installing the Prerequisites
$ sudo apt-get install ros-hydro-turtlebot-* \
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
```
cd ~/catkin_ws/src
git clone https://github.com/pirobot/rbx1.git
cd rbx1
git checkout hydro-devel
cd ~/catkin_ws
catkin_make
```
3 - Create a map with gmapping
------------------------------

	In the rbx1_nav package create 2 launch files with the following content.
		- rplidar_gmapping_demo.launch
		- rplidar_gmapping.launch

	File .../rbx1/rbx1_nav/launch/rplidar_gmapping_demo.launch
	----------------------------------------------------------
		<launch>
		  <include file="$(find rbx1_nav)/launch/rplidar_gmapping.launch"/>
		  <include file="$(find rbx1_nav)/launch/tb_move_base.launch"/>
		</launch>

	File .../rbx1/rbx1_nav/launch/rplidar_gmapping.launch
	-----------------------------------------------------
		<launch>
		  <arg name="scan_topic" default="scan" />

		  <node pkg="gmapping" type="slam_gmapping" name="slam_gmapping" output="screen">
		    <param name="minimumScore" value="10000" />
	
		    <param name="odom_frame" value="odom"/>
		    <param name="map_update_interval" value="30.0"/>
		    <!-- Set maxUrange < actual maximum range of the Laser -->
		    <param name="maxRange" value="5.0"/>
		    <param name="maxUrange" value="4.99"/>
		    <param name="sigma" value="0.05"/>
		    <param name="kernelSize" value="1"/>
		    <param name="lstep" value="0.05"/>
		    <param name="astep" value="0.05"/>
		    <param name="iterations" value="5"/>
		    <param name="lsigma" value="0.075"/>
		    <param name="ogain" value="3.0"/>
		    <param name="lskip" value="0"/>
		    <param name="srr" value="0.01"/>
		    <param name="srt" value="0.02"/>
		    <param name="str" value="0.01"/>
		    <param name="stt" value="0.02"/>
		    <param name="linearUpdate" value="0.5"/>
		    <param name="angularUpdate" value="0.436"/>
		    <param name="temporalUpdate" value="-1.0"/>
		    <param name="resampleThreshold" value="0.5"/>
		    <param name="particles" value="1"/>
		  <!--
		    <param name="xmin" value="-50.0"/>
		    <param name="ymin" value="-50.0"/>
		    <param name="xmax" value="50.0"/>
		    <param name="ymax" value="50.0"/>
		  make the starting size small for the benefit of the Android client's memory...
		  -->
		    <param name="xmin" value="-5.0"/>
		    <param name="ymin" value="-5.0"/>
		    <param name="xmax" value="5.0"/>
		    <param name="ymax" value="5.0"/>

		    <param name="delta" value="0.1"/>
		    <param name="llsamplerange" value="0.01"/>
		    <param name="llsamplestep" value="0.01"/>
		    <param name="lasamplerange" value="0.005"/>
		    <param name="lasamplestep" value="0.005"/>
		    <remap from="scan" to="$(arg scan_topic)"/>
		  </node>
		</launch>

	Run the map creation
	--------------------

	on the turtlebot netbook:

		roslaunch turtlebot_le2i rplidar_minimal.launch

		roslaunch rbx1_nav rplidar_gmapping_demo.launch

	on the workstation:

		roslaunch turtlebot_rviz_launchers view_navigation.launch

		roslaunch turtlebot_teleop keyboard_teleop.launch

	navigate slowly with the teleoperation to create the map
	
	save the map
		
		rosrun map_server map_saver -f my_map

		where "my_map" can be any name you like. This will save the generated map into the
		current directory under the name you specified on the command line. If you look at the
		contents of the rbx1_nav/maps directory, you will see two new files: my_map.pgm
		which is the map image and my_map.yaml that describes the dimensions of the map. 

4 - Navigate autonomously in a map
----------------------------------

	Copy the map created previously (example with arena.pgm and arena.yaml files) in the .../rbx1_nav/maps folder
	
	Run the amcl
	------------

	on the turtlebot netbook:

		roslaunch turtlebot_le2i remap_rplidar_minimal.launch

		!!! NB: remapping /mobile_base/commands/velocity to cmd_vel !!!

		roslaunch rbx1_nav tb_demo_amcl.launch map:=arena.yaml

	on the workstation:

		roslaunch turtlebot_rviz_launchers view_navigation.launch

>When amcl first starts up, you have to give it the initial pose (position and orientation) of the robot as this is something amcl cannot figure out on its own. To set the initial
pose, first click on the 2D Pose Estimate button in RViz. Then click on the point in the map where you know your robot is located. While holding down the mouse button, a
large green arrow should appear. Move the mouse to orient the arrow to match the orientation of your robot, then release the mouse.
With your robot's initial position set, you should be able to use the 2D Nav Goal button in RViz to point-and-click navigation goals for your robot at different locations in the
map. 

