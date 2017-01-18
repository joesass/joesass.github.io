---
layout: post
title: Dr. Jekyll, MD
---

What we are about to explore is a fantastic tool for writing words on the web. It's called markdown and it's what this blog is built on.

It was created in 2004 by John Gruber and Aaron Swartz with the purpose of creating a language that:

> Would enable people to write using an easy-to-read, easy-to-write plain text format, and optionally convert it to structurally valid XHTML/HTML

We are probably all familiar with markdown from README's on Github, which supports it's own flavor of Markdown surprisingly referred to as Github Flavored Markdown.

The reason it's called markdown is because it's similar to HTML markup but without all the annoying tags.

### Headers

For example, we are all familiar with the good old `h1` tag.

Writing a header usually looks something like this:  
`<h1>I am an important title</h1>`

In markdown we would write the same thing like this:  
`# I am an important title`

and it will look like this:

## I am an important title

and for each descending header size add another `#`.

## This is an H2

### This is an H3

#### H4

##### etc.

###### What the hell is the point of the H6 tag? It's smaller than p text!! Has anybody ever actually used one? Like why would you want to make a header that's smaller than the paragraph text. It's barely even readable on a regular screen! Damn.

### Lists

One of the coolest things about markdown is how easy it is to make a list, because honestly, who doesn't love to make lists?

In order to make a list all you have to do is make bullets using `-, *, or +`

```md
Doctor Stuff:

- stethoscope
- scalpel
- anasthesia
```

This will look like this:

Doctor Stuff:

- stethoscope
- scalpel
- anasthesia

You can even create nested lists by indenting the bullets:

Doctor Stuff:

- stethoscope
- knives
  - scalpel
  - machete
- anasthesia

### Blockquotes

> You can create blockquotes very easily by adding a `>` at the beginning of each line that is part of the quote.

This will set the quoted text apart from the rest of your text to show that it is quoted

### Links

Another extremely easy thing to do is creating [hyperlinks](http://i.giphy.com/DgLsbUL7SG3kI.gif). All you need to do is surround the word that you want to display with `[]` and put the url in an adjacent `()` like this:  
`[google](http://www.google.com)` and it will look like this:

[google](http://www.google.com)

There are other ways to create links in a footnote style you can read about them [here](https://daringfireball.net/projects/markdown/syntax#link)

### Images

Inserting images is similarly easy and has a very close syntax, in fact it is the same syntax with a `!` in the front:

`![alt-text](http://picture.com/awesome-pic.jpg)`

Here is a picture of The Starry Night, because it's awesome!

![starry-night](http://lh6.ggpht.com/HlgucZ0ylJAfZgusynnUwxNIgIp5htNhShF559x3dRXiuy_UdP3UQVLYW6c=s1200)

### Code formatting and Code Fences

Perhaps the most useful part of markdown for programmers like us is the way it handles code. All we need in order to turn an inline string into code formatting is to surround the string with backticks:

```md
I just woke up to find `some weird code`
```

and this is what it looks like:

I just woke up to find `some weird code`

Even cooler than inline code formatting are code fences.

A code fence is a full block of code surrounded by a "fence" of 3 backticks
before and three backticks after:

![code-block](https://snag.gy/CJxY9l.jpg)

```
code block
```

![ruby-block](https://snag.gy/bL8dkX.jpg)

```ruby
# Whats extra cool about this one is that most renderers preserve
# syntax highlighting if you specify a language

class Awesome < Fantastic
  def initialize(great)
    @great = great
  end

  def output
    puts "Isn't this #{@great}?"
  end
end
```

### Text Editors

The greatest part of markdown is that it doesn't need any specific text editor in order to write it, because it is all in plain text. But, there are editors that add features and make it easy to preview and understand it better.

- [Macdown](http://macdown.uranusjr.com/) is a cool one that gives you a split pane preview
- [Atom](https://atom.io/) is my favorite choice with a few packages that make it even better
  - [markdown-writer](https://atom.io/packages/markdown-writer) this adds a lot of nifty features like automatically creating bullet points when you press enter
  - [markdown-preview](https://atom.io/packages/markdown-preview) you can preview the rendered markdown by pressing control-shift-m

To start writing a markdown file, all you have to do is create a new file with a `.md` extension, start typing, and your file will automatically be rendered as markdown by browsers.

### What is Jekyll?

This blog is being powered by [Jekyll](https://jekyllrb.com/) and all it does is take my markdown files and created a nice looking and working site out of it. Officially it's a static site generator which means it can do a lot more than just blogs, but for my purposes blogs are enough.

Some of the useful features are the template engines and the themes that you can use. Also the way it structures URL's is based on tags which can allow you to categorize your posts. Most posts include a header with some YAML in it that includes metadata, for example:

```yml
---
layout: post
title: Dr. Jekyll, MD
---
```

I'm still new to Jekyll so hopefully I can pick up some more tricks and write about them in a later blog post.
But for now the basics are good enough for me.

### Github Pages

Another great resource that I found when starting the blog was github pages which allows you to host a static site for free under your github username using your github repo.
The instructions to get it set up are very easy and clear and it takes about 10 minutes. [Getting Started With Jekyll + Github Pages](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)

Other Links:

[Daring Fireball Official Documentation](https://daringfireball.net/projects/markdown/)

[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
