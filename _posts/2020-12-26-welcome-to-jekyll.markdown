---
layout: post
title:  "Making a Github Pages page"
date:   2020-12-26 20:05:43 -0500
categories: Web
---


So I decided to make use github pages to make a personal website.

Why? I've been meaning to start a personal website for awhile, and this seemed like the least amount of friction possible. 

Now making your own Github Pages page is very simple, you just make a git repo with the name $user_name.github.io, and then whatever site you put there is 
served for free at https://$user_name.github.io. You don't need to put anything fancy there, for example for a long time I had nothing but an empty index.html file that shouted "hello world!", but github dutifully served that to anyone navigating to that URL. Pretty cool, and pretty convenient if you already use github.

So, that's Easy enough, I could roll my own HTML (I still might, I do like to dabble a lot ..) but instead I opted to use something called Jekyll to create this blog. Jekyll is a static site generator that apparently integrates well with github pages. A static site generator is a framework which takes as input various markdown, text, config etc files, and from them creates a static HTML website. Static sites have the advantage of being simple and lightweight, and it's not like I need a lot to serve up simple blog posts.

Anyway, that's what you are looking at now. I just write a blog post with some markdown and when I commit and push the changes up, github updates my website for me.

from there I just followed the instructions [here](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll).

I have to say I'm not totally satisfied with this setup. I had to fumble around with versioning and permission issues with ruby, gem, bundler, and jekyll. It seems like everytime I try to run jekyll serve, something goes wrong and I have to install something. Surely this is my fault, likely having to do with having several different versions of these different tools on my system. But still, this seems like a lot of overhead to just serve some HTML. I'm sure at some point I'll hand write my own HTML and serve it from a server I own, I just like that idea better. 

But for now, this will do, and it's surely the right solution for a lot of people. The website is attractive, and once it's set up it really is pretty easy. 
