---
layout: post
title: Setting up a Rails App with Rspec and Postgres
---

I recently started a new project with Ruby on Rails called Hava Amina. It's still in development, the code is on Github [here](https://github.com/joesasson/hava-amina).

While running `rails new` I realized that I actually wanted to change certain things.

First of all, I recently started learning TDD with [Everyday Rails Testing with Rspec](https://leanpub.com/everydayrailsrspec) and I'm loving the readability and organization of the specs that they show. So I wanted to use rspec in my app instead of the default minitest.

Also, I wanted to use postgresql for my database instead of the default sqlite3 because I normally deploy with [heroku](`https://heroku.com`) which is easy, free, and fast. Heroku does not support sqlite3 databases, so it's always a hassle to go into the config files and change it yourself and make sure you're getting everything right.

So I did a little poking around and I found in a [blog post](http://julianveling.com/?p=18) that there is a command line argument I can pass to `rails new` in order to use pg as a database and another one to remove the default test framework.

Here is the one line command:

```bash
rails new app-name -T -d postgresql
```

The `-T` will skip the testing framework altogether, and the `-d postgresql` will set postgres as the db.

Now, in order to use rspec, all I need to do is add `gem 'rspec'` to the Gemfile in the test and development group and `bundle install`.

Then, to generate the folders and files and helpers for the project, run:

```
rails generate rspec:install
```
(Thanks Edward Woodcock for the correction)

This will add the `spec` folder along with the `rails_helper` and `spec_helper` and also`.rspec` file in the main directory.

I normally add `--format documentation` when running rspec in order to see a description of the test as it's running instead of just a dot. If I want this option every time I run rspec, I can add it to the `.rspec` file that was generated.

Now my app is perfect, but I know that I want this configuration for the next time I want to set up a new rails app and the time after that and so on. Is there a way that I can just run this setup every time?

I discovered through some nice [blog post](https://richonrails.com/articles/customizing-the-rails-app-generator)[s](https://www.benpickles.com/articles/64-my-dot-railsrc) that there is a file called `.railsrc` that does something similar to what the `.rspec` file does above. It passes in whatever command line arguments you define in it every time you call `rails new`. You can edit it by calling `atom ~/.railsrc` (or whatever editor you prefer) from any directory in your command line.

At the end of the day my `.railsrc` file looked like this:

`~/.railsrc`

```bash
-T
-d postgresql
```

There are other ways of customizing more fully using rails templates (in blog post linked above), but ultimately I decided I can do that later, because I'm lazy.

Now, every time I run `rails new`, I get postgres and no minitest. 🔥🔥🔥

Now for some words from my 7 month old son:

```
:
''
...........]];;;                        CBBEA
'
```

Thanks Mo!
