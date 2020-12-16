.. _Development:


=========================
Development Setup
=========================
Here we discuss the software used, from the most bare bone ardupilot parts to the last piece in the puzzle QGC. They are listed in an order of increasing risk and complexity. As a beginner you can probably start from QGC, then move to doing things on companion computer. Almost 99% of special case applications can be achieved doing this. You should look towards changing the autopilot code itself as a last option.
All forks and code used for our education platforms are located on our github `page <https://github.com/makeraxis/>`__

==========================================
QGroundContorl development Environment
=========================================
You can setup the QGC development environment on any desktop platform MAC/Windows/Linux. Linux will probably be the easiest to setup, followed by MAC and then windows. For the sake of simplicity, we will keep our tutorial to Linux only. 

Install latest Ubuntu
=======================
It is generally recommended to install Ubuntu native. But if you are a little afraid to do that, it is ok to do it on VM as well. We prefer virtual box but any vm will do. When using Ubuntu inside a VM you might have to struggle with USB and network port forwarding issues. Nothing unsolvable, but these things do irritate.


Install Qt
=============
For installation instructions on your development platform of choice, it is best to use upto date instructions from `here <http://doc.qt.io/qt-5/gettingstarted.html>`__ 
If you plan to build QGC app for android also, setup android environment also as explained in the above link.

The official download server might be slow. You can try one of the `mirrors <https://download.qt.io/static/mirrorlist/>`__ US and Japanese mirrors are quite good. 

Downlaod QGC
=============
For compiling QGC follow the instructions in QGC developer guide `here <https://dev.qgroundcontrol.com/en/>`__. As you will be using our platform, it will be better to clone from our QGC `repo <https://github.com/makeraxis/qgroundcontrol>__

.. code-block:: console 
    
    git clone https://github.com/makeraxis/qgroundcontrol


Hoverbird's additions to QGC.
=============================
The playground code that we obtained from our partners at `Sense Robotics <http://senserobotics.com>`__ adds a few clever things to QGC for quick hacking. The user side aspect of these mods are spread across the usage instructions on our wiki. In this section we will only discuss it from a developer point of view, the code that went into QGC to make that possible.

.. using youtube https://github.com/sphinx-contrib/youtube
.. #youtube:: 8kx9glzO7os



Software on Companion Computer (Raspberry Pi)
==============================================
Raspberry Pi runs a few daemons that make it possible for QGC to properly communicate with the platform. Some of these daemons also provide additional functionality like camera control etc.

These binaries are located at /home/pi/ardupilot/bins/. The corresponding code will be located one step up in their individually cloned git repositories. 

The development environment is fully setup. You can build any of these binaries yourself and use.


picontrolserver service
************************
This is a systemctl service that starts the Qt based http server. Basically it launches the picontrolserver binary using wrapper script `picontrolserver.sh <https://github.com/makeraxis/pi_bins/blob/master/picontrolserver.sh>`__ which sets up its runtime environment.

The serves takes incoming http requests send by QGC and acts on them. It handles camera options, wifi options and it is responsible for setting up MAVlink between vehicle and QGC. Code can be found `here <https://github.com/makeraxis/picontrolserver>`__


ardupilot service
********************
This service is required only when you want to run ardupilot SITL or using a Raspberry Pi cape like Raspilot. If you are using it a standalone autopilot like Pixhawk. This server doesn't run anything. The service is handled by shell script `start_ardupilot.sh <https://github.com/makeraxis/pi_bins/blob/master/start_ardupilot.sh>`__


MAVProxy
*********
We use MAVProxy to dynamically setup a mavlink UDP line to the QGC that requests it. Basically once you have wifi connected, QGC discovers the Vehicle on local LAN using beacons sent out by picontrolserver. Once discovered it asks picontrolserver to start sending mavlink packets to it. Picontrolsever removes any existing instance of MAVProxy and laucnhes a new one with IP of QGC as target. 

picontrolserver invokes `start_mavproxy.sh <https://github.com/makeraxis/pi_bins/blob/master/start_mavproxy.sh>`__ with the target IP address to achieve this.



Switching between Pixhawk and SITL
***********************************
Pi has a full clone of ardupilot as well along with development environment. This helps in two ways. You can run SITL to test your newly developed QGC features. If you go as far as writing new features for ardupilot firmware itself, you can test them through SITL before actually using it on pixhawk.

the directory /home/pi/ardupilot/bins/ is actaully a clone of https://github.com/makeraxis/pi_bins and contains different branches for SITL and pixhawk. Stop picontrolserver and arudpilot services using 

.. code-block:: console

    systemctl stop picontrolserver.service
    systemctl stop ardupilot.service

Then if you want to work with SITL, switch to pi3_sitl_onboardwifi branch using 

.. code-block:: console

    git fetch origin # this will update your repo
    git checkout -t origin/pi3_sitl_onboardwifi

if you want to work with Pixhawk, switch to pi3_pixhawk_onbaordwifi branch using 

.. code-block:: console

    git checkout -t origin/pi3_pixhawk_onbaordwifi

.. youtube:: n3PlTJ_fV_M


Software on Autopilot(pixhawk)
===========================================

We run Ardupilot on Pixhawk. The Ardupilot wiki `here <http://ardupilot.org/dev/docs/building-the-code.html>`__ has excellent instructions on how to setup a build environment for ardupilot itself. As a beginner you will never need to do this. Changing the autopilot code is more specialized work and you should not do it unless you have a good reason to do so. If you can manage what you want on the companion computer, mavpoxy or QGC, it is always better as these are relatively low risk components when compared to autopilot code.

Innovative Ideas for Development
=================================

You can do a lot of things with Ardupilot and Raspberry Pi. Ardupilot project maintains a list of good list of such ideas especially for students `here <http://ardupilot.org/dev/docs/gsoc-ideas-list.html>`__
