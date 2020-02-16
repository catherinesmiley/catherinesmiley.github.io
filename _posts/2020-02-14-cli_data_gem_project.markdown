---
layout: post
title:      "CLI Data Gem Project"
date:       2020-02-14 23:12:34 -0500
permalink:  cli_data_gem_project
---


Living in NYC, the land where everything costs 5 times more than it should, for the past 10 years has turned me into someone who relentlessly looks for the best deal at all times. I'm also a huge book nerd, which can become an expensive hobby if you're not a dedicated deal-seeker. There are cheap options for books, such as thrift store purchases and a subscription to BookBub's daily newsletter that lists e-books that are currently $5.99 or less, and there are free options, which include a library card and entering giveaways just in case you have a lucky moment and win. To keep tabs on the most recent giveaways on Goodreads, I scraped their Fiction Giveaways page for my CLI Data Gem Project. 



This CLI program allows the user to pull up the list of recently added Fiction Giveaways on Goodreads and to learn more details of the giveaway and the book itself.

My CLI program consists of three classes - Scraper, Controller, and Book. There is one scraping method in the Scraper class, .scrape_fiction_giveaway, which scrapes the Fiction Giveaways page (https://www.goodreads.com/giveaway/genre/Fiction?name=Fiction&sort=recently_listed&tab=recently_listed) using the Nokogiri and Open URI gems. Details about each book and its giveaway (title, author, release_date, and giveaway_details) are scraped from the page and saved in a hash within the array of Books that is the return value of the .scrape_fiction_giveaway method. 

The program, when run, first creates a new instance of the Controller class and runs the #run method on that new instance. 

The #run method of the Controller class contains three instance methods within the Controller class: 

1. #create_books  sets a local variable, book_array, equal to the .scrape_fiction_giveaway method of the Scraper class. It then passes that array as an argument to the .create_from_site method within the Book class, which iterates through that array and instantiates a new Book instance for each book element and uses metaprogramming, through the #initialize method, to assign the key/value pairs from that array of hashes as attributes of each new book instance, and stores each new book instance in the @@all class variable of the Book class. 
2. #welcome prints out two lines of welcoming text to the user using two puts methods
3. #get_input gets user input and uses if, elsif, and else statements to trigger individual methods and print out instructional lines of text depending on the user's input.

The first level of the program is revealed to the user by typing "list", which prints out a list of each Book instance along its affiliated number by calling #each.with_index(1) on the @@all class variable of the Book class. The user can then access the second level of the program and select a book by entering a number, 1-30, each of which corresponds to a Book instance. When the user enters a valid number (1-30), each attribute of that Book instance is printed out to the user: title, author, release date, and giveaway details. If the user enters a number outside of the range of 1-30, or if the user enters a typo at any time, an error message is displayed to the user and the instructions are printed out again. The program will continue to run and print out instructional messages to the user until the user enters "quit", which triggers the #quit_app method that exits the program.

I was overwhelmingly intimated by this project and felt it looming ahead of me from the very beginning of this boot camp, and although my end result is extremely simple, completing it feels like a huge accomplishment. The resources that were shared with us, particularly the suggested timeline of work broken down by day, Annabel's live lecture walking through the directory setup, and the multiple open office hours every weekday were what made it doable - thanks, Annabel, and Flatiron School! 


