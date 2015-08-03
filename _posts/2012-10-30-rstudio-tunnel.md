---
layout: post
title: Rstudio server through SSH tunnel
tags: R
comments: true
---

Our Rstudio server instance runs on a server which is not directly connected to the internet at large.  We can connect to it through a ssh tunnel to an intermediate server.  This works fine for the command line, but to access the Rstudio instance we need to be able to connect using our browser.  This can be accomplished easily on linux/OSX by the following ssh command.

Assume the tunnel server is tunnel, remote is remote, rstudio port is 8787


{% highlight bash %}
ssh -f -N -L 1234:remote:8787 tunnel
{% endhighlight %}

The equivalent putty settings are in the following screenshot, assuming you are connecting to the tunnel server through putty

![putty](https://dl.dropbox.com/u/16233623/figure/rstudio-remote-putty.png)

You can then connect to your rstudio session from http://localhost:1234
