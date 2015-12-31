---
title:  "STACK4 Life Sign"
date:   2015-12-31 15:00:00
categories: [general, unity, dlang]
---

![STACK4 firetv]({{ site.url }}/assets/s4-firetv.png){: .center-image }

During the holidays I finally found the time to work a little bit on my mobile app [STACK4](http://www.stack4.de).
It has been a while but I still have a few points on my todo list for this project.

### Multiplayer on iOS

I want to update the iOS Version to be on par with android, especially to support online multiplayer too. Finally iOS Players will be able to play against Android users.

In order to do that the biggest roadblock was connecting to the Apple Push Notification Service (APNS).
The STACK4 Backend is written in [dlang](http://dlang.org/) and there is no implementation yet that supports APNS.
To have a working version on the market soon I decided to use a working proven software as a middleware instead of rewriting it in D (for now): [node-apn](https://github.com/argon/node-apn)
The plan is still to implement it later in D natively: [apn-d](https://github.com/Extrawurst/apn-d).

It is easy to provide a REST interface in node and to connect to a REST interface from D so this is the route I am taking for now.

### New UI

Unity for years had only one official method to implement User Interfaces: OnGUI (an intermediate GUI) which was ineffiecient and difficult to get visually appealing.

It was easy to prototype stuff with it but hard to get a scalable UI that worked for all display sizes and used little drawcalls. Another caveat: No support for gamepad/key input.

Supporting more platforms and more sophisticated UI Screens was no option with the old system. So switching to the new uGUI was also on my list. The following Screenshots all show the new UI using uGUI that is fully controllable using gamepads and keyboards too.

### FireTV & AppleTV

I am a big fan of the FireTV and new Apple-TV and supporting these platforms aswell would be cool. 

The biggest issue with them is the input. STACK4 used to be controlled via touch exclusively and now it needed GameController Input aswell. As mentioned before this was hard to do with the old UI. Thanks to the new UI we have key/gamepad support now (an early version). 

Some screens are not yet implemented and some flows do not work yet but I am on a good way and hope to finish this in January 2016. Following you see a low-res gif-video of the new UI and how it is controlled using Keys:

![STACK4 new ui]({{ site.url }}/assets/s4-key-controls.gif){: .center-image }
