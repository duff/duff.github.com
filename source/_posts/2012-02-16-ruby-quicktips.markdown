---
layout: post
---

I've been enjoying the [Ruby Quicktips](http://rubyquicktips.com/) blog.  Even if you've been using Ruby for years, I'm betting there's still things there you'll find useful.

[For example](http://rubyquicktips.com/post/2311239580/array-first-and-array-last-parameters), did you know you can pass a number parameter to Array#first and Array#last?

``` ruby
x = [1,2,3,4,5,6,7,8,9,10]

x.first 5
=> [1,2,3,4,5]

x.last 2
=> [9,10]
```

