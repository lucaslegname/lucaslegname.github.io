---
layout: post
title:  "Installing a self-signed certificate on a tvOS device"
date:   2020-04-10 20:20:20 +0200
categories: mitmproxy
published: true
exclude: true
---

_For the purpose of this tutorial, I used an Apple TV 4K running **tvOS 13**._

The first thing to know when you want to install a certificate on an Apple TV is that there is no sub-menu in the **tvOS** settings allowing you to do so.

Fortunately, it is possible to proceed downloading a certificate file directly from the tvOS device. I will show you how to do this.

## Locating the mitmproxy certificate

Depending on how you installed mitmproxy on your computer, the certificate folder should be either `~/.mitmproxy` or the local folder you passed as parameter in the **Docker** command.

Once you found the folder, locate the `mitmproxy-ca-cert.cer` file among the multiple certificate files. If it is not there, then try starting **mitmproxy** and check once again.

## Uploading the certificate

We now need to send the certificate (the `mitmproxy-ca-cert.cer` file) to your Apple TV. To do so, we first need to host it somewhere.

You can use <a href="http://dropbox.com" target="_blank">Dropbox</a> for example : send the file in the cloud and get the **download link**. You should get something looking like this : 

```
https://www.dropbox.com/s/ppldnxk2531p4ze/mitmproxy-ca-cert.cer?dl=0
```

Just replace the `?dl=0` with `?dl=1` and you get a link totally usable by your device.

## Installing the certificate

Go to **Settings** > **General** > **Privacy**, select **Share Apple TV Analytics** and hit the **PLAY** button on the remote.

![Download profile](/assets/images/mitmproxy/network-settings-tvos-01.jpg)

You should be in the **Profiles** hidden menu. From there, click on **Add Profile** and enter the url from your hosted certificate.

![Download profile](/assets/images/mitmproxy/network-settings-tvos-02.jpg)

Once you are done, hit **Done** and the certificate should be added to your Apple TV.

## Enabling the certificate

Last but not least, you need to enable the mitmproxy certificate. First, go to **Settings** > **General** > **About**, select the **Certificate Trust Settings** sub-menu and hit **OK**.

![Enable certificate](/assets/images/mitmproxy/certificate-install-tvos-01.jpg)

Here you should see your freshly added mitmproxy certificate. To trust it, just select the certificate, click **OK** then **Continue**. Once tvOS mentions **Trusted** next to the certificate, you are good.

![Enable certificate](/assets/images/mitmproxy/certificate-install-tvos-02.jpg)

That's it! The mitmproxy certificate is now installed on your device : let's go back to the main tutorial and try catching a few requests.

<a href="{% post_url mitmproxy/2020-04-10-mitmproxy %}#network-settings-on-the-target-device">_< Back to the **mitmproxy** tutorial_</a>
