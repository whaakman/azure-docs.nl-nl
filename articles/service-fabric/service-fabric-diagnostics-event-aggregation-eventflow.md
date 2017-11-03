---
title: Aggregatie van Azure Service Fabric-gebeurtenis met EventFlow | Microsoft Docs
description: Meer informatie over het aggregeren en het verzamelen van gebeurtenissen met EventFlow voor controle en diagnostische gegevens van Azure Service Fabric-clusters.
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
ms.openlocfilehash: 9a6e629582b6966d270a2378e585572efe133f3e
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="event-aggregation-and-collection-using-eventflow"></a>Gebeurtenis-aggregatie en verzameling op basis van EventFlow

[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) gebeurtenissen van een knooppunt naar een of meer controle bestemmingen kunt versturen. Omdat het is opgenomen als een NuGet-pakket in uw serviceproject, reizen EventFlow code en configuratie met de service, het incompatibiliteitsprobleem per knooppunt configuratie eerder genoemde over Azure Diagnostics. EventFlow binnen uw serviceproces wordt uitgevoerd en rechtstreeks verbinding maakt met de geconfigureerde uitvoer. Vanwege de rechtstreekse verbinding werkt EventFlow voor Azure-container en on-premises service-implementaties. Let als u EventFlow in high-density scenario's, zoals in een container uitvoert, omdat elke EventFlow pijplijn een externe verbinding wordt. Dus als u verschillende processen host, u verschillende uitgaande verbindingen krijgt! Is dit niet zo veel mogelijk een probleem voor Service Fabric-toepassingen, omdat alle replica's van een `ServiceType` in hetzelfde proces worden uitgevoerd en dit beperkt het aantal uitgaande verbindingen. EventFlow biedt ook filteren van gebeurtenissen, zodat alleen de gebeurtenissen die overeenkomen met het opgegeven filter worden verzonden.

## <a name="set-up-eventflow"></a>EventFlow instellen

EventFlow binaire bestanden zijn beschikbaar als een reeks NuGet-pakketten. EventFlow toevoegen aan een Service Fabric-service-project, met de rechtermuisknop op het project in Solution Explorer en kiest u 'Manage NuGet packages'. Overschakelen naar het tabblad 'Zoeken' en zoek naar '`Diagnostics.EventFlow`':

![EventFlow NuGet-pakketten in Visual Studio-NuGet-Pakketbeheer gebruikersinterface](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

U ziet een lijst met verschillende pakketten die worden weergegeven, met labels met 'Invoer' en 'Levert'. EventFlow ondersteunt verschillende andere logboekregistratie providers en analyzers. De service die als host fungeert voor EventFlow moet de juiste pakketten, afhankelijk van de bron en bestemming voor de toepassingslogboeken bevatten. Naast het core ServiceFabric pakket, moet u ook minimaal één invoer en uitvoer geconfigureerd. U kunt bijvoorbeeld de volgende pakketten toevoegen aan verzonden EventSource gebeurtenissen naar Application Insights:

* `Microsoft.Diagnostics.EventFlow.Input.EventSource`voor het vastleggen van gegevens uit de serviceklasse EventSource, en standard EventSources zoals *ServiceFabric-Microsoft-Services* en *Microsoft-ServiceFabric-Actors*)
* `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`(we gaan de logboeken verzenden naar een Azure Application Insights-resource)
* `Microsoft.Diagnostics.EventFlow.ServiceFabric`(Hiermee initialisatie van de pijplijn EventFlow van de configuratie van de Service Fabric-service en eventuele problemen met het verzenden van diagnostische gegevens als Service Fabric-statusrapporten rapporten)

>[!NOTE]
>`Microsoft.Diagnostics.EventFlow.Input.EventSource`pakket is de service-project toe te passen van .NET Framework 4.6 of hoger vereist. Zorg ervoor dat u het juiste doel-framework in Projecteigenschappen instellen voordat u dit pakket installeert.

Nadat alle pakketten zijn geïnstalleerd, wordt de volgende stap is het configureren en inschakelen van EventFlow in de service.

## <a name="configure-and-enable-log-collection"></a>Configureren en inschakelen van verzamelen
De pijplijn EventFlow verantwoordelijk voor het verzenden van de logboeken wordt gemaakt van een specificatie die is opgeslagen in een configuratiebestand. De `Microsoft.Diagnostics.EventFlow.ServiceFabric` pakket installeert u een eerste EventFlow configuratiebestand onder `PackageRoot\Config` oplossingenmap met de naam `eventFlowConfig.json`. Dit configuratiebestand moet worden gewijzigd voor het vastleggen van gegevens van de standaardservice `EventSource` klasse en alle andere invoer die u wilt configureren en gegevens verzenden naar de juiste plaats.

Hier volgt een voorbeeld *eventFlowConfig.json* op basis van de hierboven genoemde NuGet-pakketten:
```json
{
  "inputs": [
    {
      "type": "EventSource",
      "sources": [
        { "providerName": "Microsoft-ServiceFabric-Services" },
        { "providerName": "Microsoft-ServiceFabric-Actors" },
        // (replace the following value with your service's ServiceEventSource name)
        { "providerName": "your-service-EventSource-name" }
      ]
    }
  ],
  "filters": [
    {
      "type": "drop",
      "include": "Level == Verbose"
    }
  ],
  "outputs": [
    {
      "type": "ApplicationInsights",
      // (replace the following value with your AI resource's instrumentation key)
      "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
  ],
  "schemaVersion": "2016-08-11"
}
```

De naam van de ServiceEventSource van de service is de waarde van de eigenschap Name van de `EventSourceAttribute` toegepast op de klasse ServiceEventSource. Deze al is opgegeven in de `ServiceEventSource.cs` -bestand, dat deel uitmaakt van de servicecode. In het volgende codefragment de naam van de ServiceEventSource is bijvoorbeeld *mijnbedrijf-Toepassing1-Stateless1*:

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

Houd er rekening mee dat `eventFlowConfig.json` bestand maakt deel uit van het configuratiepakket service. Wijzigingen in dit bestand kunnen worden opgenomen in de volledige - of configuratie-only upgrades van de service, zijn de Service Fabric-upgrade statuscontroles en automatisch terugdraaien als er een mislukte upgrade. Zie voor meer informatie [upgrade van de Service Fabric-toepassing](service-fabric-application-upgrade.md).

De *filters* sectie van de configuratie kunt u verder aanpassen van de informatie die u naar de uitvoer, zodat u kunt verwijderen of bepaalde informatie bevatten die via de pipeline EventFlow of wijzig de structuur van de gebeurtenisgegevens worden opgeslagen. Zie voor meer informatie over filters [EventFlow filters](https://github.com/Azure/diagnostics-eventflow#filters).

De laatste stap is het instantiëren van EventFlow pijplijn in uw service starten van de code, zich in `Program.cs` bestand:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric;
using Microsoft.ServiceFabric.Services.Runtime;

// **** EventFlow namespace
using Microsoft.Diagnostics.EventFlow.ServiceFabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>
        private static void Main()
        {
            try
            {
                // **** Instantiate log collection via EventFlow
                using (var diagnosticsPipeline = ServiceFabricDiagnosticPipelineFactory.CreatePipeline("MyApplication-MyService-DiagnosticsPipeline"))
                {

                    ServiceRuntime.RegisterServiceAsync("Stateless1Type",
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                    ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                    Thread.Sleep(Timeout.Infinite);
                }
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
                throw;
            }
        }
    }
}
```

Naam van de doorgegeven als parameter van de `CreatePipeline` methode van de `ServiceFabricDiagnosticsPipelineFactory` is de naam van de *health entiteit* die de pijplijn EventFlow logboek verzameling vertegenwoordigt. Deze naam wordt gebruikt als EventFlow aantreft en de fout en gerapporteerd via het subsysteem van de health Service Fabric.

### <a name="use-service-fabric-settings-and-application-parameters-in-eventflowconfig"></a>Gebruik Service Fabric-instellingen en parameters voor de toepassing in eventFlowConfig

EventFlow ondersteunt het gebruik van Service Fabric-instellingen en parameters van de toepassing om EventFlow instellingen te configureren. U kunt verwijzen naar Service Fabric-instellingen parameters in met behulp van deze speciale syntaxis voor waarden:

```json
servicefabric:/<section-name>/<setting-name>
```

`<section-name>`de naam van de configuratiesectie Service Fabric en `<setting-name>` is de configuratieinstelling de waarde die wordt gebruikt voor het configureren van een instelling EventFlow op te geven. Om te lezen over hoe u dit doet, gaat u naar [ondersteuning voor Service Fabric-instellingen en parameters voor de toepassing](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).

## <a name="verification"></a>Verificatie

De service starten en houd rekening met de opdracht Debug uitvoervenster in Visual Studio. Nadat de service is gestart, moet u eerst zien bewijs dat uw service records verzendt naar de uitvoer die u hebt geconfigureerd. Navigeer naar uw platform voor analyse en visualisatie van gebeurtenis en controleer of de logboeken hebt gestart om weer te geven tot (kan enkele minuten duren).

## <a name="next-steps"></a>Volgende stappen

* [Analyse van gebeurtenis en visualisatie met Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Analyse van gebeurtenis en visualisatie met OMS](service-fabric-diagnostics-event-analysis-oms.md)
* [EventFlow documentatie](https://github.com/Azure/diagnostics-eventflow)
