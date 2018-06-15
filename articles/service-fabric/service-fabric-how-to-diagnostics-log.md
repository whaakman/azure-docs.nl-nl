---
title: Logboekgebeurtenissen genereren vanuit een .NET Service Fabric-app in Azure of een zelfstandige-cluster
description: Meer informatie over het toevoegen van logboekregistratie voor uw toepassing .NET Service Fabric is gehost op een Azure-cluster of een zelfstandige-cluster.
services: service-fabric
documentationcenter: .net
author: thraka
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/27/2018
ms.author: adegeo
ms.openlocfilehash: ed9aaf67b4f6749ea6d505a51fbc76e3d1cf0870
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204869"
---
# <a name="add-logging-to-your-service-fabric-application"></a>Logboekregistratie aan uw Service Fabric-toepassing toevoegen

Uw toepassing moet voldoende informatie geven om volledig debug wanneer zich problemen voordoen. Logboekregistratie is een van de belangrijkste dingen die u aan uw Service Fabric-toepassing toevoegen kunt. Wanneer een fout optreedt, kunt goed logboekregistratie geven u een manier voor het onderzoeken van problemen. Logboek patronen analyseren, merkt u verbeteren de prestaties of het ontwerp van uw toepassing. Dit document ziet u enkele andere aanmeldopties.

## <a name="eventflow"></a>EventFlow

De [EventFlow bibliotheek](https://github.com/Azure/diagnostics-eventflow) suite zorgt ervoor dat toepassingen om te definiëren welke diagnostische gegevens te verzamelen en waar ze moeten output aan. Diagnostische gegevens kunnen worden alles van prestatiemeteritems toepassingstraceringen. Deze wordt uitgevoerd in hetzelfde proces als de toepassing, zodat de overhead van de communicatie wordt geminimaliseerd. Zie voor meer informatie over EventFlow en Service Fabric [aggregatie van Azure Service Fabric-gebeurtenis met EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md).

### <a name="using-structured-eventsource-events"></a>Het gebruik van gestructureerde EventSource gebeurtenissen

Definiërende bericht gebeurtenissen op gebruiksvoorbeeld kunt u gegevens over de gebeurtenis, in de context van de gebeurtenis. Kunt u eenvoudig zoeken en filteren op basis van de namen of waarden van de eigenschappen van de opgegeven gebeurtenis. Zorgt ervoor dat de uitvoer instrumentation structureren vereist gemakkelijker te lezen, maar meer gedachte en tijd voor het definiëren van een gebeurtenis voor elke gebruiksvoorbeeld. 

Sommige Gebeurtenisdefinities kunnen worden gedeeld door de gehele toepassing. Bijvoorbeeld, een methode starten of stoppen zou gebeurtenis hele veel services in een toepassing worden gebruikt. Een service, domeinspecifieke, zoals een systeem volgorde wellicht een **CreateOrder** gebeurtenis een eigen unieke gebeurtenis heeft. Deze aanpak mogelijk veel gebeurtenissen genereren en mogelijk coördinatie vereisen van id's op projectteams. 

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

    ...

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

    ...

```

Een hybride van gestructureerde en algemene instrumentation kunt ook werken goed. Gestructureerde instrumentation wordt gebruikt voor het melden van fouten en metrische gegevens. Algemene gebeurtenissen kunnen worden gebruikt voor de gedetailleerde logboekregistratie die wordt gebruikt door engineers voor probleemoplossing.

## <a name="microsoftextensionslogging"></a>Microsoft.Extensions.Logging

De ASP.NET Core logboekregistratie ([Microsoft.Extensions.Logging NuGet-pakket](https://www.nuget.org/packages/Microsoft.Extensions.Logging)) is een framework voor logboekregistratie met een API standaard logboekregistratie voor uw toepassing. Ondersteuning voor andere back-ends voor logboekregistratie kan worden aangesloten op ASP.NET Core logboekregistratie. Dit biedt u een groot aantal ondersteuning voor logboekregistratie in uw toepassing is verwerkt, zonder te veel code te wijzigen.

1. Voeg de **Microsoft.Extensions.Logging** NuGet pakket aan het project die u wilt instrument. Alle pakketten provider ook toe te voegen. Zie voor meer informatie [logboekregistratie in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Voeg een **met** richtlijn voor **Microsoft.Extensions.Logging** in uw servicebestand.
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

### <a name="using-other-logging-providers"></a>Andere providers voor logboekregistratie

Sommige leveranciers gebruik de methode die wordt beschreven in de vorige sectie, inclusief [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), en [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). U kunt elk van deze in ASP.NET Core logboekregistratie aansluiten of u afzonderlijk kunt gebruiken. Serilog bevat een functie waarmee alle berichten van een logboek verrijkt. Deze functie kan nuttig zijn voor de servicenaam, het type en de partitiegegevens uitvoer. Als u deze mogelijkheid in de infrastructuur van ASP.NET Core, voer deze stappen uit:

1. Voeg de **Serilog**, **Serilog.Extensions.Logging**, **Serilog.Sinks.Literate**, en **Serilog.Sinks.Observable** NuGet-pakketten aan het project. 
2. Maak een `LoggerConfiguration` en het exemplaar van het berichtenlogboek.

   ```csharp
   Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
   ```

3. Voeg een `Serilog.ILogger` argument voor de service-constructor aan, en geeft u de zojuist gemaakte berichtenlogboek.

   ```csharp
   ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
   ```

4. In de constructor service maakt eigenschap enrichers voor **ServiceTypeName**, **ServiceName**, **PartitionId**, en **InstanceId** .

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
   >We raden aan dat u *niet* gebruiken vaste `Log.Logger` met het voorgaande voorbeeld. Service Fabric kunt meerdere exemplaren van hetzelfde servicetype binnen een enkel proces hosten. Als u de statische `Log.Logger`, de laatste schrijver van de eigenschap enrichers wordt weergegeven voor alle exemplaren die worden uitgevoerd. Dit is een reden waarom de variabele _logger een variabele persoonlijk lid van de serviceklasse. Bovendien moet u ervoor de `_logger` beschikbaar is voor algemene code, die kan worden gebruikt op services.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie lezen over [toepassing bewaken in Service Fabric](service-fabric-diagnostics-event-generation-app.md).
- Meer informatie over logboekregistratie met [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) en [Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md).










