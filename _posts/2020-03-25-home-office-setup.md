---
layout: post
title:  Home office setup
date:   2020-03-25
excerpt_separator: <!--more-->
---

Given that we should prepare ourselves for a quite long home office run I dig deep into my - almost forgotten - carton box, which contains a lot of cables, webcams, spare keyboards and mouses, etc. to create a reasonable setup for working from home.

The basic idea was, that as I have a Bluetooth keyboard and mouse too (which I was using in the office in general) I should use them to connect both my laptop and PC and control them with the same input sources. I wanted to create such a setup where hitting a few buttons can help me to switch between those 2 devices wireless, without the need to disconnect/reconnect USB and HDMI cables.
 <!--more-->
Devices:
* [Asus Zenbook](https://www.asus.com/Laptops/ASUS-ZenBook-UX410UA/), which I use for work
* PC for Spotify, gaming, and personal life in general
* [Logitech K380 Bluetooth Keyboard](https://www.logitech.com/en-us/product/multi-device-keyboard-k380)
* [Logitech G603 Lightspeed Bluetooth Mouse](https://www.logitechg.com/en-us/products/gaming-mice/g603-lightspeed-wireless-gaming-mouse.html)
* [24" Monitor](https://www.dell.com/lv/business/p/dell-p2414h/pd)

## Starting with the screen setup

### Cables / connectivity
As my screen has only 1 HDMI and 1 DisplayPort I had to realize, that without using both of those plugs I won't be able to switch easily. Previously I used only the HDMI, so I needed a DisplayPort cable for the win. But considering that neither my PC nor my laptop has a DisplayPort, I was in trouble a bit, which was solved by a borrowed Type-C <-> DisplayPort cable which was ok for the laptop.

So the final version looks like this:

Laptop `{Type-C} <-> {Displayport}` Screen `{DVI-D} <-> {HDMI}` PC

### Color setup

Connection established, I'm sitting in front of the screen with a huge smile on my face, which lasted only for seconds. After switching to the laptop's input the screen seemed to be grey, and without proper colors. I was in a small panic for a couple of minutes, but after that period the logical part of my brain won, and I started debugging: cables changed, tried several setups, google searches for the issue, so you can imagine, I did everything possible.

 As expected a StackOverflow post helped me, which was written like the user manuals of the devices targeting users, who are not familiar with the product. It contained the general rules: turn it off / on, switch cables, check connectivity, and **check the brightness levels**. Yes, that's true. I just had to adjust the brightness for the DisplayPort input to the same level as the HDMI input was, and the screen turned into beauty mode: perfect colors, especially white things were finally white, not something like [beige](https://en.wikipedia.org/wiki/Beige).

**Fun fact about beige**

 I know that color only because once I ordered a taxi in my hometown, and the operator told me, that a beige Mercedes will pick me up in a couple of minutes. After the call, I googled immediately for this word, as I knew that time already, that it's a color, but I wasn't aware of the position on the [color palette of men](https://9gag.com/gag/106661/how-women-and-men-see-colors).

### Hotkeys

After a couple of switches between the input sources, it felt annoying that I have to go deep into the menu of the screen to perform this pretty easy task, especially that both the buttons and the menu layout comes with quite uncomfortable UX. Fortunately, I found out that I can assign custom actions to the 2 top buttons. Using that now I have to push the top button twice to switch to the laptop, and the other one twice for the PC's signal. Awesome!

---

## Mouse

The mouse was the easiest one, as it works both with Bluetooth and through its USB receiver. I plugged in the receiver into the PC and connected the mouse to the laptop via Bluetooth. Switching between them is quick, I just have to push a button on the bottom of the mouse. It indicates with a different color the state, so I have the feedback directly from the device about the state.

## Keyboard

I thought it will be simple to use my keyboard with both devices, as it allows you to connect 3 different Bluetooth channels, and the keyboard has hotkeys to switch between them. With the laptop it was working already, but the PC has no built-in Bluetooth adapter, so I needed to dig into my backup carton box for a USB Bluetooth adapter. After a 1 hour search it turned out that it disappeared somehow, I could not find it anywhere, even though I knew I had one at some point in the past. I tried to connect it with the mouse's receiver (even though I didn't know which kind of communication channel it's using, but I thought it's ok to give it a shot), but after a couple of unsuccessful rounds it turned out, the receiver can accept other Lightspeed devices only.

I received prompt help from my neighbor (who is also my colleague); he lent me the exact device I needed: a Bluetooth adapter with USB he did not use anymore. I plugged it into the PC, paired with the keyboard and finally (after a bunch of victorious switches between the laptop and PC) I felt happiness and satisfaction.

## Lack of numpad

In the afternoon my friends invited me to a CS GO match, and at the beginning of the first game, I just realized an issue with this setup: [the previous keyboard](https://en.roccat.org/Support/Product/Ryos-MK) I used for the PC (now it's part of my girlfriends home office setup) had numpad, but with the actual setup I had to say goodbye for that part of the keyboard. If you're wondering why is it an issue: to buy weapons and equipment like grenades and kevlar I had hotkeys assigned to the numpad; I just hit `15789+` on numpad and I was fully equipped with an AK47, kevlar vest and helmet, and 1 from each type of the grenades.

So I needed to [create new keybindings](https://csgobuynds.com/buy-binds-generator.html#!/), but I found still some unused keys in the game: the weapons went to the arrow keys, and the equipment is now available with the `F6 - F12` buttons.

## Lights
As my flat is quite dark (we have only 1 window, which isn't towards the sunlight) I had to implement some additional lighting near my desk. Fortunately, I had an unused spotlight at home which I mounted to the wall. It's pointing to the white ceiling, which reflects the light properly: not to disturb my eyes, but create a warm space in that corner. I connected it into a power strip, which I can switch off for nights, as the spotlight had no built-in switch.

## Room for improvement

1. The jack cable

    There is one cable I still have to exchange: the jack cable for my headphones or the speakers. It happened already a couple of times that during the day I needed the headphone for communicating with my team, but for example, during lunch I wanted to listen to some jazz music from the speakers. It's not a major pain to exchange those cables, but I was thinking already about how to resolve the issue. I also have a [bluetooth audio receiver](https://store.mi.com/in/item/3180600001), I'm pretty sure somehow I'll be able to add it to the setup and avoid changing those cables too.

2. Decoration

    Unfortunately, as I mentioned there is no sunlight in the desk's corner, so it's not possible to have some nice plants there. But it just came to my mind, that I received 2 cool posters from one of my teammates, maybe I'll put it on the walls somehow.

## The final version

<img src="https://matevojts.github.io//assets/images/IMG_20200321_122133.jpg" style="display: block; margin: auto;" />

As maybe you realized from the photo I also have a sit/standing desk to maintain my health. I have to mention that it's the best furniture I ever had. More in [this post](https://www.matevojts.hu/2019/12/02/standind-desk.html).

I hope I could give you some ideas about how to improve your setup too, to survive this period with minimized interruption and using the tools you maybe already have at home.
Have a nice day!
