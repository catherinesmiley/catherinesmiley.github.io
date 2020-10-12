---
layout: post
title:      "React-Redux Final Project"
date:       2020-10-11 10:50:35 -0400
permalink:  react-redux_final_project
---


One of my favorite things in the world is camping. I don't consider myself an outdoor person but there's something about waking up, opening your eyes, and seeing nothing but trees and the sky that makes me never want to go back to NYC. That and you can be lazy all day long - read, nap in a hammock, do crossword puzzles by the fire - and it's totally acceptable. My boyfriend and I go at least once a year and we always have an amazing time but when we went a few weeks ago we forgot a few key items - paper towels and soap - and had to leave the campsite to go buy those things, which totally ruins the camping experience! This made us decide to create an extensive camping checklist so we never make the same mistake again, and thus this project idea was born.

I created a checklist library app with pre-made checklists for users to be able to have a thorough list of all necessary items for different situations so they can be confident they have absolutely everything they need. Of course, the first checklist category is for camping, and first on the list is paper towels and soap!

To create this app I first created a Ruby on Rails API to serve as the back end. For the front end, I used the React library with ES6 JavaScript.  I also added Redux and Redux-Thunk middleware to handle state change and to use fetch  actions to communicate with the server. I created `category` and `item` models and associated them with a `has_many` and `belongs_to` relationship (a category has many items and an item belongs to a category). 

Similar to the previous project built in vanilla JavaScript, I utilized fetch requests to send data to and receive data from the server - namely, to retrieve all of the checklist categories that have been created so that users can access them. Since I installed Redux and Redux-Thunk middleware in this project, though, these fetch requests look a little different. Redux follows the core flow of Action -> Reducer -> Updated State. Redux-Thunk allows actions creators to return a function whereas they typically expect just an object. In this way, we can create a fetch request as an action whose `return` statement receives `dispatch` as an argument, and we can then dispatch an action after the fetch request has resolved:


```
export const fetchCategories = () => {
    return dispatch => {
        fetch('http://localhost:3090/categories')
        .then(resp => resp.json())
        .then(responseJSON => dispatch({ type: 'FETCH_CATEGORIES', categories: responseJSON }))
    }
}
```

In the example above, the `return` statement of `fetchCategories` accepts `dispatch` as an argument. We then execute a fetch request to our categories index endpoint, convert the data from that returned promise to JSON, and use the data from the second returned promise to dispatch our `FETCH_CATEGORIES` action, passing along the returned data as the value of categories. 

Our `FETCH_CATEGORIES` action is then sent to our `categoriesReducer`, which takes in the arguments of `state` and `action`, and matches the received action to the `FETCH_CATEGORIES` case statement:


```
case 'FETCH_CATEGORIES':
            return {
                ...state,
                categories: action.categories,
                loading: false
            }
```

Here, we're returning an object that includes the current state through use of the spread operator, sets the categories state attribute to the value of `action.categories` (which includes the data returned from our fetch request that we set to the key of categories), and sets the loading state attribute to false. Now, our state includes all categories that we fetched from our Rails API, and we can utilize these checklist categories throughout our app!
