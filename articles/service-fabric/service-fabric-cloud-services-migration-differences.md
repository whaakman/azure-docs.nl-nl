---
title: Verschillen tussen Cloudservices en het Service Fabric | Microsoft Docs
description: Conceptueel overzicht voor het migreren van toepassingen van Cloud-Services naar Service Fabric.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 0b87b1d3-88ad-4658-a465-9f05a3376dee
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 4bb5d92cd46533b46b388d178990f230424b09dc
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Meer informatie over de verschillen tussen Cloudservices en Service Fabric voordat het migreren van toepassingen.
Microsoft Azure Service Fabric is de volgende generatie toepassing cloudplatform voor een zeer schaalbaar, maximaal betrouwbare gedistribueerde toepassingen. Het geeft veel nieuwe functies voor verpakking, implementeren, bijwerken en beheren van gedistribueerde cloud-toepassingen. 

Dit is een inleidende handleiding voor het migreren van toepassingen van Cloud-Services naar Service Fabric. Het richt zich voornamelijk op architectuur en verschillen tussen Cloudservices en Service Fabric ontwerpen.

## <a name="applications-and-infrastructure"></a>Toepassingen en infrastructuur
Een fundamentele verschil tussen Cloud Services en de Service Fabric is de relatie tussen virtuele machines, workloads en toepassingen. Hier een werkbelasting wordt gedefinieerd als de code schrijven naar een bepaalde taak of een service leveren.

* **Er is een cloud-Services over het implementeren van toepassingen als virtuele machines.** De code die u schrijft is nauw gekoppeld aan een VM-instantie, zoals een webservice of de Werkrol. Is voor het implementeren van een werkbelasting in Cloudservices voor het implementeren van een of meer VM-exemplaren die de werkbelasting worden uitgevoerd. Er is geen scheiding van toepassingen en virtuele machines en dus er is geen formele definitie van een toepassing. Een toepassing worden beschouwd als een reeks Web- of Werkrol-exemplaren in een Cloud Services-implementatie of als een volledige Cloud Services-implementatie. In dit voorbeeld wordt een toepassing worden weergegeven als een reeks rolinstanties.

![Cloud Services-toepassingen en -topologie][1]

* **Service Fabric is over het implementeren van toepassingen aan bestaande virtuele machines of machines Service Fabric uitgevoerd op Windows of Linux.** De services die u schrijft zijn volledig ontkoppelde van de onderliggende infrastructuur, beschouwing verwijderd door het Service Fabric-toepassing-platform, zodat een toepassing kan worden geïmplementeerd in meerdere omgevingen. Een workload in Service Fabric is een 'service' genoemd en een of meer services zijn gegroepeerd in een formeel gedefinieerde toepassing die wordt uitgevoerd op de Service Fabric-toepassing-platform. Meerdere toepassingen kunnen worden geïmplementeerd op één Service Fabric-cluster.

![Service Fabric-toepassingen en -topologie][2]

Service Fabric zelf is een platform toepassingslaag die wordt uitgevoerd op Windows of Linux, terwijl Cloud Services een systeem is voor het implementeren van beheerde Azure VM's met de werkbelastingen die zijn gekoppeld.
Het model Service Fabric-toepassing heeft een aantal voordelen:

* Snelle implementatietijden. VM-instanties maken kan enige tijd duren. In Service Fabric worden virtuele machines alleen geïmplementeerd wanneer voor een cluster wordt gevormd die als host fungeert voor de Service Fabric-toepassing-platform. Vanaf dat moment op kunnen toepassingspakketten worden geïmplementeerd op het cluster zeer snel.
* High-density hosten. In de Cloud-Services, een werknemer rol virtuele machine fungeert als host een werkbelasting. Toepassingen zijn in Service Fabric gescheiden van de virtuele machines die worden uitgevoerd, wat betekent dat u een groot aantal toepassingen naar een klein aantal virtuele machines, die de totale kosten voor grotere implementaties kunt verlagen kunt implementeren.
* De Service Fabric platform van een willekeurige plaats die uitvoeren kunt is Windows Server of Linux-machines of Azure of on-premises. Het platform biedt een abstractielaag over de onderliggende infrastructuur, zodat uw toepassing op verschillende omgevingen uitvoeren kunt. 
* Beheer van gedistribueerde toepassingen. Service Fabric is een platform dat niet alleen hosts toepassingen gedistribueerde, maar ook helpt de hele levenscyclus onafhankelijk van de hosting VM of machine levenscyclus beheren.

## <a name="application-architecture"></a>Toepassingsarchitectuur
De architectuur van een Cloud Services-toepassing omvat gewoonlijk talrijke externe service-afhankelijkheden, zoals Service Bus, Azure Table en Blob Storage, SQL, Redis en anderen voor het beheren van de status en gegevens van een toepassing en de communicatie tussen Web- en werkrollen in een Cloud Services-implementatie. Een voorbeeld van een volledige Cloud Services-toepassing uitzien als volgt:  

![Cloud Services-architectuur][9]

Service Fabric-toepassingen kunt ook gebruik van dezelfde externe services in een volledige toepassing. Met dit voorbeeld Cloud Services-architectuur, is het eenvoudigste migratiepad van Cloud Services naar Service Fabric alleen in de Cloud Services-implementatie vervangen door een Service Fabric-toepassing, de algehele architectuur dezelfde houden. De Web- en werkrollen worden overgezet naar Service Fabric stateless services met minimale codewijzigingen.

![Service Fabric-architectuur na de migratie van eenvoudige][10]

In deze fase moet het systeem doorgaan met werken hetzelfde als voor. Voordeel te halen van de Service-Fabric stateful functies, externe status winkels kan worden internalized als stateful services indien van toepassing. Dit is meer betrokken dan een eenvoudige migratie van Web- en werkrollen naar Service Fabric stateless services, zoals het schrijven van aangepaste services die dezelfde functionaliteit voor uw toepassing bieden als de externe services die zijn gedaan voordat vereist. De voordelen hiervan zijn: 

* Externe afhankelijkheden verwijderen 
* Werking van de implementatie, beheer en upgrade-modellen. 

Een voorbeeld van de resulterende architectuur van deze services internalizing kan er als volgt uitzien:

![Service Fabric-architectuur na volledige migratie][11]

## <a name="communication-and-workflow"></a>Werkstroom en communicatie
De meeste Service in de Cloud-toepassingen bestaan uit meer dan één laag. Op deze manier wordt een Service Fabric-toepassing bestaat uit meer dan één service (meestal veel services). Twee algemene communicatie-modellen zijn rechtstreekse communicatie en via een externe duurzame opslag.

### <a name="direct-communication"></a>Rechtstreekse communicatie
Met directe communicatie communiceren lagen rechtstreeks via eindpunt die worden weergegeven door elke laag. In staatloze omgevingen zoals Cloudservices, dit betekent dat een exemplaar van een VM-rol, ofwel willekeurig selecteren of round-robin load balance en rechtstreeks verbinding maken met het eindpunt.

![Cloud Services rechtstreekse communicatie][5]

 Rechtstreekse communicatie is een algemene communicatiemodel in Service Fabric. Het belangrijkste verschil tussen Service Fabric en Cloud-Services is dat in Cloudservices u verbinding met een virtuele machine maakt, terwijl in Service Fabric u verbinding met een service maakt. Dit is een belangrijk verschil voor een aantal oorzaken hebben:

* Services in Service Fabric zijn niet gekoppeld aan de virtuele machines die als host fungeren. Services in het cluster kan verplaatsen en in feite naar verwachting om verschillende redenen verplaatsen: Resource balancing, failover, toepassingen en infrastructuur upgrades en plaatsing of load-beperkingen. Dit betekent dat het dat adres van een service-exemplaar op elk gewenst moment kunt wijzigen. 
* Een virtuele machine in Service Fabric kan meerdere services, elk met unieke eindpunten hosten.

Service Fabric biedt een service discovery mechanisme, de Naming Service, die kan worden gebruikt voor het omzetten van de adressen van de eindpunten van services genoemd. 

![Service Fabric rechtstreekse communicatie][6]

### <a name="queues"></a>Wachtrijen
Een algemene mechanisme voor communicatie tussen lagen in staatloze omgevingen zoals Cloud Services is het gebruik van een wachtrij externe opslag voor het opslaan van blijvend Werktaken van één laag naar een andere. Een gebruikelijk scenario is een weblaag die taken verzendt naar een Azure-wachtrij of de Service Bus waarbij Werkrol exemplaren kunnen uit de wachtrij halen en verwerken van de taken.

![Cloud Services wachtrij communicatie][7]

Het communicatiemodel met dezelfde kan worden gebruikt in Service Fabric. Dit kan nuttig zijn bij het migreren van een bestaande Cloud Services-toepassing naar Service Fabric. 

![Service Fabric rechtstreekse communicatie][8]

## <a name="next-steps"></a>Volgende stappen
De eenvoudigste migratiepad van Cloud Services naar Service Fabric is ter vervanging van de implementatie van de Cloud Services met een Service Fabric-toepassing houden de algehele architectuur van uw toepassing ongeveer hetzelfde. Het volgende artikel biedt richtlijnen voor het converteren van een Web- of Worker-rol bij een stateless Service Fabric-service.

* [Eenvoudige migratie: een Web- of Werkrol converteren naar een stateless Service Fabric-service](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
