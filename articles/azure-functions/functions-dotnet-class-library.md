---
title: Azure Functions C# referentie voor ontwikkelaars
description: Begrijpen hoe Azure-functies met C# ontwikkelen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/12/2017
ms.author: glenga
ms.openlocfilehash: 3de1e9b042a7a356c3c88e604e1e26c256d85657
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-functions-c-developer-reference"></a>Azure Functions C# referentie voor ontwikkelaars

<!-- When updating this article, make corresponding changes to any duplicate content in functions-reference-csharp.md -->

Dit artikel bevat een inleiding tot Azure Functions ontwikkelen met behulp van C# in .NET-klassebibliotheken.

Azure Functions ondersteunt C# en C# script programmeertalen. Als u zoekt richtlijnen op [met C# in de Azure portal](functions-create-function-app-portal.md), Zie [C#-script (.csx) referentie voor ontwikkelaars](functions-reference-csharp.md).

In dit artikel wordt ervan uitgegaan dat u de volgende artikelen al hebt gelezen:

* [Azure Functions-handleiding voor ontwikkelaars](functions-reference.md)
* [2017 van Visual Studio-hulpprogramma's van Azure Functions](functions-develop-vs.md)

## <a name="functions-class-library-project"></a>Functies class library-project

In Visual Studio de **Azure Functions** projectsjabloon maakt u een C# class library-project met de volgende bestanden:

* [host.JSON](functions-host-json.md) -configuratie-instellingen die van invloed zijn op alle functies in het project bij het uitvoeren van lokaal of in Azure worden opgeslagen.
* [Local.Settings.JSON](functions-run-local.md#local-settings-file) -app-instellingen en verbindingsreeksen die worden gebruikt bij het uitvoeren van lokaal worden opgeslagen.

### <a name="functionname-and-trigger-attributes"></a>Functienaam en trigger kenmerken

In een class-bibliotheek is een functie een statische methode met een `FunctionName` en een trigger-kenmerk, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
} 
```

De `FunctionName` kenmerk markeert de methode als een functie-ingangspunt. De naam moet uniek zijn binnen een project.

Het kenmerk trigger geeft het triggertype en invoergegevens koppelt aan een methodeparameter. De voorbeeldfunctie wordt geactiveerd door een wachtrijbericht en bericht uit de wachtrij wordt doorgegeven aan de methode in de `myQueueItem` parameter.

### <a name="additional-binding-attributes"></a>Aanvullende binding kenmerken

Extra invoer en uitvoer binding kenmerken kunnen worden gebruikt. Het volgende voorbeeld wijzigt het vorige item door een binding van de wachtrij uitvoer toe te voegen. De functie schrijft de invoerwachtrij-bericht naar een nieuwe wachtrijbericht in een andere wachtrij.

```csharp
public static class SimpleExampleWithOutput
{
    [FunctionName("CopyQueueMessage")]
    public static void Run(
        [QueueTrigger("myqueue-items-source")] string myQueueItem, 
        [Queue("myqueue-items-destination")] out string myQueueItemCopy,
        TraceWriter log)
    {
        log.Info($"CopyQueueMessage function processed: {myQueueItem}");
        myQueueItemCopy = myQueueItem;
    }
}
```

### <a name="conversion-to-functionjson"></a>Conversie naar function.json

De buildproces maakt een *function.json* bestand in de map van een functie in de build-map. Dit bestand is niet bedoeld om rechtstreeks worden bewerkt. Bindingconfiguratie wijzigen kan of de functie uitschakelen door dit bestand te bewerken. 

Het doel van dit bestand informatie wordt verstrekt aan de controller schaal moet worden gebruikt voor [schalen beslissingen te nemen aan het plan verbruik](functions-scale.md#how-the-consumption-plan-works). Daarom heeft het bestand alleen trigger info, geen invoer of uitvoer bindingen.

De gegenereerde *function.json* bestand bevat een `configurationSource` eigenschap bevatten die de runtime .NET-kenmerken worden gebruikt voor bindingen, plaats *function.json* configuratie. Hier volgt een voorbeeld:

```json
{
  "generatedBy": "Microsoft.NET.Sdk.Functions-1.0.0.0",
  "configurationSource": "attributes",
  "bindings": [
    {
      "type": "queueTrigger",
      "queueName": "%input-queue-name%",
      "name": "myQueueItem"
    }
  ],
  "disabled": false,
  "scriptFile": "..\\bin\\FunctionApp1.dll",
  "entryPoint": "FunctionApp1.QueueTrigger.Run"
}
```

De *function.json* Bestandsgeneratie wordt uitgevoerd door het NuGet-pakket [Microsoft\.NET\.Sdk\.functies](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). De broncode is beschikbaar in de GitHub-repo [azure\-functies\-tegenover\-bouwen\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="supported-types-for-bindings"></a>Ondersteunde typen voor bindingen

Elke binding heeft een eigen ondersteunde typen; bijvoorbeeld, een blob-trigger-kenmerk kan worden toegepast op een tekenreeksparameter gebruikt, een parameter POCO een `CloudBlockBlob` parameter of een van de diverse andere ondersteunde typen. De [verwijzingsartikel binding voor blob-bindingen](functions-bindings-storage-blob.md#trigger---usage) parametertypen voor een lijst met alle ondersteund. Zie voor meer informatie [Triggers en bindingen](functions-triggers-bindings.md) en de [binding verwijzing docs voor elk bindingstype](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>Het binden aan de retourwaarde van methode

Zoals weergegeven in het volgende voorbeeld, kunt u de geretourneerde waarde van een methode voor een binding uitvoer:

```csharp
public static class ReturnValueOutputBinding
{
    [FunctionName("CopyQueueMessageUsingReturnValue")]
    [return: Queue("myqueue-items-destination")]
    public static string Run(
        [QueueTrigger("myqueue-items-source-2")] string myQueueItem,
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
        return myQueueItem;
    }
}
```

## <a name="writing-multiple-output-values"></a>Schrijven van meerdere uitvoerwaarden

Als meerdere waarden opslaan op een uitvoer-binding, gebruiken de [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) of [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) typen. Deze typen zijn alleen-schrijven verzamelingen die zijn geschreven voor de binding uitvoer wanneer de methode is voltooid.

In dit voorbeeld meerdere Wachtrijberichten schrijft naar de dezelfde wachtrij met behulp `ICollector`:

```csharp
public static class ICollectorExample
{
    [FunctionName("CopyQueueMessageICollector")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-3")] string myQueueItem,
        [Queue("myqueue-items-destination")] ICollector<string> myQueueItemCopy,
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
        myQueueItemCopy.Add($"Copy 1: {myQueueItem}");
        myQueueItemCopy.Add($"Copy 2: {myQueueItem}");
    }
}
```

## <a name="logging"></a>Logboekregistratie

Om te registreren uitvoer in uw streaminglogboeken in C#, een argument van het type bevatten `TraceWriter`. Het is raadzaam dat u deze de naam `log`. Vermijd het gebruik van `Console.Write` in Azure Functions. 

`TraceWriter`is gedefinieerd in de [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs). Het logboek-niveau voor `TraceWriter` kunnen worden geconfigureerd in [host.json](functions-host-json.md).

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
} 
```

> [!NOTE]
> Voor informatie over een nieuwere framework voor logboekregistratie die u in plaats van gebruiken kunt `TraceWriter`, Zie [schrijven Logboeken in C#-functies](functions-monitoring.md#write-logs-in-c-functions) in de **Monitor Azure Functions** artikel.

## <a name="async"></a>Async

Als u een functie asynchrone, gebruikt u de `async` sleutelwoord en retourneren een `Task` object.

```csharp
public static class AsyncExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token,
        TraceWriter log)
    {
        log.Info($"BlobCopy function processed.");
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

## <a name="cancellation-tokens"></a>Annulering tokens

Bepaalde bewerkingen vereisen correct afsluiten. Terwijl het is verstandig code schrijven waarmee gecrasht kan verwerken, in gevallen waarin u wilt verwerken aanvragen afsluiten, definieert een [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument opgegeven.  Een `CancellationToken` wordt geleverd om aan te geven dat een afsluiten van de host is geactiveerd.

```csharp
public static class CancellationTokenExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

## <a name="environment-variables"></a>Omgevingsvariabelen

Als u een omgevingsvariabele of een app die waarde instellen, gebruikt `System.Environment.GetEnvironmentVariable`, zoals weergegeven in het volgende voorbeeld:

```csharp
public static class EnvironmentVariablesExample
{
    [FunctionName("GetEnvironmentVariables")]
    public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
    {
        log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
        log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
        log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    }

    public static string GetEnvironmentVariable(string name)
    {
        return name + ": " +
            System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
    }
}
```

## <a name="binding-at-runtime"></a>Binding tijdens runtime

In C# en andere .NET-talen, kunt u een [imperatieve](https://en.wikipedia.org/wiki/Imperative_programming) binding patroon, niet de [ *declaratieve* ](https://en.wikipedia.org/wiki/Declarative_programming) bindingen in kenmerken. Imperatieve binding is handig wanneer bindende parameters moeten worden berekend tijdens runtime in plaats van ontwerp. Met dit patroon, kunt u binden aan de ondersteunde invoer en uitvoer bindingen op het moment in uw functiecode.

Definieer een imperatieve binding als volgt:

- **Geen** een kenmerk opnemen in de functiehandtekening voor uw gewenste imperatieve bindingen.
- Geeft een invoerparameter [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) of [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- De volgende C#-patroon gebruiken om uit te voeren van de gegevensbinding.

  ```cs
  using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
  {
      ...
  }
  ```

  `BindingTypeAttribute`is de .NET-kenmerk dat de binding wordt gedefinieerd en `T` is een invoer- of -type dat wordt ondersteund door deze bindingstype. `T`kan niet een `out` parametertype (zoals `out JObject`). Bijvoorbeeld, de tabel Mobile Apps binding ondersteunt uitvoer [zes typen uitvoer](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), maar u kunt alleen [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) of [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)met imperatieve binding.

### <a name="single-attribute-example"></a>Voorbeeld van één kenmerk

De volgende voorbeeldcode maakt een [Storage-blob uitvoer binding](functions-bindings-storage-blob.md#output) met blob pad dat gedefinieerd tijdens runtime, hierna schrijft een tekenreeks naar de blob.

```cs
public static class IBinderExample
{
    [FunctionName("CreateBlobUsingBinder")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-4")] string myQueueItem,
        IBinder binder,
        TraceWriter log)
    {
        log.Info($"CreateBlobUsingBinder function processed: {myQueueItem}");
        using (var writer = binder.Bind<TextWriter>(new BlobAttribute(
                    $"samples-output/{myQueueItem}", FileAccess.Write)))
        {
            writer.Write("Hello World!");
        };
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definieert de [Storage-blob](functions-bindings-storage-blob.md) invoer of uitvoer van de binding en [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) is een ondersteunde binding uitvoertype.

### <a name="multiple-attribute-example"></a>Voorbeeld van meerdere kenmerk

Het voorgaande voorbeeld wordt de app-instelling voor de functie-app belangrijkste account opslagverbindingsreeks (dit is `AzureWebJobsStorage`). U kunt een aangepaste app-instelling te gebruiken voor het opslagaccount door toe te voegen opgeven de [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) en het doorgeven van de kenmerk-matrix in `BindAsync<T>()`. Gebruik een `Binder` parameter, niet `IBinder`.  Bijvoorbeeld:

```cs
public static class IBinderExampleMultipleAttributes
{
    [FunctionName("CreateBlobInDifferentStorageAccount")]
    public async static Task RunAsync(
            [QueueTrigger("myqueue-items-source-binder2")] string myQueueItem,
            Binder binder,
            TraceWriter log)
    {
        log.Info($"CreateBlobInDifferentStorageAccount function processed: {myQueueItem}");
        var attributes = new Attribute[]
        {
        new BlobAttribute($"samples-output/{myQueueItem}", FileAccess.Write),
        new StorageAccountAttribute("MyStorageAccount")
        };
        using (var writer = await binder.BindAsync<TextWriter>(attributes))
        {
            await writer.WriteAsync("Hello World!!");
        }
    }
}
```

## <a name="triggers-and-bindings"></a>Triggers en bindingen 

De volgende tabel bevat de trigger en binding-kenmerken die beschikbaar in een Azure Functions class library-project zijn. Alle kenmerken zijn in de naamruimte `Microsoft.Azure.WebJobs`.

| Trigger | Invoer | Uitvoer|
|------   | ------    | ------  |
| [BlobTrigger](functions-bindings-storage-blob.md#trigger---attributes)| [Blob](functions-bindings-storage-blob.md#input---attributes)| [Blob](functions-bindings-storage-blob.md#output---attributes)|
| [CosmosDBTrigger](functions-bindings-cosmosdb.md#trigger---attributes)| [DocumentDB](functions-bindings-cosmosdb.md#input---attributes)| [DocumentDB](functions-bindings-cosmosdb.md#output---attributes) |
| [EventHubTrigger](functions-bindings-event-hubs.md#trigger---attributes)|| [EventHub](functions-bindings-event-hubs.md#output---attributes) |
| [HTTPTrigger](functions-bindings-http-webhook.md#trigger---attributes)|||
| [QueueTrigger](functions-bindings-storage-queue.md#trigger---attributes)|| [Queue](functions-bindings-storage-queue.md#output---attributes) |
| [ServiceBusTrigger](functions-bindings-service-bus.md#trigger---attributes)|| [ServiceBus](functions-bindings-service-bus.md#output---attributes) |
| [TimerTrigger](functions-bindings-timer.md#attributes) | ||
| |[ApiHubFile](functions-bindings-external-file.md)| [ApiHubFile](functions-bindings-external-file.md)|
| |[MobileTable](functions-bindings-mobile-apps.md#input---attributes)| [MobileTable](functions-bindings-mobile-apps.md#output---attributes) | 
| |[Tabel](functions-bindings-storage-table.md#input---attributes)| [Tabel](functions-bindings-storage-table.md#output---attributes)  | 
| ||[NotificationHub](functions-bindings-notification-hubs.md#attributes) |
| ||[SendGrid](functions-bindings-sendgrid.md#attributes) |
| ||[Twilio](functions-bindings-twilio.md#attributes)| 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over triggers en bindingen](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Meer informatie over best practices voor Azure Functions](functions-best-practices.md)
