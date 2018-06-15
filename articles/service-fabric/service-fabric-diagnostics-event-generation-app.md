---
title: Azure Service Fabric-toepassingsniveau bewaking | Microsoft Docs
description: Meer informatie over de toepassing en service level gebeurtenissen en logboeken die worden gebruikt om te bewaken en onderzoeken van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2018
ms.author: dekapur
ms.openlocfilehash: b8118d83e2be452c6aa5bbc8b7a3c220d26903a1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204328"
---
# <a name="application-and-service-level-logging"></a>Toepassing en service logboekregistratie

De code instrumenteren vormt de basis voor de meeste andere aspecten van de bewaking van uw services. Instrumentatie is de enige manier kunt u weet dat er iets mis is, en op te sporen wat moet worden opgelost. Hoewel het is technisch mogelijk verbinding maken met een foutopsporingsprogramma een productieservice, maar het is niet een gangbare optie. Dus is het belangrijk gedetailleerde gegevens.

Sommige producten softwareontwikkelaars automatisch uw code. Hoewel deze oplossingen goed werken kunnen, is bijna altijd handmatige instrumentation vereist. U moet voldoende informatie voor foutopsporing van de toepassing volledig hebben in het einde. Dit document beschrijft de verschillende methoden voor het instrumenteren van uw code en het kiezen van een methode op een andere.

Zie voor voorbeelden voor het gebruik van deze suggesties [logboekregistratie toevoegen aan uw Service Fabric-toepassing](service-fabric-how-to-diagnostics-log.md).

## <a name="eventsource"></a>EventSource

Wanneer u een Service Fabric-oplossing van een sjabloon in Visual Studio maakt een **EventSource**-afgeleide klasse (**ServiceEventSource** of **ActorEventSource**) wordt gegenereerd. Een sjabloon is gemaakt, waarin u de gebeurtenissen voor uw toepassing of service kunt toevoegen. De **EventSource** naam **moet** uniek zijn en moet worden gewijzigd van de sjabloon standaardtekenreeks mijnbedrijf -&lt;oplossing&gt;-&lt;project&gt;. Meerdere **EventSource** definities die dezelfde naam gebruiken voor een probleem zorgt tijdens runtime. Elke gedefinieerde gebeurtenis moet een unieke id hebben. Als een id is niet uniek is, treedt er een runtime-fout op. Sommige organisaties preassign bereiken met waarden voor de id's voorkomen van conflicten tussen afzonderlijke ontwikkelteams. Zie voor meer informatie [van Vance blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) of de [MSDN-documentatie](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

## <a name="aspnet-core-logging"></a>ASP.NET Core logboekregistratie

Het is belangrijk dat u zorgvuldig plannen hoe u uw code wordt instrumenteren. Het plan rechts instrumentation kunt u mogelijk uw codebasis destabilizing en hoeven de code reinstrument voorkomen. Om risico te beperken, kunt u een instrumentatiebiblotheek zoals [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), die deel uitmaakt van Microsoft ASP.NET Core. ASP.NET Core is een [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) interface die u met de leverancier van uw keuze, gebruiken kunt terwijl het effect op de bestaande code worden geminimaliseerd. U kunt de code in ASP.NET Core voor Windows en Linux, en in het volledige .NET Framework, zodat uw code instrumentation gestandaardiseerd.

## <a name="application-insights-sdk"></a>Application Insights-SDK

Application Insights is een uitgebreide geïntegreerd met Service Fabric gebruiksklaar. Gebruikers kunnen de AI Service Fabric-nuget-pakketten toevoegen en ontvangen van gegevens en logboekbestanden gemaakt en verzameld die kunnen worden weergegeven in de Azure portal. Gebruikers wordt bovendien aangeraden om toe te voegen met hun eigen telemetrie om te onderzoeken en fouten opsporen in hun toepassingen en bijhouden welke services en onderdelen van de toepassing zijn het meest gebruikt. De [TelemetryClient](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient?view=azure-dotnet) klasse in de SDK biedt veel verschillende manieren om bij te houden van telemetrie in uw toepassingen. Bekijk een voorbeeld van het instrumenteren en application insights toevoegen aan uw toepassing in onze zelfstudie voor [bewaking en het onderzoeken van een .NET-toepassing](service-fabric-tutorial-monitoring-aspnet.md)


## <a name="next-steps"></a>Volgende stappen

Nadat u ervoor uw provider logboekregistratie gekozen hebt softwareontwikkelaars uw toepassingen en services, moeten uw logboeken en gebeurtenissen worden geaggregeerd voordat ze kunnen worden verzonden naar een willekeurig platform analyse. Meer informatie over [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md), [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md), en [af](service-fabric-diagnostics-event-aggregation-wad.md) om enkele van de aanbevolen opties beter te begrijpen.
