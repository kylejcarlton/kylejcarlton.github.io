---
layout: single
excerpt: "Soldering the parts together."
author_profile: true
permalink: /:categories/:year/:month/:day/:title/
header:
  overlay_image: raspi.jpg
title:  "Universal Remote - Part 3"
tags:
  - Universal Remote
---

The only items I didn’t purchase online were the supplies for soldering and wiring. I went with the [ECG 20 Watt Miniature Corded Soldering Iron], [Sn60/Pb40 Solder] and [22AWG Solid Copper Wire].
<br><br>
![Soldering Supplies]({{ site.url }}/images/Soldering_Supplies.png){: .align-center}
<br><br>
With all the components and tools needed, the first step was assembling the Adafruit Prototyping Pi Plate Kit following [these instructions]. Once complete I built [the circuit] for the IR receiver and transmitter. With the assembly complete the Prototyping Pi Plate connects to the Raspberry Pi via the GPIO pins. In addition to referencing the circuit diagram the [high resolution pictures from alexba.in] were very useful.
<br><br>
![Completed Remote Build]({{ site.url }}/images/Completed_Remote_Build.png){: .align-center}
<br><br>
With the hardware assembled the next step is to install [Raspbian] as the OS on the SD card, install the application [LIRC] for controlling the hardware and then test functionality.


[ECG 20 Watt Miniature Corded Soldering Iron]: http://www.frys.com/product/7008601
[Sn60/Pb40 Solder]: http://www.frys.com/product/5842273
[22AWG Solid Copper Wire]: http://www.frys.com/product/7716148/
[these instructions]: https://learn.adafruit.com/adafruit-prototyping-pi-plate/solder-it
[the circuit]: https://upverter.com/alexbain/f24516375cfae8b9/Open-Source-Universal-Remote/#/
[high resolution pictures from alexba.in]: http://alexba.in/blog/2013/06/08/open-source-universal-remote-parts-and-pictures/
[Raspbian]: https://www.raspberrypi.org/downloads/raspbian/
[LIRC]: http://www.lirc.org/
