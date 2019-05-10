---
title: Azure Service Fabric-toepassing aanbevolen procedures voor ontwerp | Microsoft Docs
description: Aanbevolen procedures voor het ontwikkelen van Service Fabric-toepassingen.
services: service-fabric
documentationcenter: .net
author: markfussell
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/26/2019
ms.author: msfussell
ms.openlocfilehash: 55f043effc7cdb102acea856e89c58f660d0cde5
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65237745"
---
# <a name="azure-service-fabric-application-design-best-practices"></a>Azure Service Fabric-toepassing aanbevolen procedures voor ontwerp

Dit artikel bevat richtlijnen voor best practices voor het bouwen van toepassingen en services in Service Fabric.
 
## <a name="get-familiar-with-service-fabric"></a>Raak vertrouwd met de Service Fabric
* [Zo wilt u meer informatie over Service Fabric?](service-fabric-content-roadmap.md)
* Meer informatie over [scenario's voor Service Fabric-toepassing](service-fabric-application-scenarios.md)
* Vervolgens informatie over de programming model-opties met [Service Fabric-programmeermodellen beveiligingsmodellen](service-fabric-choose-framework.md)



## <a name="application-design-guidance"></a>Ontwerprichtlijnen voor toepassing
Vertrouwd raken met de [algemene architectuur](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) van een Service Fabric-toepassing en de bijbehorende [ontwerpoverwegingen](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#design-considerations).

### <a name="choose-an-api-gateway"></a>Kies een API-gateway
Gebruik een API-gateway-service die communiceert met de back-end-services die vervolgens kunnen worden uitgebreid. De meest voorkomende API-gateway-services die worden gebruikt zijn:

- [Met Azure API Management](https://docs.microsoft.com/azure/service-fabric/service-fabric-api-management-overview), namelijk [geïntegreerd met Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-api-management)
- [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) of [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/) met behulp van de [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor) lezen uit Event Hub-partities
- [Træfik omgekeerde proxy](https://blogs.msdn.microsoft.com/azureservicefabric/2018/04/05/intelligent-routing-on-service-fabric-with-traefik/) met behulp van de [Azure Service Fabric-provider](https://docs.traefik.io/configuration/backends/servicefabric/)
- [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/) Opmerking: dit is niet rechtstreeks geïntegreerd met Service Fabric en Azure API Management is doorgaans een uitstekende keuze
- Bouw uw eigen [ASP.NET Core](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore) web application-gateway

### <a name="choose-stateless-services"></a>Kies stateless services
Het wordt aangeraden dat u altijd begint met het bouwen van stateless services met behulp van [Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction) status opslaan in een Azure-Database, Azure cosmos DB of Azure storage. Status externalized is de meer vertrouwde benadering voor de meeste ontwikkelaars en kunt u ook profiteren van de query-mogelijkheden in de store.  

### <a name="when-to-choose-stateful-services"></a>Bij stateful services kiezen
Stateful services overwegen wanneer u een scenario voor lage latentie en nodig hebt om de gegevens dicht bij de berekening te houden. Enkele voorbeelden zijn een digitale kopie-IoT-apparaten, status van het spel, sessiestatus, caching van gegevens uit een database en langlopende werkstromen om bij te houden van aanroepen naar andere services.

Bepaal het tijdsbestek voor bewaren van gegevens:

- Gegevens in de cache - u opslaan in cache waarbij de latentie voor externe stores een probleem is. Een stateful service gebruiken als uw eigen gegevens in de cache of kunt u overwegen de [open source SoCreate service-fabric-gedistribueerd-cache](https://github.com/SoCreate/service-fabric-distributed-cache). In dit scenario, verliest u de gegevens in de cache en het maakt niet uit.
- Tijdsgebonden-gegevens - u wilt bewaren van gegevens in de buurt om te berekenen voor latentie voor een bepaalde periode, maar die gegevens niet verloren gaan een *na noodgevallen* scenario. Bijvoorbeeld veel IoT-oplossingen moeten dicht bij compute, bijvoorbeeld berekenen van de gemiddelde temperatuur in de afgelopen paar dagen, maar als deze gegevens verloren gaan, klikt u vervolgens de specifieke gegevens verwijst vastgelegd niet die belangrijk zijn. Ook in dit scenario u niet doorgaans geïnteresseerd bent in back-up van de afzonderlijke gegevenspunten, alleen van de berekende gemiddelden die periodiek naar externe opslag worden geschreven.  
- Lange-termijngegevens - betrouwbare verzamelingen gegevens kunnen opslaan definitief. In dit geval u moet echter [voorbereiden voor herstel na noodgevallen](https://docs.microsoft.com/azure/service-fabric/service-fabric-disaster-recovery) inclusief [periodieke back-upbeleid configureren](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-configure-periodic-backup) voor uw clusters. In feite configureert u wat gebeurt er als uw cluster is verwijderd in een scenario voor noodherstel, waar u moet een nieuw cluster maken en over het implementeren van nieuwe exemplaren van de toepassing en het herstellen van de meest recente back-up.

Kosten besparen en de beschikbaarheid te verbeteren:
- U kunt de kosten met stateful services verlagen omdat u niet in rekening op kosten van gegevens in access en transacties van de externe opslag gebracht en hoeft geen gebruik van een andere service, zoals Azure Redis.
- Met behulp van stateful services voornamelijk voor opslag en niet voor compute is duur en niet aanbevolen. Houd rekening met stateful services als compute met goedkope lokale opslag.
- Door het verwijderen van afhankelijkheden van andere services, kunt u de beschikbaarheid van uw service te verbeteren. Met de status die wordt beheerd met HA in het cluster worden geïsoleerd van andere service-downtimes of latentieproblemen.

## <a name="how-to-properly-work-with-reliable-services"></a>Goed werken met betrouwbare Services
Reliable Services kunnen u eenvoudig staatloze en stateful services maken. Lees de [Kennismaking met betrouwbare Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction)
- Altijd in acht neemt de [annulering token](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-lifecycle#stateful-service-primary-swaps) in de `RunAsync()` methode voor staatloze en stateful services en de `ChangeRole()` methode voor stateful services. Zonder deze weet Service Fabric niet als uw service kan worden gesloten. Bijvoorbeeld, kan niet naleven van de token van de annulering resulteren in veel langer application upgrade tijden.
-   Openen en sluiten [Communicatielisteners](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication) tijdig waarbij de annuleringstokens worden gehandhaafd.
-   Nooit combineren sync-code met async-code. Bijvoorbeeld, gebruik geen `.GetAwaiter().GetResult()` in uw async-aanroepen; het moet asynchrone *helemaal* via de aanroepstack.

## <a name="how-to-properly-work-with-reliable-actors"></a>Goed werken met betrouwbare actoren
Reliable Actors kunt u eenvoudig maken stateful, virtuele actoren. Lees de [Inleiding tot Reliable Actors](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction)

- Overweeg daarom sterk pub/sub-uitwisseling van berichten tussen uw actoren voor het schalen van de toepassing. Bijvoorbeeld, de [open source SoCreate pub/sub](https://service-fabric-pub-sub.socreate.it/) of [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).
- Controleer de actorstatus als [gedetailleerde mogelijk](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices).
- Beheren van de [levenscyclus van de actor](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices). Actoren verwijderen als u niet verder stromen ooit opnieuw te gebruiken. Dit geldt met name bij het gebruik van de [VolatileState provider](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#state-persistence-and-replication) zoals alle de status is opgeslagen in het geheugen.
- Vanwege hun [inschakelen op basis van gelijktijdigheid](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction#concurrency) actoren beste kunnen worden gebruikt als onafhankelijke objecten. Geen grafieken van meerdere actor, synchrone methodeaanroepen (elk van deze waarschijnlijk wordt een afzonderlijk netwerk-aanroep) maken of cirkelvormige actor aanvragen; Deze heeft aanzienlijke invloed op prestaties en schaalbaarheid.
- Combineer geen Synchronisatiecode met async-code; er nodig zijn asynchrone helemaal, om prestatieproblemen te voorkomen dat.
- De uitvoering lang duurt niet aanroepen in actors, overige aanroepen naar de dezelfde actor, vanwege de gelijktijdigheid van de beurt worden geblokkeerd.
- Als communicatie met andere services met behulp van [externe communicatie met Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-remoting) en u maakt een `ServiceProxyFactory`, maakt u de fabrieksinstellingen op het [actor-service](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-using) niveau en *niet*op het niveau van de actor.


## <a name="application-diagnostics"></a>Application diagnostics
- Worden bij het toevoegen van uitgebreide [toepassingslogboeken](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) in serviceaanroepen. Dit helpt bij de diagnose van scenario's waar services elkaar aanroepen. Bijvoorbeeld, als A -> B -> C D overal; fungeren als de aanroep -> Als er niet voldoende logboekregistratie, is het moeilijk zijn om op te sporen. Als de services zijn te veel vanwege aanroep volumes logboekregistratie, ten minste moet aanmelden fouten en waarschuwingen.

## <a name="iot-and-messaging-applications"></a>IoT en toepassingen-berichten
Bij het lezen van berichten van [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) of [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/) gebruiken [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor) die kan worden geïntegreerd met Service Fabric Reliable Services te onderhouden de status van het lezen van de Event Hub-partities en nieuwe berichten verstuurd naar uw services via de `IEventProcessor::ProcessEventsAsync()` methode.


## <a name="design-guidance-on-azure"></a>Ontwerprichtlijnen voor Azure
* Ga naar de [Azure architecture center](https://docs.microsoft.com/azure/architecture/microservices/) voor hulp bij het ontwerp [microservices bouwen op Azure](https://docs.microsoft.com/azure/architecture/microservices/)

* Ga naar [aan de slag met Azure voor Gaming](https://docs.microsoft.com/gaming/azure/) voor hulp bij het ontwerp [met behulp van Service Fabric in gamingservices](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf)
