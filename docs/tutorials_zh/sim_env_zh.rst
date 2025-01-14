.. _tutorial_sitl_sim_zh:

用于软件验证的仿真环境
=========================

我们使用 Pursuit 自动驾驶仪为各种场景设计仿真环境，以便用户可以在模拟环境中直接验证他们的软件，无需现场测试。该环境基于 Gazebo 引擎。

.. Hint::

        软件包通过定制U盘发售，需额外付费，请咨询我们的销售人员如何购买。

1. 工作区目录
-----------------
模拟环境的工作区位于提供的 U 盘中的 ~/pursuit_space/sitl_space_pursuit 中。用户可以按照用户手册轻松地从 U 盘启动他们的计算机。

.. image:: ../img/sim/snap_pursuit_base.png
   :height: 450 px
   :width: 750 px
   :scale: 60 %
   :align: center

.. raw:: html

    <center> (a) Simulation for a ground vehicle in empty environment
    </center>
   <br><br>


.. figure:: ../img/sim/snap_pursuit_base_lidar.png
   :height: 450 px
   :width: 750 px
   :scale: 60 %
   :align: center

.. raw:: html

    <center> (b) Simulation for a ground vehicle in clustered environment
    </center>
   <br><br>



2. 如何使用
------------------

我们支持以下几种模拟案例。

Case 1: 使用 GPS 导航的空旷环境
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

这是最简单的模拟情况，可以通过以下方式启动：

::

        bash sitl_run.sh $PWD/bin/px4 none gazebo pursuit_base

然后用户可以在 QGroundcontrol 软件中操作地面车辆。

Case 2: 带 GPS 导航的 Offboard 控制
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

对于开发 offboard 控制应用程序的用户，模拟也可以启动所有必要的节点，包括 mavros，如下所示。请注意，
catkinws_nav工作区必须位于同一目录（~/pursuit_space）中，以便 setup_pursuit.bash 脚本可以获取正确的 mavros 包。

::

        source setup_pursuit.bash
        roslaunch launch/mavros_posix_sitl_pursuit_base.launch


Case 3: 使用 2D 激光雷达在室外集群环境中进行 Offboard 控制
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

我们还支持对配备 2D 激光雷达的地面车辆进行仿真，这是我们 Model A+ 的主要特点。下面的启动文件可以激活 2D 激光雷达及其对应的 ROS 主题。
用户必须在 QGroundcontrol 中手动将 GND_RMC_OBS_EN 设置为 0 才能启用离线控制。

::

        source setup_pursuit.bash
        roslaunch launch/mavros_posix_sitl_pursuit_base_rplidar.launch







