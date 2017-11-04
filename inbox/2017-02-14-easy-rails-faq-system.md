---
title: Quick-and-Easy Rails Help/FAQ System
---

yadda


#### The Controller

Code re-use is a good thing. But when you start trying to do this in your Views, it can lead to a tangled,
horrific mess. Conditionals and calculated values strewn all about. This is where Cells comes in - it
essentially creates a Model wrapper around specific views. So your "menu bar" no longer is just a partial,
it's now an actual Model with state and methods and validation. The Cell code combines a model and the view as
well, so everything's kept together.

```ruby
class HelppagesController < ApplicationController
  def index
  end

  def content
    md = File.read(File.expand_path(params[:p]))
    parser = Redcarpet::Markdown.new(Redcarpet::Render::HTML, autolink: true, tables: true)
    html = parser.render(md)
    render :plain => html
  end

  def self.format_page_name(s, p)
    p.gsub("#{HELP_PATH}#{s}/", "").gsub(".md", "").gsub("___", "?").gsub("_", " ").gsub(".md", "").split("/").last
  end
end

```

#### .../views/helppages/index.html.erb

```ruby
<% title "Help" %>
<h1 class="yieldh1"><%= yield(:title) %></h1>

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

#### .../views/helppages/_modal.html.erb

```
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

```
get 'helppages/index'
get 'helppages/content'
get 'help' => 'helppages#index'
```