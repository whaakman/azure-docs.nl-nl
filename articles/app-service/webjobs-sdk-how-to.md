---
title: Het gebruik van de Azure WebJobs SDK
description: Meer informatie over het schrijven van code voor de WebJobs SDK. Gebeurtenisgestuurde achtergrond verwerking taken maken waarmee toegang gegevens in Azure-services en services van derden tot.
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/27/2018
ms.author: glenga
ms.openlocfilehash: d2a0e14ec4708539c7fd8640c8dd3dacef10f508
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52961889"
---
# <a name="how-to-use-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Over het gebruik van de Azure WebJobs SDK voor verwerking op de achtergrond gebeurtenisgestuurde

Dit artikel bevat richtlijnen over het schrijven van code voor [de Azure WebJobs SDK](webjobs-sdk-get-started.md). De documentatie van toepassing op versie 2.x en 3.x behalve indien anders vermeld. De belangrijkste wijziging geïntroduceerd door 3.x is het gebruik van .NET Core in plaats van .NET Framework.

>[!NOTE]
> [Azure Functions](../azure-functions/functions-overview.md) is gebouwd op de WebJobs SDK en in dit artikel bevat koppelingen naar documentatie voor Azure Functions voor bepaalde onderwerpen. Houd rekening met de volgende verschillen tussen Functions en de WebJobs SDK:
> * Azure Functions-versie 1.x komt overeen met de WebJobs SDK-versie 2.x en Azure Functions 2.x komt overeen met de WebJobs SDK 3.x. Broncodeopslagplaatsen volgt u de WebJobs SDK nummering en veel v2.x branches weergegeven, met de master-vertakking momenteel met 3.x code hebben.
> * Voorbeeldcode voor Azure Functions C#-klassebibliotheken is, net als de WebJobs SDK-code, behalve hoeft u niet een `FunctionName` kenmerk in een WebJobs SDK-project.
> * Sommige bindingstypen worden alleen ondersteund in de functies, zoals HTTP, webhook en Event Grid (die is gebaseerd op HTTP). 
> 
> Zie voor meer informatie, [Vergelijk de WebJobs SDK en Azure Functions](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs). 

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt gelezen [aan de slag met de WebJobs SDK](webjobs-sdk-get-started.md).

## <a name="jobhost"></a>JobHost

De `JobHost` -object is de container runtime van functions: er wordt geluisterd naar triggers en aanroepen van functies. U maakt de `JobHost` in uw code en het schrijven van code voor het eigen gedrag aanpassen.

Dit is een belangrijk verschil tussen de WebJobs SDK rechtstreeks gebruiken en indirect met behulp van Azure Functions. In Azure Functions, de besturingselementen van de service de `JobHost`, en kan niet worden aangepast door code te schrijven. Azure Functions kunt u aanpassen van het gedrag van de host via instellingen in de *host.json* bestand. Deze instellingen zijn tekenreeksen, niet-code, die beperkt de soorten aanpassingen die u kunt doen.

### <a name="jobhost-connection-strings"></a>Verbindingsreeksen JobHost

De WebJobs SDK zoekt naar opslag- en Service Bus-verbindingsreeksen in *local.settings.json* bij het uitvoeren lokaal of in de omgeving van de webtaak wanneer u in Azure uitvoert. Als u wilt uw eigen namen voor deze verbindingsreeksen gebruiken of deze ergens anders opslaan, kunt u deze instellen in code, zoals hier wordt weergegeven:

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

De `JobHostConfiguration` klasse heeft een `UseDevelopmentSettings` methode die u aanroepen kunt lokale ontwikkeling om efficiënter te maken. Hier volgen enkele van de instellingen voor deze methode wordt gewijzigd:

| Eigenschap | Ontwikkeling instellen |
| ------------- | ------------- |
| `Tracing.ConsoleLevel` | `TraceLevel.Verbose` voor een maximale logboekuitvoer. |
| `Queues.MaxPollingInterval`  | Een lage waarde om ervoor te zorgen wachtrij methoden onmiddellijk worden geactiveerd.  |
| `Singleton.ListenerLockPeriod` | 15 seconden bij snelle iteratieve ontwikkeling. |

Het volgende voorbeeld ziet hoe u instellingen voor het ontwikkelen. Om `config.IsDevelopment` retourneren `true` bij lokale uitvoering, stelt u een lokale omgevingsvariabele `AzureWebJobsEnv` met waarde `Development`.

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

### <a name="jobhost-servicepointmanager-settings"></a>Instellingen voor JobHost ServicePointManager

.NET Framework bevat een API met de naam [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) die bepaalt het aantal gelijktijdige verbindingen met een host. Het is raadzaam dat u deze waarde van de standaardwaarde van 2 verhogen voordat u begint met de WebJobs-host.

Alle uitgaande HTTP-aanvragen die u in een functie met behulp van aanbrengt `HttpClient` langs de `ServicePointManager`. Zodra u bereikt de `DefaultConnectionLimit`, wordt de `ServicePointManager` begint queueing aanvragen voordat ze worden verzonden. Stel dat uw `DefaultConnectionLimit` is ingesteld op 2 en de code maakt 1000 HTTP-aanvragen. In eerste instantie alleen 2 aanvragen daadwerkelijk mogen via het besturingssysteem. De andere 998 in de wachtrij geplaatst totdat er ruimte voor hen. Dit betekent dat uw `HttpClient` time-out, mogelijk omdat het *denkt* deze de aanvraag heeft gemaakt, maar de aanvraag is nooit verzonden door het besturingssysteem naar de doelserver. Zo ziet u mogelijk gedrag op dat lijkt niet te zijn: uw lokale `HttpClient` duurt 10 seconden om een aanvraag te voltooien, maar uw service retourneert elke aanvraag in 200 ms. 

De standaardwaarde voor ASP.NET-toepassingen is `Int32.MaxValue`, en die waarschijnlijk werken goed voor WebJobs die worden uitgevoerd in een standaard of hoger App Service-plan. WebJobs moet meestal de instelling voor Always On en die alleen door App Service-plannen Basic en hoger wordt ondersteund. 

Als de webtaak wordt uitgevoerd in een gratis of gedeeld App Service-Plan, uw toepassing wordt beperkt door de App Service-sandbox momenteel heeft een [limiet van 300 verbindingen](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#per-sandbox-per-appper-site-numerical-limits). Met een niet-afhankelijke verbindingslimiet in `ServicePointManager`, is het meer waarschijnlijk dat de sandbox verbindingen drempelwaarde wordt bereikt en de site wordt afgesloten. In dat geval instellen `DefaultConnectionLimit` in een kleine, zoals 50 of 100, kunt dit te voorkomen en dat er nog steeds voldoende doorvoer.

De instelling moet worden geconfigureerd voordat een HTTP-aanvragen worden gedaan. Om deze reden de host WebJobs om aan te passen van de instelling mag niet probeert automatisch; mogelijk zijn er HTTP-aanvragen die plaatsvinden voordat de host wordt gestart en dit tot onverwacht gedrag leiden kan. De aanbevolen aanpak is om in te stellen de waarde onmiddellijk in uw `Main` methode voor het initialiseren van de `JobHost`, zoals wordt weergegeven in het volgende voorbeeld

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

Functies moet openbare methoden zijn en moet één triggerkenmerk hebben of de [NoAutomaticTrigger](#manual-trigger) kenmerk.

### <a name="automatic-trigger"></a>Automatische trigger

Automatische triggers wordt een functie aanroepen in reactie op een gebeurtenis. Zie voor een voorbeeld: de wachtrijtrigger in de [Get de slag-artikel](webjobs-sdk-get-started.md).

### <a name="manual-trigger"></a>Handmatige trigger

Een functie als handmatig wilt activeren, gebruikt u de `NoAutomaticTrigger` kenmerk, zoals wordt weergegeven in het volgende voorbeeld:

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

## <a name="input-and-output-bindings"></a>Invoer- en uitvoerbindingen

Invoerbindingen bieden een declaratieve manier om gegevens vanuit Azure of services van derden beschikbaar te maken voor uw code. Uitvoerbindingen bieden een manier om gegevens te werken. De [Get de slag-artikel](webjobs-sdk-get-started.md) toont een voorbeeld van elk.

U kunt de geretourneerde waarde van een methode voor een Uitvoerbinding gebruiken door het kenmerk toe te passen op de geretourneerde waarde van de methode. Zie het voorbeeld in de Azure-Functions [Triggers en bindingen](../azure-functions/functions-triggers-bindings.md#using-the-function-return-value) artikel.

## <a name="binding-types"></a>Bindingstypen

De volgende typen van de trigger en binding zijn opgenomen in de `Microsoft.Azure.WebJobs` pakket:

* Blob Storage
* Queue Storage
* Table Storage

Voor het gebruik van andere trigger en bindingstypen, installeer het NuGet-pakket dat ze bevat en roept een `Use<binding>` methode voor het `JobHostConfiguration` object. Bijvoorbeeld, als u een timertrigger gebruikt wilt, installeert `Microsoft.Azure.WebJobs.Extensions` en roep `UseTimers` in de `Main` methode, zoals in dit voorbeeld:

```cs
static void Main()
{
    config = new JobHostConfiguration();
    config.UseTimers();
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

U vindt het pakket te installeren voor een bepaalde bindingstype in de **pakketten** sectie van dit bindingstype [naslagartikel](#binding-reference-information) voor Azure Functions. Een uitzondering is de trigger bestanden en binding (voor het lokale bestandssysteem), wat niet wordt ondersteund door Azure Functions. om de bestanden te gebruiken binding installeren `Microsoft.Azure.WebJobs.Extensions` en roep `UseFiles`.

### <a name="usecore"></a>UseCore

De `Microsoft.Azure.WebJobs.Extensions` eerder genoemde pakket biedt ook een speciale bindingstype die u registreren met het aanroepen van kunt de `UseCore` methode. Deze binding kunt u definiëren een [ExecutionContext](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Core/ExecutionContext.cs) parameter in uw functiehandtekening. Het context-object geeft u toegang tot de aanroep-ID, die u gebruiken kunt voor het correleren van alle logboeken die worden geproduceerd door een bepaalde functie-aanroep. Hier volgt een voorbeeld:

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

## <a name="binding-configuration"></a>Bindingsconfiguratie van

Veel activeren en typen binding kunt u hun gedrag configureren door het instellen van eigenschappen in een configuratieobject dat u in doorgeven aan de `JobHost`.

### <a name="queue-trigger-configuration"></a>Configuratie van de wachtrij-trigger

De instellingen die u voor de trigger van de Storage-wachtrij configureren kunt worden beschreven in de Azure-Functions [naslaginformatie over host.json](../azure-functions/functions-host-json.md#queues). In het volgende voorbeeld ziet u hoe u deze instelt in een WebJobs SDK-project:

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

Sommige typen trigger en binding definiëren hun eigen aangepaste configuratie-type. De bestand-trigger kunt u het pad naar de hoofdmap voor het bewaken van opgeven:

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

In het kenmerk constructor parameters, kunt u expressies die wordt omgezet in waarden van verschillende bronnen. Bijvoorbeeld, in de volgende code, het pad voor de `BlobTrigger` kenmerk maakt u een expressie met de naam `filename`. Wanneer u gebruikt voor de Uitvoerbinding `filename` wordt omgezet naar de naam van de activerende blob.
 
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

Zie voor meer informatie over de expressies voor gegevensbinding [bindende expressies en patronen](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns) in de documentatie van Azure Functions.

### <a name="custom-binding-expressions"></a>Aangepaste binding expressies

Soms wilt u de naam van een wachtrij, een blob-naam of container opgeven of een tabel in de code in plaats van programmeren Geef deze de naam. Bijvoorbeeld, u misschien wilt opgeven van de naam van de wachtrij voor de `QueueTrigger` kenmerk in een configuratie-bestand of de omgeving variabele.

U kunt dit doen door te geven in een `NameResolver` object toe aan de `JobHostConfiguration` object. U tijdelijke aanduidingen opnemen in de trigger of bindparameters kenmerk constructor, en uw `NameResolver` code geeft de werkelijke waarden moet worden gebruikt in plaats van de tijdelijke aanduidingen. De tijdelijke aanduidingen worden geïdentificeerd door % (%) tekens, zoals wordt weergegeven in het volgende voorbeeld:
 
```cs
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

Deze code kunt u een wachtrij met de naam logqueuetest in de testomgeving en één benoemde logqueueprod in productie gebruiken. In plaats van een vastgelegde wachtrijnaam, geeft u de naam van een vermelding in de `appSettings` verzameling. 

Er is een standaardwaarde NameResolver die van kracht als u een aangepaste classificatie niet opgeeft. Standaard haalt de waarden van app-instellingen of omgevingsvariabelen.

Uw `NameResolver` klasse opgehaald uit de naam van de wachtrij `appSettings` zoals wordt weergegeven in het volgende voorbeeld:

```cs
public class CustomNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

Geeft uw `NameResolver` in klasse aan de `JobHost` object, zoals wordt weergegeven in het volgende voorbeeld:

```cs
 static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.NameResolver = new CustomNameResolver();
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Azure Functions worden `INameResolver` waarden ophalen uit de app-instellingen, zoals wordt weergegeven in het voorbeeld. Wanneer u rechtstreeks de WebJobs SDK gebruikt, kunt u een aangepaste implementatie die tijdelijke aanduiding vervangende waarden opgehaald uit welke bron die u wilt schrijven. 

## <a name="binding-at-runtime"></a>Binding tijdens runtime

Als u ook werken in uw functie wilt voordat u een binding-kenmerk zoals `Queue`, `Blob`, of `Table`, kunt u de `IBinder` interface.

Het volgende voorbeeld wordt een bericht invoerwachtrij en maakt u een nieuw bericht met de inhoud van een uitvoerwachtrij. De naam van de uitvoer-wachtrij wordt ingesteld door de code in de hoofdtekst van de functie.

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

Zie voor meer informatie, [Binding tijdens runtime](../azure-functions/functions-dotnet-class-library.md#binding-at-runtime) in de documentatie van Azure Functions.

## <a name="binding-reference-information"></a>Binding referentie-informatie

Referentie-informatie over elk bindingstype is opgegeven in de documentatie voor Azure Functions. Storage-wachtrij als voorbeeld gebruikt, vindt u de volgende gegevens in elk artikel binding-verwijzing:

* [Pakketten](../azure-functions/functions-bindings-storage-queue.md#packages---functions-1x) -welk pakket te installeren om te kunnen bieden ondersteuning voor de binding in een WebJobs SDK-project.
* [Voorbeelden](../azure-functions/functions-bindings-storage-queue.md#trigger---example) -de klasse bibliotheek voorbeeld met C# is van toepassing op de WebJobs SDK; alleen laat de `FunctionName` kenmerk.
* [Kenmerken](../azure-functions/functions-bindings-storage-queue.md#trigger---attributes) -de kenmerken moet worden gebruikt voor het bindingstype.
* [Configuratie](../azure-functions/functions-bindings-storage-queue.md#trigger---configuration) -uitleg van de eigenschappen van kenmerk en de parameters van constructor.
* [Gebruik](../azure-functions/functions-bindings-storage-queue.md#trigger---usage) : wat u van het type verbinding kan maken met en informatie over hoe u de binding werkt. Bijvoorbeeld: polling-algoritme, poison verwerking van de wachtrij.
  
Zie voor een lijst van het binden van naslagartikelen **ondersteund bindingen** in de [Triggers en bindingen](../azure-functions/functions-triggers-bindings.md#supported-bindings) artikel voor Azure Functions. In de lijst, worden de HTTP-webhook en Event Grid-bindingen alleen ondersteund door Azure Functions, niet door de WebJobs SDK.

## <a name="disable-attribute"></a>Kenmerk uitschakelen 

De [uitschakelen](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs) kenmerk kunt u bepalen of een functie kunnen worden geactiveerd. 

In het volgende voorbeeld als de app-instelling `Disable_TestJob` heeft de waarde "1" of "True" (niet hoofdlettergevoelig), de functie wordt niet uitgevoerd. In dat geval de runtime maakt u een logboekbericht *functie 'Functions.TestJob' is uitgeschakeld*.

```cs
[Disable("Disable_TestJob")]
public static void TestJob([QueueTrigger("testqueue2")] string message)
{
    Console.WriteLine("Function with Disable attribute executed!");
}
```

Wanneer u een app in Azure portal-instellingswaarden wijzigt, het ervoor zorgt dat de webtaak opnieuw worden gestart, de nieuwe instelling ophalen.

Het kenmerk kan worden gedeclareerd op de parameter, klasseniveau of methode. Naam van de instelling kan ook binding expressies bevatten.

## <a name="timeout-attribute"></a>Time-outkenmerk

De [time-out](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TimeoutAttribute.cs) kenmerk zorgt ervoor dat een functie om te worden geannuleerd als deze wordt niet voltooid binnen een opgegeven tijdsduur. In het volgende voorbeeld wordt de functie uitgevoerd gedurende één dag zonder de time-out. De functie is met de time-out geannuleerd na 15 seconden.

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

U kunt de time-outkenmerk op het niveau van klasse of methode toepassen, en kunt u een globale time-out opgeven met behulp van `JobHostConfiguration.FunctionTimeout`. Klasse of methode niveau time-outs overschrijven de algemene time-out.

## <a name="singleton-attribute"></a>Singleton-kenmerk

Gebruik de [Singleton](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/SingletonAttribute.cs) kenmerk om ervoor te zorgen dat slechts één exemplaar van een functie wordt uitgevoerd zelfs maken wanneer er meerdere exemplaren van de host-web-app. Dit gebeurt door het implementeren van [gedistribueerd vergrendelen](#viewing-lease-blobs).

In het volgende voorbeeld wordt slechts één exemplaar van de `ProcessImage` functie wordt uitgevoerd op een bepaald moment:

```cs
[Singleton]
public static async Task ProcessImage([BlobTrigger("images")] Stream image)
{
     // Process the image
}
```

### <a name="singletonmodelistener"></a>SingletonMode.Listener

Sommige triggers hebben een ingebouwde ondersteuning voor het gelijktijdigheidsbeheer van:

* **QueueTrigger** : Stel `JobHostConfiguration.Queues.BatchSize` op 1.
* **ServiceBusTrigger** : Stel `ServiceBusConfiguration.MessageOptions.MaxConcurrentCalls` op 1.
* **FileTrigger** : Stel `FileProcessor.MaxDegreeOfParallelism` op 1.

U kunt deze instellingen gebruiken om ervoor te zorgen dat de functie wordt uitgevoerd als een singleton op één exemplaar. Als u wilt zorgen dat slechts één exemplaar van de functie wordt uitgevoerd wanneer de web-app wordt geschaald naar meerdere exemplaren, een listener niveau Singleton-vergrendeling van toepassing op de functie (`[Singleton(Mode = SingletonMode.Listener)]`). Listener vergrendelingen zijn verkregen bij het starten van de JobHost. Als alle drie uitgeschaalde-exemplaren op hetzelfde moment wordt gestart, wordt slechts één van de exemplaren verkrijgt de vergrendeling en slechts één listener wordt gestart.

### <a name="scope-values"></a>Waarden voor het bereik

Kunt u een **het bereik van de waarde van deexpressie/** op de Singleton die zorgt ervoor dat alle uitvoeringen van de functie op dat bereik worden geserialiseerd. Implementatie van gedetailleerdere vergrendeling op deze manier kunt toestaan voor een bepaalde mate van parallelle uitvoering voor de functie tijdens de serialisatie van andere aanroepen, zoals bepaald door uw vereisten. Bijvoorbeeld, in het volgende voorbeeld de scope-expressie wordt gebonden aan de `Region` waarde van het binnenkomende bericht. Als de wachtrij 3 berichten in de regio's "Oost-", 'Oost' en 'West' respectievelijk de berichten die regio '-Oost bevat' worden opeenvolgend worden uitgevoerd terwijl het bericht met 'West' wordt uitgevoerd in combinatie met de regio.

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

Het bereik van standaard voor een vergrendeling is `SingletonScope.Function` wat betekent dat het vergrendelingsbereik (blob-lease pad) is gekoppeld aan de naam van de volledig gekwalificeerde functie. Als u wilt vergrendelen voor functies, geef `SingletonScope.Host` en gebruik een bereik-ID-naam die is hetzelfde voor alle van de functies die u niet wilt dat tegelijkertijd uit te voeren. In het volgende voorbeeld wordt slechts één exemplaar van `AddItem` of `RemoveItem` wordt uitgevoerd op een tijdstip:

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

### <a name="viewing-lease-blobs"></a>Lease-blobs weergeven

De WebJobs-SDK gebruikt [Azure blob-lease](../storage/common/storage-concurrency.md#pessimistic-concurrency-for-blobs) op de achtergrond voor het implementeren van gedistribueerde vergrendelen. De lease-blobs die worden gebruikt door Singleton kunnen u vinden in de `azure-webjobs-host` -container in de `AzureWebJobsStorage` storage-account onder pad 'vergrendelingen'. Bijvoorbeeld, de lease blob-pad voor de eerste `ProcessImage` eerdere voorbeeld mogelijk `locks/061851c758f04938a4426aa9ab3869c0/WebJobs.Functions.ProcessImage`. Alle paden bevatten de JobHost-ID, in dit geval 061851c758f04938a4426aa9ab3869c0.

## <a name="async-functions"></a>Async-functies

Zie voor informatie over hoe u code async-functies, de documentatie voor Azure Functions op [asynchrone functies](../azure-functions/functions-dotnet-class-library.md#async).

## <a name="cancellation-tokens"></a>Annuleringstokens

Zie de documentatie voor Azure Functions op voor informatie over het afhandelen van annuleringstokens [annuleringstokens en correct afsluiten](../azure-functions/functions-dotnet-class-library.md#cancellation-tokens).

## <a name="multiple-instances"></a>Meerdere exemplaren

Als uw web-app wordt uitgevoerd op meerdere exemplaren, is een doorlopende webtaak wordt uitgevoerd op elk exemplaar luisteren naar triggers en functies aan te roepen. De verschillende trigger-bindingen zijn ontworpen voor het efficiënt werken samen voor instanties, delen kunt zodat uitschalen naar meer instanties u om meer belasting te verwerken.

De wachtrij en de blob-triggers automatisch te voorkomen dat een functie van het verwerken van een wachtrijbericht of blob-meer dan één keer; functies hoeven niet te zijn idempotent.

De timertrigger zorgt automatisch voor dat slechts één exemplaar van de timer wordt uitgevoerd, zodat u niet meer dan één functie-instantie die wordt uitgevoerd op een bepaalde geplande tijdstip.

Als u wilt ervoor zorgen dat slechts één exemplaar van een functie wordt uitgevoerd, zelfs als er meerdere exemplaren van de host-web-app, kunt u de [Singleton](#singleton) kenmerk.
    
## <a name="filters"></a>Filters 

Functie-Filters (preview) bieden een manier om aan te passen van de pijplijn voor de uitvoering van WebJobs met uw eigen logica. Filters zijn vergelijkbaar met [ASP.NET Core Filters](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters). Ze kunnen worden geïmplementeerd als declaratieve kenmerken die worden toegepast op uw functies of klassen. Zie voor meer informatie, [functie Filters](https://github.com/Azure/azure-webjobs-sdk/wiki/Function-Filters).

## <a name="logging-and-monitoring"></a>Logboekregistratie en bewaking

We raden aan het framework voor logboekregistratie die is ontwikkeld voor ASP.NET, en de [aan de slag](webjobs-sdk-get-started.md) artikel ziet u hoe u deze. 

### <a name="log-filtering"></a>Logboek filteren

Elk logboek wordt gemaakt door een `ILogger` exemplaar heeft een bijbehorende `Category` en `Level`. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) een opsomming, en de code geheel getal geeft aan dat het relatieve belang:

|LogLevel    |Code|
|------------|---|
|Tracering       | 0 |
|Fouten opsporen       | 1 |
|Informatie | 2 |
|Waarschuwing     | 3 |
|Fout       | 4 |
|Kritiek    | 5 |
|Geen        | 6 |

Elke categorie kan onafhankelijk van elkaar worden gefilterd tot een bepaald [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel). Bijvoorbeeld, kunt u alle logboeken voor de blob trigger verwerking, maar alleen `Error` en hoger voor alle andere.

Als u wilt maken het gemakkelijker om op te geven van filterregels, de WebJobs SDK biedt de `LogCategoryFilter` die kunnen worden doorgegeven in veel van de bestaande logboekregistratie-providers, met inbegrip van Application Insights en -Console.

De `LogCategoryFilter` heeft een `Default` eigenschap met een aanvankelijke waarde van `Information`, wat betekent dat alle berichten met niveaus van `Information`, `Warning`, `Error`, of `Critical` bent aangemeld, maar geen berichten met niveaus van `Debug` of `Trace` onmiddellijk worden gefilterd.

De `CategoryLevels` eigenschap kunt u de logboekniveaus voor specifieke categorieën opgeven zodat u de uitvoer van de logboekregistratie kunt aanpassen. Als er geen overeenkomst wordt gevonden in de `CategoryLevels` woordenlijst, het filter schakelt terug naar de `Default` waarde als u besluit om te filteren van het bericht.

Het volgende voorbeeld wordt een filter dat standaard alle logboeken op filtert de `Warning` niveau. Categorieën van `Function` of `Host.Results` worden gefilterd op de `Error` niveau. De `LogCategoryFilter` vergelijkt de huidige categorie voor alle geregistreerde `CategoryLevels` en kiest u de langste overeenkomende reeks. Dit betekent dat de `Debug` niveau geregistreerd voor `Host.Triggers` komt overeen met `Host.Triggers.Queue` of `Host.Triggers.Blob`. Hiermee kunt u voor het beheren van bredere categorieën zonder om toe te voegen van elkaar.

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

Intern maakt de `TelemetryClient` gemaakt door de Application Insights-provider voor de WebJobs-SDK gebruikt de [ServerTelemetryChannel](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs). Wanneer de Application Insights-eindpunt is niet beschikbaar of te beperken binnenkomende aanvragen, dit kanaal [aanvragen opgeslagen in het bestandssysteem van de web-app en deze later opnieuw indient](https://apmtips.com/blog/2015/09/03/more-telemetry-channels).

De `TelemetryClient` wordt gemaakt door een klasse die `ITelemetryClientFactory`. Dit is standaard de [DefaultTelemetryClientFactory](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/DefaultTelemetryClientFactory.cs).

Als u wijzigen van een deel van de Application Insights-pijplijn wilt, kunt u opgeven, uw eigen `ITelemetryClientFactory`, en de host de klasse wordt gebruikt om samen te stellen een `TelemetryClient`. Bijvoorbeeld: deze code overschrijft de `DefaultTelemetryClientFactory` te wijzigen van een eigenschap van de `ServerTelemetryChannel`:

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

Het object SamplingPercentageEstimatorSettings configureert [adaptieve steekproeven](https://docs.microsoft.com/azure/application-insights/app-insights-sampling#adaptive-sampling-at-your-web-server). Dit betekent dat in bepaalde scenario's met hoge volumes App Insights een geselecteerde subset van telemetriegegevens naar de server verzendt.

Als u de telemetrie-factory hebt gemaakt, kunt u deze in doorgeven aan de logboekregistratie voor Application Insights-provider:

```csharp
var clientFactory = new CustomTelemetryClientFactory(instrumentationKey, filter.Filter);

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(clientFactory);
```

## <a id="nextsteps"></a> Volgende stappen

Deze handleiding is opgegeven codefragmenten die laten hoe u algemene scenario's zien voor het werken met de WebJobs SDK. Zie voor volledige voorbeelden [azure-webjobs-sdk-samples](https://github.com/Azure/azure-webjobs-sdk-samples).