.. _tutorial_firmware_upgrade:

Upgrade Autopilot Firmware Remotely
=======================================================

The firmware of the Pursuit autopilot can be upgraded to ensure the software stability and obtain the latest improvements.
The upgrade process is easy to conduct with our customized QGroundcontrol software released here
(`<https://github.com/cloudkernel-tech/qgroundcontrol/releases/download/v0.2.1/QGC_3.5.6_kerloud_202411.AppImage>`_).

The steps for upgrade are as follows:

- Download the QGroundcontrol software in a Ubuntu 18.04 host computer.

- Run the following command in a terminal to setup the environment properly.

::

        sudo usermod -a -G dialout $USER
        sudo apt-get remove modemmanager -y
        sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y

        # Then logout and login again to enable the change to user permissions.

        chmod +x ./QGC_3.5.6_kerloud_202411.AppImage

- Start the QGroundcontrol software

::

        # start the app with command
        ./QGC_3.5.6_kerloud_202411.AppImage

- Select the Firmware tab under the Setting menu, and then connect to the Pursuit autopilot via the Micro USB cable. Click the Advanced settings at the right side, and select the option "Pursuit autopilot release (Cloudkernel)". Then the automatic upgrade process can be triggered by clicking the "OK" button in the upper right corner.

.. image:: ../img/firmware_upgrade/qgc_upgrade.png
   :height: 350 px
   :width: 750 px
   :scale: 100 %
   :align: center
