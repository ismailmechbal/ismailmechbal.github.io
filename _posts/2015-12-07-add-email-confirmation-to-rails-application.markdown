---
layout: archive
author_profile: true
title:  "Add email confirmation to an already running rails application:"
date:   2015-12-07
---

To add email confirmation to an already running rails application:

### Add devise :confirmable to your models/user.rb
{% highlight ruby %}
devise :registerable, :confirmable
{% endhighlight %}

### Add a new migration
rails g migration add_confirmable_to_devise

Make sure the contents of the migration is:

{% highlight ruby %}
class AddConfirmableToDevise < ActiveRecord::Migration
  def change
    change_table(:users) do |t|
      t.string   :confirmation_token
      t.datetime :confirmed_at
      t.datetime :confirmation_sent_at
      t.string   :unconfirmed_email
    end
    add_index :users, :confirmation_token, :unique => true
  end
end
{% endhighlight %}

Then run your migration with rake db:migrate.

### If you get the following error:
{% highlight ruby %}
ActionView::Template::Error
Missing host to link to! Please provide the :host parameter, set default_url_options[:host], or set :only_path to true
{% endhighlight %}

Just add the following line of code to your development, test & production environments file:
config/environments/development.rb
{% highlight ruby %}
config.action_mailer.default_url_options = { :host => 'localhost:3000' }
{% endhighlight %}


### Just in case you missed it, fix the mailer sender.
{% highlight ruby %}
#config/initializers/devise.rb
config.mailer_sender = 'hi@youremail.com'
{% endhighlight %}
