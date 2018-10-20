---
title: Azure Service Fabric-bewaking en diagnostische gegevens overzicht | Microsoft Docs
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
ms.date: 10/18/2018
ms.author: srrengar
ms.openlocfilehash: 5fc2674a145be99fb8867c5cf1b1f65ba860db80
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49457830"
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Bewaking en diagnose voor Azure Service Fabric

Dit artikel bevat een overzicht van controle en diagnostische gegevens voor Azure Service Fabric. Controle en diagnose zijn essentieel voor het ontwikkelen, testen en implementeren van workloads in een cloudomgeving. Bewaking, kunt u bijhouden hoe uw toepassingen worden gebruikt, gebruik van uw resources en de algemene status van uw cluster. U kunt deze informatie gebruiken voor het opsporen en corrigeren van problemen met en voorkomen dat er problemen optreden in de toekomst. 

## <a name="application-monitoring"></a>Toepassingsbewaking
Bewaking van toepassingen wordt bijgehouden hoe functies en onderdelen van uw toepassing worden gebruikt. U wilt bewaken van uw toepassingen om te maken of problemen die van belang zijn worden opgepikt door gebruikers. Bewaking van uw toepassingen kan nuttig zijn in de volgende scenario's:
* Bepalen van de belasting van toepassing en gebruiker verkeer - heb ik nodig voor het schalen van uw services om te voldoen aan de eisen van de gebruiker of het adres van een mogelijke knelpunt in uw toepassing?
* Identificeren van problemen met servicecommunicatie en voor externe toegang in uw cluster
* Essentieel dat u weet wat uw gebruikers met uw toepassing doen-het verzamelen van telemetrie in uw toepassingen kunt ontwikkelen van de handleiding voor toekomstige functies en betere diagnostische gegevens voor app-fouten
* Wat gebeurt er in uw actieve containers bewaken

Service Fabric ondersteunt veel opties voor het instrumenteren van de code van uw toepassing met de juiste traceringen en telemetrie. U wordt aangeraden dat u Application Insights (AI) gebruiken. De AI-integratie met Service Fabric bevat verschillende hulpprogramma's voor Visual Studio en Azure portal, evenals Service Fabric-specifieke metrische gegevens, biedt een uitgebreide logboekregistratie voor out-of-the-box-ervaring. Hoewel veel logboeken worden automatisch gemaakt en die voor u worden verzameld met AI, raden wij u aan meer aangepaste logboekregistratie voor uw toepassingen voor het maken van een rijkere ervaring voor diagnostische gegevens. Zie voor meer informatie over aan de slag met Application Insights met Service Fabric op [gebeurtenisanalyse met Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md).

## <a name="platform-cluster-monitoring"></a>Bewaking van platform (Cluster)
Bewaking van uw Service Fabric-cluster is van essentieel belang om ervoor te zorgen dat het platform en alle workloads worden uitgevoerd zoals bedoeld. Een van de doelstellingen van Service Fabric is om toepassingen te houden tegen hardwarefouten. Dit doel wordt bereikt door middel van het platform systeemservices mogelijkheid voor het detecteren van problemen met infrastructuur en snel failover-workloads naar andere knooppunten in het cluster. Maar in dit geval, wat gebeurt er als de systeemservices zelf problemen? Of als bij een poging om te verplaatsen van een werkbelasting, regels voor de plaatsing van services zijn geschonden? Bewaking van het cluster, kunt u op de hoogte van activiteiten die plaatsvinden in uw cluster, dit helpt bij het vaststellen van problemen en het verhelpen van deze effectief blijven. Er zijn enkele belangrijke dingen die u wilt zoeken naar:
* Is de Service Fabric werkt zoals verwacht, wat betreft de plaatsen van uw toepassingen en netwerktaakverdeling omzeilen of het cluster? 
* Acties van de gebruiker gaat in uw cluster bevestigd en op worden uitgevoerd zoals verwacht? Dit is vooral relevant zijn bij het schalen van een cluster.
* Service Fabric verwerkt uw gegevens en uw service-service-communicatie binnen het cluster correct?

Service Fabric biedt een uitgebreide set met gebeurtenissen uit het vak. Deze [Service Fabric-gebeurtenissen](service-fabric-diagnostics-events.md) is toegankelijk via de APIs EventStore of het operationele kanaal (event kanaal die worden weergegeven door het platform). 
* EventStore - de EventStore (beschikbaar op Windows in versie 6.2 en later, Linux nog steeds wordt uitgevoerd vanaf de laatst bijgewerkt op de in dit artikel), wordt deze gebeurtenissen via een reeks API's (toegankelijk via REST-eindpunten of via de clientbibliotheek). Meer informatie over de EventStore op de [overzicht van EventStore](service-fabric-diagnostics-eventstore.md).
* Service Fabric event kanalen - op Windows Service Fabric-gebeurtenissen zijn beschikbaar vanuit één ETW-provider met een set van relevante `logLevelKeywordFilters` gebruikt om te kiezen tussen de operationele gegevens & Messaging en kanalen - dit is de manier waarop we uitgaande onderscheiden Service Fabric-gebeurtenissen worden gefilterd op die nodig zijn. Op Linux Service Fabric-gebeurtenissen binnenkomen via LTTng en van waar ze kunnen worden gefilterd zo nodig in een opslagtabel, zijn geplaatst. Deze kanalen bevatten samengestelde, gestructureerde gebeurtenissen die kunnen worden gebruikt voor meer informatie over de status van uw cluster. Diagnostische gegevens zijn standaard ingeschakeld op het moment van het maken van het cluster, die het maken van een Azure Storage-tabel waar de gebeurtenissen van deze kanalen kunt u query's uitvoeren in de toekomst worden verzonden. 

We raden u aan de EventStore gebruiken voor snelle analyse en voor een momentopname beter beeld van hoe uw cluster wordt uitgevoerd, en als dingen gebeuren als er verwacht. Voor het verzamelen van Logboeken en gebeurtenissen die worden gegenereerd door het cluster, het algemeen wordt aangeraden met behulp van de [Azure Diagnostics-extensie](service-fabric-diagnostics-event-aggregation-wad.md). Dit kan worden geïntegreerd met Service Fabric-analyse, Log Analytics Service Fabric specifieke oplossing, biedt een aangepast dashboard voor het bewaken van Service Fabric-clusters en kunt u query's uitvoeren van uw cluster gebeurtenissen en waarschuwingen instellen. Meer informatie over deze [gebeurtenisanalyse met Log Analytics](service-fabric-diagnostics-event-analysis-oms.md). 

 U kunt meer lezen over het controleren van uw cluster op [Platform op gebeurtenis- en logboekbestanden genereren](service-fabric-diagnostics-event-generation-infra.md).

## <a name="performance-monitoring"></a>Prestatiebewaking
Bewaking van de onderliggende infrastructuur is een belangrijk onderdeel van de status van het cluster en het gebruik van uw resources begrijpen. Meten van prestaties van het systeem, is afhankelijk van diverse factoren, die elk gewoonlijk via een Key Performance Indicators (KPI's gemeten wordt). Service Fabric die relevante KPI's kunnen worden toegewezen aan de metrische gegevens die kunnen worden verzameld van de knooppunten in uw cluster als prestatiemeteritems.
Deze KPI's kunnen helpen met:
* Inzicht krijgen in Resourcegebruik en belasting - voor het schalen van het cluster, of het optimaliseren van uw service wordt verwerkt.
* Infrastructuurfouten - voorspellen heel wat problemen worden voorafgegaan door plotselinge wijzigingen (val) in de prestaties, zodat u KPI's, zoals netwerk-i/o en CPU-gebruik gebruiken kunt om te voorspellen en infrastructurele problemen vaststellen.

Een lijst met prestatiemeteritems die moeten worden verzameld op het niveau van de infrastructuur kan worden gevonden op [maatstaven voor prestaties](service-fabric-diagnostics-event-generation-perf.md). 

Service Fabric biedt een set prestatiemeteritems voor de programmeermodellen Reliable Services en actoren. Als u een van deze modellen, krijgt deze prestatiemeteritems u KPI's waarmee u ervoor zorgen dat uw actors zijn draaiende omhoog en omlaag correct of dat uw betrouwbare serviceaanvragen snel genoeg worden verwerkt. Zie voor meer informatie, [bewaking voor externe communicatie Reliable Service](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) en [prestatiecontrole voor betrouwbare actoren](service-fabric-reliable-actors-diagnostics.md#performance-counters). Naast deze Application Insights beschikt ook over een reeks maatstaven voor prestaties die het verzamelt, als geconfigureerd met uw toepassing.

Gebruik de [Log Analytics-agent](service-fabric-diagnostics-oms-agent.md) het verzamelen van de juiste prestatiemeteritems en het weergeven van deze KPI's in Azure Log Analytics.

![Grafiek met diagnostische gegevens](media/service-fabric-diagnostics-overview/diagnostics-overview.png)

## <a name="health-monitoring"></a>Statuscontrole
Het Service Fabric-platform bevat een health-model, waarmee u uitbreidbare health rapportage van de status van entiteiten in een cluster. Elk knooppunt, toepassing, service, partitie, replica of instantie heeft de status health continu worden bijgewerkt. De status kan zijn 'OK', "Waarschuwing" of "Error". De status wordt gewijzigd via de statusrapporten die worden gegenereerd voor elke entiteit, op basis van problemen in het cluster. De status van uw entiteiten kan worden gecontroleerd op elk gewenst moment in Service Fabric Explorer (SFX), zoals hieronder wordt weergegeven, of via de Health-API van de platformen kan worden opgevraagd. U kunt ook statusrapporten aanpassen en de status van een entiteit wijzigen door het toevoegen van uw eigen statusrapporten of met behulp van de Health-API. Meer informatie over het status-model kunnen worden gevonden op [Inleiding tot Service Fabric-statuscontrole](service-fabric-health-introduction.md).

![SFX health dashboard](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)

Naast het meest recente statusrapporten in SFX ervaart, is elk rapport ook beschikbaar als een gebeurtenis. Statusgebeurtenissen kunnen worden verzameld via het operationele kanaal (Zie [gebeurtenis aggregatie met Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)), en die zijn opgeslagen in Log Analytics voor waarschuwingen en query's uitvoeren in de toekomst. Dit helpt met het detecteren van problemen die mogelijk van invloed zijn op de beschikbaarheid van uw toepassing, raden wij aan dat u waarschuwingen voor de juiste foutscenario's (aangepaste waarschuwingen via Log Analytics) instellen.

## <a name="other-logging-solutions"></a>Andere oplossingen voor logboekregistratie

Hoewel de twee oplossingen die we aanbevolen, [Azure Log Analytics](service-fabric-diagnostics-event-analysis-oms.md) en [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) hebt gebouwd in integratie met Service Fabric, veel gebeurtenissen worden geschreven via ETW-providers en zijn uit te breiden met andere oplossingen voor logboekregistratie. U moet ook zoeken in de [Elastic Stack](https://www.elastic.co/products) (met name als u een cluster uitvoert in een offline-omgeving overweegt), [Dynatrace](https://www.dynatrace.com/), of elk ander platform van uw voorkeur. We hebben een lijst met geïntegreerde partners beschikbaar [hier](service-fabric-diagnostics-partners.md).

De belangrijkste punten voor elk platform dat u kiest moeten zijn opgenomen hoe vertrouwd bent met de gebruikersinterface en opvragen van opties voor de mogelijkheid om te visualiseren en makkelijk leesbare dashboards en de extra hulpprogramma's die ze bieden ter verbetering van de bewaking, maken zoals automatische waarschuwingen.

## <a name="next-steps"></a>Volgende stappen

* Zie voor het aan de slag met het instrumenteren van uw toepassingen, [toepassing niveau gebeurtenis- en logboekbestanden genereren](service-fabric-diagnostics-event-generation-app.md).
* Meer informatie over het controleren van het platform en de Service Fabric biedt voor u op gebeurtenissen [Platform op gebeurtenis- en logboekbestanden genereren](service-fabric-diagnostics-event-generation-infra.md).
* Doorloop de stappen voor het instellen van AI voor uw toepassing met [bewaken en diagnosticeren van een ASP.NET Core-toepassing in Service Fabric](service-fabric-tutorial-monitoring-aspnet.md).
* Meer informatie over het instellen van OMS Log Analytics voor het bewaken van containers- [bewaking en diagnostische gegevens voor Windows-Containers in Azure Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md).
* Zie het voorbeeld van de diagnostische gegevens over problemen en oplossingen met Service Fabric in [oplossen van veelvoorkomende scenario's](service-fabric-diagnostics-common-scenarios.md)
* Bekijk andere producten van diagnostische gegevens die kunnen worden geïntegreerd met Service Fabric in [diagnostische Service Fabric-partners](service-fabric-diagnostics-partners.md)
* Meer informatie over algemene aanbevelingen voor bewaking voor Azure-resources - [Best Practices - bewaking en diagnose](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 
