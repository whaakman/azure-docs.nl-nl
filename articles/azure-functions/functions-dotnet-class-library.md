---
title: .NET-klassebibliotheken gebruiken met Azure Functions | Microsoft Docs
description: Meer informatie over het ontwerpen van .NET-klassebibliotheken voor gebruik met Azure Functions
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: 9f5db0c2-a88e-4fa8-9b59-37a7096fc828
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/10/2017
ms.author: glenga
ms.openlocfilehash: a3bc07623505371b4f3c230ebadeb577a70fdb5e
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="using-net-class-libraries-with-azure-functions"></a>Met behulp van .NET-klassebibliotheken met Azure Functions

Naast de scriptbestanden, Azure Functions ondersteunt een class-bibliotheek publiceren als de uitvoering voor een of meer functies. Het is raadzaam dat u de [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).

## <a name="prerequisites"></a>Vereisten 

In dit artikel heeft de volgende vereisten:

- [Visual Studio 2017 versie 15,3](https://www.visualstudio.com/vs/), of een latere versie.
- Installeer de **ontwikkelen van Azure** werkbelasting.

## <a name="functions-class-library-project"></a>Functies class library-project

Vanuit Visual Studio een nieuw Azure Functions-project te maken. Het nieuwe projectsjabloon maakt u de bestanden *host.json* en *local.settings.json*. U kunt [aanpassen van Azure Functions-runtime-instellingen in host.json](functions-host-json.md). 

Het bestand *local.settings.json* app-instellingen, verbindingsreeksen en instellingen voor Azure Functions Core hulpprogramma's worden opgeslagen. Zie voor meer informatie over de structuur, [Code en testen van Azure functions lokaal](functions-run-local.md#local-settings).

### <a name="functionname-attribute"></a>Functienaam kenmerk

Het kenmerk [ `FunctionNameAttribute` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) markeert een methode als een functie-ingangspunt. Deze moet worden gebruikt met exact één trigger en 0 of meer invoer en uitvoer bindingen.

### <a name="conversion-to-functionjson"></a>Conversie naar function.json

Wanneer u een Azure Functions-project maakt een *function.json* bestand wordt gemaakt in de directory van de functie. De mapnaam is hetzelfde als de functie naam die de `[FunctionName]` kenmerk wordt opgegeven. De *function.json* -bestand bevat triggers en bindingen en verwijst naar het project assembly-bestand.

Deze conversie wordt uitgevoerd door het NuGet-pakket [Microsoft\.NET\.Sdk\.functies](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). De bron is beschikbaar in de GitHub-repo [azure\-functies\-tegenover\-bouwen\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="triggers-and-bindings"></a>Triggers en bindingen 

De volgende tabel bevat de triggers en bindingen die beschikbaar in een Azure Functions class library-project zijn. Alle kenmerken zijn in de naamruimte `Microsoft.Azure.WebJobs`.

| Binding | Kenmerk | NuGet-pakket |
|------   | ------    | ------        |
| [BLOB storage trigger, invoer, uitvoer](#blob-storage) | [BlobAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | [Blobopslag] |
| [Cosmos DB trigger](#cosmos-db) | [CosmosDBTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions.DocumentDB] | 
| [Cosmos DB invoer en uitvoer](#cosmos-db) | [DocumentDBAttribute] | [Microsoft.Azure.WebJobs.Extensions.DocumentDB] |
| [Event Hubs-trigger en uitvoer](#event-hub) | [EventHubTriggerAttribute], [EventHubAttribute] | [Microsoft.Azure.WebJobs.ServiceBus] |
| [Bestand met externe invoer en uitvoer](#api-hub) | [ApiHubFileAttribute] | [Microsoft.Azure.WebJobs.Extensions.ApiHub] |
| [HTTP- en webhook trigger](#http) | [HttpTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions.Http] |
| [Mobile Apps-invoer en uitvoer](#mobile-apps) | [MobileTableAttribute] | [Microsoft.Azure.WebJobs.Extensions.MobileApps] | 
| [Notification Hubs-uitvoer](#nh) | [NotificationHubAttribute] | [Microsoft.Azure.WebJobs.Extensions.NotificationHubs] | 
| [Queue storage trigger en uitvoer](#queue) | [QueueAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | 
| [SendGrid-uitvoer](#sendgrid) | [SendGridAttribute] | [Microsoft.Azure.WebJobs.Extensions.SendGrid] | 
| [Service Bus-trigger en uitvoer](#service-bus) | [ServiceBusAttribute], [ServiceBusAccountAttribute] | [Microsoft.Azure.WebJobs.ServiceBus]
| [Table storage invoer en uitvoer](#table) | [TableAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | 
| [Timertrigger](#timer) | [TimerTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions] | 
| [Twilio-uitvoer](#twilio) | [TwilioSmsAttribute] | [Microsoft.Azure.WebJobs.Extensions.Twilio] | 

<a name="blob-storage"></a>

### <a name="blob-storage-trigger-input-bindings-and-output-bindings"></a>BLOB storage worden geactiveerd, bindingen invoer en uitvoer bindingen

Azure Functions ondersteunt worden geactiveerd, invoer en uitvoer van de bindingen voor Azure Blob-opslag. Zie voor meer informatie over expressies voor gegevensbinding en metagegevens [bindingen van Azure Functions Blob storage](functions-bindings-storage-blob.md).

Een blob-trigger is gedefinieerd met de `[BlobTrigger]` kenmerk. U kunt het kenmerk `[StorageAccount]` voor het definiëren van de naam van de app-instelling met de verbindingsreeks naar het storage-account dat wordt gebruikt door een hele functie of de klasse.

```csharp
[StorageAccount("AzureWebJobsStorage")]
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

BLOB-invoer en uitvoer worden gedefinieerd met behulp van de `[Blob]` kenmerk toe, samen met een `FileAccess` parameter die aangeeft lezen of schrijven. Het volgende voorbeeld wordt een blob trigger en blob uitvoer binding.

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```        

<a name="cosmos-db"></a>

### <a name="cosmos-db-trigger-input-bindings-and-output-bindings"></a>Cosmos DB-trigger bindingen invoer en uitvoer bindingen

Azure Functions ondersteunt triggers en invoer en uitvoer van de bindingen voor Cosmos-DB. Zie voor meer informatie over de functies van de Cosmos-DB-binding, [bindingen van Azure Functions Cosmos DB](functions-bindings-documentdb.md).

Het kenmerk gebruiken om te activeren uit een Cosmos-DB-document, `[CosmosDBTrigger]` in het NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.DocumentDB]. Het volgende voorbeeld-triggers met een specifiek `database` en `collection`. De instelling `myCosmosDB` bevat de verbinding met het exemplaar van de Cosmos-DB. 

```csharp
[FunctionName("DocumentUpdates")]
public static void Run(
    [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
IReadOnlyList<Document> documents, TraceWriter log)
{
        log.Info("Documents modified " + documents.Count);
        log.Info("First document Id " + documents[0].Id);
}
```

Als u wilt koppelen aan een Cosmos-DB-document, gebruikt u het kenmerk `[DocumentDB]` in het NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.DocumentDB]. Het volgende voorbeeld heeft de trigger van een wachtrij en een DocumentDB-API uitvoer van de binding.

```csharp
[FunctionName("QueueToDocDB")]        
public static void Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, 
    [DocumentDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
{
    document = new { Text = myQueueItem, id = Guid.NewGuid() };
}

```

<a name="event-hub"></a>

### <a name="event-hubs-trigger-and-output"></a>Event Hubs-trigger en uitvoer

Azure Functions ondersteunt activeren en uitvoer van de bindingen voor Event Hubs. Zie voor meer informatie [bindingen van Azure Functions Event Hub](functions-bindings-event-hubs.md).

De typen `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` en `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` zijn gedefinieerd in het NuGet-pakket [Microsoft.Azure.WebJobs.ServiceBus]. 

Het volgende voorbeeld wordt een Event Hub-trigger:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Het volgende voorbeeld heeft een Event Hub uitvoer met behulp van de geretourneerde waarde van de methode als uitvoer:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

<a name="api-hub"></a>

### <a name="external-file-input-and-output"></a>Bestand met externe invoer en uitvoer

Azure Functions ondersteunt trigger, invoer en uitvoer bindingen voor externe bestanden, zoals Google Drive, Dropbox en OneDrive. Zie voor meer informatie, [bindingen van Azure Functions-bestand met externe](functions-bindings-external-file.md). De kenmerken `[ExternalFileTrigger]` en `[ExternalFile]` zijn gedefinieerd in het NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.ApiHub].

De volgende C#-voorbeeld wordt een bestand met externe invoer en uitvoer van de binding. De code wordt het bestand voor invoer gekopieerd naar het uitvoerbestand.

```csharp
[StorageAccount("MyStorageConnection")]
[FunctionName("ExternalFile")]
[return: ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}-Copy", FileAccess.Write)]
public static string Run([QueueTrigger("myqueue-items")] string myQueueItem, 
    [ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}", FileAccess.Read)] string myInputFile, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    return myInputFile;
}
```

<a name="http"></a>

### <a name="http-and-webhooks"></a>HTTP en webhooks

Gebruik de `HttpTrigger` kenmerk voor het definiëren van een HTTP-trigger of webhook. Dit kenmerk wordt gedefinieerd in het NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.Http]. U kunt de autorisatieniveau, webhook-type, route en methoden. Het volgende voorbeeld definieert een HTTP-trigger met anonieme verificatie en _genericJson_ webhook-type.

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a name="mobile-apps"></a>

### <a name="mobile-apps-input-and-output"></a>Mobile Apps-invoer en uitvoer

Azure Functions ondersteunt invoer en uitvoer van de bindingen voor mobiele Apps. Zie voor meer informatie, [mobiele Apps van Azure Functions bindingen](functions-bindings-mobile-apps.md). Het kenmerk `[MobileTable]` is gedefinieerd in het NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.MobileApps].

Het volgende voorbeeld toont een Mobile Apps binding uitvoer:

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

<a name="nh"></a>

### <a name="notification-hubs-output"></a>Notification Hubs-uitvoer

Azure Functions ondersteunt een uitvoer-binding voor Notification Hubs. Zie voor meer informatie, [Azure Functions Notification Hub uitvoer binding](functions-bindings-notification-hubs.md). Het kenmerk `[NotificationHub]` is gedefinieerd in het NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].

<a name="queue"></a>

### <a name="queue-storage-trigger-and-output"></a>Queue storage trigger en uitvoer

Azure Functions ondersteunt activeren en uitvoer van de bindingen voor Azure wachtrijen. Zie voor meer informatie [bindingen van Azure Functions Queue Storage](functions-bindings-storage-queue.md).

Het volgende voorbeeld ziet u hoe u het retourtype van functie met de uitvoer van een wachtrij binding, met behulp van de `[Queue]` kenmerk. 

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    // HTTP trigger with queue output binding
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }
}

```

Gebruik het definiëren van een trigger voor de wachtrij de `[QueueTrigger]` kenmerk.
```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    // Queue trigger
    [FunctionName("QueueTrigger")]
    [StorageAccount("AzureWebJobsStorage")]
    public static void QueueTrigger([QueueTrigger("myqueue-items")] string myQueueItem, TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}

```


<a name="sendgrid"></a>

### <a name="sendgrid-output"></a>SendGrid-uitvoer

Azure Functions ondersteunt een SendGrid-uitvoer binding voor het programmatisch verzenden van e-mail. Zie voor meer informatie, [bindingen van Azure Functions SendGrid](functions-bindings-sendgrid.md).

Het kenmerk `[SendGrid]` is gedefinieerd in het NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.SendGrid]. Een binding SendGrid vereist een toepassingsinstelling benoemde `AzureWebJobsSendGridApiKey`, die uw SendGrid-API-sleutel bevat. Dit is de standaardnaam van de instelling voor uw SendGrid-API-sleutel. Als u meer dan één SendGrid sleutel of kies een andere Instellingsnaam hebben moet, kunt u instellen deze naam met behulp van de `ApiKey` eigenschap van de `SendGrid` binding-kenmerk, zoals in het volgende voorbeeld:

    [SendGrid(ApiKey = "MyCustomSendGridKeyName")]

Hieronder volgt een voorbeeld van een Service Bus-wachtrij-trigger gebruikt en een SendGrid uitvoer binding met `SendGridMessage`:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```
Opmerking dat in dit voorbeeld de SendGrid-API-sleutel heeft aan opslag in een toepassing instelling met de naam `AzureWebJobsSendGridApiKey`.

<a name="service-bus"></a>

### <a name="service-bus-trigger-and-output"></a>Service Bus-trigger en uitvoer

Azure Functions ondersteunt activeren en uitvoer van de bindingen voor Service Bus-wachtrijen en onderwerpen. Zie voor meer informatie over het configureren van bindingen [bindingen van Azure Functions Service Bus](functions-bindings-service-bus.md).

De kenmerken `[ServiceBusTrigger]` en `[ServiceBus]` zijn gedefinieerd in het NuGet-pakket [Microsoft.Azure.WebJobs.ServiceBus]. 

Hier volgt een voorbeeld van een Service Bus-wachtrij trigger:

```csharp
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run([ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

Hier volgt een voorbeeld van een Service Bus-uitvoer binding, met behulp van het retourtype van methode als uitvoer:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```        

<a name="table"></a>

### <a name="table-storage-input-and-output"></a>Table storage invoer en uitvoer

Azure Functions ondersteunt invoer en uitvoer van de bindingen voor Azure Table storage. Zie voor meer informatie, [bindingen van Azure Functions Table storage](functions-bindings-storage-table.md).

Het volgende voorbeeld is een klasse met twee functies, tabeluitvoer van opslag en invoer bindingen te demonstreren. 

```csharp
[StorageAccount("AzureWebJobsStorage")]
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, TraceWriter log)
    {
        log.Info($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }

    // use the metadata parameter "queueTrigger" to bind the queue payload
    [FunctionName("TableInput")]
    public static void TableInput([QueueTrigger("table-items")] string input, [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, TraceWriter log)
    {
        log.Info($"C# function processed: {poco.Text}");
    }
}

```

<a name="timer"></a>

### <a name="timer-trigger"></a>Timertrigger

Azure Functions heeft een timer trigger binding waarmee u de functiecode op basis van een ingesteld schema uitvoeren. Zie voor meer informatie over de functies van de binding, [plannen van de uitvoering van code met Azure Functions](functions-bindings-timer.md).

Op het plan verbruik, kunt u schema's met een [CRON expressie](http://en.wikipedia.org/wiki/Cron#CRON_expression). Als u een App Service-abonnement, kunt u ook een TimeSpan-tekenreeks. 

Het volgende voorbeeld definieert een timertrigger die elke vijf minuten wordt uitgevoerd:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

<a name="twilio"></a>

### <a name="twilio-output"></a>Twilio-uitvoer

Azure Functions ondersteunt Twilio uitvoer bindingen zodat uw functies SMS-berichten verzenden. Zie voor meer informatie, [verzenden SMS-berichten van de functies van Azure met behulp van de Twilio uitvoer binding](functions-bindings-twilio.md). 

Het kenmerk `[TwilioSms]` is gedefinieerd in het pakket [Microsoft.Azure.WebJobs.Extensions.Twilio].

Het volgende C#-voorbeeld wordt een wachtrij trigger en een Twilio binding uitvoer:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure Functions in C# scripting [Azure Functions C\# referentie voor ontwikkelaars script](functions-reference-csharp.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


<!-- NuGet packages --> 
[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta4
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1
[Microsoft.Azure.WebJobs.Extensions.BotFramework]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.BotFramework/1.0.15-beta
[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4
[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1


<!-- Links to source --> 
[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs
[CosmosDBTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/Trigger/CosmosDBTriggerAttribute.cs
[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs
[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs
[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs
[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs 
[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs
[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs
[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs
[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs
[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs
[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs
[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs
[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs
[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs
[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs
[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs
