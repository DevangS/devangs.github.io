---
layout: post
title: When Duck Typing Goes Wrong
category: programming 
---

What Do We Want 
=============== 
Duck typing is the idea looking at the object
based on its properties such as the types of the variables and methods it
contains, instead of looking at an object based on what class it inherits from.

Why Do We Want It
===============
Here's an example in Scala to demonstrate this, though it should be noted 
that technically Scala has _Structural Typing_ which is a more robust version 
of duck typing, but the principle is the same. This should print out 
"I am a man" if a Male is passed in, and "I am a woman" if a Female is 
passed in. 

    object Male {
    	def speak(value: String) = {
          "I am a man"
        }
    }

    object Female {
    	def speak(value: String) = {
    	  "I am a woman"
    	}
    }

Now we can define a method makeThemTalk that will accept anything that
matches the description above:

    def makeThemTalk(human: {def speak(value: String): String}) {
        println(human.speak())
    }

There are several reasons why this is helpful - for one the code is cleaner,
and more importantly, we can now pass in any object that has a speak method
which takes in a String, and returns a String. There's no need to keep track
of allowable object types nor is there a need to take an object oriented
approach by making each object inherit from some superclass and then 
checking for the existence of such a relationship. This can quickly become
dangerous however if one is not careful. 

Why We Shouldn't Want It
======================== 
Let's say I'm working on some software that allows the police to arrest someone
with the click of a button. So I write an object and method like this:

    object BadGuy {
      private var name = "Danny Ocean"
      def getName() = {
        name
      }
    }

    def arrest(person: { def getName(): String}) {
      findAndTerminate(person)
    }

This seems all fine and dandy, and indeed it works, but we must be careful.
Let's say we wanted to expand our application so that we could look up
the information for regular people as well. So we write: 

    object GoodGuy {
      private var name = "Danny Doogooder"
      def getName() = {
        name
      }
    }

    def getProfile(person: { def getName(): String}) {
      findProfile(person)
    }


The problem here is that from the viewpoint of the arrest method, GoodGuy and
BadGuy are structually equivalent. This means that it is just as easy to pass
in a GoodGuy object to arrest, as it is to pass in a BadGuy. Using duck typing
does away with the clear separation between objects that you get with using
something like inheritance. As a result, it is easy to make a mistake like
the above which can result in potential security issues by being able to
pass objects to methods that you really do not want to be able to pass in.