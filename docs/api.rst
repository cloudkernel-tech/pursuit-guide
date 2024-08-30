.. _section_api_en:

Application Programming Interface (API)
==========================================

We provide open source API for development with Pursuit autopilot, and candidate API interfaces will support C++, python and other languages.
Details will be updated continuously in this part. Note that we assume users are familiar with basic concepts about ROS, and those who don't meet the
prerequisite are recommended to go through official tutorials in `<http://wiki.ros.org/ROS/Tutorials>`_.

.. caution::

    📌 The officially supported development environment is Ubuntu 18.04 with ROS Melodic only. Users can attempt the Docker method in other host environment.

ROS API (C++)
----------------------

The ROS API for Pursuit autopilot is implemented in ROS packages, and we maintain several packages as follows:

- **pursuit_driver** (main branch): the ROS package to interface with the autopilot via serial communication, and the repository is released in `<https://gitee.com/cloudkernel-tech/pursuit_driver>`_

- **pursuit_msgs** (main branch): customized pursuit messages, and the repository is released in `<https://gitee.com/cloudkernel-tech/pursuit_msgs>`_

- **mavros** (dev_pursuit_agv branch) : the customized mavros package for pursuit autopilot, and it's an alternative method to interface with the autopilot (recommended for advanced developers only). The repository is released in `<https://gitee.com/cloudkernel-tech/mavros>`_.


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

The customized messages can be referred in `<https://gitee.com/cloudkernel-tech/pursuit_msgs/tree/main/msg>`_, and they are self-explanatory in corresponding definitions. For instance,
the content of VcuBaseStatus message is shown below, which contains the VCU status of the AGV chassis such as steering angle, forward speed, heading rate, etc. The valid fields are
dependent on the vehicle type.

::

        # Base type definitions
        uint8 VCU_BASE_TYPE_UNDEFINED = 0
        uint8 VCU_BASE_TYPE_ACKERMANN = 1
        uint8 VCU_BASE_TYPE_DDRIVE_4WHEELS = 2

        # Gear position definitions
        uint8 VCU_GEAR_POSITION_UNDEFINED = 0   # undefined
        uint8 VCU_GEAR_POSITION_P = 1   # pause/stop
        uint8 VCU_GEAR_POSITION_R = 2   # recede
        uint8 VCU_GEAR_POSITION_N = 3   # null
        uint8 VCU_GEAR_POSITION_D = 4   # forward

        # Operating mode definitions
        uint8 VCU_OPERATING_MODE_AUTO = 0
        uint8 VCU_OPERATING_MODE_REMOTE = 1
        uint8 VCU_OPERATING_MODE_STOP = 2

        std_msgs/Header 	header

        uint8       vcu_base_type         # vcu base type
        uint8       gear_position         # current gear position
        float32     speed                 # current moving speed, unit: m/s, positive value only
        bool        steering_angle_valid    # valid flag for steering angle
        float32     steering_angle          # steering angle in radians
        bool        twist_valid
        float32[3]  vel                     # velocity along body axes (m/s)
        bool        heading_rate_valid
        float32     heading_rate            # heading rate (rad/s)

        uint8       operating_mode          # current operating mode


3. mavros package
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The mavros package originates from the PX4 community (`<http://wiki.ros.org/mavros>`_). We add customized messages for the Pursuit autopilot. Most messages can be referred in the official mavros wiki page,
and we only list those that are commonly used in our case.




