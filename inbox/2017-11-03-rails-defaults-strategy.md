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

I like to use the `facade` settings for defining things like what icon to use for different things in the system, as well as branding attributes and the like. This way everything that's related to the look and feel of the App is more or less in one place. Also, since SettingsLogic actually provides a model, it makes it possible to create helper-like functions to do things like condition settings for use in forms.

### default_value_for
