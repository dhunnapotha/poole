---
layout: post
title: MySQL Cheatsheet
---

__Creating a user__

{% highlight bash %}
CREATE USER 'jeffrey'@'localhost' IDENTIFIED BY 'mypass';
{% endhighlight %}

__Grant permissions on a db to a user__

{% highlight bash %}
GRANT ALL ON mydb.* TO 'jeffrey'@'localhost';
{% endhighlight %}

