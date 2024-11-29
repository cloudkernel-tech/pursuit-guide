.. _tutorial_firmware_upgrade_zh:

自驾仪固件远程升级
=============================


Pursuit 飞控的固件可以升级，保证软件的稳定性并获得最新的改进。 使用我们在此处发布的定制 QGroundcontrol 软件 (`<https://github.com/cloudkernel-tech/qgroundcontrol/releases/download/v0.2.1/QGC_3.5.6_kerloud_202411.AppImage>`_)，可以轻松进行升级


升级步骤如下：

- 在 Ubuntu 18.04 主机上下载 QGroundcontrol 软件。

- 在终端中运行以下命令以正确设置环境，完成后需重启计算机使设置生效。

::

        sudo usermod -a -G dialout $USER
        sudo apt-get remove modemmanager -y
        sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y

        chmod +x ./QGC_3.5.6_kerloud_202411.AppImage

- 启动 QGroundcontrol 软件

::

        # start the app with command
        ./QGC_3.5.6_kerloud_202411.AppImage

- 选择 设置 菜单下的 固件 选项卡。 断开自驾仪的直流电源，然后通过Micro USB线连接到计算机。点击右侧的 高级设置（Advanced settings） ，选择“Pursuit autopilot release （Cloudkernel）”选项。然后可以通过单击右上角的“OK”按钮来触发自动升级过程。

.. image:: ../img/firmware_upgrade/qgc_upgrade.png
   :height: 350 px
   :width: 750 px
   :scale: 100 %
   :align: center
