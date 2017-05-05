---
layout: single
excerpt: "Installing LIRC and Wireless Communication."
author_profile: true
permalink: /:categories/:year/:month/:day/:title/
header:
  overlay_image: raspi.jpg
title:  "Universal Remote - Part 4"
tags:
  - Universal Remote
---

With the hardware built the next step is getting the Raspberry Pi up and running and installing [LIRC]. [Alexba.in] has a comprehensive post for both of these things: [RaspberryPi Quickstart] and [Setting Up LIRC on the RaspberryPi]. I discovered and modified a few things along the way, so here’s what I did.

- Download the latest [Raspbian Image] and follow their [Installation Guide].
- Network the Pi over wired Ethernet using RJ45 connector.
- Connect to the Pi over SSH with [PuTTy].

Since I’d like to connect over WiFi I’ve added a [Belkin USB F7D2101]. For future development, I also added a [ORICO BTA-402 USB Bluetooth 4.0 Micro Adapter Dongle] for controlling a Play Station 3 using [GIMX].

<br><br>
![Universal Remote HW]({{ site.url}}/images/remote_build_wireless_com.png)
<br><br>

- With the Pi temporarily connected by Ethernet cable, I [set up the wireless connection via the command line] over SSH.    
- The following commands update the Software and Firmware then sync the Time with a source on the Internet:
{% highlight script%}
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo apt-get install git-core
sudo wget http://goo.gl/1BOfJ -O /usr/bin/rpi-update && sudo chmod +x /usr/bin/rpi-update
sudo rpi-update

sudo apt-get install ntpdate
sudo ntpdate -u ntp.ubuntu.com
{% endhighlight %}
- Next install [LIRC]:
{% highlight script%}
sudo apt-get install lirc
{% endhighlight %}
- Modify <i>/etc/modules</i> and <i>/etc/lirc/hardware.conf</i> for the specific hardware being used:
<b>/etc/modules</b>
{% highlight script%}
lirc_dev    
lirc_rpi gpio_in_pin=23 gpio_out_pin=22
{% endhighlight %}
<b>/etc/lirc/hardware.conf</b>
{% highlight script%}
########################################################    
# /etc/lirc/hardware.conf    
#    
# Arguments which will be used when launching lircd    
LIRCD_ARGS="--uinput"    
# Don't start lircmd even if there seems to be a good config file    
# START_LIRCMD=false    
# Don't start irexec, even if a good config file seems to exist.    
# START_IREXEC=false    
# Try to load appropriate kernel modules LOAD_MODULES=true    
# Run "lircd --driver=help" for a list of supported drivers.    
DRIVER="default"    
# usually /dev/lirc0 is the correct setting for systems using udev    
DEVICE="/dev/lirc0"    
MODULES="lirc_rpi"
# Default configuration files for your hardware if any    
LIRCD_CONF="" LIRCMD_CONF=""    
########################################################    
{% endhighlight %}
- Restart LIRC to pick up these changes:    
{% highlight script%}
sudo /etc/init.d/lirc stop
sudo /etc/init.d/lirc start
{% endhighlight %}
- Stop LIRC and start in raw data mode to verify that IR receiver is working:
{% highlight script%}
sudo /etc/init.d/lirc stop
mode2 -d /dev/lirc0
{% endhighlight %}
- Pressing buttons on an IR remote pointed at the receiver and activity similar to the following should be displayed:
{% highlight script%}
space 16300    
pulse 95    
space 28794    
pulse 80    
space 19395    
pulse 83    
{% endhighlight %}
In the next post on this topic I’ll cover recording the IR signal from remotes using the [irrecord] command and testing functionality from the command line. I’ll also install [Node.js] and the client library [lirc_node] for controlling LIRC from a web site.     


[LIRC]: http://www.lirc.org/
[Alexba.in]: http://alexba.in/
[RaspberryPi Quickstart]: http://alexba.in/blog/2013/01/04/raspberrypi-quickstart/
[Setting Up LIRC on the RaspberryPi]: http://alexba.in/blog/2013/01/06/setting-up-lirc-on-the-raspberrypi/

[Raspbian Image]: https://www.raspberrypi.org/downloads/raspbian/
[Installation Guide]: https://www.raspberrypi.org/documentation/installation/installing-images/README.md
[PuTTy]: http://www.putty.org/

[Belkin USB F7D2101]: http://www.belkin.com/us/support-product?pid=01t80000002G16OAAS&clickid=w1SRafxTOUl3RfRz3MQE83ZCUkkXNDQxxw-FSE0&utm_campaign=Belkin+Store+Home+Page&utm_medium=affiliate&utm_source=impactradius&irgwc=1
[ORICO BTA-402 USB Bluetooth 4.0 Micro Adapter Dongle]: https://www.amazon.com/ORICO-BTA-402-Bluetooth-Adapter-Controller/dp/B00AKO7XOW/ref=cm_cr_arp_d_product_top?ie=UTF8
[GIMX]: https://gimx.fr/wiki/index.php?title=Main_Page

[set up the wireless connection via the command line]: https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md

[irrecord]: http://www.lirc.org/html/irrecord.html
[Node.js]: https://nodejs.org
[lirc_node]: https://github.com/alexbain/lirc_node
