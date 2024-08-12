Quick Start
=================

The Pursuit autopilot is designed to focus more on simple user interface with a tablet + touch style. Most users
only need to click and slide buttons to conduct their missions. The following guideline presents steps to command a ground vehicle
equipped with our Pursuit autopilot.

Mission Mode (For Users)
------------------------------

#. Installation and Wiring

    - Attach the Pursuit autopilot to the top surface of the vehicle using 3M adhesive.
    - The autopilot needs to be used in conjunction with RTK and LiDAR. Install the RTK at the highest point of the unmanned vehicle using a bracket, and connect the LiDAR to the host computer.
    - Connect the CAN interface, RTK interface, and USB communication interface to the chassis, RTK, and host computer, respectively, according to the requirements.
    - Provide power to the host computer and power the autopilot using the vehicle's 12V DC power supply.


#. Start the Onboard Computer ROS Nodes

    For machines officially configured by us, the onboard computer will automatically run the relevant nodes after startup,
    which is dependent on product options. For models with RTK navigation and automatic obstacle avoidance functions, users can also use a convenient script to execute them.

    .. code-block:: bash

            cd ~/catkinws_nav
            bash run_pursuit_agv_nodes.sh

#. Start the Ground Control Software

    - Download the Ground Station APP, `http://qgroundcontrol.com <https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html>`_
    - Connect the ground station computer to the telemetry communication module via a Micro USB cable.
    - Open the ground station and wait for the automatic connection.

#. Waypoint Mission

    - Enter the waypoint setting page.
    - Add waypoints as shown in the figure.
    - Upload the waypoints, swipe right to unlock and start the mission, and the unmanned vehicle will follow the set waypoints.

    .. image:: img/quick-start/waypoint.jpg
       :height: 500 px
       :width: 750 px
       :scale: 60 %
       :alt: 航路点任务设置
       :align: center

#. Survey Mission

    - Enter the waypoint setting page.
    - Add a pattern, create a mapping pattern, and select the cruise mission area as shown in the figure.
    - Upload the area trajectory points, swipe right to unlock and start the mission, and the unmanned vehicle will follow the generated trajectory.

    .. image:: img/quick-start/survey.jpg
       :height: 450 px
       :width: 750 px
       :scale: 60 %
       :alt: 巡航任务区域设置
       :align: center



