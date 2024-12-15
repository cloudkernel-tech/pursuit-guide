.. _tutorial_docker_env_zh:

在机载电脑部署docker容器
=========================

**注意：本教程仅适用于Model A+ 或更高款型的自驾仪**


我们提供自定义的 docker 镜像，以促进使用 Pursuit autopilot 进行软件开发。docker 方法的好处包括两个折叠：它减轻了为产品发布设置繁琐环境的需要，并且软件稳定性也可以
保证，因为 Docker 容器与主机中的操作系统隔离。

我们仅维护 x86 和 arm 架构的 Docker 映像，目前它们随我们的产品一起交付给我们的客户。我们将当软件组件达到成熟状态时对外发布。

关于docker
--------------

Docker 是一个开源容器化平台，允许开发人员将其应用程序及其依赖项打包到一个可移植的容器中
然后将其发布以在任何支持 Docker 的计算机上运行，而无需担心环境变量和依赖项。容器是轻量级的
独立于平台的软件包，其中包含可在任何环境中运行的应用程序及其依赖项。Docker 使用容器技术
实现快速部署、可移植性、效率以及可扩展的应用程序打包和交付。

- 官方 docker 文档：`<http://docs.docker.com/>`_

- Docker 备忘：`<https://docs.docker.com/get-started/docker_cheatsheet.pdf>`_

设置步骤
----------------

可以使用 docker load 命令将 docker 镜像导入到主机。我们提供了方便的脚本供用户设置主机环境。/dev/ttyPursuit代表Pursuit自驾仪和机载电脑的USB连接端口。

::

        # make the directory for navigation modules
        cd ~ && mkdir src/catkin_nav

        # Clone release files
        cd ~
        git clone https://gitee.com/cloudkernel-tech/pursuit_agv_release

        # Perform installation in the host computer
        cd ~/pursuit_agv_release
        bash Tools/install_prerequisites_host.sh

        # Load the docker image we provide
        docker load < cloudkernel_dasa_rosnav.tar.gz

        # First run, if nvidia runtime error happens, please refer to:
        # https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html

        # Create pursuit_agv_container container from our docker image, and make sure the Pursuit autopilot is connected to the host computer
        docker run -it --device /dev/ttyPursuit -v /home/$USER/src/catkinws_nav:/home/src/catkinws_nav --network host --name pursuit_agv_container dasa_rosnav:latest

        # exit the container
        exit

后续如要启动 docker 容器，以下命令可以在容器内启动 bash 终端。

::

        # Enter the container
        docker start pursuit_agv_container
        docker exec -it pursuit_agv_container bash

        # Stop the container
        docker stop pursuit_agv_container