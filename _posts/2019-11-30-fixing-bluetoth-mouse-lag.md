---
layout: post
title:  "Fixing bluetooth mouse lag on Ubuntu"
date:   2019-11-30
excerpt_separator: <!--more-->
---

Friday morning, you are sitting on a train heading to Praha, building some xml layouts for a new screen of the app you're working on right now.

Meantime your fiancée is a bit sad because she left her mouse at home.

The solution is pretty easy, as you are using the keyboard for almost everything: you will lend her your [Logitech G603 Bluetooth mouse](https://www.logitechg.com/en-us/products/gaming-mice/g603-lightspeed-wireless-gaming-mouse.html).

Steps to reproduce the story:
1. Switch on the mouse
2. Open its panel in the Bluetooth settings
3. Click `Remove Device`
4. **Disaster**
 <!--more-->

---

Next time I reconnected the mouse with my notebook the cursor moved only around **30 Hz**. It was incredibly annoying, so I started to search for solutions immediately.
Unfortunately, none of them worked for me...

Finally, after a couple of weeks, I found this [article](https://unix.stackexchange.com/questions/539122/bluetooth-mouse-lag), and I got my good old friend back!

The solution was:

`sudo vim /var/lib/bluetooth/pc_bluetooth_address/mouse_bluetooth_address/info`

Just replace `pc_bluetooth_address` and `mouse_bluetooth_address` with your device's values like `A1:B2:C3:D4:E5:67`

Add these lines to the end of that file:
```
[ConnectionParameters]
MinInterval=6
MaxInterval=7
Latency=0
Timeout=216
```
Press `:wq`, or save and exit, if the command above is not familiar for you, and it's done.

Note: I had to reboot my notebook, the reconnection of the mouse was not enough for the changes to take place.

Thanks to solution above I'm able to enjoy my wireless setup again, only one cable to AC is needed.

Considering that I'm using Ubuntu daily for almost 2 years, and this issue was the greatest one: well done, I like this OS.
