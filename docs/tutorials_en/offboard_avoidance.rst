.. _tutorial_offboard_avoidance:

Avoidance in the Offboard Mode
=================================

The autonomous obstacle avoidance in offboard mode refers to the ability of the ground vehicle to avoid obstacles when the user
issues target points in offboard mode. The initiation process is exactly the same as described in the :ref:`tutorial_mission_avoidance` section,
and the subsequent steps are:

- Enter Offboard Mode: Use the ~/mavros/set_mode service to switch to offboard mode

- Arm: Use ~/mavros/cmd/arming to arm

- Send the position target: Use the ~/mavros/setpoint_position/local, or ~/mavros/setpoint_position/global topic to publish the position target. Later, the built-in navigation module will publish the topics ~/mavros/vcu_command_velocity/from_nav for obstacle avoidance when necessary.

.. hint::

    If the user needs to send twist control commands directly to the autopilot, please use the ~/mavros/vcu_command_velocity/ctrl topic, and do not use the built-in ~/mavros/vcu_command_velocity/from_nav.