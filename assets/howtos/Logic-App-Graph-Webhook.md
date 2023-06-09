---
title: Graph - Logic App Webhook
permalink: /assets/howtos/Logic-App-Graph-Webhook/
parent: How to guides
layout: home
nav_order: 2
---
#  How-To 
## Subscribing to Graph Change Notifications with Logic Apps
In this how-to, I will cover the fundamental concepts and logic that needs to be implimented in order to leverage a Logic App as a Webhook with Graph Change notifications.  If you want a hands-on lab that walks you through the process please [click here]() to navigate to the *hands-on labs* 

## Process the Validation Token that is sent when the Graph Subscription is created
1. When you create the Graph Subscription, Graph will send a Validation Token to the notificationUrl, in this case, it will be our Logic App.

2. When the Logic App receives the Validation Token it must respond with a status of 202 and it must include in the reponse body the exact same Validate Token that Graph sent the Logic App as plain text.
[Click here](https://learn.microsoft.com/en-us/graph/change-notifications-delivery-webhooks?tabs=http){:target="_blank"} to read the documentation on how to handle the Validation Token. 

## Process the change notification
Once the Logic App has successfully processed the Validate Token, Graph will start to send change notification details to the notificaitonUrl when a change occurs.  

1. When Graph sends the Logic App a change notification it will send in the request body a JSON collection with the change notificaiton details.  Graph will continue to retry until it receives a 200 status code with nothing in the response body.

You can review the *Change Notificaiton Example* section in the [above link](https://learn.microsoft.com/en-us/graph/change-notifications-delivery-webhooks?tabs=http){:target="_blank"} to better understand the format of the JSON and how Graph is expecting the Logic App to respond.

## Closing Summary
The Logic App must respond to the Validation Token in order for you to be able to create the Graph Subscription and it must properly respond to the change notification.  At this point if the Logic App is properly handling the Validation Token and the change notificaitons you are ready to add business logic.

If you need help setting up the Graph subscription, [click here](/assets/howtos/create-graph-subscription/) for steps on how to create a Graph Subscription.


----
