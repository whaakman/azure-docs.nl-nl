---
title: Azure Functions C# script developer reference
description: Lees hoe u voor het ontwikkelen van Azure Functions met behulp van C#-script.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 12/12/2017
ms.author: glenga
ms.openlocfilehash: 232a235cdbf9dc3934bdac14f9612d6865718823
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58892412"
---
# <a name="azure-functions-c-script-csx-developer-reference"></a>Azure Functions C#-script (.csx) referentie voor ontwikkelaars

<!-- When updating this article, make corresponding changes to any duplicate content in functions-dotnet-class-library.md -->

In dit artikel bevat een inleiding tot Azure Functions ontwikkelen met behulp van C#-script (*.csx*).

Azure Functions biedt ondersteuning voor C# en C#-script programmeertalen. Als u zoekt richtlijnen op [met C# in een Visual Studio-klassebibliotheekproject](functions-develop-vs.md), Zie [C#-naslaginformatie](functions-dotnet-class-library.md).

In dit artikel wordt ervan uitgegaan dat u al hebt gelezen de [handleiding voor ontwikkelaars van Azure Functions voor](functions-reference.md).

## <a name="how-csx-works"></a>De werking van .csx

De C#-script-ervaring voor Azure Functions is gebaseerd op de [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki/Introduction). Gegevens worden overgebracht naar de C#-functie via argumenten van de methode. Argumentnamen zijn opgegeven in een `function.json` -bestand en er zijn vooraf gedefinieerde namen voor toegang tot objecten, zoals de functie logger en annulering tokens.

De *.csx* indeling kunt u minder 'standaard' schrijven en te concentreren op het schrijven van slechts een C#-functie. In plaats van de wrapping alles in een naamruimte en klassenaam, definieert u alleen een `Run` methode. Een assembly-verwijzingen en naamruimten aan het begin van het bestand zoals gebruikelijk bevatten.

Een functie-app *.csx* bestanden worden gecompileerd wanneer een exemplaar wordt geïnitialiseerd. Deze compilatiestap betekent onder andere koude start duurt mogelijk langer voor C#-script-functies in vergelijking met C#-klassebibliotheken. Deze compilatiestap is ook waarom C#-script-functies zijn bewerkt in Azure portal, terwijl C#-klassebibliotheken niet.

## <a name="folder-structure"></a>mapstructuur

De mapstructuur voor een project C#-script ziet er als volgt uit:

```
FunctionsProject
 | - MyFirstFunction
 | | - run.csx
 | | - function.json
 | | - function.proj
 | - MySecondFunction
 | | - run.csx
 | | - function.json
 | | - function.proj
 | - host.json
 | - extensions.csproj
 | - bin
```

Er is een gedeelde [host.json](functions-host-json.md) -bestand dat kan worden gebruikt voor het configureren van de functie-app. Elke functie heeft een eigen codebestand (.csx) en de binding-configuratiebestand (function.json).

De binding-extensies vereist in [versie 2.x](functions-versions.md) van de functies runtime zijn gedefinieerd in de `extensions.csproj` bestand met de werkelijke dll-bestanden in de `bin` map. Als u lokaal ontwikkelt, moet u [bindinguitbreidingen registreren](./functions-bindings-register.md#local-development-azure-functions-core-tools). Bij het ontwikkelen van functies in Azure portal, geldt deze registratie voor u.

## <a name="binding-to-arguments"></a>Binding met argumenten

Invoer- of -gegevens is gebonden aan een C#-script functieparameter via de `name` eigenschap in de *function.json* configuratiebestand. Het volgende voorbeeld wordt een *function.json* bestand en *run.csx* -bestand voor een wachtrij geactiveerde functie. De parameter die gegevens van het wachtrijbericht ontvangt heet `myQueueItem` omdat dat de waarde van de `name` eigenschap.

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem.AsString}");
}
```

De `#r` instructie wordt uitgelegd [verderop in dit artikel](#referencing-external-assemblies).

## <a name="supported-types-for-bindings"></a>Ondersteunde typen voor bindingen

Elke binding heeft een eigen ondersteunde typen; bijvoorbeeld, een blobtrigger kan worden gebruikt met een queryreeks-parameter, een POCO-parameter, een `CloudBlockBlob` parameter of een van verschillende andere ondersteunde typen. De [binding naslagartikel voor blob-bindingen](functions-bindings-storage-blob.md#trigger---usage) een lijst met alle ondersteunde parametertypen voor blobtriggers. Zie voor meer informatie, [Triggers en bindingen](functions-triggers-bindings.md) en de [binding referentiedocumenten voor elk bindingstype](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="referencing-custom-classes"></a>Verwijst naar een aangepaste klassen

Als u nodig hebt om een aangepaste klasse van gewone oude CLR Object (POCO) te gebruiken, kunt u de klassendefinitie in hetzelfde bestand bevatten of in een afzonderlijk bestand plaatsen.

Het volgende voorbeeld wordt een *run.csx* voorbeeld met een POCO-klassedefinitie.

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

Een POCO-klasse beschikken over een getter en setter gedefinieerd voor elke eigenschap.

## <a name="reusing-csx-code"></a>Hergebruik van code .csx

U kunt klassen en methoden die zijn gedefinieerd in andere *.csx* bestanden in uw *run.csx* bestand. Om dit te doen, gebruik `#load` richtlijnen in uw *run.csx* bestand. In het volgende voorbeeld wordt een routine logboekregistratie met de naam `MyLogger` wordt gedeeld in *myLogger.csx* en geladen in *run.csx* met behulp van de `#load` richtlijn:

Voorbeeld *run.csx*:

```csharp
#load "mylogger.csx"

using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Voorbeeld *mylogger.csx*:

```csharp
public static void MyLogger(ILogger log, string logtext)
{
    log.LogInformation(logtext);
}
```

Met behulp van een gedeelde *.csx* bestand is een algemeen patroon wanneer u ten zeerste typt u de gegevens die worden doorgegeven tussen functies met behulp van een POCO-object. In het volgende eenvoudige voorbeeld wordt een HTTP-trigger en wachtrijtrigger delen een POCO-object met de naam `Order` sterk de volgorde om gegevens te typen:

Voorbeeld *run.csx* voor HTTP-trigger:

```cs
#load "..\shared\order.csx"

using System.Net;
using Microsoft.Extensions.Logging;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function received an order.");
    log.LogInformation(req.ToString());
    log.LogInformation("Submitting to processing queue.");

    if (req.orderId == null)
    {
        return new HttpResponseMessage(HttpStatusCode.BadRequest);
    }
    else
    {
        await outputQueueItem.AddAsync(req);
        return new HttpResponseMessage(HttpStatusCode.OK);
    }
}
```

Voorbeeld *run.csx* voor wachtrijtrigger:

```cs
#load "..\shared\order.csx"

using System;
using Microsoft.Extensions.Logging;

public static void Run(Order myQueueItem, out Order outputQueueItem, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed order...");
    log.LogInformation(myQueueItem.ToString());

    outputQueueItem = myQueueItem;
}
```

Voorbeeld *order.csx*:

```cs
public class Order
{
    public string orderId {get; set; }
    public string custName {get; set;}
    public string custAddress {get; set;}
    public string custEmail {get; set;}
    public string cartId {get; set; }

    public override String ToString()
    {
        return "\n{\n\torderId : " + orderId +
                  "\n\tcustName : " + custName +             
                  "\n\tcustAddress : " + custAddress +             
                  "\n\tcustEmail : " + custEmail +             
                  "\n\tcartId : " + cartId + "\n}";             
    }
}
```

U kunt een relatief pad met de `#load` richtlijn:

* `#load "mylogger.csx"` een bestand in de map van de functie laadt.
* `#load "loadedfiles\mylogger.csx"` een bestand in een map in de map van de functie laadt.
* `#load "..\shared\mylogger.csx"` een bestand in een map op hetzelfde niveau als de functie een map, dat wil zeggen, laadt direct onder de *wwwroot*.

De `#load` richtlijn werkt alleen met *.csx* bestanden, niet met *.cs* bestanden.

## <a name="binding-to-method-return-value"></a>Binding met de geretourneerde waarde methode

U kunt de geretourneerde waarde van een methode voor een Uitvoerbinding met behulp van de naam van de `$return` in *function.json*. Zie voor voorbeelden van [Triggers en bindingen](./functions-bindings-return-value.md).

De geretourneerde waarde alleen gebruiken als een geslaagde uitvoering van een functie altijd in een retourwaarde resulteert doorgeven aan de Uitvoerbinding. Gebruik anders `ICollector` of `IAsyncCollector`, zoals wordt weergegeven in de volgende sectie.

## <a name="writing-multiple-output-values"></a>Meerdere uitvoerwaarden schrijven

Meerdere waarden naar een Uitvoerbinding schrijven, of als een geslaagde functieaanroepen kan niet leiden tot iets moet worden doorgegeven aan de Uitvoerbinding, gebruikt u de [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) of [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) typen. Deze typen zijn alleen-schrijven verzamelingen die zijn geschreven naar de Uitvoerbinding wanneer de methode is voltooid.

In dit voorbeeld schrijft u meerdere berichten in wachtrij plaatsen in de dezelfde wachtrij met behulp van `ICollector`:

```csharp
public static void Run(ICollector<string> myQueue, ILogger log)
{
    myQueue.Add("Hello");
    myQueue.Add("World!");
}
```

## <a name="logging"></a>Logboekregistratie

Om aan te melden uitvoer naar de streaminglogboeken in C#, een argument van het type bevatten [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger). Het is raadzaam dat u deze de naam `log`. Vermijd het gebruik van `Console.Write` in Azure Functions.

```csharp
public static void Run(string myBlob, ILogger log)
{
    log.LogInformation($"C# Blob trigger function processed: {myBlob}");
}
```

> [!NOTE]
> Voor informatie over een nieuwere framework voor logboekregistratie die u in plaats van gebruiken kunt `TraceWriter`, Zie [schrijven Logboeken in C#-functies](functions-monitoring.md#write-logs-in-c-functions) in de **Monitor Azure Functions** artikel.

## <a name="async"></a>Async

Om te maken van een functie [asynchrone](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/), gebruikt u de `async` sleutelwoord en keer terug een `Task` object.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096);
}
```

U kunt geen gebruiken `out` parameters in de asynchrone functies. Voor uitvoerbindingen, gebruikt u de [functie retourwaarde](#binding-to-method-return-value) of een [object collector](#writing-multiple-output-values) in plaats daarvan.

## <a name="cancellation-tokens"></a>Annuleringstokens

Een functie accepteert een [CancellationToken](/dotnet/api/system.threading.cancellationtoken) parameter waarmee het besturingssysteem op uw code op de hoogte gesteld wanneer de functie op het punt te worden beëindigd. U kunt deze melding kunt gebruiken om te controleren of dat de functie niet onverwacht wordt beëindigd op een manier die blijven de gegevens in een inconsistente status.

Het volgende voorbeeld laat zien hoe om te controleren voor aanstaande beëindiging van een functie.

```csharp
using System;
using System.IO;
using System.Threading;

public static void Run(
    string inputText,
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
```

## <a name="importing-namespaces"></a>Naamruimten importeren

Als u importeren van naamruimten wilt, kunt u doen dus gebruikelijke met de `using` component.

```csharp
using System.Net;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log)
```

De volgende naamruimten automatisch worden geïmporteerd en daarom optioneel zijn:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a>Verwijst naar een externe assembly 's

Voor framework-assembly's, voeg verwijzingen toe met behulp van de `#r "AssemblyName"` richtlijn.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log)
```

De volgende assembly's worden automatisch toegevoegd door de omgeving voor het hosten van Azure Functions:

* `mscorlib`
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`

De volgende assembly's kunnen worden verwezen door eenvoudige-naam (bijvoorbeeld `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a>Verwijst naar een aangepaste assembly 's

Als u wilt verwijzen naar een aangepaste assembly, kunt u ofwel een *gedeelde* assembly of een *persoonlijke* assembly:

* Gedeelde assembly's worden verdeeld over alle functies in een functie-app. Om te verwijzen naar een aangepaste assembly, uploadt u de assembly naar een map met de naam `bin` in uw [functie-app-hoofdmap](functions-reference.md#folder-structure) (wwwroot).

* Privé-assembly's maken deel uit van een bepaalde functie context en ondersteuning voor sideloading van verschillende versies. Privé-assembly's moeten worden geüpload een `bin` map in de functie-map. Verwijzen naar de assembly's met behulp van de bestandsnaam, zoals `#r "MyAssembly.dll"`.

Zie de sectie voor informatie over het uploaden van bestanden naar de map van uw functie op [van Pakketbeheer](#using-nuget-packages).

### <a name="watched-directories"></a>Gecontroleerde mappen

De map waarin het scriptbestand functie wordt automatisch weergegeven voor wijzigingen in de assembly's. Als u wilt bekijken voor assembly wijzigingen in andere directory's, voeg deze toe aan de `watchDirectories` in lijst [host.json](functions-host-json.md).

## <a name="using-nuget-packages"></a>Met behulp van NuGet-pakketten
Het gebruik van NuGet-pakketten in een 2.x C# werken, upload een *function.proj* -bestand naar de map van de functie in het bestandssysteem van de functie-app. Hier volgt een voorbeeld *function.proj* -bestand dat wordt toegevoegd een verwijzing naar *Microsoft.ProjectOxford.Face* versie *1.1.0*:

```xml
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
    </PropertyGroup>
    
    <ItemGroup>
        <PackageReference Include="Microsoft.ProjectOxford.Face" Version="1.1.0" />
    </ItemGroup>
</Project>
```

Geef voor het gebruik van een aangepaste NuGet-feed, de feed in een *Nuget.Config* bestand in de hoofdmap van de functie-App. Zie voor meer informatie, [NuGet configureren gedrag](/nuget/consume-packages/configuring-nuget-behavior). 

> [!NOTE]
> In 1.x C# functies, NuGet-pakketten zijn waarnaar wordt verwezen met een *project.json* in plaats van het bestand een *function.proj* bestand.

Gebruik voor de 1.x-functies, een *project.json* in plaats daarvan het bestand. Hier volgt een voorbeeld *project.json* bestand: 

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

### <a name="using-a-functionproj-file"></a>Met behulp van een bestand function.proj

1. De functie openen in de Azure-portal. De uitvoer van de installatie van pakket wordt weergegeven in het tabblad Logboeken.
2. Het uploaden van een *function.proj* bestand, gebruik een van de methoden die worden beschreven in de [het bijwerken van de functie app-bestanden](functions-reference.md#fileupdate) in het naslagonderwerp voor ontwikkelaars van Azure Functions.
3. Na de *function.proj* bestand is geüpload, ziet u uitvoer zoals in het volgende voorbeeld in de functie de streaming-logboek:

```
2018-12-14T22:00:48.658 [Information] Restoring packages.
2018-12-14T22:00:48.681 [Information] Starting packages restore
2018-12-14T22:00:57.064 [Information] Restoring packages for D:\local\Temp\9e814101-fe35-42aa-ada5-f8435253eb83\function.proj...
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\function.proj...
2018-12-14T22:01:00.844 [Information] Installing Newtonsoft.Json 10.0.2.
2018-12-14T22:01:01.041 [Information] Installing Microsoft.ProjectOxford.Common.DotNetStandard 1.0.0.
2018-12-14T22:01:01.140 [Information] Installing Microsoft.ProjectOxford.Face.DotNetStandard 1.0.0.
2018-12-14T22:01:09.799 [Information] Restore completed in 5.79 sec for D:\local\Temp\9e814101-fe35-42aa-ada5-f8435253eb83\function.proj.
2018-12-14T22:01:10.905 [Information] Packages restored.
```

## <a name="environment-variables"></a>Omgevingsvariabelen

Als u een omgevingsvariabele of een app-instelling waarde, gebruikt u `System.Environment.GetEnvironmentVariable`, zoals weergegeven in het volgende codevoorbeeld:

```csharp
public static void Run(TimerInfo myTimer, ILogger log)
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
```

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime"></a>Binding tijdens runtime

In C# en andere .NET-talen, kunt u een [imperatieve](https://en.wikipedia.org/wiki/Imperative_programming) binding patroon, plaats de [ *declaratieve* ](https://en.wikipedia.org/wiki/Declarative_programming) bindingen in *function.json*. Imperatieve binding is handig als de bindende parameters moeten worden berekend tijdens runtime in plaats van ontwerp. Met dit patroon, kunt u koppelt aan de ondersteunde invoer en uitvoer-bindingen op het begeven in uw functiecode aan te geven.

Definieer het concurrentievermogen binding als volgt:

- **Geen** een vermelding in *function.json* voor uw gewenste imperatieve bindingen.
- Geeft een invoerparameter [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) of [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Gebruik de volgende C#-patroon om uit te voeren van de gegevensbinding.

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

`BindingTypeAttribute` is van het .NET-kenmerk dat de binding wordt gedefinieerd en `T` is een invoer- of -type dat wordt ondersteund door dit bindingstype. `T` mag niet een `out` parametertype (zoals `out JObject`). Bijvoorbeeld, de mobiele Apps voor tabeluitvoer binding ondersteunt [zes typen uitvoer](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), maar u kunt alleen [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) of [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)voor `T`.

### <a name="single-attribute-example"></a>Voorbeeld van één kenmerk

Hiermee maakt u de volgende voorbeeldcode een [Storage-blob Storage-Uitvoerbinding](functions-bindings-storage-blob.md#output) met blob-pad dat gedefinieerd tijdens runtime, schrijft u een tekenreeks naar de blob.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    using (var writer = await binder.BindAsync<TextWriter>(new BlobAttribute("samples-output/path")))
    {
        writer.Write("Hello World!!");
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definieert de [Storage-blob](functions-bindings-storage-blob.md) invoer of uitvoer verbinding maakt, en [TextWriter](/dotnet/api/system.io.textwriter) is een ondersteunde binding uitvoertype.

### <a name="multiple-attribute-example"></a>Voorbeeld van meerdere kenmerk

Het vorige voorbeeld wordt de app-instelling voor de functie-app hoofd verbindingsreeks Opslagaccount (dit is `AzureWebJobsStorage`). Kunt u een aangepaste app-instelling te gebruiken voor het opslagaccount door toe te voegen de [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) en door te geven de kenmerk-matrix in `BindAsync<T>()`. Gebruik een `Binder` parameter niet `IBinder`.  Bijvoorbeeld:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    var attributes = new Attribute[]
    {    
        new BlobAttribute("samples-output/path"),
        new StorageAccountAttribute("MyStorageAccount")
    };

    using (var writer = await binder.BindAsync<TextWriter>(attributes))
    {
        writer.Write("Hello World!");
    }
}
```

De volgende tabel bevat de .NET-kenmerken voor elk bindingstype en de pakketten waarin ze zijn gedefinieerd.

> [!div class="mx-codeBreakAll"]
> | Binding | Kenmerk | Verwijzing toevoegen |
> |------|------|------|
> | Cosmos DB | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.CosmosDB"` |
> | Event Hubs | [`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
> | Mobile Apps | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
> | Notification Hubs | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
> | Service Bus | [`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
> | Opslagwachtrij | [`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
> | Opslagblob | [`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
> | Storage-tabel | [`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
> | Twilio | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over triggers en bindingen](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Meer informatie over aanbevolen procedures voor Azure Functions](functions-best-practices.md)
