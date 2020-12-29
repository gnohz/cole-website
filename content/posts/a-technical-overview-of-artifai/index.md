+++
title = "A Technical Overview of Artifai"
date = 2020-12-28T12:06:00-05:00
tags = ["artifai"]
categories = ["Software-Dev"]
draft = false
+++

# Overview {#overview}

The goal of Artifai is to help people generate art from photos. I had a problem: My walls were bare. I wanted to decorate them with memories, but I wanted the decorations to be artistic at the same time.

This lead me to discover neural-style, an algorithm that takes two images as input, and copies the style from one onto the other while retaining its content. Neural-style lets you take a meaningful photo and turn it into awesome art! There were several implementations available on Github, but I didn't see a way for non-technical users to easily use it and have the result shipped to their home. And so I began building Artifai with a group of friends; We are a group of university students and this was our first fullstack app! You can visit the website at [https://artif.ai](https://artif.ai).


# Specifications {#specifications}

There is a lot of functionality required from such a webapp:

-   An intuitive user interface
-   A database for keeping track of a user's artifaications and orders
-   Authentication for enabling user identification
-   A pipeline for running GPU intensive jobs
-   On Demand canvas printing
-   Payment processing


# Implementation Details {#implementation-details}


## User Interface {#user-interface}

We built the user interface using a combination of Webflow and React. Webflow is used solely for the landing page, and the rest is done with React and Material-UI. The sections of the website:

Landing page
: Users can learn about the general idea of neural-style

Create page
: Users upload a content, select a style, and press "Artifai" to generate an artifaication (refers to the art they made). Users can select a style from the popular styles or upload their own.

Explore page
: A feed of the most recent public artifaications

Artifaication page
: Users can link to this page to share their artifaications with others

Product page
: Displays the mockup of an artifaication on a canvas and gives the user the option to purchase it.

Profile page
: Displays a users own artifaications. User can toggle post options like "public" vs "private".


## Database {#database}

We used Google Firestore for the database. It's here that we store a user's artifaications, orders, and styles. We build the "Explore" feed by taking the most recent "public" artifaications.

When starting out we didn't really understand Google Firebase, and made the silly mistake of trying to combine it with a GraphQL backend. Firestore has security rules that makes this completely unnecessary. After we realized this we corrected course :)


## Authentication {#authentication}

We used Google Firebase for dealing with Authentication. It made the process way easier. With a few lines of code we offer:

-   Sign in with Google
-   Sign in with Facebook
-   Passwordless Email Signup

It was also critical to have anonymous users which let's users get the full Artifai experience without even needing to create an account.


## Artifaication Processing {#artifaication-processing}

This was a very challenging part of the project; The goal was to run GPU intensive jobs on Google Cloud Compute without keeping a GPU VM running 24/7 (GPU VM hours aren't cheap!). The difficulty is in starting gpu vms on demand without significantly increasing the time a user waits for their artifaication to complete.

We tried all kinds of things inlcluding gpu containers, knative, etc. In the end we were able to solve this problem with the following strategy: We built an image with all the dependencies for getting started on computing right away. When a user submits an artifaication, it adds it to a "queue" represented as a collection of documents in the database, and creates a gpu vm (up to a maximum of 5 running concurrently). The GPU vms pull artifaications from the queue one by one until there are none left, at which point they destroy themselves. Note that we built off of [neural-style-pt](https://github.com/ProGamerGov/neural-style-pt) for the neural-style algorithm which took care of the most of the difficulty associated with implementing neural-style.

{{< image src="./artifaication-processing-pipeline.png" caption="Artifaication Processing Pipeline">}}


## On Demand Canvas Printing {#on-demand-canvas-printing}

For canvas printing we relied on Printify, a print on demand platform that makes it simple to fulfill and send your products to your customers. We use them for generating mockups of user artifaications, and for fulling orders. They don't handle payment processing, so we used Stripe for that.


# Architecture Overview {#architecture-overview}

{{< image src="./artifai-architecture-overview.png" caption="Artifai Architecture Overview">}}


# Meta {#meta}

We started working on Artifai in March 2020 and have been working on it on and off up to the present. We are a team of 5, with two main developers, two minor developers, and one social media manager.

The next steps are to continue to build out features and dig into marketing. So far we are still pretty far in the red with 2 unknown canvas purchasers.

Please let us know if you have any feedback or suggestions!
