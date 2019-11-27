---
title: Simple Blog System For Rails
excerpt: When creating new products, adding a simple blog to document concepts and changes is a frequent need.  What I ended up doing was create a flat-file blog system with Markdown-formatted posts and a YAML definition file for the index and configuration.
---

When creating new products, adding a simple blog to document concepts and changes is a frequent need. In the early stages you're making at most about two dozen posts a year, usually coincidental to a new release. So it'd take a couple years before one outgrew a 'simple' solution.

What I ended up doing was create a flat-file blog system with Markdown-formatted posts and a YAML definition file for the index and configuration. I went with the SettingsLogic gem (<https://github.com/binarylogic/settingslogic>) for the configuration side and the RedCarpet gem (<https://github.com/vmg/redcarpet>) for the rendering.

The file structure is as follows:

```yaml
RAILS_ROOT/
  app/
    models/
      blog,rb
  static/
    index.yml
    posts/
      first_post.md
      second_post.md
  public/
    blog/
      splash_image.jpg
      social_image.jpg
      post_image_1.png
      post_image_2.png
```

Very easy to manage.

The `Blog` model loads the YAML index file and has some helper methods:

```ruby
# blog.rb

# https://github.com/settingslogic/settingslogic

class Blog < Settingslogic
  source "#{Rails.root}/static/blog/index.yml"
  namespace Rails.env
  suppress_errors Rails.env.production?
  load!

  def select_post(slug)
    posts.find {|p| p['slug'] == slug }
  end

  def splash_image
    "/blog/#{splash}"
  end

  def meta_splash_image
    "/blog/#{meta_splash}"
  end

  def render_post(file)
    begin
      body = File.read("#{Rails.root}/static/blog/posts/#{file}")
      redcarpet_flags = {
          :safe_links_only => true,
          :autolink => true,
          :disable_indented_code_blocks => false,
          :fenced_code_blocks => true,
          :no_images => true,
          :filter_htm => true,
          :hard_wrap => true
        }
      markdown = Redcarpet::Markdown.new(Redcarpet::Render::HTML, redcarpet_flags)
      filtered_text = markdown.render(body)
      filtered_text
    rescue
      ""
    end
  end

end
```

As is the configuration file:

```yaml
# index.yml

# https://github.com/settingslogic/settingslogic

defaults: &defaults
  name: Abstretta Blog
  splash: splash.jpg
  meta_splash: social.jpg
  posts:
    - name: First Post
      date: 2018-09-01
      file: first_post.md
      slug: first_post
      tags: ""
      excerpt: The first post on the new Abstretta blogging system
    - name: "'Opportunity Knocks' Release"
      date: 2018-09-11
      file: opportunity_knocks.md
      slug: opportunity_knocks
      tags: ""
      excerpt: "Big new features in the latest release, including 'Opportunities'"

development:
  <<: *defaults

test:
  <<: *defaults

production:
  <<: *defaults

staging:
  <<: *defaults
```

Again, this is pretty easy stuff. The controller for just showing a post is also easy stuff:

```
# blog_controller.rb

class BlogController < ApplicationController

  skip_before_action :authenticate_user!

  def index
  end

  def show
    @post = Blog.select_post(params[:id])
    @body = Blog.render_post(@post['file'])
  end

  def list
  end
end
```

I haven't done the list-type pages yet, but by the time this goes live they'll be done. Note the `skip_before_action` ... this also implies I didn't need to write an admin system to manage the blog. Since I'm the only one posting, this is a good thing.
