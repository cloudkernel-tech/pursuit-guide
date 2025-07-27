.. _tutorial_minimal_guide:

The Minimal User Guide
===========================

#. Mount the autopilot to the AGV base: refer to Section 3.3 in the user manual.

#. Peripheral wire connection（RTK, CAN, ROS ports): refer to Section 2.2 in the user manual.

#. Measure the mounting location of the autopilot, and configure the autopilot remotely： we will provide a form with necessary parameters, and assist customers to configure the autopilot remotely.

#. Upgrade the autopilot firmware to the latest version： refer to :ref:`tutorial_firmware_upgrade` 。

#. Peripheral test:

    .. caution::

        Note that the distance between the ground telemetry and the autopilot must not be less than 3 meters, otherwise it may cause the communication module to overheat and burn out.

    - AGV Chassis test： refer to Section AGV Chassis Layer in the tutorial (:ref:`tutorial_qgc_console`).

    - RTK test： refer to Section RTK Layer Status in the tutorial (:ref:`tutorial_qgc_console`). If the RTK unit is installed with the onboard computer, then refer to :ref:`tutorial_rtk_onboard` alternatively.


#. Onboard Software Deployment: refer to Chapter 5.2 of user manual for operations related to the docker images and containers on the onboard computer; at the same time, we need to synchronize the cloud software settings to meet requirements such as obstacle avoidance.

#. Launch software： start the ROS nodes on the onboard computer according to Section 5.4 of the user manual.

#. Field tests： we will aid our customer remotely for field tests from open environments, static obstacle condition to slow-moving obstacle condition.
