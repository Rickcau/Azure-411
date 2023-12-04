---
title: Graph - Logic App Webhook
permalink: /assets/howtos/fix-apim-swagger-import-issue/
parent: How to guides
layout: home
nav_order: 2
---
#  How-To 
## When trying to create an API in APIM you may get an error during import
In this how-to, I will cover a workaround to file this.  In this how-to, I will cover a workaround to fix this issue.  In this case we are referring to the [Azure Open AI Swagger files found here]( https://github.com/Azure/azure-rest-api-specs/tree/main/specification/cognitiveservices/data-plane/AzureOpenAI/inference/preview) {:target="_blank"}.  

## Steps to reproduce the issue

1. Navigate to API Management Services in the Azure Portal
2. Next, click on **+ Add API**
3. Under **Create from definition** you will see **OpenAI** click on it. The **Create from OpenAPI specification** dialog will appear.
4. Next, click on **Select a file** and select one of the Swagger files you downloaded from [here]( https://github.com/Azure/azure-rest-api-specs/tree/main/specification/cognitiveservices/data-plane/AzureOpenAI/inference/preview) {:target="_blank"}

You will get an error message that states: "One or more fields contains incorrect values".  Below is an example of the error.
*place image here*

## Steps to workaround the issue.
The workaround is very simple.  You create a blank API, then you import the Swagger.  Here are the exact steps to fix this issue.

1. Navigate to API Management Services in the Azure Portal
2. Next, click on **+ Add API**
3. Under **Define a new API** click on **HTTP**, this will open the **Create an HTTP API**
4. In the **Display Name** field enter **Azure Open AI Demo** and click on **Create** as depicted in the image below

*place holder for image*

At this point, you will have a blank API.  Now, we need to import the Swagger for this blank API, below at the steps.
5. Click on the **...** located to the right of the API.
6. A context menu will appear, now click on **Import**
7. You will see a blade open to the right labeled as **Import API** and below this you see **OpenAI**, click on it.  This will open the **Import from OpenAPI specification* dialog.
8. Next, click on **Select a File** and select the Swagger file you would like to import, then click on **Import**

At this point all the operations will be created based on the Swagger file you just imported.  Below is an example of what the operations may look like after following these steps.

**place holder for image**

## Closing Summary
This issue has been reported so if you are not experiencing this issue, it means it has been fixed.

----
