---
layout: post
title:      "Partials in Rails and Two Lessons Learned"
date:       2020-06-19 08:30:27 -0400
permalink:  partials_in_rails
---


From the very early days of our boot camp we've learned to keep our code DRY (Don't Repeat Yourself). A few ways to do this include controller helper methods, model helper methods, and my favorite - partials! 

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

In order for a partial to work properly across various views, we use locals:  

`locals: {playlist: @playlist}`

The locals parameter is a hash whose key is the local variable in the partial and whose value is the instance variable that is defined in the controller action that renders the view. In this example, `@playlist` is defined in the playlists new action as `@playlist = Playlist.new`. By setting `@playlist` as the value in the locals hash, all of the information stored in `@playlist` is passed to the local variable in our partial: `playlist`. 

Partials are a great, and really easy, way to Not Repeat Yourself.

Something about meeting the requirements for this Rails project that was not so easy was incorporating my join table's user-submittable attribute. My cohort lead, Annabel, helped me to figure out how to make my project idea of an app that allows a user to create songs and playlists meet the requirements by explaining how a category can connect a song and a playlist. Therefore, my join table for playlists and songs, categories, has one user-submittable attribute, which is name. 

I had already given songs a genre attribute, which is very similar to a category, and I ended up creating an entire program that used my join table but did not actually incorporate the user-submittable attribute, name, in any way. This was a massive oversight on my end that caused huge headaches and an enormous amount of stress and confusion as I tried to work the category name attribute in to my existing code. The original program I created had much more of the functionality I wanted. For example, when a user created a new playlist, he/she could:

1. Create it without any songs
2. Create it with only existing songs
3. Create it with only new songs
4. Create it with both existing and new songs

Adding in the category name attribute to my existing code turned out to be much more complicated than I expected. The biggest issue I was having was allowing a user to create a new playlist with existing songs - because when the user wanted to add an existing song, he/she would have to give that song a category name and both that song and its new category name had to be associated with the new playlist. I spent a ton of time trying to make everything work with the full functionality I wanted but because of the project deadline, I ended up simplifying my project a great deal. Now, a playlist can only be created without any songs, and songs can be added to the new playlist only after it's been created. I was initially really discouraged about this but I fully intend to keep working on this project on the side so that I can build it up to the full functionality I envisioned!

One of the hardest parts about getting my code working without any errors again while simplifying it a great deal was identifying all of the unnecessary and now-harmful lines of code that were leftover from my more ambitious (but not meeting requirements!) code. This led to an embarrassing, and thoroughly humbling, browser error experience only five minutes in to my first assessment attempt. 

I'm so glad that I was able to overcome these two difficulties and complete a Rails project that meets all of the requirements, and although it was extremely tough and felt very overwhelming and discouraging in the moment, I learned two huge lessons that I'm positive will help me finish boot camp and continue my coding journey successfully:

1. TEST EVERYTHING!!!!!! When you think you've tested everything, test it all over again! Have your cohort lead test your code, have your classmates test your code, test your code in absolutely every way you can think of. You can never test enough.
2. Make sure you're meeting project requirements before you get too deep in code that you're committed to but is not going to let you pass. You can always come back to your project and make it the project of your dreams later on, but for now, meeting the requirements is what's most important both for your success in boot camp and for your sanity!
