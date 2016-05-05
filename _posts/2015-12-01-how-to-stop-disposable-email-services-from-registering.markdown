---
layout: archive
author_profile: true
title:  "How to stop disposable email services such as mailinator from registering to your website"
date:   2015-12-01
---

It is very easy, simply add the gem "valid_email2"

### Installation
Add this line to your application's Gemfile:
gem "valid_email2"

And then execute:
$ bundle

### Usage
Use with ActiveModel

{% highlight ruby %}
class User < ActiveRecord::Base
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable
  validates :email, email: { disposable: true }
{% endhighlight %}

More options:
https://github.com/lisinge/valid_email2
