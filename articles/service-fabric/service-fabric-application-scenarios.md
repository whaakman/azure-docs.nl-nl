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
ms.date: 7/02/2017
ms.author: atsenthi
ms.openlocfilehash: c9b2f9ac131e71b7c6b37ed85568adc0c3978dc2
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668234"
---
# <a name="service-fabric-application-scenarios"></a>Scenario's voor service Fabric-toepassing
Azure Service Fabric biedt een betrouwbare en flexibele platform waarmee u kunt schrijven en uitvoeren van veel soorten toepassingen en services. Deze toepassingen en microservices kunnen stateless of stateful, en ze zijn resource-met gelijke taakverdeling voor virtuele machines voor een maximale efficiency. De unieke architectuur van Service Fabric kunt u om uit te voeren bijna realtime gegevensanalyse, in-memory-berekeningen, parallelle transacties en in uw toepassingen voor verwerking van gebeurtenissen. U kunt gemakkelijk schalen uw toepassingen omhoog of omlaag (echt inkomend of uitgaand), afhankelijk van uw veranderende resourcevereisten.

De Service Fabric-platform in Azure is ideaal voor de volgende categorieën van toepassingen:

* **Maximaal beschikbare services**: Service Fabric-services bieden een snelle failover door te maken van meerdere secundaire service-replica's. Als een knooppunt, een proces of een afzonderlijke service uitgeschakeld vanwege hardware of een andere storing wordt, is een van de secundaire replica's gepromoveerd naar een primaire replica met minimale verlies van de service.
* **Schaalbare services**: Afzonderlijke services kunnen worden gepartitioneerd, waardoor de status in het cluster worden uitgebreid. Afzonderlijke services kunnen bovendien worden gemaakt en verwijderd op elk gewenst moment. Services kunnen snel en eenvoudig uitgeschaalde uit een aantal exemplaren op een paar knooppunten tot duizenden instanties op veel knooppunten van, en vervolgens nogmaals ingeschaald, afhankelijk van uw resource nodig heeft. Service Fabric kunt u deze services maken en beheren van de volledige levenscycli hiervan.
* **Berekening van niet-statische gegevens**: Service Fabric kunt u gegevens van de build, i/o en compute-intensieve stateful toepassingen. Service Fabric kunt de plaatsing van de verwerking (berekening) en gegevens in toepassingen. Normaal gesproken wanneer uw toepassing toegang tot gegevens vereist, is de netwerklatentie die is gekoppeld aan een externe gegevens cache of storage-laag. Met stateful Service Fabric-services, is dat latentie geëlimineerd, waardoor meer goed presterende leest en schrijft. Stel bijvoorbeeld dat u hebt een toepassing die wordt uitgevoerd in de buurt van real-time aanbeveling selecties voor klanten met een vereiste retourtijd van minder dan 100 milliseconden. De latentie en prestatiekenmerken van Service Fabric-services (waar de berekening van de selectie van de aanbeveling wordt op dezelfde locatie als de gegevens en regels) biedt een reactie van de gebruikerservaring voor de gebruiker in vergelijking met het model voor standaardimplementatie hebben de benodigde gegevens ophalen uit externe opslag.  
* **Interactieve toepassingen op basis van een sessie**: Service Fabric is handig als uw toepassingen, zoals onlinegames of instant messaging, leesbewerkingen met lage latentie- en schrijfbewerkingen vereisen. Service Fabric kunt u deze interactieve, stateful toepassingen zonder te hoeven maken van een afzonderlijk archief of cache, zoals vereist voor stateless apps bouwen. (Dit verhoogt de latentie en introduceert mogelijk problemen met de consistentiecontrole.).
* **Analyse van tijdreeksgegevens en werkstromen**: De snelle lees- en schrijfbewerkingen van Service Fabric kunnen toepassingen die op betrouwbare wijze gebeurtenissen of gegevensstromen verwerkt moeten. Service Fabric kunt ook toepassingen waarin wordt beschreven verwerkingspijplijnen, waar resultaten betrouwbaar en doorgegeven aan de volgende verwerkingsfase zonder gegevensverlies moeten. Het gaat hierbij om transactionele en financiële systemen waarbij consistentie en berekeningen garanties van gegevens essentieel zijn.
* **Gegevensverzameling, verwerking en IoT**: Omdat Service Fabric grote schaal worden verwerkt en lage latentie tot en met de stateful services heeft, is het ideaal voor gegevensverwerking op miljoenen apparaten waar de gegevens voor het apparaat en de berekening worden geplaatst.
We hebben gezien verschillende klanten die hebben gebouwd met behulp van Service Fabric met inbegrip van IoT-systemen [BMW](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/24/service-fabric-customer-profile-bmw-technology-corporation/), [Schneider Electric](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/05/service-fabric-customer-profile-schneider-electric/) en [systemen net](https://blogs.msdn.microsoft.com/azureservicefabric/2016/06/20/service-fabric-customer-profile-mesh-systems/).

## <a name="application-design-case-studies"></a>Casestudy voor ontwerp toepassing
Een aantal casestudy's wordt weergegeven hoe de Service Fabric wordt gebruikt voor het ontwerpen van toepassingen zijn gepubliceerd op de [Service Fabric-teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/) en de [microservices oplossingen site](https://azure.microsoft.com/solutions/microservice-applications/).

## <a name="design-applications-composed-of-stateless-and-stateful-microservices"></a>Toepassingen die bestaan uit staatloze en stateful microservices ontwerpen
Het bouwen van toepassingen met Azure Cloud Service-werkrollen is een voorbeeld van een staatloze service. Stateful microservices behouden daarentegen hun gezaghebbende status, buiten de aanvraag en de reactie. Dit biedt hoge beschikbaarheid en consistentie van de status via eenvoudige API's die transactionele garanties die worden ondersteund door de replicatie bieden. Service-Fabric stateful services maken hoge beschikbaarheid, – Introduceer voor alle typen toepassingen, niet alleen databases en andere gegevensarchieven. Dit is een natuurlijk vervolg op. Toepassingen hebben al verplaatst puur relationele databases voor hoge beschikbaarheid voor NoSQL-databases te gebruiken. De toepassingen zelf kunnen nu hun 'hot' status en gegevens die worden beheerd in deze voor de prestaties verbeteren zonder dat dit ten koste gaat van betrouwbaarheid, consistentie en beschikbaarheid hebben.

Bij het bouwen van toepassingen die bestaan uit microservices, hebt u meestal een combinatie van staatloze web-apps (ASP.NET, Node.js, enzovoort) aanroepen naar de middelste laag met staatloze en stateful Bedrijfsservices, geïmplementeerd in de dezelfde Service Fabric-cluster met de implementatie van Service Fabric-opdrachten. Elk van deze services is onafhankelijk met betrekking tot de schaal, betrouwbaarheid en resource-gebruik, aanzienlijk sneller verloopt flexibiliteit in ontwikkeling en levenscyclusbeheer.

Stateful microservices vereenvoudigd toepassingen mogelijk te maken, omdat ze de noodzaak voor de extra wachtrijen en -caches die gewoonlijk zijn nodig om de vereisten voor beschikbaarheid en latentie van puur stateless toepassingen. Aangezien stateful services op natuurlijke wijze maximaal beschikbare zijn en lage latentie en dit betekent dat er minder bewegende onderdelen om te beheren in uw toepassing als geheel. De volgende diagrammen ziet u de verschillen tussen het ontwerpen van een toepassing die is staatloos en een stateful. Door te profiteren van de [Reliable Services](service-fabric-reliable-services-introduction.md) en [Reliable Actors](service-fabric-reliable-actors-introduction.md) programmeermodellen, stateful services complexiteit van de toepassing verminderen bij het bereiken van hoge doorvoer en lage latentie.

## <a name="an-application-built-using-stateless-services"></a>Een toepassing die is gebouwd met behulp van stateless services
![Toepassing met behulp van de stateless service][Image1]

## <a name="an-application-built-using-stateful-services"></a>Een toepassing die is gebouwd met behulp van stateful services
![Toepassing met behulp van de stateless service][Image2]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [casestudy's](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/)
* Meer informatie over [patronen en scenario's](service-fabric-patterns-and-scenarios.md)

* Aan de slag met het bouwen van een staatloze en stateful services met de Service Fabric [betrouwbare services](service-fabric-reliable-services-quick-start.md) en [betrouwbare actoren](service-fabric-reliable-actors-get-started.md) programmeermodellen.
* Zie ook de volgende onderwerpen:
  * [Meer informatie over microservices](service-fabric-overview-microservices.md)
  * [Definiëren en beheren van de status van service](service-fabric-concepts-state.md)
  * [Beschikbaarheid van Service Fabric-services](service-fabric-availability-services.md)
  * [Service Fabric-services voor schalen](service-fabric-concepts-scalability.md)
  * [Service Fabric-services partitioneren](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg
