---
title: Inleiding tot de Service Fabric Cluster Resource Manager | Microsoft Docs
description: Een inleiding met de Service Fabric Cluster Resource Manager.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: cfab735b-923d-4246-a2a8-220d4f4e0c64
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: f25a422385abfcdb7020eb7477c0ae2ee55cd8fb
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Inleiding tot de Service Fabric-cluster resourcemanager
Traditioneel beheren van IT-systemen of onlineservices bedoeld specifieke fysieke of virtuele machines dat die specifieke services of systemen. Services zijn ontworpen als lagen. Zou er een laag met 'web' of 'data' of 'opslag'. Toepassingen heeft een messaging-laag waar aanvragen gestroomd en afmelden, evenals een aantal machines toegewezen aan de cache. Elke fase of type werkbelasting specifieke machines toegewezen aan deze had: de database hebt u een aantal machines die zijn toegewezen, de webservers van een enkele. Als een bepaald type werkbelasting de machines was veroorzaakt bij uitvoeren te hot, en u meer machines met dezelfde configuratie toegevoegd aan die laag. Echter niet alle werkbelastingen kunnen heel eenvoudig worden uitgebreid - met name met de gegevenslaag zou u meestal machines met een grotere machines vervangen. Eenvoudig. Als een machine is mislukt, is dat deel van de algemene toepassing werd uitgevoerd op een lagere capaciteit totdat de computer kan worden hersteld. Nog steeds redelijk eenvoudig (indien niet per se leuk).

Nu echter de wereld van service- en software-architectuur is gewijzigd. Het is gebruikelijk dat toepassingen een scale-out-ontwerp hebt vastgesteld. Het is gebruikelijk voor het bouwen van toepassingen met containers of microservices (of beide). Terwijl u nog steeds alleen enkele machines hebt, zijn ze nu niet slechts één exemplaar van een werkbelasting uitgevoerd. Ze kunnen ook worden uitgevoerd meerdere verschillende werkbelastingen op hetzelfde moment. U hebt nu tientallen verschillende soorten services (geen resources aan een volledige machine verbruikt), mogelijk honderden verschillende exemplaren van deze services. Elk benoemd exemplaar heeft een of meer exemplaren of replica's voor hoge beschikbaarheid (HA). Afhankelijk van de grootte van deze werkbelastingen en hoe bezet zijn en zult u zien met honderden of duizenden computers. 

Plotseling beheer van uw omgeving is niet zo eenvoudig als het beheer van enkele machines toegewezen aan één typen werkbelastingen. Uw servers virtueel zijn en niet langer namen hebben (u hebt gekozen mindsets van [huisdieren naar runderen](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) nadat alle). Configuratie is minder over de machines en meer informatie over de services zelf. Hardware die is toegewezen aan één exemplaar van een werkbelasting is grotendeels een nu tot het verleden. Services zelf geworden kleine gedistribueerde systemen die meerdere kleinere delen van basishardware omvatten.

Omdat uw app niet langer een reeks monolieten verdeeld over meerdere lagen is, hebt u nu veel meer combinaties omgaan met. Die bepaalt wat typen werkbelastingen kunnen uitvoeren op welke hardware, of hoeveel? Welke workloads goede werking op dezelfde hardware, en die conflicteren? U weet wat er op deze machine is uitgevoerd wanneer een machine gaat omlaag hoe doen? Wie is verantwoordelijk om ervoor te zorgen dat de werkbelasting wordt gestart opnieuw uit te voeren? Wachten op de computer (virtueel?) keert u terug of uw werkbelastingen automatisch een failover naar de andere machines en uitvoering? Menselijke tussenkomst vereist is? Hoe zit het upgrades in deze omgeving?

Als ontwikkelaars en operators in deze omgeving omgaan, gaan we u hulp nodig bij het beheren van deze complexiteit. Een Aanstellend binge en wilt verbergen, de complexiteit met mensen is waarschijnlijk niet het juiste antwoord, dus wat we doen?

## <a name="introducing-orchestrators"></a>Inleiding tot orchestrators
Een "Orchestrator" is de algemene term voor een stukje software waarmee beheerders kunnen deze soorten omgevingen beheren. Orchestrators zijn de onderdelen die in aanvragen nemen zoals "Ik wil graag vijf exemplaren van deze service wordt uitgevoerd op mijn omgeving." Ze proberen te maken van de omgeving die overeenkomen met de gewenste status, ongeacht wat er gebeurt.

Orchestrators (geen mensen) zijn wat actie ondernemen als een computer uitvalt of een werkbelasting voor een bepaalde reden onverwacht beëindigd. De meeste orchestrators dan alleen te maken met de fout. Andere functies die ze hebben beheert nieuwe implementaties, upgrades verwerking en omgaan met brongebruik en beheeracties. Alle orchestrators zijn fundamenteel over het beheren van sommige gewenste status van de configuratie in de omgeving. U kunt een orchestrator zien wat u wilt en deze doen het zware werk. Aurora boven op Mesos, Docker-Datacenter/Docker Swarm Kubernetes en Service Fabric zijn alle voorbeelden van orchestrators. Deze orchestrators worden actief ontwikkeld om te voldoen aan de behoeften van echte werkbelastingen in productieomgevingen. 

## <a name="orchestration-as-a-service"></a>Orchestration als een service
De Cluster Resource Manager is het onderdeel dat zorgt voor orchestration in Service Fabric. De Cluster Resource Manager-taak is onderverdeeld in drie delen:

1. Regels afdwingen
2. Uw omgeving te optimaliseren
3. Helpt met andere processen

Als u wilt zien hoe de Cluster Resource Manager werkt, moet u de volgende Microsoft Virtual Academy-video bekijken: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=d4tka66yC_5706218965">
<img src="./media/service-fabric-cluster-resource-manager-introduction/ConceptsAndDemoVid.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="what-it-isnt"></a>Het is niet
In traditionele N laag toepassingen is altijd een [Load Balancer](https://en.wikipedia.org/wiki/Load_balancing_(computing)). Dit is meestal een Network Load Balancer (NLB) of een toepassing Load Balancer (ALB), afhankelijk van waar het zat in de netwerkstack. Sommige netwerktaakverdelers hardwarematige zoals van F5 BigIP aanbieding, anderen zijn op basis van software zoals Microsoft NLB de. In andere omgevingen ziet u mogelijk iets zoals HAProxy, nginx, Istio of Envoy in deze rol. In deze architectuur is de taak van taakverdeling om ervoor te zorgen staatloze werkbelastingen ontvangen (ongeveer) dezelfde hoeveelheid werk. Strategieën voor netwerktaakverdeling uiteenlopende laden. Sommige balancers zou elke andere aanroep van een andere server verzenden. Anderen vastmaken/sessiepersistentie opgegeven. Meer geavanceerde balancers gebruik schatting van de daadwerkelijke laadbewerking of rapportage voor het routeren van een aanroep op basis van de verwachte kosten en de huidige machine-werkbelasting.

Netwerk balancers of bericht-routers is geprobeerd om ervoor te zorgen dat de laag web/worker ongeveer taakverdeling gebleven. Strategieën voor de gegevenslaag balancing zijn verschillende en afhankelijke op het mechanisme voor opslag van gegevens. De gegevenslaag Balancing vertrouwen op gegevens sharding, caching, beheerde weergaven, opgeslagen procedures en andere mechanismen voor store-specifieke.

Hoewel sommige van deze strategieën interessante, is Service Fabric Cluster Resource Manager niet iets zoals een network load balancer of een cache. Een Network Load Balancer een compromis tussen de frontends door het verkeer verspreid over frontends. De Service Fabric Cluster Resource Manager heeft een andere strategie. Fundamenteel, Service Fabric verplaatst *services* waar ze zinvol zijn het meest, verkeer verwacht of laden als u wilt uitvoeren. Deze Verplaats bijvoorbeeld services op knooppunten die momenteel koude omdat de services die er zijn veel werk niet doen. De knooppunten mogelijk koude omdat de services die aanwezig waren zijn verwijderd of verplaatst. Als een ander voorbeeld kan de Cluster Resource Manager een service niet op een machine ook verplaatsen. Mogelijk de computer worden bijgewerkt, is of vanwege een piek in de verbruik is overbelast door de services die erop worden uitgevoerd. Alernatively, de resourcevereisten van de service is mogelijk verhoogd. Als gevolg hiervan er zijn niet voldoende bronnen op deze computer om door te gaan uitvoeren. 

Omdat de Cluster Resource Manager verantwoordelijk is voor het verplaatsen van services rond, bevat deze een andere functieset ten opzichte van wat u in een network load balancer vinden wilt. Dit komt doordat de netwerktaakverdelers netwerk netwerkverkeer leveren waar services al zijn, zelfs als deze locatie niet ideaal is voor het uitvoeren van de service zelf. De Service Fabric Cluster Resource Manager veiligheidsmaatregelen fundamenteel verschillende strategieën om ervoor te zorgen dat de bronnen in het cluster efficiënt worden gebruikt.

## <a name="next-steps"></a>Volgende stappen
- Bekijk voor meer informatie over de architectuur en de informatiestroom binnen het Cluster Resource Manager, [in dit artikel ](service-fabric-cluster-resource-manager-architecture.md)
- De Cluster Resource Manager bevat veel opties voor het beschrijven van het cluster. Meer informatie over metrische gegevens, Bekijk dit artikel op [met een beschrijving van een Service Fabric-cluster](service-fabric-cluster-resource-manager-cluster-description.md)
- Voor meer informatie over het configureren van services, [informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Metrische gegevens zijn hoe de Service Fabric-Cluster Resource Manager beheert gebruiks- en capaciteit in het cluster. Voor meer informatie over metrische gegevens en het configureren van uitchecken [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)
- De Cluster Resource Manager werkt met de beheermogelijkheden van Service Fabric. Lees meer informatie over deze integratie, [in dit artikel](service-fabric-cluster-resource-manager-management-integration.md)
- Als u wilt weten over hoe de Cluster Resource Manager beheert en een compromis tussen de werklast van het cluster, Raadpleeg het artikel op [load balancing](service-fabric-cluster-resource-manager-balancing.md)
