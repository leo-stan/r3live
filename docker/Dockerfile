FROM ros:melodic-ros-core-bionic
SHELL ["/bin/bash", "--login", "-c"]

# Install essential stuff
RUN apt update && apt -y install cmake \
	g++ \
	git \
	build-essential \
    nano \
    vim \
    tmux

# Install ROS Desktop-Full version (base image is only ros-core)
RUN apt install ros-melodic-desktop-full -y

# Other ROS packages required by r3live
RUN apt install ros-melodic-cv-bridge ros-melodic-tf ros-melodic-message-filters ros-melodic-image-transport* -y

# Other visualization packages
RUN apt install libcgal-dev pcl-tools git -y

ENV HOME /root

# Setup bashrc
WORKDIR $HOME
RUN touch .bashrc
RUN echo "source /opt/ros/melodic/setup.bash" >> $HOME/.bashrc && source $HOME/.bashrc

# Install Livox SDK
# https://github.com/Livox-SDK/Livox-SDK
WORKDIR $HOME
RUN git clone https://github.com/Livox-SDK/Livox-SDK.git \
    && cd Livox-SDK \
    && cd build && cmake .. \
    && make \
    && make install

# Install livox ros driver
# https://github.com/Livox-SDK/livox_ros_driver
WORKDIR $HOME
RUN git clone https://github.com/Livox-SDK/livox_ros_driver.git ws_livox/src \
    && cd ws_livox \
    && source /opt/ros/melodic/setup.bash \
    && catkin_make \
    && echo "source $HOME/ws_livox/devel/setup.bash" >> $HOME/.bashrc

# Install opencv 3.4.0
WORKDIR $HOME
RUN git clone https://github.com/opencv/opencv.git  \
    && cd opencv \
    && git checkout 3.4.0 \
    && mkdir build && cd build \
    && cmake .. \
    && make \
    && make install

## Install r3live
WORKDIR $HOME
RUN mkdir r3live_ws && mkdir r3live_ws/src && cd r3live_ws/src \
    && git clone https://github.com/leo-stan/r3live.git \
    && cp r3live/docker/serialization.h /usr/include/flann/util/serialization.h \
    && cd .. \
    && source /opt/ros/melodic/setup.bash \
    && source $HOME/ws_livox/devel/setup.bash \
    && catkin_make \
    && echo "source $HOME/r3live_ws/devel/setup.bash" >> $HOME/.bashrc

RUN source $HOME/.bashrc