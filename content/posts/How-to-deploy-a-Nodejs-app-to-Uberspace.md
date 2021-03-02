---
title: "How to Deploy a Node.js app to Uberspace"
date: 2021-03-01T19:29:11+01:00
draft: true
cover:
    image: "/Uberspace-Nodejs.png"
    alt: "Screenshot of the Uberspace homepage saying 'Universal Hosting for Hackers'"
---

# Introduction

After years and years of different hosting providers (Yahoo Geocities, anyone?),
I was very pleased to come across [Uberspace](https://uberspace.de/en/) a couple
of years ago. Besides the more-than-fair pricing ("Pay what you think is right"),
the feature set excels anything I had seen in the past - ever. In their own words,

> You get all the freedom of the Linux shell you know running on our powerful hardware.
> Host your homepage, store git repositories, compile your own software or run your own web service.
> You can do it all!

With a Node.js application that I was developing, I had recently reached the limits
of Heroku's free plan and was looking for a low-cost alternative, since their pricing
was rather steep for my use case. However, I had come to love the ease of the
deployment with Heroku. A simple `git push heroku master`, and after a couple
of seconds, my changes were live.

## Disclaimer

My Node.js app is rather rudimentary, as in: I do not have a CI pipeline, I do not
have tests, I usually do not tag. I realize that this leaves much to be desired.
For my app, however, I decided that this would do for now.

## The goal

For this post, my goal is to

- have a Node.js application deployed to Uberspace
- be able to deploy a new version with just `git push`
- run database migration after a deployment
- have the Node.js application restarted after deployment

# Let's get our hands dirty!

![Person putting on heavy duty gloves](/stock-photos/pexels-andrea-piacquadio-3846440-720x437.jpg "Well, maybe not THAT dirty.")

---

Image attributions:
- [Uberspace homepage](https://uberspace.de/en/) via screenshot
- [Node.js logo](https://commons.wikimedia.org/wiki/File:Node.js_logo.svg) by Node.js authors via Wikipedia
- ["Anonymous worker in heavy duty gloves"](https://www.pexels.com/photo/anonymous-worker-in-heavy-duty-gloves-3846440/) by [Andrea Piacquadio](https://www.pexels.com/@olly) via [Pexels](https://www.pexels.com/license/)
