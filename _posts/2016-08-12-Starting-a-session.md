---
layout: drozer-guide
title: "Starting a Session"
description: 
categories:
  - drozer
  - usage
tags:
---

## Starting a Session

You should now have the drozer Console installed on your PC, and the Agent running on your test device. Now, you need to connect the two and you’re ready to start exploring.

We will use the server embedded in the drozer Agent to do this.

If using the Android emulator, you need to set up a suitable port forward so that your PC can connect to a TCP socket opened by the Agent inside the emulator, or on the device. By default, drozer uses port 31415:

`$ adb forward tcp:31415 tcp:31415`

Now, launch the Agent, select the “Embedded Server” option and tap “Enable” to start the server. You should see a notification that the server has started.

Then, on your PC, connect using the drozer Console:

`$ drozer console connect`

If using a real device, the IP address of the device on the network must be specified:

`$ drozer console connect --server 192.168.0.10`

You should be presented with a drozer command prompt:

```
selecting f75640f67144d9a3 (unknown sdk 4.1.1)  
dz>
```
The prompt confirms the Android ID of the device you have connected to, along with the manufacturer, model and Android software version.

You are now ready to start exploring the device.
