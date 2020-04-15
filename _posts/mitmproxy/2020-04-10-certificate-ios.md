---
layout: post
title:  "Installing a self-signed certificate on an iOS device"
date:   2020-04-10 20:20:20 +0200
categories: mitmproxy
published: true
exclude: true
---

_For the purpose of this tutorial, I used an iPhone running **iOS 13**._

## Let's mitm.it

On the phone or tablet, open a web browser and go to the following url : 
```
http://mitm.it
```

If the network settings are properly set on your device, you should land on a page looking like this one :

![Certificate](/assets/images/mitmproxy/certificate-install-ios-01.png)

Hit the Apple logo in order to download the mitmproxy certificate. An alert should appear, saying that you are about to download a configuration profile : hit **Allow**.

Another alert will confirm that the profile has been downloaded and that you need to review it before installing it.

## Installing the profile

Go to **Settings**, on the main screen you should see a new entry named **Profile Downloaded**.

![Certificate](/assets/images/mitmproxy/certificate-install-ios-02.png)

Open it and a modal allowing you to install the configuration profile (containing the mitmproxy certificate) should appear : hit **Install**. iOS will ask for you password and then you will have to hit **Install** once again.

## Enabling the certificate

Last but not least, you need to enable the mitmproxy certificate. First, go to **Settings** > **General** > **About**.

Now, at the bottom of the list, hit **Certificate Trust Settings** and enable the mitmproxy certificate.

![Certificate](/assets/images/mitmproxy/certificate-install-ios-03.png)

That's it! The mitmproxy certificate is now installed on your device : let's go back to the main tutorial and try catching a few requests.

<a href="{% post_url mitmproxy/2020-04-10-mitmproxy %}#installing-the-self-signed-certificate">_< Back to the **mitmproxy** tutorial_</a>