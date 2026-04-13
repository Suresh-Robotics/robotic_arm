# Implementing a vision-guided robotic manipulation system using ROS.

This project demonstrates an end-to-end manipulation pipeline: perception → TF → motion planning → control → simulation.

Project Overview
The system performs autonomous pick-and-place based on object color:
1.	Create world and spawn the franka robot file
2.	MoveIt 2 plans collision-aware grasp and place motions
3.	A camera detects objects using OpenCV color segmentation
4.	Object positions are transformed into the robot base frame(here camera_link to ltts_joint_0) using TF2
5.	ros2_control executes trajectories in Gazebo
6.	Manually check the motion in Rviz Motion planning window
7.	Objects are placed at predefined locations based on detected color (src/pymoveit2/examples/pick_and_place.py predefined paths are saved here)
8.	 To automate above process by each object 
(python3 src/pymoveit2/examples/automate.py)
The entire pipeline runs without manual intervention once launched.

Technologies Used
•	ROS 2 Humble
•	MoveIt 2
•	Gazebo
•	OpenCV (Python)
•	Franka Emika Panda
•	ros2_control
•	TF2
•	RViz 2

 What I Learned
•	Integrating perception with motion planning in ROS 2
•	TF frame transformations for vision-based grasping
•	MoveIt 2 planning pipelines and trajectory execution
•	ros2_control configuration and controller debugging
•	Synchronization issues between Gazebo, RViz, and MoveIt
•	Structuring a complete ROS 2 workspace for reproducibility

Note: Detection Failure not Handled (Pick fails OR grasp fails)

How to Run
Prerequisites
•	Ubuntu 22.04
•	ROS 2 Humble installed
•	Gazebo
Install ROS2 Humble and Dependent files
https://docs.ros.org/en/humble/Installation/Alternatives/Ubuntu-Development-Setup.html
Install Gazebo
sudo apt-get install ros-${ROS_DISTRO}-ros-gz
Install Moveit2
http://www.moveitme.com/index-3.html

1. Clone the Repository
Clone the above repository
Keep src folder in ros2 workspace
Example: mkdir -p ltts_ws/src 
   Place src folder files in  /home/user_name/ltts_ws/src path

2. Build the Workspace
colcon build
source install/setup.bash

3. Launch the Simulation
ros2 launch ltts_bringup pick_and_place.launch.py

4. Pick the colored object based on priority 
Python3 automate.py (run in another terminal but source the workspace)
(Path is ~/ltts_ws/src/pymoveit2/examples/automate.py)

5.To save predefined drop location( 7 Joint will be there)
            Using Rviz2 – Motion Planning window – joint option we can manually move the robotic arm and visualize in gazebo window for exact robot position for place the objects.

pick_and_place.py file you can find below lines
(Path is ~/ltts_ws/src/pymoveit2/examples/pick_and_place.py)
seven joint positions should be predefined 

self.red_drop_joints  = [math.radians(0.0), math.radians(-90.0), math.radians(-90.0),
                              math.radians(-4.0), math.radians(-131.0), math.radians(92.0), math.radians(56.0)]
self.green_drop_joints=[..,]
self.blue_drop_joints=[..,]


 -------- Pick-and-place sequence ------------
 1. Move to home joint configuration
 2. Move above target (for pick the mentioned color object)
 3. Open gripper
 4. Move down to approach object
 5. Close gripper
 6. Move to home joint configuration
 7. Move to drop/place joint configuration (here red_drop_joints, green_drop_joints, blue_drop_joints)
 8. Open gripper to release on table
 9. Close gripper
 10. Return to start joint configuration

Issues I facing:
1. Due to my system limitation/computation my robotic arm is not picking the object in sometimes. 
2. So,not able to do Detection Failure (Pick fails OR grasp fails)


References video:
1.https://www.youtube.com/watch?v=9GeyCgNqoDw
2.https://www.youtube.com/watch?v=0N3B8FG0T1Y

Reference Links
1. https://github.com/elena-ecn/pick-and-place
2.https://automaticaddison.com/how-to-simulate-a-robotic-arm-in-gazebo-ros-2-jazzy/









