.. _tutorial_rtk_onboard:

Access the RTK installed with the onboard computer
=============================================================

In cases where customers sometimes install RTK GPS directly with the onboard computer,
we also support sending the corresponding RTK signals from the ROS side to the autopilot for high-precision positioning.

QF RTK case
---------------

Taking the QF RTK as an example, we refer to its ROS source code: https://gitee.com/qonfon/qfrtk_t3 .
After starting the ROS driver, it will publish three location-related topics, with message types: gpgga, navOdometry, and navSatFix.
When starting, we need to modify the enu_pose parameter in its default launch file to output ENU coordinates:

::

    <param name = "enu_use" value = "1"/>  //1: ENU frame; 2: NED frame; 0: ECEF frame

We need to remap its three corresponding topics to the following mavros topics:

::

    /mavros/gpsstatus/rtk/gpgga_output
    /mavros/gpsstatus/rtk/navsatfix_output
    /mavros/gpsstatus/rtk/odom_output

On the autopilot side, we set the GPS_RTK_ONB_EN parameter to 1, and then we can start the mavros node in the docker container
following the normal operating procedure.