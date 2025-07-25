.. _tutorial_mission_avoidance:

Obstacle Avoidance in the Mission Mode
==============================================

Autonomous obstacle avoidance in the mission mode means that the user sets waypoints on QGC, and the vehicle can autonomously
avoid obstacles when it encounters obstacles during patrol. In this process, if there are no obstacles, the autopilot will control
the chassis of the unmanned vehicle alone; after encountering obstacles, the autopilot will follow the algorithm instructions
from the onboard computer for avoidance purpose. Our software packages currently support lidar-based autonomous obstacle avoidance.

Autopilot Configuration
----------------------------

The following parameters need to be set for the autopilot via the ground control software:

::

    ROS_LINK_EN: 0   # enable mavros channel in autopilot instead of pursuit_driver
    GND_RMC_OBS_EN: 1  # enable obstacle avoidance functionality

When the GND_RMC_OBS_EN parameter is set to 1, the onboard computer ROS must publish the obstacle avoidance status topic
\~/pursuit_outdoor_avoidance/avoidance_status, otherwise the autopilot will refuse to unlock and run operations, and prompt the ground station
avoidance status not ready.

Software Package
-----------------
We have released the product function package (commercial software not open source) on the server https://mirror.cloudkernel.cn .
Users can install it in the pursuit_agv_container container by downloading it from the server.

- pursuit_nav: This package includes navigation configuration and launch files for different vehicle models.

- pursuit_outdoor_avoidance: This package mainly implements obstacle status detection based on LiDAR and publishes pursuit_outdoor_avoidance/avoidance_status topics.

- navigation: This package implements local path planning, has a customized interface, and publishes local speed control instructions when encountering obstacles.

Launch Process
----------------

We can start the required ROS nodes as follows, after installing the pursuit product feature package in the container,


- Download the latest launch files and scripts:

::

    # In the host computer
    cd ~
    git clone https://gitee.com/cloudkernel-tech/pursuit_agv_release


- Copy the script and launch file to the \~/src/catkinws_nav directory

::

    # In the host computer
    cd ~/pursuit_agv_release
    # copy quick start script to workspace for nav avoidance
    cp Tools/run_pursuit_agv_nodes.sh ~/src/catkinws_nav/

    # copy launch files
    cp -r launch ~/src/catkinws_nav/


- Set the corresponding chassis name in the \~/src/catkinws_nav/launch file, and the corresponding parameter name is robot_name.

- Set the startup script to be executable. By default, mavros, pursuit_outdoor_avoidance and navigation related nodes will be started.

::

    # In the host computer
    cd ~/src/catkinws_nav/
    sudo chmod a+rw run_pursuit_agv_nodes.sh

    # run the script
    bash run_pursuit_agv_nodes.sh

.. hint::

    The screen operation is used in the run_pursuit_agv_nodes.sh script, and the script in the container is started
    accordingly. Users can adjust the content where applicable.

- Start the script automatically on boot (optional)

The script can be set to start on boot automatically through the crontab tool, and pay attention to the USER_NAME setting.

::

    sudo crontab -e

    # Add the following text with correct USER NAME for the host computer
    @reboot sleep 30s && /home/<USER_NAME>/src/catkinws_nav/run_pursuit_agv_nodes.sh

