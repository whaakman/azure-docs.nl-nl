---
title: Maak kennis met de Service Fabric Cluster Resource Manager | Microsoft Docs
description: Een inleiding tot de Service Fabric Cluster Resource Manager.
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
ms.openlocfilehash: e3cf87ca49ae39966cffbb768dc1c191991d4036
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55096905"
---
# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Maak kennis met de Service Fabric-cluster resource manager
Traditioneel beheer van IT-systemen of online services bedoeld specifieke fysieke of virtuele machines toe die specifieke services of systemen. Services zijn ontworpen als lagen. Er worden een 'web'-laag en een laag "gegevens" of 'opslag'. Toepassingen hoeft een berichtenservice waar aanvragen gestroomd en afmelden, evenals een set van machines speciaal voor opslaan in cache. Elke servicelaag of het type workload specifieke computers die zijn toegewezen aan deze was: de database hebt u een aantal machines die zijn toegewezen, de webservers van een enkele. Als een bepaald type werkbelasting de machines er veroorzaakt op uitvoeren te ' hot ', en vervolgens u meer machines met dezelfde configuratie hebt toegevoegd aan die laag. Echter niet van alle werkbelastingen kunnen zo gemakkelijk worden uitgebreid - met name met de gegevenslaag zou u meestal machines met grotere machines vervangen. Eenvoudig. Als een virtuele machine is mislukt, wordt die deel uitmaken van de algemene toepassing is op lagere capaciteit totdat de computer kan worden hersteld. Nog steeds redelijk eenvoudig is (als deze niet per se leuke).

Nu echter de wereld van service- en software-architectuur is gewijzigd. Het is gebruikelijk dat toepassingen een scale-out-ontwerp hebt vastgesteld. Het bouwen van toepassingen met containers of microservices (of beide) is het gebruikelijk. Terwijl u nog steeds slechts een paar machines hebt, worden ze nu niet slechts één exemplaar van een workload uitgevoerd. Ze kunnen ook worden uitgevoerd meerdere verschillende werkbelastingen op hetzelfde moment. U hebt nu vele verschillende soorten services (geen verbruikt aan resources van een volledige virtuele machine), misschien wel honderden verschillende exemplaren van deze services. Elke benoemd exemplaar heeft een of meer exemplaren of replica's voor hoge beschikbaarheid (HA). Afhankelijk van de grootte van deze werkbelastingen, en hoe bezet zijn, merkt u misschien zelf met honderden of duizenden machines. 

Plotseling beheer van uw omgeving is niet zo eenvoudig als het beheer van enkele machines die zijn toegewezen aan één typen workloads. Uw servers, virtuele en hoeft niet langer namen (u bent overgeschakeld naar hebt mindsets van [huisdieren naar vee](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) nadat alle). Configuratie is minder over de machines en meer informatie over de services zelf. Hardware die is toegewezen aan één exemplaar van een werkbelasting is grotendeels een het verleden. Services zelf geworden kleine gedistribueerde systemen met betrekking meerdere kleinere delen met basishardware tot.

Omdat de app niet langer een reeks monolieten verdeeld over verschillende categorieën is, hebt u nu veel meer combinaties om op te lossen. Die bepaalt welke soorten workloads kunnen uitvoeren op welke hardware, of hoeveel? Welke workloads goed werken op dezelfde hardware, en die conflicteren? U weet wat er op deze machine werd uitgevoerd als een virtuele machine gaat omlaag hoe doe? Wie is verantwoordelijk voor te zorgen dat die werkbelasting weer wordt uitgevoerd? Moet u wachten voor de machine (virtueel?) keert u terug of uw workloads automatisch failover naar een andere machines en de uitvoering? Menselijke tussenkomst vereist is? Hoe zit het met upgrades in deze omgeving?

Als ontwikkelaars en operators omgaan in deze omgeving, gaan we willen hulp bij het beheren van deze complexiteit. Een aannemen van personeel binge en wilt de complexiteit met personen verbergen is waarschijnlijk niet het juiste antwoord, dus wat we doen?

## <a name="introducing-orchestrators"></a>Maak kennis met orchestrators
Een "Orchestrator" is de algemene term voor een stukje software waarmee beheerders deze typen omgevingen beheren. Orchestrators zijn de onderdelen die ondernemen in aanvragen, zoals "Ik wil graag vijf exemplaren van deze service die wordt uitgevoerd in de omgeving." Ze proberen te maken van de omgeving die overeenkomen met de gewenste status, ongeacht wat er gebeurt.

Orchestrators (niet mens) zijn wat kan actie ondernemen wanneer een virtuele machine is mislukt of een werkbelasting om een bepaalde reden onverwacht beëindigd. De meeste orchestrators meer dan alleen omgaan met fouten. Andere functies die ze hebben beheert nieuwe implementaties, upgrades verwerken, en het oplossen van resourceverbruik en beheer. Alle orchestrators zijn fundamenteel over het onderhouden van een gewenste status van de configuratie in de omgeving. U kunt een orchestrator zien wat u wilt en deze het zware werk aan. Aurora boven op Mesos, Docker Datacenter/Docker Swarm, Kubernetes en Service Fabric worden alle voorbeelden van orchestrators. Deze orchestrators zijn actief wordt ontwikkeld om te voldoen aan de behoeften van echte werkbelastingen in een productieomgeving. 

## <a name="orchestration-as-a-service"></a>Indeling als een service
Cluster Resource Manager is het onderdeel van die verantwoordelijk is voor indelen in Service Fabric. De Cluster Resource Manager-taak is onderverdeeld in drie delen:

1. Regels afdwingen
2. Uw omgeving te optimaliseren
3. Hulp bij het met andere processen

Om te zien hoe de Cluster Resource Manager werkt, bekijkt u de volgende video van Microsoft Virtual Academy: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=d4tka66yC_5706218965">
<img src="./media/service-fabric-cluster-resource-manager-introduction/ConceptsAndDemoVid.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="what-it-isnt"></a>Wat is het niet
In traditionele toepassingen met N lagen, er is altijd een [Load Balancer](https://en.wikipedia.org/wiki/Load_balancing_(computing)). Dit is meestal een Network Load Balancer (NLB) of een toepassing Load Balancer (ALB), afhankelijk van waar het zit in de netwerkstack. Sommige load balancers zijn Hardware-systemen, zoals van F5 BigIP aanbieding, andere zijn op basis van software, zoals Microsoft NLB de. In andere omgevingen ziet u mogelijk iets zoals HAProxy, nginx, Istio of Envoy in deze rol. In deze architectuur is de taak van taakverdeling om ervoor te zorgen staatloze werkbelastingen ontvangen (ongeveer) dezelfde hoeveelheid werk. Strategieën voor taakverdeling uiteenlopende laden. Sommige balancers zou elke andere aanroep naar een andere server verzenden. Anderen vastmaken/sessiepersistentie geboden. Meer geavanceerde balancers gebruiken schatting van de werkelijke belasting of rapportage voor het routeren van een aanroep op basis van de verwachte kosten en de huidige belasting van de machine.

Balancers voor het netwerk of bericht routers geprobeerd om ervoor te zorgen dat de laag web/worker bleef ongeveer met gelijke taakverdeling. Strategieën voor taakverdeling van de gegevenslaag zijn verschillende en afhankelijke op als het gegevensopslagmechanisme. Taakverdeling van de gegevenslaag waarop gegevenssharding, caching, beheerde weergaven, opgeslagen procedures en andere mechanismen voor store-specifieke.

Hoewel sommige van deze strategieën interessante zijn, is Service Fabric Cluster Resource Manager niet alles, zoals een network load balancer of een cache. Een Network Load Balancer verdeelt front-ends door verkeer spreiden over front-ends. De Service Fabric Cluster Resource Manager heeft een andere strategie. Fundamentele, Service Fabric verplaatst *services* waar ze zinvol zijn het meest, verkeer wordt verwacht of laden als u wilt volgen. Het kan bijvoorbeeld services verplaatsen naar knooppunten die zich momenteel koude omdat de services die er zijn niet veel werk doen. De knooppunten mogelijk koude omdat de services die aanwezig waren zijn verwijderd of verplaatst. Cluster Resource Manager kan nog een voorbeeld: een service van een virtuele machine ook verplaatsen. De machine is wellicht worden bijgewerkt, of door de services die erop worden uitgevoerd vanwege een piek in het verbruik is overbelast. U kunt ook kunnen de resourcevereisten van de service zijn toegenomen. Als gevolg hiervan er zijn niet voldoende bronnen zijn op deze computer om door te gaan waarop deze wordt uitgevoerd. 

Omdat met Cluster Resource Manager verantwoordelijk is voor het verplaatsen van services rondom, bevat deze een andere functieset in vergelijking met wat u in een network load balancer zoeken wilt. Dit is omdat netwerk load balancers netwerkverkeer leveren waar services al zijn, zelfs als deze locatie niet ideaal is voor het uitvoeren van de service zelf. De Service Fabric Cluster Resource Manager maakt gebruik van fundamenteel verschillende strategieën om ervoor te zorgen dat de resources in het cluster efficiënt worden gebruikt.

## <a name="next-steps"></a>Volgende stappen
- Bekijk voor meer informatie over de architectuur en de informatiestroom binnen het Cluster Resource Manager [in dit artikel ](service-fabric-cluster-resource-manager-architecture.md)
- Cluster Resource Manager beschikt over veel opties voor het beschrijven van het cluster. Meer informatie over metrische gegevens, Bekijk dit artikel op [met een beschrijving van een Service Fabric-cluster](service-fabric-cluster-resource-manager-cluster-description.md)
- Voor meer informatie over het configureren van services, [meer informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)
- Metrische gegevens zijn hoe gebruiks- en capaciteit in het cluster worden beheerd door de Service Fabric-Cluster Resource Manager. Voor meer informatie over metrische gegevens en het configureren van uitchecken [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)
- Cluster Resource Manager werkt met de functionaliteit van Service Fabric. Lees meer informatie over deze integratie, [in dit artikel](service-fabric-cluster-resource-manager-management-integration.md)
- Als u wilt weten over hoe met Cluster Resource Manager beheert en verdeelt de taken in het cluster, Zie het artikel op [taakverdeling](service-fabric-cluster-resource-manager-balancing.md)
