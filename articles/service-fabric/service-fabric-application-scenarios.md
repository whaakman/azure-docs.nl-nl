---
title: Toepassingsscenario's en ontwerpen | Microsoft Docs
description: Overzicht van categorieën van cloud-toepassingen in Service Fabric. Ontwerp van de toepassing die gebruikmaakt van services voor stateful en staatloze besproken.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 3a8ca6ea-b8e9-4bc3-9e20-262437d2528e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/02/2017
ms.author: mfussell
ms.openlocfilehash: c0a9b24704a91d6a6893937b4ee03765fb05f092
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207533"
---
# <a name="service-fabric-application-scenarios"></a>Service Fabric toepassingsscenario 's
Azure Service Fabric biedt een betrouwbare en flexibele platform waarmee u schrijven en uitvoeren van tal van zakelijke toepassingen en services. Deze toepassingen en microservices kan stateless of stateful, en ze resource verdeeld zijn over virtuele machines om te optimaliseren. De unieke architectuur van Service Fabric kunt u om uit te voeren bijna realtime gegevensanalyse, in het geheugen berekening, parallelle transacties en gebeurtenisverwerking in uw toepassingen. U kunt eenvoudig uw toepassingen omhoog of omlaag schalen (echt inkomend of uitgaand), afhankelijk van uw veranderende resourcevereisten.

Het Service Fabric-platform in Azure is ideaal voor de volgende categorieën van toepassingen:

* **Maximaal beschikbare services**: Service Fabric-services bieden snelle failover met meerdere service secundaire replica's maken. Als een knooppunt, een proces of een afzonderlijke service uitgeschakeld vanwege hardware of andere mislukken wordt, wordt een van de secundaire replica's gepromoveerd naar een primaire replica met minimale verlies van de service.
* **Schaalbare services**: afzonderlijke services kunnen worden gepartitioneerd, waardoor de status in het cluster worden uitgebreid. Bovendien kunnen afzonderlijke services worden gemaakt en verwijderd op elk gewenst moment. Services kunnen snel en eenvoudig uitgebreid van enkele exemplaren op een paar knooppunten naar duizenden exemplaren op veel knooppunten, en vervolgens geschaald in, afhankelijk van uw resourcebehoeften. Service Fabric kunt u deze services bouwen en de volledige levenscyclus beheren.
* **Berekening van niet-statische gegevens**: Service Fabric Hiermee schakelt u voor het bouwen van gegevens, i/o, en compute van gegevensintensieve stateful toepassingen. Service Fabric kunnen het onderbrengen van verwerking (berekening) en in toepassingen. Normaal gesproken wanneer uw toepassing toegang tot gegevens vereist, is dit netwerklatentie die zijn gekoppeld aan een externe gegevensbron cache of storage-laag. Met stateful Service Fabric-services, is dat latentie geëlimineerd, waardoor meer zodat leest en schrijft. Stel bijvoorbeeld dat er een toepassing die wordt uitgevoerd in de buurt van realtime aanbeveling selecties voor klanten met een vereiste round trip-tijd van minder dan 100 milliseconden. De latentie en prestatiekenmerken van Service Fabric-services (waarbij de berekening van de aanbeveling selectie worden samengevoegd met de gegevens en regels) biedt een responsief ervaring voor de gebruiker vergeleken met het model standaardimplementatie van met de benodigde gegevens ophalen uit externe opslag.  
* **Interactieve toepassingen op basis van sessies**: Service Fabric is handig als uw toepassingen, zoals online gaming of chatberichten, lage latentie lees- en schrijfbewerkingen zijn vereist. Service Fabric kunt u deze interactieve stateful toepassingen zonder te hoeven maken van een afzonderlijk archief of een cache, zoals is vereist voor stateless apps bouwen. (Dit verhoogt de latentie en mogelijk introduceert consistentieproblemen.).
* **Gegevensanalyse en werkstromen**: de snelle lees- en schrijfbewerkingen van Service Fabric kunnen toepassingen betrouwbaar gebeurtenissen of gegevensstromen moeten verwerken. Service Fabric kunnen ook toepassingen die verwerking pijplijnen beschrijven, waarbij de resultaten betrouwbaar en doorgegeven aan de volgende verwerkingsfase zonder gegevensverlies zijn moeten. Het gaat hierbij om transactionele en financiële systemen, waarbij gegevens consistentie en berekeningen garanties essentieel zijn.
* **Gegevens die zijn verzameld, verwerkt en IoT**: omdat Service Fabric grote schaal worden verwerkt en lage latentie via de stateful services heeft, is het ideaal voor gegevensverwerking op miljoenen apparaten waar de gegevens voor het apparaat en de berekening worden geplaatst.
Wij hebben verschillende klanten die IoT systemen, met inbegrip van Service Fabric hebt gebouwd [BMW](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/24/service-fabric-customer-profile-bmw-technology-corporation/), [Schneider elektrische](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/05/service-fabric-customer-profile-schneider-electric/) en [systemen net](https://blogs.msdn.microsoft.com/azureservicefabric/2016/06/20/service-fabric-customer-profile-mesh-systems/).

## <a name="application-design-case-studies"></a>Casestudy voor ontwerp toepassing
Een aantal casestudy's die laat zien hoe de Service Fabric wordt gebruikt voor toepassingen ontwerpen worden gepubliceerd op de [Service Fabric-teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/) en de [microservices oplossingen site](https://azure.microsoft.com/solutions/microservice-applications/).

## <a name="design-applications-composed-of-stateless-and-stateful-microservices"></a>Toepassingen bestaan uit staatloze en stateful microservices ontwerpen
Toepassingen maken met Azure Cloud Service-werkrollen is een voorbeeld van een staatloze service. Daarentegen behouden stateful microservices hun gezaghebbende status afgezien van de aanvraag en het antwoord. Dit biedt hoge beschikbaarheid en consistentie van de status via eenvoudige API's die transactionele garanties die worden ondersteund door replicatie bieden. Service-Fabric stateful services democratize hoge beschikbaarheid, dat de toepassing voor alle typen toepassingen, niet alleen databases en andere gegevensarchieven. Dit is een natuurlijke voortgang. Toepassingen hebt al verplaatst puur relationele databases voor hoge beschikbaarheid met NoSQL-databases te gebruiken. De toepassingen kunnen nu hun 'hot' status en gegevens die worden beheerd in deze voor de prestaties verbeteren zonder verlies van betrouwbaarheid, consistentiecontrole of beschikbaarheid hebben.

Tijdens het bouwen van toepassingen die bestaan uit microservices, hebt u doorgaans een combinatie van staatloze web-apps (ASP.NET, Node.js, enz.) op staatloze en stateful Bedrijfsservices van middelste laag aanroept, geïmplementeerd in dezelfde Service Fabric-cluster met behulp van de opdrachten voor het implementeren van Service Fabric. Elk van deze services is niet afhankelijk met betrekking tot de schaal, betrouwbaarheid en resource-gebruik aanzienlijk verbeterd flexibiliteit in ontwikkeling levenscyclusbeheer.

Stateful microservices vereenvoudigen toepassing ontwerpen, omdat ze de noodzaak van de aanvullende wachtrijen en caches die traditioneel is nodig om de beschikbaarheid en latentie vereisten van puur staatloze toepassingen verwijderen. Aangezien stateful services natuurlijk maximaal beschikbaar zijn en lage latentie en dit betekent dat er minder bewegende onderdelen beheren in uw toepassing als geheel. De diagrammen hieronder ziet u de verschillen tussen het ontwerpen van een toepassing die staatloze en een stateful. Door gebruik te maken van de [Reliable Services](service-fabric-reliable-services-introduction.md) en [Reliable Actors](service-fabric-reliable-actors-introduction.md) programming modellen, stateful services complexiteit te verminderen toepassing bij het bereiken van maximale doorvoer en lage latentie.

## <a name="an-application-built-using-stateless-services"></a>Een toepassing die is gebouwd met behulp van stateless services
![Toepassing met behulp van staatloze service][Image1]

## <a name="an-application-built-using-stateful-services"></a>Een toepassing die is gebouwd met behulp van stateful services
![Toepassing met behulp van staatloze service][Image2]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

* Luisteren naar [casestudy's van klanten](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=qDJnf86yC_5206218965
)
* Meer informatie over [casestudy's van klanten](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/)
* Meer informatie over [patronen en scenario's](service-fabric-patterns-and-scenarios.md)

* Aan de slag ontwikkelen staatloze en stateful services met de Service Fabric [betrouwbare services](service-fabric-reliable-services-quick-start.md) en [betrouwbare actoren](service-fabric-reliable-actors-get-started.md) programming modellen.
* Zie ook de volgende onderwerpen:
  * [Meer informatie over microservices](service-fabric-overview-microservices.md)
  * [Definiëren en beheren van de status van service](service-fabric-concepts-state.md)
  * [Beschikbaarheid van Service Fabric-services](service-fabric-availability-services.md)
  * [Schaal Service Fabric-services](service-fabric-concepts-scalability.md)
  * [Partitie Service Fabric-services](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg
