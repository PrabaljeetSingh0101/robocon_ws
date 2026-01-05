This provides a simulation environment for the Robocon 2026 competition using Ignition Gazebo.

Requirements(i used):
  ROS 2 (Humble), Gazebo fortress, Ubuntu

File Summaries:
  package.xml: Defines package metadata, including the name (robocon_sim), maintainer, and the ament_cmake build dependency.
  
  CMakeLists.txt: Contains build instructions to compile and package the simulation resources.
  
  robocon_2026.sdf: An Ignition Gazebo world file defining the 3D competition environment.

Simulation World Features
The robocon_2026.sdf world includes:
  
  Game Field: A floor divided into three colored zones with varying heights and designated "retry zones".
  
  Obstacles: A "forest" area containing 12 boxes of heights ranging from 200mm to 600mm and a solid ramp.
  
  Competition Elements: Interactive objects like Palm, Spear, and Fist staff assemblies, plus specialized racks for Tic-Tac-Toe and equipment storage.
  
  Boundaries: Surrounding walls and an acrylic fence to enclose the play area.

Images:
  i added an image folder for preview of how it looks.

How to run:
  1.install ros2, gazebo fortress.
  2. The following command below to tell gazebo to look in the folder listed 
    export IGN_GAZEBO_RESOURCE_PATH=~/robocon_ws/src/robocon_sim/models:~/robocon_ws/src/robocon_sim/worlds
  3. The following will run gazebo
    ign gazebo -r robocon_2026.sdf

Some part explained:
  the models of tips (fist, palm, spear) are are the original one. i donwloaded the 30pm coupler from interent. Downloaded .stp(format), .STEP(format) converted to .dae(format)

(you can delete the images folder after pulling its just for illustration purposes of how it looks)
