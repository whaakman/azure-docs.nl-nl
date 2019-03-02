---
title: Azure Service Fabric-Platform niveau bewaking | Microsoft Docs
description: Meer informatie over het platform op gebeurtenissen en logboeken die worden gebruikt om te controleren en een diagnose van Azure Service Fabric-clusters.
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
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 90ec06b01b11b5cbe119f41483eaf794af4e991b
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57243088"
---
# <a name="monitoring-the-cluster"></a>Bewaking van het cluster

Het is belangrijk om te controleren op het niveau van het cluster om te bepalen of uw hardware en het cluster gedragen zich zoals verwacht. Hoewel Service Fabric kunnen toepassingen die worden uitgevoerd tijdens een hardware-uitval houden, maar u toch wilt vaststellen of een fout optreedt in een toepassing of in de onderliggende infrastructuur. Ook moet u controleren uw clusters voor het beter plannen van capaciteit, waardoor in beslissingen te nemen over het toevoegen of verwijderen van hardware.

Service Fabric beschrijft verschillende gestructureerde platform gebeurtenissen, als [Service Fabric-gebeurtenissen](service-fabric-diagnostics-events.md), via de EventStore en verschillende log kanalen out-of-the-box. 

Op Windows, Service Fabric-gebeurtenissen zijn beschikbaar vanuit één ETW-provider met een set van relevante `logLevelKeywordFilters` gebruikt om te kiezen tussen de operationele gegevens & Messaging en kanalen - dit is de manier waarop we afzonderlijke uitgaande Service Fabric-gebeurtenissen worden gefilterd op die nodig zijn.

* **Operationele** op hoog niveau bewerkingen die worden uitgevoerd door de Service Fabric en het cluster, met inbegrip van gebeurtenissen voor een knooppunt te komen, een nieuwe toepassing wordt geïmplementeerd of een upgrade terugdraaien, enzovoort. Zie de volledige lijst van gebeurtenissen [hier](service-fabric-diagnostics-event-generation-operational.md).  

* **Operationele - gedetailleerde**  
Beslissingen voor taakverdeling en statusrapporten.

Het kanaal opnieuw kan worden benaderd met diverse manieren ETW/Windows-gebeurtenislogboeken, inclusief de [EventStore](service-fabric-diagnostics-eventstore.md) (beschikbaar op Windows in versies 6.2 en later voor een Windows-clusters). De EventStore hebt u toegang tot de gebeurtenissen van uw cluster op basis van per entiteit (entiteiten inclusief cluster, knooppunten, toepassingen, services, partities, replica's en containers) en wordt aangegeven dat ze via REST-API's en de Service Fabric-clientbibliotheek. De EventStore gebruiken voor het bewaken van uw dev/test-clusters, en voor het ophalen van een point-in-time-begrip van de status van uw productieclusters.

* **Gegevens & berichten**  
Kritieke logboeken en gebeurtenissen die worden gegenereerd in de berichten (momenteel alleen de ReverseProxy) en het gegevenspad (modellen van betrouwbare services).

* **& Messaging - gedetailleerde gegevens**  
Uitgebreide kanaal met alle niet-kritieke logboeken van gegevens en berichten in het cluster (dit kanaal heeft een zeer groot aantal gebeurtenissen).

Naast deze zijn er twee gestructureerde EventSource kanalen opgegeven, evenals logboeken die we verzamelen voor ondersteuning.

* [Reliable Services-gebeurtenissen](service-fabric-reliable-services-diagnostics.md)  
Programming model specifieke gebeurtenissen.

* [Reliable Actors-gebeurtenissen](service-fabric-reliable-actors-diagnostics.md)  
Programming model specifieke gebeurtenissen en prestatiemeteritems.

* Ondersteuning voor logboeken  
Systeemlogboeken die worden gegenereerd door de Service Fabric alleen moet worden gebruikt door ons wanneer er ondersteuning wordt geboden.

Deze verschillende kanalen beslaat van de logboekregistratie voor platform op die wordt aanbevolen. Voor een betere logboekregistratie op de platform, houd rekening met investeren in betere informatie over het status-model en aangepaste statusrapporten toevoegen en toevoegen van aangepaste **prestatiemeteritems** aan het bouwen van een realtime inzicht in de impact van uw Services en toepassingen op het cluster.

Om te profiteren van deze logboeken, is het raadzaam om te laten 'Diagnostische gegevens' ingeschakeld tijdens het maken van een cluster in Azure Portal. Door het inschakelen van diagnostische gegevens, wanneer het cluster wordt geïmplementeerd, Windows Azure Diagnostics kan erkent de operationele, Reliable Services en Reliable actors-kanalen en de gegevens worden opgeslagen als verder uiteengezet in [gebeurtenissen samenstellen met Azure Diagnostische gegevens over](service-fabric-diagnostics-event-aggregation-wad.md).

## <a name="azure-service-fabric-health-and-load-reporting"></a>Azure Service Fabric-status en werkbelastingsrapportage

Service Fabric heeft een eigen statusmodel, die wordt beschreven in deze artikelen:

- [Inleiding tot Service Fabric-statuscontrole](service-fabric-health-introduction.md)
- [Servicestatus rapporteren en controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Aangepaste statusrapporten van Service Fabric toevoegen](service-fabric-report-health.md)
- [Service Fabric-statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)

Statuscontrole is essentieel dat meerdere aspecten van het besturingssysteem van een service, met name tijdens een upgrade van de toepassing. Na de upgrade van elk upgradedomein van de service, moet het upgradedomein statuscontroles slagen voordat de implementatie wordt verplaatst naar het volgende upgradedomein. Als OK integriteitsstatus niet kan worden bereikt, is de implementatie teruggedraaid, zodat de toepassing een bekende status OK hebben blijft. Hoewel sommige klanten gevolgen van ondervinden mogelijk voordat u de services worden teruggedraaid, wordt niet de meeste klanten een probleem ondervindt. Een oplossing doet zich ook relatief snel zonder te wachten op actie van een menselijke operator komt kijken. De meer statuscontroles die zijn opgenomen in uw code, het beter bestand is tegen die problemen bij de implementatie uw service is.

Een ander aspect van de status van de service is metrische gegevens van de service reporting. Metrische gegevens zijn belangrijk in Service Fabric, omdat ze worden gebruikt voor het gebruik van bronnen in balans brengen. Metrische gegevens kan ook worden voor een indicatie van de systeemstatus. Bijvoorbeeld, mogelijk hebt u een toepassing die veel services heeft en elk exemplaar een aanvragen per seconde (RPS) metriek rapporten. Als een service van andere bronnen dan een andere service gebruikmaakt, wordt in Service Fabric service-exemplaren in het cluster, om te proberen te onderhouden, zelfs Resourcegebruik verplaatst. Zie voor een meer gedetailleerde uitleg van de werking van Resourcegebruik [resourceverbruik beheren en laden in Service Fabric met metrische gegevens over](service-fabric-cluster-resource-manager-metrics.md).

Metrische gegevens ook kunt u inzicht geven in hoe uw service wordt uitgevoerd. Na verloop van tijd kunt u metrische gegevens om te controleren dat de service wordt uitgevoerd binnen de verwachte parameters. Bijvoorbeeld, als trends weergeven dat de gemiddelde RPS om 9 uur op maandagochtend 1000 is, klikt u vervolgens u mogelijk instellen een health-rapport dat u wordt gewaarschuwd als de RPS minder dan 500 of hoger 1500 is. Alles is mogelijk geen bezwaar, maar het kan zijn waard om ervoor te zorgen dat uw klanten een fantastische ervaring hebt. Uw service kunt een set metrische gegevens die worden gerapporteerd voor de doeleinden van selectievakje, maar die niet van invloed op de bron-balancing van het cluster definiëren. U doet dit door het gewicht van de metrische te ingesteld op nul. U wordt aangeraden dat u alle metrische gegevens met een gewicht van nul beginnen en het gewicht niet verhogen tot u zeker weet dat u hoe een weging van de metrische gegevens is van invloed op bron begrijpt-balancing voor uw cluster.

> [!TIP]
> Gebruik niet te veel gewogen metrische gegevens. Het kan lastig zijn om te begrijpen waarom service-exemplaren worden verplaatst om voor taakverdeling. Een aantal metrische gegevens kan worden achterhaald!

Alle informatie die op de status en prestaties van uw toepassing duiden kan is een kandidaat voor metrische gegevens en statusrapporten. **Een prestatiemeteritem CPU kan aangeven hoe het knooppunt wordt gebruikt, maar deze niet u vertellen of een bepaalde service is in orde is, omdat er meerdere services kunnen worden uitgevoerd op een enkel knooppunt.** Metrische gegevens zoals RPS, items verwerkt, maar alle latentie van aanvraag kan duiden op de status van een specifieke service.

## <a name="service-fabric-support-logs"></a>Logboeken van de service Fabric-ondersteuning

Als u contact opnemen met Microsoft ondersteuning voor hulp bij uw Azure Service Fabric-cluster moet, zijn bijna altijd Ondersteuningslogboeken vereist. Als uw cluster wordt gehost in Azure, worden logboeken automatisch geconfigureerd en die worden verzameld als onderdeel van het maken van een cluster. De logboeken worden opgeslagen in een speciaal opslagaccount in de resourcegroep van uw cluster. De storage-account beschikt niet over een vaste naam, maar in het account, ziet u de blob-containers en tabellen met namen die met beginnen *fabric*. Zie voor meer informatie over het instellen van logboekgegevens voor een zelfstandige cluster [maken en beheren van een zelfstandig Azure Service Fabric-cluster](service-fabric-cluster-creation-for-windows-server.md) en [configuratie-instellingen voor een zelfstandige Windows cluster](service-fabric-cluster-manifest.md). Voor zelfstandige Service Fabric-instanties, moeten de logboeken worden verzonden naar een lokaal bestand-share. U bent **vereist** dat deze logboeken voor ondersteuning, maar ze zijn niet bedoeld om te worden gebruikt door iemand buiten het team van Microsoft customer support.

## <a name="measuring-performance"></a>Meten van prestaties

Prestaties van de meting van het cluster krijgt u inzicht in hoe belangrijk het is laden en station beslissingen over het schalen van uw cluster overweg kan met (Zie voor meer informatie over het schalen van een cluster [op Azure](service-fabric-cluster-scale-up-down.md), of [on-premises](service-fabric-cluster-windows-server-add-remove-nodes.md)). Prestatiegegevens is ook handig zijn in vergelijking met acties die u of uw toepassingen en services kunnen hebt genomen, bij het analyseren van Logboeken in de toekomst. 

Zie voor een lijst met prestatiemeteritems voor het verzamelen van bij het gebruik van Service Fabric, [prestatiemeteritems in Service Fabric](service-fabric-diagnostics-event-generation-perf.md)

Hier volgen twee algemene manieren waarop u van het verzamelen van prestatiegegevens voor uw cluster kunt instellen:

* **Met behulp van een agent**  
Dit is de beste manier van het verzamelen van prestaties van een virtuele machine, aangezien agents hebben meestal een lijst met mogelijke prestaties metrische gegevens die kunnen worden verzameld en het is een relatief eenvoudig proces om te kiezen van de metrische gegevens die u wilt verzamelen of wijzigen. Het lezen over de Azure Monitor biedt Azure Monitor-Logboeken in de Service Fabric [Azure Monitor-integratie van logboeken](service-fabric-diagnostics-event-analysis-oms.md) en [instellen van de Log Analytics-agent](../log-analytics/log-analytics-windows-agent.md) voor meer informatie over de Log Analytics-agent die is een dergelijke monitoring agent die kan verzamelen van prestatiegegevens voor cluster-VM's en containers geïmplementeerd.

* **Prestatiemeteritems met Azure Table Storage**  
U kunt ook metrische gegevens voor prestaties verzenden naar de dezelfde table storage als de gebeurtenissen. Hiervoor moet het wijzigen van de Azure Diagnostics-configuratie om op te halen uit de VM's in uw cluster de juiste prestatiemeteritems en het inschakelen van het docker-statistieken ophalen als u alle containers gaat implementeren. Meer informatie over het configureren van [prestatiemeteritems in WAD](service-fabric-diagnostics-event-aggregation-wad.md) in Service Fabric voor het instellen van het verzamelen van prestatiemeteritems.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over Service-Fabric [Azure Monitor-integratie van logboeken](service-fabric-diagnostics-event-analysis-oms.md) cluster diagnostische gegevens worden verzameld en aangepaste query's en waarschuwingen maken
* Meer informatie over Service-Fabric in gebouwde uiterst nuttig de [EventStore](service-fabric-diagnostics-eventstore.md)
* Doorloop enkele [algemene scenario's voor diagnostische](service-fabric-diagnostics-common-scenarios.md) in Service Fabric
