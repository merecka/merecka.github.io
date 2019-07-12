---
layout: post
title:      "Sinatra Project - A Freelance Job Board"
date:       2019-07-11 20:45:54 -0400
permalink:  sinatra_project_-_freelance_job_board
---


Part of the FlatIron Bootcamp curriculum is to create a web application using Sinatra, a framework built on top of the Ruby language (http://sinatrarb.com/).  Sinatra was developed by Blake Mizerany and is described as a "DSL for quickly creating web applications in Ruby with minimal effort".  Being that this is my first framework to use and learn, I cannot compare it's difficultiness relative to other frameworks but I can say that I was able to grasp the basic understanding of it in about a week.  Once I did a couple of small projects on it through the FlatIron curriculum, I found it pretty easy to develop my own web app from scratch.  The most difficult part that I found was 1) coming up with a solid idea for a web app, and 2) setting up and making sure my ActiveRecord associations were working properly.  Overall I really enjoyed the experience of building this web app and it has given me an appreciation for all of the gem's and framework's that are out there for web developers' use.  

**Getting Started**
Basically the first step of creating my project was utilizing the gem called Corneal.  Corneal is a gem (written by a FlatIron alumn) that initializes and sets up the initial project directory and folders with all of the required files (with the correct naming conventions).  This is really useful as it automates some of the 'grunt' work out of initializing the project https://thebrianemory.github.io/corneal/.   



![](https://i.imgur.com/Q4APcl9.png)


**The Work Begins**
After getting the project directory structure initialized the next step was trying to figure out my Models, Views, and Controllers.  But before I could do all that, I had to really figure out my Models and their Associations.  This is where the struggle started as ActiveRecord associations are still a work-in-progress for me.  In order to help my accomplish this, I decided to draw out my tables on paper in order to visualize them easier.  

![](https://i.imgur.com/ZoMsMHZ.png)

The biggest challenge here was figuring out the association between Users, Owners, and Jobs.  Since Jobs are created by Owners, they essentially belong to Owners.  But I wanted to implement a way for Users (aka:  job applicants) to be able to apply to jobs and in essence take ownership of Jobs too.  But in this case, a User would need to be able to share ownership over many Jobs.  This is where the Users-Jobs 'joins' table comes in.  By having a 'joins' table, we can use the has_and_belongs_to_many ActiveRecord association between Users and Jobs which will allow multiple Job records to be associated to multiple Users at the same time.  And further, an individual User can be associated to multiple Jobs at the same time.  

Once the assocations were setup, then it was time to start building the Controllers and Routes.  Naturally the first route I built was the index ' get '/' do' route.  This is basically the index (homepage) of app.  


The homepage of my app:
![](https://i.imgur.com/x5HeGG8.png)



From there I had to decide what Views I wanted to use in order to build additional routes.  I knew that I would need separate Show pages for Users and Owners since they would need to see different data.  Owners (aka:  Companies) would be interested in creating job postings as well as viewing their job postings while Users (aka:  Job Applicants) would be intersted in viewing available job postings as well as knowing which jobs they had applied to.  

This is the User's show page:
![](https://i.imgur.com/C55A4wl.png)



And the Owner's show page:
![](https://i.imgur.com/rZ10VNZ.png)



The functionality of this app is that Owner's have the ability to create job postings while User's can only view job postings.  

The Owner's job creation page:
![](https://i.imgur.com/bytO72F.png)

All Jobs and Users & Owners accounts are saved to a SQLite database.  User and Owner accounts are created username, email address, and passwords:



![](https://i.imgur.com/X1m8EAh.png)



Passwords are stored using the has_secure_password macro which utilizes the BCrypt gem to encrypt passwords https://en.wikipedia.org/wiki/Bcrypt.  

**Conclusion**
As I mentioned at the beginning of this post, this overall experience was very benficial to my programming development.  I have more confidence in my programming abilities and also am beginning to understand the vast possibilities of applications that can be created using a framework like Sinatra.  That being said, I also feel like I've barely scratched the surface when it comes to web app development and I am definitely hungry to learn more.  

Even though I call this app complete, I still feel there are lots of additional features I could add to make it more versatile.  In addition, there is definitley lots of room for adding additional HTML/CSS styling to make it more aesthetically pleasing from a UX point of view.  

But those things will have to wait for now because next week I will begin the adventure of learning Ruby on Rails..
