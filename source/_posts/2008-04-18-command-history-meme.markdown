---
layout: post
---
Why not?  [Everyone else](http://blog.codefront.net/2008/04/18/command-history-meme) seems to be.  

{% highlight bash %}
history | awk '{a[$2]++}END{for(i in a){print a[i] " " i}}' | sort -rn | head
{% endhighlight %}

{% highlight text %}
125 git
66 gs
64 rake
56 cd
45 cap
33 ls
18 g
16 ./script/server
9 mv
8 svn
{% endhighlight %}

<code>g</code> is an alias for \`git\`.  <code>gs</code> is an alias for \`git status\`.  I like Chu's alias of <code>ss</code> for \`script/server\`.  I think I'll do that.



