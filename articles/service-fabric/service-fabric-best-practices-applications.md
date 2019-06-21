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
ms.date: 06/18/2019
ms.author: msfussell
ms.openlocfilehash: 30d696337061ade6b79c7ec0e4c4de67651f0dad
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203455"
---
# <a name="azure-service-fabric-application-design-best-practices"></a>Azure Service Fabric-toepassing aanbevolen procedures voor ontwerp

Dit artikel bevat richtlijnen voor best practices voor het bouwen van toepassingen en services in Azure Service Fabric.
 
## <a name="get-familiar-with-service-fabric"></a>Raak vertrouwd met de Service Fabric
* Lees de [, zodat u wilt meer informatie over Service Fabric?](service-fabric-content-roadmap.md) artikel.
* Meer informatie over [scenario's voor Service Fabric application](service-fabric-application-scenarios.md).
* Inzicht in de programming model keuzes door te lezen [Service Fabric programming model overview](service-fabric-choose-framework.md).



## <a name="application-design-guidance"></a>Ontwerprichtlijnen voor toepassing
Vertrouwd raken met de [algemene architectuur](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) van Service Fabric-toepassingen en de bijbehorende [ontwerpoverwegingen](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#design-considerations).

### <a name="choose-an-api-gateway"></a>Kies een API-gateway
Gebruik een API-gateway-service die communiceert met de back-end-services die vervolgens kunnen worden uitgebreid. De meest voorkomende API-gateway-services die worden gebruikt zijn:

- [Met Azure API Management](https://docs.microsoft.com/azure/service-fabric/service-fabric-api-management-overview), die is [geïntegreerd met Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-api-management).
- [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) of [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/), met de [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor) lezen uit Event Hub-partities.
- [Træfik omgekeerde proxy](https://blogs.msdn.microsoft.com/azureservicefabric/2018/04/05/intelligent-routing-on-service-fabric-with-traefik/), met de [Azure Service Fabric-provider](https://docs.traefik.io/configuration/backends/servicefabric/).
- [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/).

   > [!NOTE] 
   > Azure Application Gateway wordt niet rechtstreeks geïntegreerd met Service Fabric. Azure API Management is doorgaans de beste keuze.
- Uw eigen op maat gemaakte [ASP.NET Core](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore) web application-gateway.

### <a name="stateless-services"></a>Stateless services
Het wordt aangeraden dat u altijd beginnen bij het inbouwen van stateless services met behulp van [Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction) en status op te slaan in een Azure-database, Azure Cosmos DB of Azure Storage. Externalized status is de meer vertrouwde methode voor de meeste ontwikkelaars. Deze methode kunt u profiteren van de query-mogelijkheden in de store.  

### <a name="when-to-use-stateful-services"></a>Wanneer u stateful services
Stateful services overwegen wanneer u een scenario voor lage latentie en nodig hebt om de gegevens dicht bij de berekening te houden. Sommige scenario's met voorbeelden zijn digitale kopie-IoT-apparaten, status van het spel, sessiestatus, caching van gegevens uit een database en langlopende werkstromen om bij te houden van aanroepen naar andere services.

Bepaal op het tijdsbestek voor bewaren van gegevens:

- **In de cache opgeslagen gegevens**. Gebruik in cache plaatsen wanneer een probleem is met de latentie voor externe stores. Een stateful service als de gegevenscache van uw eigen gebruiken of kunt u overwegen de [open-source SoCreate Service Fabric gedistribueerde Cache](https://github.com/SoCreate/service-fabric-distributed-cache). In dit scenario hoeft u te worden betrokken als u al uw gegevens in de cache raakt.
- **Tijdsgebonden gegevens**. In dit scenario moet u gegevens in de buurt houden om te berekenen voor een bepaalde periode voor latentie, maar u kunt het zich veroorloven verloren gaan de gegevens in een *na noodgevallen*. Bijvoorbeeld, in veel IoT-oplossingen, gegevens moeten worden dicht bij de Reken-, zoals wanneer de gemiddelde temperatuur gedurende de afgelopen paar dagen wordt berekend, maar als deze gegevens verloren gaan, de specifieke gegevenspunten vastgelegd dat belangrijke niet. Ook in dit scenario niet doorgaans zo belangrijk back-ups van de afzonderlijke gegevenspunten. U alleen back-up berekende gemiddelden die regelmatig worden geschreven naar de externe opslag.  
- **Lange-termijngegevens**. Uw gegevens kunnen permanent opslaan in betrouwbare verzamelingen. Maar in dit geval moet u [voorbereiden voor herstel na noodgevallen](https://docs.microsoft.com/azure/service-fabric/service-fabric-disaster-recovery), waaronder [periodieke back-upbeleid configureren](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-configure-periodic-backup) voor uw clusters. In feite configureert u wat gebeurt er als uw cluster is verwijderd in een noodgeval, waar u moet een nieuw cluster maken en over het implementeren van nieuwe exemplaren van de toepassing en het herstellen van de meest recente back-up.

Kosten besparen en de beschikbaarheid te verbeteren:
- U kunt met behulp van stateful services omdat u hiervoor geen toegang tot gegevens en transacties kosten verminderen van de externe opslag, en omdat u niet hoeft te gebruiken een andere service, zoals Azure Cache voor Redis.
- Met behulp van stateful services voornamelijk voor opslag en niet voor compute is duur en wordt niet aanbevolen. Stateful services beschouwen als Computing met goedkope lokale opslag.
- Door het verwijderen van afhankelijkheden van andere services, kunt u de beschikbaarheid van uw service te verbeteren. Status beheren met HA in het cluster worden geïsoleerd van andere service-downtimes of latentieproblemen.

## <a name="how-to-work-with-reliable-services"></a>Over het werken met betrouwbare Services
Service Fabric Reliable Services kunt u eenvoudig staatloze en stateful services maken. Zie voor meer informatie de [Kennismaking met betrouwbare Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction).
- Altijd in acht neemt de [annulering token](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-lifecycle#stateful-service-primary-swaps) in de `RunAsync()` methode voor staatloze en stateful services en de `ChangeRole()` methode voor stateful services. Als u dit niet doet, biedt geen Service Fabric weet Als uw service kan worden gesloten. Bijvoorbeeld, als u niet voldoen aan het token annulering, kunnen veel langer application upgrade tijden optreden.
-   Openen en sluiten [communicatielisteners](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication) op tijdige wijze, waarbij de annuleringstokens worden gehandhaafd.
-   Nooit combineren sync-code met async-code. Bijvoorbeeld niet gebruiken `.GetAwaiter().GetResult()` in uw async-aanroepen. Gebruik asynchrone *helemaal* via de aanroepstack.

## <a name="how-to-work-with-reliable-actors"></a>Over het werken met betrouwbare actoren
Service Fabric Reliable Actors kunt u eenvoudig maken stateful, virtuele actoren. Zie voor meer informatie de [Inleiding tot Reliable Actors](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction).

- Overweeg daarom ernstig pub/sub-uitwisseling van berichten tussen uw actoren voor het schalen van uw toepassing. Hulpprogramma's die deze service omvatten de [open-source SoCreate Service Fabric Pub/Sub](https://service-fabric-pub-sub.socreate.it/) en [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).
- Controleer de actorstatus als [gedetailleerde mogelijk](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices).
- Beheren van de [levenscyclus van de actor](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices). Actoren verwijderen als u niet verder stromen opnieuw te gebruiken. Verwijderen van overbodige actoren is vooral belangrijk wanneer u de [vluchtige state-provider](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#state-persistence-and-replication), omdat alle de status is opgeslagen in het geheugen.
- Vanwege hun [inschakelen op basis van gelijktijdigheid](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction#concurrency), het meest geschikt voor actors als onafhankelijke objecten. Geen grafieken van meerdere actor, synchrone methodeaanroepen (elk van deze waarschijnlijk wordt een afzonderlijk netwerk-aanroep) maken of cirkelvormige actor verzoeken om te maken. Deze heeft aanzienlijke invloed op prestaties en schaalbaarheid.
- Combineer geen Synchronisatiecode met async-code. Asynchrone consistent gebruiken om te voorkomen dat prestatieproblemen.
- Maak niet langdurig aanroepen in actoren. Overige aanroepen naar de dezelfde actor, vanwege de gelijktijdigheid van de beurt, langlopende aanroepen worden geblokkeerd.
- Als u communiceert met andere services met behulp van [externe communicatie met Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-remoting) en u maakt een `ServiceProxyFactory`, maken van de gegevensfactory op de [actor-service](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-using) niveau en *niet* op het niveau van de actor.


## <a name="application-diagnostics"></a>Application diagnostics
Over het toevoegen van grondig worden [toepassingslogboeken](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) in serviceaanroepen. Dit helpt u vaststellen scenario's waarin services elkaar aanroepen. Bijvoorbeeld, wanneer een aanroepen roept service B C D aanroept, kan de aanroep mislukken overal. Als u geen voldoende logboekregistratie, fouten, zijn moeilijk op te sporen. Als de services zijn te veel vanwege aanroep volumes logboekregistratie, zorg er dan voor dat ten minste aanmelden fouten en waarschuwingen.

## <a name="iot-and-messaging-applications"></a>IoT en toepassingen-berichten
Wanneer u berichten van leest [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) of [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/), gebruikt u [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor). ServiceFabricProcessor kan worden geïntegreerd met Service Fabric Reliable Services voor het onderhouden van de status van het lezen van de event hub-partities en nieuwe berichten verstuurd naar uw services via de `IEventProcessor::ProcessEventsAsync()` methode.


## <a name="design-guidance-on-azure"></a>Ontwerprichtlijnen voor Azure
* Ga naar de [Azure architecture center](https://docs.microsoft.com/azure/architecture/microservices/) voor hulp bij het ontwerp [microservices bouwen op Azure](https://docs.microsoft.com/azure/architecture/microservices/).

* Ga naar [aan de slag met Azure voor Gaming](https://docs.microsoft.com/gaming/azure/) voor hulp bij het ontwerp [met behulp van Service Fabric in gamingservices](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf).
