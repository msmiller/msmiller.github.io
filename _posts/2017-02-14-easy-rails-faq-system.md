---
title: Quick-and-Easy Rails Help/FAQ System
excerpt: A complete help/faq system in about 100 lines of code. There may be more elegant ways to accomplish this, but for quickly generating internal documentation this will get the job done. All your documentation is in easy to edit Markdown files and you can re-arrange things by simply moving things between folders on the desktop and then pushing the code back to git.
---

Any web application needs a help or FAQ section. There has to be documentation. At the same time, you as the developer don't want to spend a ton of time building this part of the code, or have to learn a whole new kind of syntax for your documents. Some FAQ gems rely on ActiveRecord but - seriously - how often does your documentation change? And when it changes, it's usually in relation to a code push, right? So what I prefer is a flat-file approach and Markdown formatted documents. That keeps things simple and easy to maintain.

#### The Files

The implementation is pretty simple. One controller, 3 views, and an initializer - and add a new entry to `config/routes.rb`. Everything else is static document files which can be easily modified and re-arranged as your system grows or changes.

```
- root
  - app
    - controllers
      - cover_letters_controller.rb
    - views
      - helppages
        - _modal.html.erb
        - content.html.erb
        - index.html.erb
    - config
      - initializers
        - help_pages.rb
  - static
    - docs
      - section1
        - doc1.md
        - doc2.md
        - ...
      - section2
      - section3
```

#### config/initializers/openresume/help_pages.rb

The initializer sets up your document path and initializes the sections and pages in each section.

```ruby
HELP_PATH = "static/docs"
HELPDIR = File.expand_path(HELP_PATH ) << "/" # don't miss adding "/"
HELP_SECTIONS = Dir[File.join(HELPDIR, '**')].map {|f| f.sub!(HELPDIR, "")}
HELP_PAGES = {}
HELP_SECTIONS.each do |s|
  HELP_PAGES[s] = Dir[File.join(HELPDIR + '/',  s, '**')].map {|f| f.sub!(HELPDIR, HELP_PATH )}
end
```

#### The Controller

The controller is pretty basic. The `index` method just loads a view which shows the sections and pages. Most of the work is done in the view. The `content` method simply loads the selected Markdown file and returns the rendered HTML over AJAX. Again,  the goal here is simplicity and ease of maintenance.

```ruby
class HelppagesController < ApplicationController

  # Show the index of all help/faq
  def index
  end

  # Render a specific page as plain AJAX/HTML
  def content
    md = File.read(File.expand_path(params[:p]))
    parser = Redcarpet::Markdown.new(Redcarpet::Render::HTML, autolink: true, tables: true)
    html = parser.render(md)
    render :plain => html
  end

  # Create a human readable page name from the file name.
  # '___' is used to indicate a question-mark (useful for FAQ)
  def self.format_page_name(s, p)
    p.gsub("#{HELP_PATH}#{s}/", "").gsub(".md", "").gsub("___", "?").gsub("_", " ").gsub(".md", "").split("/").last
  end

end

```


#### .../views/helppages/index.html.erb

The `index` view iterates over the `HELP_SECTIONS` and shows the list of documents in each. Each document link kicks out to `showHelpModal()` which loads the AJAX content into an modal popup. This example uses Semantic UI, but

```ruby
<% title "Help" %>
<h1><%= yield(:title) %></h1>

<%= render :partial => "modal" %>

<div class="ui relaxed divided list">
<% HELP_SECTIONS.each do |s| -%>

  <div class="item"><i class="folder icon"></i><div class="content">

      <div class="header"><%= s.upcase.gsub("_", " ") %></div>

      <div class="ui relaxed divided list">
        <% HELP_PAGES[s].each do |p| -%>
          <div class="item"><i class="file blue icon"></i><div class="content">
            <%= link_to HelppagesController::format_page_name(s, p),  "javascript:showHelpModal('#{p}');" %>
          </div></div>
        <% end -%>
      </div>

  </div></div>

<% end -%>
</div>
```

Using Semantic UI will yield an index that looks like this:

<img src="{{ site.url }}/assets/postimages/helppages.png">

The individual documents will appear as a modal pop-up window as defined below in `_modal.html.erb`.

#### .../views/helppages/_modal.html.erb

This view instantiates the Semantic UI modal popup and defines the JavaScript to load and trigger it. If you'd rather have individual pages load as a regular routes, then you can skip this and change the index view to handle links in that manner.

```ruby
<div class="ui modal" id="helpModal" style="height:400px; max-height: 400px; overflow:auto;">
  <i class="close icon"></i>
  <div class="content">
    <div class="description" id="helpcontent">
    </div>
  </div>
</div>

<script type="text/javascript">
function showHelpModal(p) {
  $( '#helpcontent' ).load( "/helppages/content?p=" + p );
  $( '#helpModal' ).modal({
    offset: 0,
    inverted: true
  }).modal('show');
};
</script>
```

#### .../config/routes.rb

Finally, update your routes to access the controller and set up a default `/help` route.

```ruby
get 'helppages/index'
get 'helppages/content'
get 'help' => 'helppages#index'
```

----

And there you have it. A complete help/faq system in about 100 lines of code. There may be more elegant ways to accomplish this, but for quickly generating internal documentation this will get the job done. All your documentation is in easy to edit Markdown files and you can re-arrange things by simply moving things between folders on the desktop and then pushing the code back to git.
