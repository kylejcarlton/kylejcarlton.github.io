---
layout: single
excerpt: "Recording Remotes and Control from a Website."
author_profile: true
permalink: /:categories/:year/:month/:day/:title/
header:
  overlay_image: raspi.jpg
title:  "Universal Remote - Part 5"
tags:
  - Universal Remote
---
<br><br>
![Universal Remote HW]({{ site.url}}/images/Remote_Web_Interface.png)
<br><br>
In the last post I tested the IR receiver; next step verify the IR LEDs emit light. If you are lucky enough to have a remote in the database maintained @ [lirc.sourceforge.net/remotes/], you may not even need to record. None of these worked for my remotes, so I created custom config files for each remote. However, these are great for testing functionality. I found it easier to connect to the Raspberry Pi via FTP with [FileZilla] to move config files around than via SSH with [PuTTy]. This did require adjusting permissions using [chmod] on <i>/etc/lirc/lircd.conf</i> via SSH, to allow the Write operation.

1. Back up the original <i>/etc/lirc/lircd.conf</i> and then replace it with a working config from the [LIRC Database].

2. Restart LIRC to pick up these changes:

    > sudo /etc/init.d/lirc stop
    > sudo /etc/init.d/lirc start

3. Use [irsend] to list the commands available for the specific remote (name line in .conf file), for example samsung:

    > irsend LIST samsung “”

4. Use [irsend] to initiate an IR signal:

    > irsend SEND_ONCE samsung KEY_POWER

    The easiest way to verify the IR LEDs are working is to point them at a camera that detects IR and check if the LEDs emit light when initiating the SEND_ONCE command. Some cameras may have a filter to remove light in this spectrum. Older devices are less likely to have these filters; IR was visible on the built in webcam of my [HP Envy laptop].

    Now that the hardware and LIRC are working, the next step is control from a website. [Lirc_web] relies on [Node.js] and luckily it’s easy to install on the Raspberry Pi thanks to [node-arm.herokuapp.com]. For most of the following steps I referenced [alexba.in/blog/2013/02/23/controlling-lirc-from-the-web].

5. Install and verify Node.js:

    > wget http://node-arm.herokuapp.com/node_latest_armhf.deb
    > sudo dpkg -i node_latest_armhf.deb
    > node -v

6. Install and start lirc_web:

    > wget https://github.com/alexbain/lirc_web/archive/master.zip
    > unzip master.zip
    > mv lirc_web-master lirc_web
    > rm master.zip
    > cd lirc_web
    > npm install
    > node app.js

7. Create an initialization script in <i>/etc/init.d</i> and register it using <i>update-rc.d</i>, this way lirc_web starts automatically when the Raspberry Pi boots. For more details see [stuffaboutcode.com/2012/06/raspberry-pi-run-program-at-start-up] and [wiki.debian.org/LSBInitScripts].

    > sudo nano /etc/init.d/URemote

    > #! /bin/sh    
    > # /etc/init.d/lirc_web     
    > ### BEGIN INIT INFO     
    > # Provides:          lirc_web    
    > # Required-Start:    $remote_fs $syslog    
    > # Required-Stop:     $remote_fs $syslog    
    > # Default-Start:     2 3 4 5    
    > # Default-Stop:      0 1 6    
    > # Short-Description: Simple script to start a program at boot    
    > # Description:       A simple script from www.stuffaboutcode.com which will start / stop a program a boot / shutdown.    
    > ### END INIT INFO    
    > # If you want a command to always run, put it here    
    > # Carry out specific functions when asked to by the system    
    > <b>case "$1" in    
    > &nbsp;&nbsp;start)    
    > &nbsp;&nbsp;&nbsp;echo "Starting lirc_web"    
    > &nbsp;&nbsp;&nbsp;# run application you want to start    
    > &nbsp;&nbsp;&nbsp;node /home/pi/lirc_web/app.js    
    > &nbsp;&nbsp;&nbsp;;;    
    > &nbsp;&nbsp;stop)    
    > &nbsp;&nbsp;&nbsp;echo “Stopping lirc_web”    
    > &nbsp;&nbsp;&nbsp;# kill application you want to stop    
    > &nbsp;&nbsp;&nbsp;killall /home/pi/lirc_web/app.js    
    > &nbsp;&nbsp;&nbsp; ;;    
    > &nbsp;&nbsp;*)    
    > &nbsp;&nbsp;&nbsp;echo “Usage: node /home/pi/lirc_web/app.js {start|stop}”    
    > &nbsp;&nbsp;&nbsp;exit 1    
    > &nbsp;&nbsp;&nbsp;;;    
    > esac    
    > exit 0 </b>    

    Make the script executable:

    > sudo chmod 755 /etc/init.d/URemote

    Test starting lirc_web with the script:

    > sudo /etc/init.d/URemote start

    Test stopping lirc_web with the script:

    > sudo /etc/init.d/URemote stop

    Register the script to be run at start-up:

    > sudo update-rc.d URemote defaults

    If your remotes aren’t in the [available index] or those config files don’t work, [irrecord] can be used.

8. Stop lirc to free up /dev/lirc0:

    > sudo /etc/init.d/lirc stop

9.  Create a new remote control configuration file using <i>/dev/lirc0</i> and save the output to <i>~/lircd.conf</i>. Once a file is generated for each remote it can be compiled into a single lircd.conf, this is much easier with FTP access. A list of namespace values for buttons is available @[ocinside.de/modding_en/linux_ir_irrecord_list].

    > irrecord -d /dev/lirc0 ~/lircd.conf

10. After all the remotes are recorded and compiled, the lircd.conf file can be placed @ <i>/etc/lirc/</i> and then restart LIRC to pick up the new configuration:

    > sudo /etc/init.d/lirc stop
    > sudo /etc/init.d/lirc start

To finish of the hardware build I made a case out of Lego, so it stays oriented vertically and has some protection.

<br><br>
![Universal Remote HW]({{ site.url}}/images/Remote_with_Lego_Case.png)
<br><br>

Now control of IR devices is possible from a website, next up controlling Bluetooth devices the same way using [GIMX].



[lirc.sourceforge.net/remotes/]: http://lirc.sourceforge.net/remotes/
[FileZilla]: https://filezilla-project.org/
[PuTTy]: http://www.putty.org/
[chmod]: https://en.wikipedia.org/wiki/Chmod

[LIRC Database]: http://lirc.sourceforge.net/remotes/

[irsend]: http://www.lirc.org/html/irsend.html

[HP Envy laptop]: https://www.cnet.com/products/hp-envy-14-beats-edition/review/

[Lirc_web]: https://github.com/alexbain/lirc_web
[Node.js]: https://nodejs.org  
[node-arm.herokuapp.com]: http://node-arm.herokuapp.com/

[alexba.in/blog/2013/02/23/controlling-lirc-from-the-web]: http://alexba.in/blog/2013/02/23/controlling-lirc-from-the-web/

[stuffaboutcode.com/2012/06/raspberry-pi-run-program-at-start-up]: http://www.stuffaboutcode.com/2012/06/raspberry-pi-run-program-at-start-up.html
[wiki.debian.org/LSBInitScripts]: https://wiki.debian.org/LSBInitScripts

[available index]: http://lirc.sourceforge.net/remotes/
[irrecord]: http://www.lirc.org/html/irrecord.html

[ocinside.de/modding_en/linux_ir_irrecord_list]: http://www.ocinside.de/modding_en/linux_ir_irrecord_list/

[GIMX]: http://gimx.fr/wiki/index.php?title=Command_line  
