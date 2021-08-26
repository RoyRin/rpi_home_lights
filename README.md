# Raspberry Pi lights!


In 2016, I moved to my first apartment in NYC. To truly embrace living in NYC, I decided to go all in, and rent an apartment where my room did not have a window. In order to prevent my circadian rhythm from going completely haywire, I decided to make my Raspberry Pi control my sound and lights system, and act as a sunrise in the morning Here's an outline of how I went about connecting my system.

![lights](https://github.com/RoyRin/rpi_home_lights/blob/main/pictures/lights.png)

## Parts List:
### Required:
- [Raspberry Pi](https://www.raspberrypi.org/products) $35. A Raspberry Pi is effectively a computer, so once you figure out how to interface with it, you can treat it as such.
- [IR controllable Lights](https://www.amazon.com/gp/product/B00ASHQQKI/ref=oh_aui_search_detailpage?ie=UTF8&psc=1) $25
- At least 1 IR LED $1 

### Recommended
- [Wifi Dongle](https://www.amazon.com/Official-Raspberry-Pi-WiFi-dongle/dp/B014HTNO52/ref=sr_1_3?s=electronics&ie=UTF8&qid=1506220801&sr=1-3&keywords=raspberry+pi+wifi+dongle). ~ $12
- [GPIO break out board](https://www.amazon.com/Breakout-Board-Ribbon-CableRaspberry/dp/B00OJHF8WU/ref=sr_1_12?ie=UTF8&qid=1506220563&sr=8-12&keywords=raspberry+pi+gpio+breakout+board). ~10
- A breadboard

Total : ~80 ish (if you have nothing)
Profits: 0

Rewards: You can wake up feeling like you’re waking up to the sunrise, even in your room. 
Except you’re not waking up to the sun, you’re trapped in a cage of your own design, and 
there is escape, but not for you. You’re going to wake up, go to work, gossip about Kendra, 
then go back to sleep, like you’ve done 100 times before. Only now, you’ll feel slightly better 
doing it.

Is it worth it: Likely.
------------------------------------------------------------


## Raspberry Pi Set up: 
(There are plenty of resources on the internet that go into greater detail about how to set up and connect to your Raspberry Pi).

![rpi](https://github.com/RoyRin/rpi_home_lights/blob/main/pictures/rpi.png)

From there you can very easily connect from your laptop, into the Raspberry Pi to 
tell it commands (using ssh) so at that point I have a Raspberry Pi which I can control from 
my laptop, as long as I am on the same wifi. [Here is an Instructables on the matter](http://www.instructables.com/id/Use-ssh-to-talk-with-your-Raspberry-Pi/).


## Now the actual lights project began: 

It doesn’t really matter what LEDS you buy, as long as they have some IR remote control. I 
recommend these lights regardless, because they are nice even if you don’t do this entire 
project.

![remote](https://github.com/RoyRin/rpi_home_lights/blob/main/pictures/remote.png)

I recommend get Velcro packs, so that you can hang up the remote around the room.

The important part about the lights you buy is that they have a remote that uses IR 
(infrared) to communicate to a little box (the white thing in the picture) to tell it to turn on, 
and what lights to turn on Red Green Blue, or any combination I found this thing online. 
The way these controllers work is that they pulse IR blasts in a certain pattern to 
communicate a particular signal. So our goal now, is to get the Raspberry Pi somehow to 
communicate those codes instead of the controller.

1. First thing, I did was I bought 1 IR LED. Well, in reality, you can’t buy 1 LED, so I bought 50, 
but it cost about a dollar, so it was a sacrifice I was willing to make. Connecting the IR led to 
the GPIO can be dangerous, as you can fry your entire Raspberry Pi – so be careful.
http://www.raspberry-pi-geek.com/Archive/2015/10/Raspberry-Pi-IR-remote and other 
forums say that you need to use a transistor, but powering a single LED does not require 
very much ampage, and it turns out that you don’t need very high speeds, so just 
connecting your LED to a pull up resistor is fine. (Namely, 5V to Resistor, to LED, to Pin out 
18, and then controlling pin out 18). That being said, I’m not responsible for your 
electronics – you gotta do what you gotta do. 

![IR_led](https://github.com/RoyRin/rpi_home_lights/blob/main/pictures/IR_led.png)

2. So now, my Raspberry Pi GPIO (pin in/outs) are connected to an IR LED, so now we need to 
tell them what to do.
You can find most of these patterns for any IR remote controller at [LIRC.org](http://www.lirc.org/html/lircd.html ) which basically tells you the IR "codes" for nearly any kind of controllers. To find the IR controls for your particular remote you may have to  search the internet for a little bit, but If you want to use the same LEDS at the one that I posted, you can find the config file attached on the forum : https://sourceforge.net/p/lircremotes/mailman/lirc-remotes-users/?viewmonth=201503 (just ctrl-f for “lirc44.conf”). 
Now, you need to set up “lirc” on your Raspberry Pi, to be able to interpret those codes that 
you just found.


Here is a nice tutorial for how to set up [LIRC on your Raspberry Pi](http://alexba.in/blog/2013/01/06/setting-up-lirc-on-the-raspberrypi/).
1. Now you can control your lights from your home computer, by ssh-ing into your raspberry 
pi, then typing in the command irsend SEND_ONCE LED_44_KEY POWER. Or any name
you want in place of “Power”, as it is written in your .conf file. (This should all be explained in
the LIRC webpage).
2. Now I wrote a nice Python program, so that it would do a chain of events one after another 
(I’ve copied the code into the end of this document).
And now we are nearly done. 
3. Crontab is a program that will allow you to schedule events on linux (i.e. run some program 
every 5 minutes, or every Friday at 6:01 pm). [Install crontab as instructed here](https://www.raspberrypi.org/documentation/linux/usage/cron.md).
Then type in `crontab –e` into terminal
And you can set your own schedule:

And Ta Da! You have a light-alarm clock!
-----

Note: If you want to incorporate sounds into this, it’s very easy, since nearly all Raspberry 
Pi’s (with the exception of the Zero) have a headphone jack (or bluetooth). And you can easily write into your code to play a sound from a saved file on your computer. 
We leave this as an exercise for  the reader. 

Python 3 code:

