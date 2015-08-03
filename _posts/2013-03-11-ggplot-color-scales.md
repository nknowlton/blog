---
layout: post
title: Setting ggplot2 default color scales
tags: R
comments: true
---

[ggplot2](http://www.ggplot2.org) is a very nice R plotting library, however some
people do not like the default color scales for plots.  You can explicitly set
the color scale for each one of your plots, but a better solution would be to
simply change the defaults.


{% highlight text %}
​Warning in rm(scale_colour_discrete): object 'scale_colour_discrete' not
found

{% endhighlight %}

{% highlight r %}
#default colors
qplot(data=iris, Petal.Length, Petal.Width, color=Species, size=I(4))
{% endhighlight %}

<img src="{{ site.baseurl }}/figure/ggplot_default_color_scales_Title-1.png" title="plot of chunk ggplot_default_color_scales_Title" alt="plot of chunk ggplot_default_color_scales_Title"  />

{% highlight r %}
#change default without arguments
scale_colour_discrete <- scale_colour_grey
last_plot()
{% endhighlight %}

<img src="{{ site.baseurl }}/figure/ggplot_default_color_scales_Title-2.png" title="plot of chunk ggplot_default_color_scales_Title" alt="plot of chunk ggplot_default_color_scales_Title"  />

{% highlight r %}
#change default with arguments
scale_colour_discrete <- function(...) scale_color_brewer(palette="Set1")
last_plot()
{% endhighlight %}

<img src="{{ site.baseurl }}/figure/ggplot_default_color_scales_Title-3.png" title="plot of chunk ggplot_default_color_scales_Title" alt="plot of chunk ggplot_default_color_scales_Title"  />
