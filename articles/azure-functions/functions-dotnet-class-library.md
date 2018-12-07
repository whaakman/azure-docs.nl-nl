---
title: Azure Functions C# referentie voor ontwikkelaars
description: Lees hoe u voor het ontwikkelen van Azure Functions met C#.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/12/2018
ms.author: glenga
ms.openlocfilehash: 7e84e8e99000e9d8bd7a21d343588b1df777b56d
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52994541"
---
# <a name="azure-functions-c-developer-reference"></a>Azure Functions C# referentie voor ontwikkelaars

<!-- When updating this article, make corresponding changes to any duplicate content in functions-reference-csharp.md -->

In dit artikel bevat een inleiding tot Azure Functions ontwikkelen met behulp van C# in .NET-klassebibliotheken.

Azure Functions biedt ondersteuning voor C# en C#-script programmeertalen. Als u zoekt richtlijnen op [met C# in Azure portal](functions-create-function-app-portal.md), Zie [C#-script (.csx) referentie voor ontwikkelaars](functions-reference-csharp.md).

In dit artikel wordt ervan uitgegaan dat u de volgende artikelen al hebt gelezen:

* [Azure Functions-handleiding voor ontwikkelaars](functions-reference.md)
* [Visual Studio 2017-hulpprogramma's van Azure Functions](functions-develop-vs.md)

## <a name="functions-class-library-project"></a>Functions-klassebibliotheekproject

In Visual Studio, de **Azure Functions** projectsjabloon, maken een C#-klasse bibliotheek-project gemaakt met de volgende bestanden:

* [host.JSON](functions-host-json.md) -configuratie-instellingen die van invloed zijn op alle functies in het project bij het uitvoeren van lokaal of in Azure worden opgeslagen.
* [Local.Settings.JSON](functions-run-local.md#local-settings-file) -app-instellingen en verbindingsreeksen die worden gebruikt bij het uitvoeren van lokaal worden opgeslagen. Dit bestand bevat geheimen en niet worden gepubliceerd in uw functie-app in Azure. In plaats daarvan moet u [app-instellingen toevoegen aan uw functie-app](functions-develop-vs.md#function-app-settings).

Wanneer u het project bouwt, wordt in een mapstructuur die lijkt op het volgende wordt gegenereerd in de build directory uitvoer:

```
<framework.version>
 | - bin
 | - MyFirstFunction
 | | - function.json
 | - MySecondFunction
 | | - function.json
 | - host.json
```

Deze map is wat wordt geïmplementeerd naar uw functie-app in Azure. De binding-extensies vereist in [versie 2.x](functions-versions.md) van de functies runtime zijn [toegevoegd aan het project als NuGet-pakketten](functions-triggers-bindings.md#c-class-library-with-visual-studio-2017).

> [!IMPORTANT]
> De buildproces maakt een *function.json* -bestand voor elke functie. Dit *function.json* bestand is niet bedoeld voor rechtstreeks worden bewerkt. Bindingsconfiguratie wijzigen kan of de functie uitschakelen door dit bestand te bewerken. Zie voor informatie over het uitschakelen van een functie, [functies uitschakelen](disable-function.md#functions-2x---c-class-libraries).

## <a name="methods-recognized-as-functions"></a>Methoden die als functies

In een klassenbibliotheek is een functie een statische methode met een `FunctionName` en een triggerkenmerk, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
} 
```

De `FunctionName` kenmerk markeert de methode als een functie-ingangspunt. De naam moet uniek zijn binnen een project, met een letter beginnen en alleen letters, cijfers, spaties `_` en `-`, maximaal 127 tekens. Project-sjablonen maken vaak een methode met de naam `Run`, maar de naam van de methode kan een geldige C#-methode-naam.

De triggerkenmerk geeft het triggertype en invoergegevens koppelt aan een parameter van de methode. De voorbeeldfunctie wordt geactiveerd door een wachtrijbericht en bericht uit de wachtrij wordt doorgegeven aan de methode in de `myQueueItem` parameter.

## <a name="method-signature-parameters"></a>Methodeparameters handtekening

Handtekening van de methode kan parameters dan de regio die wordt gebruikt in combinatie met het triggerkenmerk bevatten. Hier volgen enkele van de extra parameters die u kunt opnemen:

* [Invoer- en uitvoerbindingen](functions-triggers-bindings.md) als zodanig gemarkeerd door met het inrichten van deze kenmerken.  
* Een `ILogger` of `TraceWriter` ([versie 1.x-alleen](functions-versions.md#creating-1x-apps))-parameter voor [logboekregistratie](#logging).
* Een `CancellationToken` parameter voor [correct afsluiten](#cancellation-tokens).
* [Binding van expressies](functions-triggers-bindings.md#binding-expressions-and-patterns) parameters om op te halen de metagegevens van activeren.

De volgorde van de parameters in de functiehandtekening niet van belang. Bijvoorbeeld, kunt u trigger parameters plaatsen vóór of na de andere bindingen en kunt u de parameter logger plaatsen vóór of na een trigger of bindende parameters.

### <a name="output-binding-example"></a>Voorbeeld van uitvoer-binding

Het volgende voorbeeld wijzigt de voorgaande één door een wachtrij-Uitvoerbinding toe te voegen. De functie schrijft het wachtrijbericht dat de functie aan een nieuwe wachtrijbericht in een andere wachtrij activeert.

```csharp
public static class SimpleExampleWithOutput
{
    [FunctionName("CopyQueueMessage")]
    public static void Run(
        [QueueTrigger("myqueue-items-source")] string myQueueItem, 
        [Queue("myqueue-items-destination")] out string myQueueItemCopy,
        ILogger log)
    {
        log.LogInformation($"CopyQueueMessage function processed: {myQueueItem}");
        myQueueItemCopy = myQueueItem;
    }
}
```

De binding-naslaginformatie ([opslagwachtrijen](functions-bindings-storage-queue.md), bijvoorbeeld) wordt uitgelegd welke typen methodeparameters kunt u met de trigger, invoer of uitvoer binding kenmerken.

### <a name="binding-expressions-example"></a>Voorbeeld van de binding-expressies

De volgende code haalt u de naam van de wachtrij voor het bewaken van een app-instelling en krijgt de aanmaaktijd van de wachtrij-bericht de `insertionTime` parameter.

```csharp
public static class BindingExpressionsExample
{
    [FunctionName("LogQueueMessage")]
    public static void Run(
        [QueueTrigger("%queueappsetting%")] string myQueueItem,
        DateTimeOffset insertionTime,
        ILogger log)
    {
        log.LogInformation($"Message content: {myQueueItem}");
        log.LogInformation($"Created at: {insertionTime}");
    }
}
```

## <a name="autogenerated-functionjson"></a>Automatisch gegenereerde function.json

De buildproces maakt een *function.json* bestand in een functie in de build-map. Als u eerder hebt genoteerd, wordt dit bestand is niet bedoeld rechtstreeks worden bewerkt. Bindingsconfiguratie wijzigen kan of de functie uitschakelen door dit bestand te bewerken. 

Het doel van dit bestand informatie wordt verstrekt aan de scale-controller moet worden gebruikt voor [beslissingen schalen op het abonnement consumption](functions-scale.md#how-the-consumption-plan-works). Om deze reden heeft het bestand alleen trigger info, geen invoer of uitvoerbindingen.

Het gegenereerde *function.json* bestand bevat een `configurationSource` eigenschap waarin de runtime gebruikmaken van .NET-kenmerken voor bindingen, in plaats van *function.json* configuratie. Hier volgt een voorbeeld:

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

## <a name="microsoftnetsdkfunctions"></a>Microsoft.NET.Sdk.Functions

De *function.json* bestand wordt gemaakt via het NuGet-pakket [Microsoft\.NET\.Sdk\.functies](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). 

Hetzelfde pakket wordt gebruikt voor beide versie 1.x en 2.x van de Functions-runtime. Het doelframework is wat een 1.x-project van een project 2.x onderscheidt. Hier vindt u belangrijke onderdelen van *.csproj* bestanden, met verschillende frameworks en hetzelfde doel `Sdk` pakket:

**Functies 1.x**

```xml
<PropertyGroup>
  <TargetFramework>net461</TargetFramework>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

**Functies 2.x**

```xml
<PropertyGroup>
  <TargetFramework>netstandard2.0</TargetFramework>
  <AzureFunctionsVersion>v2</AzureFunctionsVersion>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

Tussen de `Sdk` pakketafhankelijkheden zijn triggers en bindingen. Een 1.x-project verwijst naar de 1.x-triggers en bindingen omdat die zijn gericht op het .NET Framework, terwijl 2.x triggers en bindingen gericht op .NET Core.

De `Sdk` pakket ook afhankelijk van [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json), en klik op indirect [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage). Deze afhankelijkheden Zorg ervoor dat het project gebruikmaakt van de versies van de pakketten die met Functions runtime-versie werken die de project-doelen. Bijvoorbeeld, `Newtonsoft.Json` versie 11 voor .NET Framework 4.6.1 is, maar de Functions-runtime die gericht is op .NET Framework 4.6.1 is alleen compatibel is met `Newtonsoft.Json` 9.0.1. Zodat uw functiecode aan te geven in dat project ook heeft gebruik van `Newtonsoft.Json` 9.0.1.

De broncode voor `Microsoft.NET.Sdk.Functions` is beschikbaar in de GitHub-opslagplaats [azure\-functies\-vs\-bouwen\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="runtime-version"></a>Runtime-versi

Visual Studio gebruikt het [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools) om uit te voeren van Functions-projecten. De essentiële hulpprogramma is een opdrachtregelinterface voor de Functions-runtime.

Als u de Core Tools installeren met behulp van npm, die niet van invloed op Core-hulpprogramma's die worden gebruikt door Visual Studio. Voor de Functions-runtime-versie 1.x, Visual Studio opgeslagen versies in Core Tools *%USERPROFILE%\AppData\Local\Azure.Functions.Cli* en maakt er gebruik van de meest recente versie die zijn opgeslagen. Voor 2.x functies, de essentiële hulpprogramma zijn opgenomen in de **Azure Functions en Webjobs-hulpprogramma's** extensie. Voor zowel 1.x en 2.x, kunt u zien welke versie wordt gebruikt in de console-uitvoer wanneer u een Functions-project uitvoeren:

```terminal
[3/1/2018 9:59:53 AM] Starting Host (HostId=contoso2-1518597420, Version=2.0.11353.0, ProcessId=22020, Debug=False, Attempt=0, FunctionsExtensionVersion=)
```

## <a name="supported-types-for-bindings"></a>Ondersteunde typen voor bindingen

Elke binding heeft een eigen ondersteunde typen; bijvoorbeeld, een blob-trigger-kenmerk kan worden toegepast op een queryreeks-parameter, een POCO-parameter, een `CloudBlockBlob` parameter of een van verschillende andere ondersteunde typen. De [binding naslagartikel voor blob-bindingen](functions-bindings-storage-blob.md#trigger---usage) een lijst met alle ondersteunde parametertypen. Zie voor meer informatie, [Triggers en bindingen](functions-triggers-bindings.md) en de [binding referentiedocumenten voor elk bindingstype](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>Binding met de geretourneerde waarde methode

U kunt de geretourneerde waarde van een methode voor een Uitvoerbinding gebruiken door het kenmerk toe te passen op de geretourneerde waarde van de methode. Zie voor voorbeelden van [Triggers en bindingen](functions-triggers-bindings.md#using-the-function-return-value). 

De geretourneerde waarde alleen gebruiken als een geslaagde uitvoering van een functie altijd in een retourwaarde resulteert doorgeven aan de Uitvoerbinding. Gebruik anders `ICollector` of `IAsyncCollector`, zoals wordt weergegeven in de volgende sectie.

## <a name="writing-multiple-output-values"></a>Meerdere uitvoerwaarden schrijven

Meerdere waarden naar een Uitvoerbinding schrijven, of als een geslaagde functieaanroepen kan niet leiden tot iets moet worden doorgegeven aan de Uitvoerbinding, gebruikt u de [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) of [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) typen. Deze typen zijn alleen-schrijven verzamelingen die zijn geschreven naar de Uitvoerbinding wanneer de methode is voltooid.

In dit voorbeeld schrijft u meerdere berichten in wachtrij plaatsen in de dezelfde wachtrij met behulp van `ICollector`:

```csharp
public static class ICollectorExample
{
    [FunctionName("CopyQueueMessageICollector")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-3")] string myQueueItem,
        [Queue("myqueue-items-destination")] ICollector<string> myDestinationQueue,
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
        myDestinationQueue.Add($"Copy 1: {myQueueItem}");
        myDestinationQueue.Add($"Copy 2: {myQueueItem}");
    }
}
```

## <a name="logging"></a>Logboekregistratie

Om aan te melden uitvoer naar de streaminglogboeken in C#, een argument van het type bevatten [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger). Het is raadzaam dat u deze de naam `log`, zoals in het volgende voorbeeld:  

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
} 
```

Vermijd het gebruik van `Console.Write` in Azure Functions. Zie voor meer informatie, [schrijven Logboeken in C# functies](functions-monitoring.md#write-logs-in-c-functions) in de **Monitor Azure Functions** artikel.

## <a name="async"></a>Asynchrone

Om te maken van een functie [asynchrone](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/), gebruikt u de `async` sleutelwoord en keer terug een `Task` object.

```csharp
public static class AsyncExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token,
        ILogger log)
    {
        log.LogInformation($"BlobCopy function processed.");
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

U kunt geen gebruiken `out` parameters in de asynchrone functies. Voor uitvoerbindingen, gebruikt u de [functie retourwaarde](#binding-to-method-return-value) of een [object collector](#writing-multiple-output-values) in plaats daarvan.

## <a name="cancellation-tokens"></a>Annuleringstokens

Een functie accepteert een [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) parameter waarmee het besturingssysteem op uw code op de hoogte gesteld wanneer de functie op het punt te worden beëindigd. U kunt deze melding kunt gebruiken om te controleren of dat de functie niet onverwacht wordt beëindigd op een manier die blijven de gegevens in een inconsistente status.

Het volgende voorbeeld laat zien hoe om te controleren voor aanstaande beëindiging van een functie.

```csharp
public static class CancellationTokenExample
{
    public static void Run(
        [QueueTrigger("inputqueue")] string inputText,
        TextWriter logger,
        CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(5000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }
}
```

## <a name="environment-variables"></a>Omgevingsvariabelen

Als u een omgevingsvariabele of een app-instelling waarde, gebruikt u `System.Environment.GetEnvironmentVariable`, zoals weergegeven in het volgende codevoorbeeld:

```csharp
public static class EnvironmentVariablesExample
{
    [FunctionName("GetEnvironmentVariables")]
    public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, ILogger log)
    {
        log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
        log.LogInformation(GetEnvironmentVariable("AzureWebJobsStorage"));
        log.LogInformation(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    }

    public static string GetEnvironmentVariable(string name)
    {
        return name + ": " +
            System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
    }
}
```

App-instellingen kunnen worden gelezen uit omgevingsvariabelen, zowel bij het lokaal ontwikkelen en bij het uitvoeren in Azure. Bij het lokaal ontwikkelen, appinstellingen zijn afkomstig van de `Values` verzameling in de *local.settings.json* bestand. In beide omgevingen, lokale en Azure, `GetEnvironmentVariable("<app setting name>")` haalt de waarde van de benoemde app-instelling. Bijvoorbeeld, wanneer u lokaal uitvoert, "Mijn Site-Name" zou worden geretourneerd als uw *local.settings.json* -bestand bevat `{ "Values": { "WEBSITE_SITE_NAME": "My Site Name" } }`.

De [System.Configuration.ConfigurationManager.AppSettings](https://docs.microsoft.com/dotnet/api/system.configuration.configurationmanager.appsettings) eigenschap is een alternatieve API voor het ophalen van app-instellingswaarden, maar het is raadzaam dat u `GetEnvironmentVariable` zoals hier wordt weergegeven.

## <a name="binding-at-runtime"></a>Binding tijdens runtime

In C# en andere .NET-talen, kunt u een [imperatieve](https://en.wikipedia.org/wiki/Imperative_programming) binding patroon, plaats de [ *declaratieve* ](https://en.wikipedia.org/wiki/Declarative_programming) bindingen in kenmerken. Imperatieve binding is handig als de bindende parameters moeten worden berekend tijdens runtime in plaats van ontwerp. Met dit patroon, kunt u koppelt aan de ondersteunde invoer en uitvoer-bindingen op het begeven in uw functiecode aan te geven.

Definieer het concurrentievermogen binding als volgt:

- **Geen** opnemen van een kenmerk in de functiehandtekening voor uw gewenste imperatieve bindingen.
- Geeft een invoerparameter [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) of [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Gebruik de volgende C#-patroon om uit te voeren van de gegevensbinding.

  ```cs
  using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
  {
      ...
  }
  ```

  `BindingTypeAttribute` is van het .NET-kenmerk dat de binding wordt gedefinieerd en `T` is een invoer- of -type dat wordt ondersteund door dit bindingstype. `T` mag niet een `out` parametertype (zoals `out JObject`). Bijvoorbeeld, de mobiele Apps voor tabeluitvoer binding ondersteunt [zes typen uitvoer](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), maar u kunt alleen [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) of [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)met imperatieve binding.

### <a name="single-attribute-example"></a>Voorbeeld van één kenmerk

Hiermee maakt u de volgende voorbeeldcode een [Storage-blob Storage-Uitvoerbinding](functions-bindings-storage-blob.md#output) met blob-pad dat gedefinieerd tijdens runtime, schrijft u een tekenreeks naar de blob.

```cs
public static class IBinderExample
{
    [FunctionName("CreateBlobUsingBinder")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-4")] string myQueueItem,
        IBinder binder,
        ILogger log)
    {
        log.LogInformation($"CreateBlobUsingBinder function processed: {myQueueItem}");
        using (var writer = binder.Bind<TextWriter>(new BlobAttribute(
                    $"samples-output/{myQueueItem}", FileAccess.Write)))
        {
            writer.Write("Hello World!");
        };
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definieert de [Storage-blob](functions-bindings-storage-blob.md) invoer of uitvoer verbinding maakt, en [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) is een ondersteunde binding uitvoertype.

### <a name="multiple-attribute-example"></a>Voorbeeld van meerdere kenmerk

Het vorige voorbeeld wordt de app-instelling voor de functie-app hoofd verbindingsreeks Opslagaccount (dit is `AzureWebJobsStorage`). Kunt u een aangepaste app-instelling te gebruiken voor het opslagaccount door toe te voegen de [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) en door te geven de kenmerk-matrix in `BindAsync<T>()`. Gebruik een `Binder` parameter niet `IBinder`.  Bijvoorbeeld:

```cs
public static class IBinderExampleMultipleAttributes
{
    [FunctionName("CreateBlobInDifferentStorageAccount")]
    public async static Task RunAsync(
            [QueueTrigger("myqueue-items-source-binder2")] string myQueueItem,
            Binder binder,
            ILogger log)
    {
        log.LogInformation($"CreateBlobInDifferentStorageAccount function processed: {myQueueItem}");
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

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over triggers en bindingen](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Meer informatie over aanbevolen procedures voor Azure Functions](functions-best-practices.md)
