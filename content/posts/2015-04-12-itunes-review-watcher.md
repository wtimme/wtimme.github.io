---
title: "iTunes Review Watcher"
date: 2015-04-12 18:20:00 +0100
draft: false
---

### Introduction
In my sparetime I'm creating a iOS application that focusses on customer
reviews on *Apple*'s App Store. It consists of the native app (written in
*Objective-C*, as of now) and a REST server that gathers all the information from
*Apple*'s servers.

As much as I love happily coding away, I also appreciate our users' feedback.
For iOS applications, a very common way of receiving feedback are the reviews
on the *iTunes Store*. Customers can vote a version of an application using a
numerical rating (1 to 5 stars) and enter a text.

### The official "iTunes Connect" app
These reviews are not only visible on the App Store's website or within *Apple*'s
App Store app, but also from within *iTunes Connect*, the interface
for managing everything related to the app (e. g. InApp purchases, the app's
description, etc.). *Apple* has also released
[a native iOS app](https://itunes.apple.com/us/app/itunes-connect/id376771144?mt=8)
which allows access to most (but not all) features of the web interface.
When it comes to reading customer reviews, however, the native app falls a bit
short:

* It's only possible to read reviews from **one store country at a time**. Reading all
reviews for an app is a painful process of hopping back and forth between the
country list and the reviews.
* The native app has **no support for push notifications** whatsoever. Noticing
new reviews is pure luck. I find customer feedback not only valuable but also
incredible useful (sometimes), e. g. when there's a bug that causes my
application to crash.
* There's **little history for the reviews**. Older ones disappear and it's
difficult to paint a broader image.

![Screenshot of the iTunes Connect app describing the territory list dilemma](/itunes-connect-territory-list.png "This is one of my biggest issue with this app")

### Product requirements
These are my requirements of the project:

* Let users search for and follow apps (or other content)
* Query *Apple*'s server on a regular basis and cache the data, making it
available infinitely
* Post push notifications for when a new review has been added or an older one was
updated

Other features that might be added later:

* **Sync** the reviews with the device and make the review history available
offline
* **Keep track** on the app's ranking in the App Store charts
* **(Further) Analyze** and use the data gathered

### Personal goals
With this project, I plan on

* learning to work with [NoSQL](https://en.wikipedia.org/wiki/NoSQL) databases
such as [MongoDB](https://www.mongodb.org/)
* learning to a [RESTful web interface](https://en.wikipedia.org/wiki/Representational_state_transfer)
* learning to work with the APNS (*[Apple Push Notification Service](https://en.wikipedia.org/wiki/Apple_Push_Notification_Service)*)
* developing a server application with a near infinite uptime
* (finally) releasing an application on the *Apple App Store*
* hopefully making the life of other people easier

### First steps
I started by researching ways to get the data from *Apple*.

#### Reviews via RSS
The web interface of *iTunes Connect* allows the developer to subscribe to
reviews using [a RSS feed](https://en.wikipedia.org/wiki/RSS). For example,
the most recent reviews for the official *Facebook* iOS application
[can be
retrieved using this link](http://itunes.apple.com/rss/customerreviews/id=284882215/sortby=mostrecent/xml?cc=us).
After playing with the parameters a bit I went ahead and created a simple
[node](https://nodejs.org/) app that would download the XML and parse it into
[JSON](https://en.wikipedia.org/wiki/JSON) objects:

```
{
    "updatedDate": "2014-08-13T15:07:00.000Z",
    "entryId": 1045982842,
    "title": "Great game",
    "content": "Very cool game, good graphics, interesting gameplay",
    "rating": 5,
    "appVersion": "1.1.8",
    "authorName": "Иллжоаро",
    "trackId": 564777203,
    "rssFeedCountryCode": "us",
}
```

#### Saving reviews to database
For the next step, I began saving the reviews to
[a MySQL database](https://en.wikipedia.org/wiki/MySQL). The JavaScript quickly
got long and the queries were difficult to edit and maintain.

After talking to co-workers and some research I decided to use
[MongoDB](https://www.mongodb.org/) instead. Using
[the node package manager (NPM) registry](https://www.npmjs.com/) I found
[mongoose](http://mongoosejs.com/), an object modeling module for *node*. In my
opinion this makes for cleaner code, as the models can be easily created,
searched and updated.

#### Rewriting the whole server
In order to clean up the server project (everything was in one *app.js* file)
I decided to rewrite it from scratch and splitting functionality into modules
that I can `require` whenever I need.

#### The RESTful API
As mentioned, the server provides the data using a RESTful interface. *node* has
plenty of REST modules available and I settled for
[restify](http://mcavage.me/node-restify/). That made it easy to implement the
calls necessary for the app:

* `GET /storeItems` returns a list of all *iTunes Store* items in
the server's database
* `GET /storeItems/:storeItemId/reviews` returns a list of all reviews for a given *iTunes Store* item ID

#### Creating the basic iOS app
With the server ready to scrape and serve reviews I went ahead and started
developing the iOS app. It consists of two view controllers: The first one
contains a list of all *iTunes Store* items saved in the database. The second one
displays the list of reviews that the server has scraped. For the first
iterations I don't plan on winning any award with the UI, so the basic
[Cocoa Touch](https://en.wikipedia.org/wiki/Cocoa_Touch) elements will do.

![Screenshot of my app's interface](/facebook-reviews.png "This is a first version of my app")

### Current status
Having the list of reviews in the app is already quite a success for me.
I've also added a basic *iTunes Store* search API and plan on adding management
functionality (add/remove apps) to the iOS app.

I would also like to release the source code to the server to the general
public but haven't decided yet just what I want to do with the application.
As the app is not deployed on any server but my home computer it does not
periodically check for updates to the reviews yet. This is also an issue
that I would like to tackle rather soon, just to have some records to work with.
