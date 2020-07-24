---
layout: post
title:  "Creating scripts for mitmproxy"
date:   2020-05-08 20:20:20 +0200
categories: mitmproxy
tags: [mitmproxy]
excerpt: "Now that we covered the first steps to get started with mitmproxy, I would like to develop a bit more one of the best features that the tool offers : the ability to run Python scripts."
published: true
---

Now that we covered the first steps to get started with **mitmproxy**, I would like to develop a bit more one of the best features the tool has to offer : the ability to run **Python** scripts.

While writting addons is purely optional, automating requests (or responses) editing with custom scripts considerably extends the boundaries of mitmproxy. Through this post, we will see a few concrete examples of how you can use addons to test apps.

If you already managed to set everything in order to run mitmproxy, writting scripts should not be too complicated for you. Feel free to re-use and alter the examples provided in this post in order to match your needs.

![mitmproxy](/assets/images/mitmproxy/mitmproxy.jpg)

## Writting and executing scripts

As I mentioned earlier, scripts for mitmproxy must be written using the <a href="https://en.wikipedia.org/wiki/Python_(programming_language)" target="_blank">Python</a> programming language. The Python syntax is not particularly complicated and if you already used any object-oriented programming language then you should be fine.

One advice though : **pay attention to indentation**! Python is an indent-sensitive language which means that it uses leading whitespaces (instead of curly-brackets `{}`) in order to compute the indentation level of the line. 

### Example

As the best way to learn is often to see an example, here is a simple one. This script will change the HTTP code of any response which's url matches the `filter` param.

```python
from mitmproxy import http
from mitmproxy import ctx

class ChangeHTTPCode:
    filter = "airbnb.com"

    def response(self, flow: http.HTTPFlow) -> None:
        if (self.filter in flow.request.pretty_url):
            flow.response.status_code = 500

addons = [ChangeHTTPCode()]
```

To execute the addon, just save it in a file named `myscript.py` and execute the following command :

```bash
mitmproxy -s ./myscript.py
```

Now if you try to start the **Airbnb** app with a device set to use mitmproxy, all requests will appear as errors and you should see an error message on your phone.

![Google script](/assets/images/mitmproxy/script-01.jpg)

### Events

In the previous example, we used the `response` event in order to alter the response before it reaches the device. **mitmproxy** allows developers to access to multiple **HTTP**, **TCP**, **WebSocket** or **generic** events.

The full list of supported events is available in the <a href="https://docs.mitmproxy.org/stable/addons-events/" target="_blank">mitmproxy documentation</a> but let's have a look at several useful ones.

- `load()` is triggered **when you launch mitmproxy**, it is very convenient if you need to adapt the tool's settings to run your script

```python
def load(self, loader):
    ctx.options.http2 = False
```

- `request()` is triggered **before the request is sent**, the perfect moment to override an **user-token** for example

```python
def request(self, flow: http.HTTPFlow) -> None:
    flow.request.headers["authorization"] = "Bearer NEWUSERTOKEN"
```

- `response()` is triggered **before the response reaches the device**, the right moment to override the **response content** for example

```python
def response(self, flow: http.HTTPFlow) -> None:
    flow.response.content = "custom response content"
```

### The request and response objects

Both **request** and **response** objects are part of the `http.HTTPFlow` variable passed as parameter of all HTTP events. 

Editing the request (or response) is quite simple as long as you have hints about the object structure. Often the best way to find the right attribute to use is to directly have a look at the project <a href="https://github.com/mitmproxy/mitmproxy" target="_blank">Github repository</a>.

- Here are a few useful attributes from the <a href="https://github.com/mitmproxy/mitmproxy/blob/v5.1.1/mitmproxy/net/http/request.py#L50" target="_blank">request object</a> :

```python
self.method = method
self.host = host
self.port = port
self.path = path
self.headers = headers
self.content = content
```
It is also possible to use the `self.url` property/setter to update the full url.

- Here are a few useful attributes from the <a href="https://github.com/mitmproxy/mitmproxy/blob/v5.1.1/mitmproxy/net/http/response.py#L37" target="_blank">response object</a> :

```python
self.status_code = status_code
self.headers = headers
self.content = content
```
You can also use the `self.reason` property to get the reason phrase corresponding to the status code (ex : "_Not found_" for a 404 error).

### Sending logs

The console can be useful when you are developing new scripts. To open the console, hit **E** from the main screen.

To send logs from a script, you can use the following function : 

```python
ctx.log.info("Hello world")
```

Make sure to use a level of verbosity (`debug`,`alert`, `info`, `warn` and `error`) high-enough to see you logs. **mitmproxy**'s default level of verbosity is `info`.


## Using scripts for apps-debugging

Now that we have seen the main events and objects structure, let's see a few concrete examples of scripts you can use for apps debugging.

### Redirecting requests to another environment

Our first case is pretty common : you have an app that gets data from an API. The next version of your API is available on your **preprod** environment and you would like to test it using the store build of your app.

Of course, the store build of your app is configured to send requests to your **prod** env and no debug setting is accessible.

Here we need a script to intercept requests to the **prod** env and re-route them to the **preprod** env :

```python
from mitmproxy import http
from mitmproxy import ctx

class Redirect:
    redirect_rules = {
        "api.domain.com": "api-preprod.domain.com" # replace `api.domain.com` with `api-preprod.domain.com` in urls
    }

    def load(self, loader):
        ctx.options.http2 = False # HTTP2 won't let you update the url

    def request(self, flow: http.HTTPFlow) -> None:
        for init_domain, new_domain in self.redirect_rules.items():
            if (init_domain in flow.request.pretty_url):
                flow.request.url = flow.request.pretty_url.replace(init_domain, new_domain)

addons = [Redirect()]
```

About this example : 
- you can add multiple redirect rules in the `redirect_rules` object
- we use a simple "replace" function to update the url so you can replace the domain or any other part of the string
- since you cannot change requests url when using HTTP/2, we force the use of HTTP on launch

### Use a special user-agent for testing

Now imagine that you are debugging your API and that you need to easily find the logs produced by your device among all production logs.

The simplest way to achieve this would be to use a custom **user-agent**. Fortunately, doing this is straightforward with **mitmproxy** :

```python
from mitmproxy import http
from mitmproxy import ctx

class ChangeUserAgent:
    user_agent = "my-custom-user-agent"

    def request(self, flow: http.HTTPFlow) -> None:
        flow.request.headers["user-agent"] = self.user_agent

addons = [ChangeUserAgent()]
```

About this example : 
- using the `flow.request.headers` array you can update any request header (`authorization`, `accept-language`, ...)

### Create an editable cache for API responses

For this last example, imagine you need to test how your app behaves when it receives a specific response after calling a specific endpoint.

Wouldn't it be great to be able to save requests locally in order to edit them manually ? That is exactly what the following script allows you to do.

The code from this script is far from being perfect, the idea here is to show you how powerful **mitmproxy** scripts can be.

```python
from mitmproxy import http
from mitmproxy import ctx
import os

class EditableCache:
    url_filter = "www.only-redirect-requests-from-this-domain.com"
    path = "/Full/path/to/cache/folder"

    def response(self, flow: http.HTTPFlow) -> None:
        if (self.url_filter in flow.request.pretty_url):
            fullpath = self.path + "/" + self.clean_url(flow.request.url)
            if not os.path.isfile(fullpath):
                if not os.path.exists(os.path.dirname(fullpath)):
                    os.makedirs(os.path.dirname(fullpath))
                file = open(fullpath, "w")
                file.write(flow.response.text)
                file.close()

    def request(self, flow: http.HTTPFlow) -> None:
        if (self.url_filter in flow.request.pretty_url):
            fullpath = self.path + "/" + self.clean_url(flow.request.url)
            if os.path.isfile(fullpath):
                with open(fullpath, 'r') as cache_file:
                    cache_content = cache_file.read()
                flow.response = http.HTTPResponse.make(200,cache_content)

    def clean_url(self, url):
        filename = url
        filename = filename.replace("https://", "")
        filename = filename.replace("?", "_")
        filename = filename.replace(",", "-")
        return filename

addons = [EditableCache()]
```

About this example : 
- the `url_filter` param allows you to only save some of the requests that goes through **mitmproxy**
- the `path` param is the local folder where you would like to save the responses

## Other posts about mitmproxy

If you want to learn more about **mitmproxy**, don't hesitate to check my other posts about this topic : 

- [How to inspect network traffic using mitmproxy]({% post_url mitmproxy/2020-04-10-mitmproxy %})
- [Tips to get started with mitmproxy]({% post_url 2020-04-22-mitmproxy-tips %})
