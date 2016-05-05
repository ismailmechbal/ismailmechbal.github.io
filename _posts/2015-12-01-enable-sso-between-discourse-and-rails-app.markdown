---
layout: archive
author_profile: true
title:  "Enable SSO between discourse and your rails app"
date:   2015-12-01
---

#### Put the reference implementation - https://github.com/discourse/discourse/blob/master/lib/single_sign_on.rb - in your #{Rails.root}/lib directory

#### Add this route to routes.rb

{% highlight ruby %}
get 'discourse/sso' => 'discourse_sso#sso'
{% endhighlight %}

#### Create the following controller discourse_sso_controller.rb and add the following into it:

{% highlight ruby %}
require 'single_sign_on'

class DiscourseSsoController < ApplicationController
  before_action :authenticate_user! # ensures user must login

  def sso
    secret = "MY_SECRET_STRING"
    sso = SingleSignOn.parse(request.query_string, secret)
    sso.email = current_user.email # from devise
    sso.name = current_user.full_name # this is a custom method on the User class
    sso.username = current_user.email # from devise
    sso.external_id = current_user.id # from devise
    sso.sso_secret = secret

    redirect_to sso.to_url("http://your_discource_server/session/sso_login")
  end
end
{% endhighlight %}

#### Set up the SSO config in discourse (admin/site_settings/category/login) to have the following

sso url: http://your_rails_server/discourse/sso
sso secret : what you set as MY_SECRET_STRING above

#### Disable other login types in discourse

#### You're done
