---
layout: post
title: Putting the Oh! in Oauth
---

If you've ever built an application that is to be used by real people, you know how important it is to know that a user is who they say they are.

![im-doctor](https://media.giphy.com/media/8xHQoMpEXa45q/giphy.gif)

It is for this reason that we implement a authentication system for all of our apps.
There are many ways to implement this system. The most basic being writing it yourself and using bcrypt to encrypt the password. That's the most basic authentication possible, but it works perfectly fine.

In the "basic" way, the user simply puts in their unique username and enters a password, which gets encrypted and stored in our database, and we can run the same encryption on it to compare it with what we have in our database, and let the user in if it matches.

There is however another, more complicated way to verify your users' identities called Oauth.

Why would anybody want to do things the more complicated way? Aren't all programmers lazy?

The answer is, we do it for our users.

When trying to get someone to sign up for your site, besides for actually getting them to want your product or service, there are another few hurdles to overcome.
- Many people signing up to a new site are wary of the security of the site, and don't want their information stored on the site's database.
- Many people are lazy and don't want to fill out multiple forms with information just to see what a site offers
- People would rather not have to type at all in order to sign up and log in

There is a solution to this problem. In fact, if you think about it, most people already are signed in to some other site, maybe a well known site like facebook, twitter, or linkedin. Wouldn't it be wonderful if we can just ask facebook to verify our user's identity for us?

Well, that's Oauth in a nutshell.

Oauth is a protocol that allows our site to request credentials from a third party API to verify a user's identity, as well as other information based on what the user wants the app to access.

We benefit our users in the following ways:

- They don't have to re-enter any information
- They don't have to type anything
- They can sign up and log in with one click
- They don't have to remember their password from our site
- They don't have to store their sensitive information on our site
- and many more benefits!

The way Oauth works is that an app is configured to redirect a user when they click on the signup or login button to the application that hosts the authentication API. The user is then presented with the login page for that app asking for their permission for the original app to access the API. When their information is verified correctly, they are redirected back to the original app with the user's information and a token which is stored at the API. This token can now be used to make requests for the user's credentials in the future.

One of the ways of doing Oauth with Rails is called omniauth which is an authentication system that has many "strategies". Like for example using twitter's API would be one strategy. Using facebook's API would be another.

I set up a simple implementation of Oauth2 following [this tutorial](https://www.sitepoint.com/rails-authentication-oauth-2-0-omniauth/) for a rails app.


I won't go into every detail but the main steps involved in getting it to work are:

- Creating a rails app
- Adding the omniauth strategy gem to the Gemfile i.e. [`omniauth-twitter`](https://github.com/arunagw/omniauth-twitter)
- Registering an app with the provider which involves going to twitter's developer section and filling out a form about your app and defining a callback url for twitter to redirect to after a successful authentication
- Defining a model, controller and route in our app to handle the incoming information

Another important detail of the Oauth protocol is the permissions that a user authorizes our app for.

Any provider of Oauth has levels of permission that are asked of the user when the user signs in to authorize the app. Sometimes, we only want to get the user's sign in credentials or profile info, when we don't need any of their other information. But in other cases, we want other information about the user. Maybe it's their email address, or maybe it's something within the application like in the case of twitter, getting the user's tweets. To go a level higher, what if our app wants to post tweets on behalf of the user? There are many different things that the API can give access to and it's up to the app to request permission for those things and for the user to authorize the request.

There is a lot of information available about Oauth2 and how to implement it in defferent environments.

You can check out [my own notes](https://github.com/joesasson/hava-amina/blob/master/notes.md#oauth) that I took while implementing it as a standalone project (including integrating it with devise and using figaro to store the API keys and a heck of a lot more, maybe too much)

Links:

- [OAuth docs](https://oauth.net/2/)
- [Tutorial from Digital Ocean](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2)
- [omniauth twitter gem](https://github.com/arunagw/omniauth-twitter)
- [omniauth gem](https://github.com/omniauth/omniauth)
- [sitepoint tutorial](https://www.sitepoint.com/rails-authentication-oauth-2-0-omniauth/)
- [my own humble notes](https://github.com/joesasson/hava-amina/blob/master/notes.md#oauth)
