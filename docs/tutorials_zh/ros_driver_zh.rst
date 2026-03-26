.. _tutorial_ros_driver_zh:


使用 ROS 驱动程序访问自驾仪
=================================

我们提供了定制的mavros包， 位于`mavros <https://gitee.com/cloudkernel-tech/mavros>`_  （分支dev_pursuit_agv）， 面向高级开发者设计，并支持物联网通信和机器人模拟等扩展功能。

如何使用mavros包
------------------------

在自驾仪端将参数设置为以下内容来启用与 mavros 包的接口：

::

        SER_TEL2_BAUD = 921600  # the default baud rate for mavros interface


用户需在机载电脑上找到自动驾驶仪的 USB 端口（默认设置名称为/dev/ttyPursuit），然后使用以下命令启动节点：

::

        cd ~/src/catkinws_nav
        source devel/setup.bash

        # launch for default port with 921600 baud rate
        roslaunch mavros px4.launch fcu_url:='/dev/ttyPursuit:921600'


.. image:: ../img/ros_driver/mavros_cmd.png
   :height: 350 px
   :width: 750 px
   :scale: 100 %
   :align: center

mavros节点的局部rqt graph视图是：

.. image:: ../img/ros_driver/rqt_mavros.png
   :height: 350 px
   :width: 750 px
   :scale: 100 %
   :align: center