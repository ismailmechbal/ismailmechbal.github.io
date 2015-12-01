---
layout: post
title:  "Using nginx together with Discourse"
date:   2015-12-01
---

Yesterday i tried to install Discourse in a server running nginx, i got two problems that were solved quickly just by googling:

## Error when starting the container:
Error response from daemon: Cannot start container 270d47cd8401c50f13e0a1e51428f5b9833a9480b53bc98735df55b425a70b6d: Error starting userland proxy: listen tcp 0.0.0.0:80: bind: address already in use

It was expected, because nginx runs on port 80.

in /var/docker/containers/app.yml there is an entry:

{% highlight ruby %}
expose:
  - "80:80"   # fwd host port 80   to container port 80 (http)
  - "2222:22" # fwd host port 2222 to container port 22 (ssh)
{% endhighlight %}

Change port 80 to 8080

{% highlight ruby %}
expose:
  - "8080:80"
  - "2222:22"
{% endhighlight %}

Then rebuild and restart the container

{% highlight ruby %}
cd /var/discourse
sudo ./launcher rebuild app # To apply the changes
sudo ./launcher start app
{% endhighlight %}

## Setting up http forwarding to direct to this new alternate port (8080)

Create a new virtual host file for Discourse and put in it:

{% highlight ruby %}
upstream discourse {
#fail_timeout is optional; I throw it in to see errors quickly
  server 127.0.0.1:8080 fail_timeout=5;
}
# configure the virtual host
server {
  # replace with your domain name
  server_name forum.myappconverter.com;
  location / {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    #pass to the upstream discourse server mentioned above
    proxy_pass http://discourse;
  }
}
{% endhighlight %}

Sources:
https://meta.discourse.org/t/using-nginx-alongside-the-docker-install/15282/4
http://mdworld.nl/blog/mypc/2014/11/16/discourse-ubuntu/
