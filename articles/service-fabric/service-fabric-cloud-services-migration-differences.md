---
title: Verschillen tussen Cloudservices en Service Fabric | Microsoft Docs
description: Een conceptueel overzicht voor het migreren van toepassingen van Cloud Services naar Service Fabric.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 0b87b1d3-88ad-4658-a465-9f05a3376dee
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 4682e47e664384a6869e1a74e3de6d9083db082b
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669448"
---
# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Meer informatie over de verschillen tussen Cloud Services en Service Fabric voor het migreren van toepassingen.
Microsoft Azure Service Fabric is de volgende generatie toepassingen cloudplatform voor uiterst schaalbare webhostingservice met uiterst betrouwbare gedistribueerde toepassingen. Het geeft veel nieuwe functies voor het verpakken, implementeren, bijwerken en beheren van gedistribueerde cloud-Apps. 

Dit is een inleidende handleiding voor het migreren van toepassingen van Cloud Services naar Service Fabric. Het richt zich voornamelijk op architectuur en ontwerp van de verschillen tussen Cloud Services en Service Fabric.

## <a name="applications-and-infrastructure"></a>Toepassingen en infrastructuur
Een fundamenteel verschil tussen Cloud Services en Service Fabric is de relatie tussen virtuele machines, workloads en toepassingen. Hier een werkbelasting wordt gedefinieerd als de code schrijven naar een specifieke taak uitvoeren- of -service biedt.

* **Cloudservices is over het implementeren van toepassingen, zoals virtuele machines.** De code die u schrijft is nauw gekoppeld aan een VM-exemplaar, zoals een Web- of Werkrol. Is het implementeren van een of meer VM-exemplaren die worden uitgevoerd van de werkbelasting voor het implementeren van een workload in Cloud Services. Er is geen scheiding van toepassingen en virtuele machines en dus er is geen officiële definitie van een toepassing. Een toepassing kan van worden beschouwd als een reeks Web- of Werkrol-exemplaren in een implementatie van Cloud Services of als een volledige implementatie van Cloud Services. In dit voorbeeld wordt wordt een toepassing weergegeven als een set rolinstanties.

![Cloud Services-toepassingen en -topologie][1]

* **Service Fabric is over het implementeren van toepassingen aan bestaande virtuele machines of machines waarop Windows of Linux, Service Fabric.** De services die u schrijft zijn volledig elkaar zijn losgekoppeld van de onderliggende infrastructuur, die abstract opgeslagen door de Service Fabric-toepassing-platform gemaakt is, zodat een toepassing kan worden geïmplementeerd op meerdere omgevingen. Een workload in Service Fabric is een 'service' genoemd en een of meer services worden gegroepeerd in een formeel gedefinieerde toepassing die wordt uitgevoerd op het platform voor Service Fabric-toepassingen. Meerdere toepassingen kunnen worden geïmplementeerd in een Service Fabric-cluster.

![Service Fabric-toepassingen en -topologie][2]

Service Fabric zelf is een platform toepassingslaag die wordt uitgevoerd op Windows of Linux, terwijl Cloud Services een systeem is voor het implementeren van virtuele machines van Azure worden beheerd met werkbelastingen die zijn gekoppeld.
De Service Fabric-toepassingsmodel heeft een aantal voordelen:

* Snelle implementatietijden. Het maken van VM-exemplaren kan enige tijd duren. Virtuele machines worden in Service Fabric, alleen geïmplementeerd zodra u een cluster wordt gevormd die als host fungeert voor het Service Fabric-platform voor toepassingen. Vanaf dat moment op kunnen toepassingspakketten worden geïmplementeerd met het cluster erg snel.
* Hosting met hoge dichtheid. In Cloud Services, een Worker-rol virtuele machine fungeert als host een werkbelasting. In Service Fabric zijn de toepassingen gescheiden van de virtuele machines die worden uitgevoerd, wat betekent dat u een groot aantal toepassingen op een klein aantal virtuele machines, die de totale kosten voor grotere implementaties kan verlagen kunt implementeren.
* De Service Fabric platform overal kan worden uitgevoerd die is Windows Server of Linux-machines, of u nu Azure of on-premises. Het platform biedt een abstractielaag over de onderliggende infrastructuur, zodat uw toepassing op verschillende omgevingen uitvoeren kunt. 
* Beheer van gedistribueerde toepassingen. Service Fabric is een platform dat niet alleen hosts toepassingen gedistribueerde, maar ook helpt hun levensduur onafhankelijk van de host virtuele machine of computer levenscyclus beheren.

## <a name="application-architecture"></a>Toepassingsarchitectuur
De architectuur van een Cloud Services-toepassing omvat gewoonlijk talrijke externe service-afhankelijkheden, zoals Service Bus, Azure Table en Blob Storage, SQL, Redis en anderen voor het beheren van de status en gegevens van een toepassing en de communicatie tussen Web en Werkrollen in een implementatie van Cloud Services. Een voorbeeld van een volledige Cloud Services-toepassing kan er als volgt uitzien:  

![Architectuur van cloud Services][9]

Service Fabric-toepassingen kunt ook de dezelfde externe services gebruiken in een volledige toepassing. Met behulp van dit voorbeeld van de architectuur van Cloud Services, wordt het eenvoudigste migratiepad van Cloud Services naar Service Fabric vervangen door de implementatie van de Cloudservices met een Service Fabric-toepassing, de algehele architectuur blijven hetzelfde. De Web- en werkrollen worden overgezet naar de stateless Service Fabric-services met minimale codewijzigingen.

![Service Fabric-architectuur na de migratie van eenvoudige][10]

In deze fase, moet het systeem nog steeds werken hetzelfde als voorheen. U gebruik blijft maken van Service Fabric van stateful functies, externe status stores kunt worden internalized zoals stateful services waar van toepassing. Dit is meer betrokken dan een eenvoudige migratie van Web- en werkrollen naar de stateless Service Fabric-services, zoals het schrijven van aangepaste services die eerder voor de externe services bieden dezelfde functionaliteit voor uw toepassing vereist. De voordelen hiervan zijn: 

* Externe afhankelijkheden verwijderen 
* Werking van de implementatie, beheer en upgrade-modellen. 

Een voorbeeld van de resulterende architectuur van deze services internalizing kan er als volgt:

![Service Fabric-architectuur na volledige migratie][11]

## <a name="communication-and-workflow"></a>Communicatie en werkstroom
De meeste Cloud Service-toepassingen bestaan uit meer dan één laag. Op dezelfde manier, een Service Fabric-toepassing bestaat uit meer dan één service (meestal veel services). Twee algemene communicatie-modellen zijn rechtstreekse communicatie en via een externe, duurzame opslag.

### <a name="direct-communication"></a>Rechtstreekse communicatie
Met rechtstreekse communicatie communiceren lagen rechtstreeks met eindpunt door elke laag beschikbaar gesteld. In de stateless omgevingen zoals Cloud Services, dit betekent dat een exemplaar van een VM-rol, een willekeurig selecteren of round-robin saldo load en rechtstreeks verbinding maken met het eindpunt.

![Cloud Services rechtstreekse communicatie][5]

 Rechtstreekse communicatie is een algemene communicatiemodel in Service Fabric. Het belangrijkste verschil tussen Service Fabric en Cloud Services is dat in Cloud-Services dat u verbinding met een virtuele machine maakt, terwijl in Service Fabric u verbinding met een service maken. Dit is een belangrijke onderscheidende factor zijn voor een aantal redenen:

* Services in Service Fabric zijn niet gebonden aan de virtuele machines die als host fungeren. Services kunnen worden verplaatst in het cluster, en in feite wordt verwacht dat het om diverse redenen verplaatsen: Bron-balancing, failover, toepassingen en infrastructuur upgrades en plaatsing of load-beperkingen. Dit betekent dat-adres van een service-exemplaar kunt op elk gewenst moment wijzigen. 
* Een virtuele machine in Service Fabric kan meerdere services, elk met unieke eindpunten hosten.

Service Fabric biedt een mechanisme service detectie, met de naam de Naming-Service, die kan worden gebruikt om op te lossen eindpuntadressen van services. 

![Service Fabric rechtstreekse communicatie][6]

### <a name="queues"></a>Wachtrijen
Een algemene mechanisme voor communicatie tussen lagen in stateless omgevingen zoals Cloud Services is het gebruik van een externe storage-wachtrij voor het opslaan van blijvend Werktaken van één laag naar een andere. Een veelvoorkomend scenario is een weblaag waarmee taken worden verzonden naar een Azure-wachtrij of een Service Bus waarbij instanties van de werknemer kunnen uit de wachtrij verwijderen en de taken worden verwerkt.

![Cloud Services wachtrij communicatie][7]

Het communicatiemodel met dezelfde kan worden gebruikt in Service Fabric. Dit kan nuttig zijn bij het migreren van een bestaande Cloud Services-toepassing in Service Fabric. 

![Service Fabric rechtstreekse communicatie][8]

## <a name="next-steps"></a>Volgende stappen
Het eenvoudigste migratiepad van Cloud Services naar Service Fabric wordt vervangen door de implementatie van de Cloudservices met een Service Fabric-toepassing, blijven de algehele architectuur van uw toepassing ongeveer hetzelfde. Het volgende artikel bevat richtlijnen om u te helpen bij het converteren van een Web- of Werkrol naar een stateless Service Fabric-service.

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
