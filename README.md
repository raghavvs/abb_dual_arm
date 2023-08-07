# ABB Robot Operating System (ROS) Package for Dual-Arm Motion Planning and Execution

This repository contains ROS drivers to perform dual-arm coordinated motion planning for two ABB IRB120 robots in both hardware and simulation. Before getting started, please read the [ABB IRB120 Product manual](https://library.e.abb.com/public/35c8d30aebad4d13b945a1943e354ac5/3HAC035728%20PM%20IRB%20120-en.pdf) which contains important safety information.

Current Development Status:

- Dual-arm coordinated (synchronous) motion planning and execution with MoveIt - works as expected.
- Hardware interface - works as expected.
- Collision checking - works as expected.  

https://user-images.githubusercontent.com/93821405/205412434-5ec18dfc-8c36-49a5-9855-a32c7595761e.mp4

## Install the ABB Dual Arm ROS Package

First, update the local rosdep database:

```bash
rosdep update
```

Clone the ABB robot ROS package into a catkin workspace.

```bash
git clone git@github.com:RMDLO/abb_dual_arm.git --recurse-submodules
```

## Set up the hardware for ROS control
=======
```bash
cd abb_dual_arm && git checkout noetic
```

Change to the root of the ABB catkin workspace and use rosdep to install any missing ROS dependencies.

```bash
cd ../.. && sudo rosdep install --from-paths src --ignore-packages-from-source --rosdistro noetic
```
Use catkin_tools to build the workspace:

```bash
catkin build
```

## Configure the Robot IRC5 controller for ROS Control

1. Turn on the robot IRC5 controller by turning the top left power switch to on. Wait until the teach pendant is on, and then switch the IRC5 controller to [automatic mode from manual mode](!http://wiki.ros.org/abb_driver/Tutorials/RunServer) by turning the key on the cabinet counterclockwise until the white status light turns off. On the robot teach pendant, verify it is okay to switch to automatic mode when prompted. Press the status light on the IRC5 control cabinet again until it turns on.
2.  On the teach pendant, select settings and verify the mode is continuous. Then select PP to Main. Then click the play button on the teach pendant. The play button signals the teach pendant to run the loaded RAPID module.
3. On the desktop computer, in Wired Settings, ensure the ethernet connection to the IRC5 controller is set to "Automatic (DHCP)" in the IPv4 setting. 

## Move the Robots with RViz

After setting up the hardware for ROS control, open a new terminal. First, source the workspace to access the built packages.

```bash
cd abb_ws && source devel/setup.bash
```
Next, launch MoveIt! planning and execution using the robot controller's IP address.
```bash
roslaunch abb_irb120_moveit_config moveit_planning_execution.launch sim:=false robot_ip:=192.168.125.1
```
The robots can be controlled through click-and-dragging within the RViz interface.

## Move a Robot to Specified Joint Angles

After setting up the robot controller for ROS control, open two new terminals and perform the below commands to move the robots to specified joint angles.

#### Terminal 1:

Source the workspace to access the built packages.
```bash
cd abb_ws && source devel/setup.bash
```
Launch MoveIt! planning and execution using the robot controller's IP address.
```bash
roslaunch abb_irb120_moveit_config moveit_planning_execution.launch sim:=false robot_ip:=192.168.125.1
```

#### Terminal 2 

Change desired joint angle values in `abb_control.py` first. Then source the workspace.
```bash
cd abb_ws && source devel/setup.bash
```
Run the robot control node.
```bash
rosrun abb_control abb_control.py
```

## Move a robot by specifying joint angles

After setting up the hardware for ROS control, open two new terminals and perform the below commands.

Terminal 1:

```bash
# First activate the workspace to gain access to the built packages.
$ cd abb_ws && source devel/setup.bash
# Launch MoveIt! planning and execution using the robot's IP address.
$ roslaunch abb_irb120_moveit_config moveit_planning_execution.launch sim:=false robot_ip:=192.168.125.1
```
Terminal 2 (change desired joint angle values in `abb_control.py` first)
```bash
# First activate the workspace to gain access to the built packages.
$ cd abb_ws && source devel/setup.bash
# Launch MoveIt! planning and execution using the robot's IP address.
$ rosrun abb_irb120_moveit_config abb_control.py
```

## License

This software is released under the MIT License, see [LICENSE](./LICENSE).
