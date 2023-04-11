test

**Prerequisites**
1. [Click here](https://learn.microsoft.com/en-us/graph/graph-explorer/graph-explorer-features){:target=_blank} an read this document on working with Graph Explorer.
2. [Click here](){:target=_blank} an read this document to better under how to consent to permissions.
3. User account with M365 licesne so you can run the Mail Graph API queries.  You can [click here](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/assign-licenses-to-users?view=o365-worldwide){:target=_blank} to read details about how to grant a user an M365 license.  

# How-to: Use Graph Explorer

**How to: 1 - Run a simple Graph API Query**
1. Navigate to the Graph Explorer by [clicking here](){:target=_blank}.
2. Click on the *Sign in* icon to the right and sign in to your Azure Account.
![]({{ site.baseurl }}/assets/images/graph/Graph-SignIn.jpg)
3. You will need to consent to allowing Graph Explorer to read your profile infomation.
![]({{ site.baseurl }}/assets/images/graph/graph-perms-request.jpg)
4. Once you consent, you can click on *Run Query* to execut the *GET* request to *https://graph.microsoft.com/v1.0/me*.  It will return the following results:
![]({{ site.baseurl }}/assets/images/graph/GraphQuery1.jpg)

**How-to: 2 - Get the total count of messages in your inbox**
1. Run a query to get the total message count in your inbox folder by entering the following query:
![]({{ site.baseurl }}/assets/images/graph/graph-query-totalmsg.jpg)


GET https://graph.microsoft.com/v1.0/me/mailFolders
