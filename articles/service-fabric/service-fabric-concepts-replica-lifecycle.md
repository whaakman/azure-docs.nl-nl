---
title: Replica's en -exemplaren in Azure Service Fabric | Microsoft Docs
description: Replica's en instanties--de functie en de levenscyclus begrijpen
services: service-fabric
documentationcenter: .net
author: appi101
manager: anuragg
editor: ''
ms.assetid: d5ab75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2018
ms.author: aprameyr
ms.openlocfilehash: 7f8638365b40395a5dd82457c40e5c15209ba1a7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60882389"
---
# <a name="replicas-and-instances"></a>Replica's en instanties 
In dit artikel biedt een overzicht van de levenscyclus van replica's van stateful services en exemplaren van stateless services.

## <a name="instances-of-stateless-services"></a>Exemplaren van stateless services
Een exemplaar van een staatloze service is een kopie van de logica van de service die wordt uitgevoerd op een van de knooppunten van het cluster. Een exemplaar binnen een partitie wordt uniek geïdentificeerd door de **InstanceId**. De levenscyclus van een exemplaar is gemodelleerd in het volgende diagram:

![Levenscyclus van exemplaar](./media/service-fabric-concepts-replica-lifecycle/instance.png)

### <a name="inbuild-ib"></a>InBuild (IB)
Nadat het Cluster Resource Manager een plaatsing voor het exemplaar bepaalt, wordt de status van deze levenscyclus. Het exemplaar wordt gestart op het knooppunt. De toepassingshost wordt gestart, het exemplaar wordt gemaakt en vervolgens geopend. Nadat het opstarten is voltooid, wordt het exemplaar verandert de status gereed. 

Als de toepassingshost of het knooppunt voor dit exemplaar vastloopt, verandert deze in de verwijderde staat.

### <a name="ready-rd"></a>Ready (extern bureaublad)
De status gereed heeft, worden in het exemplaar actief en werkend is op het knooppunt. Als dit exemplaar een betrouwbare service is **RunAsync** is aangeroepen. 

Als de toepassingshost of het knooppunt voor dit exemplaar vastloopt, verandert deze in de verwijderde staat.

### <a name="closing-cl"></a>Afsluiten (CL)
In de status sluiten is Azure Service Fabric momenteel het exemplaar op dit knooppunt wordt afgesloten. Deze afsluiting wordt mogelijk veroorzaakt door verschillende redenen, bijvoorbeeld een upgrade van de toepassing, taakverdeling of de service wordt verwijderd. Nadat de afsluiting is voltooid, verandert deze in de verwijderde status.

### <a name="dropped-dd"></a>Verwijderde (DD)
Het exemplaar wordt niet meer in de verwijderde status uitgevoerd op het knooppunt. Op dit moment onderhoudt Service Fabric de metagegevens voor dit exemplaar, wordt uiteindelijk ook verwijderd.

> [!NOTE]
> Het is mogelijk om een overgang van een staat op de status van de verwijderde met behulp van de **ForceRemove** kiezen op `Remove-ServiceFabricReplica`.
>

## <a name="replicas-of-stateful-services"></a>Replica's van stateful services
Een replica van een stateful service is een kopie van de logica van de service die wordt uitgevoerd op een van de knooppunten van het cluster. Bovendien houdt de replica een kopie van de status van de betreffende service. Twee verwante concepten beschrijven de levenscyclus en het gedrag van stateful replica's:
- Levenscyclus van replica
- Replicarol

Het volgende onderwerp beschrijft de persistente stateful services. Voor vluchtige (of in het geheugen) stateful services zijn de Staten omlaag en verwijderde gelijk.

![Levenscyclus van replica](./media/service-fabric-concepts-replica-lifecycle/replica.png)

### <a name="inbuild-ib"></a>InBuild (IB)
Een InBuild-replica is een replica die is gemaakt of voorbereid voor het lidmaatschap van de replicaset. Afhankelijk van de replicarol heeft het IB verschillende semantiek. 

Als de toepassingshost of het knooppunt voor een InBuild-replica vastloopt, verandert deze in de status down.

   - **Primaire InBuild-replica's**: Primaire InBuild zijn de eerste replica's voor een partitie. Deze replica gebeurt meestal wanneer de partitie wordt gemaakt. Primaire InBuild-replica's zich ook voordoen wanneer alle replica's van een partitie opnieuw opstarten of worden verwijderd.

   - **IdleSecondary InBuild-replica's**: Deze nieuwe replica's die zijn gemaakt door de Cluster Resource Manager of bestaande replica's die is verzonden naar beneden en moeten worden toegevoegd in de set. Deze replica's zijn gemaakt of die zijn gemaakt door de primaire voordat ze kunnen deelnemen aan de replicaset als ActiveSecondary en deelnemen aan quorum bevestiging van bewerkingen.

   - **ActiveSecondary InBuild-replica's**: Deze status is waargenomen in sommige query's. Het is een optimalisatie waarin de replica instellen wordt niet gewijzigd, maar er moet een replica kunnen worden gebouwd. De replica zelf volgt de normale statusovergangen machine (zoals wordt beschreven in de sectie voor replica-rollen).

### <a name="ready-rd"></a>Ready (extern bureaublad)
Een replica gereed is een replica die deel uitmaakt van replicatie en het quorum bevestiging van bewerkingen. De status gereed is van toepassing op de primaire en de actieve secundaire replica's.

Als de toepassingshost of het knooppunt voor een replica gereed vastloopt, verandert deze in de status down.

### <a name="closing-cl"></a>Afsluiten (CL)
Een replica voert de status van de afsluiting in de volgende scenario's:

- **De code voor de replica wordt afgesloten**: Service Fabric mogelijk om de code uitvoeren voor een replica af te sluiten. Hierdoor kan om verschillende redenen zijn. Het kan bijvoorbeeld gebeuren vanwege een toepassing, een fabric of een upgrade van de infrastructuur, of vanwege een fout gerapporteerd door de replica. Wanneer de replica sluit is voltooid, verandert de replica in de status down. De permanente status die is gekoppeld aan deze replica die opgeslagen op schijf zijn niet opgeschoond.

- **De replica verwijderen uit het cluster**: Service Fabric mogelijk voor het verwijderen van de permanente status en de code uitvoeren voor een replica sluit. Hierdoor kan zijn om verschillende redenen, bijvoorbeeld voor taakverdeling.

### <a name="dropped-dd"></a>Verwijderde (DD)
Het exemplaar wordt niet meer in de verwijderde status uitgevoerd op het knooppunt. Er is ook geen status links op het knooppunt. Op dit moment onderhoudt Service Fabric de metagegevens voor dit exemplaar, wordt uiteindelijk ook verwijderd.

### <a name="down-d"></a>Omlaag (D)
De code van de replica wordt niet uitgevoerd in de status down, maar de permanente status voor deze replica bestaat op dat knooppunt. Een replica niet beschikbaar mag zijn om verschillende redenen, bijvoorbeeld, het knooppunt wordt naar beneden, een crash in de code van de replica of een upgrade van de toepassing een replica fouten.

Een omlaag replica is geopend door Service Fabric als vereist, bijvoorbeeld wanneer de upgrade is voltooid op het knooppunt.

De replicarol is niet relevant in de status down.

### <a name="opening-op"></a>Openen (OP)
Een replica omlaag krijgt de status openen wanneer Service Fabric nodig heeft om de replica een back-up opnieuw uit. Deze status kan bijvoorbeeld zijn nadat de upgrade van een code voor de toepassing is op een knooppunt is voltooid. 

Als de toepassingshost of het knooppunt voor een replica openen vastloopt, verandert deze in de status down.

De replicarol is niet relevant in de status geopend.

### <a name="standby-sb"></a>Stand-by (SB)
Een stand-by-replica is een replica van een persistente service die werd afgesloten en vervolgens is geopend. Deze replica kan worden gebruikt door Service Fabric, indien nodig een andere replica toevoegen aan de replicaset (omdat de replica al een gedeelte van de status heeft en het bouwproces sneller is). Nadat de StandByReplicaKeepDuration is verlopen, wordt de standby-replica verwijderd.

Als de toepassingshost of het knooppunt voor een stand-by-replica vastloopt, verandert deze in de status down.

De replicarol is niet relevant in de stand-by-status.

> [!NOTE]
> Alle replica's die niet is niet actief of verbroken wordt beschouwd als *van*.
>

> [!NOTE]
> Het is mogelijk om een overgang van een staat op de status van de verwijderde met behulp van de **ForceRemove** kiezen op `Remove-ServiceFabricReplica`.
>

## <a name="replica-role"></a>Replicarol 
De rol van de replica is bepalend voor de functie in de replicaset:

- **Primaire (P)**: Er is een primaire in de replica die verantwoordelijk is voor het uitvoeren van lees- en schrijfbewerkingen. 
- **ActiveSecondary (S)**: Dit zijn de replica's die zijn ontvangen van updates van de status van de primaire, past deze toe en verzend back bevestigingen. Er zijn meerdere actieve secundaire databases in de replicaset. Het nummer van deze actieve secundaire databases bepaalt het aantal fouten die kan worden verwerkt door de service.
- **IdleSecondary (I)**: Deze replica's zijn gebouwd op de primaire. Ze ontvangen status van de primaire voordat ze kunnen worden gepromoveerd tot de actieve secundaire. 
- **Geen (N)**: Deze replica's geen een verantwoordelijkheid in de replicaset.
- **Onbekend (U)**: Dit is de eerste rol van een replica voordat er een **ChangeRole** API-aanroep van Service Fabric.

Het volgende diagram illustreert de replica rol overgangen en enkele voorbeeld-scenario's waarin ze kunnen optreden:

![Replicarol](./media/service-fabric-concepts-replica-lifecycle/role.png)

- U -> P: Het maken van een nieuwe primaire replica.
- U -> I: Het maken van een nieuwe niet-actieve replica.
- U -> N: Verwijdering van een stand-by-replica.
- I -> S: Promotie van de niet-actieve secundaire naar de actieve secundaire zodat de bevestigingen aan quorum bijgedragen.
- I -> P: Promotie van de niet-actieve secundaire naar primaire. Dit kan gebeuren onder speciale rekening gebracht wanneer de niet-actieve secundaire de juiste kandidaat is voor de primaire.
- I -> N: Verwijdering van de niet-actieve secundaire replica.
- S -> P: Promotie van de actieve secundaire naar primaire. Dit kan zijn vanwege een failover van de primaire of een primaire verplaatsing gestart door de Cluster Resource Manager. Het kan bijvoorbeeld zijn in reactie op een upgrade van de toepassing of taakverdeling.
- S -> N: Verwijdering van de actieve secundaire replica.
- P -> S: Degradatie van de primaire replica. Dit kan zijn als gevolg van een primaire verplaatsingen gestart door de Cluster Resource Manager. Het kan bijvoorbeeld zijn in reactie op een upgrade van de toepassing of taakverdeling.
- P -> N: Verwijdering van de primaire replica.

> [!NOTE]
> Een hoger niveau programmeren modellen, zoals [Reliable Actors](service-fabric-reliable-actors-introduction.md) en [Reliable Services](service-fabric-reliable-services-introduction.md), het concept van rollen van de replica van de ontwikkelaar verbergen. In Actors is het begrip van een rol niet nodig. In de Services, het grotendeels vereenvoudigd voor de meeste scenario's.
>

## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel voor meer informatie over Service Fabric-concepten:

[Levenscyclus van Reliable Services - C#](service-fabric-reliable-services-lifecycle.md)

