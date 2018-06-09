---
title: Het gebruik van de Azure WebJobs SDK
description: Meer informatie over het schrijven van code voor de WebJobs SDK. Maak gebeurtenisafhankelijke achtergrond taken voor de verwerking die toegang gegevens in Azure-services en services van derden tot.
services: app-service\web, storage
documentationcenter: .net
author: tdykstra
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/27/2018
ms.author: tdykstra
ms.openlocfilehash: 08272ba7d828f744336723f25b482bf06b9e43dc
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234647"
---
# <a name="how-to-use-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Het gebruik van de Azure WebJobs SDK voor gebeurtenisafhankelijke achtergrondverwerking

In dit artikel biedt richtlijnen voor het schrijven van code voor [de Azure WebJobs SDK](webjobs-sdk-get-started.md). De documentatie van toepassing op versies 2.x en 3.x tenzij anders anders wordt vermeld. De belangrijkste wijziging geïntroduceerd door 3.x is het gebruik van .NET Core in plaats van .NET Framework.

>[!NOTE]
> [Azure Functions](../azure-functions/functions-overview.md) is gebaseerd op de WebJobs SDK en dit bevat artikelkoppelingen naar documentatie van Azure Functions voor sommige onderwerpen. Houd rekening met de volgende verschillen tussen de functies en de WebJobs SDK:
> * Azure Functions-versie 1.x komt overeen met de WebJobs SDK versie 2.x en Azure Functions 2.x komt overeen met de WebJobs SDK 3.x. Broncodeopslagplaatsen, volg de WebJobs SDK nummering en veel v2.x filialen, met de hoofdvertakking momenteel met 3.x-code hebben.
> * Voorbeeldcode voor Azure Functions C#-klassebibliotheken is net als de WebJobs SDK code, behalve hoeft u niet een `FunctionName` kenmerk in een WebJobs SDK-project.
> * Sommige bindingstypen worden alleen ondersteund in functies, zoals HTTP-webhook en gebeurtenis raster (die is gebaseerd op HTTP). 
> 
> Zie voor meer informatie [Vergelijk de WebJobs SDK en de Azure Functions](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs). 

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt gelezen [aan de slag met de WebJobs SDK](webjobs-sdk-get-started.md).

## <a name="jobhost"></a>JobHost

De `JobHost` object bevat de runtime voor functies: luistert deze naar de functies triggers en aanroepen. U maakt de `JobHost` in uw code en de code schrijven voor het aanpassen van het gedrag.

Dit is een belangrijk verschil tussen de WebJobs SDK rechtstreeks gebruiken en indirect met behulp van Azure Functions. In de Azure Functions, de service-besturingselementen de `JobHost`, en u kunt deze niet aanpassen door code te schrijven. Azure Functions, kunt u aanpassen gedrag van de host via instellingen in de *host.json* bestand. Deze instellingen zijn tekenreeksen, geen code, die beperkt de soorten aanpassingen die u kunt doen.

### <a name="jobhost-connection-strings"></a>Verbindingsreeksen JobHost

De WebJobs SDK wordt gezocht naar opslag en Service Bus verbindingsreeksen in *local.settings.json* wanneer u uitvoert lokaal of in de webtaak omgeving wanneer u in Azure uitvoert. Als u wilt uw eigen namen voor deze verbindingsreeksen gebruiken of deze ergens anders opslaan, kunt u deze instellen in code als volgt te werk:

```cs
static void Main(string[] args)
{
    var _storageConn = ConfigurationManager
        .ConnectionStrings["MyStorageConnection"].ConnectionString;

    var _dashboardConn = ConfigurationManager
        .ConnectionStrings["MyDashboardConnection"].ConnectionString;

    JobHostConfiguration config = new JobHostConfiguration();
    config.StorageConnectionString = _storageConn;
    config.DashboardConnectionString = _dashboardConn;
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="jobhost-development-settings"></a>Instellingen voor het ontwikkelen van JobHost

De `JobHostConfiguration` klasse heeft een `UseDevelopmentSettings` methode die u aanroepen kunt lokale ontwikkeling om efficiënter te maken. Hier volgen enkele van de instellingen die door deze methode wordt gewijzigd:

| Eigenschap | Ontwikkeling van instelling |
| ------------- | ------------- |
| `Tracing.ConsoleLevel` | `TraceLevel.Verbose` om te maximaliseren logboekuitvoer. |
| `Queues.MaxPollingInterval`  | Een lage waarde om te controleren of de wachtrij methoden onmiddellijk worden geactiveerd.  |
| `Singleton.ListenerLockPeriod` | 15 seconden voor snelle iteratieve ontwikkeling. |

Het volgende voorbeeld ziet hoe u instellingen voor het ontwikkelen. Om ervoor te `config.IsDevelopment` retourneren `true` bij lokale uitvoering, instellen van een lokale omgeving-variabele met de naam `AzureWebJobsEnv` met waarde `Development`.

```cs
static void Main()
{
    config = new JobHostConfiguration();

    if (config.IsDevelopment)
    {
        config.UseDevelopmentSettings();
    }

    var host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="jobhost-servicepointmanager-settings"></a>JobHost ServicePointManager instellingen

.NET Framework bevat een API aangeroepen [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) die bepaalt het aantal gelijktijdige verbindingen met een host. Het is raadzaam dat u deze waarde van de standaardwaarde van 2 voordat u uw host WebJobs verhogen.

Alle uitgaande HTTP-aanvragen die u in een functie met behulp van aanbrengt `HttpClient` doorstromen de `ServicePointManager`. Zodra u raakt de `DefaultConnectionLimit`, wordt de `ServicePointManager` queueing aanvragen gestart voordat ze worden verzonden. Stel dat uw `DefaultConnectionLimit` is ingesteld op 2 en uw code maakt 1000 HTTP-aanvragen. In eerste instantie slechts 2 aanvragen daadwerkelijk mogen via het besturingssysteem. De andere 998 in de wachtrij staan totdat er ruimte is voor deze. Dit betekent dat uw `HttpClient` time-out, mogelijk omdat het *denkt dat* deze heeft de aanvraag heeft ingediend, maar de aanvraag is nooit verzonden door het besturingssysteem naar de doelserver. Zo ziet u mogelijk gedrag die geen zinvol: uw lokale `HttpClient` duurt 10 seconden een aanvraag voltooid, maar uw-service retourneert elke aanvraag in 200 ms. 

De standaardwaarde voor ASP.NET-toepassingen is `Int32.MaxValue`, en dat zich waarschijnlijk geschikt voor WebJobs uitgevoerd in een App Service-abonnement Basic of hoger. WebJobs moet doorgaans de AlwaysOn-instelling en die wordt alleen ondersteund door App Service-abonnementen Basic en hoger. 

Als uw webtaak wordt uitgevoerd in een Free of Shared App Service-Plan, uw toepassing wordt beperkt door de sandbox-App Service, die momenteel is een [verbindingslimiet van 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#per-sandbox-per-appper-site-numerical-limits). Met een niet-afhankelijke verbindingslimiet in `ServicePointManager`, is het waarschijnlijker dat de sandbox verbinding drempelwaarde wordt bereikt en de site wordt afgesloten. In dat geval instelling `DefaultConnectionLimit` iets lager ligt, zoals 50 of 100, kan dit te voorkomen en nog steeds toestaan voor een voldoende doorvoer.

De instelling moet worden geconfigureerd voordat een HTTP-aanvragen worden gedaan. Daarom de WebJobs-host om aan te passen van de instelling mag niet probeert automatisch; mogelijk zijn er HTTP-aanvragen die optreden voordat de host wordt gestart en dit tot onverwacht gedrag leiden kan. De beste manier is de waarde direct in uw `Main` methode voor het initialiseren van de `JobHost`, zoals wordt weergegeven in het volgende voorbeeld

```csharp
static void Main(string[] args)
{
    // Set this immediately so that it is used by all requests.
    ServicePointManager.DefaultConnectionLimit = Int32.MaxValue;

    var host = new JobHost();
    host.RunAndBlock();
}
```

## <a name="triggers"></a>Triggers

Functies moet openbare methoden zijn en moet één trigger-kenmerk hebben of de [NoAutomaticTrigger](#manual-trigger) kenmerk.

### <a name="automatic-trigger"></a>Automatische trigger

Automatische triggers aanroepen van een functie in reactie op een gebeurtenis. Zie voor een voorbeeld: de trigger wachtrij in de [Get gestart artikel](webjobs-sdk-get-started.md).

### <a name="manual-trigger"></a>Handmatige trigger

Als u wilt een functie handmatig activeren, gebruiken de `NoAutomaticTrigger` kenmerk toe, zoals weergegeven in het volgende voorbeeld:

```cs
static void Main(string[] args)
{
    JobHost host = new JobHost();
    host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
}
```

```cs
[NoAutomaticTrigger]
public static void CreateQueueMessage(
    TextWriter logger,
    string value,
    [Queue("outputqueue")] out string message)
{
    message = value;
    logger.WriteLine("Creating queue message: ", message);
}
```

## <a name="input-and-output-bindings"></a>Invoer en uitvoer bindingen

Invoer bindingen bieden een declaratieve manier om gegevens uit Azure of services van derden beschikbaar te maken voor uw code. Uitvoer bindingen bieden een manier om gegevens te werken. De [Get gestart artikel](webjobs-sdk-get-started.md) toont een voorbeeld van elk.

U kunt een retourwaarde van de methode gebruiken voor een uitvoer-binding door toe te passen van het kenmerk op de geretourneerde waarde van de methode. Zie het voorbeeld in de Azure Functions [Triggers en bindingen](../azure-functions/functions-triggers-bindings.md#using-the-function-return-value) artikel.

## <a name="binding-types"></a>Bindingstypen

De volgende typen van de trigger en binding zijn opgenomen in de `Microsoft.Azure.WebJobs` pakket:

* Blob Storage
* Queue Storage
* Table Storage

Als u andere trigger en bindingstypen, installeer het NuGet-pakket dat ze zich bevinden en roep een `Use<binding>` methode op de `JobHostConfiguration` object. Bijvoorbeeld, als u een timertrigger gebruikt wilt, installeert u `Microsoft.Azure.WebJobs.Extensions` en roep `UseTimers` in de `Main` methode, zoals in dit voorbeeld:

```cs
static void Main()
{
    config = new JobHostConfiguration();
    config.UseTimers();
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

U vindt het pakket te installeren voor een bepaalde bindingstype in de **pakketten** sectie van dit bindingstype [verwijzingsartikel](#binding-reference-information) voor Azure Functions. Een uitzondering is de trigger bestanden en de binding (voor het lokale bestandssysteem), die niet wordt ondersteund door Azure Functions. om de bestanden te gebruiken binding, installeren `Microsoft.Azure.WebJobs.Extensions` en roep `UseFiles`.

### <a name="usecore"></a>UseCore

De `Microsoft.Azure.WebJobs.Extensions` eerder genoemde pakket biedt ook een speciale bindingstype die u registreren met het aanroepen van kunt de `UseCore` methode. Deze binding kunt u definiëren een [ExecutionContext](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Core/ExecutionContext.cs) parameter in de functiehandtekening. Het context-object geeft u toegang tot de aanroep-ID die u gebruiken kunt voor het correleren van alle logboeken die wordt geproduceerd door een bepaalde functie-aanroep. Hier volgt een voorbeeld:

```cs
class Program
{
    static void Main()
    {
        config = new JobHostConfiguration();
        config.UseCore();
        var host = new JobHost(config);
        host.RunAndBlock();
    }
}
public class Functions
{
    public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        ExecutionContext executionContext,
        ILogger logger)
    {
        logger.LogInformation($"{message}\n{executionContext.InvocationId}");
    }
}
```

## <a name="binding-configuration"></a>Bindingconfiguratie

Veel activeren en een binding van de typen, kunt u configureren dat hun gedrag door het instellen van eigenschappen in een configuratieobject dat u in doorgeven aan de `JobHost`.

### <a name="queue-trigger-configuration"></a>Configuratie van de trigger wachtrij

De instellingen die u voor de opslag wachtrij trigger configureren kunt worden beschreven in de Azure Functions [host.json verwijzing](../azure-functions/functions-host-json.md#queues). Het instellen daarvan in een project WebJobs SDK wordt weergegeven in het volgende voorbeeld:

```cs
static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.Queues.BatchSize = 8;
    config.Queues.NewBatchThreshold = 4;
    config.Queues.MaxDequeueCount = 4;
    config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="configuration-for-other-bindings"></a>Configuratie voor andere bindingen

Sommige typen trigger en binding bepalen het type van hun eigen aangepaste configuratie. De trigger bestand kunt u het pad naar de hoofdmap voor het bewaken van opgeven:

```cs
static void Main()
{
    config = new JobHostConfiguration();
    var filesConfig = new FilesConfiguration
    {
        RootPath = @"c:\data\import"
    };
    config.UseFiles(filesConfig);
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

## <a name="binding-expressions"></a>Expressies voor gegevensbinding

In het kenmerk constructorparameters, kunt u expressies dat uit diverse bronnen worden omgezet naar waarden. Bijvoorbeeld, in de volgende code, het pad voor de `BlobTrigger` kenmerk maakt een expressie met de naam `filename`. Wanneer gebruikt voor de binding uitvoer `filename` wordt omgezet naar de naam van de activerende blob.
 
```cs
public static void CreateThumbnail(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    ILogger logger)
{
    logger.Info($"Blob trigger processing: {filename}");
    // ...
}
```

Zie voor meer informatie over expressies voor gegevensbinding [Binding expressies en patronen](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns) in de documentatie van Azure Functions.

### <a name="custom-binding-expressions"></a>Aangepaste bindingsexpressies

Soms wilt u de naam van een wachtrij, een blob-naam of -container opgeven of een tabel naam in de code in plaats van harde-code. Bijvoorbeeld, u wilt mogelijk opgeven de naam van de wachtrij voor de `QueueTrigger` kenmerk in een configuratie-bestand of de omgeving variabele.

U kunt dit doen door doorgeven in een `NameResolver` object toe aan de `JobHostConfiguration` object. U tijdelijke aanduidingen in de trigger of bindparameters kenmerk constructor, opnemen en uw `NameResolver` biedt u de werkelijke waarden moet worden gebruikt in plaats van de tijdelijke aanduidingen. De tijdelijke aanduidingen worden geïdentificeerd door omsluiten met procent (%) tekens, zoals wordt weergegeven in het volgende voorbeeld:
 
```cs
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

Deze code kunt u een wachtrij met de naam logqueuetest in de testomgeving en een benoemde logqueueprod in productie te gebruiken. In plaats van een vastgelegde wachtrijnaam, u de naam van een vermelding in de `appSettings` verzameling. 

Er is een standaard NameResolver die van kracht als u een aangepaste niet opgeeft. De standaardwaarde haalt de waarden van app-instellingen of omgevingsvariabelen.

Uw `NameResolver` klasse opgehaald uit de naam van de wachtrij `appSettings` zoals weergegeven in het volgende voorbeeld:

```cs
public class CustomNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

Geeft uw `NameResolver` -klasse in voor de `JobHost` object, zoals wordt weergegeven in het volgende voorbeeld:

```cs
 static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.NameResolver = new CustomNameResolver();
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Azure Functions implements `INameResolver` waarden ophalen van app-instellingen, zoals wordt weergegeven in het voorbeeld. Wanneer u rechtstreeks de WebJobs SDK gebruikt, kunt u een aangepaste implementatie dat tijdelijke aanduiding voor vervangende waarden ophaalt uit welke bron u liever schrijven. 

## <a name="binding-at-runtime"></a>Binding tijdens runtime

Als u werken in uw functie wilt voordat u een binding-kenmerk, zoals `Queue`, `Blob`, of `Table`, kunt u de `IBinder` interface.

Het volgende voorbeeld wordt een bericht invoerwachtrij en maakt u een nieuw bericht met de inhoud van een wachtrij voor de uitvoer. De naam van de uitvoer-wachtrij is ingesteld door de code in de hoofdtekst van de functie.

```cs
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    IBinder binder)
{
    string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
    QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
    CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
    outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
}
```

Zie voor meer informatie [Binding tijdens runtime](../azure-functions/functions-dotnet-class-library.md#binding-at-runtime) in de documentatie van Azure Functions.

## <a name="binding-reference-information"></a>Binding referentie-informatie

Naslaginformatie over elk bindingstype is opgegeven in de documentatie van Azure Functions. Opslagwachtrij als voorbeeld gebruikt, vindt u de volgende gegevens in elke binding verwijzingsartikel:

* [Pakketten](../azure-functions/functions-bindings-storage-queue.md#packages---functions-1x) -welk pakket installeren om te kunnen bieden ondersteuning voor de binding in een WebJobs SDK-project.
* [Voorbeelden](../azure-functions/functions-bindings-storage-queue.md#trigger---example) -de C# class library-voorbeeld is van toepassing op de WebJobs SDK; alleen weglaten de `FunctionName` kenmerk.
* [Kenmerken](../azure-functions/functions-bindings-storage-queue.md#trigger---attributes) -de te gebruiken voor het bindingstype kenmerken.
* [Configuratie](../azure-functions/functions-bindings-storage-queue.md#trigger---configuration) -uitleg van de kenmerkeigenschappen en constructorparameters.
* [Gebruik](../azure-functions/functions-bindings-storage-queue.md#trigger---usage) : wat u van het type kan worden verbonden met en informatie over hoe u de binding werkt. Bijvoorbeeld: wachtrijbewerkingen polling algoritme, poison.
  
Zie voor een lijst van bindende naslaginformatie **ondersteund bindingen** in de [Triggers en bindingen](../azure-functions/functions-triggers-bindings.md#supported-bindings) artikel voor Azure Functions. In de lijst, worden de HTTP-webhook en gebeurtenis raster bindingen alleen ondersteund door Azure Functions, niet door de WebJobs SDK.

## <a name="disable-attribute"></a>Kenmerk uitschakelen 

De [uitschakelen](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs) kenmerk kunt u bepalen of een functie kunnen worden geactiveerd. 

In het volgende voorbeeld als de app-instelling `Disable_TestJob` heeft de waarde '1' of 'True' (hoofdlettergevoelig), de functie wordt niet uitgevoerd. In dat geval de runtime maakt een logboekbericht *'Functions.TestJob' de functie is uitgeschakeld*.

```cs
[Disable("Disable_TestJob")]
public static void TestJob([QueueTrigger("testqueue2")] string message)
{
    Console.WriteLine("Function with Disable attribute executed!");
}
```

Wanneer u de waarden van app-instelling in de Azure portal wijzigt, het ervoor zorgt dat de webtaak opnieuw worden opgestart, de nieuwe instelling ophalen.

Het kenmerk kan worden gedeclareerd op het parameter, klasseniveau of methode. Naam van de instelling kan ook de expressies voor gegevensbinding bevatten.

## <a name="timeout-attribute"></a>Time-outkenmerk

De [time-out](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TimeoutAttribute.cs) -kenmerk veroorzaakt door een functie als deze niet voltooid binnen een opgegeven tijdsduur worden geannuleerd. In het volgende voorbeeld wordt de functie uitgevoerd voor één dag zonder de time-out. Met de time-out is de functie na 15 seconden geannuleerd.

```cs
[Timeout("00:00:15")]
public static async Task TimeoutJob(
    [QueueTrigger("testqueue2")] string message,
    CancellationToken token,
    TextWriter log)
{
    await log.WriteLineAsync("Job starting");
    await Task.Delay(TimeSpan.FromDays(1), token);
    await log.WriteLineAsync("Job completed");
}
```

U kunt de time-outkenmerk op niveau van de klasse of methode toepassen en kunt u een globale time-out opgeven met behulp van `JobHostConfiguration.FunctionTimeout`. Klasse of methode niveau time-outs overschrijven de algemene time-out.

## <a name="singleton-attribute"></a>Singleton-kenmerk

Gebruik de [Singleton](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/SingletonAttribute.cs) kenmerk om te zorgen dat slechts één exemplaar van een functie wordt uitgevoerd zelfs weer wanneer er meerdere exemplaren van de host-web-app. Dit gebeurt door het implementeren van [gedistribueerd vergrendelen](#viewing-lease-blobs).

In het volgende voorbeeld wordt slechts één exemplaar van de `ProcessImage` functie wordt uitgevoerd op elk moment:

```cs
[Singleton]
public static async Task ProcessImage([BlobTrigger("images")] Stream image)
{
     // Process the image
}
```

### <a name="singletonmodelistener"></a>SingletonMode.Listener

Sommige triggers hebben een ingebouwde ondersteuning voor het beheer van gelijktijdigheid van taken:

* **QueueTrigger** : Stel `JobHostConfiguration.Queues.BatchSize` op 1.
* **ServiceBusTrigger** : Stel `ServiceBusConfiguration.MessageOptions.MaxConcurrentCalls` op 1.
* **FileTrigger** : Stel `FileProcessor.MaxDegreeOfParallelism` op 1.

U kunt deze instellingen gebruiken om ervoor te zorgen dat de functie op één instantie wordt uitgevoerd als een singleton. Slechts één exemplaar van de functie wordt uitgevoerd wanneer de web-app uitgeschaald naar meerdere exemplaren, zodat een listener niveau Singleton vergrendeling van toepassing op de functie (`[Singleton(Mode = SingletonMode.Listener)]`). Listener vergrendelingen worden verkregen bij het starten van de JobHost. Als alle drie uitgebreid-exemplaren op hetzelfde moment wordt gestart, wordt slechts één van de exemplaren verkrijgt de vergrendeling en slechts één listener wordt gestart.

### <a name="scope-values"></a>Waarden voor het bereik

Kunt u een **expressiewaarde/bereik** op de Singleton wordt om ervoor te zorgen dat alle uitvoeringen van de functie voor deze scope worden geserialiseerd. Implementatie van gedetailleerdere vergrendeling op deze manier kunt toestaan voor een bepaalde mate van parallelle uitvoering voor de functie tijdens het serialiseren van andere aanroepen, zoals bepaald door uw vereisten. Bijvoorbeeld, in het volgende voorbeeld de scope-expressie wordt gebonden aan de `Region` waarde van het binnenkomende bericht. Als de wachtrij 3 berichten in de regio's 'Oost', 'Oost' en 'West' respectievelijk de berichten die regio 'Oost bevat' worden opeenvolgend uitgevoerd tijdens het bericht met 'West' wordt uitgevoerd in combinatie met de regio hebben.

```csharp
[Singleton("{Region}")]
public static async Task ProcessWorkItem([QueueTrigger("workitems")] WorkItem workItem)
{
     // Process the work item
}

public class WorkItem
{
     public int ID { get; set; }
     public string Region { get; set; }
     public int Category { get; set; }
     public string Description { get; set; }
}
```

### <a name="singletonscopehost"></a>SingletonScope.Host

Het standaardbereik voor een vergrendeling is `SingletonScope.Function` wat betekent dat het bereik (blob lease pad) is gekoppeld aan de volledig gekwalificeerde functienaam. Als u wilt vergrendelen via de functies, geef `SingletonScope.Host` en gebruik een bereik-ID-naam die is hetzelfde voor alle van de functies die u niet wilt dat tegelijkertijd uit te voeren. In het volgende voorbeeld wordt slechts één exemplaar van `AddItem` of `RemoveItem` tegelijkertijd wordt uitgevoerd:

```charp
[Singleton("ItemsLock", SingletonScope.Host)]
public static void AddItem([QueueTrigger("add-item")] string message)
{
     // Perform the add operation
}

[Singleton("ItemsLock", SingletonScope.Host)]
public static void RemoveItem([QueueTrigger("remove-item")] string message)
{
     // Perform the remove operation
}
```

### <a name="viewing-lease-blobs"></a>Weergave lease blobs

Maakt gebruik van de WebJobs SDK [Azure blob-leases](../storage/common/storage-concurrency.md#pessimistic-concurrency-for-blobs) achter de voor het implementeren van gedistribueerde vergrendelen. De lease blobs die worden gebruikt door Singleton vindt u in de `azure-webjobs-host` container in de `AzureWebJobsStorage` opslagaccount onder pad 'vergrendelingen'. Bijvoorbeeld, de lease blob-pad voor de eerste `ProcessImage` voorbeeld eerder mogelijk `locks/061851c758f04938a4426aa9ab3869c0/WebJobs.Functions.ProcessImage`. Alle paden opnemen de JobHost-ID in dit geval 061851c758f04938a4426aa9ab3869c0.

## <a name="async-functions"></a>Async-functies

Zie voor informatie over hoe u code async-functies, de documentatie van Azure Functions op [Async functies](../azure-functions/functions-dotnet-class-library.md#async).

## <a name="cancellation-tokens"></a>Annulering tokens

Zie de documentatie van Azure Functions op voor informatie over het afhandelen van de annulering tokens [annulering tokens en correct afsluiten](../azure-functions/functions-dotnet-class-library.md#cancellation-tokens).

## <a name="multiple-instances"></a>Meerdere exemplaren

Als uw web-app wordt uitgevoerd op meerdere exemplaren, is een doorlopende webtaak wordt uitgevoerd op elk exemplaar luisteren voor triggers en functies aanroepen. De verschillende trigger-bindingen zijn ontworpen om te delen werk efficiënt samenwerken op exemplaren, kunt zodat uitbreiden naar meer exemplaren u om meer belasting te verwerken.

De wachtrij en de blob-triggers automatisch te voorkomen dat een functie voor het verwerken van een wachtrijbericht of blob-meer dan één keer; functies hoeft niet te worden idempotent.

De timertrigger zorgt automatisch voor dat slechts één exemplaar van de timer wordt uitgevoerd, zodat u niet beschikt over meer dan een functie-exemplaar dat wordt uitgevoerd op een bepaald moment geplande.

Als u wilt zorgen dat slechts één exemplaar van een functie wordt uitgevoerd zelfs als er meerdere exemplaren van de host-web-app, kunt u de [Singleton](#singleton) kenmerk.
    
## <a name="filters"></a>Filters 

Functie-Filters (preview) bieden een manier voor het aanpassen van de API-pijplijn met uw eigen logica. Filters zijn vergelijkbaar met [ASP.NET Core Filters](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters). Ze kunnen worden geïmplementeerd als declaratieve kenmerken die worden toegepast op de functies of klassen. Zie voor meer informatie [functie Filters](https://github.com/Azure/azure-webjobs-sdk/wiki/Function-Filters).

## <a name="logging-and-monitoring"></a>Logboekregistratie en bewaking

Het framework voor logboekregistratie die is ontwikkeld voor ASP.NET, wordt aangeraden en de [aan de slag](webjobs-sdk-get-started.md) artikel laat zien hoe u deze gebruiken. 

### <a name="log-filtering"></a>Logboek filteren

Elk logboek wordt gemaakt door een `ILogger` exemplaar heeft een bijbehorende `Category` en `Level`. [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel#Microsoft_Extensions_Logging_LogLevel) is een inventarisatie en de code integer geeft relatieve belang:

|LogLevel    |Code|
|------------|---|
|Tracering       | 0 |
|Fouten opsporen       | 1 |
|Informatie | 2 |
|Waarschuwing     | 3 |
|Fout       | 4 |
|Kritiek    | 5 |
|Geen        | 6 |

Elke categorie onafhankelijk kan worden gefilterd aan een bepaalde [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel). Bijvoorbeeld, u mogelijk wilt laten zien alle logboeken voor de blob trigger verwerking maar alleen `Error` en hoger voor alle andere.

Om op te geven filterregels te vereenvoudigen de WebJobs SDK kunt u de `LogCategoryFilter` die kunnen worden doorgegeven in veel van de bestaande logboekregistratie-providers, inclusief Application Insights en de Console.

De `LogCategoryFilter` heeft een `Default` eigenschap met de eerste waarde `Information`, wat betekent dat alle berichten met niveaus van `Information`, `Warning`, `Error`, of `Critical` worden geregistreerd, maar alle berichten met niveaus van `Debug` of `Trace` opslag worden gefilterd.

De `CategoryLevels` eigenschap kunt u opgeven van de logboekniveaus voor specifieke categorieën zodat u de uitvoer van de logboekregistratie kunt aanpassen. Als er geen overeenkomst is gevonden binnen de `CategoryLevels` woordenlijst, het filter valt terug op de `Default` waarde bij het bepalen of u wilt filteren van het bericht.

Het volgende voorbeeld wordt een filter dat standaard filtert alle logboeken op de `Warning` niveau. Categorieën van `Function` of `Host.Results` worden gefilterd op de `Error` niveau. De `LogCategoryFilter` vergelijkt de huidige categorie voor alle geregistreerde `CategoryLevels` en kiest u een overeenkomst met de langste. Dit betekent dat de `Debug` niveau geregistreerd voor `Host.Triggers` komt overeen met `Host.Triggers.Queue` of `Host.Triggers.Blob`. Hiermee kunt u bredere categorieën beheren zonder dat elk criterium toevoegen.

```csharp
var filter = new LogCategoryFilter();
filter.DefaultLevel = LogLevel.Warning;
filter.CategoryLevels[LogCategories.Function] = LogLevel.Error;
filter.CategoryLevels[LogCategories.Results] = LogLevel.Error;
filter.CategoryLevels["Host.Triggers"] = LogLevel.Debug;

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(instrumentationKey, filter.Filter)
    .AddConsole(filter.Filter);
```

### <a name="custom-telemetry-for-application-insights"></a>Aangepaste telemetrie voor Application Insights

Intern maakt de `TelemetryClient` gemaakt door de Application Insights-provider voor de WebJobs SDK gebruikt de [ServerTelemetryChannel](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs). Wanneer de Application Insights-eindpunt is niet beschikbaar of bandbreedteregeling inkomende aanvragen en dit kanaal [aanvragen worden opgeslagen in de web-app-bestandssysteem en deze later opnieuw indient](http://apmtips.com/blog/2015/09/03/more-telemetry-channels).

De `TelemetryClient` wordt gemaakt door een klasse die implementeert `ITelemetryClientFactory`. Dit is standaard de [DefaultTelemetryClientFactory](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/DefaultTelemetryClientFactory.cs).

Als u wijzigen, een deel van de Application Insights-pijplijn wilt, kunt u opgeven uw eigen `ITelemetryClientFactory`, en de host gebruikt uw klasse bouwen een `TelemetryClient`. Bijvoorbeeld: deze code overschrijft de `DefaultTelemetryClientFactory` te wijzigen van een eigenschap van de `ServerTelemetryChannel`:

```csharp
private class CustomTelemetryClientFactory : DefaultTelemetryClientFactory
{
    public CustomTelemetryClientFactory(string instrumentationKey, Func<string, LogLevel, bool> filter)
        : base(instrumentationKey, new SamplingPercentageEstimatorSettings(), filter)
    {
    }

    protected override ITelemetryChannel CreateTelemetryChannel()
    {
        ServerTelemetryChannel channel = new ServerTelemetryChannel();

        // change the default from 30 seconds to 15 seconds
        channel.MaxTelemetryBufferDelay = TimeSpan.FromSeconds(15);

        return channel;
    }
}
```

Het object SamplingPercentageEstimatorSettings configureert [adaptieve steekproeven](https://docs.microsoft.com/azure/application-insights/app-insights-sampling#adaptive-sampling-at-your-web-server). Dit betekent dat in bepaalde gevallen hoog volume App Insights een geselecteerde subset van telemetriegegevens naar de server verzendt.

Als u de telemetrie-factory hebt gemaakt, kunt u deze in doorgeven aan de logboekregistratie van Application Insights-provider:

```csharp
var clientFactory = new CustomTelemetryClientFactory(instrumentationKey, filter.Filter);

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(clientFactory);
```

## <a id="nextsteps"></a> Volgende stappen

Deze handleiding is opgegeven codefragmenten die laten hoe u algemene scenario's zien voor het werken met de WebJobs SDK verwerken. Raadpleeg voor volledige voorbeelden [azure webjobs-sdk samples](https://github.com/Azure/azure-webjobs-sdk-samples).