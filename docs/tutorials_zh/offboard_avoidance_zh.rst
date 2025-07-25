.. _tutorial_offboard_avoidance_zh:

在线控制模式下的自主避障实现
=================================

Offboard模式下的自主避障指的是用户在offboard模式下下发目标点时，无人车仍可以实现避障的功能。它的启动过程与 :ref:`tutorial_mission_avoidance_zh` 章节完全一致，之后的使用步骤是：

- 进入offboard模式： 使用\~/mavros/set\_mode服务切换到offboard模式

- 解锁： 使用\~/mavros/cmd/arming进行解锁

- 发送目标航路点： 使用\~/mavros/setpoint_position/local， 或者\~/mavros/setpoint_position/global两个话题发布目标航路点位置。之后在遇到障碍物时内置导航模块会发布\~/mavros/vcu_command_velocity/from_nav话题进行局部避障导引。

.. hint::

    如果用户需要直接向自驾仪发送twist控制指令，请使用\~/mavros/vcu_command_velocity/ctrl话题，而不要使用内置的\~/mavros/vcu_command_velocity/from_nav
