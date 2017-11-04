---
title: Azure Functions C# Script-referentie voor ontwikkelaars | Microsoft Docs
description: Begrijpen hoe Azure-functies met C# ontwikkelen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.assetid: f28cda01-15f3-4047-83f3-e89d5728301c
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/07/2017
ms.author: glenga
ms.openlocfilehash: 18d3a87da8c240a3153dfa68f9b1d8bd17bbe693
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="azure-functions-c-script-developer-reference"></a>Azure Functions C# script referentie voor ontwikkelaars
[!INCLUDE [functions-selector-languages](../../includes/functions-selector-languages.md)]

De C# script-ervaring voor Azure Functions is gebaseerd op de Azure WebJobs SDK. Gegevensstromen in uw C#-functie via argumenten van de methode. Argumentnamen zijn opgegeven in `function.json`, en er zijn vooraf gedefinieerde namen voor toegang tot de beheerder de functie berichtenlogboek en annulering tokens.

In dit artikel wordt ervan uitgegaan dat u al hebt gelezen de [naslaginformatie voor ontwikkelaars van Azure Functions](functions-reference.md).

Klasse voor informatie over het gebruik van C#-bibliotheken, Zie [met behulp van .NET-klassebibliotheken met Azure Functions](functions-dotnet-class-library.md).

## <a name="how-csx-works"></a>De werking van .csx
De `.csx` indeling kunt u minder 'standaard' schrijven en zich richten op het schrijven van slechts een C#-functie. Assembly-verwijzingen en naamruimten aan het begin van het bestand zoals gebruikelijk bevatten. In plaats van dat u alles verpakt in een naamruimte en klassenaam, net definiëren een `Run` methode. Als u wilt bijvoorbeeld alle klassen, zijn gewoon oude CLR Object (POCO) om objecten te definiëren, kunt u een klasse in hetzelfde bestand opnemen.   

## <a name="binding-to-arguments"></a>Binding met argumenten
De verschillende bindingen zijn gebonden aan een functie C# via de `name` eigenschap in de *function.json* configuratie. Elke binding heeft een eigen ondersteunde typen; een trigger blob kan bijvoorbeeld een tekenreeks, een POCO of een CloudBlockBlob ondersteunen. De ondersteunde typen zijn gedocumenteerd in de documentatie voor elke binding. Een POCO-object moet een getter en setter gedefinieerd voor elke eigenschap hebben.

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

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="using-method-return-value-for-output-binding"></a>Met behulp van de retourwaarde van de methode voor het binden van uitvoer

U kunt de geretourneerde waarde van een methode voor een binding uitvoer met de naam `$return` in *function.json*:

```json
{
    "type": "queue",
    "direction": "out",
    "name": "$return",
    "queueName": "outqueue",
    "connection": "MyStorageConnectionString",
}
```

```csharp
public static string Run(string input, TraceWriter log)
{
    return input;
}
```

## <a name="writing-multiple-output-values"></a>Schrijven van meerdere uitvoerwaarden

Als meerdere waarden opslaan op een uitvoer-binding, gebruiken de [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) of [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) typen. Deze typen zijn alleen-schrijven verzamelingen die zijn geschreven voor de binding uitvoer wanneer de methode is voltooid.

In dit voorbeeld meerdere Wachtrijberichten schrijft naar de dezelfde wachtrij met behulp `ICollector`:

```csharp
public static void Run(ICollector<string> myQueueItem, TraceWriter log)
{
    myQueueItem.Add("Hello");
    myQueueItem.Add("World!");
}
```

## <a name="logging"></a>Logboekregistratie
Om te registreren uitvoer in uw streaminglogboeken in C#, een argument van het type bevatten `TraceWriter`. Het is raadzaam dat u deze de naam `log`. Vermijd het gebruik van `Console.Write` in Azure Functions. 

`TraceWriter`is gedefinieerd in de [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs). Het logboek-niveau voor `TraceWriter` kunnen worden geconfigureerd in [host.json](functions-host-json.md).

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Asynchrone
Als u een functie asynchrone, gebruikt u de `async` sleutelwoord en retourneren een `Task` object.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096, token);
}
```

## <a name="cancellation-token"></a>Annulering token
Bepaalde bewerkingen vereisen correct afsluiten. Terwijl het is verstandig code schrijven waarmee gecrasht kan verwerken, in gevallen waar u wilt aanvragen voor het afhandelen van correct afsluiten, definieert u een [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument opgegeven.  Een `CancellationToken` wordt geleverd om aan te geven dat een afsluiten van de host is geactiveerd.

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName,
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Naamruimten importeren
Als u importeren, naamruimten wilt, kunt u doen dus gebruikelijke met de `using` component.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

De volgende naamruimten automatisch worden geïmporteerd en zijn daarom optioneel:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a>Verwijzen naar externe assembly 's
Voor de framework-assembly's, voeg verwijzingen toe met behulp van de `#r "AssemblyName"` richtlijn.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

De volgende assembly's worden automatisch toegevoegd door de Azure Functions hostomgeving:

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

De volgende assembly's kunnen worden verwezen met eenvoudige naam (bijvoorbeeld `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a>Aangepaste assembly's die verwijzen naar

Om te verwijzen naar een aangepaste assembly, kunt u ofwel een *gedeelde* assembly of een *persoonlijke* assembly:
- Gedeelde assembly's worden gedeeld door alle functies binnen een functie-app. Om te verwijzen naar een aangepaste assembly, uploadt u de assembly die de functie-app, zoals een `bin` map in de functie app-hoofdmap. 
- Persoonlijke assembly's deel uitmaken van een bepaalde functie context en ondersteuning voor sideloading van verschillende versies. Persoonlijke assembly's moeten worden geüpload een `bin` map in de functie Active directory. Verwijst naar de bestandsnaam, zoals met `#r "MyAssembly.dll"`. 

Zie voor meer informatie over de bestanden uploaden naar uw map voor de functie in de volgende sectie op het gebied van pakket.

### <a name="watched-directories"></a>Gecontroleerde mappen

De map waarin het scriptbestand functie wordt automatisch weergegeven voor wijzigingen in de assembly's. Als u wilt bekijken voor assembly-wijzigingen in andere directory's, voeg deze toe aan de `watchDirectories` in lijst [host.json](functions-host-json.md).

## <a name="using-nuget-packages"></a>NuGet-pakketten gebruiken
Als u NuGet-pakketten in een C#-functie, uploaden een *project.json* -bestand naar de map van de functie in het bestandssysteem van de functie-app. Hier volgt een voorbeeld *project.json* -bestand dat wordt toegevoegd een verwijzing naar versie 1.1.0 Microsoft.ProjectOxford.Face:

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

Alleen .NET Framework 4.6 wordt ondersteund, dus zorg ervoor dat uw *project.json* bestand geeft `net46` zoals hier wordt weergegeven.

Wanneer u uploadt een *project.json* bestand, de runtime opgehaald van de pakketten en verwijzingen naar de pakket-assembly's worden automatisch toegevoegd. U hoeft niet te voegen `#r "AssemblyName"` richtlijnen. Voeg de vereiste voor het gebruik van de typen gedefinieerd in de NuGet-pakketten `using` instructies voor uw *run.csx* bestand. 

In de runtime van Functions NuGet terugzetten werkt door te vergelijken `project.json` en `project.lock.json`. Als de stempels datum en tijd van de bestanden **niet** overeenkomst, een NuGet-herstelbewerking wordt uitgevoerd en NuGet downloads pakketten bijgewerkt. Echter, als de stempels datum en tijd van de bestanden **doen** overeenkomen, NuGet voert een terugzetbewerking. Daarom `project.lock.json` moeten niet worden geïmplementeerd omdat het ervoor zorgt dat NuGet package restore overslaan. U kunt de implementatie van het vergrendelingsbestand voorkomen de `project.lock.json` naar de `.gitignore` bestand.

Geef de feed in voor het gebruik van een aangepaste NuGet feed een *Nuget.Config* bestand in de hoofdmap van de functie-App. Zie voor meer informatie [NuGet configureren gedrag](/nuget/consume-packages/configuring-nuget-behavior).

### <a name="using-a-projectjson-file"></a>Met behulp van een bestand project.json
1. De functie openen in de Azure-portal. Het tabblad logboeken geeft de uitvoer van de installatie van pakket.
2. Als u wilt een project.json-bestand uploadt, gebruikt u een van de methoden die worden beschreven de [het bijwerken van de functie app-bestanden](functions-reference.md#fileupdate) in het naslagonderwerp voor ontwikkelaars van Azure Functions.
3. Na de *project.json* bestand wordt geüpload, ziet u uitvoer zoals in het volgende voorbeeld in de functie de streaming-logboek:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Omgevingsvariabelen
Als u een omgevingsvariabele of een app die waarde instellen, gebruikt `System.Environment.GetEnvironmentVariable`, zoals weergegeven in het volgende voorbeeld:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
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
```

## <a name="reusing-csx-code"></a>Hergebruik van code .csx
U kunt klassen en methoden die zijn gedefinieerd in andere *.csx* bestanden in uw *run.csx* bestand. Gebruik hiervoor `#load` richtlijnen op uw *run.csx* bestand. In het volgende voorbeeld wordt een routine logboekregistratie met de naam `MyLogger` wordt gedeeld *myLogger.csx* en geladen in *run.csx* met behulp van de `#load` richtlijn:

Voorbeeld *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Voorbeeld *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext);
}
```

Met behulp van een gedeelde *.csx* is een algemene patroon wanneer u wilt uw sterk typeargumenten tussen functies met een POCO-object. In de volgende eenvoudige voorbeeld een HTTP-trigger en wachtrij-trigger delen een POCO-object met de naam `Order` sterk de volgorde om gegevens te typen:

Voorbeeld *run.csx* voor HTTP-trigger:

```cs
#load "..\shared\order.csx"

using System.Net;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function received an order.");
    log.Info(req.ToString());
    log.Info("Submitting to processing queue.");

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

Voorbeeld *run.csx* voor wachtrij trigger:

```cs
#load "..\shared\order.csx"

using System;

public static void Run(Order myQueueItem, out Order outputQueueItem,TraceWriter log)
{
    log.Info($"C# Queue trigger function processed order...");
    log.Info(myQueueItem.ToString());

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

U kunt een relatief pad zijn met de `#load` richtlijn:

* `#load "mylogger.csx"`een bestand in de map van de functie laadt.
* `#load "loadedfiles\mylogger.csx"`een bestand in een map in de map functie laadt.
* `#load "..\shared\mylogger.csx"`een bestand in een map op hetzelfde niveau als de functie-map, dat wil zeggen, laadt rechtstreeks onder *wwwroot*.

De `#load` richtlijn werkt alleen met *.csx* (C# script)-bestanden, niet bij *.cs* bestanden.

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime-via-imperative-bindings"></a>Binding tijdens runtime via imperatieve bindingen

In C# en andere .NET-talen, kunt u een [imperatieve](https://en.wikipedia.org/wiki/Imperative_programming) binding patroon, niet de [ *declaratieve* ](https://en.wikipedia.org/wiki/Declarative_programming) bindingen in *function.json*. Imperatieve binding is handig wanneer bindende parameters moeten worden berekend tijdens runtime in plaats van ontwerp. U kunt met dit patroon binden aan de ondersteunde invoer en uitvoer van de binding op het moment in uw functiecode.

Definieer een imperatieve binding als volgt:

- **Geen** een vermelding in *function.json* voor uw gewenste imperatieve bindingen.
- Geeft een invoerparameter [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) of [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- De volgende C#-patroon gebruiken om uit te voeren van de gegevensbinding.

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

`BindingTypeAttribute`is de .NET-kenmerk dat de binding wordt gedefinieerd en `T` is de invoer- of -type dat wordt ondersteund door deze bindingstype. `T`ook kan niet een `out` parametertype (zoals `out JObject`). Bijvoorbeeld, de tabel Mobile Apps binding ondersteunt uitvoer [zes typen uitvoer](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), maar u kunt alleen [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) of [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) voor `T`.

De volgende voorbeeldcode maakt een [Storage-blob uitvoer binding](functions-bindings-storage-blob.md#using-a-blob-output-binding) met blob pad dat gedefinieerd tijdens runtime, hierna schrijft een tekenreeks naar de blob.

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

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definieert de [Storage-blob](functions-bindings-storage-blob.md) invoer of uitvoer van de binding en [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) is een ondersteunde binding uitvoertype.
In het vorige codevoorbeeld haalt de code de app-instelling voor de functie-app belangrijkste account opslagverbindingsreeks (dit is `AzureWebJobsStorage`). U kunt een aangepaste app-instelling te gebruiken voor het opslagaccount door toe te voegen opgeven de [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) en het doorgeven van de kenmerk-matrix in `BindAsync<T>()`. Bijvoorbeeld:

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

De volgende tabel bevat de .NET-kenmerken voor elk bindingstype en de pakketten waarin ze worden gedefinieerd.

> [!div class="mx-codeBreakAll"]
| Binding | Kenmerk | Verwijzing toevoegen |
|------|------|------|
| Cosmos DB | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.DocumentDB"` |
| Event Hubs | [`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
| Mobile Apps | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
| Notification Hubs | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
| Service Bus | [`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
| Storage-wachtrij | [`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Storage-blob | [`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Table Storage | [`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Twilio | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |



## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie:

* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
