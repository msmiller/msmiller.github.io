---
title: Core Rails Stack
---

Setting up a new Rails project requires first putting in place all the core Gems and plugins you like to use. After years
of performing this task, I've settled on a set for Rails 5 that "just works" for me. One of the main goals is using Gems
which reduce the bloat of Models and Controllers, and which generally make life easier.


## Cells

## active_interaction

Active Interaction is a way to split out business logic from your Models and Controllers. Any time I have code
that's going to work on more than one Model class, or where the operation will take more than 20 lines, I like
to spin up an ActiveInteraction so that the logic is isolated and doesn't clog up my Models and Controllers.
ActiveInteractions have validation so this is also another firewall against passing bad data into the bowels of
your system

```ruby
#### define the ActiveInteraction ...

require 'active_interaction'

class Square < ActiveInteraction::Base
  float :x

  def execute
    x**2
  end
end

#### using the ActiveInteraction ...

outcome = Square.run(x: 'two point one')
outcome.valid?
# => nil
outcome.errors.messages
# => {:x=>["is not a valid float"]}

outcome = Square.run(x: 2.1)
outcome.valid?
# => true
outcome.result
# => 4.41
```

What you can also do is organize your ActiveInteractions into subfolders that either relate to
Models or abstract concepts (like 'send_emails').

```
# .../config/application.rb
config.autoload_paths += Dir.glob("#{config.root}/app/interactions/*")
```

**Get It:** [draper on Github](https://github.com/orgsync/active_interaction)

**Learn More:** [http://devblog.orgsync.com/active_interaction/](http://devblog.orgsync.com/active_interaction/)

## draper

Draper lets you move all your view-centric methods out of Models to keep them smaller and lighter. Why do you want this
when Rails already provides Helpers? One reason is that Helpers are more or less bound to ActionView. So if you're
kicking data out over JSON, they may not be as helpful. It also allows you to make your Rails app more of an object
broker in that the back-end has control over representation without getting page rendering into the mix.

```ruby
# Decorator example
class ArticleDecorator < Draper::Decorator
  delegate_all

  def publication_status
    if published?
      "Published at #{published_at}"
    else
      "Unpublished"
    end
  end

  def published_at
    object.published_at.strftime("%A, %B %e")
  end
end
```

```ruby
# Using a decorator:
@articles = Article.popular.decorate
```

**Get It:** [draper on Github](https://github.com/drapergem/draper)

## Pundit

## devise

Odds are whatever you're coding will require users to create accounts and log in. Devise is pretty much the standard
Gem for doing this.

## Rolify

## semantic-ui-sass

Most people seem to have gravitated to either the Bootstrap or Foundation UI frameworks. These are both great,
mature products which take a "mobile first" perspective. The problem I kept having is getting them to work
with Rails. There were frequent dependency clashes with SASS and Compass, not to mention issues with the
Javascript components and Turbolinks.

Semantic UI is very stable, looks great, and installs without issue. It's not a "one-line drop-in" like the
others. You need to wire up JavaScript for various classes to work. The other toolkits provide an "all-in-one"
JavaScript function that runs all the components ... which is "easier" but also a lot less transparent and
prone to conflicts.

**Get It:** [semantic-ui-sass on Github](https://github.com/doabit/semantic-ui-sass)

**Learn More:** [http://semantic-ui.com/](http://semantic-ui.com/)

## settingslogic

SettingsLogic is like wrapping a Model around a loaded YAML configuration file. There are so many cases where you have data that's
either static or changes very seldomly - but you want more than just a table of values loaded from YAML. You want to be able
to condition the settings for use in the system, or make decisions before returning a value. This is where SettingsLogic
is so handy. I use it for things like billing plans (which rarely change) or branding settings (where I want to condition the
setting value for rendering).

##### 1. Define the Model

```ruby
class Settings < Settingslogic
  source "#{Rails.root}/config/application.yml"
  namespace Rails.env
end
```

##### 2. Define the Settings

```ruby
# config/application.yml
defaults: &defaults
  cool:
    saweet: nested settings
  neat_setting: 24
  awesome_setting: <%= "Did you know 5 + 5 = #{5 + 5}?" %>

development:
  <<: *defaults
  neat_setting: 800

test:
  <<: *defaults

production:
  <<: *defaults
```

##### 3. Use the Model

```ruby
>> Rails.env
=> "development"

>> Settings.cool
=> "#<Settingslogic::Settings ... >"

>> Settings.cool.saweet
=> "nested settings"

>> Settings.neat_setting
=> 800

>> Settings.awesome_setting
=> "Did you know 5 + 5 = 10?"
```

**Get It:** [settingslogic on Github](https://github.com/binarylogic/settingslogic)

## Kanimari

## seedbank

Seed Bank lets you break your database seeds into multiple files. This lets you do things like:

```bash
rake db:seed                    # Load the seed data from db/seeds.rb, db/seeds/*.seeds.rb and db/seeds/ENVIRONMENT/*.seeds.rb. ENVIRONMENT is the current environment in Rails.env.
rake db:seed:bar                # Load the seed data from db/seeds/bar.seeds.rb
rake db:seed:common             # Load the seed data from db/seeds.rb and db/seeds/*.seeds.rb.
rake db:seed:development        # Load the seed data from db/seeds.rb, db/seeds/*.seeds.rb and db/seeds/development/*.seeds.rb.
rake db:seed:development:users  # Load the seed data from db/seeds/development/users.seeds.rb
rake db:seed:original           # Load the seed data from db/seeds.rb
```

**Get It:** [seedbank on Github](https://github.com/james2m/seedbank)

## awesome_print

I rely on print statements a lot when bringing up new code or debugging. It's just faster for my style of working. AwesomePrint will
print out the object in question all nicely formatted so you can see what's in there without going blind trying to visually
parse a raw `object.inspect` output.

```ruby
rails> ap Account
class Account < ActiveRecord::Base {
                 :id => :integer,
            :user_id => :integer,
        :assigned_to => :integer,
               :name => :string,
             :access => :string,
            :website => :string,
    :toll_free_phone => :string,
              :phone => :string,
                :fax => :string,
         :deleted_at => :datetime,
         :created_at => :datetime,
         :updated_at => :datetime,
              :email => :string,
    :background_info => :string
}
```

**Get It:** [awesome_print on Github](https://github.com/awesome-print/awesome_print)

## ActsAsTaggabble

If your users will be organizing content in any way, you want tagging. [TBD]

## high_voltage

You're always going to need some static pages. Privacy Policy. About Us. And so on. High Voltage is an easy drop-in engine
to help get those pages rigged in a hurry. If you need something fancier - like ACL on some pages - it's easy enough to
paste in the example PagesController and tweak it to your liking.

**Get It:** [high_voltage on Github](https://github.com/thoughtbot/high_voltage)

## sendgrid

If your users will be logging in, that means you'll be sending account confirmations - so you need email. There's a few
choices for email-send providers, but I've found SendGrid to be the best. You can send via SMTP, API, or their "bulk send" API
(for when you're really pounding out the emails). Why not just use your server's built-in SMTP? Well, you can do that. But SendGrid
also provides click tracking, delivery confirmations, and email-open tracking. Plus it means you don't need to spend money
on a server just for sending emails.

**Get It:** [sendgrid on Github](https://github.com/stephenb/sendgrid)

**Learn More:** [http://sendgrid.com](http://sendgrid.com)

## paperclip
