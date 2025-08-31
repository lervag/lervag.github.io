---
title: 'The tools that I love: Tailscale'
author: Karl Yngve Lervåg
date: 2025-08-31T21:30:00.000Z
---
> In this series, I will briefly talk about tools that I love and that I use often.
> I will keep things brief, as there is already a lot of good content that goes into more depth.

I often have to traverse networks with e.g. SSH to work on something on some server or on a different computer.
This could be simply reaching a computer at work from a home computer.
To do this, I would traditionally need to use a VPN service or establish a reverse ssh tunnel from my work computer.
Or something similar.
Since 2022, I've been using [Tailscale](https://tailscale.com/).
Tailscale is like a simple VPN service[^1] that makes it easy to achieve secure connections between computers in my private network.
For instance, if I install and configure Tailscale both on a server I host somewhere and on my home computer, then I can immediately `ssh myserver` from my home computer.
It just works, almost like magic[^2]:

{{< figure
  src="/static/tailscale-1.png"
  alt="tailscale status"
  caption="Easily see hosts and their status with `tailscale status` - here I've used `grep` to filter out hosts that were not purely mine. Yes, I am naming my hosts after composers 🎼 🎵."
>}}

{{< figure
  src="/static/tailscale-3.png"
  alt="ping tallis"
  caption="Pinging shows that network traffic is passing through the tailnet."
>}}

{{< figure
  src="/static/tailscale-2.png"
  alt="ssh tallis"
  caption="`ssh` just works<sup>™️</sup>!"
>}}

Now, Tailscale _is_ a commercial product[^3].
However, everything in Tailscale is open source, except the GUI clients for Windows and MacOS and the control server.
One may use [Headscale](https://github.com/juanfont/headscale), which is an open source alternative to the Tailscale control server.
I might consider running my own control server in the future, but for now, I'm happy to rely on the free service.
The Tailscale free tier for personal usage is very generous and provides all the features I need.

Some of the features I've found useful in addition to the general networking capabilities:

- [Tailscale SSH](https://tailscale.com/kb/1193/tailscale-ssh)
  - I've set up Tailscale on a test server for one of our applications at work and enabled Tailscale SSH for that server.
    This allows me (and other specified colleagues) to connect to that server without creating and sharing keys - the authentication is entirely done through the Tailscale service.
  - I can also imagine that this may be useful to e.g. connect to a server from my phone, where it is harder to create ssh keys.
- [Tailscale Funnel](https://tailscale.com/kb/1223/funnel) and [Tailscale Serve](https://tailscale.com/kb/1312/serve)
  - Funnel and Serve lets me route traffic from my computer either to the broader internet (Funnel) or to the limited private network (Serve).
  - This makes it essentially very easy to provide access to something I'm working on/developing locally on my computer to anyone.

Tailscale has also been very good at providing top notch documentation.
The [Tailscale Quickstart](https://tailscale.com/kb/1017/install) is a very good place to begin.
I also found the recent blog post [How I use Tailscale](https://chameth.com/how-i-use-tailscale/) by Chris Smith (2025-06-25) interesting.
You may too!

[^1]: It uses [Wireguard](https://www.wireguard.com/quickstart/) behind the scenes, if I've understood correctly.

[^2]: Clearly, it's not magic - David Anderson at Tailscale wrote a very interesting blog post about how Tailscale works and how it can provide direct connections across NATs: [How NAT traversal works](https://tailscale.com/blog/how-nat-traversal-works/).

[^3]:
    I don't know anyone at Tailscale and I'm not being paid by anyone to write this post.
    I just like the product and find it very useful!

