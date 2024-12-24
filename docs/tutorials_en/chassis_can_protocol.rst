.. _tutorial_open_can_protocol:

Open CAN Protocol for AGV Chassis Integration
================================================

Due to increasing user demand for customized AGV Chassis integration, we decide to open the CAN protocol utilized by Pursuit autopilot.
The CAN interface provides a reliable communication between the autopilot and the AGV Chassis, and it also permits further extension to other
peripherals.

Protocol Configuration
-------------------------
The Pursuit autopilot supports the CAN 2.0B (extended format) protocol, which can be referred in https://www.ti.com/lit/an/sloa101b/sloa101b.pdf.

.. image:: ../img/can_protocol/extended_can.png
   :height: 100 px
   :width: 750 px
   :scale: 100 %
   :align: center

The configuration for our CAN protocol is:

- Baud rate: 500K
- Time Segment 1 (TSEQ1): 8
- Time Segment 2 (TSEQ2): 3
- Synchronization Jump Width (SJW): 2

Messages for Ackermann Chassis
----------------------------------

#. **Steering Control Command**

    .. image:: ../img/can_protocol/acker_steering_ctrl_msg.png
       :width: 700 px
       :height: 700 px
       :scale: 100 %
       :align: center


#. **Control Feedback Command**

    .. image:: ../img/can_protocol/acker_steering_ctrl_fb_msg.png
       :width: 700 px
       :height: 700 px
       :scale: 100 %
       :align: center

Messages for 4WS-4WD Chassis
---------------------------------

The 4WS-4WD (Four-Wheel Steering and Four-Wheel Drive) chassis is featured with its advanced maneuverability in confined space.

#. **4WS-4WD Control Command**

    .. image:: ../img/can_protocol/4W4D_ctrl_cmd_msg.png
       :width: 700 px
       :height: 700 px
       :scale: 100 %
       :align: center

#. **Steering Control Command**

    .. image:: ../img/can_protocol/4W4D_steering_ctrl_cmd_msg.png
       :width: 700 px
       :height: 700 px
       :scale: 100 %
       :align: center

#. **4W4D Control Feedback Command**

    .. image:: ../img/can_protocol/4W4D_ctrl_fb_msg.png
       :width: 700 px
       :height: 700 px
       :scale: 100 %
       :align: center

#. **Steering Control Feedback Command**

This message is used together with Steering Control Command.

    .. image:: ../img/can_protocol/4W4D_steering_ctrl_fb_msg.png
       :width: 700 px
       :height: 700 px
       :scale: 100 %
       :align: center