<properties
   pageTitle="Using Azure Functions with Logic Apps | Microsoft Azure"
   description="See how to use Azure Functions with Logic Apps"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="using-azure-functions-with-logic-apps"></a>Using Azure Functions with Logic Apps

You can run custom snippets of C# or node.js by using Azure Functions from within a logic app.  [Azure Functions](../azure-functions/functions-overview.md) offers server-free computing in Microsoft Azure. This is useful for performing the following tasks:

* Advanced formatting or compute of fields within a Logic App
* Performing calculations within a workflow
* Extending the functionality of Logic Apps with functions that are supported in C# or node.js

## <a name="create-a-function-for-logic-apps"></a>Create a function for Logic Apps

We recommend that you create a new function in the Azure Functions portal by using the **Generic Webhook - Node** or **Generic Webhook - C#** templates. This auto-populates a template that accepts `application/json` from a logic app.  If you select the **Integrate** tab in Azure Functions it should have **Mode** set to **Webhook** and **Webhook type** of **Generic JSON**.  Functions that use these templates are automatically discovered and listed in the Logic Apps designer under **Azure Functions in my region.**

Webhook functions accept a request and pass it into the method via a `data` variable. You can access the properties of your payload by using dot notation like `data.foo`.  For example, a simple JavaScript function that converts a DateTime value into a date string looks like the following example:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-a-logic-app"></a>Call Azure Functions from a logic app

In the designer, if you click the **Actions** menu, you can select **Azure Functions in my Region**.  This lists the containers in your subscription and enables you to choose the function that you want to call.  

After you select the function, you are prompted to specify an input payload object. This is the message that the logic app sends to the function, and it must be a JSON object. For example, if you want to pass in the **Last Modified** date from a Salesforce trigger, the function payload might look like this:

![Last modfied date][1]

## <a name="trigger-logic-apps-from-a-function"></a>Trigger logic apps from a function

It's also possible to trigger a logic app from within a function.  To do this, simply create a logic app with a manual trigger. For more information, see [Logic apps as callable endpoints](app-service-logic-http-endpoint.md).  Then, within your function, generate an HTTP POST to the manual trigger URL with the payload that you want to send to the logic app.

### <a name="create-a-function-from-the-designer"></a>Create a function from the designer

You can also create a node.js webhook function from within the designer. First, select **Azure Functions in my Region,** and then choose a container for your function.  If you don't yet have a container, you need to create one from the [Azure Functions portal](https://functions.azure.com/signin). Then select **Create New**.  

To generate a template based on the data that you want to compute, specify the context object that you plan to pass into a function. This must be a JSON object. For example, if you pass in the file content from an FTP action, the context payload will look like this:

![Context payload][2]

>[AZURE.NOTE] Because this object wasn't cast as a string, the content will be added directly to the JSON payload. However, it will error out if it is not a JSON token (that is, a string or a JSON object/array). To cast it as a string, simply add quotes as shown in the first illustration in this article.

The designer then generates a function template that you can create inline. Variables are pre-created based on the context that you plan to pass into the function.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png
