---
layout: post
title: "dev"
categories: ["dev"]
---

What I did...

I'm looking for a way that I can manage my github-pages through mobile phone.
So I started to search terminal things in Playstore. I found Termux app.

Termux is a kind of linux terminal that I can use *apt* command so that I can install some apps in which exists in apt list that the termux supports.

```
apt list
```

The one thing that I get from termux app  is there is a way that I can use **GIT PULL/PUSH** to my github-pages.
So I did as the following steps.

> 1.     install termux in android play store
2. install git with apt 
 - apt install git
 - apt install vim / emacs
3. clone my github page, git push with vim

It works well, from now on, I can post through my phone. 

But it is not easy to make a markdown file because I need the real-time preview markdown editor. But the vim/emacs that I installed at Termux is not avaiable to do it.
So I installed **MarkdownX** in play store.

So my plan is ~
> 1.  type post in the **MarkdownX**
2. copy the context and make a .md file on /_post  in the Termux.
3.  git push to my github-pages


This is the first post that I write down through my phone & bluetooth keyboard.

