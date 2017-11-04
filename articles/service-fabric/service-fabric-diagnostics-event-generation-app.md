---
title: Azure Service Fabric-toepassingsniveau bewaking | Microsoft Docs
description: Meer informatie over de toepassing en service level gebeurtenissen en logboeken die worden gebruikt om te bewaken en onderzoeken van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 9b07c7cd256cea31c5654f0c3325e9fb675b39f8
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="application-and-service-level-event-and-log-generation"></a>Toepassing en service level genereren van gebeurtenis- en logboekbestanden

## <a name="instrumenting-the-code-with-custom-events"></a>De code met aangepaste gebeurtenissen instrumenteren

De code instrumenteren vormt de basis voor de meeste andere aspecten van de bewaking van uw services. Instrumentatie is de enige manier kunt u weet dat er iets mis is, en op te sporen wat moet worden opgelost. Hoewel het is technisch mogelijk verbinding maken met een foutopsporingsprogramma een productieservice, maar het is niet een gangbare optie. Dus is het belangrijk gedetailleerde gegevens.

Sommige producten softwareontwikkelaars automatisch uw code. Hoewel deze oplossingen goed werken kunnen, is bijna altijd handmatige instrumentation vereist. U moet voldoende informatie voor foutopsporing van de toepassing volledig hebben in het einde. Dit document beschrijft de verschillende methoden voor het instrumenteren van uw code en het kiezen van een methode op een andere.

## <a name="eventsource"></a>EventSource
Wanneer u een Service Fabric-oplossing van een sjabloon in Visual Studio maakt een **EventSource**-afgeleide klasse (**ServiceEventSource** of **ActorEventSource**) wordt gegenereerd. Een sjabloon is gemaakt, waarin u de gebeurtenissen voor uw toepassing of service kunt toevoegen. De **EventSource** naam **moet** uniek zijn en moet worden gewijzigd van de sjabloon standaardtekenreeks mijnbedrijf -&lt;oplossing&gt;-&lt;project&gt;. Meerdere **EventSource** definities die dezelfde naam gebruiken voor een probleem zorgt tijdens runtime. Elke gedefinieerde gebeurtenis moet een unieke id hebben. Als een id is niet uniek is, treedt er een runtime-fout op. Sommige organisaties preassign bereiken met waarden voor de id's voorkomen van conflicten tussen afzonderlijke ontwikkelteams. Zie voor meer informatie [van Vance blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) of de [MSDN-documentatie](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

### <a name="using-structured-eventsource-events"></a>Het gebruik van gestructureerde EventSource gebeurtenissen

Elk van de gebeurtenissen in de codevoorbeelden in deze sectie worden gedefinieerd voor een specifieke aanvraag, bijvoorbeeld wanneer een servicetype is geregistreerd. Wanneer u berichten door gebruiksvoorbeeld definiëren, kunnen gegevens worden verpakt met de tekst van de fout en u meer kunt eenvoudig zoeken en filteren op basis van de namen of waarden van de opgegeven eigenschappen. Zorgt ervoor dat de uitvoer instrumentation structureren vereist gemakkelijker te gebruiken, maar meer gedachte en tijd voor het definiëren van een nieuwe gebeurtenis voor elke gebruiksvoorbeeld. Sommige Gebeurtenisdefinities kunnen worden gedeeld door de gehele toepassing. Bijvoorbeeld, een methode starten of stoppen zou gebeurtenis hele veel services in een toepassing worden gebruikt. Een service, domeinspecifieke, zoals een systeem volgorde wellicht een **CreateOrder** gebeurtenis een eigen unieke gebeurtenis heeft. Deze aanpak mogelijk veel gebeurtenissen genereren en mogelijk coördinatie vereisen van id's op projectteams. 

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The instance constructor is private to enforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        // The ServiceTypeRegistered event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceTypeRegisteredEventId = 3;
        [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
        public void ServiceTypeRegistered(int hostProcessId, string serviceType)
        {
            WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
        }

        // The ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceHostInitializationFailedEventId = 4;
        [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
        public void ServiceHostInitializationFailed(string exception)
        {
            WriteEvent(ServiceHostInitializationFailedEventId, exception);
        }
```

### <a name="using-eventsource-generically"></a>EventSource gebruik algemeen

Omdat het definiëren van specifieke gebeurtenissen kan lastig zijn, definiëren veel mensen enkele gebeurtenissen met een gemeenschappelijke set parameters die in het algemeen hun informatie als een tekenreeks voeren. Veel van de gestructureerde aspect gaat verloren en is het moeilijker om te zoeken en filteren van de resultaten. In deze methode voert worden enkele gebeurtenissen die meestal met de registratieniveaus overeen gedefinieerd. Het volgende fragment definieert een bericht voor foutopsporing en de fout:

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The Instance constructor is private, to enforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        private const int DebugEventId = 10;
        [Event(DebugEventId, Level = EventLevel.Verbose, Message = "{0}")]
        public void Debug(string msg)
        {
            WriteEvent(DebugEventId, msg);
        }

        private const int ErrorEventId = 11;
        [Event(ErrorEventId, Level = EventLevel.Error, Message = "Error: {0} - {1}")]
        public void Error(string error, string msg)
        {
            WriteEvent(ErrorEventId, error, msg);
        }
```

Een hybride van gestructureerde en algemene instrumentation kunt ook werken goed. Gestructureerde instrumentation wordt gebruikt voor het melden van fouten en metrische gegevens. Algemene gebeurtenissen kunnen worden gebruikt voor de gedetailleerde logboekregistratie die wordt gebruikt door engineers voor probleemoplossing.

## <a name="aspnet-core-logging"></a>ASP.NET Core logboekregistratie

Het is belangrijk dat u zorgvuldig plannen hoe u uw code wordt instrumenteren. Het plan rechts instrumentation kunt u mogelijk uw codebasis destabilizing en hoeven de code reinstrument voorkomen. Om risico te beperken, kunt u een instrumentatiebiblotheek zoals [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), die deel uitmaakt van Microsoft ASP.NET Core. ASP.NET Core is een [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) interface die u met de leverancier van uw keuze, gebruiken kunt terwijl het effect op de bestaande code worden geminimaliseerd. U kunt de code in ASP.NET Core voor Windows en Linux, en in het volledige .NET Framework, zodat uw code instrumentation gestandaardiseerd. Dit is meer verkend hieronder:

### <a name="using-microsoftextensionslogging-in-service-fabric"></a>Met behulp van Microsoft.Extensions.Logging in Service Fabric

1. Het Microsoft.Extensions.Logging NuGet-pakket aan het project dat u wilt toevoegen is gekoppeld. Ook alle pakketten provider toe te voegen (Zie het volgende voorbeeld voor een pakket van derden). Zie voor meer informatie [logboekregistratie in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Voeg een **met** richtlijn voor Microsoft.Extensions.Logging in uw servicebestand.
3. Definieer een persoonlijke variabele binnen uw serviceklasse.

  ```csharp
  private ILogger _logger = null;

  ```
4. Voeg deze code in de constructor van uw serviceklasse:

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. Start uw code in uw methoden instrumenteren. Hier volgen enkele voorbeelden:

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and the duration of the request.
  // Later in the article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a>Andere providers voor logboekregistratie

Sommige leveranciers gebruik de methode die wordt beschreven in de vorige sectie, inclusief [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), en [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). U kunt elk van deze in ASP.NET Core logboekregistratie aansluiten of u afzonderlijk kunt gebruiken. Serilog bevat een functie waarmee alle berichten van een logboek verrijkt. Deze functie kan nuttig zijn voor de servicenaam, het type en de partitiegegevens uitvoer. Als u deze mogelijkheid in de infrastructuur van ASP.NET Core, voer deze stappen uit:

1. De Serilog Serilog.Extensions.Logging en Serilog.Sinks.Observable NuGet-pakketten toevoegen aan het project. Voor het volgende voorbeeld moet u ook Serilog.Sinks.Literate toevoegen. Een betere benadering wordt verderop in dit artikel weergegeven.
2. In Serilog, maakt u een LoggerConfiguration en het exemplaar van het berichtenlogboek.

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. Een argument Serilog.ILogger toevoegen aan de constructor service en de zojuist gemaakte berichtenlogboek doorgeven.

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. Voeg in de constructor service de volgende code, die wordt gemaakt van de eigenschap enrichers voor de **ServiceTypeName**, **ServiceName**, **PartitionId**, en **InstanceId** eigenschappen van de service. Ook wordt toegevoegd een eigenschap enricher ASP.NET Core logboekregistratie fabriek, zodat u Microsoft.Extensions.Logging.ILogger in uw code gebruiken kunt.

  ```csharp
  public Stateless(StatelessServiceContext context, Serilog.ILogger serilog)
      : base(context)
  {
      PropertyEnricher[] properties = new PropertyEnricher[]
      {
          new PropertyEnricher("ServiceTypeName", context.ServiceTypeName),
          new PropertyEnricher("ServiceName", context.ServiceName),
          new PropertyEnricher("PartitionId", context.PartitionId),
          new PropertyEnricher("InstanceId", context.ReplicaOrInstanceId),
      };

      serilog.ForContext(properties);

      _logger = new LoggerFactory().AddSerilog(serilog.ForContext(properties)).CreateLogger<Stateless>();
  }
  ```

5. Softwareontwikkelaars de code hetzelfde als wanneer u ASP.NET Core zonder Serilog.

  >[!NOTE]
  >U wordt aangeraden dat u de statische Log.Logger niet met het voorgaande voorbeeld gebruiken. Service Fabric kunt meerdere exemplaren van hetzelfde servicetype binnen een enkel proces hosten. Als u de statische Log.Logger gebruikt, ziet de laatste schrijver van de eigenschap enrichers waarden voor alle exemplaren die worden uitgevoerd. Dit is een reden waarom de variabele _logger een variabele persoonlijk lid van de serviceklasse. Bovendien moet u de _logger beschikbaar is voor algemene code, die kan worden gebruikt op services.

## <a name="choosing-a-logging-provider"></a>Een provider logboekregistratie kiezen

Als uw toepassing afhankelijk van hoge prestaties is, **EventSource** is meestal een goede benadering. **EventSource** *doorgaans* minder bronnen gebruikt en beter presteert dan ASP.NET Core logboekregistratie of een van de beschikbare oplossingen van derden.  Dit is een probleem voor veel services, maar als uw service prestatiegerichte, met is niet **EventSource** mogelijk een betere keuze. Gestructureerde echter ophalen van deze voordelen van logboekregistratie, **EventSource** moet een grotere geïnvesteerd van het technische team. Indien mogelijk een snelle prototype van een aantal logboekregistratieopties doen en kies vervolgens die het beste voldoet aan uw behoeften.

## <a name="next-steps"></a>Volgende stappen

Nadat u ervoor uw provider logboekregistratie gekozen hebt softwareontwikkelaars uw toepassingen en services, moeten uw logboeken en gebeurtenissen worden geaggregeerd voordat ze kunnen worden verzonden naar een willekeurig platform analyse. Meer informatie over [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) en [af](service-fabric-diagnostics-event-aggregation-wad.md) om enkele van de aanbevolen opties beter te begrijpen.
