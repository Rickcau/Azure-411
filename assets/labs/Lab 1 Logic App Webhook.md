---
title: Lab 1 - Logic App Webhook
permalink: /assets/labs/Logic-App-Graph-Webhook/
parent: Labs
layout: home
nav_order: 2
---
# Lab 1 - Logic App Webhook
In this lab we will create a Graph Subscripton that sends change notifications to a Logic App.  We will subscribe to change notifications on a Shared Mailbox.  When completed you will have a Logic App that is able to handle the Validation Token as well as properly respond to the change notifications.

## Prerequisites 
1. Read and study the details of how the Validation Token and Change Notification needs to be handled by [clicking here](assets/howtos/Logic-App-Graph-Webhook/){:target="_blank"}.

2. Access to a Shared Mailbox. You don't have to use a shared box for this lab, but if you don't you will need change your Graph Subscription appropriately. For more details on Graph Subscriptions [click here](/assets/howtos/create-graph-subscription/)


#3 How to create a Logic App that can be used as a Graph Webhook
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
