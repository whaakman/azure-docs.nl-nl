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
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2018
ms.author: dekapur
ms.openlocfilehash: f3e7b9c7432538c0f78662213544d4d691652f13
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
# <a name="application-and-service-level-logging"></a>Toepassing en service logboekregistratie

De code instrumenteren vormt de basis voor de meeste andere aspecten van de bewaking van uw services. Instrumentatie is de enige manier kunt u weet dat er iets mis is, en op te sporen wat moet worden opgelost. Hoewel het is technisch mogelijk verbinding maken met een foutopsporingsprogramma een productieservice, maar het is niet een gangbare optie. Dus is het belangrijk gedetailleerde gegevens.

Sommige producten softwareontwikkelaars automatisch uw code. Hoewel deze oplossingen goed werken kunnen, is bijna altijd handmatige instrumentation vereist. U moet voldoende informatie voor foutopsporing van de toepassing volledig hebben in het einde. Dit document beschrijft de verschillende methoden voor het instrumenteren van uw code en het kiezen van een methode op een andere.

Zie voor voorbeelden voor het gebruik van deze suggesties [logboekregistratie toevoegen aan uw Service Fabric-toepassing](service-fabric-how-to-diagnostics-log.md).

## <a name="eventsource"></a>EventSource

Wanneer u een Service Fabric-oplossing van een sjabloon in Visual Studio maakt een **EventSource**-afgeleide klasse (**ServiceEventSource** of **ActorEventSource**) wordt gegenereerd. Een sjabloon is gemaakt, waarin u de gebeurtenissen voor uw toepassing of service kunt toevoegen. De **EventSource** naam **moet** uniek zijn en moet worden gewijzigd van de sjabloon standaardtekenreeks mijnbedrijf -&lt;oplossing&gt;-&lt;project&gt;. Meerdere **EventSource** definities die dezelfde naam gebruiken voor een probleem zorgt tijdens runtime. Elke gedefinieerde gebeurtenis moet een unieke id hebben. Als een id is niet uniek is, treedt er een runtime-fout op. Sommige organisaties preassign bereiken met waarden voor de id's voorkomen van conflicten tussen afzonderlijke ontwikkelteams. Zie voor meer informatie [van Vance blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) of de [MSDN-documentatie](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

## <a name="aspnet-core-logging"></a>ASP.NET Core logboekregistratie

Het is belangrijk dat u zorgvuldig plannen hoe u uw code wordt instrumenteren. Het plan rechts instrumentation kunt u mogelijk uw codebasis destabilizing en hoeven de code reinstrument voorkomen. Om risico te beperken, kunt u een instrumentatiebiblotheek zoals [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), die deel uitmaakt van Microsoft ASP.NET Core. ASP.NET Core is een [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) interface die u met de leverancier van uw keuze, gebruiken kunt terwijl het effect op de bestaande code worden geminimaliseerd. U kunt de code in ASP.NET Core voor Windows en Linux, en in het volledige .NET Framework, zodat uw code instrumentation gestandaardiseerd.

## <a name="choosing-a-logging-provider"></a>Een provider logboekregistratie kiezen

Als uw toepassing afhankelijk van hoge prestaties is, **EventSource** is meestal een goede benadering. **EventSource** *doorgaans* minder bronnen gebruikt en beter presteert dan ASP.NET Core logboekregistratie of een van de beschikbare oplossingen van derden.  Dit is een probleem voor veel services, maar als uw service prestatiegerichte, met is niet **EventSource** mogelijk een betere keuze. Gestructureerde echter ophalen van deze voordelen van logboekregistratie, **EventSource** moet een grotere geïnvesteerd van het technische team. Indien mogelijk een snelle prototype van een aantal logboekregistratieopties doen en kies vervolgens die het beste voldoet aan uw behoeften.

## <a name="next-steps"></a>Volgende stappen

Nadat u ervoor uw provider logboekregistratie gekozen hebt softwareontwikkelaars uw toepassingen en services, moeten uw logboeken en gebeurtenissen worden geaggregeerd voordat ze kunnen worden verzonden naar een willekeurig platform analyse. Meer informatie over [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) en [af](service-fabric-diagnostics-event-aggregation-wad.md) om enkele van de aanbevolen opties beter te begrijpen.
