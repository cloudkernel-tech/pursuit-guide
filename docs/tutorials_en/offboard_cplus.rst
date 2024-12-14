.. _tutorial_offboard_cplus:

Offboard Control with C++ in ROS
======================================

.. Hint::

    The tutorial is intended for ROS developers only, other users should refer to the quick start guide and user manual instead.

The Pursuit autopilot provides great flexibility for developers interested in customizing their applications. The API interface (:ref:`section_api_en`) is maintained
continuously with standard messages and protocols, so that users with minimum ROS experience can get started quickly and benefit from our advanced features later on.

1. Software Components
-------------------------

The recommended environment is Ubuntu 18.04, ROS melodic and python 3.6.9. Users can also attempt the Docker method as shown in :ref:`tutorial_docker_env`. Software components are listed below:

- mavros (dev_pursuit_agv branch): https://gitee.com/cloudkernel-tech/mavros.git
- mavlink (dev_pursuit_agv branch): https://gitee.com/cloudkernel-tech/mavlink-gdp-release.git

The mavros package is a customized package from the ROS community (http://wiki.ros.org/mavros), and it implements all API interfaces for developers shown in :ref:`section_api_en`. The mavlink package
is the message protocol to communicate with the autopilot. Please remember to checkout correct branches for above packages.


2. Create and Build Workspace
-------------------------------




3. Code Explanation
-----------------------------



4. Tips for Deployment
-----------------------------







