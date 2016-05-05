---
layout: archive
author_profile: true
title:  "upgrade Passenger + Nginx on Ubuntu"
date:   2015-12-09
---

This week Passenger announced a security update and i would like to note how easy it is to upgrade Passenger + Nginx:

If Passenger was installed through the Phusion Passenger APT repository, then you can upgrade Passenger along with Nginx through APT using the following command:

{% highlight ruby %}
sudo apt-get update
sudo apt-get upgrade
{% endhighlight %}

That's all
