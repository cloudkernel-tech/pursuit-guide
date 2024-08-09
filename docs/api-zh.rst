应用开发接口（API）
==============================

我们为Pursuit自驾仪提供开源API开发支持，候选API接口将支持C++、Python等语言。详细信息将持续更新在此部分。请注意，我们假定用户已经熟悉ROS的基本概念，未满足此前提的用户建议先通过官方教程：http://wiki.ros.org/ROS/Tutorials。

.. caution::

    我们官方支持的开发环境仅为Ubuntu 18.04和ROS Melodic。用户可以在其他主机环境中尝试使用Docker方法。


ROS API (C++)
----------------------

Pursuit自动驾驶仪的ROS API在ROS包中实现，我们维护了以下几个包：

- **pursuit_driver** (main branch): 通过串行通信与自动驾驶仪接口的ROS包，仓库地址：`<https://gitee.com/cloudkernel-tech/pursuit_driver>`_

- **pursuit_msgs** (main branch): 定制的Pursuit消息，仓库地址： `<https://gitee.com/cloudkernel-tech/pursuit_msgs>`_

- **mavros** (dev_pursuit_agv branch)：为Pursuit自动驾驶仪定制的mavros包，仓库地址： `<https://gitee.com/cloudkernel-tech/mavros>`_


1. pursuit_driver node
^^^^^^^^^^^^^^^^^^^^^^^

(1) Subscribed topics
""""""""""""""""""""""



**以下话题仅用于产品发布，开发者不建议使用**

- /pursuit_outdoor_avoidance/trajectory/generated (`mavros_msgs::Trajectory <http://docs.ros.org/en/melodic/api/mavros_msgs/html/msg/Trajectory.html>`_):

   导航模块生成的轨迹，将被发送到Pursuit自动驾驶仪。

- /pursuit_outdoor_avoidance/avoidance_status (`pursuit_msgs::AvoidanceStatus <https://gitee.com/cloudkernel-tech/pursuit_msgs/blob/main/msg/AvoidanceStatus.msg>`_):

    使用机载激光扫描仪进行障碍物检测的避障状态

- /pursuit_nav/smooth_cmd_vel (`geometry_msgs::Twist <https://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/Twist.html>`_):

    导航模块生成的平滑速度命令，将被发送到Pursuit自动驾驶仪。


(2) Published topics
""""""""""""""""""""""""

- ~pursuit_driver/ackermann_drive_cmd ( `ackermann_msgs::AckermannDriveStamped <https://docs.ros.org/en/melodic/api/ackermann_msgs/html/msg/AckermannDriveStamped.html>`_ ):

    由自动驾驶仪发送给VCU AGV底盘的驾驶命令，适用于阿克曼车辆。

- ~pursuit_driver/vehicle_gps_position ( `mavros_msgs::GPSRAW <https://docs.ros.org/en/melodic/api/mavros_msgs/html/msg/GPSRAW.html>`_):

    车辆的GPS位置，这是机载RTK设备的输出。

- ~pursuit_driver/vehicle_local_pose ( `geometry_msgs::PoseStamped <https://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/PoseStamped.html>`_):

    车辆在定位准备完成且GPS锁定后，以东-北-上(ENU)坐标系中表达的本地位置和姿态。

- ~pursuit_driver/vehicle_local_velocity (`geometry_msgs::TwistStamped <https://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/TwistStamped.html>`_):

    在ENU坐标系中表达的车辆本地速度。

- ~pursuit_driver/trajectory/desired (`mavros_msgs::Trajectory <http://docs.ros.org/en/melodic/api/mavros_msgs/html/msg/Trajectory.html>`_):

    由自动驾驶仪请求的期望航点设定。

- ~pursuit_driver/vehicle_ctrl_state (`pursuit_msgs::VehicleCtrlState <https://gitee.com/cloudkernel-tech/pursuit_msgs/blob/main/msg/VehicleCtrlState.msg>`_):

    自动驾驶仪定义的控制状态，包括解锁状态、导航状态等。

- ~pursuit_driver/vcu_base_status (`pursuit_msgs::VcuBaseStatus <https://gitee.com/cloudkernel-tech/pursuit_msgs/blob/main/msg/VcuBaseStatus.msg>`_):

    地面车辆中VCU单元的状态。

- ~pursuit_driver/vcu_cmd_vel (`geometry_msgs::Twist <https://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/Twist.html>`_):

    Pursuit自动驾驶仪发送给VCU单元的速度命令。

- ~pursuit_driver/vehicle_angular_velocity (`std_msgs::Float32MultiArray <https://docs.ros.org/en/melodic/api/std_msgs/html/msg/Float32MultiArray.html>`_):

    在ENU坐标系中表达的车辆姿态角速率。


2. pursuit_msgs package
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

定制消息可以在 `<https://gitee.com/cloudkernel-tech/pursuit_msgs/tree/main/msg>`_ 查阅，它们的含义在定义中很容易理解。



