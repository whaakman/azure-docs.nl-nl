---
title: Azure Service Fabric Event aggregatie samenstellen met EventFlow | Microsoft Docs
description: Meer informatie over aggregeren en het verzamelen van gebeurtenissen met EventFlow voor controle en diagnose van Azure Service Fabric-clusters.
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
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: eee5e71863f9a9195dc1f6f711cbacbc95ab84f2
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57246760"
---
# <a name="event-aggregation-and-collection-using-eventflow"></a>Aggregatie van de gebeurtenis en verzameling met EventFlow

[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) gebeurtenissen van een knooppunt naar een of meer controle bestemmingen kunt versturen. Omdat het is opgenomen als een NuGet-pakket in uw serviceproject, reizen EventFlow code en configuratie met de service, het incompatibiliteitsprobleem per knooppunt configuratie gezegd over Azure Diagnostics. EventFlow binnen uw serviceproces wordt uitgevoerd, en maakt rechtstreeks verbinding met de geconfigureerde uitvoer. Vanwege de rechtstreekse verbinding werkt EventFlow voor Azure, container en on-premises service-implementaties. Let goed op als u EventFlow in high-density scenario's, zoals in een container, omdat elke pijplijn die EventFlow een externe verbinding maakt. Dus als u verschillende processen hosten, krijgt u verschillende uitgaande verbindingen! Dit is niet zo veel belang voor Service Fabric-toepassingen, omdat alle replica's van een `ServiceType` worden uitgevoerd in hetzelfde proces, en dit beperkt het aantal uitgaande verbindingen. EventFlow biedt ook filteren van gebeurtenissen, zodat alleen de gebeurtenissen die overeenkomen met het opgegeven filter worden verzonden.

## <a name="set-up-eventflow"></a>Instellen van EventFlow

EventFlow binaire bestanden zijn beschikbaar als een set met NuGet-pakketten. EventFlow toevoegen aan een Service Fabric service-project, met de rechtermuisknop op het project in Solution Explorer en kies 'Manage NuGet packages'. Ga naar het tabblad "Zoeken" en zoek naar '`Diagnostics.EventFlow`':

![EventFlow NuGet-pakketten in Visual Studio-NuGet-Pakketbeheer UI](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

Hier ziet u een lijst verschillende pakketten die worden weergegeven, met een label met 'Invoer' en 'Levert'. EventFlow ondersteunt verschillende providers van verschillende logboekregistratie en analyse. De service die als host fungeert EventFlow moet de juiste pakketten afhankelijk van de bron en bestemming voor de toepassingslogboeken bevatten. Naast het core ServiceFabric-pakket, moet u ook ten minste één invoer en uitvoer geconfigureerd. U kunt bijvoorbeeld de volgende pakketten EventSource gebeurtenissen verzenden naar Application Insights toevoegen:

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` voor het vastleggen van gegevens van de serviceklasse gebeurtenisbron en van standard EventSources zoals *Microsoft-service fabric-Services* en *Microsoft-service fabric-actoren*)
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` (we gaan de logboeken verzenden naar een Azure Application Insights-resource)
* `Microsoft.Diagnostics.EventFlow.ServiceFabric`(kan de initialisatie van de pijplijn EventFlow van configuratie van Service Fabric-service en eventuele problemen met het verzenden van diagnostische gegevens als statusrapporten van Service Fabric-rapporten)

>[!NOTE]
>`Microsoft.Diagnostics.EventFlow.Inputs.EventSource` pakket is de service-project voor .NET Framework 4.6 of hoger vereist. Zorg ervoor dat u de juiste doelframework instellen in de Projecteigenschappen voordat u dit pakket installeert.

Nadat de pakketten zijn geïnstalleerd, wordt de volgende stap is het configureren en inschakelen van EventFlow in de service.

## <a name="configure-and-enable-log-collection"></a>Configureren en logboekverzameling inschakelen
De pijplijn EventFlow verantwoordelijk voor het verzenden van de logboeken is gemaakt op basis van een specificatie die is opgeslagen in een configuratiebestand. De `Microsoft.Diagnostics.EventFlow.ServiceFabric` pakket installeert een vanaf EventFlow configuratiebestand onder `PackageRoot\Config` oplossingenmap, met de naam `eventFlowConfig.json`. Dit configuratie-item moet worden aangepast voor het vastleggen van gegevens uit de standaardservice `EventSource` klasse en alle andere invoer die u wilt configureren en gegevens verzenden naar de juiste plaats.

>[!NOTE]
>Als uw projectbestand VisualStudio 2017-indeling heeft de `eventFlowConfig.json` bestand wordt niet automatisch worden toegevoegd. Om op te lossen dit Maak het bestand in de `Config` map en de build-actie ingesteld op `Copy if newer`. 

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

De naam van de service ServiceEventSource is de waarde van de naameigenschap van de `EventSourceAttribute` toegepast op de klasse ServiceEventSource. Er is al opgegeven in de `ServiceEventSource.cs` -bestand, dat deel van de servicecode uitmaakt. Bijvoorbeeld, in het volgende codefragment de naam van de ServiceEventSource is *mijnbedrijf-Toepassing1-Stateless1*:

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

Houd er rekening mee dat `eventFlowConfig.json` bestand maakt deel uit van het configuratiepakket voor de service. Wijzigingen in dit bestand kunnen worden opgenomen in de volledige - of alleen-configuratiereplica upgrades van de service, afhankelijk van Service Fabric-upgrade statuscontroles en automatische terugdraaien als er een mislukte upgrade. Zie voor meer informatie, [Service Fabric-toepassingsupgrade](service-fabric-application-upgrade.md).

De *filters* sectie van de configuratie kunt u verder aanpassen van de informatie die u naar de uitvoer, zodat u kunt verwijderen of bepaalde informatie bevatten die via de pijplijn EventFlow of wijzigen van de structuur van de de gegevens van de gebeurtenis. Zie voor meer informatie over het filteren van [EventFlow filters](https://github.com/Azure/diagnostics-eventflow#filters).

De laatste stap is het instantiëren van de pijplijn EventFlow in van uw service opstartcode, zich in `Program.cs` bestand:

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

De naam die wordt doorgegeven als parameter van de `CreatePipeline` -methode van de `ServiceFabricDiagnosticsPipelineFactory` is de naam van de *health entiteit* voor de pijplijn EventFlow log verzameling. Deze naam wordt gebruikt als EventFlow en de fout en rapporteert deze via het subsysteem van de health Service Fabric.

### <a name="use-service-fabric-settings-and-application-parameters-in-eventflowconfig"></a>Service Fabric-instellingen en parameters voor de toepassing in eventFlowConfig gebruiken

EventFlow ondersteunt het gebruik van Service Fabric-instellingen en parameters voor de toepassing om EventFlow instellingen te configureren. U kunt verwijzen naar Service Fabric-instellingen parameters met behulp van deze speciale syntaxis voor waarden:

```json
servicefabric:/<section-name>/<setting-name>
```

`<section-name>` de naam van de sectie van de configuratie van Service Fabric en `<setting-name>` is de configuratie-instelling die de waarde die wordt gebruikt om een instelling EventFlow te configureren. Voor meer informatie over hoe u dit doet, gaat u naar [ondersteuning voor Service Fabric-instellingen en parameters voor de toepassing](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).

## <a name="verification"></a>Verificatie

Start de service en houd rekening met de foutopsporing uitvoervenster in Visual Studio. Nadat de service is gestart, moet u beginnen zien blijkt dat uw service kan de records worden verzonden naar de uitvoer die u hebt geconfigureerd. Navigeer naar uw event-platform voor analyses en visualisatie en Bevestig dat de logboeken zijn gestart om weer te geven tot (kan een paar minuten duren).

## <a name="next-steps"></a>Volgende stappen

* [Gebeurtenis analyses en visualisatie met Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Gebeurtenis analyses en visualisatie met Azure Monitor-Logboeken](service-fabric-diagnostics-event-analysis-oms.md)
* [Documentatie voor EventFlow](https://github.com/Azure/diagnostics-eventflow)
