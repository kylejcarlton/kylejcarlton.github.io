---
layout: single
excerpt: "Real-Time Dashboard of Temperature using Google Apps Script."
author_profile: true
permalink: /:categories/:year/:month/:day/:title/
header:
  overlay_image: raspi.jpg
title:  "WiFi Temperature Monitor - Part 2"
tags:
  - WiFi Temp
---

<br><br>
![TMP36 Graph]({{ site.url }}/images/Google_Apps_Script_Temp.png ){: .align-center}     
<br><br>

Since writing the first WiFi temperature monitor post, I’ve implemented retrieving temperature values on a schedule, to generate a real-time dashboard. I came across this [Gadgets Apps Hacks Post], which utilizes [Google Apps Script’s] ability to connect to [External APIs] and record stock ticker values over time in [Google Sheets]. The method I used in the first part to write the temperature sensor value in a Sheet is more suited for a single import of a larger data set in JSON format. There is also a [tutorial from Particle] that uses [IFTTT] to log the data in a Sheet. Although the tutorial from Particle might be a little easier to implement, I chose to work solely with Google Apps Script; since I wanted to pull data from other APIs. I’ll use [WeatherUnderground] for the outside temperature and [Nest] for a comparison of inside temperature from another device.

For communication with my Nest Thermostat, I didn’t implement the [OAuth2.0] standard completely inside the Apps Script; although this would be possible using [apps-script-oauth2]. Following the [REST Quick Guide], I generated a [PIN for my Nest] and then used [Postman] to initiate the POST call for the Access Token to be used in the script.

Here are the results after a few days:

<iframe width="697" height="431" seamless frameborder="0" scrolling="no" style="display: block; margin: 0 auto" src="https://docs.google.com/spreadsheets/d/1ir8ENcChkleHsPGUWlmbGlXQQTnxPHI-o29nMX9jvO8/pubchart?oid=280457042&amp;format=interactive"></iframe>

The [Google Sheet] is here (create a copy to view Script Editor and make changes) and I also posted the code as a [Gist here]. API keys, device ID’s etc. are all variables to be defined at the beginning of the Script.  

[Gadgets Apps Hacks Post]: http://www.gadgetsappshacks.com/2014/01/how-to-record-daily-portfolio-values-in.html
[Google Apps Script’s]: https://developers.google.com/apps-script/
[External APIs]: https://developers.google.com/apps-script/guides/services/external
[Google Sheets]: https://www.google.com/sheets/about/
[tutorial from Particle]: https://docs.particle.io/tutorials/projects/maker-kit/#tutorial-4-temperature-logger
[IFTTT]: https://ifttt.com/
[WeatherUnderground]: https://www.wunderground.com/weather/api/d/docs
[Nest]: https://developers.nest.com/documentation/cloud/get-started

[OAuth2.0]: https://oauth.net/2/
[apps-script-oauth2]: https://github.com/googlesamples/apps-script-oauth2
[REST Quick Guide]: https://developers.nest.com/documentation/cloud/how-to-auth
[PIN for my Nest]: https://developers.nest.com/documentation/cloud/authorization-overview#pin-based-authorization
[Postman]: https://www.getpostman.com/

[Google Sheet]: https://docs.google.com/spreadsheets/d/1ir8ENcChkleHsPGUWlmbGlXQQTnxPHI-o29nMX9jvO8/edit
[Graph is here]: https://docs.google.com/spreadsheets/d/1ir8ENcChkleHsPGUWlmbGlXQQTnxPHI-o29nMX9jvO8/pubchart?oid=280457042&format=interactive
[Gist here]: https://gist.github.com/kylejcarlton/12a85c4a5b375eaff62ee509d76a6720
