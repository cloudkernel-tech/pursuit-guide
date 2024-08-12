快速启动
==================

Pursuit 自动驾驶仪的设计更侧重于简单的用户界面，采用平板电脑 + 触摸风格。大多数用户只需要点击和滑动按钮来执行他们的任务。以下介绍了运行配置我们Pursuit自驾仪的无人车的步骤。

任务模式（针对用户）
------------------------

#. 安装与接线

    - 将Pursuit自动驾驶仪 用3M胶平稳粘好在车顶平面
    - 自驾仪需要配合RTK和激光雷达使用，将RTK通过支架安装在无人车的最高处，激光雷达接到上位机
    - 分别按要求将CAN接口、RTK接口和USB通信接口连接到底盘、RTK和上位机
    - 给上位机供电，并使用车载12V直流电源给自驾仪上电


#. 启动机载电脑ROS节点

    对于我们官方配置完成的机器，机载电脑会在启动后自动运行相关节点，具体软件与产品选项相关。对于RTK导航和自动避障功能的机型，用户也可以使用便捷脚本进行执行：

    .. code-block:: bash

            cd ~/catkinws_nav
            bash run_pursuit_agv_nodes.sh


#. 启动地面站软件

    * 下载地面站APP，链接：`http://qgroundcontrol.com <https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html>`_
    * 地面站电脑通过Micro USB线连接数传通信模块。
    * 打开地面站，等待自动连接


#. 航路点巡航任务

    - 进入航点设置页面
    - 添加航路点，如图所示
    - 上传航路点，右滑解锁开启任务，无人车即按照设定的航路点运行

    .. image:: img/quick-start/waypoint.jpg
       :height: 500 px
       :width: 750 px
       :scale: 60 %
       :alt: 航路点任务设置
       :align: center


#. 区域巡航任务

    - 进入航路点设置页面
    - 添加图案，创建测绘图案，框选巡航任务区域，如图所示
    - 上传区域轨迹点，右滑解锁开启任务，无人车即按照所生成的轨迹运行

    .. image:: img/quick-start/survey.jpg
       :height: 450 px
       :width: 750 px
       :scale: 60 %
       :alt: 巡航任务区域设置
       :align: center