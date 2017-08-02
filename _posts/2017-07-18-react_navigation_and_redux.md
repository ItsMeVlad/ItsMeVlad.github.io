---
title: "Part 1: React-navigation and Redux"
excerpt: "The first part to a series dedicated on understanding how Redux and react-navigation work together."
layout: post
categories:
 - Development
tags:
 - react-native
 - react-navigation
 - redux
 - mobile development
author_profile: false
---
{: style="text-align: justify;"}
If you're first starting off with [react-native](https://facebook.github.io/react-native/)  it's probably best to get to good grips with Javascript and its fundamentals (npm, JSX, ES6 etc). It will save you plenty of time in wondering what certain things are, how and why they do what they do. But as you already know this is not an introductory post on react-native. We're going to try and keep things simple here, and attempt at delivering the idea rather than a purely technical breakdown.

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/react_native+redux.png" alt="">
</figure>

### React-navigation

There are many ways to accomplish navigation in react-native, however, as of [January 26th](https://github.com/react-community/react-navigation/commits/master?after=cc22138b1927a006b6e2a29c2b3dd38da736a164+314), the react-community released [react-navigation](https://reactnavigation.org/), aimed at providing:

> An extensible yet easy-to-use navigation solution.

Its meant to be used in favor of NavigationExperimental and replace Ex-Navigation (React Native's Navigator). So you're definitely on the right path when it comes to picking the right navigator for you react-native app.

React-navigation uses components called navigators to help you move from page to page or screens react-navigation like to call it. I'll go into what exactly navigators are shortly, but for the time just remember that there are different types of navigators, depending on what kinds of navigation you want. Okay, now that that's out of the way let's talk about what navigators are by the use of an example.

The most basic navigator is the [StackNavigator](https://reactnavigation.org/docs/navigators/stack), think of it like a stack of cards, or a memory stack if you prefer (if you don't know what a memory stack is, don't worry, it's not relevant to this post or topic). In a StackNavigator, each page is put onto the stack like a card being added to a deck, and later discarded from the deck when it's no longer needed.

In order for a screen to be represented, it has to be part of a navigator, what this means is that even if you have only two pages, they have to be part of a navigator in order for you to access them and therefore present them to the user.

You can also embed navigators within themselves, this comes really handy when you have something like a tabbed application, where each tab is a different page. Here, you would make each tab a StackNavigator, which contains multiple screens. That way, you can move to different pages from the initial tab that you started from.

In order to not get too overwhelmed with what navigators are, and their different types, we've covered them in an example below.

We'll start with our root component, this is the component we start the application with.

<script src="https://gist.github.com/ItsMeVlad/54b955b1ca42a86c125bb9761108e4ed.js"></script>

As we can see it's quite basic, and only contains a single view, if you were to run this, you would see a single page with the text "This is just a page", quick, easy, simple.

Now we go forward and create a 'route.js' file, which will contain our navigators, more on how to add these to our app further down this post. The route.js file contains a single navigator, called a [TabNavigator](https://reactnavigation.org/docs/navigators/tab), which is empty for the time being.

<script src="https://gist.github.com/ItsMeVlad/route.js"></script>

To include this tab navigator in our application, we will import it in our index.ios.js file and use it to replace the view we currently have. When you run this, you will now get an empty page, which makes sense since we've removed the View and added an empty navigator.

<script src="https://gist.github.com/ItsMeVlad/index.ios.js"></script>

As previously mentioned, in order for us to show different tabs, we need to show individual screens, and since every screen needs to be part of a navigator we will create two separate navigators. For our purposes, the StackNavigator is the ideal candidate. So we create two StackNavigators and nest them within our TabNavigator. If you refresh the page now, you will see two tabs (Yay!).

<script src="https://gist.github.com/ItsMeVlad/route.js"></script>

Now to the fun part, we need to add content to these screens, to do this we need to reference components that have something to show us, so, we will create two files that contain a View similar to the one we saw before we created and added the TabNavigator, let's call these, the tabOneScreen and tabTwoScreen. We will then import these in our route.js and add them to our StackNavigator tabOne and tabTwo respectively.

<script src="https://gist.github.com/ItsMeVlad/tabOneScreen.js"></script>
<script src="https://gist.github.com/ItsMeVlad/tabTwoScreen.js"></script>
<script src="https://gist.github.com/ItsMeVlad/route.js"></script>

Now, if everything was built and connected correctly, we should get a tabbed application with two tabs, where each tab shows us the text "This is tab one" and "This is tab two" respectively.

### Redux

In its basic sense [Redux](http://redux.js.org/) is a way for you to manage your applications [state](https://facebook.github.io/react-native/docs/state.html). What is state? State is the 'state' of the current data tied to your component. Nice and sweet but this is something best explained with an example.

If you have a component used as a screen, and it has a button, for us to know whether the button has been pressed or not, we need to know the 'state' of the button. In react-native, this is controlled by having an object represent the state of said button

{% highlight react %}
  this.state = { buttonSelectedStatus: false }; // The object called 'state' containing a 'buttonSelectedStatus' property.
{% endhighlight %}

Now, when we select the button, we're going to change the button pressed status by calling

{% highlight react %}
  this.setState({ buttonSelectedStatus: true }); // setState changes the value of our state, here we change
                                                 // 'buttonSelectedStatus' to true.
{% endhighlight %}

Important: Every time you change the 'state' of your state object, the component re-renders.

How is this useful? Here's some half-pseudo code to put it all together (start with the constructor then the render method).

<script src="https://gist.github.com/ItsMeVlad/54b955b1ca42a86c125bb9761108e4ed.js"></script>

With the above code, we have changed the text of our component by changing the state, when a button was pressed.


Now, enough with background info, and onto the real stuff, Redux! As we saw above, state allows you to change the 'state' of your component, what Redux allows you to do is hold the entire state of your application in a 'state' object. What this means is that you no longer have a state meant only for your component, but a central application-wide state, which your components access individually. How is this helpful? Well, having a central state means that you can transfer information between components. It also means that you can re-render components based on external input, that is extremely beneficial when you work with external APIs or databases.
