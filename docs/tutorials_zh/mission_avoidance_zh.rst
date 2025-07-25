.. _tutorial_mission_avoidance_zh:

Mission模式下的自主避障实现
===========================

Mission模式下的自主避障指的是用户在QGC上制定航路点，无人车在巡线中遇到障碍物可以自主躲避的功能。这个过程中，没有遇到障碍物的情况下，自驾仪会独自控制无人车底盘；遇到障碍物后，自驾仪会听从机载电脑的算法控制指令实现绕障过程。目前Pursuit自驾仪提供的软件包可以实现基于激光雷达的自主避障。

自驾仪参数设置
--------------

以下参数需要通过地面站软件对自驾仪进行设置：

::

    ROS_LINK_EN: 0   # enable mavros channel in autopilot instead of pursuit_driver
    GND_RMC_OBS_EN: 1  # enable obstacle avoidance functionality

当GND_RMC_OBS_EN参数设置为1后，机载电脑ROS端必须发布避障状态话题 \~/pursuit_outdoor_avoidance/avoidance_status，否则自驾仪端会拒绝解锁和运行操作，并在地面站提示
avoidance status not ready.

产品功能软件包
-----------------
我们在服务器 https://mirror.cloudkernel.cn 上发布了产品的功能包（属商业软件未开源），用户可以在pursuit_agv_container容器中通过服务器下载方式进行安装。


- pursuit_nav包： 该功能包包括了针对不同车型的导航配置和launch文件。

- pursuit_outdoor_avoidance包： 该功能包主要实现基于激光雷达的障碍物状态探测，发布pursuit_outdoor_avoidance/avoidance_status话题。

- navigation包： 该功能包实现局部路径规划，有定制接口，在遇到障碍物时发布局部速度控制指令。

启动过程
----------------

在完成在容器中安装pursuit产品功能软件包之后，我们可以按以下步骤启动所需要的ROS节点，


- 下载最新的启动文件和脚本

::

    # In the host computer
    cd ~
    git clone https://gitee.com/cloudkernel-tech/pursuit_agv_release


- 将脚本和launch文件复制到\~/src/catkinws_nav目录下

::

    # In the host computer
    cd ~/pursuit_agv_release
    # copy quick start script to workspace for nav avoidance
    cp Tools/run_pursuit_agv_nodes.sh ~/src/catkinws_nav/

    # copy launch files
    cp -r launch ~/src/catkinws_nav/


- 在\~/src/catkinws_nav/launch文件中设置对应的底盘名称，对应参数名称为robot_name。


- 修改执行文件权限后可以执行，默认会启动mavros, pursuit_outdoor_avoidance和navigation相关节点。

::

    # In the host computer
    cd ~/src/catkinws_nav/
    sudo chmod a+rw run_pursuit_agv_nodes.sh

    # run the script
    bash run_pursuit_agv_nodes.sh

.. hint::

    run_pursuit_agv_nodes.sh脚本中使用了screen操作，并对应启动了容器中的脚本，用户可以根据实际情况调整其中的内容。

- 设置开机自动启动（可选）

用户可以通过crontab 工具将上述脚本加入到自动启动项，注意用户名的设置。

::

    sudo crontab -e

    # Add the following text with correct USER NAME for the host computer
    @reboot sleep 30s && /home/<USER_NAME>/src/catkinws_nav/run_pursuit_agv_nodes.sh

