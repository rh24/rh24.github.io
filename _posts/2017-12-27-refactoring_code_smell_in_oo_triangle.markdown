---
layout: post
title:      " Refactoring Code Smell in OO Triangle"
date:       2017-12-27 21:05:36 -0500
permalink:  refactoring_code_smell_in_oo_triangle
---


I'll be focusing on my initial solution as compared to the solution code found in the learn.co repo for the OO Triangle lab. I've been wondering what to write my next blog post on, mostly because maintaining a technical blog is something that is so foreign to me. My knowledge of blogging only goes as far as angsty middle school Xanga posts and, more recently, a Tumblr account that fills up with reposts from my dashboard of embedded pictures and the occasional existentialist quote. Writing about something I discovered while coding? I've yet to do that beyond pausing video lectures to take note my confusion in a neat little Notes folder in iCloud. Actually publishing those rambling thoughts? This will be my first time.


I have to say though, the stark contrast between my initial submission and the repo solution code was most certainly compelling enough to warrant a blog post. I had so much fun reading and breaking down the prettier solution to the lab, and I wanted to document my understanding of it.

![first solution](https://imgur.com/a/6fG4c)


Avi, in one of his lectures on Object Oriented Programming, talks about this idea of single responsibility principle (SRP) in programming as he prepares to refactor his code example.

“This is kind of ugly,” he says, referring to his code sample.

As I look at my first solution to the lab, my thoughts are all the same, and his words reverberate into my coding conscience. Time for  refactor!

So, what's code smell? And what is single responsibility programming?

The way I've understood it so far is that code smells when the solution may be working upon test, but it's certainly not the most maintainable in that it can be made more abstract, less repetitious, more readable, and extendable. It's precise, clear and succinct without sacrificing functionality. Single responsibility principle, as Avi [explains](https://www.youtube.com/watch?time_continue=962&v=oXwdOdBUyCI) (@15:58), is this idea that "every single thing you do in your code should only be doing one thing. One class should only represent one concept. One method should only respresent one procedure. One line of code should represent one implementation of logic."

In light of this information, I noticed right off the bat there was something hideously distasteful about the way I'd named by method arguments. I had explicitly named them `side_one`, `side_two`, and `side_three`, which seemed to make sense at the time, but this explicit naming convention actually makes less sense in retrospect. Pythagoras himself in his theorem uses the letter variables `a`, `b`, and `c` in his equation along with the illustrations to prove his point. In fact, most all geometric proofs utilize uppercase and lowercase letters. Refactoring my method arguments to reflect this sensibility was the first step in eliminating my code smell.

![refactor commit](https://imgur.com/a/LjT4t)

Next, I saw that the repo solution code does something that hadn't even crossed my mind reading the README of the lab. We're instructed to write an instance method `kind` that should confirm whether or not our side arguments can indeed be sides of a triangle based on the mathemetical principles that dictate the laws of triangles. My first thought was to write a massive `if` statement that would execute the following code block if the law of triangles proved true. However, This enormous piece of logic can absolutely be encapsulated by an entirely separate instance method! In this refactoring, we change call this method `validate_triangle`. That would actually be a better practice according to SRP.

Refactoring further, I admired how the repo solution elegantly created an array of 'switches' composed of true statements concerning the law of triangles, which states that the sum of any two sides of a triangle must be greater than the third side. In other words, `real_triangle = [(a + b > c), (a + c > b), (b + c > a)]` produces `real_triangle = [true, true, true]` if the sides do indeed make up a real triangle. A triangle is also false if the any of the sides are less than or equal to zero. So, the next step in our `validate_triangle` instance method would be to iterate over an array of our sides, `[a, b, c]`, and shovel in `false` if any of these sides is `<= 0`.

Now that we've gotten these two constraints down, it should be clearer that we do NOT have a real triangle if our `real_triangle` array contains any false element. So, last we use the `include?` method on our `real_triangle` array to test for any false elements, and we raise our custom `TriangleError`if so.

Closing thoughts: I really hope one day I get to a point where I can refactor my solutions more elegantly with less obvious hand holding, but until then, I will be a voracious reader of code that smells far better than mine. I'll get into the nitty gritty of refactoring, analyze my side by side commit comparision, and hope that one day my solutions will smell better by association.

