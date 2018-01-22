---
layout: post
title: "Random Rupi: Generating Rupi Kaur-style poems in R with Markov chains"
date: 2018-01-05
comments: true
<!-- categories: R markovchain poems -->
---

I was listening to the [Random Kanye](http://lineardigressions.com/episodes/2017/12/24/re-release-random-kanye) episode of Linear Digressions (100% recommend!) and wanted to try Markov chain poetry for myself. Admittedly, the only poetry I read is [Rupi Kaur](https://rupikaur.com/). After searching a bit, I found a random generator for Rupi Kaur-style poems in Python so I decided to try to make one with R!

Let's begin, and make some art!

Gathering and cleaning the data
===============================

To do this project, we will use the `readtext`, `tidyverse`, and `Markovchain` packages.

``` r
library(readtext) # reading in all of the text files
library(tidyverse) # wrangling data
library(markovchain) # creating the Markov chain
```

I found text data for this project from Albert Xu's [Rupi Kaur Poetry repo](https://github.com/albertkx/rupi-kaur-poetry). Many thanks!

If you're running this analysis on your own machine, I've also stored the data on my [github repo](https://github.com/katiejolly/blog/tree/gh-pages/_preposts/rupi_kaur) for this project.

``` r
rupi <- readtext("txt_files/*.txt") # take all the files from this subdirectory

glimpse(rupi) # take a look at what we have
```

    ## Observations: 202
    ## Variables: 2
    ## $ doc_id <chr> "rupi0.txt", "rupi1.txt", "rupi10.txt", "rupi100.txt", ...
    ## $ text   <chr> "how is it so easy for you\nto be kind to people he ask...

Using the `readtext` package, I combined all 202 poems (stored in .txt files) into one dataframe with the `readtext` function. Each poem is a row, with its original filename listed as a variable for reference.

Next, we need to clean up the data a bit. I'm going to take out punctuation (except apostrophes) and the newline characters (`\n`). Both of these will use `gsub`!

``` r
rupi$text <- gsub("\n", " ", rupi$text) # replace newline characters with a space
rupi$text <- gsub("[^[:alnum:][:space:]']", "", rupi$text) # remove all punctuation
```

To run a Markov chain, we need a vector of the terms. To do this I'll first split the terms on spaces and then use `unlist` to create the vector.

``` r
terms <- unlist(strsplit(rupi$text, ' ')) # create a vector of all the words from the poems, split on spaces
```

We now have 5599 words to work with in our Markov chain poems!

Fitting the poem generator
==========================

Quick intro to Markov chains
----------------------------

Markov chains are a way to generate data based on probabilities that something will follow something else. To be more specific in the case of this project, the Markov chain will choose the next word in the poem based on the probability that it comes after the word we have just chosen. Importantly, each "step" is independent of all the other steps.

Let's say we start with my name, "Katie". For the sake of simplicity we have three other words in our vector: "is", "jumps", and "tree". "Is" will follow "Katie" about 70% of the time, "jumps" will follow 29% of the time, and "tree" will follow 1% of the time. So, on 70% of the steps from "Katie", the Markov chain will follow it with "is", and so on. This process will create our short poems! For a more in-depth overview, this [blog post](http://techeffigytutorials.blogspot.com/2015/01/markov-chains-explained.html) will help!

The Markov chain poems
----------------------

We will fit our model using `markovchainFit`. The only input is our vector of words called `terms`. I'm keeping the default fit method, which is `method = MLE`. Usually, I would set the seed (`set.seed`) but in the case of this project, I think half the fun is not knowing what we are going to get, so I'm leaving out that step.

``` r
poem_fit <- markovchainFit(terms)
```

Now we are all set to write poems!

Using the `markovchainSequence` function, we can write a poem of any length. I'll do a few different ones to show some of the possibilities!

With this function, we are creating a Markov chain of `n` words, then putting them together with a space in between to create a poem.

``` r
paste(markovchainSequence(n=25, markovchain=poem_fit$estimate), collapse=' ')
```

    ## [1] "me kiss me as if you tell if it doesnt leave when we are some people even touching me when they know what im a"

Some Markov chain poem examples (with different lengths)
--------------------------------------------------------

    ## [1] "tell him than whole city big in particular you look like rubber against an easy person who live with the way they realize how at"

    ## [1] "shocked it doesnt leave a mind to the rest of the sun to give you have spent less color of i do not that were the conversation going what i"

    ## [1] "than painful love is the stomach 16 breathe the soft enough milk and this will be put my tongue so"

    ## [1] "really believe them actually loving you themselves did i will taste the only whispers into"

    ## [1] "you thought you out of man tell if he still"
