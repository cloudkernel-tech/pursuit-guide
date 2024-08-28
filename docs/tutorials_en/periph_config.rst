.. _tutorial_periph_config:

Peripheral Configurations
==============================

Peripheral configurations can be easily done via the parameter setting interface in the QGroundcontrol software as depicted below.
By default, products we deliver are configured properly in factory for peripherals selected by our customers. The documentation
here provides a reference for adaptation on the user side.

.. image:: ../img/periph_config/param_interface.png
   :height: 350 px
   :width: 750 px
   :scale: 100 %
   :align: center

AGV Chassis
-----------------

**YUHESEN Robot**
^^^^^^^^^^^^^^^^^^^

- Model MK ROBOT-01

        .. image:: ../img/periph_config/mkrobot-1.png
           :height: 450 px
           :width: 750 px
           :scale: 50 %
           :align: center

        Parameters to enable the MK robot-01 are listed as:

        ::

                VCU_BASE_TYPE = 1  # enable the ackermann vehicle type
                VCU_ACK_MID = 1   # model id for MK robot -01


- Model FW-MAX

        .. image:: ../img/periph_config/fwmax.png
           :height: 450 px
           :width: 750 px
           :scale: 50 %
           :align: center

        Parameters to enable the FW-max are listed as:

        ::

                VCU_BASE_TYPE = 2  # enable the 4 wheels differential drive vehicle type
                VCU_DD4_MID = 1    # model id for FW-max



RTK Devices
-----------------

**QF RTK**
^^^^^^^^^^^^

- Model R3C

        .. image:: ../img/periph_config/qf_r3c.png
           :height: 550 px
           :width: 700 px
           :scale: 40 %
           :align: center

        Parameters to enable the R3C RTK are listed as:

        ::

                SER_GPS1_BAUD = 115200  # baud rate for RTK serial communication
                GPS_RTK_ID = 1          # model id



**Comnav Technology**
^^^^^^^^^^^^^^^^^^^^^^^^^^

- Model M100

        .. image:: ../img/periph_config/M100.png
           :height: 550 px
           :width: 700 px
           :scale: 40 %
           :align: center

        Parameters to enable the M100 Comnav RTK with the comnav binary protocol are listed as:

        ::

                SER_GPS1_BAUD = 115200  # baud rate for RTK serial communication
                GPS_RTK_ID = 2          # model id