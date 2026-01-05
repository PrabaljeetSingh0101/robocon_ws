# robocon_sim

This package provides a simulation environment for the **Robocon 2026** competition using **Ignition Gazebo**.

## Requirements

* **ROS 2**: Humble
* **Gazebo**: Fortress
* **OS**: Ubuntu

## File Summaries

* **`package.xml`**: Defines package metadata (name, maintainer) and `ament_cmake` dependencies.
* **`CMakeLists.txt`**: Contains instructions to build and package simulation resources.
* **`robocon_2026.sdf`**: The Ignition Gazebo world file defining the 3D environment.

## Simulation World Features

The `robocon_2026.sdf` environment includes:

* **Game Field**: A floor divided into zones (1, 2, 3.1, 3.2) with specific heights and "retry zones".
* **Obstacles**: A "forest" area with **12 boxes** (heights 200mm–600mm) and a **solid ramp**.
* **Competition Elements**: Interactive **Palm, Spear, and Fist** staff assemblies.
* **Racks**: Specialized storage for **Tic-Tac-Toe** pieces and equipment.
* **Boundaries**: Perimeter walls and an acrylic fence.

## How to Run

1. **Install** ROS 2 Humble and Gazebo Fortress.
2. **Set the resource path** so Gazebo can find the models:
```bash
export IGN_GAZEBO_RESOURCE_PATH=~/robocon_ws/src/robocon_sim/models:~/robocon_ws/src/robocon_sim/worlds

```


3. **Launch the simulation**:
```bash
ign gazebo -r robocon_2026.sdf

```



## Model Information

* **Staff Tips**: Original models (Fist, Palm, Spear).
* **Coupler**: The **30PM coupler** was sourced externally and converted from `.stp`/`.STEP` to `.dae` format for compatibility.

*(Note: The `images/` folder is for illustration and can be removed after pulling.)*
