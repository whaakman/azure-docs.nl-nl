---
title: Toepassingsscenario's en ontwerp | Microsoft Docs
description: Overzicht van de categorieën van cloud-toepassingen in Service Fabric. Ontwerp van een toepassing die gebruikmaakt van stateful en stateless services besproken.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: 3a8ca6ea-b8e9-4bc3-9e20-262437d2528e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 4/24/2019
ms.author: atsenthi
ms.openlocfilehash: f7ad295eb30d257cd195ae02d5a5b1d85de76bda
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65228498"
---
# <a name="service-fabric-application-scenarios"></a>Scenario's voor service Fabric-toepassing
Azure Service Fabric biedt een betrouwbare en flexibele platform waarmee u kunt schrijven en uitvoeren van veel soorten toepassingen en services. Deze toepassingen en microservices kunnen stateless of stateful, en ze zijn resource-met gelijke taakverdeling voor virtuele machines voor een maximale efficiency. De unieke architectuur van Service Fabric kunt u om uit te voeren bijna realtime gegevensanalyse, in-memory-berekeningen, parallelle transacties en in uw toepassingen voor verwerking van gebeurtenissen. U kunt gemakkelijk schalen uw toepassingen omhoog of omlaag (echt inkomend of uitgaand), afhankelijk van uw veranderende resourcevereisten.

Lees voor ontwerprichtlijnen voor het bouwen van toepassingen, [architectuur met microservices op Azure Service Fabric](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) en [aanbevolen procedures voor het toepassingsontwerp van de met behulp van Service Fabric](service-fabric-best-practices-applications.md)

De Service Fabric-platform is ideaal voor de volgende typen toepassingen:

* **Gegevens verzamelen, verwerking en IoT**: Omdat Service Fabric grote schaal worden verwerkt en lage latentie tot en met de stateful services heeft, is het ideaal voor gegevensverwerking op miljoenen apparaten waar de gegevens voor het apparaat en de berekening worden geplaatst.

    Klanten die hebben gebouwd IoT-services met behulp van Service Fabric zijn onder andere [Honeywell](https://customers.microsoft.com/story/honeywell-builds-microservices-based-thermostats-on-azure), [PCL bouw](https://customers.microsoft.com/story/pcl-construction-professional-services-azure), [Crestron](https://customers.microsoft.com/story/crestron-partner-professional-services-azure), [BMW](https://customers.microsoft.com/story/bmw-enables-driver-mobility-via-azure-service-fabric/), [ Schneider Electric](https://customers.microsoft.com/story/schneider-electric-powers-engergy-solutions-on-azure-service-fabric), en [systemen net](https://customers.microsoft.com/story/mesh-systems-lights-up-the-market-with-iot-based-azure-solutions).

* **Games en op sessies gebaseerde interactieve toepassingen**: Service Fabric is ideaal als uw toepassing leesbewerkingen met lage latentie- en schrijfbewerkingen, zoals in onlinegames of instant messaging vereist. Service Fabric kunt u deze interactieve, stateful toepassingen zonder te hoeven maken van een afzonderlijk archief of cache. Ga naar [oplossingen voor Azure-gaming](https://azure.microsoft.com/solutions/gaming/) voor hulp bij het ontwerp [met behulp van Service Fabric in gamingservices](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf)

    Klanten die hebben gebouwd gamingservices zijn onder andere [Next Games](https://customers.microsoft.com/story/next-games-media-telecommunications-azure) en [Digamore](https://customers.microsoft.com/story/digamore-entertainment-scores-with-a-new-gaming-platform-based-on-azure-service-fabric/). Klanten die hebben gebouwd interactieve sessies zijn onder andere [Honeywell met Hololens](https://customers.microsoft.com/story/honeywell-manufacturing-hololens)

* **Analyse van tijdreeksgegevens en verwerking van de werkstroom**: Toepassingen die moeten op betrouwbare wijze gebeurtenissen of gegevensstromen ook voordeel van het geoptimaliseerde lees- en schrijfbewerkingen in Service Fabric verwerkt. Service Fabric ondersteunt bovendien verwerkingspijplijnen van toepassing, waarbij resultaten betrouwbaar en doorgegeven aan de volgende verwerkingsfase zonder verlies moeten. Het gaat hierbij om transactionele en financiële systemen waarbij consistentie en berekeningen garanties van gegevens essentieel zijn.

    Klanten die zakelijke werkstroomservices hebt gebouwd zijn onder andere [Zeiss groep](https://customers.microsoft.com/story/zeiss-group-focuses-on-azure-service-fabric-for-key-integration-platform) en [Digamore](https://customers.microsoft.com/story/digamore-entertainment-scores-with-a-new-gaming-platform-based-on-azure-service-fabric/)

* **Berekeningen op gegevens**: Service Fabric kunt u gegevens berekening intensieve stateful toepassingen te bouwen. Service Fabric kunt de CO-locatie van de verwerking (berekening) en gegevens in toepassingen. Normaal gesproken wanneer uw toepassing toegang tot gegevens vereist, beperkt de netwerklatentie die is gekoppeld aan een cache of storage-laag van externe gegevens het berekenen van de tijd. Met de stateful Service Fabric-services, die latentie wordt geëlimineerd, waardoor meer geoptimaliseerd leest en schrijft. Neem bijvoorbeeld een toepassing die wordt uitgevoerd in de buurt van real-time aanbeveling selecties voor klanten met een vereiste retourtijd van minder dan 100 milliseconden. De latentie en prestatiekenmerken van Service Fabric-services (waarbij de berekening van de selectie van de aanbeveling is geplaatst met de gegevens en regels) biedt een reactie van de gebruikerservaring voor de gebruiker in vergelijking met het model voor standaardimplementatie hebben de benodigde gegevens ophalen uit externe opslag.

    Klanten die services voor berekening hebt gebouwd zijn onder andere [Solidsoft antwoord](https://customers.microsoft.com/story/solidsoft-reply-platform-powers-e-verification-of-pharmaceuticals) en [Infosupport](https://customers.microsoft.com/story/service-fabric-customer-profile-info-support-and-fudura)

* **Maximaal beschikbare services**: Service Fabric biedt snelle failover door te maken van meerdere secundaire service-replica's. Als een knooppunt, een proces of een afzonderlijke service uitgeschakeld vanwege hardware of een andere storing wordt, is een van de secundaire replica's gepromoveerd naar een primaire replica met minimale verlies van de service.

* **Schaalbare services**: Afzonderlijke services kunnen worden gepartitioneerd, waardoor de status in het cluster worden uitgebreid. Afzonderlijke services kunnen bovendien worden gemaakt en verwijderd op elk gewenst moment. Services kunnen snel en eenvoudig worden geschaald van een paar exemplaren op een paar knooppunten tot duizenden instanties op veel knooppunten van, en vervolgens nogmaals ingeschaald, afhankelijk van uw resource nodig heeft. Service Fabric kunt u deze services maken en beheren van de volledige levenscyclus.

## <a name="application-design-case-studies"></a>Casestudy voor ontwerp toepassing
Een aantal casestudy's wordt weergegeven hoe de Service Fabric wordt gebruikt voor het ontwerpen van toepassingen zijn gepubliceerd op [verhalen van klanten](https://customers.microsoft.com/search?sq=%22Azure%20Service%20Fabric%22&ff=&p=0&so=story_publish_date%20desc/) en de [microservices oplossingen site](https://azure.microsoft.com/solutions/microservice-applications/).

## <a name="design-applications-composed-of-stateless-and-stateful-microservices"></a>Toepassingen die bestaan uit staatloze en stateful microservices ontwerpen
Het bouwen van toepassingen met Azure Cloud Service-werkrollen is een voorbeeld van een staatloze service. Stateful microservices behouden daarentegen hun gezaghebbende status, buiten de aanvraag en de reactie. Deze functionaliteit biedt hoge beschikbaarheid en consistentie van de status via eenvoudige API's die transactionele garanties die worden ondersteund door de replicatie bieden. Service-Fabric stateful services maken hoge beschikbaarheid, – Introduceer voor alle typen toepassingen, niet alleen databases en andere gegevensarchieven. Dit is een natuurlijk vervolg op. Toepassingen hebben al verplaatst puur relationele databases voor hoge beschikbaarheid voor NoSQL-databases te gebruiken. De toepassingen zelf kunnen nu hun 'hot' status en gegevens die worden beheerd in deze voor de prestaties verbeteren zonder dat dit ten koste gaat van betrouwbaarheid, consistentie en beschikbaarheid hebben.

Bij het bouwen van toepassingen die bestaan uit microservices, hebt u meestal een combinatie van staatloze web-apps (ASP.NET, Node.js, enzovoort) aanroepen naar de middelste laag met staatloze en stateful Bedrijfsservices, geïmplementeerd in de dezelfde Service Fabric-cluster met de implementatie van Service Fabric-opdrachten. Elk van deze services is niet afhankelijk zijn met betrekking tot de schaal, betrouwbaarheid en Resourcegebruik, wendbaarheid en flexibiliteit in ontwikkeling en het beheer van levenscyclus aanzienlijk wordt verbeterd.

Stateful microservices vereenvoudigd toepassingen mogelijk te maken, omdat ze de noodzaak voor de extra wachtrijen en -caches die gewoonlijk zijn nodig om de vereisten voor beschikbaarheid en latentie van puur stateless toepassingen. Omdat het stateful services op een natuurlijke manier voor hoge beschikbaarheid en lage latentie, zijn er minder bewegende onderdelen om te beheren in uw toepassing als geheel. De volgende diagrammen ziet u de verschillen tussen het ontwerpen van een toepassing die is staatloos en een stateful. Door te profiteren van de [Reliable Services](service-fabric-reliable-services-introduction.md) en [Reliable Actors](service-fabric-reliable-actors-introduction.md) programmeermodellen, stateful services complexiteit van de toepassing verminderen bij het bereiken van hoge doorvoer en lage latentie.

## <a name="an-application-built-using-stateless-services"></a>Een toepassing die is gebouwd met behulp van stateless services
![Toepassing met behulp van de stateless service][Image1]

## <a name="an-application-built-using-stateful-services"></a>Een toepassing die is gebouwd met behulp van stateful services
![Toepassing met behulp van de stateless service][Image2]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [casestudy's](https://customers.microsoft.com/search?sq=%22Azure%20Service%20Fabric%22&ff=&p=0&so=story_publish_date%20desc)
* Meer informatie over [patronen en scenario's](service-fabric-patterns-and-scenarios.md)

* Aan de slag met het bouwen van een staatloze en stateful services met de Service Fabric [betrouwbare services](service-fabric-reliable-services-quick-start.md) en [betrouwbare actoren](service-fabric-reliable-actors-get-started.md) programmeermodellen.
* Ga naar het Azure architecture center voor richtlijnen over [microservices bouwen op Azure](https://docs.microsoft.com/azure/architecture/microservices/)
* Ga naar [aanbevolen procedures voor Azure Service Fabric-toepassing en cluster](service-fabric-best-practices-overview.md) voor begeleiding bij het ontwerp van toepassing.

* Zie ook de volgende onderwerpen:
  * [Meer informatie over microservices](service-fabric-overview-microservices.md)
  * [Definiëren en beheren van de status van service](service-fabric-concepts-state.md)
  * [Beschikbaarheid van Service Fabric-services](service-fabric-availability-services.md)
  * [Service Fabric-services voor schalen](service-fabric-concepts-scalability.md)
  * [Service Fabric-services partitioneren](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg
