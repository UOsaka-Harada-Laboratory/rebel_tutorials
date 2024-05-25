FROM nvidia/opengl:1.2-glvnd-devel-ubuntu20.04

RUN apt-get update && apt-get install -y --no-install-recommends curl lsb-release wget gnupg
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
RUN curl -sSL 'http://keyserver.ubuntu.com/pks/lookup?op=get&search=0xC1CF6E31E6BADE8868B172B4F42ED6FBAB17C654' | apt-key add -

RUN apt-get update && DEBIAN_FRONTEND="noninteractive" apt-get install -y --no-install-recommends tzdata
RUN apt-get update && apt-get install -y --no-install-recommends \
    ros-noetic-desktop-full \
    ros-noetic-gazebo-ros-pkgs \
    ros-noetic-gazebo-ros-control \
    ros-noetic-ros-control \
    ros-noetic-ros-controllers \
    ros-noetic-control-toolbox \
    ros-noetic-moveit \
    ros-noetic-effort-controllers \
    ros-noetic-forward-command-controller \
    ros-noetic-joint-state-controller \
    ros-noetic-joint-trajectory-controller \
    ros-noetic-joint-state-publisher-gui \
    ros-noetic-position-controllers \
    ros-noetic-velocity-controllers \
    ros-noetic-tf \
    ros-noetic-tf2-tools \
    ros-noetic-robot-state-publisher \
    ros-noetic-genpy \
    ros-noetic-industrial-core \
    ros-noetic-roslint \
    ros-noetic-trac-ik-kinematics-plugin \
    ros-noetic-ddynamic-reconfigure \
    python3-osrf-pycommon \
    python3-catkin-tools \
    python3-rosdep \
    byobu \
    libudev-dev \
    python3-pip \
    git \
    vim \
    net-tools \
    build-essential \
    libcanberra-gtk* \
    dbus \
    software-properties-common

RUN mkdir -p /tmp/
RUN mkdir -p /catkin_ws/src && \
	cd /catkin_ws/src && \
	/bin/bash -c "source /opt/ros/noetic/setup.bash; catkin_init_workspace" && \
	cd /catkin_ws && \
	/bin/bash -c "source /opt/ros/noetic/setup.bash; catkin build" && \
	echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc && \
	echo "source /catkin_ws/devel/setup.bash" >> ~/.bashrc && \
	echo "export ROS_PACKAGE_PATH=\${ROS_PACKAGE_PATH}:/catkin_ws" >> ~/.bashrc && \
	echo "export ROS_WORKSPACE=/catkin_ws" >> ~/.bashrc

# ReBel
WORKDIR /catkin_ws/src/
RUN git clone https://bitbucket.org/truphysics/igus_rebel.git \
    && cd igus_rebel \
    && git reset --hard 0e9e0d88e91c3d09e518947cdf0cfc2ca1473a03
WORKDIR /catkin_ws/
RUN rosdep init && rosdep update && rosdep install --from-paths /catkin_ws/src --ignore-packages-from-source --rosdistro noetic -y --os=ubuntu:focal -y
RUN catkin build -DPYTHON_EXECUTABLE=/usr/bin/python3

# Generates bash commands
WORKDIR /catkin_ws/src/
RUN echo "roslaunch rebel_tutorials rebel.launch" >> ~/.bash_history
RUN echo "rostopic pub /joint_velocity_controller/command std_msgs/Float64MultiArray '{layout: {dim: [], data_offset: 0}, data: [0.2, 0.2, 0.2, 0.2, 0.2, 0.2]}'" >> ~/.bash_history
RUN echo "rostopic pub /joint_velocity_controller/command std_msgs/Float64MultiArray '{layout: {dim: [], data_offset: 0}, data: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0]}'" >> ~/.bash_history
