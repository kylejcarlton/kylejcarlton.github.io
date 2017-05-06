---
layout: single
excerpt: "Recording Temperature with Particle Photon."
author_profile: true
permalink: /:categories/:year/:month/:day/:title/
header:
  overlay_image: raspi.jpg
title:  "WiFi Temperature Monitor - Part 1"
tags:
  - WiFi Temp
---

[Particle.io] has some relatively inexpensive and lightweight IoT boards that connect via WiFi ([Photon]) or Cellular ([Electron]) networks. They are focused on providing a fully functioning cloud based IDE for development and production devices. Programming is accomplished via [Wiring], the same framework as [Arduino]. Since the framework is open source down to the bare metal, you can also use C/C++ or ARM assembly.

For my first project with the Photon, I created a wireless temperature monitor that displays in a [Google Sheet]. I used a [TMP36 Sensor] and imported the results utilizing Script Editor and this [Import JSON Script]. The JSON output from the Photon looks like this (“deviceID” intentionally obscured):

{% highlight json %}
{
 "cmd": “VarReturn”,
 "name": “analogvalue”,
 "result": 964,
 "coreInfo": {
   "last_app": “”,
   "last_heard": “2016-05-22T22:00:00.209Z”,
   "connected": true,
   "last_handshake_at": “2016-05-22T21:20:20.002Z”,
   "deviceID": “UNIQUE_ID”,
   "product_id": 6
 }
}
{% endhighlight %}

Then the temperature can be calculated based on the voltage output from the TMP36 using <i>Temp °C = 100*(reading in V) - 50</i>.

<br><br>
![TMP36 Graph]({{ site.url }}/images/TMP36_Graph.png ){: .align-center}     
<br><br>

After importing I also converted to Fahrenheit using <i>T(°F) = T(°C) × 1.8 + 32</i>. Here’s the final output in the Google Sheet using <b>ImportJSON(https://api.particle.io/v1/devices/UNIQUE_ID/analogvalue?access_token=UNIQUE_TOKEN)</b>:    

<br><br>
![TMP36 Graph]({{ site.url }}/images/TMP36_Gsheet.png ){: .align-center}     
<br><br>

[Particle.io]: https://www.particle.io/
[Photon]: https://www.particle.io/products/hardware/photon-wifi-dev-kit
[Electron]: https://store.particle.io/collections/electron
[Wiring]: http://wiring.org.co/
[Arduino]: https://www.arduino.cc/

[Google Sheet]: https://www.google.com/sheets/about/
[TMP36 Sensor]: http://www.analog.com/en/products/analog-to-digital-converters/integrated-special-purpose-converters/digital-temperature-sensors/tmp36.html
[Import JSON Script]: https://github.com/fastfedora/google-docs/blob/master/scripts/ImportJSON/Code.gs
