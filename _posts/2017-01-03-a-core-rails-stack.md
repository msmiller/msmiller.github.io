---
title: Core Rails Stack
---

Setting up a new Rails project requires first putting in place all the core Gems and plugins you like to use. After years
of performing this task, I've settled on a set for Rails 5 that "just works" for me. One of the main goals is using Gems
which reduce the bloat of Models and Controllers, and which generally make life easier.


## Cells

## Active-Interactions

## Draper

## Pundit

## Devise

## Rolify

## Semantic-UI-SASS

## SettingsLogic

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

**Get it:** [seedbank on Github](https://github.com/james2m/seedbank)

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

**Get it:** [awesome_print on Github](https://github.com/awesome-print/awesome_print)

## ActsAsTaggabble

## high_voltage

You're always going to need some static pages. Privacy Policy. About Us. And so on. High Voltage is an easy drop-in engine
to help get those pages rigged in a hurry. If you need something fancier - like ACL on some pages - it's easy enough to
paste in the example PagesController and tweak it to your liking.

**Get it:** [high_voltage on Github](https://github.com/thoughtbot/high_voltage)
