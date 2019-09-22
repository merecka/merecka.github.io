---
layout: post
title:      "Debugging in Ruby and Rails"
date:       2019-09-21 15:02:34 -0400
permalink:  debugging_in_ruby_and_rails
---

I just completed the Rails section of the Flatiron Software Engineering bootcamp and one of the valuable lessons that I learned while completing my final Rails project is the power of effective debugging.  As any seasoned programmer will tell you, deubgging is a required everyday activity performed by software engineers and therefore it is cruicial to develop strong debugging skills in order to ensure your success as a software developer.  And while I am still fairly new in my own journey into becoming a software developer, I can already see how important it is.  

For this post I am going to talk about two debugging tools that I have used and find very helpful, namely:  

* Pry 
* Rails Console

While these are certainly not the only debugging tools available to use with Ruby or Rails, they are quite useful and the one's I have the most experience with at this time so I will focus on them for this post.

# Pry

We'll start off with discussing Pry.  Pry is an open-source gem available for anyone to use and is available for download on multiple locations including RubyGems.org and GitHub (https://github.com/pry/pry).  The easiest way to install Pry is to add it to your Gemfile in your Ruby (or Rails) project with the command `gem 'pry'`.  After this is done you can go to your terminal and navigate to the home directory of your project and execute the command 'bundle install' (assuming you have Bundler installed).  

![](https://i.imgur.com/hPIIkpl.png)

This will reach out to https://rubygems.org and locate and install all of the gems in your Gemfile (including Pry).  Once this is done you are ready to start using Pry.

So how do you actually use Pry for debugging?  Well here's an example:

Let's say we are trying to view the list of Rooms in the Rooms#Index view of our hotel booking Rails application.  We can see the link to 'View Available Rooms' which points to the `rooms#index` action in our controller.

![](https://i.imgur.com/RAJpNmv.png)

but when we click the link we receive the following error message:

![](https://i.imgur.com/8bxym12.png)

From the looks of this error, it appears as though there is an issue with the following line of code `<% @rooms.each do |room| %>` which exists in our rooms#index view page as is told to us at the top of the error page.  Wouldn't it be nice if we could somehow dive into our code at this exact spot where we are getting this error to see what value(s) our variables & methods are giving us and which could possibly be throwing this error?   Well we can and that's one of the big advantages of using Pry for debugging!  So how can we do this?  

Well first, we have to decide where we want to start debugging.  In this example, a logical place would be to start where we are receiving the error message (ex: before or after the line `<% @rooms.each do |room| %>`.  So with Pry we can essentially "pry" into our application at any point simply by putting in the command `binding.pry`.  And when we execute our application and it hits this line, it will allow us to perform some debugging actions at that exact point in time of execution.  So let's try putting a `binding.pry` one line after the line of code throwing the error and refresh the page and see what happens.

*Note:  We are using the format of `<% binding.pry %> ` since we are in an ERB view page.  When using Pry outside of view pages, we only need to type `binding.pry`.

![](https://i.imgur.com/zPFzsQH.png)

![](https://i.imgur.com/KDPze5z.png)

So in order to perform our debugging with Pry, we need to go to our terminal window in order to check the values of our variables, methods, etc and if our attempt at using Pry worked we should be prompted with a session to do such that.  So let's see what our terminal looks like: 

![](https://i.imgur.com/TfaDHSs.png)

Oops, it looks like even though we inserted a binding.pry into our View page, we're still unable to get into it since we don't see any prompt for Pry in our terminal.  Why don't we try putting a `binding.pry` before the line of code that's throwing the error, refresh our page, and then view our terminal to see what happens.

![](https://i.imgur.com/ukAeknR.png)

![](https://i.imgur.com/VgoKhHs.png)

Great!  Now we can see our Pry prompt at the bottom of our terminal screen which means that we've successfully 'hit' our `binding.pry`.  So what can we do now that we're in Pry?  Well we can essentially run any Ruby command as if we were in an IRB session.  In this case, it would probably make sense to check the value of our `@rooms` variable that we are trying to iterate over and see what (if any) value it holds.  So let's do that:

![](https://i.imgur.com/g8mE1ah.png)

Uh oh, it looks like we're returning `nil` for `@rooms` which could definitely be one of the causes of our problem.  So we'll need to figure out from where we're passing in the `@rooms` variable into our view page.  If we've followed the RESTful organizational structure, it should be passed in through our Rooms#Index controller action.  So let's take a look there:

![](https://i.imgur.com/bageiCf.png)

Sure enough it looks like it's there and that's where the issue is.  Can you see it?  It looks like we forgot the `@` symbol in front of `rooms` variable in order to make it an instance variable which can be passed into our #Index view.  So that would explain why `@rooms` is returning a nil value (because we haven't declared it properly in the controller).  So this should be an easy fix so let's give it a shot:

![](https://i.imgur.com/CKyt7u3.png)

So before we can refresh our page with our new code, we'll need to go into our terminal and terminate our Pry session by exiting out with the `exit!` command (note: we can also use `exit` as well but if there's other `binding.pry`'s we'll have to type 'exit' each time we hit another one).  

![](https://i.imgur.com/Tq9znsv.png)

Now that we're out of our Pry session we can restart our server and refresh our page and keep our fingers crossed that everything is working now:

*Note:  You will need to remove the `<% binding.pry %>` line in the view page prior to refreshing the page in the browser or you will keep 'hitting' it and will have to go to the terminal window each time to exit out of the Pry session.  That being said, you can essentially put in an unlimited number of `binding.pry`'s into your code to allow you to 'pry' into different parts in one Pry session.

![](https://i.imgur.com/qgBVhgy.png)

Great! It looks like our page is working now and we see a list of Rooms like we expect to in our #index action.  

As you can probably guess, this is a fairly easy, straight-forward issue that we worked through but it's main purpose is to demonstrate how Pry works and the commands & procedures you use in order to utilize it for debugging.  This is by no means the extent of what Pry can do and there is a lot of material available on the internet to read which goes more in-depth into all of the extra features & commands you can use.  But if you understand the basics of what we did here then you're well on your way to discovering the usefulness of Pry.

# Rails Console
Another very useful tool in the world of Ruby and Rails debugging is the Rails console.  The Rails console is essentially an IRB environment which is built on top of the Rails environment and therefore gives access to Rails and all it's methods and features.  The Rails console is a terminal (command-line) interface similar to Pry but the difference is that it can be activated at any time whether or not you have a web server running.  The console is started by executing the command `rails console` from the terminal command line within the root directory of your Rails application.  

![](https://i.imgur.com/1wHTEyr.png)

Once this is done you are now inside the console and can begin debugging, experimenting with different code, etc.  For instance, in our Reservation booking application we can query our SQLite3 database using the ActiveRecord method `.all` from within the Rails console in order to view our Rooms model data:

![](https://i.imgur.com/UaYJBdN.png)

We can also create and test new methods, variables, etc in order to test them inside the console before committing them to our application.  In this example we are going to try to create a variable that will return the Room (object) that has the largest occupancy.

![](https://i.imgur.com/nyiq1nU.png)

Now that we've successfully tested our code, we can now implement it into our application and commit to our codebase.  The convenience here is that this variable is only temporarily stored in the cache of our Rails console session and once we terminate the session it is not permanently stored in our application (so if we really messed up it won't affect anything permanent in our program).  Note however that any data we persist to the database with methods such as `.create`, `.update`, and `.destroy` will impact the database permanently so proceed with caution when using any of these methods.  

To terminate the Rails console session simply execute the `exit` command from the command line and your session will be ended.  

The Rails console is a very useful and simple to use interface for debugging and the nice thing is that it does not require any separate installation as it is already built into Rails.  You can access it directly from the command line and interact with your Rails application just like you would through IRB.  

# Final Thoughts
We have seen two different methods available to use for debugging in Ruby and Rails.  Both of these tools are useful to programmers while developing applications and each one has it's own advantages.  In practice both of these tools are most useful when used in conjuction with each other so therefore it recommended to master the use of both so that the full advantage of each tool can be realized.  As with learning any new tool, it takes time to master the use of these tools so the more you use them the more tricks and uses you will discover.  

Thanks for reading my blog post and I hope this info was helpful to your programming journey.












