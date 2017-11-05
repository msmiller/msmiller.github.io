---
title: A Strategy For Configs And Defaults In Rails
---

There's a plethora of configurations and default settings that are needed in a Rails App. Some examples include:

- Defining what UI icons to use for objects across the system.
- Defaults for selectors in forms.
- Feature enables.
- Instance settings (i.e. a web server is different than a background server).
- Install settings in the case where your system is being licensed out to another user.
- Default values for models.

And of course, this all has to get mapped across the various deploy environments. Sure, you can put some of these settings in the models themselves, but unless you do it consistently you'll find yourself hunting for these settings. And, yes, you could put these in initializers - but they're really not initializers, are they? Complicating this is that somethings you want your settings to be calculated off of some default. That is, sometimes what you need to provide the system is derived from a setting, not just the raw settings.

The approach described here uses three Gems which work well together to provide a very flexible and powerful strategy for managing all these settings.

### dotenv-rails

[https://github.com/bkeepers/dotenv](https://github.com/bkeepers/dotenv)

So let's "start at the bottom" as it were. **Dotenv** is a nice little Gem which automatically loads environment files before your server(s) start up. This mirrors the configuration variables that Heroku uses, so it's a nice way to keep consistent mechanisms for these settings.

```
- root
  - .env.development
  - .env.test
  - .env.foo
```

A dotenv file is like any other shell file. You simply put your environment variables in it. Note that I like to add an environment variable for `INSTANCE` so that if I have different deployments, so maybe the configuration of a Rescue server needs different settings, I'm covered.

```
export S3_BUCKET=YOURS3BUCKET
export SECRET_KEY=YOURSECRETKEYGOESHERE
export INSTANCE=staging
```

To load it using the standard files (these are described on the Github page), all you need to add is the following to your `.../config/environments/` file:

```
require 'dotenv/load'
```

You use the settings in the usual way:

```
config.fog_directory = ENV['S3_BUCKET']
```

So now you have global settings, environments, and credentials handled and in a way that will be consistent with your Heroku hosting. If you use standard Linux server, you can of course continue to use Dotenv to load `.env.production` and it all works great.

_**NOTE:** You don't want to check these files into your version control since they'll usually have credentials that you don't want being public._

### settingslogic

[https://github.com/binarylogic/settingslogic](https://github.com/binarylogic/settingslogic)

SettingsLogic is a really slick and useful Gem which loads a set up configuration data from a YAML file and then provides it as a model. This means that the model can also massage the data as needed - for instance, if a configuration hash is to be provided    to a form SELECT in a specific way.

The files you'll add, in addition to the Gem itself will be something like the following. I like to split out the SettingsLogic classes and settings files from everything else so things don't get confused.

```
- root
  - app
    - models
      - settingslogic
      - defaults.rb
      - facade.rb
      - instance.rb
    - config
      - settingslogic
        - common
          - defaults.yml
            - dev
              - instance.yml
              - facade.yml
            - test
            - foo
```

I like to use the `facade` settings for defining things like what icon to use for different things in the system, as well as branding attributes and the like. This way everything that's related to the look and feel of the App is more or less in one place. 

So, a SettingsLogic model looks something like this:

```ruby
class Settingslogic::Defaults < Settingslogic
  source "#{Rails.root}/config/settingslogic/common/defaults.yml"
  namespace Rails.env
end
```

And if your YAML file looks like:

```yaml
defaults: &defaults
  users:
    foo:             bar
    ack:             oop

development:
  <<: *defaults

test:
  <<: *defaults

production:
    <<: *defaults
```

... then you would be able to get at the settings as ...

```ruby
Settingslogic.defaults.users.foo # => 'bar'
Settingslogic.defaults.users.ack # => 'oop'
```

Note that SettingsLogic already handles development, test, and production environments. However, when you're using Heroku your "staging" environment is often configured as "production". This is where the `INSTANCE` setting from your Dotenv variables comes into play.

And since SettingsLogic provides configuration as a model, you can add in functions to make it more helpful - and to save having these little utility functions spread all over the rest of your code.

```ruby
class Settingslogic::Defaults < Settingslogic
  source "#{Rails.root}/config/settingslogic/common/defaults.yml"
  namespace Rails.env
  
  def is_foo_and_not_ack?
    (foo && !ack)
  end
end
```

And if you need to use a hash from the YAML to drive some of the UI, it'd look like this:

```ruby
<%= f.select  :some_selector,
              Settingslogic::Defines.some_model.some_selector_options %>
    
```

### default_value_for

[https://github.com/FooBarWidget/default_value_for](https://github.com/FooBarWidget/default_value_for)

The DefaultValueFor Gem adds the ability to set defaults to models up in the declarations section. These then get executed when a new object is created. It makes your default-setting much more readable. An example would be:

```ruby
class User < ActiveRecord::Base
  default_value_for :name, "(no name)"
  default_value_for :last_seen do
    Time.now
  end
end

u = User.new
u.name       # => "(no name)"
u.last_seen  # => Mon Sep 22 17:28:38 +0200 2008
```

So, now what we can do is combine this with what we set up with SettingsLogic:

```ruby
class User < ActiveRecord::Base
  default_value_for :foo, SettingsLogic::Defaults.user.foo
  default_value_for :ack, SettingsLogic::Defaults.user.ack
end
```

What's nice about this delegation of responsibility is that the settings of a default is handled in a very readable way. And the place where the settings are defined is handled in a very consistent and easy-to-maintain way. You'll know right where to look for a default if you need to change it. And if the default is dependent on environment or instance configurations, your actual Ruby Model code is completely isolated from that.

### Wrapping It All Up

So, to sum it up, we have three Gems which provide the following capabilities:

- `Dotenv` manages settings for credentials and instances across environments.
- `SettingsLogic` manages YAML-driven settings and default values across environments, and provides a place to put utility functions that relate to these settings.
- `DefaultValueFor` provides an easy-to-read mechanism for setting Model defaults that can be driven off the SettingsLogic models.

Your Model code is insulated from trying to figure out what the default could be. Your View code has one central resource group to reference for icons, selector options, and so on. And if you need to install an instance on a customer server you can quickly and easily spin up configurations for that without having to wade through a lot of code.

There are probably more elegant or fancy ways to accomplish what this does. But I tend to err on the side of ease of maintenance over complex functionality. I use the technique on multiple projects and any time I need to make configuration changes it's always a quick and easy matter. The gotcha with complex configuration Gems is that you don't use them that often. Your core settings don't usually change frequently. So if you need to make "just a quick config change" for a project or client, you don't want to burn half an hour trying to remember *how* to make the change because the fancy Gem you picked has a unique syntax or way or handing off settings. YAML is YAML is YAML ... it's easy to work with and easy to read.