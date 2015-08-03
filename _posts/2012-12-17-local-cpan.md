---
layout: post
title: Setting up a local cpan using cpanminus without root access
tags: R
comments: true
---

When asked why colleagues do not use perl modules in their work, often the
response is that they do not know how to install them without having root
access to the server they are working on. Cpan can be configured to install to
a different base directory, however this requires a number of options to be set
correctly, and can be a pain to get set up.

However using [cpan minus](http://search.cpan.org/dist/App-cpanminus/lib/App/cpanminus.pm) and the [local::lib](http://search.cpan.org/dist/local-lib/lib/local/lib.pm) module makes this process as
painless as running three simple commands, easy enough to set up for just about
anyone.  Note that I turn off testing in the following commands, which
I encourage you to do as well, I have found there are some false positive
failures, and it will save time as well.

First you have to download cpanminus and install it and the local::lib module,
change /foo to the directory you would like to install the modules to


{% highlight bash %}
wget -O- http://cpanmin.us | perl - -l /foo App::cpanminus local::lib --notest
{% endhighlight %}

Then use the local::lib package to set up the required environment variables to
point to your new module path for the current login session


{% highlight bash %}
eval $(perl -I /foo/lib/perl5 -Mlocal::lib=/foo)
{% endhighlight %}

Finally add that command to a login script for your shell so it will be run
automatically when you login, i.e. (.profile, .bash_profile, .zshenv) ect.


{% highlight bash %}
echo 'eval $(perl -I /foo/lib/perl5 -Mlocal::lib=/foo)' >> .profile
{% endhighlight %}

I also like to set a default --notest, so I don't have to test every module
I install


{% highlight bash %}
echo export PERL_CPANM_OPT="--notest" >> .profile
{% endhighlight %}

Then you can then install a module in the correct directory , e.g. the Statistics::Descriptive package, with


{% highlight bash %}
cpanm Statistics::Descriptive
{% endhighlight %}

It doesn't get much easier than that!
