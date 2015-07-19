---
layout: post
title: CORS and finding the origin details
---

When you allow access from different origins and if you would like to know from which origin the request is from, you can get that information in

{% highlight ruby %}
request.headers["origin"]
{% endhighlight %}

Once you get the origin url, you can get the host information (i.e., without http scheme and www) using the below snippet


{% highlight ruby %}
#http://stackoverflow.com/questions/6674230/how-would-you-parse-a-url-in-ruby-to-get-the-main-domain
 
# Only parses twice if url doesn't start with a scheme
def get_host_without_www(url)
  uri = URI.parse(url)
  uri = URI.parse("http://#{url}") if uri.scheme.nil?
  host = uri.host.downcase
  host.start_with?('www.') ? host[4..-1] : host
end
{% endhighlight %}