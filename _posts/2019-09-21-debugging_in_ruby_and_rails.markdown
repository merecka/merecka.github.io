---
layout: post
title:      "Debugging in Ruby and Rails"
date:       2019-09-21 15:02:34 -0400
permalink:  debugging_in_ruby_and_rails
---

I just completed the Rails section of the Flatiron Software Engineering bootcamp and one of the valuable lessons that I learned while completing my final Rails project is the power of effective debugging.  As any seasoned programmer will tell you, deubgging is an everyday activity performed by software engineers and developing strong knowledge behind will be crucial to your success as a software developer.  And while I am still fairly new in my adventure into becoming a software developer, I can already see how important it is.  

For this post I am going to talk about two debugging tools that I use that I find very helpful:  

* Pry 
* Rails Console

While these are certainly not the only debugging tools available to use with Ruby or Rails, they are quite useful and the one's I have the most experience with at this time so I will focus on them for this post.

# Pry

We'll start off with discussing Pry.  Pry is an open-source gem available for anyone to use and is available for download on multiple locations including RubyGems.org and GitHub (https://github.com/pry/pry).  The easiest way to install Pry is to add it to your Gemfile in your Ruby (or Rails) project with the command `gem 'pry'`.  After this is done you can go to your terminal and navigate to the home directory of your project and execute the command 'bundle install' (assuming you have Bundler installed).  

![](https://imgur.com/hPIIkpl)

This will reach out to https://rubygems.org and locate and install all of the gems in your Gemfile (including Pry).  Once this is done you are ready to start using Pry.

So how do you actually use Pry in the debugging phase?  Well here's an example:

Let's say we are trying to view the list of Rooms in the Rooms#Index view of our hotel booking Rails application.  We can see the link to 'View Available Rooms' which points to the `rooms#index` action in our controller.

![](https://imgur.com/RAJpNmv)

but when we click the link we receive the following error message:

![](https://imgur.com/8bxym12)

From the looks of this error, it appears as though there is an issue with the following line of code `<% @rooms.each do |room| %>` which exists in our rooms#index view page as is told to use at the top of the error screen.  Wouldn't it be nice if we could somehow diving into our code at the exact spot where we are getting this error to see what value(s) our variables & methods are giving us which could be throwing this error?   Well we can and that's one of the big advantages of using Pry for debugging!  So how can we do this?  

Well first, we have to decide where we want to start debugging.  In this example, a logical place would be to start where we are receiving the error message (ex: before or after the line `<% @rooms.each do |room| %>`.  So with Pry we can essentially "pry" into our application at any point simply by putting in the command `binding.pry`.  And when we run our application and it hits this line, it will allow us to perform some debugging actions at that exact point in time of execution.  So let's try putting a `binding.pry` one line after the line of code throwing the error and refresh the page and see what happens.

*Note:  We are using the format of `<% binding.pry %> ` since we are in an ERB view page.  When using Pry outside of view pages, we only need to type `binding.pry`.

![](https://imgur.com/zPFzsQH)

![](https://imgur.com/KDPze5z)

So in order to perform our debugging with Pry, we need to go to our terminal in order to check the values of our variables, methods, etc and if our attempt at using Pry worked we should be prompted with a session to do such that.  So let's see what our terminal looks like: 

![](https://imgur.com/TfaDHSs)

Oops, it looks like even thought we inserted a binding.pry into our View page, we're still unable to get into it since we don't see any prompt in our terminal.  Why don't we try putting our `binding.pry` before the line of code that's throwing the error, refresh our page, and then view our terminal to see what happens.

![](https://imgur.com/ukAeknR)

![](https://imgur.com/VgoKhHs)

Great!  Now we can see our Pry prompt at the bottom of our terminal screen which means that we successfully 'hit' our `binding.pry`.  So what can we do here?  Well we can essentially run any Ruby command as if we were in an IRB session.  In this case it would probably make sense to check the value of our `@rooms` variable that we are trying to iterate over and see what (if any) value it holds.  So let's do that:

![](https://imgur.com/g8mE1ah)

Uh oh, looks like we're getting a `nil` value for `@rooms` which could definitely be the cause of our problem.  So we'll need to figure out where we're passing in the `@rooms` variable from into our view page.  And if we've followed RESTful organizational structure, it should be in our Rooms#index controller action.  So let's take a look:

![](https://imgur.com/bageiCf)

Sure enough it looks like the issue is here.  Can you see it?  It looks like we forgot the `@` symbol in front of `rooms` in order to make it an instance variable that we can pass to our #index view.  So that would explain why `@rooms` is returning a nil value (because we haven't declared it properly in the controller).  So this should be an easy fix so let's give it a shot:

![](https://imgur.com/CKyt7u3)

So before we can refresh our page with our new code, we'll need to go into our terminal and terminate our Pry session by exiting out with the `exit!` command (note: we can also use `exit` as well but if there's other `binding.pry`'s we'll have to type 'exit' each time we hit another one).  

![](https://imgur.com/Tq9znsv)

Now that we're out of our Pry session we can restart our server and refresh our page and keep our fingers crossed that it is working now:

*Note:  You will need to remove the `<% binding.pry %>` line in the view page prior to refreshing the page in the browser or you will keep 'hitting' the Pry and will have to go to the terminal window each time to exit out of the Pry session.  

![](https://imgur.com/qgBVhgy)

Great! It looks like our page is working now and we see a list of Rooms like we expect to in our #index action.  

As you can probably guess, this is a fairly easy, straight-forward issue that we worked through but it's main purpose is to demonstrate how Pry works and the commands & procedures you use in order to utilize it for debugging.  This is by no means the extent of what Pry can do and there is a lot of material available on the internet to read which goes more indepth into all of the extra features & commands you can use.  But if you understand the basics of what we did here then you're well on your way to discovering the usefulness of Pry.










