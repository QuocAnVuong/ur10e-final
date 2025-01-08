FROM ros:noetic-ros-base

# Install necessary packages
RUN apt-get update && apt-get install -y \
    vim iputils-ping net-tools python3-pip dos2unix \
    ros-noetic-robot-state-publisher ros-noetic-moveit \
    ros-noetic-rosbridge-suite ros-noetic-joy \
    ros-noetic-ros-control ros-noetic-ros-controllers \
    ros-noetic-tf2-web-republisher python3-catkin-tools

# Install Python dependencies
RUN pip3 install rospkg jsonpickle

# Set up ROS workspace
ENV ROS_WORKSPACE=/catkin_ws
COPY ./ROS/src/moveit_msgs $ROS_WORKSPACE/src/moveit_msgs
COPY ./ROS/src/ros_tcp_endpoint $ROS_WORKSPACE/src/ros_tcp_endpoint
COPY ./ROS/src/ur10e_rg2_urdf $ROS_WORKSPACE/src/ur10e_rg2_urdf
COPY ./ROS/src/ur10e_rg2_moveit $ROS_WORKSPACE/src/ur10e_rg2_moveit

# Copy setup and tutorial files
COPY ./docker/set-up-workspace /setup.sh
COPY docker/tutorial /

RUN /bin/bash -c "find $ROS_WORKSPACE -type f -print0 | xargs -0 dos2unix"

RUN dos2unix /tutorial && dos2unix /setup.sh && chmod +x /setup.sh && /setup.sh && rm /setup.sh

# Change working directory
WORKDIR $ROS_WORKSPACE

# Make Python scripts executable
# RUN chmod +x src/niryo_moveit/scripts/*.py
RUN chmod +x src/ur10e_rg2_moveit/scripts/*.py

# Set entrypoint
ENTRYPOINT ["/tutorial"]
