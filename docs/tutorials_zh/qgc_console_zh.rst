.. _tutorial_qgc_console_zh:

地面站的控制台界面
============================

我们在 QGroundcontrol 软件的控制台界面中提供了方便的命令。这些命令可以帮助用户显示系统状态并定位硬件和软件层中的故障。

.. image:: ../img/qgc_console/qgc_console.png
   :height: 350 px
   :width: 750 px
   :scale: 100 %
   :align: center


传感器状态
--------------

Pursuit 自动驾驶仪由多个传感器组成，用于全面融合传感器，包括加速度计、陀螺仪、气压计和磁力计。
传感器数据对系统运行至关重要，但这些传感器并非防错，因为它们可能会因长期振动、过热和潮湿而退化。
因此，当系统未按预期工作时，始终建议用户先检查传感器数据。

要在自动驾驶仪中查看传感器数据，只需在 Mavlink 控制台中输入命令即可：

::

        listener sensor_accel  # display accelerometer data
        listener sensor_gyro   # display gyroscope data
        listener sensor_baro   # display barometer data
        listener sensor_mag    # display magnetometer data

当相应命令没有输出时，传感器有问题。

无人车底盘状态
------------------

AGV 底盘的 VCU 单元通过 CAN 总线连接到自动驾驶仪。

要查看 VCU 状态，只需尝试：

::

        vcu_bridge status           # display the driver status for VCU communication

        listener vcu_base_status    # display the vcu base status
        listener vcu_control_cmd    # display the command sent to the vcu base

用户也可以通过指令使底盘进行简单运动测试：

        vcu_bridge test


RTK定位状态
------------------

RTK gps 通过串口连接到自动驾驶仪，可以通过以下方式查看 gps 数据：


::

        gps status                      # display the gps driver status
        listener vehicle_gps_position   #display gps data published to the sensor fusion module


无人车系统状态
-------------

整车状态（姿态、位置和速度）可以通过以下方式查看：

::

        listener vehicle_attitude           # display vehicle attitude from the sensor fusion module
        listener vehicle_local_position     # display vehicle position and velocity from the sensor fusion module

.. image:: ../img/qgc_console/vehicle_position.png
   :height: 450 px
   :width: 650 px
   :scale: 75 %
   :align: center

.. caution::

        字段 xy_valid、z_valid 和 v_xy_valid 表示定位状态，只有当所有这些字段都为 true 时，才能操作车辆。在 RTK 室外定位的情况下，eph 和 epv 的典型值应小于 1.0，以保证定位精度。


上层算法状态
-----------------

对于具有避障功能的 RTK 导航方案，下面的命令可以显示障碍物状态、激光数据和规划器输出有效状态。

::

        listener avoidance_status

下面列出了avoidance_status主题的定义：

::

        bool  flag_obstacle_in_far_front   # Obstacle is in far front of the vehicle
        bool  flag_obstacle_far_nearby  # Obstacle is in the neighbourhood of the vehicle with a large distance
        bool  flag_obstacle_in_front       # Obstacle is at the front of the vehicle
        bool  flag_obstacle_in_rear        # Obstacle is at the back of the vehicle
        bool  flag_obstacle_nearby         # Obstacle is within a distance from the vehicle
        bool  flag_nav_task_active         # True when navigation task is active onboard
        bool  flag_nav_local_plan_valid    # The local plan from navigation is valid for obstacle avoidance
        bool  flag_laser_scan_data_valid   # Laser scan data is valid for obstacle detection

.. caution::

        当激光雷达数据在机载计算机中有效时，字段 flag_laser_scan_data_valid 为 true。





