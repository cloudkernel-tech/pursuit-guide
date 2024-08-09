Application Programming Interface (API)
==========================================

We provide open source API for development with the Kerloud UAV series, and candidate API interfaces will support C++, python and other languages.
Details will be updated continuously in this part. Note that we assume users are familiar with basic concepts about ROS, and those who don't meet the
prerequisite are recommended to go through official tutorials in http://wiki.ros.org/ROS/Tutorials.

.. caution::

    📌 The officially supported development environment is Ubuntu 18.04 with ROS Melodic only. Users can attempt the Docker method in other host environment.

ROS API (C++)
----------------------

The ROS API for Pursuit autopilot is implemented in ROS packages, and we maintain several packages as follows:

- **pursuit_driver** (main branch): the ROS package to interface with the autopilot via serial communication, and the repository is released in `<https://gitee.com/cloudkernel-tech/pursuit_driver>`_

- **pursuit_msgs** (main branch): customized pursuit messages, and the repository is released in `<https://gitee.com/cloudkernel-tech/pursuit_msgs>`_

- **mavros** (dev_pursuit_agv branch) : the customized mavros package for pursuit autopilot, and the repository is released in https://github.com/cloudkernel-tech/mavros


1. pursuit_driver node
^^^^^^^^^^^^^^^^^^^^^^^

(1) Subscribed topics
""""""""""""""""""""""

**The following topics are used in product release only, and shall not be used by developers**

- /pursuit_outdoor_avoidance/trajectory/generated (`mavros_msgs::Trajectory <http://docs.ros.org/en/melodic/api/mavros_msgs/html/msg/Trajectory.html>`_):

    The generated trajectory from the navigation module to be sent to the Pursuit autopilot.

- /pursuit_outdoor_avoidance/avoidance_status (`pursuit_msgs::AvoidanceStatus <https://gitee.com/cloudkernel-tech/pursuit_msgs/blob/main/msg/AvoidanceStatus.msg>`_):

    The avoidance status for obstacle detection with the onboard laser scanner.

- /pursuit_nav/smooth_cmd_vel (`geometry_msgs::Twist <https://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/Twist.html>`_):

    The smooth command velocity from the navigation module to be sent to the Pursuit autopilot.



(2) Published topics
""""""""""""""""""""""""

- ~pursuit_driver/ackermann_drive_cmd ( `ackermann_msgs::AckermannDriveStamped <https://docs.ros.org/en/melodic/api/ackermann_msgs/html/msg/AckermannDriveStamped.html>`_ ):

        The drive command sent to the VCU AGV chassis by the autopilot, applicable for Ackermann vehicles only.

- ~pursuit_driver/vehicle_gps_position ( `mavros_msgs::GPSRAW <https://docs.ros.org/en/melodic/api/mavros_msgs/html/msg/GPSRAW.html>`_):

        The GPS position of the vehicle, which is the output from the onboard RTK device.

- ~pursuit_driver/vehicle_local_pose ( `geometry_msgs::PoseStamped <https://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/PoseStamped.html>`_):

        The local position and attitude of the vehicle expressed in ENU (East-North_Up) frame when the vehicle localization is ready after GPS lock.

- ~pursuit_driver/vehicle_local_velocity (`geometry_msgs::TwistStamped <https://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/TwistStamped.html>`_):

        The local velocity of the vehicle expressed in ENU frame.

- ~pursuit_driver/trajectory/desired (`mavros_msgs::Trajectory <http://docs.ros.org/en/melodic/api/mavros_msgs/html/msg/Trajectory.html>`_):

        The desired waypoint setpoint requested by the autopilot.

- ~pursuit_driver/vehicle_ctrl_state (`pursuit_msgs::VehicleCtrlState <https://gitee.com/cloudkernel-tech/pursuit_msgs/blob/main/msg/VehicleCtrlState.msg>`_):

        The control state defined by the autopilot, including arming state, navigation state, etc.

- ~pursuit_driver/vcu_base_status (`pursuit_msgs::VcuBaseStatus <https://gitee.com/cloudkernel-tech/pursuit_msgs/blob/main/msg/VcuBaseStatus.msg>`_):

        The state of VCU base in the ground vehicle.

- ~pursuit_driver/vcu_cmd_vel (`geometry_msgs::Twist <https://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/Twist.html>`_):

        The velocity command sent to the VCU base by the Pursuit autopilot.

- ~pursuit_driver/vehicle_angular_velocity (`std_msgs::Float32MultiArray <https://docs.ros.org/en/melodic/api/std_msgs/html/msg/Float32MultiArray.html>`_):

        The attitude rate of the vehicle expressed in the ENU frame.



2. pursuit_msgs package
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The customized messages can be referred in `<https://gitee.com/cloudkernel-tech/pursuit_msgs/tree/main/msg>`_, and they are self-explanatory in corresponding definitions.


