---
layout: post
title:      "My first program (a CLI project)"
date:       2019-05-11 22:51:45 +0000
permalink:  my_first_program_a_cli_project
---

I've officially made it through the Object Oriented Programming (OOP) section of the Ruby track of the FlatIron Software Engineering bootcamp and as it is for all of the other sections of the course, we were given a project to complete that would reinforce our understanding of the material we just learned (in this case, Object Oriented Programming & Web Scraping).  The requirements for the project were to create a CLI (Command Line Interface) program that scrapes HTML / CSS data from a website of our choosing and presents a menu (at least one level deep) to the user of our program.  Beyond that, the rest of the requirements were open to our choosing.  

I have a very strong passion for surfing and I have traveled around the world finding the best beaches and waves to surf.  One of the spots that has always been very close to my heart is Costa Rica.  For this reason, I decided it would be fun to create a CLI program that would display the daily surf report for various famous surfspots in Costa Rica.  To do this, I decided to create a menu driven Ruby gem that would scrape surf report data from the web and print it via the Terminal to the user of the gem.  For the surf report data, I decided to utilize the website Magicseaweed.com due to the quality of up-to-date data it has.  I utilized the Nokogiri gem to perform the actual scraping of the data into XML elements and then wrote various methods & objects to parse and collect the data into a readable format.  

Overall this was a very challenging but rewarding experience and has helped strengthen my understanding of OOP in general as well as helped me gain confidence in my overall programming abilities.  Below are some of the aspects of this experience that have stood out or were challenging and made me learn quite a bit more about them.

**Git Hub**

One of the first things I had to learn about when creating my gem project was GitHub.   I realized very early that I needed somewhere and someway to save my code throughout the process (and using Notepad to save it didn't sound like a great idea).  So I started researching how to link my code to GitHub and discovered new terms like 'repository', 'git add, 'git commit' and 'git push'.  I also learned how to clone my project to my IDE because I was using an IDE provided by FlatIron that utilizes integration with GitHub but does not actually store data locally.  After the project was over, I felt that I am much more comfortable using GitHub and I am starting to realize it's value to software development.

**HTML / CSS Scraping**



I had some previous experience with HTML scraping from a few prior labs that I had completed but this project really helped reinforce this process.  I also learned more about HTML / CSS data structures in general by having to parse through the data in order to obtain what I wanted for my program.  I discovered a few Google Chrome extensions that assisted me with finding a CSS selector that I could use to help parse the data.  These are SelectorGadget (https://selectorgadget.com/) and ChroPath (https://autonomiq.io/chropath/).  Both of these extensions assist you in finding appropriate CSS selectors for the data you are trying to parse.

**Snapshot of Program**

Here is the basic outline and function of my program.  The program starts and presents the user with a first-level menu which is a list list of different geographical provinces within Costa Rica.  The user is able to select which province they want to view by typing the corresponding number and pressing 'Enter'.

![](https://i.imgur.com/bcaHhZF.jpg)

Once this is done, the user is then presented with a second-level menu which displays a list of surf spots located in the selected province.  Again, the user is able to select which surfspot they would like to view by typing the corresponding number and pressing 'Enter'.

![](https://i.imgur.com/ewy8kem.jpg)

At this point, the program will pull the current surf report data from the MagicSeaweed website for the chosen surf spot and display it to the user in the Terminal, as depicted here.

![](https://i.imgur.com/VF0dLQG.jpg)

Lastly, the user is presented with an option to 'See another forecast' or 'Exit'.  If the 'See another forecast' option is chosen, the program will re-run from the beginning and present the previous menu options to the user in the same order.  

At the heart of this program is the webscraping performed by a Ruby Gem called Nokogiri.  This Gem will take HTML / CSS data from a website and store it into arrays of XML elements.  From here, Ruby code can be written to iterate through and display or manipulate the data however the developer decides.  Below I will show a sample of the website pages I used and the corresponding Nokogiri code used to extract the data.

First-level menu webpage:  Costa Rica Provinces

![](https://i.imgur.com/JZisxM0.jpg)

And the corresponding Nokogiri web scraping code:

![](https://i.imgur.com/aWTWo7a.jpg)

Second-level menu webpage:  Surf Spot List (based on selected Province)

![](https://i.imgur.com/E1pA6rB.jpg)

And the corresponding Nokogiri webscraping code:

![](https://i.imgur.com/2AYRWLT.jpg)

Third-level menu webpage:  Surf Spot Forecast (based on selected Surf Spot)

![](https://i.imgur.com/lrSdsit.jpg)

And the corresponding Nokogiri webscraping code:

![](https://i.imgur.com/cWtsdKp.jpg)




