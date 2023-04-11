---
title: Creating Graph Subscription
permalink: /assets/howtos/create-graph-subscription/
parent: How to guides
layout: home
nav_order: 4
---
# **Prerequisites**
1. [Read this document](https://learn.microsoft.com/en-us/graph/api/subscription-post-subscriptions?view=graph-rest-1.0&tabs=http){:target="_blank"} to get a fundamental understanding of how to create Graph Subscriptions.
2. Access to a Shared Mailbox 

# 1. How-to: - Create a change notificaiton subscription for email in a Shared Mailbox
You can create a subscription by sending a POST request to the subscriptions endpoint and in the Request body passing the proper JSON.  Below is an example of what the JSON shoujld look like.
~~~
{
   "changeType": "created",
   "notificationUrl": "<https://YOUR URL THAT WILL BE CALLED>",
   "resource": "/users/PoCSMB@XXX.onmicrosoft.com//mailfolders('inbox')/messages",
   "expirationDateTime":"2023-04-04T11:00:00Z",
   "clientState": "secretClientValue",
   "latestSupportedTlsVersion": "v1_2"
}
~~~
Below is a screenhot of what this would look like in Graph Explorer.
![CreateSub]({{ site.baseurl }}/assets/images/graph/GraphCreateSub.jpg)


### Get a list of Graph subscriptions ###
1. Navigate to [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer){:target="_blank"} and make sure you are logged-in to an account that has permissions to Graph.
2. 
3. Send a GET request to the subscriptions endpoint https://graph.microsoft.com/v1.0/subscriptions.  Below is an example. If you have any subscriptions they will be noted in the Response Preview Window.
![GetSubs]({{ site.baseurl }}/assets/images/graph/graphGraphGetSubs.jpg)

