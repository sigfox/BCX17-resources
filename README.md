# Bosch Connected Experience 2017 - Useful resources

[TOC]

## Sigfox basics

* [Developer Portal](http://makers.sigfox.com)
* [Youtube videos](http://youtube.com/sigfox)
* [Q&A Platform](http://ask.sigfox.com)
* [Github code samples](http://github.com/sigfox)
* [Network coverage : 32 countries and counting ;) ](http://sigfox.com/coverage)
* [BCX17 Intro slides](https://www.slideshare.net/nicolsc-slides/bcx17-sigfox-intro)


## Sensit

### Register

You need the unique `ID` of the device (printed on the back), and its `PAC`(provided 1 to 1)

Head out to the [Sigfox backend](https://backend.sigfox.com/activate/sensit) and fill in the form.  
(If you already have a Sigfox account, log in first)


You will receive an automated email asking you to setup your password and confirm your account

You're good to go!

---
⚠ It may take up to one hour to attach the device to your account, in case it was previously attached to a demo account.
> Good things come to those who wait 😉 


### First message

Double-press the Sensit button.  
The LED should blink for ~6 seconds, while its sending a first message

### First steps on Sigfox backend

Log-in to your [Sigfox account](http://backend.sigfox.com), and then access the `DEVICE TYPE` menu, then click on the name of the newly created device type. It should be called `Sensit EVK`

#### Access your device

Enter the `Associated Devices` menu, and click on the ID of the only one listed.
![Associated devices](./img/devices.png)

### Check messages
![Messages menu](./img/device-menu.png)

Your incoming messages will appear here, listing the time of reception, raw data payload & reception quality indicators.
![Messages panel](./img/messages.png)

### Push data to your server

To push incoming data to your server, you need to use the _callbacks_ mechanism.  
Callbacks are configured at the _device type_ level. Which means that the day you'll have 3 millions units of your device, you'll need to set the configuration once for all of them 

#### Access the callbacks menu

In your _device type_ entry, click on the _callbacks_ menu in the left panel.  

#### List of existing callbacks (if any)

The `callbacks` screen list the configured callbacks, and allow you to activate/deactivate any of them on the fly.

![Callbacks](./img/callbacks-list.png)

#### Set up a new callback

On the _callbacks_ page click on the `New` button in the top right corner.  

Select `AWS IoT` or `Azure IoTHub` if you want to use these services, through the easy Sigfox bridge built with them.

Select `custom callback` if you just want to set up a callback to any URL.

Then select `DATA` > `UPLINK` to make sure it will be called upon any incoming transmission.  
Then set up URI, HTTP method, content type, HTTP headers & request body

![New callback](./img/new-callback.png)

### Parse the incoming data frame

Checkout the documentation about the [Sensit](http://sensit.io) frames. 
The data is sent packed in a few bytes, containing active mode, sensor values, battery status, ...  

* [Uplink messages](./Sensit-uplink-frames.md) : Messages from the device to the network
* [Downlink messages](./Sensit-downlink-frames.md) : Messages from the network to the device


---
⚠ To enforce the ETSI regulation on the 868MHz, the Sensit won't send more than 6 messages per hour (duty cycle).  
If you need to send more than that for testing purposed during the week end, [contact Nicolas](mailto:devrelations@sigfox.com) 



## NXP Development board

The complete documentation of the `OL2385` development kit is available on [NXP website](http://www.nxp.com/products/rf/low-power-tx-rx-ics/sub-ghz-rf/om2385-sf001-ol2385-wireless-sub-ghz-transceiver-sigfox-development-kit-with-kl43z:OM2385-SF001)

Please get in touch with Vivek Tyagi if you need more information about this kit.


## Using the Sigfox API

⚠ API should be used as a sync/backup mechanism.  
We recommend using the callbacks (push) as primary data access channel. No need to poll the API every second ;)

Extensive documentation of the Sigfox API available [here](./Sigfox-API.md) 

You'll also find the API documentation in the `Group` > `Api Access` menu.  
Once you set up a pair of API credentials (look for the `new` button on the top right), you'll have a link to the documentation matching the access rights you chose.

## Hono Platform

Mohammad Zubair (@Bosch SI) wrote a Sigfox adapter for the [Eclipse Hono](http://hono.bosch-iot-suite.com) platform.  
Read the documentation & step-by-step guide [here](./Sigfox-to-hono.md)