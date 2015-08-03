---
layout: post
title: Commandline Java applications from symlinks
tags: R
comments: true
---

I dislike java commandline tools for a number of reasons, including their typical
extreme verbosity in arguments and lack of one letter substitutes.  Another
reason is that typically they do not have a simple shortcut which can be used 
to run them, you have to do something ugly like


{% highlight bash %}
java -jar lib/blah/blue/file.jar other_arguments
{% endhighlight %}

Even if the developer is nice enough to include a wrapping script they often 
assume you will be running the program from the directory the java program 
resides in.  There is no way to run the program from your working directory.

However if java developers would simply use this idiom in their wrapping 
scripts people could simply make a symlink to the wrapper and put it in their 
PATH, and everything would work nicely.


{% highlight bash %}
java -jar `dirname "$(readlink -f $0)"`/lib/blah/blue/file.jar other_arguments
{% endhighlight %}
