[TOC]
Installing the Authority server on Ubuntu 18.04 LTS (Long Term Support)
======================================================================

Install Ubuntu Server edition
-----------------------------
[Download Ubuntu Server 18.04 LTS](https://www.ubuntu.com/download/server)  
Note: if you are on a 32-bit system you need the 32-bit installer.

Make a bootable USB by using a tool like Etcher or Rufus.

Boot from the USB-stick you made, and [install Ubuntu](https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-server)  
Note: Remember to include a graphical interface if you are not Linux savvy (ubuntu-desktop or GNOME).

Install Docker
--------------
Follow the excellent Docker install-guide [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/).
The guide involves removing any old versions of docker, adding the docker-ce repository and then installing it.

After the installation is complete you need to grant your Ubuntu-user
permission to actually use the docker program. This is done by executing the
following commands:

    sudo usermod -aG docker $USER

Note: After running the commands above you need to log out of Ubuntu and log back in for the change to
take effect. Verify that the command was successful by opening the terminal and running

    docker run hello-world

This should then generate the «hello-world» image (note that sudo is not used).

TL:DR; (For experienced users)
-------
Install Ubuntu Server. Update and install required packages:

    sudo apt-get update -y && sudo apt-get upgrade -y
    sudo apt-get install curl git apt-transport-https ca-certificates curl software-properties-common -y
    
Add the docker-ce repository and install:

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt-get install docker-ce
    
Add docker privileges to the current user:

    sudo usermod -aG docker $USER
   
Logout/login 
