.. _section_quickstart_zh:

快速启动
==================

Pursuit 自动驾驶仪的设计更侧重于简单的用户界面，采用平板电脑 + 触摸风格。大多数用户只需要点击和滑动按钮来执行他们的任务。以下介绍了运行配置我们Pursuit自驾仪的无人车的步骤。


软硬件工具准备
--------------

硬件工具
^^^^^^^^^^^^^^^^^

- 一台安装windows11或者ubuntu 18.04系统的计算机
- Pursuit自驾仪
- 已经部署配套软件的无人车底盘
- 数传天线地面端

软件工具
^^^^^^^^^^^^^^^^^

(1) Qgroundcontrol地面站软件

Pursuit专用QGC地面站发布在我司官方github页面： `<https://github.com/cloudkernel-tech/qgroundcontrol/releases>`_ ，并会持续更新。

- windows11系统：

`<https://github.com/cloudkernel-tech/qgroundcontrol/releases/download/v0.3/QGroundControl-pursuit-installer.exe>`_

- Ubuntu 18.04系统：

`<https://github.com/cloudkernel-tech/qgroundcontrol/releases/download/v0.3/QGroundControl_pursuit.AppImage>`_

下载后用户需要执行下述指令使软件生效，并重新登录系统：

    ::

        sudo usermod -a -G dialout $USER
        sudo apt-get remove modemmanager -y
        sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y

        # Then logout and login again to enable the change to user permissions.

        chmod +x ./QGroundControl_pursuit.AppImage
        ./QGroundControl_pursuit.AppImage



(2) 数传串口驱动(仅限windows11系统)

百度云下载链接: `<https://pan.baidu.com/s/1391Qkr-uLmmnIo6WG9F6qg>`_ , 提取码: gw84


任务模式（针对用户）
------------------------

* 安装与接线

    - 将Pursuit自动驾驶仪 用3M胶平稳粘好在车顶平面
    - 自驾仪需要配合RTK和激光雷达使用，将RTK通过支架安装在无人车的最高处，激光雷达接到上位机
    - 分别按要求将CAN接口、RTK接口和USB通信接口连接到底盘、RTK和上位机
    - 给上位机供电，并使用车载12V直流电源给自驾仪上电

* 启动机载电脑ROS节点 （仅适用于A+或更高款型）

    对于我们官方配置完成的机器，机载电脑会在启动后自动运行相关节点，具体软件与产品选项相关。对于RTK导航和自动避障功能的机型，用户也可以使用便捷脚本进行执行：

    .. code-block:: bash

            cd ~/catkinws_nav
            bash run_pursuit_agv_nodes.sh


* 启动地面站软件，将数传地面端通过micro usb线连接到地面站电脑， 等待自动连接


* 航路点巡航任务

    - 进入航点设置页面
    - 添加航路点，如图所示
    - 上传航路点，右滑解锁开启任务，无人车即按照设定的航路点运行

    .. image:: img/quick-start/waypoint.jpg
       :height: 500 px
       :width: 750 px
       :scale: 60 %
       :alt: 航路点任务设置
       :align: center


*  区域巡航任务

    - 进入航路点设置页面
    - 添加图案，创建测绘图案，框选巡航任务区域，如图所示
    - 上传区域轨迹点，右滑解锁开启任务，无人车即按照所生成的轨迹运行

    .. image:: img/quick-start/survey.jpg
       :height: 450 px
       :width: 750 px
       :scale: 60 %
       :alt: 巡航任务区域设置
       :align: center



.. hint::

     视频演示可以参考：https://www.bilibili.com/video/BV1Ty411a7Eg