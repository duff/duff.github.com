---
layout: post
title: "Pimping Skitch"
date: 2012-10-23 10:29
---
[Skitch](http://skitch.com) was acquired by Evernote awhile back and they recently published their 2.0 upgrade.  The internets were abuzz with discontent about some
of the changes.  Personally, I'm fond of a number of the improvements.  The only "improvement" that's been a bit hurtful is that the uploading of images goes to the Evernote
server and it's not very cinchy to get the direct url of the image that was uploaded.

All I really want is the ability to hit a keyboard shortcut, take a quick screenshot, add a big fat arrow or two and maybe some text, hit another keyboard shortcut and know that the
clipboard now has the url of the uploaded image.  It's something I do a number of times a day.

I started with some hackery to use the evernote url but they're doing some funky things with redirects and permissions such that some people have been unable to
see the images.  I noticed my friend Brandon [was now using CloudApp](https://alpha.app.net/imathis/post/880318) so I thought I'd give that a shot.  Here's what I came
up with:

<!-- more -->
I'm still using Skitch to take the screenshot and edit the image. Then I trigger a [Keyboard Maestro](http://www.keyboardmaestro.com/main/) macro that's only
available when Skitch is the activated application.  Here's what it looks like:

<img src="/files/skitch_keyboard_maestro.png" width="480px" />

That one keystroke copies the skitch image to the clipboard, triggers the keyboard shortcut to send it to [CloudApp](http://getcloudapp.com), waits until the
clipboard contains the CloudApp url, and then runs a shell script to get the url of the image itself rather than a url to a page containing the image.  Getting the
real url of the image allows me to just paste the url into [Campfire](http://campfirenow.com) or [HipChat](https://www.hipchat.com) and have the image show up
inline.

Here's the little shell script:

``` ruby
#!/usr/bin/env ruby

def clipboard
  value = IO.popen('pbpaste', 'r+').read
  unless value.include?("http://cl.ly")
    puts "The url in the clipboard must start with http://cl.ly"
    exit 0
  end
  value
end

image_url = "#{clipboard}/content.png"

puts "Got the image!\n#{image_url}"
IO.popen('pbcopy', 'w').print image_url
```
The *puts* lines show up on my screen briefly because of this in the Keyboard Maestro macro:

<img src="/files/display_results_briefly.png" width="480px" />

After that one keystroke, my clipboard now has the url.  It looks something like this:

[http://cl.ly/image/2f2u1J2j2b2z/content.png](http://cl.ly/image/2f2u1J2j2b2z/content.png)
