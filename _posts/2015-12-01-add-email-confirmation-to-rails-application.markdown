---
layout: post
title:  "Add email confirmation to an already running rails application:"
date:   2015-12-07
---

To add email confirmation to an already running rails application:
1. Add devise :confirmable to your models/user.rb
{% highlight ruby %}
devise :registerable, :confirmable
{% endhighlight %}

2. Add a new migration
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

3. Just in case you missed it, fix the mailer sender.
#config/initializers/devise.rb
{% highlight ruby %}
config.mailer_sender = 'hello@myappconverter.com'
{% endhighlight %}
