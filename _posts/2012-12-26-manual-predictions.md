---
layout: post
title: Plotting manual fitted model predictions using ggplot
tags: R
comments: true
---

ggplot provides convenient smoothing functions for fitting models to data with 
the built in geom_smooth and stat_smooth methods.

{% highlight r %}
library(ggplot2)
(points = ggplot(data=mtcars, aes(x=hp,y=mpg)) + geom_point())
{% endhighlight %}

<img src="{{ site.baseurl }}/figure/manual_predictions_ex1-1.png" title="plot of chunk manual_predictions_ex1" alt="plot of chunk manual_predictions_ex1"  />

{% highlight r %}
(points_smoothed = points + geom_smooth(method="lm", se=F))
{% endhighlight %}

<img src="{{ site.baseurl }}/figure/manual_predictions_ex1-2.png" title="plot of chunk manual_predictions_ex1" alt="plot of chunk manual_predictions_ex1"  />

{% highlight r %}
(one_facet <- points_smoothed + facet_wrap(~cyl))
{% endhighlight %}

<img src="{{ site.baseurl }}/figure/manual_predictions_ex1-3.png" title="plot of chunk manual_predictions_ex1" alt="plot of chunk manual_predictions_ex1"  />
When you are faceting data, either spatially or by color/linetype/shape doing the subsetting and model fitting manually can be somewhat daunting.

{% highlight r %}
(two_facet = points_smoothed + facet_grid(cyl~gear))
{% endhighlight %}

<img src="{{ site.baseurl }}/figure/manual_predicitons_ex2-1.png" title="plot of chunk manual_predicitons_ex2" alt="plot of chunk manual_predicitons_ex2"  />
However once you understand the process, and are familiar with the plyr library of 
functions it is actually very straightforward.
## One facet ##

{% highlight r %}
library(plyr)
models = dlply(mtcars, .(cyl), function(df) lm(mpg ~ hp,data=df))
predictions = ldply(models, function(mod) {
  grid = expand.grid(hp=sort(unique(mod$model$hp)))
  grid$pred = predict(mod,newdata=grid)
  grid
})
one_facet + geom_line(data=predictions,aes(y=pred),linetype="dashed",size=2)
{% endhighlight %}

<img src="{{ site.baseurl }}/figure/manual_predictions_one-1.png" title="plot of chunk manual_predictions_one" alt="plot of chunk manual_predictions_one"  />
The only change for two facets is how you break up the models
## Two facets ##

{% highlight r %}
models = dlply(mtcars, .(cyl, gear), function(df) lm(mpg ~ hp,data=df))
predictions = ldply(models, function(mod) {
  grid = expand.grid(hp=sort(unique(mod$model$hp)))
  grid$pred = predict(mod,newdata=grid)
  grid
})
two_facet + geom_line(data=predictions,aes(y=pred),linetype="dashed",size=2)
{% endhighlight %}

<img src="{{ site.baseurl }}/figure/manual_predictions_two-1.png" title="plot of chunk manual_predictions_two" alt="plot of chunk manual_predictions_two"  />

If you want to perform predictions across the full range of data you can use
expand.grid with the full dataset rather than just the subset, this is
analogous to the fullrange option in stat_smooth

{% highlight r %}
grid = expand.grid(hp=sort(unique(mtcars$hp)))
models = dlply(mtcars, .(cyl, gear), function(df) lm(mpg ~ hp,data=df))
predictions = ldply(models, function(mod) {
  grid$pred = predict(mod,newdata=grid)
  grid
})
points +
  stat_smooth(fullrange=T,se=F,method="lm") +
  facet_grid(cyl~gear) +
  geom_line(data=predictions, aes(y=pred), linetype="dashed", size=2)
{% endhighlight %}

<img src="{{ site.baseurl }}/figure/manual_predictions_full-1.png" title="plot of chunk manual_predictions_full" alt="plot of chunk manual_predictions_full"  />

So you can see that plotting manual predictions is actually very
straightforward, and this can be a powerful technique in exploratory data
analysis.
