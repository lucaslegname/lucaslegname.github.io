---
layout: post
title:  "Using mitmproxy through Docker with a M1 Mac"
date:   2021-01-22 10:20:20 +0200
categories: mitmproxy
tags: [mitmproxy]
excerpt: "In a previous post, I suggested using mitmproxy through Docker. So as I freshly received my M1-powered Macbook Air, I wanted to do the same thing with my new computer."
published: true
---

The new M1-powered Macs are available since a few months now and even though most apps are already compatible with the ARM chip (natively or through **Rosetta 2**), there is still one big missing : **Docker**.

To be accurate, Docker is currently available on M1 Macs but only as a **Tech Preview**. If you are looking for something stable, you should better wait a bit but if you don't mind wasting some time on weird issues and want to experience Docker on ARM before everyone else then this build is for you.

In a previous post, I suggested using **mitmproxy** through **Docker** to keep you computer clean and ease versions switching. So as I freshly received my M1-powered Macbook Air, I wanted to do the same thing with my new computer.

![Docker logo](/assets/images/docker/logo.png)

## Let's run the official mitmproxy Docker image

Once Docker was set up and running, I naturally tried to use the official **mitmproxy** Docker image as I used to do on my previous Intel-powered Mac. 

So I simply ran the following command :

```bash
docker run --rm -it -p 8080:8080 mitmproxy/mitmproxy:6.0.2
```

And... it mostly worked ! I was able to use the official **mitmproxy** image thanks to Docker's platform emulation but I still got this annoying warning message on start : 

```bash
WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested
```

This message appears because mitmproxy does not offer multi platform images for the moment.

If you check the tags from mitmproxy's <a href="https://hub.docker.com/r/mitmproxy/mitmproxy/tags" target="_blank">DockerHub page</a>, you will notice that there is only one architecture (`linux/amd64`) per tag.

So if we want to avoid the previous warning message and to enjoy better performance, we need to build a multi platform **Docker** image.

![mitmproxy is alive](/assets/images/mitmproxy/mitmproxy-empty.jpg)

## Building a multi platform Docker image

As the **mitmproxy** project source code is <a href="https://github.com/mitmproxy/mitmproxy" target="_blank">available on Github</a>, I wanted to try building my own ARM-friendly Docker image. So let's checkout the code and see how it goes. 

After a few researches, I found out I would need to use Docker **Buildx** to create a multi platform image. Buildx is an experimental feature so if you intend to use it, make sure experimental features are enabled in your Docker settings.

The good news is that the rest is straightforward, once you located the **Dockerfile** from the project, execute the following command :

```bash
docker buildx build --push -t lucaslegname/mitmproxy:6.0.2 --platform linux/amd64,linux/arm64 --file release/docker/Dockerfile .
```

This command will build a Docker image compatible with the `linux/amd64` and `linux/arm64` platforms and push it to <a href="https://hub.docker.com/r/lucaslegname/mitmproxy" target="_blank">a DockerHub repository</a>.

Once you are there you can run this new M1-friendly image using the following command : 

```bash
docker run --rm -it -p 8080:8080 lucaslegname/mitmproxy:6.0.2
```

Now you can run **mitmproxy** through **Docker** with an optimised image and you even know how to create multi platform images using **Buildx**. Enjoy !

### Sidenote

By default, M1-Macs build `linux/arm64` images so you can also use **Buildx** in case you need to share **Docker** images with people ~~still~~ using Intel-powered computers.

## References

- <a href="https://www.docker.com/blog/multi-arch-build-and-images-the-simple-way/" target="_blank">Multi-arch build and images, the simple way</a>
- <a href="https://medium.com/better-programming/how-to-actually-deploy-docker-images-built-on-a-m1-macs-with-apple-silicon-a35e39318e97" target="_blank">How to Actually Deploy Docker Images Built on M1 Macs With Apple Silicon</a>
