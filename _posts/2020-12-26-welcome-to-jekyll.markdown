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

I started following the instruction [here](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll
), though I can tell you I found that a lot of their commands didn't work for me, so I decided to put what I did do here.

First, I needed to install ruby and gem, which is a language and an installer for that language respectively. MacOS actually ships with these, but something was not quite right with my native versions, I instead opted to install them fresh from homebrew:

```
brew install ruby
```

I am 90% sure this also installs gem, the ruby package manager. Now as I mentioned earlier we have another copy of ruby installed already on our system, let's just overwrite our system path to ignore it, add the following to zshrc:

```
export PATH="/usr/local/opt/ruby/bin:$PATH"
```

Fine, now after sourcing this file, we can install jekyll with our new version of gem. 

```
gem install jekyll
```

Now, clone the github page you've made for yourself. For me this was:

```
git clone https://github.com/sgillen/sgillen.github.io 
```

Now I just do:

```
cd sgillen.github.io
jekyll 4.2.0 new .
```

Bingo bango. Now I have a blank Jekyll website. After fudging around a bit I found I can edit about.md, _config.yml, and add posts to _posts/$(post_name).md
That's basically what I've done here.

Cool hope you enjoyed this trash tier blog post that I'll probably take down almost immediately.
Still writing practice is good.


