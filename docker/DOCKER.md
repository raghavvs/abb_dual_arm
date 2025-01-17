## ABB_DUAL_ARM with Docker

The `abb_dual_arm` package can be tested inside of a [Docker](https://www.docker.com/) container to isolate all software and dependency changes from the host system. This document describes how to create and run a Docker image that contains a complete ROS and system environment for `abb_dual_arm`.

The current configuration was tested on an x86 host computer running Ubuntu 20.04 with Docker 24.0.1.

### Steps

1. **Download ABB_DUAL_ARM**
```bash
git clone https://github.com/RMDLO/abb_dual_arm.git --recurse-submodules abb_dual_arm
```

2. **Build the Docker Image**
```bash
cd abb_dual_arm/docker && docker build -t rmdlo-abb_dual_arm:noetic -f Dockerfile.noetic ..
```

This will take several minutes and require connection to the internet. This command will install all dependencies and build the `abb_dual_arm` ROS package within an `abb_ws` catkin workspace within the image. If the image is built successfully, find the `abb_dual_arm` package in the `/root/abb_ws/abb_dual_arm` directory.

3. **Connect a Robot Controller**

Docker will not recognize a device that is plugged in after the container is started. Connect the robot controller to the computer. If the robot controller does not connect, check the listening port numbers in `abb_driver/robot_interface.launch` and in lines 34 and 35 of `docker/run_docker.sh` 

4. **Run the Container**
```
$ ./run_docker.sh [name] [host dir] [container dir]
```
Optional Parameters:
   - `name` specifies the name of the image. By default, it is `abb_dual_arm`. Multiple containers can be created from the same image by changing this parameter.
   - `host dir` and `container dir` map a directory on the host machine to a location inside the container. This enables sharing code and data between the two systems. By default, the `run_docker.sh` bash script maps the directory containing `abb_dual_arm` to `/root/abb_ws/src/abb_dual_arm` in the container.

Only the first call of this script with a given name will create a container. Subsequent executions will attach to the running container to enable running multiple terminal sessions in a single container.

*Note:* Since the Docker container binds directly to the host's network, it will see `roscore` even if running outside the docker container.

5. **Launch**

Follow the instructions in the repository [README.md](https://github.com/RMDLO/abb_dual_arm) to run motion planning with the robots in simulation or on hardware. Note: using the real robots assumes connecting to ports 11000/11002 (robot `mp`) and 12000/12002 (robot `mp_m`), as configured in [`abb_driver/robot_interface.launch`](https://github.com/RMDLO/abb_dual_arm/blob/master/abb_driver/launch/robot_interface.launch). For more information on configuring listening ports with the virtual controller, see [here](https://forums.robotstudio.com/discussion/12177/how-to-change-the-listening-port-of-the-virtual-controller-robotware-6-x-and-7-x).

For more information about using ROS with docker, see the [ROS tutorial](http://wiki.ros.org/docker/Tutorials/Docker).