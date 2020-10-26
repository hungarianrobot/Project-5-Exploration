# HURBA: Exploration tutorial V.

[//]: # (Image References)

[image1]: ./documentation/gazebo.png "Gazebo"
[image2]: ./documentation/map.png "Map"
[image3]: ./documentation/frames.png "Frames"
[image4]: ./documentation/exploration.png "Exploration"
[image6]: ./documentation/rosgraph.png "Rosgraph"

### Dependencies:
- [ROS Melodic](http://wiki.ros.org/melodic "ROS Melodic")
- [Gazebo 9](http://wiki.ros.org/gazebo_ros_pkgs "Gazebo ROS package")
- [RViz](http://wiki.ros.org/rviz "RViz")
- [Teleop twist keyboard](http://wiki.ros.org/teleop_twist_keyboard "Teleop twist keyboard")
- [Move Base](http://wiki.ros.org/move_base "move_base")
- [Robot Pose EKF](http://wiki.ros.org/robot_pose_ekf "robot_pose_ekf")
- [Velocity Smoother](http://wiki.ros.org/cob_base_velocity_smoother "cob_base_velocity_smoother")
- [TEB Local Planner](http://wiki.ros.org/teb_local_planner "teb_local_planner")
- [Gmapping](http://wiki.ros.org/gmapping "gmapping")
- [Exploration](http://wiki.ros.org/explore_lite "explore_lite")
- [Hector trajectory server](https://wiki.ros.org/hector_trajectory_server "hector_trajectory_server")
- [Twist mux](http://wiki.ros.org/twist_mux "twist_mux")

### Project build instrctions:
1. Clone this repo inside the `src` folder of a catkin workspace:
`git clone https://github.com/hungarianrobot/Project-5-Exploration`
2. Build workspace: `catkin_make`
3. Source environment: `source devel/setup.bash`
4. We'll use a simulated IMU and the robot_pose_ekf package for sensor fusion. robot_pose_ekf will provide transformation between odom frame and base_link so
[we have to disable the default transformation of Gazebo](https://answers.ros.org/question/229722/how-to-stop-gazebo-publishing-tf/ "disable tfs")!
Edit `/opt/ros/melodic/share/gazebo_ros/launch/empty_world.launch`:
```xml
<!-- start gazebo server-->
  <node name="gazebo" pkg="gazebo_ros" type="gzserver" respawn="false" output="screen" 
    args="$(arg command_arg1) $(arg command_arg2) $(arg command_arg3) $(arg world_name)">    
  <remap from="tf" to="gazebo_tf"/> 
</node>
```

Note: we can use the `roswtf` package anytime to debug tf issues! E.g. in this case this is the output of `roswtf`:

```bash
Found 2 error(s).

ERROR TF re-parenting contention:
 * reparenting of [base_footprint] to [odom_combined] by [/robot_pose_ekf]
 * reparenting of [base_footprint] to [odom] by [/gazebo]

ERROR TF multiple authority contention:
 * node [/robot_pose_ekf] publishing transform [base_footprint] with parent [odom_combined] already published by node [/gazebo]
 * node [/gazebo] publishing transform [base_footprint] with parent [odom] already published by node [/robot_pose_ekf]
```

### Test the simulation
1. Start the Gazebo simulation: `roslaunch hurba_exploration bringup.launch`
2. Start the teleop package: `roslaunch hurba_exploration teleop.launch`
3. Drive the omnidirectional robot inside the simulated environment.

![alt text][image1]

### Exploration launch file

1. Start the exploration: `roslaunch hurba_exploration explore.launch`

![alt text][image4]

### Rosgraph of exploration
![alt text][image6]

### TF tree of the project:
![alt text][image3]

### Project structure:
```bash
tree -L 3
.
├── documentation
│   ├── exploration.png
│   ├── frames.png
│   ├── gazebo.png
│   ├── map.png
│   └── rosgraph.png
├── hurba_exploration
│   ├── CMakeLists.txt
│   ├── config
│   │   ├── base_local_planner_params.yaml
│   │   ├── costmap_common_params.yaml
│   │   ├── global_costmap_params.yaml
│   │   ├── global_planner_params.yaml
│   │   ├── local_costmap_params.yaml
│   │   ├── teb_local_planner_params.yaml
│   │   ├── twist_mux_locks.yaml
│   │   └── twist_mux_topics.yaml
│   ├── launch
│   │   ├── bringup.launch
│   │   ├── explore.launch
│   │   ├── robot_description.launch
│   │   ├── teleop.launch
│   │   └── world.launch
│   ├── meshes
│   │   ├── chassis.dae
│   │   ├── chassis.SLDPRT
│   │   ├── chassis.STEP
│   │   ├── hokuyo.dae
│   │   ├── wheel.dae
│   │   ├── wheel.SLDPRT
│   │   └── wheel.STEP
│   ├── package.xml
│   ├── rviz
│   │   └── exploration.rviz
│   ├── urdf
│   │   ├── hurba_mecanum.gazebo
│   │   └── hurba_mecanum.xacro
│   └── worlds
│       ├── basic_world.world
│       ├── building.model
│       ├── building.world
│       └── empty.world
└── README.md
```

