---
layout: post
title: Uglifying a file
---


When you want to uglify a single file instead of going through the rails assets precompilation process, you can use the below command

{% highlight ruby %}
Uglifier.compile(input_file_path)
{% endhighlight %}


If you want to spit out the uglified file into an output file, use the below

{% highlight ruby %}
File.write output_file_name, Uglifier.compile(File.read(input_file_path))
{% endhighlight %}
