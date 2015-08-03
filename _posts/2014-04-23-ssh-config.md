---
layout: post
title: Responsive Remote Completion
tags: R
comments: true
---

I have been using remote completion from remote boxes for a long time, as it is
available in both zsh and bash.  However because establishing an SSH connection
is slow, the completion is not terribly useful, you spend more time waiting for
it to complete than it saves in typing.  However if you set up a few SSH
options to keep a master connection alive and for subsequent SSH connections to
use the master connection it then is very snappy.

First you need to set up password-less authentication, the easiest way is to use ssh-copy-id.


{% highlight bash %}
ssh-copy-id remote-server
{% endhighlight %}

then make a `controlmasters` directory in your `.ssh` directory, which will store the controlmaster session information.


{% highlight bash %}
mkdir -p ~/.ssh/controlmasters/
{% endhighlight %}

Lastly add the following lines to your `.ssh/config` file.
{% highlight text %}
Host *
  ControlPath ~/.ssh/controlmasters/%r@%h:%p
  ControlMaster auto
  ControlPersist yes
{% endhighlight %}

Then use your now snappy ssh completions!
