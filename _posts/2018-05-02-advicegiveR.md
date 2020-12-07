---
layout: post
title: "advicegiveR: Developing my first package!"
date: 2018-05-02
author: "Katie Jolly"
comments: true
<!-- categories: R markovchain poems -->
---

I passed another one of my R milestones this week: I built my first R package! It's simple, easy to maintain, fun, and a great way for me to start learning about package development. I'd been reading documentation for other packages for a while trying to learn the patterns and workflows and finally decided to give it a try.

I wanted to build a wrapper package for an API because I figured that it would be a nice way to create a relatively simple set of functions. (Provided that I picked a simple API.) I searched something along the lines of "fun APIs" on Google and found a few lists. I liked the [advice slip API](http://api.adviceslip.com/) and did a quick check to see if there were existing R packages for it. After not finding anything immediately I decided to go ahead with making my own! I decided to make a package that can get a piece of advice and then print it on a "poster".

Last week I helped organize an informal workshop on R package development. Another student who had been working on some packages had offered to lead a session! We talked about how packages don't have to be earth-shattering technological advancements-- they just need to be useful and unique. I've kept that conversation in the back of my head since then.

Setting up the package
----------------------

Before setting up any of the package files, I made some practice functions in my console to get a feel for the API and how it worked. It was also helpful to diagram with pen and paper how I wanted the functions in my package to work together and what each of them should produce. This would essentially become my documentation later, but it helped me immensely to have it all written out first.

I used the RStudio interface to set up the templates for all the necessary files & folders. Once I had those, I could figure out what to do for the most part, but I read through different sections of [R packages by Hadley Wickham](http://r-pkgs.had.co.nz/) over and over to understand what was actually happening.

Since the package would only have three functions, I decided to put them all in one R script. (I know this is frowned upon, but I figured it made sense in this case.) That made documentation fairly straightforward and helped me organize my thoughts.

I also set up a GitHub repository (with a working branch for making edits before adding them to the master branch) fairly early on to be continuously saving. It's a habit I picked up early on.

The code itself
---------------

I have three functions in my package.

* `get_advice(id)`

This can either grab a random piece of advice or a specific one (by giving an integer as the id parameter. Defaults to random)

* `load_image(image)`

From a pre-loaded list of images, pick one as the background for your "poster"! You can also load your own image to use with `magick::image_read()`. The only requirement is that you actually do use `magick` to load the image, the function will give you an error otherwise.

* `print_advice(image, advice)`

This is the function that brings it all together! Give it a background image and the advice you want printed and it will design the poster for you! It returns an image that you can save.

Demo the package!
-----------------

You can install `advicegiveR` with the code below.

``` r
#install.packages(devtools)
#devtools::install_github("katiejolly/advicegiveR")
library(advicegiveR)
```

First, we can just get a random advice string.

``` r
get_advice() # randomizes uses runif()
```

    ## The id you are using is 214

    ## API endpoint = http://api.adviceslip.com/advice/214

    ## [1] "Things are just things. Don't get too attached to them."

I've added a message feature that tells you what `id` the function ended up using so that you can find it again in the future :)

But if we know exactly which piece of advice we want, we can use an `id` instead.

``` r
advice <- get_advice(id = 14)
```

    ## The id you are using is 14

    ## API endpoint = http://api.adviceslip.com/advice/14

``` r
advice
```

    ## [1] "Life is better when you sing about bananas."

Then we can pick a background image to use. For this example I'll use `paper`. The other options are `ocean`, `mountain`, `sunset`, `clouds`, `sunset`, or `rainbow`. Eventually I'll add more!

``` r
paper <- load_image("paper")

paper
```

![](https://raw.githubusercontent.com/katiejolly/blog/master/assets/advicegiveR/paper.png)

Finally let's make the poster!

You can specify the text size and color if you so choose.

``` r
print_advice(image = paper, advice = advice, textcolor = "black")
```

![](https://raw.githubusercontent.com/katiejolly/blog/master/assets/advicegiveR/paper_annotated.png)

There are more examples (and source code) in the [GitHub repo](https://github.com/katiejolly/advicegiveR) for this package.

Concluding thoughts
-------------------

I was surprised at how seamless the package development process seemed (keeping in mind that I'd been reading about it for a while and created a simple package). There were certain things that got me stuck for quite a while, though. I had no idea what it meant to "export" a function or "not run" an example. I was able to find answers easily but many times I wasn't quite sure what my error was. Those are errors that I just had to see to learn and I don't think they'll cause me as much trouble in the future. I also was surprised at how happy I felt when I got `0 | 0 | 0` on my checks. Never would I have thought something like that would make me feel so satisfied with myself.

On the whole this was a very positive experience! I'm looking forward to learning more about packages and creating some more complex ones in the future. For now, if you want to suggest new background images or functions/features feel free to let me know via Twitter (@katiejolly6) or GitHub! Along the same lines, if you have any suggestions for how I could have improved the functions/workflow I'm open to feedback and excited to learn :)
