---
layout: post
title:      "Split Panes, Test Files, and Understanding Object Relationships"
date:       2018-01-01 19:48:09 -0500
permalink:  split_panes_test_files_and_understanding_object_relationships
---


Ever since I started the section on Object Oriented Ruby, I'd been struggling with understanding object relationships. Looking back on it as I'm about to start my CLI Data Gem project, if you're reading this and are in a similar position, I implore you to take heart and keep at it because there is light at the end of the confusion ridden tunnel. A few tips:

**1. Take notes on the lessons and lecture videos.**
I received this tip from my program mentor when I asked her for ways I can absorb the information I'll be learning. I have to say, it is immensely helpful. It's true what they say that you're made aware of the gaps in your knowledge once you have to verbalize it, or explain it, in some way. In teaching, we call this declarative knowledge, whereas procedural knowledge would be knowing *how* to do something without necessarily being able to describe how or why it works. I strive to be comprehensively knowledgeable about programming, so notetaking has been really helpful in formulating in my own sentences why the concepts I've learned work the way they do.

**2. Read your test files.**
Having my test files side by side with my code was one of the most helpful tools I used to get a grasp on what my code was supposed to be doing. It seems so simple in hindsight, this idea that the instance methods we construct in labs can only be called on objects that are instances of that same class, but it took time for that to really make sense to me.

Following the artist and song example in the lessons, here is what I've gathered:
<script src="https://gist.github.com/rh24/b03aa742bc095277d3956b0e22f77c45.js"></script>

I can only chain methods like this if I'm passing in instances, not strings.

What really solidified this concept for me was dissecting my test file and drawing primitive diagrams in which I was trying to understand what objects were being passed in where and why I can call instance methods of a separate class inside of instance methods of another class. It's through the relationships created between the classes.

For example, below is a screenshot of the `ruby-objects-has-many-lab-v-000` in which I try to break down the `add_song` method.

[Imgur](https://i.imgur.com/te3sZ25.png)

The test in the lefthand pane creates a song instance `hello = Song.new("Hello")` and passes it in to `add_song(song)` which is being called on an `Artist` instance `adele`. Now, one way in which we create a relationship between `Song` and `Artist` classes is by setting `song.artist =` to `self`, which in this specific test would be

`hello.artist = adele`

We can call `artist` in our `Artist` class's instance method `add_song(song)` because we set up our `Song` class with the `attr_accessor :artist`. In other words, `artist=()` is an instance method of the `Song` class. Since `hello` *is* an instance of the song class, calling `artist=()` is possible! On the other hand, if I wanted to call `adele.artist=` I would receive a NoMethod error since `adele` is an instance of the `Artist` class and does not have access to the `Song` class instance methods.

**3. Don't give up!**

At this moment in time, I look back on some of my code and wonder what I was thinking. Sometimes, I question how much I've really absorbed. I'm always wondering if there are better ways to write things, but at the end of the day, no matter how much lack I perceive, I'm still so grateful for whatever progress I've made thus far because it was only a week ago that the concept of object relationships felt very overwhelming. With the generous, patient help of the technical coaches and my peers, I feel like I'm a few steps closer to my goal of becoming proficient. So, I'll close with this friendly reminder that you most certainly can do it too! :)
