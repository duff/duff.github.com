---
layout: post
title: "Rails Plugin SVN messages Updated"
---

I created a rails plugin to help me keep clients up to date on the status of projects.  This post is an update to [the original]( /blog/2007/08/22/rails-plugin-svn-messages ) svn_messages plugin.

The plugin now uses capistrano 2 (Multistage).  I use it in 2 instances - whenever I deploy a site, and whenever I send a weekly status report.  For a deployment, this is what I type at the command line:

``` bash
cap staging svn:deployment_report
```

This gives me the following output:

``` text
Deployment to staging (Revision 34)
I deployed the latest.  It includes:

* Started adding user registration system. (domelia)
* Now translating validation errors and their attribute names. (bjones)
* Reorganized views a bit. (domelia)
* Now sending activation email when user signs up. (fsmith)
* Added support for "remember me" when logging in. (domelia)
```

I could have also typed:

``` bash
cap production svn:deployment_report
```


For a weekly status report, I type the following:

``` bash
cap svn:messages r=70 u=domelia
```

70 is the oldest subversion revision I care about.  domelia is me.  I specify this because I only care about the svn commits that I personally made.  This gives me the following output:

``` text
* Started adding user registration system.
* Reorganized views a bit.
* Added support for "remember me" when logging in.
```


I like the plugin because I can keep clients up to date and spend very little time doing it.

To install:

``` bash
svn export https://terralien.devguard.com/svn/projects/plugins/svn_messages vendor/plugins/svn_messages
```

If you'd like this deployment report to happen every time you deploy, you can add the folllowing to your deploy.rb:
``` ruby
before  "deploy:update_code", "svn:deployment_report"
```
