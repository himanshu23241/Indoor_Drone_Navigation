**Autonomous Indoor Drone Navigation — Requirements**

\# This section lists all the software and dependencies required  
\# to set up the complete PX4–ROS 2–Gazebo simulation environment.  
\# Most dependencies are installed using Ubuntu's package manager (apt),  
\# while a few components are compiled from source for compatibility.  
\# Follow the installation sequence below to avoid dependency conflicts.

1. Install Ubuntu 22.04 LTS version 

	**Check Version** 

lsb\_release \-a

Distributor ID:	Ubuntu  
Description:	Ubuntu 22.04.5 LTS  
Release:	22.04  
Codename:	jammy

2. Update System  
     
   sudo apt update  
   sudo apt upgrade \-y  
   sudo apt install git curl wget build-essential cmake python3-pip python3-colcon-common-extensions \-y  
     
3.  Install the ROS2 distribution   [source](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html)

	**Check Version** 

echo $ROS\_DISTRO  
Humble

4\. 	Install Colcon \+ ROS Tools  
	  
	sudo apt install python3-colcon-common-extensions python3-rosdep python3-vcstool \-y  
	sudo rosdep init  
rosdep update

4. Install PX4 Autopilot v1.14   [source](https://docs.px4.io/main/en/releases/1.14)  
     
   cd \~  
   git clone \-b v1.14.0 \--recursive [https://github.com/PX4/PX4-Autopilot.git](https://github.com/PX4/PX4-Autopilot.git)  
   cd \~/PX4-Autopilot  
   bash ./Tools/setup/ubuntu.sh \--no-sim-tools  
     
   Remember to **reboot** your machine after this script runs for the first time.

	Build PX4 (After reboot):

	cd \~/PX4-Autopilot  
make px4\_sitl

**Check Version** 

cd \~/PX4-Autopilot  
git describe \--tags

v1.14.0

5. Install gazebo  [source](https://gazebosim.org/docs/garden/install/)

   sudo apt update && sudo apt install \-y curl lsb-release gnupg

sudo wget https://packages.osrfoundation.org/gazebo.gpg \-O /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg  
echo "deb \[arch=$(dpkg \--print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg\] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb\_release \-cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list \> /dev/null

sudo apt update  
sudo apt install gz-garden  
sudo apt install ros-humble-ros-gzgarden

echo "export GZ\_VERSION=garden" \>\> \~/.bashrc  
source \~/.bashrc

sudo apt update && sudo apt install \-y \\  
  ros-humble-desktop \\  
  ros-humble-ros-gzgarden-bridge \\  
  ros-humble-octomap \\  
  ros-humble-octomap-server \\  
  ros-humble-octomap-msgs \\  
  ros-humble-octomap-ros \\  
  ros-humble-octomap-rviz-plugins \\  
  ros-humble-pcl-ros

**Check Version** 

gz sim \--version  
Gazebo Sim, version 7.9.0  
Copyright (C) 2018 Open Source Robotics Foundation.  
Released under the Apache 2.0 License.

6. Micro XRCE-DDS Agent Installation      [source](https://docs.px4.io/main/en/middleware/uxrce_dds#humble)  
     
   git clone \-b v2.4.2 https://github.com/eProsima/Micro-XRCE-DDS-Agent.git  
   cd Micro-XRCE-DDS-Agent  
   mkdir build  
   cd build  
   cmake ..  
   make  
   sudo make install  
   sudo ldconfig /usr/local/lib/  
     
   **Check Version**   
     
   cd \~/Micro-XRCE-DDS-Agent  
   git describe \--tags  
     
   v2.4.2

   

7. Install PX4 ROS2 Messages 


   mkdir \-p \~/px4\_ros2\_ws/src

   cd \~/px4\ros2\ws/src

   git clone https://github.com/PX4/px4\msgs.git \-b release/1.14
   cd \~/px4\ros2\ws
   source /opt/ros/humble/setup.bash

   colcon build

   

* After building, source before running any code  
    
  source \~/px4\_ros2\_ws/install/setup.bash

* Otherwise 

  echo "source \~/px4\_ros2\_ws/install/setup.bash" \>\> \~/.bashrc  
  source \~/.bashrc


8. If you want to rebuild colcon after some changes in file

cd \~/px4\_ros2\_ws   
colcon build \--symlink-install   
source install/setup.bash

9. Kill all the background command

killall \-9 px4  
killall \-9 ruby  
killall \-9 MicroXRCEAgent  
pkill \-9 \-f "gz sim"  
pkill \-9 \-f "parameter\_bridge"

