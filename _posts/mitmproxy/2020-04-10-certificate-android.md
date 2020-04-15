---
layout: post
title:  "Installing a self-signed certificate on an Android device"
date:   2020-04-10 20:20:20 +0200
categories: mitmproxy
published: true
exclude: true
---

_For the purpose of this tutorial, I used a Samsung Galaxy S9 running **Android 9**._

## Let's mitm.it

On the phone or tablet, open a web browser and go to the following url : 
```
http://mitm.it
```

If the network settings are properly set on your device, you should land on a page looking like this one :

![Certificate](/assets/images/mitmproxy/certificate-install-android-01.jpg)

Hit the Android logo in order to download the **mitmproxy certificate**. An alert should appear, saying that you are about to download the `mitmproxy-ca-cert.pem` file : hit **Download**.

Another alert will ask you to set a password on your device in order to use self-signed certificates, if the device already has a password you will be asked to key it in.

Now you should be asked for a certificate name, insert any name then hit **OK**.

On **Android** self-signed certificates installed by users are automatically enabled so your device should be ready at this point.

If you need to manage certificates you installed, you can find them when you go to **Settings** > **Biometrics and security** > **Other security settings** and hit **User certificates**.

---

That's it! Now the mitmproxy certificate is installed on your device : let's go back to mitmproxy tutorial and try catching a few requests.

<a href="{% post_url mitmproxy/2020-04-10-mitmproxy %}#installing-the-self-signed-certificate">_< Back to the **mitmproxy** tutorial_</a>