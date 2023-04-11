---
title: Using Graph Explorer
permalink: /assets/howtos/using-graph-explorer/
parent: How to guides
layout: home
nav_order: 3
---
    Prerequisites
# **Prerequisites**
1. [Read this document](https://learn.microsoft.com/en-us/graph/graph-explorer/graph-explorer-features){:target="_blank"} to get a fundamental understanding of how to us Graph Explorer and to consent to permissions.
3. A User account with M365 license so you can run the Mail Graph API queries.  You can [click here](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/assign-licenses-to-users?view=o365-worldwide){:target="_blank"}to read details about how to grant a user an M365 license.  You will have issues with any of the Mail queries without an M365 license.  

# How to: 1 - Run a simple Graph API Query
1. Navigate to the Graph Explorer by [clicking here](https://developer.microsoft.com/en-us/graph/graph-explorer){:target=_blank}.
2. Click on the *Sign in* icon to the right and sign in to your Azure Account.
![]({{ site.baseurl }}/assets/images/graph/Graph-SignIn.jpg)
3. You will need to consent to allowing Graph Explorer to read your profile infomation.
![]({{ site.baseurl }}/assets/images/graph/graph-perms-request.jpg)
4. Once you consent, you can click on *Run Query* to execut the *GET* request to *https://graph.microsoft.com/v1.0/me*.  It will return the following results:
![]({{ site.baseurl }}/assets/images/graph/GraphQuery1.jpg)

# How-to: 2 - Get the total count of messages in your inbox
*Note: refer to item 3 in the prerequisites section*
1. Run a query to get the total message count in your inbox folder by entering the following query:
![]({{ site.baseurl }}/assets/images/graph/graph-query-totalmsg.jpg)

*Important Note*
If you don't have an M365 license then you don't have a mailbox, so you will get the following error when running this query:
![]({{ site.baseurl }}/assets/images/graph/graph-no-mailbox.jpg)



GET https://graph.microsoft.com/v1.0/me/mailFolders
