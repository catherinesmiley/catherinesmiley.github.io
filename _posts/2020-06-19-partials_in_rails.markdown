---
layout: post
title:      "Partials in Rails"
date:       2020-06-19 12:30:26 +0000
permalink:  partials_in_rails
---


From the very early days of our boot camp we've learned to keep our code DRY (Don't Repeat Yourself). A few ways to do this are controller helper methods, model helper methods, and my favorite - partials! 

Partials are files saved in our view folders that contain a block of HTML that is used in multiple view files. For example, the form code in an app's new and edit views are typically exactly the same. The only differences between the two are the form's method and action, and thankfully Rails is magical enough to set those up for us (as long as we've defined the necessary object as an instance variable in the controller action that renders the view). Therefore, creating a partial allows you to insert several lines of code into a view using just one line. 

This line in my playlists new file (playlists/new.html.erb): 

`<%= render partial: 'form', locals: {playlist: @playlist} %>`

Renders all of this code, found in my playlists form partial (playlists/_form.html.erb):

```
<%= form_for playlist do |f| %>
    <%= f.label :name %>
    <%= f.text_field :name %><br><br>
    <%= f.label :description %>
    <%= f.text_area :description %><p>

    <%= f.submit %>
<% end %>
```

In order for a partial to work properly across various views is through the use of locals:  

`locals: {playlist: @playlist}`

The locals parameter is a hash whose key is the local variable in the partial and whose value is the instance variable that is defined in the controller action that renders the view. In this example, `@playlist` is defined in the playlists new action as `@playlist = Playlist.new`. By setting `@playlist` as the value in the locals hash, all of the information stored in `@playlist` is passed to the local variable in our partial: `playlist`. 

Partials are a great, and really easy, way to Not Repeat Yourself, and I was excited to use them for my Rails project!
