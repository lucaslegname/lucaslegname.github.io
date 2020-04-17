---
layout: post
title:  "Setting a proxy on a tvOS device"
date:   2020-04-10 20:20:20 +0200
categories: mitmproxy
published: true
exclude: true
---

_For the purpose of this tutorial, I used an Apple TV 4K running **tvOS 13**. Please note that since **Apple Configurator** is necessary to set a proxy on Apple TV, a Mac is needed to realize the following steps._

The first thing to know when you want to set a proxy on an Apple TV is that there is no sub-menu in the **tvOS** settings allowing you to do so.

Fortunately, it is possible to proceed using a configuration profile created with **Apple Configurator 2**. You can download this tool from the <a href="https://apps.apple.com/us/app/apple-configurator-2/id1037126344?mt=12" target="_blank">Appstore</a>.

## Configuration profile creation

Start **Apple Configurator** and create a new profile (**File** > **New Profile**). Select the **Wi-Fi** section on the left then hit **Configure**.

Here enter the name of your network (SSID), the security type (WEP/WPA2/...) and the password of your wireless network.

Then set the **Proxy Setup** to **Manual** and use the following values : 
- Host name : _the local IP address of your computer running **mitmproxy**_
- Port number : _8080_
- _Leave other fields blank or with default value_

![Apple Configurator 2](/assets/images/mitmproxy/apple-configurator-wifi.jpg)

To get the local IP address of the computer serving as proxy, generally the easiest way is to open its network settings (**Settings** > **Network** on **MacOS**) and look for its local IP address, _192.168.1.42_ in this example : 

![MacOS network settings](/assets/images/mitmproxy/network-ip-macos.jpg)

Save this new configuration profile somewhere : you should get a `.mobileconfig` file.

## Uploading the file

We now need to send the configuration profile to your Apple TV. To do so, we first need to host it somewhere.

You can use <a href="http://dropbox.com" target="_blank">Dropbox</a> for example : send the file in the cloud and get the **download link**. You should get something looking like this :

```
https://www.dropbox.com/s/ppldnxk2531p4ze/atv.mobileconfig?dl=0
```

Just replace the `?dl=0` with `?dl=1` and you get a link totally usable by your device.

## Installing the profile

Go to **Settings** > **General** > **Privacy**, select **Share Apple TV Analytics** and hit the **PLAY** button on the remote.

![Download profile](/assets/images/mitmproxy/network-settings-tvos-01.jpg)

You should be in the **Profiles** hidden menu. From there, click on **Add Profile** and enter the url from your hosted configuration profile.

![Download profile](/assets/images/mitmproxy/network-settings-tvos-02.jpg)

Once you are done, hit **Done** and the profile should be added to your Apple TV.

<a href="{% post_url mitmproxy/2020-04-10-mitmproxy %}#network-settings-on-the-target-device">_< Back to the **mitmproxy** tutorial_</a>
