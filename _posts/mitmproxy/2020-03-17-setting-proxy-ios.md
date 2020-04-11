---
layout: post
title:  "Setting a proxy on an iOS device"
date:   2020-03-17 20:20:21 +0200
categories: mitmproxy
published: true
exclude: true
---

_For the purpose of this tutorial, I used an iPhone running **iOS 13**._

Before you start, make sure that both the target device and the proxy (the computer running **mitmproxy**) are on the same network.

On the target device, open **Settings** > **Wi-Fi**, show informations from your current network (hit the small **_i_** icon) and select **Configure Proxy**.

Use **Manual** and set the following values : 
- Server : _the local IP address of your computer running **mitmproxy**_
- Port : _8080_
- Authentication : _keep disabled_

![SetApp](/assets/images/mitmproxy/network-settings-ios-01.png)

To get the local IP address of the computer serving as proxy, generally the easiest way is to open its network settings (**Settings** > **Network** on **MacOS**) and look for its local IP address, _192.168.1.42_ in this example : 

![MacOS network settings](/assets/images/mitmproxy/network-ip-macos.jpg)

<a href="{% post_url mitmproxy/2020-03-17-mitmproxy %}#network-settings-on-the-target-device">_< Back to the **mitmproxy** tutorial_</a>
