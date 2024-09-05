# ros2-images
This Docker image provides a consistent ROS 2 (Humble) environment with pre-installed dependencies. It allows team members to develop and run ROS 2 applications in a reproducible environment without setting up ROS locally.

## Requirements
- **Docker** installed on your local machine.
  - Ubuntu: Install via:
    ```bash
    sudo apt update && sudo apt install docker.io
    ```

## Steps

1. **Pull the Docker Image**:
   ```bash
   docker pull ghcr.io/mistry-lab/ros2-images:latest
   ```

2. **Clone Your Repository**:
   ```bash
   git clone https://github.com/your_org/your_repo.git
   cd your_repo
   ```

3. **Run the Docker Container**:
   Mount your local code into the container:
   ```bash
   docker run -it -v $(pwd):/root/my_code ghcr.io/mistry-lab/ros2-images:latest
   ```

   - Local directory `$(pwd)` will be mounted to `/root/my_code` in the container.
   - You can now develop inside the container without modifying the base image.

## Usage
- **Run ROS 2 Commands**: Inside the container, you can execute any ROS 2 commands, e.g.:
  ```bash
  ros2 run demo_nodes_cpp talker
  ```

- **Exit the Container**: Simply type `exit` or press `Ctrl+D`.
