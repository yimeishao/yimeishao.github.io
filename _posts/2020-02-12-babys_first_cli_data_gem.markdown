---
layout: post
title:      "Baby's First CLI Data Gem"
date:       2020-02-12 00:24:05 -0500
permalink:  babys_first_cli_data_gem
---


I spent a long time trying to decide on a website to scrape for my CLI data gem project. It wasn't going to be enough to build something just to meet the project requirements. I didn't want to build something that would just sit and collect dust after. I wanted to make something that I could care about--something *alive* in a way, something that I would want to revisit, update, and play with. 

To do that, I had to consider: 
* What am I interested in? 
* Is there a website for that? 
* Is the website easily scrape-able (scrapable?) for the data I want? 

After a week of consideration (which rapidly devolved into wondering whether I even had any interests at all), I decided to create an app that pulls from Missed Connections posts.

The concept of Missed Connections is really romantic at heart. You were going about your day and you saw a beautiful stranger. You post about the encounter online with the hope that maybe they'll see it and that maybe they were going about their day when they saw a beautiful stranger (you). Because let's face it, you don't want to interrupt someone's space. What if they're not interested? What if they just wanted a quiet commute? What if you're not a beautiful stranger? You'd be just a creep. In my opinion, Craigslist is a place to practice consent and respect -- both parties have to actively look for each other. 

Obviously, if you've ever been on Missed Connections, you know it doesn't always work like this. Instead, what you get is at worst a barrage of people's often disgusting, often fetishizing, inner thoughts. At best, a sad, truncated plea for intimacy. You wonder, "What beautiful stranger is going to read this and want to connect?" 

And so, I thought it would be fun to have them read as poems instead. A message created by collaborating different voices. Modern romance, baby! 

My app collects all of the post titles from a given day and formats them into a single "poem." It now lives as a Ruby gem called bzn_poetry, which you can find [here](https://github.com/yimeishao/bzn_poetry). (My version pulls from Bozeman's Missed Connections, because that's where I currently live, but it's easily adaptable for other locations.)

![](https://i.imgur.com/1HPBSnX.png)

Here's what I knew I needed to make this happen: 
* A class (Scraper) to scrape the webpage for data
* A class (Post) to store and process the data 
* A class (CLI) that allows the user to interact with the data 

No big deal, super simple, three classes. 

I wrote out my flow: Welcome > Main Menu gives user option to enter Poems Menu or exit > if user selects Poem Menu > app goes into my storage class to pull the list of days that have posts > my CLI class shows the user the list of available days > user selects a day > app goes into my storage class to find the appropriate poem > my CLI class returns it to the user. 

I built out my scraper class. Pretty straight-forward. Then, I started to build out my CLI class. Everything was going fine. 

Then, I started to process the data and hit a wall. How can I group together all of the posts that share a date? What if I put it all in a hash? The dates would be the keys and the values would the be posts. I could pull the hash into my CLI class which could then select the appropriate hash-key pair according to index. This worked. I got it all to work using a million instance variables and convoluted methods. It was ugly. It was fragile. 

Exhausted and looking at my code through my tears, I began to ask questions like, "Why does this method live in my CLI class if it's processing data?" "What the hell is going on in my Post class?" "How are these objects interacting with each other?" 

I had a meltdown, broke everything, and wrote two new classes: Dates and Poems. Dates would feed my CLI class the list of dates. Poems would format the posts. This was a bad idea. 

I kept getting lost pushing and pulling hashes and arrays back and forth into instance variables. There was no "single source of truth." Each of my classes had their own idea of how to store and interpret the data -- which really begged the question, even have distinct classes at all? Why not just slap it all into one long running thread? 

I needed to figure out how to organize and get back to the orignal plan: no big deal, super simple, three classes. Three classes, each clearly defined objects with their own attributes and behaviors, that can interact with each other. 

Now, everything has its place. My CLI class asks my Post class for its posts. If there aren't any posts, my CLI class triggers my Scraper class, which instantiates new posts in my Post class. New posts are instantiated with a date attribute and a title attribute. My Post class contains a method that enables you to search all of the posts by their date attribute. So when the user enters in a date that's received by my CLI class, my Post class can search and return the appropriate poem. 

Because everything has its place, I can more easily write new behaviors into my code. 

For example, if I wanted to give my user the option to see the number of entries per date, I could write a method in my Post class, which takes an argument `(date) ` entered in from my CLI class, and collects all of the titles that share that date attribute. My CLI class could then run `.count` and `puts` out the count for the user.

I've created a playground for myself that can withstand future development and I've got a million ideas for it. 

But for now, a poem: 

![](https://i.imgur.com/VfmCCcI.png)



