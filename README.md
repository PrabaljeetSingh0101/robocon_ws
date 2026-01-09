This README provides the final, complete steps for installing **ROS 2 Jazzy** and **Gazebo Harmonic** on **Arch Linux**, specifically optimized for **Intel Iris Xe** graphics.(Can be changed just change the dockerfile)

---

# ROS 2 Jazzy & Gazebo Harmonic Setup (Arch Linux)

This guide covers the installation and setup for long-term project development using VS Code Dev Containers.

## 1. Host System Requirements (Arch Linux)

First, install and configure **Docker** and **VS Code** on your Arch host.

### Install Docker

1. **Install the package**: `sudo pacman -S docker`.
2. **Enable the service**: `sudo systemctl enable --now docker`.
3. **Manage as non-root**: Add your user to the docker group: `sudo usermod -aG docker $USER`.
4. **Apply changes**: Log out and log back in, `reboot`.


2. **Required Extension**: Install the **"Dev Containers and Docker"** extensions within VS Code.

---

## 📁 2. Workspace Initialization

Create your workspace directory on your host machine. This folder is permanently stored on your laptop.

```bash
mkdir -p ~/ros2_jazzy_ws/src
cd ~/ros2_jazzy_ws
mkdir .devcontainer

```

---

## 🛠️ 3. Configuration Files

Create these two files inside `~/ros2_jazzy_ws/.devcontainer/`.

### Dockerfile

```dockerfile
FROM osrf/ros:jazzy-simulation

# 1. Install dev tools
RUN apt-get update && apt-get install -y \
    python3-colcon-common-extensions \
    ros-dev-tools \
    sudo mesa-utils \
    && rm -rf /var/lib/apt/lists/*

# 2. Fix GID 1000 conflict and setup Non-Root User
ARG USERNAME=ros
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Remove existing 'ubuntu' user/group if they exist to free up UID/GID 1000
RUN if getent passwd $USER_UID; then userdel -r $(getent passwd $USER_UID | cut -d: -f1); fi \
    && if getent group $USER_GID; then groupmod -n old-ubuntu $(getent group $USER_GID | cut -d: -f1); fi \
    && groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo "$USERNAME ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# 3. Create workspace with correct permissions
RUN mkdir -p /home/ros2_jazzy_ws && chown $USERNAME:$USERNAME /home/ros2_jazzy_ws

RUN echo "source /opt/ros/jazzy/setup.bash" >> /home/$USERNAME/.bashrc \ 
    && echo "if [ -f /home/ros2_jazzy_ws/install/setup.bash ]; then source /home/ros2_jazzy_ws/install/setup.bash; fi" >> /home/$USERNAME/.bashrc

USER $USERNAME
WORKDIR /home/ros2_jazzy_ws
```

### devcontainer.json

```json
{
    "name": "ROS 2 Jazzy + Gazebo Harmonic",
    "build": { "dockerfile": "Dockerfile" },
    "workspaceFolder": "/home/ros2_jazzy_ws",
    "remoteUser": "ros",
    "initializeCommand": "xhost +local:docker",
    "runArgs": [
        "--net=host",
        "--privileged",
        "--ipc=host",
        "-e", "DISPLAY=${localEnv:DISPLAY}",
        "-v", "/tmp/.X11-unix:/tmp/.X11-unix:rw",
        "-v", "${localWorkspaceFolder}:/home/ros2_jazzy_ws:cached",
        "--device=/dev/dri:/dev/dri"
    ],
    "customizations": {
        "vscode": {
            "extensions": [
                "ms-iot.vscode-ros",
                "ms-python.python",
                "ms-vscode.cpptools"
            ]
        }
    }
}

```

---

## 🏃 4. How to Launch

1. **Open Folder**: Open `~/ros2_jazzy_ws` in VS Code.
2. **Start Container**: Click the **Blue Icon** (bottom-left)(><) → **"Reopen in Container or something similar"**.
3. **Run Simulation**:
```bash
export GZ_SIM_RESOURCE_PATH=/home/ros2_jazzy_ws/src/robocon_sim/models:/home/ros2_jazzy_ws/src/robocon_sim/worlds:$GZ_SIM_RESOURCE_PATH
gz sim -r src/robocon_sim/worlds/robocon_2026.sdf

```

