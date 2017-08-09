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

In order to not get too overwhelmed with what navigators are, and their different types, we've covered them in an example below. Bare in mind that for simplicity all the files are created in the root of the project, we'll change this further on.

We'll start with our root component, the 'index.ios.js' file, this is the component we start the application with.

<script src="https://gist.github.com/ItsMeVlad/3d8fa2c81a20c29c87fe4046c178f383.js"></script>

As we can see it's quite basic, and only contains a single view, if you were to run this, you would see a single page with the text "This is just a page", quick, easy, simple.

Now we go forward and create a 'router.js' file, which will contain our navigators, more on how to add these to our app further down this post. The 'router.js' file contains a single navigator, called a [TabNavigator](https://reactnavigation.org/docs/navigators/tab), which is empty for the time being.

<script src="https://gist.github.com/ItsMeVlad/a3e5086335f3c55849b19bbb5fa0388e.js"></script>

To include this tab navigator in our application, we will import it in our 'index.ios.js' file and use it to replace the view we currently have.

<script src="https://gist.github.com/ItsMeVlad/1d146d97f02788710e52f0c85e9ba900.js"></script>

When you run this, you will now get an error, telling you that you can't have an empty navigator, which makes sense since we've removed the View and added an empty navigator. Let's fix this by updating our 'router.js' file.
As previously mentioned, in order for us to show different tabs, we need to show individual screens, and since every screen needs to be part of a navigator we will create two separate navigators. For our purposes, the StackNavigator is the ideal candidate. So we create two StackNavigators and nest them within our TabNavigator.

<script src="https://gist.github.com/ItsMeVlad/9d6ae66d42703dad8f8f790ba6bd6e1b.js"></script>

However, if you refresh your app now, you will get a new error, complaining that it can't find the variable 'SecondTab'. This is because we've assigned screens to our navigators, but these screens don't exist. Let's create them and fill them in with a View component and some text.

<script src="https://gist.github.com/ItsMeVlad/5ee44a8bade47926f53373f28ffa0119.js"></script>
<script src="https://gist.github.com/ItsMeVlad/19c9e64483ba86463bbc169aa6bf9b5f.js"></script>

And finally, include them in our 'router.js' file.

<script src="https://gist.github.com/ItsMeVlad/71b6fd8fe3f3bf4a6cf0b6e1108849d1.js"></script>

Now, if everything was built and connected correctly, we should get a tabbed application with two tabs, where each tab shows us the text "This is the FirstTab." and "This is the SecondTab." respectively.

As you can see, we've created two _individual_ screens and added them to their *own* StackNavigators, then we add these StackNavigators as screens to our TabNavigator. Allowing us to navigate between them.

If you'd like to see the app in all it's glory, you can find it [here](https://github.com/ItsMeVlad/react-native-examples/tree/master/react-navigation).

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

Redux is based on something called the [redux flow](http://redux.js.org/docs/basics/DataFlow.html), which comes from Facebook's [Flux](https://github.com/facebook/flux). There are three main components to using redux, these are Actions, Components and Reducers. Components are just what we think they are, except in redux terms they're called 'smart' components. Smart components are simply components linked to redux.

The general flow in redux is that a component triggers an action, which potentially takes some data and passes it to a reducer, which accomplishes some sort of transformation (a techy way of saying we take the data and change it in some way) and then causes the component that triggered the action to re-render using the new data passed to it by the redux store. Now, on a technical level this may not be exactly 1-1, but for the purpose of this explanation, this is the way I understood when first learning how to use redux.

To keep things simple I am going to use the same files from our react-navigation section. However, there are some small changes to take into account. Specifically the folder structure.

As you've probably figured by now, I like examples, but in order to keep things in line with best practices, we've taken the code from 'index.ios.js' and moved it to a separate file called 'App.js' within a 'Container' directory which is inside an 'App' directory, and then placed FirstTab.js and SecondTab.js alongside it. Then, we've moved the 'router.js' file to a 'Config' folder, also inside our 'App' directory. Here is the breakdown below:

```
- react_navigation_and_redux
  - App
    - Config
      - router.js
    -Containers
      - App.js
      - FirstTab.js
      - SecondTab.js
  - index.ios.js
  - index.android.js
  - package.json
  ... other files
```

Bare in mind that since we call the router.js file in our App.js file, we need to change the import to be '../Config/router.js', as shown below.

<script src="https://gist.github.com/ItsMeVlad/991af464382e5c3931869fdb719d40b3.js"></script>

Now that that's out of the way, let's create our redux store, unfortunately, we have to first build a redux implementation in order for it to make sense when we explain it the end, so don't worry if you don't know what reducers, actions, or a redux store are yet.

We build our reducer by creating a 'Reducers' folder, and then creating a 'dummyReducer.js' file. Followed by a 'index.js' file where we import the 'dummyReducer'. The result of our 'index.js' file serves as our redux store, it combines all our various reducers together to create the store.

<script src="https://gist.github.com/ItsMeVlad/f07b04ed7fe790f0ac13f0b861ed1257.js"></script>
<script src="https://gist.github.com/ItsMeVlad/80a5b2d55f4f9906916474614745ed48.js"></script>

To make our lives easier we've created a helper function called 'createReducer' in a 'createReducer.js' file and placed it in a new folder called 'Lib'.

<script src="https://gist.github.com/ItsMeVlad/8aeba03559c2b304dceba376fceea593.js"></script>

Finally, let's create some action types, in a new 'Actions' folder. Action types help use figure out which actions to dispatch, which in turn fire off the respective reducer. For our basic example we'll only create one type.

<script src="https://gist.github.com/ItsMeVlad/3e6f2f730b421010904ec1b9e0e93106.js"></script>

Now in our 'App.js' file we will import our redux store and pass it to our main component. Since we're passing the store to our main component, all of the child components also have access to it (This is very important!).

<script src="https://gist.github.com/ItsMeVlad/f15f91dc5f6f1bbb73b304d572484d33.js"></script>

Now we will use our previous components as well, and turn them from 'dumb' components (components that don't use redux) to 'smart' components (components that do use redux). We do this by using the 'connect' function from the react-redux npm package, which connects our component to redux. Boom! it's now a smart component.

<script src="https://gist.github.com/ItsMeVlad/56349c97f9353797aaff40a19417e167.js"></script>

We will do the same for our second component.

<script src="https://gist.github.com/ItsMeVlad/406ae29d7fd8b1439dd4e280522da228.js"></script>

If you run the application now, you should get the same result you had before, except, you now have an application connected to redux (in a very basic sense). We will go into actually using redux in Part 2 of the series.

For now, you can find our new redux infused app [here](https://github.com/ItsMeVlad/react-native-examples/tree/master/react_navigation_and_redux_v1).
