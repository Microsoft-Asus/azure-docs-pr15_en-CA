<properties
    pageTitle="Azure Functions Service Bus triggers and bindings | Microsoft Azure"
    description="Understand how to use Azure Service Bus triggers and bindings in Azure Functions."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="azure functions, functions, event processing, dynamic compute, serverless architecture"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure Functions Service Bus triggers and bindings for queues and topics

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

This article explains how to configure and code Azure Service Bus triggers and bindings in Azure Functions. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="sbtrigger"></a>Service Bus queue or topic trigger

#### <a name="functionjson"></a>function.json

The *function.json* file specifies the following properties.

- `name`: The variable name used in function code for the queue or topic, or the queue or topic message. 
- `queueName`: For queue trigger only, the name of the queue to poll.
- `topicName`: For topic trigger only, the name of the topic to poll.
- `subscriptionName`: For topic trigger only, the subscription name.
- `connection`: The name of an app setting that contains a Service Bus connection string. The connection string must be for a Service Bus namespace, not limited to a specific queue or topic. If the connection string doesn't have manage rights, set the `accessRights` property. If you leave `connection` empty, the trigger or binding will work with the default Service Bus connection string for the function app, which is specified by the AzureWebJobsServiceBus app setting.
- `accessRights`: Specifies the access rights available for the connection string. Default value is `manage`. Set to `listen` if you're using a connection string that doesn't provide manage permissions. Otherwise the Functions runtime might try and fail to do operations that require manage rights.
- `type`: Must be set to *serviceBusTrigger*.
- `direction`: Must be set to *in*. 

Example *function.json* for a Service Bus queue trigger:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>C# code example that processes a Service Bus queue message

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>F# code example that processes a Service Bus queue message

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>Node.js code example that processes a Service Bus queue message

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>Supported types

The Service Bus queue message can be deserialized to any of the following types:

* Object (from JSON)
* string
* byte array 
* `BrokeredMessage`(C#) 

#### <a id="sbpeeklock"></a>PeekLock behavior

The Functions runtime receives a message in `PeekLock` mode and calls `Complete` on the message if the function finishes successfully, or calls `Abandon` if the function fails. If the function runs longer than the `PeekLock` timeout, the lock is automatically renewed.

#### <a id="sbpoison"></a>Poison message handling

Service Bus does its own poison message handling which can't be controlled or configured in Azure Functions configuration or code. 

#### <a id="sbsinglethread"></a>Single-threading

By default the Functions runtime processes multiple queue messages concurrently. To direct the runtime to process only a single queue or topic message at a time, set `serviceBus.maxConcurrrentCalls` to 1 in the *host.json* file. For information about the *host.json* file, see [Folder Structure](functions-reference.md#folder-structure) in the Developer reference article, and [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) in the WebJobs.Script repository wiki.

## <a id="sboutput"></a>Service Bus queue or topic output binding

#### <a name="functionjson"></a>function.json

The *function.json* file specifies the following properties.

- `name`: The variable name used in function code for the queue or queue message. 
- `queueName`: For queue trigger only, the name of the queue to poll.
- `topicName`: For topic trigger only, the name of the topic to poll.
- `subscriptionName`: For topic trigger only, the subscription name.
- `connection`: Same as for Service Bus trigger.
- `accessRights`: Specifies the access rights available for the connection string. Default value is `manage`. Set to `send` if you're using a connection string that doesn't provide manage permissions. Otherwise the Functions runtime might try and fail to do operations that require manage rights, such as creating queues.
- `type`: Must be set to *serviceBus*.
- `direction`: Must be set to *out*. 

Example *function.json* for using a timer trigger to write Service Bus queue messages:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>Supported types

Azure Functions can create a Service Bus queue message from any of the following types.

* Object (always creates a JSON message, creates the message with a null object if the value is null when the function ends)
* string (creates a message if the value is non-null when the function ends)
* byte array (works like string) 
* `BrokeredMessage`(C#, works like string)

For creating multiple messages in a C# function, you can use `ICollector<T>` or `IAsyncCollector<T>`. A message is created when you call the `Add` method.

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>C# code examples that create Service Bus queue messages

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>F# code example that creates a Service Bus queue message

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>Node.js code example that creates a Service Bus queue message

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>Next steps

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 