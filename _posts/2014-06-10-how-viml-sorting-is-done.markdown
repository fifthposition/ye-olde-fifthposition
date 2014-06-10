---
layout: post
title: "How VimL number sorting is done"
date: 2014-06-10 13:32:00 -0500
comments: false
categories: ["VimL"]
---

For a chapter of [the upcoming book](http://fifthposition.org/viml-book-to-occur/) I needed to sort a `List` of `Number` items representing `String` lengths, and then to get the last item &#8212; which was to have been the largest &#8212; and use that purposes of greater VimLing. So of course I wrote something like

{% highlight vim %}
let listYo = [1, 74, 18, 9, 92]
call sort(listYo)

echo listYo[-1]
{% endhighlight %}

The result? `9`.

I printed the list out:

{% highlight vim %}
echo listYo

[1, 18, 74, 9, 92]
{% endhighlight %}

You are correct (assuming that you are smarter than I, and there are not many assumptions safer to make). Vim is sorting this `List` of `Number` items as a `List` of `Strings`. I need not even have been so mystified by this, as it&#8217;s even mentioned right there in [the docs](http://vimdoc.sourceforge.net/htmldoc/eval.html#sort()):

{% highlight text %}
Uses the string representation of each item to sort on.
{% endhighlight %}

On the Vim user mailing list, [Paul Isambert helpfully pointed out](https://groups.google.com/d/msg/vim_use/Rjc5XzuvMvY/CnamEwr6azAJ) how I am supposed to do this, also mentioned in the docs: define my own sorting function. Yes, yes. The function given there:

{% highlight vim %}
func MyCompare(i1, i2)
  return a:i1 == a:i2 ? 0 : a:i1 > a:i2 ? 1 : -1
endfunc
{% endhighlight %}

It is then called like so (again, from the docs):

let sortedlist = sort(mylist, "MyCompare")
let sortedlist = sort(mylist, "MyCompare")
{% endhighlight %}

[See ZyX&#8217;s message to this thread on the mailing list](https://groups.google.com/d/msg/vim_use/Rjc5XzuvMvY/mOQF6GIJjbkJ) for all I have heard on why `sort()` works this way. If, like me, you think the world would be a better place were we all to give up docs-reading and spend a lot more time Googling, I hope you find this helpful.


