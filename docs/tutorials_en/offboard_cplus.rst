.. _tutorial_offboard_cplus:

Offboard Control with C++ in ROS
======================================

.. Hint::

    The tutorial is intended for ROS developers only, other users should refer to the quick start guide and user manual instead.

The Pursuit autopilot provides great flexibility for developers interested in customizing their applications. The API interface (:ref:`section_api_en`) is maintained
continuously with standard messages and protocols, so that users with minimum ROS experience can get started quickly and benefit from our advanced features later on.

1. Software Components
-------------------------

The recommended environment is Ubuntu 18.04, ROS melodic and python 3.6.9. Users can also attempt the Docker method as shown in :ref:`tutorial_docker_env`. Software components are listed below:

- mavros (dev_pursuit_agv branch): https://gitee.com/cloudkernel-tech/mavros.git
- mavlink (dev_pursuit_agv branch): https://gitee.com/cloudkernel-tech/mavlink-gdp-release.git
- offboard_demo (dev_pursuit_agv branch):  https://github.com/cloudkernel-tech/offboard_demo.git

The mavros package is a customized package from the ROS community (http://wiki.ros.org/mavros), and it implements all API interfaces for developers shown in :ref:`section_api_en`. The mavlink package
is the message protocol to communicate with the autopilot. The offboard_demo package is the offboard control demo that utilizes API for vehicle maneuvering.

Please remember to checkout branches correctly for above packages.

2. Create and Build Workspace
-------------------------------

Users can conduct the following commands to create a workspace for the offboard control demonstration:

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


Users have to install GeographicLib datasets for the mavros package:

::

    cd ~/src/catkinws_nav/
    ./src/mavros/mavros/scripts/install_geographiclib_datasets.sh

For users from mainland China, network problem can occur during this step, please refer to the article for advice: https://blog.csdn.net/qq_35598561/article/details/131281485


To build the workspace, simply use the catkin build tool:

::

    cd ~/src/catkinws_nav/
    catkin build


3. Code Explanation
-----------------------------

The offboard_demo package provides a template for users to utilize the API interface for Pursuit autopilot. The code example is in a standard ROS node form,
and users have to familiarize with the official ROS tutorial on writing publisher and subscriber (http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29).

To highlight, the offboard_demo node contains several files:

- offboard_demo/launch/off_mission.launch: a default launch file to launch the offboard control node.
- offboard_demo/launch/waypoints_xyzy.yaml: waypoint mission definition file with ENU coordinates and corresponding yaw values (only x, y coordinates are valid in our case).
- offboard_demo/src/off_mission_node.cpp: the ROS node file for offboard control


The mission for this example is defined as: the vehicle will drive in the position command mode to pass through several predefined waypoints, and then enter the twist control mode for 6 seconds.
In position command mode, the autopilot accepts the position setpoint from the ROS node, while it directly outputs the linear and angular velocity commands in the twist control mode.

Here we illustrate the main() function of the off_mission_node in sequence for clarity.


3.1 Declarations of subscriptions, publications and services
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We usually declare ROS subscriptions, publications and services at the beginning of a main program. To obtain the state information or send commands, various topics and services from the mavros package can be easily utilized.

These API interfaces are defined in :ref:`section_api_en`, e.g. "mavros/vcu_command_velocity/from_nav" is the topic we can publish to send position setpoints to the autopilot.

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


3.2 Service command construction
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We then construct necessary commands to call those defined services above as below. The autopilot can only accept position and twist commands in the offboard mode,
and it also should be armed first before any maneuver.

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


In simulation, the vehicle will be automatically set in the offboard mode and armed. Users need to pay attention to this setting for real vehicle operation.

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



3.3 Position Command Mode
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In the position command mode, the autopilot accepts the position setpoint published by the ROS node, and requests the vehicle to go to the desired position. The waypoints are loaded from the yaml file (waypoints_xyzy.yaml),
and the coordinates are defined in ENU frame. Note that the autopilot will keep executing the position command unless it receives new position targets, so users need to switch to the stabilized mode to stop the vehicle
to avoid any accident.

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



3.4 Twist Control Mode
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In twist control mode, the autopilot accepts the linear and angular velocity commands for direct maneuvering. The velocities are defined in the body FLU frame. Note that
users have to keep publishing the twist control commands in this mode, otherwise the vehicle will idle when the command times out in 2 seconds.

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



4. Tips for Deployment
-----------------------------

To operate the vehicle in real case, several tips are suggested:

- Set the simulation flag correctly in the launch file. Remember to set 0 for real operation.

::

    <arg name="simulation_flag"  default = "0"/>


- Be cautious with the waypoint coordinates defined in waypoints_xyzy.yaml.

- Validate your code with the simulation environment first if applicable (:ref:`tutorial_sitl_sim`).

- Keep a person standby for QGC operation. Always switch to the stabilized mode for unexpected behaviors.



