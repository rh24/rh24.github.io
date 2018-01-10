---
layout: post
title:      "cryptie-cli-gem"
date:       2018-01-10 17:49:57 -0500
permalink:  cryptie-cli-gem
---


Finally, it's come the time to code something from scratch!

I think I can speak for most Learn.co students when I say that there's something oddly comforting about coding in labs. Test Driven Development still blows my mind, and the one thing I really wish we had more practice on in the curriculum is writing in depth tests for our own programs. I've seen Avi do it many times in the OO Lecture videos, and I've taken a *tiny* stab at it in my own CLI Data Gem project. However, nothing I've written comes close to the extensive testing going on in the curriculum labs.

Tests point students in the right direction on how to get started. Each new error exposes a break in the code that can then be addressed systematically. Especially in object orientation labs, it's already outlined for us the expectations of each objects, and the classes themselves have been predefined.

So, I have to say, the second most daunting part of this project was coming up with my own classes. In the end, I'm not sure why I dreaded it to beging with because once I wrote out what I'd hoped my program would do, it was pretty simple to come up with a handful of classes.

Moving past the speculative difficulties, the real challenge lied in trying to figure out which methods belonged where. I tested out a variety of solutions, changed methods from being instance methods to class methods and vice versa. As my program become larger and as I extended it's functionality from simply being one that exposes information scraped from a wesbite, I found it difficult to call instance methods of the CLI class in my other classes. I wanted my program to have the option of breaking out and returning to the main menu at any point in the program, no matter which class it was drawing from.

Eventually, I settled on saving the `Cryptie::CLI.new` to an empty array `@@all = []` in order to refer to the first instance and actually call methods on it. If I hadn't specifically created and saved instances of that class in order to actually refer to them outside of the class, I'm not sure how I could have architected my objects in order to provide that functionality. Granted, there's still a lot I have left to learn about streamlining my solutions and structing my program in ways that make the most sense, but overall, I'm pleased with my experience. This was really like learning by being thrown into the deep end, and I had so much fun with it once the program really started coming together in my terminal.

What probably added to the excitement for me was that the data was being scraped from a live site, so depending on the times I exited and re-started the program, my output would differ. Additionally, the `order` function in my program is actually something I have to check out sites like CoinGecko for. The process of navigating to the site, then to each coin, and then to the calculator feels long and drawn out. Typing input straight into my terminal is a process I find much less clunky. It feels good to create something in which I find practical use, even if it might only be me enjoying `cryptie` when all is said and done.

**Tips:**

***1. DO NOT forget to commit everything to your github repo as the project outline indicates--early and often.***

I ran into an unsettling issue with my project when I was much further along and stood much more to lose had I not already pushed my last working code up to my github repo. I ended up having to scrap a lot of what I wrote after my last commit because I was working in an old draft that had more breaks in it than was worth fixing. I settled on copy and pasting my most updated version and extending my code from there, but the whole situation would have been better avoided completely.

When I get carried away, I can be a bit of a disorganized directory, tab, and atom window hoarder. Having all these similar looking things open caused me to work in the wrong local directory. Turns out, I had two projects by the same exact name located in different directories on my hard drive. I noticed something was off when I tried to `git push` but did not see it update in my repo.

**Note to self:** please delete stale directories and duplicate repositories, so you're not making this mistake again.


***2. Watch the entire lesson video on how to set up the CLI gem before you start.***

Perhaps, similar to my self noted disorganization comment, this is something that only needs to be suggested to the silly, excitement fueled student like me, but please, watch the whole video of Avi setting up his `DailyDeal` gem before you begin to code along. He plays around with his code and thinks out loud, which is really cool because it's *always* cool observing someone's thought process in action. Unfortunately, this also means that if you're pausing the video and coding the same things as he is, you'll also run into the myriad of errors along the way.  This can be a little frustrating, especially if your immediate reaction is to dive headfirst into debugging rather than continuing along with the video, trusting that it will explain what to do next. It will save you a lot of headache in trying to figure out how to require what in your program if you wait until Avi reaches a working way to do it. Meanwhile, as you watch, soak in the information and digest it. Then, try it on your own. Personally, I spent too much time being bogged down by `require_relative` errors at the very start of my project.


**Concluding thoughts:**

I do want to be clearer on how to set up my gem, require files, and add my gem specs. I want to understand what's really going on there instead of simply knowing how to make it work and be able to explain it. I feel like this is fundamental knowledge I should possess in order to continue building things from scratch.

I'm also looking forward to receiving guidance through constructive sessions of code reviewing and pairing. Instructors and peers are there as a resource. I'm always curious to know how someone else might solve the same problem. Being so early on in my learning journey, I can only benefit from this curiosity.
