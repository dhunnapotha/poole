---
layout: post
title: Snippet to halt the filter chain
---

returning false from a before_action filter doesn’t stop the filter chain. We should either render or redirect to prevent further filters in the chain to be executed.

Below is a small snippet which shows how to stop the filter chain and return a 401 status code

{% highlight ruby %}
render :nothing => true, :status => 401
{% endhighlight %}