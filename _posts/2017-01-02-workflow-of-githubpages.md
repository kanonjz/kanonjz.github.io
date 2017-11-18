---
layout:     post
title:      "Workflow of Github Pages"
subtitle:   ""
date:       2017-01-02
author:     "Kanon"
header-img: "https://images.pexels.com/photos/189199/pexels-photo-189199.jpeg?w=940&h=650&auto=compress&cs=tinysrgb"
catalog: false
tags:
    - 闲来研究
---

## My question

The documentation of Jekyll tells me, that the _site-directory of a Jekyll site contains the compiled version of the site I have created after running
```
Jekyll build
```
Several articles recommend, that I include the _site-directory in my .gitignore-file because "it just contains the compiled version of my site". (that's what some articles recommend. So, I am not sure if I don't understand some concept of Jekyll or some concept of git. If the _site-directory contains the compiled version of a site, shouldn't that be the thing that is on the server the provides the final website? I do understand why you put source code on github and what to do with it, but in the case of github pages, github is not a versioning system but a filehosting system and the filehosting system should host compiled versions of my work to provide it via MyUsername.github.io to users, right? My question is: shouldn't it be only the _site-directory of my Jekyll-Website that I deploy to github because that should be the compiled source code that github provides to users? So, shouldn't I put anything else in the .gitignore-file EXCEPT the _site-directory?

## research 
I remenbered that the website would work though I do nothing when I forked it from the designer, it didn't have the _site-directory as well. Why? Can github build it by its own?   

In order to find out the reason, I added an article on github website withot building in my computer. To my surprise, my blog still worked and I could see that article !

## So
It's clearly that github can build the blog automatically. It's really amazing!   

I deleted the local file in my computer, it's more sensible to write articles on the github website directily.
