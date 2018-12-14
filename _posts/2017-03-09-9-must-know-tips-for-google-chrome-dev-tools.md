---
layout: post
title: 9 Must Know Tips for Google Chrome Dev Tools
tags: chrome devtools
---

![Chrome](/images/chrome.jpg)


Google Chrome's Dev Tools is one of the most powerful debugging tools ever made. If you are developing any type of front end web application, you will inevitably be using it to debug your app.

I personally prefer it whenever possible over other debugging tools.

Here are some of the tips and tricks I've discovered while recently writing my apps [metromeets](http://metromeets.co) and [anjou](https://joesasson.github.io/anjou/):

### Key Symbol Table

- Command ⌘
- Shift ⇧
- Option ⌥
- Control ⌃

## The Goods

1) Quickly open devtools by pressing ⌘-⌥-I - this will open the first tab of devtools (which you can switch around by dragging and dropping). [More Info](https://developers.google.com/web/tools/chrome-devtools/shortcuts)

2) Quickly get to the console by pressing ⌘-⌥-J. [More Info](https://developers.google.com/web/tools/chrome-devtools/shortcuts)


3) Enable dark theme by going to the settings panel which is under the main menu (3 dots symbol). Under the heading Appearance select "Dark" from the dropdown menu. This is surprisingly hard for most people to figure out.

![Dark Theme](https://snag.gy/juzqXK.jpg)

4) ⌘-P will activate fuzzy file search which lets you find any file in your project really quickly in order to preview or set breakpoints.

5) ⌘-⇧-F will search all your files for a given string or regex expression.

6) In the console, you can use jQuery style selectors with a slight variation. `$()` will select the first matching element and `$$()` will select all matching elements. [More Info](https://developers.google.com/web/tools/chrome-devtools/console/expressions)

7) `$_` returns the most recently evaluated expression. Very useful for when you write out an expression but forget to set it to a variable.

8) ⌘-⇧-C will enable an inspect feature that will show selector information, css spacing info, and will scroll element into view in the elements panel of the dev tools. This can also be accessed by clicking on the ![icon](https://snag.gy/mrXau4.jpg) icon.

9) ⌘-⇧-M Will toggle device emulation which allows you to preview what your page will look like on a mobile device. You can choose from a number of different devices by clicking on the dropdown menu on top of the emulation. This can also be accessed by clicking on the ![em-icon](https://snag.gy/O6yVIh.jpg) icon.

That's it for now! I am sure there are so many more. If you know of a trick that you just have to share, please leave it in the comments section below.

Thanks for reading!
