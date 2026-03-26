.. _tutorial_ros_driver:

Access the Autopilot with ROS Drivers
=======================================

We provide a customized ROS package mavros (`mavros <https://gitee.com/cloudkernel-tech/mavros>`_, branch: dev_pursuit_agv) to interface with the Pursuit autopilot as mentioned in the API section.
The package is devised for advanced developers to support extensible capabilities in the future such as IOT communication and robot
simulation.

How to use mavros package
-----------------------------

The interface with the mavros package is enabled by setting parameters as:

::

        SER_TEL2_BAUD = 921600  # the default baud rate for mavros interface, which is visible only after setting ROS_LINK_EN=0

Users have to locate the USB port (the default name is /dev/ttyPursuit) for the autopilot in the companion computer, and then start the node with:

::

        cd ~/src/catkinws_nav
        source devel/setup.bash

        roslaunch mavros px4.launch fcu_url:='/dev/ttyPursuit:921600'


.. image:: ../img/ros_driver/mavros_cmd.png
   :height: 350 px
   :width: 750 px
   :scale: 100 %
   :align: center

The partial view of rqt graph for mavros node is:

.. image:: ../img/ros_driver/rqt_mavros.png
   :height: 350 px
   :width: 750 px
   :scale: 100 %
   :align: center

