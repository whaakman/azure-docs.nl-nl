---
title: Azure Service Fabric controle en diagnostische gegevens overzicht | Microsoft Docs
description: Meer informatie over controle en diagnostische gegevens voor Azure Service Fabric-clusters, toepassingen en services.
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
ms.date: 04/25/2018
ms.author: srrengar
ms.openlocfilehash: 4fcf8c514cb785dbb0a149e5b3073fc72937b68a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Controle en diagnostische gegevens voor Azure Service Fabric

In dit artikel biedt een overzicht van controle en diagnostische gegevens voor Azure Service Fabric. Controle en diagnostische gegevens zijn essentieel om te ontwikkelen, testen en implementeren van werkbelastingen in een cloudomgeving. Bewaking, kunt u bijhouden hoe uw toepassingen worden gebruikt, het brongebruik en de algehele status van het cluster. U kunt deze informatie gebruiken om te onderzoeken en verhelp eventuele problemen en voorkomen dat er problemen optreden in de toekomst. 

## <a name="application-monitoring"></a>Toepassingsbewaking
Toepassingsbewaking houdt hoe functies en onderdelen van uw toepassing worden gebruikt. U wilt bewaken van uw toepassingen en zorg ervoor dat problemen die gebruikers zijn opgepikt impact. Bewaking van uw toepassingen kan handig zijn in de volgende scenario's zijn:
* het bepalen van laden van toepassingen en gebruiker verkeer - moet u uw services om te voldoen aan de eisen van de gebruiker of een potentiële knelpunten in uw toepassing adres schalen?
* identificeren van problemen met servicecommunicatie en externe toegang via het cluster
* Uitzoeken wat gebruikers met uw toepassing doen-het verzamelen van telemetriegegevens in uw toepassingen kunt u handleiding toekomstige functie ontwikkeling en betere diagnostische gegevens voor app-fouten
* Wat gebeurt er in uw actieve containers bewaking

Service Fabric ondersteunt een groot aantal opties voor het instrumenteren van uw toepassingscode met de juiste traceringen en telemetrie. Het is raadzaam dat u Application Insights (AI). AI van integratie met Service Fabric bevat tooling ervaringen voor Visual Studio en de Azure portal, evenals specifieke metrische gegevens op het Service Fabric, een uitgebreide logboekregistratie voor out-of-the-box-ervaring te bieden. Hoewel veel logboeken automatisch worden gemaakt en die voor u zijn verzameld met AI, wordt u aangeraden dat u verdere aangepaste logboekregistratie voor uw toepassingen toevoegt te maken van een rijkere ervaring voor diagnostische gegevens. Zie voor meer informatie over aan de slag met Application Insights met Service Fabric op [analyse van gebeurtenis met Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md).

## <a name="platform-cluster-monitoring"></a>Bewaking van platform (Cluster)
Bewaking van uw Service Fabric-cluster is essentieel om ervoor te zorgen dat het platform en alle werkbelastingen worden uitgevoerd zoals bedoeld. Een van de doelstellingen van de Service Fabric is dat toepassingen robuuste hardwarefouten. Dit doel wordt bereikt door het platform systeemservices mogelijkheid voor het detecteren van infrastructuur problemen en snel failover werkbelastingen naar andere knooppunten in het cluster. Maar in dit geval, wat gebeurt er als de systeemservices zelf problemen? Of als bij een poging te verplaatsen van een werkbelasting, regels voor de plaatsing van de services zijn geschonden? Bewaking van het cluster, kunt u op de hoogte over activiteit plaatsvinden in uw cluster, helpt bij het opsporen van problemen en corrigeren effectief blijven. Er zijn een aantal belangrijke zaken die u wilt zoeken naar:
* Is de Service Fabric werkt zoals verwacht, wat betreft het plaatsen van uw toepassingen en netwerktaakverdeling werk op het cluster? 
* Worden acties van de gebruiker uitgevoerd op uw cluster bevestigd en op worden uitgevoerd zoals verwacht? Dit is vooral van belang bij het schalen van een cluster.
* Service Fabric verwerkt uw gegevens en uw service-service-communicatie binnen het cluster correct?

Service Fabric bevat een uitgebreide reeks gebeurtenissen uit het vak. Deze [Service Fabric-gebeurtenissen](service-fabric-diagnostics-events.md) toegankelijk zijn via de EventStore APIs of het operationele kanaal (gebeurteniskanaal die worden weergegeven door het platform). 
* EventStore - de EventStore (beschikbaar op Windows in versie 6.2 en later, Linux nog in voortgang vanaf de datum voor dit artikel laatst bijgewerkt), wordt deze gebeurtenissen via een set API's (toegankelijk via de REST-eindpunten of via de clientbibliotheek). Meer informatie over de EventStore op de [EventStore overzicht](service-fabric-diagnostics-eventstore.md).
* Kanalen - in Windows service Fabric-Service Fabric gebeurtenisgevallen beschikbaar zijn via één ETW-provider met een reeks relevante `logLevelKeywordFilters` gebruikt om te kiezen tussen operationele gegevens & Messaging en kanalen - dit is de manier waarop we uitgaande onderscheiden Service Fabric-gebeurtenissen te filteren op die nodig zijn. Op Linux, Service Fabric-gebeurtenissen worden geleverd via LTTng en van waar ze kunnen worden gefilterd zo nodig in één opslagruimte tabel zijn geplaatst. Deze kanalen bevatten curated, gestructureerde gebeurtenissen die kunnen worden gebruikt om de status van uw cluster beter te begrijpen. Diagnostische gegevens zijn standaard ingeschakeld op het moment van het maken van het cluster waarop een tabel met Azure Storage waar de gebeurtenissen uit deze kanalen worden verzonden voor u in de toekomst een query maken. 

We raden u aan de EventStore gebruiken voor snelle analyse en idee te krijgen een momentopname van hoe uw cluster wordt uitgevoerd en als dingen als plaatsvinden verwacht. Voor het verzamelen van Logboeken en gebeurtenissen die door het cluster wordt gegenereerd, in het algemeen wordt aangeraden met behulp van de [extensie voor diagnostische gegevens van Azure](service-fabric-diagnostics-event-aggregation-wad.md). Dit kan worden geïntegreerd met Service Fabric Analytics, OMS Log Analytics Service Fabric specifieke oplossing die een aangepast dashboard biedt voor het bewaken van Service Fabric-clusters en kunt u query uitvoeren op uw cluster gebeurtenissen en waarschuwingen instellen. Meer informatie over deze [analyse van gebeurtenis met OMS](service-fabric-diagnostics-event-analysis-oms.md). 

 Meer informatie over het bewaken van uw cluster op [Platform niveau gebeurtenis- en logboekbestanden generatie](service-fabric-diagnostics-event-generation-infra.md).


 ![OMS SF-oplossing](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

## <a name="performance-monitoring"></a>Prestatiebewaking
Bewaking van de onderliggende infrastructuur is een belangrijk onderdeel van de informatie over de status van het cluster en de bronnen beter worden benut. Meten van de systeemprestaties, is afhankelijk van verschillende factoren, die elk gewoonlijk via een Key Performance Indicators (KPI's gemeten worden). Service Fabric die relevante KPI's kunnen worden toegewezen aan de metrische gegevens die kunnen worden verzameld van de knooppunten in het cluster als prestatiemeteritems.
Deze KPI's kunnen helpen bij:
* Understanding brongebruik en load - omwille van de schaal van uw cluster of voor het optimaliseren van uw service wordt verwerkt.
* Infrastructuurfouten - voorspellen veel problemen worden voorafgegaan door plotselinge veranderingen (neerzetten) van de prestaties, zodat u KPI's zoals netwerk-i/o en CPU-gebruik gebruiken kunt voorspellen en infrastructurele problemen onderzoeken.

Een lijst met prestatiemeteritems die moeten worden verzameld op het niveau van de infrastructuur kan worden gevonden op [maatstaven voor prestaties](service-fabric-diagnostics-event-generation-perf.md). 

Service Fabric biedt een set prestatiemeteritems voor de programmeermodellen Reliable Services en actoren. Als u een van deze modellen worden gebruikt, krijgt deze prestatiemeteritems u KPI's waarmee u ervoor zorgen dat uw actoren zijn draait omhoog en omlaag correct, of dat uw betrouwbare serviceaanvragen snel genoeg worden verwerkt. Zie voor meer informatie [bewaking voor betrouwbare Service voor externe toegang](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) en [Prestatiebewaking voor Reliable Actors](service-fabric-reliable-actors-diagnostics.md#performance-counters). Hiernaast Application Insights beschikt ook over een reeks maatstaven voor prestaties die het wordt verzameld, indien geconfigureerd met uw toepassing.

Gebruik de [OMS-agent](service-fabric-diagnostics-oms-agent.md) voor het verzamelen van de relevante prestatiemeteritems en weergeven van deze KPI's in de OMS-logboekanalyse.

![Diagnostische gegevens overzicht grafiek](media/service-fabric-diagnostics-overview/diagnostics-overview.png)

## <a name="health-monitoring"></a>Statuscontrole
Het Service Fabric-platform omvat een statusmodel, wat zorgt voor extensible de statusrapportage voor de status van de entiteiten in een cluster. Elk knooppunt, toepassing, service, partitie, replica of -exemplaar heeft een voortdurend bij te werken gezondheidsstatus. De status kan zijn 'OK', 'Waarschuwing' of 'Fout'. De status wordt gewijzigd via de health-rapporten die worden gegenereerd voor elke entiteit, op basis van de problemen in het cluster. De status van de entiteiten kan worden gecontroleerd op elk gewenst moment in Service Fabric Explorer (SFX), zoals hieronder wordt weergegeven, of via de platforms Health API kan worden opgevraagd. U kunt ook statusrapporten aanpassen en wijzigen van de status van een entiteit met het toevoegen van uw eigen statusrapporten of met behulp van de Health-API. Meer informatie over het statusmodel kunnen worden gevonden op [Inleiding tot Service Fabric-statuscontrole](service-fabric-health-introduction.md).

![SFX health dashboard](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)

Naast de meest recente statusrapporten in SFX ziet, is elk rapport ook beschikbaar als een gebeurtenis. Health-gebeurtenissen kunnen worden verzameld via het operationele kanaal (Zie [gebeurtenis aggregatie met Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)), en opgeslagen in Log Analytics voor waarschuwingen en gegevensquery's in de toekomst. Dit helpt u problemen die mogelijk van invloed op de beschikbaarheid van uw toepassing, zodat het is raadzaam dat u waarschuwingen voor scenario's juiste fouten (aangepaste waarschuwingen via Log Analytics instellen) detecteren.

## <a name="other-logging-solutions"></a>Andere oplossingen voor logboekregistratie

Hoewel de twee oplossingen wordt afgeraden, [OMS](service-fabric-diagnostics-event-analysis-oms.md) en [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) hebt gebouwd in integratie met Service Fabric veel gebeurtenissen via etw-providers worden uitgeschreven en kunnen worden uitgebreid met andere oplossingen voor logboekregistratie. U moet ook zoeken in de [elastische Stack](https://www.elastic.co/products) (met name als u een cluster uitvoert in een offline omgeving overweegt), [Splunk](https://www.splunk.com/), [Dynatrace](https://www.dynatrace.com/), of andere platform van uw voorkeur. 

De belangrijkste punten opgeven die u voor elk platform dat u kiest moeten zijn opgenomen hoe vertrouwd u zich met de gebruikersinterface en query-opties, kunnen gegevens visualiseren en gemakkelijk leesbare dashboards en de aanvullende hulpprogramma's die ze bieden voor het verbeteren van de bewaking maken geautomatiseerd, zoals waarschuwingen.

## <a name="next-steps"></a>Volgende stappen

* Zie voor aan de slag met het instrumenteren van uw toepassingen, [toepassing niveau gebeurtenis- en logboekbestanden generatie](service-fabric-diagnostics-event-generation-app.md).
* Meer informatie over het controleren van het platform en de Service Fabric bevat voor gebeurtenissen [Platform niveau gebeurtenis- en logboekbestanden generatie](service-fabric-diagnostics-event-generation-infra.md).
* Doorloop de stappen voor het instellen van AI voor uw toepassing met [bewaken en onderzoeken van een toepassing ASP.NET Core in Service Fabric](service-fabric-tutorial-monitoring-aspnet.md).
* Meer informatie over het instellen van OMS logboekanalyse voor het bewaken van containers- [controle en diagnostische gegevens voor Windows Containers in Azure Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md).
* Meer informatie over de algemene controle aanbevelingen voor de Azure-resources - [Best Practices - controle en diagnostische gegevens](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 
