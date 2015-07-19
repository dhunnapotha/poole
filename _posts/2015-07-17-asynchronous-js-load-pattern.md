---
layout: post
title: Pattern to load JS libraries asynchronously
---

Sometimes, you want the external js files to be loaded asynchronously such that their loading time doesn’t impact the user experience of your site. For example, when you want to integrate google-analytics.

Which means, you will have to wait for the js file to be loaded to use any of the functions provided in the js library. There is a nice pattern which lets you bypass this.

Below is the snippet Google Analytics give you to insert in your pages.


{% highlight javascript %}
//https://developers.google.com/analytics/devguides/collection/analyticsjs/advanced
 
/**
 * Creates a temporary global ga object and loads analy  tics.js.
 * Paramenters o, a, and m are all used internally.  They could have been declared using 'var',
 * instead they are declared as parameters to save 4 bytes ('var ').
 *
 * @param {Window}      i The global context object.
 * @param {Document}    s The DOM document object.
 * @param {string}      o Must be 'script'.
 * @param {string}      g URL of the analytics.js script. Inherits protocol from page.
 * @param {string}      r Global name of analytics object.  Defaults to 'ga'.
 * @param {DOMElement?} a Async script tag.
 * @param {DOMElement?} m First script tag in document.
 */
(function(i, s, o, g, r, a, m){
  i['GoogleAnalyticsObject'] = r; // Acts as a pointer to support renaming.
 
  // Creates an initial ga() function.  The queued commands will be executed once analytics.js loads.
  i[r] = i[r] || function() {
    (i[r].q = i[r].q || []).push(arguments)
  },
 
  // Sets the time (as an integer) this tag was executed.  Used for timing hits.
  i[r].l = 1 * new Date();
 
  // Insert the script tag asynchronously.  Inserts above current tag to prevent blocking in
  // addition to using the async attribute.
  a = s.createElement(o),
  m = s.getElementsByTagName(o)[0];
  a.async = 1;
  a.src = g;
  m.parentNode.insertBefore(a, m)
})(window, document, 'script', '//www.google-analytics.com/analytics.js', 'ga');
 
ga('create', 'UA-XXXX-Y', 'auto'); // Creates the tracker with default parameters.
ga('send', 'pageview');            // Sends a pageview hit.
 
// The other part is in https://gist.github.com/dhunnapotha/5c46a93eeec8239a9e68
{% endhighlight %}



You should notice 3 important points here:

* The js file is loaded asynchronously with .sync = 1 and is being inserted into the DOM
* Until the file is loaded asynchronously, all the calls to ga (for eg: ga(create..)) will push into window[‘ga’].q
* The handler, in this case, ‘ga’ is being stored in window[‘GoogleAnalyticsObject’]


Once the file is loaded,

* We need to flush out all the entries pushed into window[‘ga’].q, which is fairly straightforward.
* And then, assign the loaded library to window[window[‘GoogleAnalyticsObject’]] such that all further calls to ga() will execute the functions defined in the library.

A sample snippet can look like this

{% highlight javascript %}

(function(){
  var handler = window['GoogleAnalyticsObject'] || 'ga';
  var queue;
  if(window[handler])
    queue = window[handler].q;
 
  queue = queue || [];
 
  while(ele = queue.shift()) {
    var args = Array.prototype.slice.call(ele);
    GoogleAnalytics.apply(GoogleAnalytics, args);
  }
  window[handler] = GoogleAnalytics;
})();

{% endhighlight %}
