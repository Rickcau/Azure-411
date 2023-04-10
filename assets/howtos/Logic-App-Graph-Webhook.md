---
title: How to - Logic App Graph API Webhook
permalink: /assets/howtos/Logic-App-Graph-Webhook/
parent: How To 
layout: home
nav_order: 2
---
# How to create a Logic App that can be used as a Graph Webhook
In this how-to guide I will go over the fundamentals of what is required for this.  If you would like to learn exactly how to do this, I suggest you go through my hands-on lab [Logic App Graph Webhook]().


# How do you create a Graph Subscription?

# Validation Tokens, Content-Type, application/json, plain/text 
These for 4 critical components that you must understand how to handle in the Logic App.

# What is a Validation Token and why does my Logic App need to understand how to handle it?
TBD

# What are the expected HTTP Responses that Graph expects?
1. Graph expects a 202 status code and for you to respond back with the Validation Token in plain/text when it sends it to you (this is done once when you subscribe to a Graph endpoint).
2. When a change notification is send Graph will send you a JSON collection with details about the change notification and it expects a 200 status code and reponse body to be an empty JSON collection.


----
