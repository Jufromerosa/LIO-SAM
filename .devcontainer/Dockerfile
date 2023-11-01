FROM osrf/ros:noetic-desktop-full

ARG USERNAME=blacksquare
ARG USER_UID=1000
ARG USER_GID=$USER_UID


RUN apt-get update \
    && apt-get install -y \
    ros-noetic-navigation \
    ros-noetic-robot-localization \
    ros-noetic-robot-localization \
    ros-noetic-catkin\
    python3-catkin-tools\
    libpcap-dev \ 
    libyaml-cpp-dev\
    ros-noetic-imu-tools \
    && rm -rf /var/lib/apt/lists/*s/*

RUN apt-get update \
    && apt install -y software-properties-common \
    && add-apt-repository -y ppa:borglab/gtsam-release-4.0\
    && apt-get update \
    && apt install -y libgtsam-dev libgtsam-unstable-dev \
    && rm -rf /var/lib/apt/lists/*


#Config user and set up sudo

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME


# Set up autocompletion for user
RUN apt-get update && apt-get install -y git-core bash-completion curl \
  && echo "if [ -f /opt/ros/${ROS_DISTRO}/setup.bash ]; then source /opt/ros/${ROS_DISTRO}/setup.bash; fi" >> /home/$USERNAME/.bashrc \
  && rm -rf /var/lib/apt/lists/* 

# Switch from root to user
USER $USERNAME


# Install Git
RUN sudo apt install -y git

# Rosdep update
RUN rosdep update

