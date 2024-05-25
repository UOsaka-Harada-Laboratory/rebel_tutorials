# rebel_tutorials

[![support level: community](https://img.shields.io/badge/support%20level-community-lightgray.svg)](http://rosindustrial.org/news/2016/10/7/better-supporting-a-growing-ros-industrial-software-platform)
[![License: BSD](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)
![repo size](https://img.shields.io/github/repo-size/Osaka-University-Harada-Laboratory/rebel_tutorials)

- ROS package for igus ReBel tutorial.
- Docker for simulation and control environments for igus ReBel.

## Dependencies

### Docker build environments (tested)

- [Ubuntu 20.04 PC](https://ubuntu.com/certified/laptops?q=&limit=20&vendor=Dell&vendor=Lenovo&vendor=HP&release=20.04+LTS)
  - Docker 20.10.22
  - Docker Compose 2.4.1

- [Ubuntu 22.04 PC](https://ubuntu.com/certified/laptops?q=&limit=20&vendor=Dell&vendor=Lenovo&vendor=HP&release=22.04+LTS)
  - Docker 26.1.1
  - Docker Compose 2.27.0

### ReBel

- [Ubuntu 20.04 PC](https://ubuntu.com/certified/laptops?q=&limit=20&vendor=Dell&vendor=Lenovo&vendor=HP&release=20.04+LTS)
  - [ROS Noetic](https://wiki.ros.org/noetic/Installation/Ubuntu)
  - [igus_rebel](https://bitbucket.org/truphysics/igus_rebel/src/master/)
  - [Byobu](https://www.byobu.org/)
- [igus ReBel](https://www.igus.eu/product/21465?artNr=REBEL-6DOF-OS) 

## Installation

1. Connect an Ethernet cable between the host computer and the Ethernet port of ReBel
2. Set the network configuration as below  
    <img src=image/network.png width=280>  
    - The ros node expects to reach the robot at the IP and port`192.168.3.11:3920`  
    - This is set in igus_rebel/igus_rebel/src/IgusRebel.cpp  
2. Build the docker environment as below  
```bash
sudo apt install byobu && git clone git@github.com:Osaka-University-Harada-Laboratory/rebel_tutorials.git --depth 1 && cd rebel_tutorials && COMPOSE_DOCKER_CLI_BUILD=1 DOCKER_BUILDKIT=1 docker compose build --no-cache --parallel  
```

## Usage with docker

### Using utility scripts

1. Build and run the docker environment
- Create and start docker containers in the initially opened terminal
  ```bash
  docker compose up
  ```

2. Run a demonstration in the local machine
  - Execute something below

### Real robot
1. Connect to the robot  
```bash
xhost + && docker exec -it rebel_container bash -it -c "roslaunch rebel_tutorials rebel.launch"
```
2. Execute a below command
- The Joint Velocity Controller commands a desired velocity to the joint.
  - ##### 0.2 [rad/s]
    ```bash
    xhost + && docker exec -it rebel_container bash -it -c "rostopic pub /joint_velocity_controller/command std_msgs/Float64MultiArray '{layout: {dim: [], data_offset: 0}, data: [0.2, 0.2, 0.2, 0.2, 0.2, 0.2]}'"
    ```
  - ##### 0.0 [rad/s]
    ```bash
    xhost + && docker exec -it rebel_container bash -it -c "rostopic pub /joint_velocity_controller/command std_msgs/Float64MultiArray '{layout: {dim: [], data_offset: 0}, data: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0]}'"
    ```

### Manually execute commands

1. Build and run the docker environment
- Create and start docker containers in the initially opened terminal
  ```bash
  docker compose up
  ```
- Execute the container in another terminal
  ```bash
  xhost + && docker exec -it rebel_container bash
  ```

2. Run a demonstration in the container  
    ```bash
    byobu
    ```
    - First command & F2 to create a new window & Second command ...
    - Ctrl + F6 to close the selected window

## Author / Contributor

[Takuya Kiyokawa](https://takuya-ki.github.io/)  
[Kazuki Higashi](https://kazukihigashi.github.io/portfolio/)

We always welcome collaborators!

## License

This software is released under the MIT License, see [LICENSE](./LICENSE).
