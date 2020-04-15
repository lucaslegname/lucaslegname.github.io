---
layout: post
title:  "Setting a proxy on an Android device"
date:   2020-04-10 20:20:20 +0200
categories: mitmproxy
published: true
exclude: true
---

_For the purpose of this tutorial, I used a Samsung Galaxy S9 running **Android 9**._

Before you start, make sure that both the target device and the proxy (the computer running **mitmproxy**) are on the same network.

On the target device, open **Settings** > **Connections** > **Wi-Fi**, show informations from your current network (hit the network you are already connected to) and select **Advanced**.

Change the proxy setting to **Manual** and set the following values : 
- Proxy host name : _the local IP address of your computer running **mitmproxy**_
- Proxy port : _8080_
- _Leave other fields blank or with default value_

![Android settings](/assets/images/mitmproxy/network-settings-android-01.jpg)

To get the local IP address of the computer serving as proxy, generally the easiest way is to open its network settings (**Settings** > **Network** on **MacOS**) and look for its local IP address, _192.168.1.42_ in this example : 

![MacOS network settings](/assets/images/mitmproxy/network-ip-macos.jpg)

<a href="{% post_url mitmproxy/2020-04-10-mitmproxy %}#network-settings-on-the-target-device">_< Back to the **mitmproxy** tutorial_</a>
