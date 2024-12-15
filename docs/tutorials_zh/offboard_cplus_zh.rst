.. _tutorial_offboard_cplus_zh:

在 ROS 中使用 C++ 进行在线控制
===========================================

.. Hint::

        本教程仅供 ROS 开发人员使用，其他用户应参考快速入门指南和用户手册。

Pursuit 自动驾驶仪为有兴趣自定义其应用程序的开发人员提供了极大的灵活性。我们持续维护 API 接口 （:ref:`section_api_zh`）使用标准消息和协议，以便具有最低 ROS 经验的用户可以快速上手，并在以后受益于我们的高级功能。

1. 软件组件
-------------------------

推荐环境为 Ubuntu 18.04、ROS melodic 和 python 3.6.9。用户也可以尝试 Docker 方法，如 :ref:`tutorial_docker_env` 所示。软件组件如下：

- mavros （dev_pursuit_agv 分支）: https://gitee.com/cloudkernel-tech/mavros.git
- mavlink（dev_pursuit_agv 分支）: https://gitee.com/cloudkernel-tech/mavlink-gdp-release.git
- offboard_demo （dev_pursuit_agv 分支）: https://github.com/cloudkernel-tech/offboard_demo.git

mavros 包是来自 ROS 社区 （http://wiki.ros.org/mavros） 的定制包，它实现了 :ref:`section_api_zh` 中所示的开发人员的所有 API 接口。mavlink 包
是与自驾仪通信的消息协议。offboard_demo 包是利用 API 进行车辆操纵的 offboard 控制演示。

请使用上述软件包正确的分支版本。


2. 创建和编译工作区
-------------------------------

用户可以执行以下命令来创建用于 offboard 控制演示的工作区：

::

    mkdir -p ~/src/catkinws_nav/src

    # initialize the workspace with catkin tool (https://catkin-tools.readthedocs.io/en/latest/installing.html)
    cd ~/src/catkinws_nav
    catkin init

    # clone all repos
    cd ~/src/catkinws_nav/src

    git clone --recursive https://gitee.com/cloudkernel-tech/mavros.git
    git clone --recursive https://gitee.com/cloudkernel-tech/mavlink-gdp-release.git ./mavlink
    git clone --recursive https://github.com/cloudkernel-tech/offboard_demo.git

    # checkout branches for pursuit
    cd ~/src/catkinws_nav/src/mavros && git checkout dev_pursuit_agv
    cd ~/src/catkinws_nav/src/mavlink && git checkout dev_pursuit_agv
    cd ~/src/catkinws_nav/src/offboard_demo && git checkout dev_pursuit_agv

用户必须为 mavros 包安装 GeographicLib 数据集：

::

    cd ~/src/catkinws_nav/
    ./src/mavros/mavros/scripts/install_geographiclib_datasets.sh


对于中国大陆的用户，此步骤可能会出现网络问题，请参考文章获取建议： https://blog.csdn.net/qq_35598561/article/details/131281485

要编译工作区，只需使用 catkin 构建工具即可：

::

    cd ~/src/catkinws_nav/
    catkin build


3. 代码说明
-----------------------------

offboard_demo包为用户提供了一个模板，用于使用自驾仪的 API 接口。代码示例采用标准的 ROS 节点形式，
用户必须熟悉ROS官方教程编写publisher和subscriber (http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29)。

为了突出显示，offboard_demo 节点包含多个文件：

- offboard_demo/launch/off_mission.launch：用于启动 offboard 控制节点的默认启动文件。
- offboard_demo/launch/waypoints_xyzy.yaml：航路点任务定义文件，其中包含 ENU 坐标和相应的偏航值（在本例中，只有 x、y 坐标有效）。
- offboard_demo/src/off_mission_node.cpp： 用于 offboard 控制的 ROS 节点文件

本例的任务定义为：机体将在位置指令模式下行驶，通过多个预定义的航点，然后进入Twist速度控制模式，持续6秒。
在位置控制模式下，自动驾驶仪接受来自 ROS 节点的位置设定值，同时在Twist速度控制模式模式下直接输出线性和角速度命令。

为了清楚起见，我们按顺序说明了 off_mission_node 的 main（） 函数。


3.1 话题订阅、发布和服务的声明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

我们通常在主程序开始时声明ROS话题订阅、发布和服务。要获取状态信息或发送命令，可以很容易地利用mavros包中的各种话题和服务。

这些API接口在 :ref:`section_api_zh` 中定义，例如 “mavros/vcu_command_velocity/from_nav” 是我们可以发布以将位置设定值发送到自动驾驶仪的话题。

::

    /*subscriptions, publications and services*/
    ros::Subscriber state_sub = nh.subscribe<mavros_msgs::State>
            ("mavros/state", 5, state_cb);

    //subscription for local position
    ros::Subscriber local_pose_sub = nh.subscribe<geometry_msgs::PoseStamped>
            ("mavros/local_position/pose", 5, local_pose_cb);

    ros::Subscriber vcu_base_status_sub = nh.subscribe<pursuit_msgs::VcuBaseStatus>
            ("mavros/vcu_base_status/output", 5, vcu_base_status_cb);

    //publication for local position setpoint
    ros::Publisher local_pos_sp_pub = nh.advertise<geometry_msgs::PoseStamped>
            ("mavros/setpoint_position/local", 5);

    ros::Publisher nav_vel_cmd_pub = nh.advertise<geometry_msgs::Twist>
            ("mavros/vcu_command_velocity/from_nav", 5);

    //service for arm/disarm
    ros::ServiceClient arming_client = nh.serviceClient<mavros_msgs::CommandBool>
            ("mavros/cmd/arming");

    //service for main mode setting
    ros::ServiceClient set_mode_client = nh.serviceClient<mavros_msgs::SetMode>
            ("mavros/set_mode");


3.2 服务指令
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

然后，我们构建必要的命令来调用上面定义的服务，如下所示。自动驾驶仪只能在offboard模式下接受位置和twist速度命令，
而且在进行任何运动之前，应该首先解锁。

::

    /*service commands*/
    mavros_msgs::SetMode offboard_set_mode;
    offboard_set_mode.request.custom_mode = "OFFBOARD";

    mavros_msgs::SetMode stabilized_set_mode;
    stabilized_set_mode.request.custom_mode = "STABILIZED";

    mavros_msgs::CommandBool arm_cmd;
    arm_cmd.request.value = true;

    mavros_msgs::CommandBool disarm_cmd;
    disarm_cmd.request.value = false;

在模拟中，程序将自动设置为offboard模式并解锁。用户需要注意此设置才能进行实际操作。

::

    if (current_wpindex == 0)
    {
        if (simulation_flag == 1)
        {
            //set offboard mode, then arm the vehicle
            if( current_state.mode != "OFFBOARD" &&
                (ros::Time::now() - last_request > ros::Duration(1.0))){

                if( set_mode_client.call(offboard_set_mode) &&
                    offboard_set_mode.response.mode_sent){
                    ROS_INFO("Offboard mode enabled");
                }

                last_request = ros::Time::now();

            } else {
                if( !current_state.armed &&
                    (ros::Time::now() - last_request > ros::Duration(1.0))){
                    if( arming_client.call(arm_cmd) &&
                        arm_cmd.response.success){
                        ROS_INFO("Vehicle armed");
                    }
                    last_request = ros::Time::now();
                }
            }
        }

    }



3.3 位置控制模式
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在位置控制模式下，自动驾驶仪接受ROS节点发布的位置设定值，并请求无人机前往所需位置。航点从yaml文件 （waypoints_xyzy.yaml） 加载，
坐标在 ENU系中定义。请注意，除非自动驾驶仪收到新的位置目标，否则它会继续执行位置控制，因此用户需要切换到自稳模式才能停止车辆，
以避免任何事故。

::

    /* publish local position setpoint for rover maneuvering*/
    case ROVER_MISSION_PHASE::POS_PHASE:{

        ROS_INFO_ONCE("Starting position guidance phase");

        geometry_msgs::PoseStamped pose; //pose to be passed to fcu

        pose = waypoints.at(current_wpindex);

        ROS_INFO_THROTTLE(2.0f, "Rover: go to position %5.3f, %5.3f, %5.3f \n",(double)pose.pose.position.x, (double)pose.pose.position.y,
                            (double)pose.pose.position.z);

        local_pos_sp_pub.publish(pose);

        updateWaypointIndex();

        break;
    }



3.4 Twist速度控制模式
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在Twist速度控制模式下，自动驾驶仪接受线性和角速度命令以进行直接操纵。速度在车辆体坐标系（FLU） 中定义。请注意，
用户必须在此模式下持续发布twist速度命令，否则机体将在停止发布命令2s后怠速。

::

    /* publish twist command for rover maneuvering*/
    case ROVER_MISSION_PHASE::TWIST_CONTROL_PHASE:{

        ROS_INFO_ONCE("Starting twist control phase");

        geometry_msgs::Twist twist_cmd;

        twist_cmd.linear.x = 1.0;
        twist_cmd.linear.y = 0.0;
        twist_cmd.linear.z = 0.0;
        twist_cmd.angular.x = 0.0;
        twist_cmd.angular.y = 0.0;
        twist_cmd.angular.z = 0.5; //about 30deg/s

        nav_vel_cmd_pub.publish(twist_cmd);

        //phase transition after a certain time
        if ( ros::Time::now() - _phase_entry_timestamp > ros::Duration(6.0)){
            _flag_mission_completed = true;
            ROS_INFO("Mission completed");
        }

        break;
    }



4. 操作提示
-----------------------------

要在实际情况下操作车辆，建议几个提示：

- 在启动文件中正确设置模拟标志。请记住将simulation_flag设置为 0 以进行实际操作。

::

    <arg name=“simulation_flag” default = “0”/>

- 请谨慎使用 waypoints_xyzy.yaml 中定义的目标点坐标。

- 测试前请使用模拟环境验证你的代码（:ref:`tutorial_sitl_sim`）。

- 需要一个人待命操作地面站软件，对于意外行为，请切换到 stabilized 模式。



