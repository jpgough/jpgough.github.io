---
layout: post
title: "Getting Started: Introduce Coding Using Swift Playgrounds on iPad"
date: 2017-05-18
---

[Swift Playgrounds](https://www.apple.com/uk/swift/playgrounds/) is an application for the iPad built by Apple to help developers learn how to write Swift applications.

>Swift Playgrounds is a revolutionary new app for iPad that makes learning Swift interactive and fun. 
Solve puzzles to master the basics using Swift — a powerful programming language created by Apple and used by the pros to 
build many of today’s most popular apps. [https://www.apple.com/uk/swift/playgrounds/]

Although the application is designed primarily for developers, it can also be used to help engage children in coding. 
For younger children, this does require adult supervision to help with explaining what the function does.

The session is designed for younger coders, for children aged 6 and upward it might be better to try the [Hour of Code](https://www.apple.com/retail/code/hourofcode_guide.pdf)
session created by Apple.

## Getting Started

### Session Time

* Around 30 minutes

### What you need

* An iPad, probably Air 2 or newer. 
The graphics that render are quite cool, but require some power. 
I haven't tried it on an older device, but it seems fine on the Air 2
* [Download Swift Playgrounds](https://itunes.apple.com/gb/app/swift-playgrounds/id908519492?mt=8) from the Apple App Store
* Download Learn to Code 1 as an in App download form the Swift Playgrounds app

![Swift Playgrounds Shop](/assets/images/blog/playgrounds/install-playgrounds.png)

### Preparation 

* It is worthwhile taking a look around the app and getting familiar with how it works. 
You don't really need any significant programming experience. 
The session will focus on moving an alien around the screen to pick up gems and activate switches.
* For younger children the trainer will need to help with reading skills and choosing the correct function. 
The functions are `moveForward()`, `turnLeft()`, `collectGem()`, `toggleSwitch()`. 
If you haven't come across the term function before, just think of it as an action the alien will perform.
* This session is really just a game, there's no programming loops just yet. These are for a different session
* You will probably find you can run this same session many times, children really like the interaction and ease of controlling the alien


### Controls & Leading Play

* Controls are as simple as pressing the name of the functions (actions) that you need. 
To go forward twice pick the `moveForward()` function twice. 
Think of it like playing a step-by-step platform game.

* If the child says they're not sure what's next just run the program. 
The alien finishes up at the last instruction and then you can correct. 
**Allow the child to make mistakes**, this is really important and it's OK to not have the correct solution first time. 
They'll struggle later if this isn't done right from the beginning as the problems become more complex.

* Let them hold the iPad and control the pressing. 
You might want to have a small session where you show them first, but they will get more from being the one pressing the buttons.

![Swift Playgrounds Shop](/assets/images/blog/playgrounds/finding-bugs.png)


The screen has a description and extra instructions of what needs to be done. The code is the list of instructions at the bottom left. Run my code/stop is at the bottom of the screen. 

### Lessons to Try

For a thirty minute session the following are probably enough to get started. 
I've also outlined some things to discuss and think about each lesson and a sample solution.

* Issuing Commands
* Adding a New Command
* Toggling a Switch
* Portal Practice
* Finding and Fixing Bugs
* Bug Squash Practice
* The Shortest Route
Below are some sample solutions that we came up with:

```swift
//Issuing Commands
moveForward()
moveForward()
moveForward()
collectGem()

//Adding a New Command
moveForward()
moveForward()
turnLeft()
moveForward()
moveForward()
collectGem()

//Toggling a Switch
moveForward()
moveForward()
turnLeft()
moveForward()
collectGem()
moveForward()
turnLeft()
moveForward()
moveForward()
toggleSwitch()

//Portal Practice
moveForward()
moveForward()
moveForward()
turnLeft()
moveForward()
moveForward()
toggleSwitch()
moveForward()
moveForward()
turnLeft()
moveForward()
moveForward()
collectGem()

//Finding and Fixing Bugs
//Run through the code and find where the alien performs the wrong action and correct
moveForward()
moveForward()
turnLeft()
moveForward()
collectGem()
moveForward()
toggleSwitch()

//Bug Squash Practice
//Run through the code and find where the alien performs the wrong action and correct
moveForward()
turnLeft()
moveForward()
moveForward()
toggleSwitch()
moveForward()
moveForward()
moveForward()
moveForward()
collectGem()

//The Shortest Route
moveForward()
moveForward()
moveForward()
collectGem()
moveForward()
moveForward()
moveForward()
moveForward()
toggleSwitch()
```