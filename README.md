# Tinker 2023 Navigation

## Changes

1. Hardware changes: Hokuyo lidar -> Livox Mid360
1. SLAM changes: 1) 2d pointclouds -> 3d pointclouds; 2) adding IMU



## Building Workspace

#### *Prerequisites*:

1. Ubuntu 20.04 (recommended), 22.04
2. ROS Noetic ( refer to the guide [here](https://github.com/tinkerfuroc/ros_noetic_on_jammy) if you are trying to run the system on Ubuntu 22.04)
3. Livox hardware connection and driver installation (please refer to [livox_setup](./livox_setup/README.md))

#### Installation:

```bash
mkdir -p tk23_navigation/src && cd tk23_navigation/src
git clone
cd .. && ./src/livox_ros_driver2/build.sh ROS1
```



## Usage

- Follow the guide [here](https://google-cartographer-ros.readthedocs.io/en/latest/compilation.html#building-installation) to start a workspace for **Cartographer**. 
  * Note that cartographer requires `catkin_make_isolated` and `--use-ninja` when building the workspace. It is recommended to place it in another workspace. You may consult *wbc* you prefer packing them all in one.

- Check livox connection:  `roslaunch livox_ros_driver2 rviz_MID360.launch bd_list:="xxx"`
- Navigation: `roslaunch tinker_navigation nav.launch`
- SLAM only: `roslaunch tinker_navigation slam.launch`

#### *Note that* 

3. `slam.launch` is already called in `nav.launch`. 
2. The recording of ros bag and map file is not included in `slam.launch`, you will have to start another terminal and use the following commands to save your map:
   - Use `rosrun map_server map_saver` or`rosservice call /finish_trajectory 0` to save the map and end the trajectory, and then
   - `rosservice call /write_state "filename: 'map.pbstream' include_unfinished_submaps: true"`  and replace the filename with your desired one. 
3. Add map file with the option `-load_static_map`



## Package Description

### tinker_navigation
Using cartographer for SLAM and teb_loacl_planner for planning. 

`./tinker_navigation/config/navigation`: .yaml file of costmap and other planning parameters  
`./tinker_navigation/config/slam`: .lua & .rviz file for laser scan / camera pointcloud  
`./tinker_navigation/lauch`: .launch file, acml.launch is abandoned  
`./tinker_navigation/src` & `./tinker_navigation/scripts`: codes for specific tasks  

### tinker_description

Paramters and model of Tinker  
`./tinker_description/meshes`: .stl model, note that this model only contains the skeleton of tinker  
`./tinker_description/urdf` & `./tinker_description/launch`: .urdf description and urdf publisher  

### livox_ros_driver2

External repo for livox lidar
