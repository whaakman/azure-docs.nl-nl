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
ms.date: 1/17/2019
ms.author: srrengar
ms.openlocfilehash: 71bff5473abe9f53804d702625b871f41309a787
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57441828"
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Bewaking en diagnose voor Azure Service Fabric

Dit artikel bevat een overzicht van controle en diagnostische gegevens voor Azure Service Fabric. Controle en diagnose zijn essentieel voor het ontwikkelen, testen en implementeren van workloads in een cloudomgeving. Bijvoorbeeld, kunt u bijhouden hoe uw toepassingen worden gebruikt, de acties die door de Service Fabric-platform, uw gebruik van resources met behulp van prestatiemeteritems en de algemene status van uw cluster. U kunt deze informatie gebruiken om te onderzoeken en oplossen van problemen en te voorkomen dat ze zich voordoet in de toekomst. De volgende gedeelten wordt kort uitgelegd voor elk gebied van Service Fabric bewaken om te overwegen voor productieworkloads. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="application-monitoring"></a>Toepassingsbewaking
Bewaking van toepassingen wordt bijgehouden hoe functies en onderdelen van uw toepassing worden gebruikt. U wilt bewaken van uw toepassingen om te maken of problemen die van belang zijn worden opgepikt door gebruikers. De verantwoordelijkheid van de toepassingsbewaking is op de gebruikers die het ontwikkelen van een toepassing en de bijbehorende services omdat deze uniek is voor de bedrijfslogica van uw toepassing. Bewaking van uw toepassingen kan nuttig zijn in de volgende scenario's:
* Hoeveel verkeer ondervindt mijn toepassing? -U moet uw services om te voldoen aan de eisen van de gebruiker of het adres een mogelijke knelpunt in uw toepassing schalen?
* Zijn mijn service-naar-serviceaanroepen geslaagd en bijgehouden?
* Welke acties worden uitgevoerd door de gebruikers van mijn toepassing? -Verzamelen van telemetrische gegevens kan dienen als richtlijn toekomstige functie ontwikkelings- en betere diagnostische gegevens van toepassingsfouten
* Mijn toepassing is die niet-verwerkte uitzonderingen veroorzaakt? 
* Wat gebeurt er in de services die worden uitgevoerd in mijn containers?

Het mooie over het controleren van toepassing is die ontwikkelaars de hulpprogramma's en framework die ze zouden graag omdat deze zich in de context van uw toepassing bevinden kunnen gebruiken. U kunt meer informatie over de Azure-oplossing voor het bewaken met Azure Monitor - Application Insights in [gebeurtenisanalyse met Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md).
We hebben ook een zelfstudie met het [Stel deze optie voor .NET-toepassingen](service-fabric-tutorial-monitoring-aspnet.md). In deze zelfstudie gaat over het installeren van de juiste hulpprogramma's, een voorbeeld van de aangepaste telemetrie schrijven in uw toepassing en de application diagnostics- en telemetrie bekijken in Azure portal. 


## <a name="platform-cluster-monitoring"></a>Bewaking van platform (Cluster)
Een gebruiker is controle over welke telemetrie afkomstig is van de toepassing omdat een gebruiker de code zelf, maar wat over de diagnostische gegevens van de Service Fabric-platform schrijft? Een van de doelstellingen van Service Fabric is om toepassingen te houden tegen hardwarefouten. Dit doel wordt bereikt door middel van het platform systeemservices mogelijkheid voor het detecteren van problemen met infrastructuur en snel failover-workloads naar andere knooppunten in het cluster. Maar in dit geval, wat gebeurt er als de systeemservices zelf problemen? Of als bij een poging om te implementeren of verplaatsen van een werkbelasting, regels voor de plaatsing van services zijn geschonden? Service Fabric biedt diagnostische gegevens voor deze en nog veel meer om te controleren of dat u worden geïnformeerd over de activiteiten die plaatsvinden in uw cluster. Enkele eenvoudige voorbeeldscenario's voor bewaking van het cluster zijn onder andere:

Service Fabric biedt een uitgebreide set met gebeurtenissen uit het vak. Deze [Service Fabric-gebeurtenissen](service-fabric-diagnostics-events.md) is toegankelijk via de EventStore of het operationele kanaal (event kanaal die worden weergegeven door het platform). 

* Service Fabric event kanalen - op Windows Service Fabric-gebeurtenissen zijn beschikbaar vanuit één ETW-provider met een set van relevante `logLevelKeywordFilters` gebruikt om te kiezen tussen de operationele gegevens & Messaging en kanalen - dit is de manier waarop we uitgaande onderscheiden Service Fabric-gebeurtenissen worden gefilterd op die nodig zijn. Op Linux Service Fabric-gebeurtenissen binnenkomen via LTTng en van waar ze kunnen worden gefilterd zo nodig in een opslagtabel, zijn geplaatst. Deze kanalen bevatten samengestelde, gestructureerde gebeurtenissen die kunnen worden gebruikt voor meer informatie over de status van uw cluster. Diagnostische gegevens zijn standaard ingeschakeld op het moment van het maken van het cluster, die het maken van een Azure Storage-tabel waar de gebeurtenissen van deze kanalen kunt u query's uitvoeren in de toekomst worden verzonden. 

* EventStore - de EventStore is een functie die wordt aangeboden door het platform waarmee u Service Fabric-platformgebeurtenissen beschikbaar in de Service Fabric Explorer en via REST-API. Hier ziet u een momentopname van wat in uw cluster voor elke entiteit bijvoorbeeld gebeurt er knooppunt, service, toepassing en query's op basis van de tijd van de gebeurtenis. U kunt ook meer informatie over de EventStore op de [overzicht van EventStore](service-fabric-diagnostics-eventstore.md).    

![EventStore](media/service-fabric-diagnostics-overview/eventstore.png)

De diagnostische gegevens over het opgegeven zijn in de vorm van een uitgebreide set met gebeurtenissen uit het vak. Deze [Service Fabric-gebeurtenissen](service-fabric-diagnostics-events.md) laten zien van acties die worden uitgevoerd door het platform op verschillende entiteiten zoals knooppunten, toepassingen, Services, partities enzovoort. In het bovenstaande laatste scenario als een knooppunt uitvallen, het platform zou verzenden een `NodeDown` gebeurtenis en u het kan onmiddellijk worden gemeld door het hulpprogramma voor bewaking van keuze. Andere algemene voorbeelden zijn `ApplicationUpgradeRollbackStarted` of `PartitionReconfigured` tijdens een failover. **Dezelfde gebeurtenissen zijn beschikbaar op zowel Windows als Linux-clusters.**

De gebeurtenissen worden verzonden via standaard kanalen op zowel Windows als Linux, en kunnen worden gelezen door een controleprogramma die ondersteuning biedt voor deze. De oplossing Azure Monitor is Azure Monitor-Logboeken. U kunt meer informatie over onze [Azure Monitor-integratie van logboeken](service-fabric-diagnostics-event-analysis-oms.md) dat een aangepast dashboard voor het cluster en enkele voorbeeldquery's die u kunt waarschuwingen maken voor een operationele omvat. Meer controle cluster-concepten zijn beschikbaar op [Platform op gebeurtenis- en logboekbestanden genereren](service-fabric-diagnostics-event-generation-infra.md).

### <a name="health-monitoring"></a>Statuscontrole
Het Service Fabric-platform bevat een health-model, waarmee u uitbreidbare health rapportage van de status van entiteiten in een cluster. Elk knooppunt, toepassing, service, partitie, replica of instantie heeft de status health continu worden bijgewerkt. De status kan zijn 'OK', "Waarschuwing" of "Error". Service Fabric-gebeurtenissen beschouwen als termen die worden uitgevoerd door het cluster voor diverse entiteiten en status als geadjectiveerde voor elke entiteit. Telkens wanneer de status van een bepaalde entiteit verandert, wordt een gebeurtenis ook worden verzonden. Deze manier waarop die u query's en waarschuwingen voor statusgebeurtenissen instellen in uw bewaking hulpprogramma naar keuze, net als andere gebeurtenis optreedt kunt. 

Bovendien toestaan we ook dat gebruikers status voor entiteiten negeren. Als uw toepassing is er een upgrade voor aangebracht en hebt u validatietests mislukt, kunt u naar Service Fabric-status schrijven met behulp van de Health-API om aan te geven van uw toepassing in orde is en Service Fabric wordt automatisch terugdraaien van de upgrade. Bekijk voor meer informatie over het status-model, de [Inleiding tot Service Fabric-statuscontrole](service-fabric-health-introduction.md)

![SFX health dashboard](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)


### <a name="watchdogs"></a>Watchdogs
Over het algemeen is een watchdog een afzonderlijke service die u kunt bekijken van status en laden in services, ping-eindpunten en de gezondheid van het rapport voor alles wat in het cluster. Dit kan helpen voorkomen dat er fouten die niet kunnen worden gedetecteerd op basis van de weergave van een service. Watchdogs zijn ook een goede plaats code kunt hosten die corrigerende acties zonder tussenkomst van de gebruiker (bijvoorbeeld het opschonen van de logboekbestanden in de opslag op bepaalde tijdsintervallen) uitvoert. U vindt een voorbeeld watchdog-service-implementatie [hier](https://github.com/Azure-Samples/service-fabric-watchdog-service).

## <a name="infrastructure-performance-monitoring"></a>Infrastructuurbewaking (prestaties)
Nu dat we de diagnostische gegevens in uw toepassing en het platform besproken hebben, hoe weet ik dat de hardware werkt zoals verwacht? Bewaking van de onderliggende infrastructuur is een belangrijk onderdeel van de status van het cluster en het gebruik van uw resources begrijpen. Meten van prestaties van het systeem, hangt af van veel factoren die kunnen subjectief zijn, afhankelijk van uw workloads. Deze factoren zijn gewoonlijk gemeten via prestatiemeteritems. Deze prestatiemeteritems kunnen afkomstig zijn uit verschillende bronnen, met inbegrip van het besturingssysteem, de .NET framework, of de Service Fabric-platform zelf wordt geboden. Sommige scenario's waarin ze handig zijn zijn

* Ben ik mijn hardware efficiënt gebruik? Wilt u het gebruik van uw hardware op 90% CPU- of 10% CPU. Dit is handig wanneer schalen van het cluster, of het optimaliseren van uw toepassing verwerkt.
* Kan ik infrastructuur problemen proactief voorspellen? -heel wat problemen worden voorafgegaan door een plotselinge wijzigingen (val) in de prestaties, zodat u kunt prestatiemeteritems, zoals netwerk-i/o en CPU-gebruik om te voorspellen en problemen proactief.

Een lijst met prestatiemeteritems die moeten worden verzameld op het niveau van de infrastructuur kan worden gevonden op [maatstaven voor prestaties](service-fabric-diagnostics-event-generation-perf.md). 

Service Fabric biedt ook een set prestatiemeteritems voor de programmeermodellen Reliable Services en actoren. Als u een van deze modellen worden gebruikt, kunnen deze prestatiemeteritems gegevens om ervoor te zorgen dat uw actors zijn draaiende omhoog en omlaag correct, of dat uw betrouwbare serviceaanvragen snel genoeg worden verwerkt. Zie voor meer informatie, [bewaking voor externe communicatie Reliable Service](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) en [prestatiecontrole voor betrouwbare actoren](service-fabric-reliable-actors-diagnostics.md#performance-counters). 

De oplossing Azure Monitor voor het verzamelen van deze is Logboeken van Azure Monitor net als platform niveau bewaking. Moet u de [Log Analytics-agent](service-fabric-diagnostics-oms-agent.md) naar de juiste prestatiemeteritems verzamelen en ze weergeven in Azure Monitor-Logboeken.

## <a name="recommended-setup"></a>Aanbevolen instellingen
Nu dat we elk gebied van controle en voorbeeld van de scenario's hebt overschreden, als volgt een samenvatting van de Azure-hulpprogramma's voor controle en het instellen van nodig voor het bewaken van alle bovenstaande gebieden. 

* Toepassingsbewaking met [Application Insights](service-fabric-tutorial-monitoring-aspnet.md)
* Bewaking van het cluster met [Diagnostics-Agent](service-fabric-diagnostics-event-aggregation-wad.md) en [Azure Monitor-Logboeken](service-fabric-diagnostics-oms-setup.md)
* Infrastructuurbewaking met [Azure Monitor-Logboeken](service-fabric-diagnostics-oms-agent.md)

U kunt ook gebruiken en wijzigen van de ARM-voorbeeldsjabloon zich [hier](service-fabric-diagnostics-oms-setup.md#deploy-azure-monitor-logs-with-azure-resource-manager) om de implementatie van alle benodigde resources en de agents te automatiseren. 

## <a name="other-logging-solutions"></a>Andere oplossingen voor logboekregistratie

Hoewel de twee oplossingen die we aanbevolen, [logboeken van Azure Monitor](service-fabric-diagnostics-event-analysis-oms.md) en [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) hebt gebouwd in integratie met Service Fabric, veel gebeurtenissen worden geschreven via ETW-providers en zijn uit te breiden met andere oplossingen voor logboekregistratie. U moet ook zoeken in de [Elastic Stack](https://www.elastic.co/products) (met name als u een cluster uitvoert in een offline-omgeving overweegt), [Dynatrace](https://www.dynatrace.com/), of elk ander platform van uw voorkeur. We hebben een lijst met geïntegreerde partners beschikbaar [hier](service-fabric-diagnostics-partners.md).

De belangrijkste punten voor elk platform dat u kiest moeten opnemen met het scenario dat u met de gebruikersinterface, de mogelijkheden van een query uitvoeren op de aangepaste visualisaties en dashboards die beschikbaar zijn en de extra hulpprogramma's bieden als u wilt uw controle-ervaring te verbeteren. 

## <a name="next-steps"></a>Volgende stappen

* Zie voor het aan de slag met het instrumenteren van uw toepassingen, [toepassing niveau gebeurtenis- en logboekbestanden genereren](service-fabric-diagnostics-event-generation-app.md).
* Doorloop de stappen voor het instellen van Application Insights voor uw toepassing met [bewaken en diagnosticeren van een ASP.NET Core-toepassing in Service Fabric](service-fabric-tutorial-monitoring-aspnet.md).
* Meer informatie over het controleren van het platform en de Service Fabric biedt voor u op gebeurtenissen [Platform op gebeurtenis- en logboekbestanden genereren](service-fabric-diagnostics-event-generation-infra.md).
* De integratie van Azure Monitor configureren met Service Fabric op [Azure Monitor-logboeken voor een cluster instellen](service-fabric-diagnostics-oms-setup.md)
* Meer informatie over het instellen van Azure Monitor-logboeken voor het bewaken van containers- [bewaking en diagnostische gegevens voor Windows-Containers in Azure Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md).
* Zie het voorbeeld van de diagnostische gegevens over problemen en oplossingen met Service Fabric in [oplossen van veelvoorkomende scenario's](service-fabric-diagnostics-common-scenarios.md)
* Bekijk andere producten van diagnostische gegevens die kunnen worden geïntegreerd met Service Fabric in [diagnostische Service Fabric-partners](service-fabric-diagnostics-partners.md)
* Meer informatie over algemene aanbevelingen voor bewaking voor Azure-resources - [Best Practices - bewaking en diagnose](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 