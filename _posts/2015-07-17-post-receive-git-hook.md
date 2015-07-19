---
layout: post
title: Post receive (server side) git hook
---

On git repo hosting server, if an action has to be performed whenever someone pushes changes, we can place the script in post-receive git hook.

Below is a small snippet which updates the app directory with master branch whenever a change is received on the server.

{% highlight bash %}
#!/bin/sh
GIT_WORK_TREE=/home/gautam/projects/app git checkout -f
{% endhighlight %}