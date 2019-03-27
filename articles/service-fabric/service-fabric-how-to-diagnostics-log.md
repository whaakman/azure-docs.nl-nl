---
title: Gebeurtenissen genereren vanuit een .NET Service Fabric-app in Azure of een zelfstandige-cluster
description: Meer informatie over hoe u logboekregistratie toevoegen aan uw .NET Service Fabric-toepassing die wordt gehost op een Azure-cluster of een zelfstandige-cluster.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/27/2018
ms.author: srrengar
ms.openlocfilehash: 04eb3a0f46cbf02f1e8e3aca687f5d425792ed76
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499757"
---
# <a name="add-logging-to-your-service-fabric-application"></a>Logboekregistratie toevoegen aan uw Service Fabric-toepassing

Uw toepassing moet voldoende informatie gegeven om volledig foutopsporing kunt uitvoeren wanneer er zich problemen voordoen opgeven. Logboekregistratie is een van de meest belangrijke dingen die u aan uw Service Fabric-toepassing toevoegen kunt. Wanneer een fout optreedt, kunt goede logboekregistratie geven u een manier voor het onderzoeken van fouten. Door het logboek patronen analyseren, merkt u misschien verbeteren de prestaties of het ontwerp van uw toepassing. Dit document ziet u een aantal verschillende logboekregistratie-opties.

## <a name="eventflow"></a>EventFlow

De [EventFlow bibliotheek](https://github.com/Azure/diagnostics-eventflow) suite zorgt ervoor dat toepassingen om te definiëren welke diagnostische gegevens te verzamelen en waar ze moeten worden output aan. Diagnostische gegevens kan van alles van prestatiemeteritems kunnen toepassingstraceringen zijn. Deze wordt uitgevoerd in hetzelfde proces als de toepassing, zodat de overhead van de communicatie wordt geminimaliseerd. Zie voor meer informatie over EventFlow en Service Fabric [Azure Service Fabric Event aggregatie samenstellen met EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md).

### <a name="using-structured-eventsource-events"></a>Met behulp van gestructureerde EventSource gebeurtenissen

Definiëren bericht gebeurtenissen door use-case kunt u gegevens over het softwarepakket over de gebeurtenis, in de context van de gebeurtenis. Kunt u eenvoudig kunt zoeken en filteren op basis van de namen of waarden van de eigenschappen van de opgegeven gebeurtenis. Structureren van de uitvoer instrumentation maakt vereist het gemakkelijker te lezen, maar meer thought en tijd voor het definiëren van een gebeurtenis voor elk use-case. 

Sommige event-definities kunnen worden gedeeld met de gehele toepassing. Bijvoorbeeld, een methode starten of stoppen zou gebeurtenis worden hergebruikt voor veel services binnen een toepassing. Een service domeinspecifieke, zoals een systeem order mogelijk een **CreateOrder** gebeurtenis die een eigen unieke evenement heeft. Deze aanpak kan veel gebeurtenissen genereren en vereisen mogelijk coördinatie van id's tussen projectteams. 

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

### <a name="using-eventsource-generically"></a>Met behulp van de gebeurtenisbron algemeen

Omdat het definiëren van specifieke gebeurtenissen kan lastig zijn, definiëren veel mensen een aantal gebeurtenissen met een gemeenschappelijke set parameters die in het algemeen hun informatie als een tekenreeks voeren. Veel van het gestructureerde aspect verloren, en is het moeilijker om te zoeken en de resultaten te filteren. In deze benadering, zijn enkele gebeurtenissen die meestal met de niveaus voor logboekregistratie overeen worden gedefinieerd. Het volgende fragment definieert een bericht voor foutopsporing en fout:

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

Met behulp van een hybride van gestructureerde en algemene instrumentation kan ook goed werken. Gestructureerde instrumentation wordt gebruikt voor het melden van fouten en metrische gegevens. Algemene gebeurtenissen kunnen worden gebruikt voor de gedetailleerde logboekregistratie die wordt gebruikt door technici voor het oplossen van problemen.

## <a name="microsoftextensionslogging"></a>Microsoft.Extensions.Logging

De ASP.NET Core logboekregistratie ([Microsoft.Extensions.Logging NuGet-pakket](https://www.nuget.org/packages/Microsoft.Extensions.Logging)) is een framework voor logboekregistratie een API standaard logboekregistratie voor uw toepassing biedt. Ondersteuning voor andere back-ends voor logboekregistratie kan worden aangesloten op ASP.NET Core logboekregistratie. Dit biedt u tal van ondersteuning voor logboekregistratie in uw toepassing is verwerkt, zonder veel code te wijzigen.

1. Voeg de **Microsoft.Extensions.Logging** NuGet pakket aan het project die u wilt die is geregistreerd. Alle pakketten provider ook toe te voegen. Zie voor meer informatie, [logboekregistratie in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Voeg een **met behulp van** richtlijn voor **Microsoft.Extensions.Logging** naar uw servicebestand.
3. Definieer een persoonlijke variabele in uw serviceklasse.

   ```csharp
   private ILogger _logger = null;
   ```

4. Voeg deze code toe in de constructor van uw serviceklasse:

   ```csharp
   _logger = new LoggerFactory().CreateLogger<Stateless>();
   ```

5. Start het instrumenteren van uw code in uw methoden. Hier volgen enkele voorbeelden:

   ```csharp
   _logger.LogDebug("Debug-level event from Microsoft.Logging");
   _logger.LogInformation("Informational-level event from Microsoft.Logging");

   // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and the duration of the request.
   // Later in the article, we discuss why this step is useful.
   _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);
   ```

### <a name="using-other-logging-providers"></a>Met behulp van andere leveranciers van logboekregistratie

Sommige providers van derden gebruiken de benadering die wordt beschreven in de vorige sectie, met inbegrip van [Serilog](https://serilog.net/), [NLog](https://nlog-project.org/), en [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). U kunt elk van deze elementen in ASP.NET Core logboekregistratie aansluiten of u afzonderlijk kunt gebruiken. Serilog heeft een functie waarmee u alle berichten van een logger verrijkt. Deze functie is handig om uit te voeren van de servicenaam, het type en de partitie-informatie. Als u deze mogelijkheid in de ASP.NET Core-infrastructuur, moet u deze stappen uitvoeren:

1. Voeg de **Serilog**, **Serilog.Extensions.Logging**, **Serilog.Sinks.Literate**, en **Serilog.Sinks.Observable** NuGet-pakketten aan het project. 
2. Maak een `LoggerConfiguration` en de logger-instantie.

   ```csharp
   Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
   ```

3. Voeg een `Serilog.ILogger` argument voor de constructor van de service, en geeft u de zojuist gemaakte logger.

   ```csharp
   ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
   ```

4. Hiermee maakt u in de constructor service eigenschap enrichers voor **ServiceTypeName**, **ServiceName**, **PartitionId**, en **InstanceId** .

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

5. Instrumenteer de code hetzelfde als wanneer u ASP.NET Core zonder Serilog gebruikten.

   >[!NOTE]
   >We raden aan dat u *niet* gebruikt u de statische `Log.Logger` met het voorgaande voorbeeld. Service Fabric kan meerdere exemplaren van het type van de dezelfde service binnen een enkel proces host. Als u de statische `Log.Logger`, de laatste schrijver van de eigenschap enrichers ziet u waarden voor alle exemplaren die worden uitgevoerd. Dit is een reden waarom de _logger variabele $a persoonlijk lid van de serviceklasse. Bovendien moet u ervoor de `_logger` beschikbaar voor algemene code, die kan worden gebruikt voor services.

## <a name="next-steps"></a>Volgende stappen

- Lees meer informatie over [toepassing bewaken in Service Fabric](service-fabric-diagnostics-event-generation-app.md).
- Meer informatie over logboekregistratie met [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) en [Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md).










