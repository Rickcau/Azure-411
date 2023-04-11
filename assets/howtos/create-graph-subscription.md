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
You can create a subscription by sending a POST request to the subscriptions endpoint and in the Request body passing the proper JSON.  Below is an example of how the JSON should be formated.
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
**Important Clarification**
You will make a POST request to the https://graph.microsoft.com/v1.0/subscription endpoint and pass in the a request body that is formatted as outlined above.  For more information on how your notificationUrl should properly respond, refer to [this documentation](https://learn.microsoft.com/en-us/graph/change-notifications-delivery-webhooks?tabs=http){:target="_blank"}

If the Request boday is properly formated, and you notificaitonUrl endpoint properly handles the Validation Token and responds with a 202, the subscription will be created and Graph will respond with these details:
![CreateSub]({{ site.baseurl }}/assets/images/graph/GraphCreateSub.jpg)


### How to get a list of Graph subscriptions ###
This will come in handy to check to see what subscriptions are still active.  Keep in mind that the various change notifications can have different expiry max timeframes.  If you need your subscription to run longer than the max limit, you will need to impliment logic to reset the subscription to a new expiration.

1. Navigate to [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer){:target="_blank"} and make sure you are logged-in to an account that has permissions to Graph.

3. Send a GET request to the subscriptions endpoint https://graph.microsoft.com/v1.0/subscriptions.  Below is an example. If you have any subscriptions they will be noted in the Response Preview Window.
![GetSubs]({{ site.baseurl }}/assets/images/graph/GraphGetSubs.jpg)

# Resources
[How to properly respond to Graph change notifications]( https://learn.microsoft.com/en-us/graph/change-notifications-delivery-webhooks?tabs=http){:target="_blank"} 
