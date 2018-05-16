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
Note: [This alternative installation guide](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04)
 could be of help if you run into any problems.

First you'll need curl for the docker installation. It should already be included as default in Ubuntu 18.04, but if it isn't then installing curl is done as follows:

    sudo apt-get install curl

You will be prompted for your linux account password here.  

Docker is installed using a terminal window. Open one, and then install the docker.io package:

    sudo apt-get install docker.io

More about the installation can be found [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

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
    sudo apt-get install curl docker.io git -y
    
Add docker privileges to the current user:

    sudo usermod -aG docker $USER
   
Logout/login 
