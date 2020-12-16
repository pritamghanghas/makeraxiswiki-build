.. _troubleshooting:

======================
Troubleshooting FAQs
======================

Things don't always work as they are supposed to. Don't loose heart, we are here to help, please follow instructions below to troubleshoot the problem. Take a methodological approach and you will find the cause.

I can't connect to the vehicle
================================
This is most common problem due to the fact that we have several systems and problem can be at lot of places. If you are not able to connect, here the things that you should check in the same order

   * We use Raspberry Pi as hostspot for connecting to vehicles. Check that you are able to connect to wifi hotspot
      * if you are not able to connect then check that raspberry pi is connected to power. If so the problem is either with your card or hostapd configuration if you have modified it. 
      * in case you image on the Raspberry Pi card has become totally corrupted, please reflash with Hoverbird's provided image for the product. Flashing instructions can be found `here <https://www.raspberrypi.org/documentation/installation/installing-images/>`__.
   * Now if you are able to get on to the wifi network, next part is the connection between the autopilot and Raspberry Pi
      * the physical wire connection might be damaged
      * MavProxy is not running on the Pi due to some misconfiguration, you can manually run to check whether you are able to connect. 
      * the serial port on the Pi has not been freed up using raspi-config please refer `this page <http://ardupilot.org/dev/docs/raspberry-pi-via-mavlink.html>`__
   * The ground station system doesn't have correct version of QGC. You might be using version from play store, our version is different, you have download either from our website or from the image provided and side load it.
   * Autopilot/Pixhawk has wrong card or not flashed. Format the card on windows and flash copter version 3.5.x, 3.5.x is verified to be working with RW460 and Nexus image provided with the product.
  

The copter doesn't lift off
============================
* The sensors are mostly likely not calibrated. Go through vehicle setup. Mostly QGC will let you know if sensors are out of whack. 
* The copter is under powered or your battery has become too weak and doesn't hold charge well enough anymore. Please check how the voltage varies as your apply throttle. Sudden large drops in voltage as you increase throttle is a sign of your battery needs replacement.


The copter flips
==================
The most common cause might be that you have connected propeller wrong. It is impossible to do if you are using our setup of motor/esc and propellers as they are matched and self tightening.


Raspberry Pi erratic behavior
=============================
It reboots randomly, wifi goes off randomly. This almost always means problem with the power supply. Shutdown the system immediately and check the power coming to raspberry Pi. It should be 5V within 0.2v variance.
Frequent reboots are generally associated with power supply not holding up when Pi draws more current under load. While wifi erratic behavior and overheating is generally caused by over voltage which can even fry you Pi.

More information
=================
More troubleshooting in general about ardupilot can be found `here <http://ardupilot.org/copter/docs/troubleshooting.html>`__. If you still have problem about Nexus or our product you can always ask it in `makeraxis forums <http://forums.makeraxis.in>`__. For questions in general about ardupilot itself can be asked on `ardupilot forums <https://discuss.ardupilot.org/>`__
