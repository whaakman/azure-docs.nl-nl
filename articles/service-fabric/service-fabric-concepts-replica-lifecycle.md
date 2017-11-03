---
title: Replica's en -exemplaren in Azure Service Fabric | Microsoft Docs
description: Replica's en -exemplaren--de functie en de levenscyclus begrijpen
services: service-fabric
documentationcenter: .net
author: appi101
manager: anuragg
editor: 
ms.assetid: d5ab75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: aprameyr
ms.openlocfilehash: b4a01752cf2658bcc8dea663462336ca5c610d70
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="replicas-and-instances"></a>Replica's en -exemplaren 
In dit artikel biedt een overzicht van de levenscyclus van replica's van stateful services en exemplaren van stateless services.

## <a name="instances-of-stateless-services"></a>Exemplaren van stateless services
Een exemplaar van een staatloze service is een kopie van de service-logica die wordt uitgevoerd op een van de knooppunten van het cluster. Een exemplaar binnen een partitie wordt uniek geïdentificeerd door de **InstanceId**. De levenscyclus van een exemplaar is gemodelleerd in het volgende diagram:

![De levenscyclus van het exemplaar](./media/service-fabric-concepts-replica-lifecycle/instance.png)

### <a name="inbuild-ib"></a>InBuild (IB)
Nadat de Cluster Resource Manager een plaatsing voor het exemplaar, schakelt over de status van deze levenscyclus. Het exemplaar is op het knooppunt gestart. De toepassingshost wordt gestart, het exemplaar wordt gemaakt en vervolgens geopend. Nadat het opstarten is voltooid, wordt het exemplaar overgezet naar de status gereed. 

Als de toepassingshost of het knooppunt voor dit exemplaar vastloopt, verandert deze in de verwijderde staat.

### <a name="ready-rd"></a>Gereed (RD)
De status gereed heeft, worden in het exemplaar actief en werkend is op het knooppunt. Als dit exemplaar een betrouwbare service is **RunAsync** is aangeroepen. 

Als de toepassingshost of het knooppunt voor dit exemplaar vastloopt, verandert deze in de verwijderde staat.

### <a name="closing-cl"></a>Afsluiten (CL)
In de toestand van de afsluiting wordt Azure Service Fabric wordt afgesloten met het exemplaar op dit knooppunt. Hierdoor wordt mogelijk veroorzaakt door diverse redenen--bijvoorbeeld een upgrade van de toepassing, taakverdeling of de service worden verwijderd. Nadat afsluiten is voltooid, verandert deze in de verwijderde staat.

### <a name="dropped-dd"></a>Verwijderde (DD)
De verwijderde status heeft, is het exemplaar op het knooppunt niet meer actief. Service Fabric onderhoudt op dit moment de metagegevens over deze instantie die uiteindelijk ook is verwijderd.

> [!NOTE]
> Het is mogelijk om een overgang van de status van de verwijderde status met behulp van de **ForceRemove** kiezen op `Remove-ServiceFabricReplica`.
>

## <a name="replicas-of-stateful-services"></a>Replica's van stateful services
Een replica van een stateful service is een kopie van de logica van de service uitgevoerd op een van de knooppunten van het cluster. De replica handhaaft bovendien een kopie van de status van die service. Twee verwante concepten beschrijven de levenscyclus van en het gedrag van stateful replica's:
- De levenscyclus van de replica
- Replicarol

De bespreking van de volgende beschrijving persistente stateful services. Voor vluchtige (of in het geheugen) stateful services zijn de status omlaag en verwijderde equivalent.

![De levenscyclus van de replica](./media/service-fabric-concepts-replica-lifecycle/replica.png)

### <a name="inbuild-ib"></a>InBuild (IB)
Een InBuild-replica is een replica die is gemaakt of worden voorbereid voor het lidmaatschap van de replicaset. Afhankelijk van de replicarol heeft de IB verschillende semantische gedeelten. 

Als de toepassingshost of het knooppunt voor een InBuild-replica vastloopt, verandert deze in de status omlaag.

   - **Primaire InBuild-replica's**: primaire InBuild zijn de eerste replica's voor een partitie. Deze replica gebeurt meestal wanneer de partitie wordt gemaakt. Primaire InBuild-replica's ook worden veroorzaakt wanneer alle replica's van een partitie opnieuw opstart of worden verwijderd.

   - **IdleSecondary InBuild-replica's**: dit zijn de nieuwe replica's die zijn gemaakt door de Cluster Resource Manager of een back-bestaande replica's die is gegaan omlaag en moeten worden toegevoegd in de set. Deze replica's zijn gemaakt of gebouwd door de primaire voordat ze kunnen deelnemen aan de replicaset als ActiveSecondary en deelnemen aan quorum bevestiging van bewerkingen.

   - **ActiveSecondary InBuild-replica's**: deze status is waargenomen in sommige query's. Het is niet wijzigen van een optimalisatie waar de replica is ingesteld, maar een replica moet worden gebouwd. De replica zelf volgt de normale status machine overgangen (zoals beschreven in de sectie van replica-functies).

### <a name="ready-rd"></a>Gereed (RD)
Een gereed replica is een replica die deel uitmaakt van replicatie en quorum bevestiging van bewerkingen. De status gereed is van toepassing op de primaire en actieve secundaire replica's.

Als de toepassingshost of het knooppunt voor een gereed replica vastloopt, verandert deze in de status omlaag.

### <a name="closing-cl"></a>Afsluiten (CL)
Een replica krijgt de status van de afsluiting in de volgende scenario's:

- **Afsluiten van de code voor de replica**: Service Fabric mogelijk afsluiten van de code voor een replica is uitgevoerd. Hierdoor kan om verschillende redenen zijn. Het kan bijvoorbeeld gebeuren vanwege een toepassing, fabric of upgrade van de infrastructuur, of vanwege een fout gerapporteerd door de replica. Als de replica sluit is voltooid, wordt de replica overgezet naar de status omlaag. De permanente status die is gekoppeld aan deze replica die opgeslagen op schijf is niet opgeruimd.

- **De replica verwijderen uit het cluster**: Service Fabric moet mogelijk de permanente status verwijderen en afsluiten van de code voor een replica is uitgevoerd. Hierdoor mogelijk om verschillende redenen, bijvoorbeeld voor de load balancer.

### <a name="dropped-dd"></a>Verwijderde (DD)
De verwijderde status heeft, is het exemplaar op het knooppunt niet meer actief. Er is ook geen status links op het knooppunt. Service Fabric onderhoudt op dit moment de metagegevens over deze instantie die uiteindelijk ook is verwijderd.

### <a name="down-d"></a>Omlaag (D)
De replica-code wordt niet uitgevoerd in de status Omlaag, maar de permanente status voor deze replica bestaat op dat knooppunt. Een replica kan niet beschikbaar om verschillende redenen--bijvoorbeeld het knooppunt buiten werking is, wordt een crash in de replica-code of een upgrade van de toepassing een replica-fouten.

Een omlaag replica is geopend door Service Fabric als vereist, bijvoorbeeld wanneer de upgrade is voltooid op het knooppunt.

De replicarol is niet relevant in de status omlaag.

### <a name="opening-op"></a>Openen (OP)
Een replica omlaag krijgt de status opening wanneer Service Fabric nodig heeft om de replica een back-up opnieuw. Deze status zijn mogelijk als een code-upgrade voor de toepassing is op een knooppunt is voltooid. 

Als de toepassingshost of het knooppunt voor de replica van een openen vastloopt, verandert deze in de status omlaag.

De replicarol is niet relevant de status openen.

### <a name="standby-sb"></a>Stand-by (SB)
De replica van een stand-by is een replica van een permanente service die werd afgesloten en vervolgens is geopend. Deze replica kan worden gebruikt door Service Fabric als een andere replica toevoegen aan de replicaset (omdat de replica is al een gedeelte van de status en het buildproces sneller is). Na het verstrijken van de StandByReplicaKeepDuration wordt de stand-by-replica genegeerd.

Als de toepassingshost of het knooppunt voor een stand-by-replica vastloopt, verandert deze in de status omlaag.

De replicarol is niet relevant de stand-by-status.

> [!NOTE]
> Een replica die niet is niet actief of verwijderd wordt beschouwd als *up*.
>

> [!NOTE]
> Het is mogelijk om een overgang van de status van de verwijderde status met behulp van de **ForceRemove** kiezen op `Remove-ServiceFabricReplica`.
>

## <a name="replica-role"></a>Replicarol 
De rol van de replica is bepalend voor de functie in de replicaset:

- **Primaire (P)**: Er is een primaire in de replicaset die verantwoordelijk is voor het uitvoeren van lees- en schrijfbewerkingen. 
- **ActiveSecondary (S)**: dit zijn replica's die zijn ontvangen van updates van de status van de primaire en verzend back bevestigingen ze toepassen. Er zijn meerdere actieve secundaire replica's in de replicaset. Het aantal van deze actieve secundaire replica's bepaalt het aantal fouten dat de service kan verwerken.
- **IdleSecondary (I)**: deze replica's door de primaire zijn gebouwd. Ze ontvangen status van de primaire voordat ze kunnen worden gepromoveerd naar actieve secundaire. 
- **Geen (N)**: deze replica's niet een verantwoordelijkheid hebben in de replicaset.
- **Onbekend (U)**: dit is de eerste rol van een replica voordat het een ontvangt **ChangeRole** API-aanroep van Service Fabric.

Het volgende diagram illustreert de rol replica overgangen en enkele voorbeeldscenario's waarin ze kunnen optreden:

![Replicarol](./media/service-fabric-concepts-replica-lifecycle/role.png)

- U->-P: maken van een nieuwe primaire replica.
- U -> I: maken van een nieuwe niet-actieve replica.
- U -> N: verwijdering van een stand-by-replica.
- Ik-S: de promotie van de niet-actieve secundaire actieve secundaire > zodat de bevestigingen aan quorum bijdragen.
- Ik-P: promotie van de niet-actieve secundaire op primaire >. Dit kan gebeuren onder speciale herconfiguraties wanneer de niet-actieve secundaire is de juiste candidate primaire worden.
- Ik -> N: verwijdering van de niet-actieve secundaire replica.
- S-P: promotie van de actieve secundaire op primaire >. Dit kan zijn vanwege de failover van de primaire of een primaire verplaatsing geïnitieerd door de Cluster Resource Manager. Het kan bijvoorbeeld zijn in reactie op een upgrade van de toepassing of taakverdeling.
- S -> N: verwijdering van de actieve secundaire replica.
- P -> S: degradatie van de primaire replica. Dit kan zijn vanwege een primaire verplaatsing geïnitieerd door de Cluster Resource Manager. Het kan bijvoorbeeld zijn in reactie op een upgrade van de toepassing of taakverdeling.
- P -> N: verwijdering van de primaire replica.

> [!NOTE]
> Een hoger niveau programmering modellen, zoals [Reliable Actors](service-fabric-reliable-actors-introduction.md) en [Reliable Services](service-fabric-reliable-services-introduction.md), verbergen van het concept van rollen van de replica van de ontwikkelaar. In actoren is het begrip van een rol niet nodig. In de Services, wordt deze grotendeels vereenvoudigd voor de meeste scenario's.
>

## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel voor meer informatie over Service Fabric-concepten:

[Levenscyclus van Reliable Services - C#](service-fabric-reliable-services-lifecycle.md)

