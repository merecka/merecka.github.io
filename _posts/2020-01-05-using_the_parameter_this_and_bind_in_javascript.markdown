---
layout: post
title:      "Using the parameter *this* and *bind()* in JavaScript"
date:       2020-01-05 20:13:01 +0000
permalink:  using_the_parameter_this_and_bind_in_javascript
---

I have just completed my project for the JavaScript section of the Flairon Software Engineering bootcamp and during this process  I learned some very valuable skills, especially pertaining to JavaScript.  

One of the more important concepts that I learned and frequently used throughout my project was the concept of *this*.  The parameter *this* is essentially a keyword that references the owner of the object that it belongs to.  If you are familiar with Ruby, then *this* is very similar to *self*. In JavaScript, the default "global" object for *this* in a browser window is `[object Window]` so whenever *this* is referenced inside the browser console in the absense of JavaScript code, it returns the Window object as demonstrated below:

![](https://imgur.com/a/0nWa3dt)

JavaScript functions are also tied to the global Window object so whenever *this* is called inside a standalone, one-dimensional function, it returns the default "global" Window object as well:

![](https://imgur.com/a/KCEZKqH)

So when does *this* ever change value to something other than the default "global" Window object?  One place this occurs is inside Event Listeners.  For example, in the code below, *this* is referenced inside an Event Listener tied to the click action of a button.  When we add a breaktpoint in the Event Listener and reference *this*, we see that it references the object tied to the Event Listener, which in this case is the button.

![](https://imgur.com/a/uwjC3YS)

Another example of *this* changing value to something other than the default "global" Window object is with the use of objects.  Javascript ES6 introduced the concept of Classes which can be used to instantiate instances of Class objects when using the Constructor function.  In the example below we have added a breakpoint inside a method that is tied to an instance of a Class object.  If *this* is referenced inside the method after the the Class object is instantiated, *this* will reference the instance of the object as seen below:

![](https://imgur.com/a/zu31Bdv)

So what practicality does *this* have and what does it actually do for us?  In essence, *this* allows us to access properties of objects.  In the example below, we have defined properties of instances of PrimaryComments objects.  Since we are accessing *this* from within the PrimaryComments class, it references the instance of the PrimaryComment that was instantiated.  So based on this, we can access the properties of the PrimaryComment object that we set in the constructor function from within methods tied to the instance such as the `fetchAndLoadPrimaryComments()` method as seen below:

```
class PrimaryComments {
	constructor(users) {
		this.users = users
		this.primarycommentsUrl = "http://localhost:3000/primary_comments"
		this.primary_comments = []
		this.secondary_comments = []
		this.adapter = new PrimaryCommentsAdapter()
		this.sec_com_adapter = new SecondaryCommentsAdapter()
		this.BindingAndEventListeners()
		this.createNewQuestionButton()
		this.fetchAndLoadPrimaryComments()
	}
	
		fetchAndLoadPrimaryComments() {
		this.adapter.getPrimaryComments()
		.then(prime_comments => {
			// Add's fetched Primary Comments to primary_comments array
			prime_comments["data"].forEach(comment => this.primary_comments.push(new PrimaryComment(comment["attributes"]))) 
		})
		.then(() => { 
			this.renderPrimaryQuestions()
		})
	}
```

So what happens if we need to change which object *this* references from the default ownership it is given?  There are definitely situations where this would be desired but can it be done?  Luckily for us it can with the use of the `bind()` function!

To start off, the `bind()` function creates a new function called a **bound function** that sets the original (target) function's *this* parameter equal to the scope of the passed-in argument.  The syntax for using `bind()` is the following:

```
function.bind(thisArg[, arg1[, arg2[, ...]]])

where the following are defined as:

function - The target function on which bind is called and who's *this* parameter will be affected.  

thisArg - The object from who's scope value the *this* parameter will inheret when the bound function is called. 

arg1, arg2, etc - Arguments to prepend to arguments provided to the bound function when invoking the target function.
```

So now that the syntax is covered, let's take a look at some examples of how `bind()` can be used in our code.  Here's an example of using `bind()` to change *this* from referencing the `new_prime_comment_button` to referencing our `Primary Comments Adapter`.  

In the example below we observe that when we execute the function `renderNewQuestionForm` from within inside the EventListener, the default value of *this* references the `new_prime_comment_button`.  However that is not what we want in this case because anytime we invoke *this* inside the callback function `renderNewQuestionForm`,  it will point to the button object and not to the current instance of the Primary Comment Adapter (represented by `this.adapter`). 

```
// Adds a 'Ask a New Question' button to the DOM
	createNewQuestionButton() {
		this.newQuestionFormDiv.innerHTML = ""
		const new_prime_comment_button = document.createElement('button')
		new_prime_comment_button.id = "new-question-button"
		new_prime_comment_button.className = "btn btn-primary"
		new_prime_comment_button.innerText = "Ask a New Question"
		const linebreak = document.createElement('br')
		new_prime_comment_button.appendChild(linebreak)
		this.newQuestionFormDiv.append(new_prime_comment_button)
		new_prime_comment_button.addEventListener("click", this.adapter.renderNewQuestionForm)
	}
```

![](https://imgur.com/a/yXvcKOD)

In order to modify the value of the *this* parameter that is passed into the `renderNewQuestionForm` function, we have to utilize the `bind()` method.  In order to do this we have to add `bind()` to the object who's *this* value we want to modify as well as supply an argument to `bind()` with the value that we want *this* to have:

```
// Adds a 'Ask a New Question' button to the DOM
	createNewQuestionButton() {
		this.newQuestionFormDiv.innerHTML = ""
		const new_prime_comment_button = document.createElement('button')
		new_prime_comment_button.id = "new-question-button"
		new_prime_comment_button.className = "btn btn-primary"
		new_prime_comment_button.innerText = "Ask a New Question"
		const linebreak = document.createElement('br')
		new_prime_comment_button.appendChild(linebreak)
		this.newQuestionFormDiv.append(new_prime_comment_button)
		new_prime_comment_button.addEventListener("click", this.adapter.renderNewQuestionForm.bind(this.adapter))
	}
```

Notice how the scope of *this* now points to an instance of the Primary Comments Adapter.  That is the result we wanted since we are calling a function in that scope and we would like to access that object's properties.  By making this modification, we can now reference the newly created instance of `Primary Comments Adapter` from within the `renderNewQuestionForm` method using the *this* paramter after we have activated the Event Listener tied to the button.

These are just a few of the applications where we can utilize *this* and `bind()` in JavaScript.  They are both very useful parameters that offer us a lot of functionality in our code.





