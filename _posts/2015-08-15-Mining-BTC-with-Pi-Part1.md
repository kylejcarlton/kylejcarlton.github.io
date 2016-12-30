---
layout: single
excerpt: "Raspberry Pi and Butterfly Labs Miner Build."
author_profile: true
permalink: /:categories/:year/:month/:day/:title/
header:
  overlay_image: raspi.jpg
title:  "Mining Bitcoin with Pi - Part 1"
tags:
  - Bitcoin Mining
---
In early 2013 I heard about Bitcoin and decided the best way to learn about it would be to start mining. If you don’t know anything about Bitcoin, check out the video below and [bitcoin.org] for more.
<br><br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/Gc2en3nHxA4" frameborder="0" allowfullscreen></iframe>
<br><br>
I ordered the [Butterfly Labs] 5 GH/s Miner in July of 2013 and actually received it in December 2013, ASIC mining was catching on like wildfire and they had a decent backlog of orders to fulfill. The most similar product they offer currently is a [10 GH/s miner]. The miner connects as a peripheral via USB, so I started mining with it connected to my Windows 7 lap top using the EasyMiner Software. This reliance on my lap top to “drive” the miner wasn’t optimal, so I found a Linux based alternative that runs on Raspberry Pi called [MinePeon]. Now I had a reliable hardware setup that required little maintenance and more importantly my lap top was free to roam.
<br><br>
![Butterfly Labs Miner and Pi]({{ site.url }}/images/Butterfly_and_Pi.png )     
<br><br>

Hardware in place, I decided to join a mining pool since my hash rate would be contributing such a small percentage of the entire Bitcoin network’s computing power. I joined BTC Guild, which has since shut down due to security and regulatory concerns. [P2Pool], [Eligius] and [BitMinter] are some mining pools still in existence. This [CoinDesk article] provides more information about mining pools.
At this point I just sat back and let the miner burn electricity. Was I making a profit? Only time would tell. Next update I’ll summarize the profitability of mining with this hardware from December 2013 - July 2015.

[bitcoin.org]: https://bitcoin.org
[Butterfly Labs]: http://www.butterflylabs.com/
[10 GH/s miner]: http://www.butterflylabs.com/
[MinePeon]: https://minepeon.com/
[P2Pool]: http://p2pool.org/
[Eligius]: http://eligius.st/~gateway/
[BitMinter]: https://bitminter.com/
[CoinDesk article]: http://www.coindesk.com/information/get-started-mining-pools/
