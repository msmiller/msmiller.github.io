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

```
rake db:seed                    # Load the seed data from db/seeds.rb, db/seeds/*.seeds.rb and db/seeds/ENVIRONMENT/*.seeds.rb. ENVIRONMENT is the current environment in Rails.env.
rake db:seed:bar                # Load the seed data from db/seeds/bar.seeds.rb
rake db:seed:common             # Load the seed data from db/seeds.rb and db/seeds/*.seeds.rb.
rake db:seed:development        # Load the seed data from db/seeds.rb, db/seeds/*.seeds.rb and db/seeds/development/*.seeds.rb.
rake db:seed:development:users  # Load the seed data from db/seeds/development/users.seeds.rb
rake db:seed:original           # Load the seed data from db/seeds.rb
```

[Seedbank on Github](https://github.com/james2m/seedbank)

## AwesomePrint

## ActsAsTaggabble

## high_voltage

You're always going to need some static pages. Privacy Policy. About Us. And so on. High Voltage is an easy drop-in
to help get those pages rigged in a hurry.

#writing/blogs
