.. _tutorial_docker_env:

Deploy a Docker Container in the Companion Computer
=======================================================

We provide customized docker images to facilitate software development with the Pursuit autopilot. The benefits with the docker method
include two folds: it alleviates the need to setup the tedious environment for product release, and software stability can also be
guaranteed as the docker container is isolated from the operating system in the host computer.

We maintain docker images for x86 and arm architectures only, and currently they are delivered to our customers with our products. We'll
release them to public when the software components reach the mature status.

About Docker
---------------------

**Note: This tutorial is applicable only for Model A+ or above**

Docker is an open-source containerization platform that allows developers to package their applications and their dependencies into a portable container
and then publish it to run on any Docker-enabled machine without worrying about environment variables and dependencies. A container is a lightweight,
platform-independent package that contains an application and its dependencies that can run in any environment. Docker uses container technology to
achieve rapid deployment, portability, efficiency, and scalable application packaging and delivery.

- The official docker documentation: `<http://docs.docker.com/>`_

- docker cheatsheet: `<https://docs.docker.com/get-started/docker_cheatsheet.pdf>`_


Setup Process
---------------------

The docker image can be imported to the host computer with the docker load command. We provide convenience scripts for users to setup the
host environment. The /dev/ttyPursuit denotes the USB port connected with the Pursuit autopilot.

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


To start the docker container afterwards, the following commands can bring up the bash terminal within the container.

::

        # Enter the container
        docker start pursuit_agv_container
        docker exec -it pursuit_agv_container bash

        # Stop the container
        docker stop pursuit_agv_container

