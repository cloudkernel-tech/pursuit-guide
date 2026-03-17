.. _tutorial_firmware_upgrade_zh:

自驾仪固件远程升级
=============================

Pursuit 飞控的固件可以定期升级，保证软件的稳定性并获得最新的改进。

升级步骤如下：

- 启动专用的QGroundcontrol 软件 （支持Ubuntu 18.04和Windows 11系统），参考 :ref:`section_quickstart_zh` 部分进行下载和安装。

- 选择 设置 菜单下的 固件 选项卡。 断开自驾仪的直流电源，然后通过Micro USB线将自驾仪的debug口连接到计算机。点击右侧的 高级设置（Advanced settings） ，选择“Pursuit autopilot release （Cloudkernel）”选项。然后可以通过单击右上角的“OK”按钮来触发自动升级过程。

.. image:: ../img/firmware_upgrade/qgc_upgrade.png
   :height: 350 px
   :width: 750 px
   :scale: 100 %
   :align: center
