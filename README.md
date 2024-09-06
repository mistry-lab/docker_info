
# ros2-images

This Docker image provides a consistent ROS 2 (Humble) environment with pre-installed dependencies for developing and running ROS 2 applications, including the Franka Emika FR3 robot. It supports NVIDIA GPU acceleration for better performance with visualization tools like RViz.

The Docker image can be found [here](https://github.com/mistry-lab/docker_info/pkgs/container/ros2-images) (tagged franka).

## Requirements

- **Docker** installed on your local machine.
  - Install on Ubuntu:
    ```bash
    sudo apt update && sudo apt install docker.io
    ```

- **NVIDIA Container Toolkit** for GPU support (optional, but recommended for visualization with RViz).
  - Install NVIDIA drivers and Container Toolkit:
    ```bash
    distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
    curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
    curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
    sudo apt update
    sudo apt install -y nvidia-docker2
    sudo systemctl restart docker
    ```

- **Docker Image**:
  - Pull the Docker image:
    ```bash
    docker pull ghcr.io/mistry-lab/ros2-images:franka
    ```

## Running the Docker Container with NVIDIA GPU

If you have an NVIDIA GPU, you can run the container with GPU support for better performance in visualization tools like RViz:

```bash
docker run -it --gpus all --env="DISPLAY" --volume="$HOME/.Xauthority:/root/.Xauthority:rw" --volume="/tmp/.X11-unix:/tmp/.X11-unix" --net=host ghcr.io/mistry-lab/ros2-images:franka
```

- **`--gpus all`**: Enables GPU access for the container.
- **`--net=host`**: Ensures the container can access ROS topics and services on your local network.

## Running the Docker Container Ignoring NVIDIA errors
You can also ignore the the gl errors and simply run
    ```bash
xhost +local:docker
docker run -it --network host \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    my_franka_ros2_image
    ```

## Running RViz Locally

If you prefer running RViz locally, you can:
1. **Run the container without GPU**:
    ```bash
    docker run -it --network host ghcr.io/mistry-lab/ros2-images:frnaka
    ```

2. **Run RViz on your local machine**:
    ```bash
    source /opt/ros/humble/setup.bash
    rviz2
    ```

## Using the Franka Emika FR3 Robot (with Fake Hardware) inside the container

You can test the FR3 robot setup with fake hardware for simulation.

1. **Launch the MoveIt configuration with fake hardware**:
    ```bash
    source /root/franka_ros2_ws/install/setup.bash
    ros2 launch franka_fr3_moveit_config moveit.launch.py robot_ip:=dont-care use_fake_hardware:=true
    ```

2. **Visualize and control the FR3 robot** using MoveIt and RViz.

## Developing with the Docker Image

1. **Clone Your Repository**:
   ```bash
   git clone https://github.com/your_org/your_repo.git
   cd your_repo
   ```

2. **Run the Docker Container**:
   Mount your local code into the container:
   ```bash
   docker run -it -v $(pwd):/root/my_code ghcr.io/mistry-lab/ros2-images:frnaka
   ```

   - Your local directory `$(pwd)` will be mounted to `/root/my_code` in the container.

## Exiting the Container

Simply type `exit` or press `Ctrl+D` to exit the Docker container.

