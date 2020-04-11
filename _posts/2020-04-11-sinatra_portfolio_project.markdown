---
layout: post
title:      "Sinatra Portfolio Project"
date:       2020-04-11 14:01:43 -0400
permalink:  sinatra_portfolio_project
---


Many people have a list of restaurants they'd like to eat at one day, although the way people keep track of this list differs by person. I keep a running list in the Notes app on my phone, save Instagram posts, and bookmark articles that I come across, but I don't have one central place that keeps track of the list of names and details about the restaurants I'd like to visit in a consistent way. I'd love to have one, so for my Sinatra Portfolio Project, I created a CRUD MVC app that allows users to keep track of restaurants on their bucket lists and to store and update information about those restaurants.

I completed the basic requirements for this project a few days early - users can create accounts, log in to, and log out of those accounts; users have many restaurants and a restaurant belongs to a user using the has_many and belongs_to macros and a foreign key on the restaurants table; users can create, read, update, and destroy restaurants that belong to that user; and users cannot update or destroy restaurants that do not belong to them. 

I wanted to add on to the functionality by incorporating a third table - menu items - that allows a user to store a name and description for up to two menu items they would like to try at each specific restaurant. To do so, I created a table of menu items that includes a foreign key for the restaurant id and ensured a menu item belongs to a restaurant and that a restaurant has many menu items by using the has_many and belongs_to macros in the restaurant and menu item models.

```
class Restaurant < ActiveRecord::Base

    belongs_to :user 
    has_many :menu_items

    validates :name, presence: true

end 
```

```
class MenuItem < ActiveRecord::Base 

    belongs_to :restaurant

    validates :name, presence: true

end 
```

I then had to add menu items to the restaurants controller and to the various restaurants views. I first had to update my new restaurant form to be a nested form so that more than one object could be created at once (one restaurant and 1-2 menu items). This involved updating the name attributes of the input tags so the params could be sent to the controller action in the form of a nested hash. To do this for the menu items params, I added an empty array in the form which allows ERB to index the array for me:

```
    <input type="text" name="restaurant[name]" /><br />
    <h3><label>City: </label></h3>
    <input type="text" name="restaurant[city]" /><br />
    <h3><label>Category: </label></h3>
    <input type="text" name="restaurant[category]" /><br />
    <h3><label>Menu Item 1: </label></h3>
    <input type="text" name="menu_items[][name]" /><br />
    <h3><label>Menu Item Description 1: </label></h3>
    <input type="text" name="menu_items[][description]" /><br />
    <h3><label>Menu Item 2: </label></h3>
```

Once the user submits the form, it's sent to the /restaurants post route. In this route, a new restaurant instance is created with the restaurant params hash, which includes the restaurant name, city, category, and notes. I set a local variable, menu_items, equal to the menu items params hash, which is an array of hashes where each hash includes a menu item name and description. I then iterate over the menu_items array of hashes and, for each item in the array, I create a new menu item instance, associate it with the restaurant by setting the menu_item's restaurant to the restaurant instance, and save it the new menu item:

```
 post '/restaurants' do 
        restaurant = Restaurant.create(params[:restaurant])
        menu_items = params[:menu_items]
        menu_items.each do |item|
            menu_item = MenuItem.create(item)
            menu_item.restaurant = restaurant
            menu_item.save
        end
        user = Helpers.current_user(session)
        restaurant.user = user 
        restaurant.save 
        redirect to "/users/#{user.id}"
    end 
```

Incorporating this third table of menu items gives my project an additional level of functionality that allows users to save and update information not only about restaurants, but also about menu items at each restaurant, which is a great way to keep track of both where you want to go and what you want to eat while you're there all in one place.
