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

### Redux

In its basic sense [Redux](http://redux.js.org/) is a way for you to manage your applications [state](https://facebook.github.io/react-native/docs/state.html). What is state? State is the 'state' of the current data tied to your component. This is something best explained with an example.

If you have a page (or component, in react-native speak), and it has a button, for us to know whether the button has been pressed or not, we need to know the 'state' of the button. In react-native, this is controlled by having an object represent the state of said button

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


Now, enough with background info, and onto the real stuff, Redux! As we saw above, state allows you to change the 'state' of your component, what Redux allows you to do is hold the entire state of your application in a 'state' object. What this means is that you no longer have a state meant only for your component, but a central state, which your components access individually. How is this helpful? Well, having a central state means that you can transfer information between components. It also means that you can re-render components based on external input, that is extremely beneficial when you work with external APIs or databases.
