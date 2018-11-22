---
title: Azure Service Fabric-toepassingsniveau bewaking | Microsoft Docs
description: Meer informatie over de toepassing en service level gebeurtenissen en logboeken die worden gebruikt om te controleren en een diagnose van Azure Service Fabric-clusters.
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
ms.openlocfilehash: 532ad8dfeff9a14d3d1533669149e1e374ed4460
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52283227"
---
# <a name="application-and-service-level-logging"></a>Toepassings- en logboekregistratie op de service

De code instrumenteren vormt de basis voor de meeste andere aspecten van uw services controleren. Instrumentatie is de enige manier weet u dat er iets mis is, en om te onderzoeken wat er moet worden hersteld. Hoewel het is technisch mogelijk een foutopsporingsprogramma verbinden met een productieservice, is het niet gebruikelijk. Gedetailleerde gegevens is dus belangrijk.

Sommige producten instrumenteer automatisch uw code. Hoewel deze oplossingen ook werken kunnen, is bijna altijd handmatige instrumentation vereist. U moet voldoende informatie voor foutopsporing van de toepassing volledig hebben in het einde. Dit document beschrijft de verschillende methoden voor het instrumenteren van uw code en wanneer u een methode kiezen op een andere.

Zie voor meer voorbeelden over het gebruik van deze suggesties [logboekregistratie toevoegen aan uw Service Fabric-toepassing](service-fabric-how-to-diagnostics-log.md).

## <a name="eventsource"></a>Gebeurtenisbron

Wanneer u een Service Fabric-oplossing met een sjabloon in Visual Studio maakt een **EventSource**-afgeleide klasse (**ServiceEventSource** of **ActorEventSource**) is gegenereerd . Een sjabloon is gemaakt, waarin u gebeurtenissen voor uw toepassing of service kunt toevoegen. De **EventSource** naam **moet** uniek zijn en moet worden gewijzigd van de sjabloon standaardtekenreeks mijnbedrijf -&lt;oplossing&gt;-&lt;project &gt;. Met meerdere **EventSource** definities die gebruikmaken van dezelfde naam zorgt ervoor dat een probleem tijdens de uitvoering. Elke gedefinieerde gebeurtenis moet een unieke id hebben. Als een id is niet uniek is, wordt er een runtime-fout optreedt. Sommige organisaties preassign bereiken van de waarden voor id's voor het voorkomen van conflicten tussen afzonderlijke ontwikkelteams. Zie voor meer informatie, [van Vance blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) of de [MSDN-documentatie](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

## <a name="aspnet-core-logging"></a>ASP.NET Core-logboekregistratie

Het is belangrijk dat u zorgvuldig plannen hoe u uw code wordt instrumenteren. Het juiste instrumentatie-abonnement kunt u mogelijk destabilizing uw codebasis en hoeven de code reinstrument voorkomen. Als risico wilt voorkomen, kunt u een instrumentatiebiblotheek zoals [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), deze maakt deel uit van Microsoft ASP.NET Core. ASP.NET Core is een [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) interface die u met de leverancier van uw keuze, gebruiken kunt terwijl het effect op bestaande code worden geminimaliseerd. U kunt de code in ASP.NET Core in Windows en Linux en in het volledige .NET Framework, zodat uw code instrumentation gestandaardiseerd.

## <a name="application-insights-sdk"></a>Application Insights-SDK

Application Insights is een krachtige integratie met Service Fabric buiten het vak. Gebruikers kunnen de AI-Service Fabric-nuget-pakketten toevoegen en ontvangen van gegevens en logboeken die zijn gemaakt en die worden verzameld in de Azure-portal kan worden weergegeven. Gebruikers wordt bovendien aangeraden om toe te voegen met hun eigen telemetrie om te kunnen vaststellen en oplossen van hun toepassingen en bijhouden welke services en delen van hun toepassing zijn het meest gebruikt. De [TelemetryClient](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient?view=azure-dotnet) klasse in de SDK biedt allerlei manieren om bij te houden van telemetrie in uw toepassingen. Bekijk een voorbeeld van het instrumenteren en application insights toevoegen aan uw toepassing in onze zelfstudie voor [bewaking en diagnose van een .NET-toepassing](service-fabric-tutorial-monitoring-aspnet.md)


## <a name="next-steps"></a>Volgende stappen

Nadat u uw provider voor logboekregistratie voor het instrumenteren van uw toepassingen en services hebt gekozen, moeten uw logboeken en gebeurtenissen ook kunnen samenvoegen voordat ze kunnen worden verzonden naar elk platform voor analyse. Meer informatie over [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md), [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md), en [WAD](service-fabric-diagnostics-event-aggregation-wad.md) om enkele van de aanbevolen opties beter te begrijpen.
