.. _tutorial_rtk_onboard_zh:

使用部署在机载电脑的RTK设备
===========================

针对客户有时将RTK GPS直接安装到机载电脑端的情况，我们也支持ROS端下发对应的RTK信号给自驾仪实现高精度定位。

全方RTK案例
--------------
以全方RTK为例，参考其ROS源码：https://gitee.com/qonfon/qfrtk_t3，启动ROS驱动后，它会发布三个定位相关话题，消息类型分别是：gpgga， navOdometry，navSatFix。
启动时我们需要将它的默认launch文件的enu_pose参数修改为ENU坐标输出：

::

    <param name = "enu_use" value = "1"/>  //1为ENU; 2为NED; 0为ECEF坐标

我们需要将它的三个对应话题remap到以下几个mavros话题：

::

    /mavros/gps_status/rtk/gpgga_output
    /mavros/gps_status/rtk/navsatfix_output
    /mavros/gps_status/rtk/odom_output

在自驾仪端我们设置GPS_RTK_ONB_EN参数为1，然后我们正常操作流程启动docker中的mavros节点即可。


