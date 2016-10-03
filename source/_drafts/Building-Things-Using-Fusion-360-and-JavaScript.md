---
title: Building Things Using Fusion 360 and JavaScript
tags: []
date: 2016-10-02 16:03:46
---

I like making things.

I used to mostly just make things that show up on the computer screen - software things.&nbsp;<span style="line-height: 1.6em;">Lately, however, I&#39;ve been re-inspired to make _real _things. Things out of wood and things out of plastic and metal and fabric and string.</span>

The way I see it, we design things either _manually_ or _generatively_.

By **manual**&nbsp;I mean that I conceive an idea then design and build it step by step. I - the human - am involved every step of the process. Imperative code is manual. Here&#39;s some pseudocode to describe what I&#39;m talking about...

<span style="color:#008000;">`<span style="line-height: 1.6em;">// step 1

// step 2

// if step 2 value is good then step 3

// else step 4 10 times</span>`</span>

<span style="line-height: 1.6em;">See what I mean?</span>

<span style="line-height: 1.6em;">I&#39;m not arguing that this sort of code and likewise this sort of technique for building is not&nbsp;_essential_. It is. I am, however, going to propose that it&#39;s often not altogether exciting or inspiring. The reason, IMO, is that the entire process is no greater than the individual or organization that implements it. An individual only has so many hours in the day and is even limited in ideas. An organization can grow rather large and put far more time and effort into a problem and obviously generate more extensive results. But the results are always linearly related to the effort input - not so exciting.</span>

<div><span style="line-height: 1.6em;">By </span>**generative**<span style="line-height: 1.6em;"> I mean that instead of creating a </span>_thing_<span style="line-height: 1.6em;">, I create </span>_rules _<span style="line-height: 1.6em;">to make a thing. The rules may be non-deterministic and the results completely unexpected - even from one run to another. The results often end up looking very much like what we find in nature - the fractal patterns in leaves, the propagation of waves on the water, or the absolute beauty of ice crystals up close.</span></div>

<span style="line-height: 1.6em;">What&#39;s exciting is when an individual or organization puts their time and effort into defining&nbsp;</span>_rules_<span style="line-height: 1.6em;">&nbsp;instead of defining&nbsp;</span>_steps_<span style="line-height: 1.6em;">. That is, after all, the way our own brains work, and in fact, that&#39;s the way the rest of nature works too. It&#39;s amazing and awesome and I would venture to say it&#39;s even miraculous.</span>

<span style="line-height: 1.6em;">I think a lot of my ideas on the matter parallel and perhaps stem from Stephen Wolfram&#39;s book _A New Kind of Science_.</span>

![](http://codefoster.blob.core.windows.net/site/image/33da539b377c4de3a701f01568b036cd/new-kind-of-science_1.png)

Most of the book is about _cellular automata_. The simple way to understand these guys is to think back to [Conway&#39;s Game of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life). The game is basically a grid of cells that each have a finite number of states - often times two states: black and white. Initially, the cells in the grid are seeded with a value and then iterations are put into place that may change the state of the cells according to some rules.

The result is way more interesting than the explanation. The cell grid appears to come to life. The fascinating part is that the behavior of the system_&nbsp;_is usually not what the author intended - it&#39;s something emergent. The creator is responsible for a) creating an initial state and b) creating some rules. The system handles the rest. It usually takes a lot of trial and error if the intention is to create something that serves some certain purpose.

Check out this system I found on Wikipedia&#39;s [page on cellular automata](https://en.wikipedia.org/wiki/Cellular_automaton) called [Gosper&#39;s](https://en.wikipedia.org/wiki/Bill_Gosper) [Glider Gun](https://en.wikipedia.org/wiki/Gun_(cellular_automaton)). It&#39;s creating _[gliders](https://en.wikipedia.org/wiki/Glider_(Conway%27s_Life))._&nbsp;

[![](//upload.wikimedia.org/wikipedia/commons/e/e5/Gospers_glider_gun.gif)](/wiki/File:Gospers_glider_gun.gif)

I don&#39;t know about you, but I find that completely awesome.

## Okay, so when are you going to get to the point of the blog post, codefoster?

Calm down. It&#39;s called&nbsp;_build up_. :)

First, let me say that generating graphics in either 2D or 3D is nothing conceptually new. What I like about discovering and learning an API for CAD software, though, is that I can not only generate something that targets the screen, I can generate something that targets the 3D printer or the laser cutter. That&#39;s all sorts of awesome!

The example I&#39;m going to show you now is a simple one that I hope will just get your gears turning. You could, by the way, take that literally and generate some gears and get them turning.

If you don&#39;t have Fusion 360, go to [fusion360.autodesk.com](http://fusion360.autodesk.com) and download it. If you&#39;re a hobbyist, maker, student, startup type you can get it for free.

If you&#39;re new to the program, let me suggest [the learning material](http://www.autodesk.com/products/fusion-360/learn-training-tutorials) on their website. It&#39;s great.

After you install Fusion 360, the first thing you need to do is launch the program. This API is attended. It requires that you open the program and launch the scripts. I have suggested to the team at Autodesk to research and consider implementing unattended scenarios as well.

Now launch the _Scripts and Add-ins..._ option from the _File _menu...

![](http://codefoster.blob.core.windows.net/site/image/1ffd3f43cba242db891da8eded8d10f7/f360api_scripts-menu_1.png)

Don&#39;t be confused by the _Add-Ins (Legacy)_ option in the same _File _menu. That&#39;s for an old system that you don&#39;t want to use anymore.

That should launch the Scripts and Add-Ins dialog...

![](http://codefoster.blob.core.windows.net/site/image/a6ba9b095b4d440fbbbd4d40275aa84c/f360api_scripts-window_1.png)

There are two tabs - Scripts and Add-Ins. They&#39;re the same thing except that Add-Ins can be run automatically when Fusion 360 starts and can provide commands that the user can see in their UI and invoke by hitting buttons. Add-Ins ask you to implement an interface of methods that get called at certain times. If you simply click the _Create _button on the Add-Ins page, it will make you a sample with most of that worked out for you already.

Let&#39;s focus on the Scripts tab for now.

You&#39;ll see a number of sample scripts in there. Some of them will have the JavaScript icon (![](http://codefoster.blob.core.windows.net/site/image/f6ad5fb7a3e547209b614101575bc996/f360api_js-icon_1.png)) and others will have the Python icon (![](http://codefoster.blob.core.windows.net/site/image/679a6ed7208e465b86723fb7fd162279/f360api_py-icon_1.png)).

The Fusion 360 API supports 3 languages: C++, Python, and JavaScript.&nbsp;

Above those, you&#39;ll see the My Scripts area that contains any scripts you have written or imported.

It&#39;s not entirely clear at first how this works. Let me explain. If you click Create at the bottom, you&#39;ll get a new script written in a strange folder location. It&#39;s good because it gives you the right files (a .js file, an .html file, and a .manifest), but it&#39;s bad because it&#39;s in such an awkward location. The best thing to do in my opinion is to hit create and get the sample code files and then move the files and their containing folder to wherever you keep your code. Then you can hit the little green plus and add code from wherever you want.

One more nuance of this dialog is that if you click the Edit button, Fusion 360 will launch an IDE of _its_&nbsp;choice. I think this is weird and should be configurable. If I edit a JavaScript file it launches Brackets. I don&#39;t use Brackets. I use Visual Studio Code. It doesn&#39;t end up being that much trouble, but it&#39;s weird.

To edit my code, I just go to my command line to whatever directory I decided to put it in and I type...

`code .`

That launches Code with this directory as the root. Here&#39;s what I see...

![](http://codefoster.blob.core.windows.net/site/image/5be21a4edfa3477cb8cad8379120cb91/f360api_code_1.png)

There you can see the .html, .js, and .manifest files.

I&#39;m not going to take the screen real estate to walk you entirely through the code. You can see it all [on GitHub](http://github.com/codefoster/f360-bumpmap). But I&#39;ll attempt to show you what it&#39;s doing at a high level.

Here&#39;s the code...

<style type="text/css">.gist {width:700px !important;}
  .gist-file
  .gist-data {max-height: 500px;max-width: 700px;}
</style>
<script src="https://gist.github.com/codefoster/0b24212710319b681453.js"></script>

Let&#39;s break that down some.

The `createNewComponent` function is just something I made. That&#39;s not a special function the API is expecting or anything. The `run `function&nbsp;_is_, however, a special function. That&#39;s the entry point.

Essentially, I&#39;m creating a 20x20 grid, prompting the user to select a body, and then doing a 2D loop to copy the selected body. The position is all done using a transformation that shifts each body into place and then offsets it a certain amount in the Z direction. In this case, I&#39;m just using a random number, but I could very well be feeding data in to this and doing something with more meaning.

Watch this short video as I create a cube and then invoke this script on it...

<video autoplay="" controls="" src="http://codefoster.com/bcms-media/Files/Download?id=823ecf8b-e3eb-46a3-b61a-a5d4003e9243" style="width:700px;height:486px">&nbsp;</video>

So, here is where you just have to sit back and stare at the ceiling and think about what&#39;s possible - about all the things you could generate with code.

My example was a basic, linear iterator. Perhaps, however, you want to create something more organic - more generative?

Check out [this example](https://www.youtube.com/watch?v=5wj6zj4-iB0)&nbsp;by Autodesk&#39;s own Mike Aubry ([@Michael_Aubry](https://twitter.com/Michael_Aubry)) where he uses Python code to persuade Fusion 360 to build a spiral using the API.

![](http://codefoster.blob.core.windows.net/site/image/b8bec32df2594f06b683961c9fbf7ec2/f360api_spiral_1.png)

That has a bit more polish than my gray cubes!

If you build something, make sure you toss a picture my way [on Twitter](http://twitter.com/codefoster) or something. I&#39;d love to see it.