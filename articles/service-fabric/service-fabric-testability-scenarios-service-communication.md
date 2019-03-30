---
title: 'Testbaarheid: Communicatie-service | Microsoft Docs'
description: Service-naar-servicecommunicatie is een kritieke integratie van een Service Fabric-toepassing. In dit artikel wordt beschreven overwegingen bij het ontwerpen en testen technieken.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 017557df-fb59-4e4a-a65d-2732f29255b8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 529c8d74b6e0a63a7969f31d5b5e8073ecb79411
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665743"
---
# <a name="service-fabric-testability-scenarios-service-communication"></a>Service Fabric testbaarheidsscenario's: Servicecommunicatie
Microservices en servicegerichte architectuurstijlen voor aanvallen op een natuurlijke manier in Azure Service Fabric. In deze typen van de gedistribueerde architectuur van samengestelde microservicetoepassingen doorgaans bestaan uit meerdere services die nodig zijn om te communiceren met elkaar. In zelfs de eenvoudigste gevallen hebt u in het algemeen ten minste een stateless webservice en een stateful data storage-service die nodig hebt om te communiceren.

Service-naar-servicecommunicatie is een kritieke integratiepunt van een toepassing, omdat elke service een externe API met andere services beschikbaar stelt. Werken met een set API grenzen die in het algemeen i/o omvat vereist zorgvuldig te werk gaan, met een goede hoeveelheid testen en valideren.

Er zijn talrijke overwegingen bij de servicegrenzen van deze samen in een gedistribueerd systeem zijn bekabelde:

* *Transportprotocol*. U gebruikt HTTP voor verbeterde interoperabiliteit, of een aangepaste binaire protocol voor een maximale doorvoer?
* *Foutafhandeling*. Hoe wordt permanent en tijdelijke fouten worden verwerkt? Wat gebeurt er wanneer een service wordt verplaatst naar een ander knooppunt?
* *Time-outs en latentie*. In een toepassingen, hoe elke servicelaag zorgt latentie via de stack en aan de gebruiker?

Of u een van de onderdelen van de ingebouwde service-communicatie geleverd door de Service Fabric gebruiken of u uw eigen, het testen van de interacties tussen uw services bouwen is van essentieel belang aan het waarborgen van tolerantie in uw toepassing.

## <a name="prepare-for-services-to-move"></a>Voorbereiden op services te verplaatsen
Service-exemplaren kunnen na verloop van tijd navigeren. Dit is vooral van toepassing wanneer ze zijn geconfigureerd met metrische gegevens laden voor optimale resource maat taakverdeling. Service Fabric wordt uw service-instanties voor een maximale hun beschikbaarheid, zelfs tijdens upgrades, failover, uitbreidbare en andere situaties die tijdens de levensduur van een gedistribueerd systeem optreden verplaatst.

Als de services worden verplaatst in het cluster, moeten uw clients en andere services worden voorbereid voor het afhandelen van twee scenario's wanneer ze met een service communiceren:

* De service-exemplaar of de partitie-replica is verplaatst sinds de laatste keer dat je het toe. Dit is een normaal onderdeel van de levenscyclus van een service en deze worden uitgevoerd tijdens de levensduur van uw toepassing moet worden verwacht.
* De service-exemplaar of de partitie-replica is verplaatst. Hoewel de failover van een service van het ene knooppunt naar een andere zeer snel in Service Fabric optreedt, kunnen er een vertraging in de beschikbaarheid als onderdeel van de communicatie van uw service langzaam om te starten.

Verwerken van deze scenario's zonder problemen is belangrijk voor een systeem vloeiend worden uitgevoerd. Om dit te doen, houd rekening met het volgende:

* Heeft elke service die kan worden verbonden met een *adres* waarop deze luistert (bijvoorbeeld http- of WebSockets). Als een service-exemplaar of de partitie worden verplaatst, wordt het eindpunt adres verandert. (Wordt verplaatst naar een ander knooppunt met een ander IP-adres.) Als u de ingebouwde communicatie-onderdelen, zorgt ze opnieuw omzetten-mailadressen voor u.
* Er is mogelijk een tijdelijke toename in latentie-service als de service wordt gestart van de listener opnieuw. Dit is afhankelijk van hoe snel de listener door de service wordt geopend nadat het service-exemplaar is verplaatst.
* Bestaande verbindingen moeten worden gesloten en heropend nadat de service op een nieuw knooppunt wordt geopend. Opnieuw opstarten of afsluiten van de correcte knooppunt kunt tijd voor bestaande verbindingen zonder problemen worden afgesloten.

### <a name="test-it-move-service-instances"></a>Testen: Verplaatsen van service-exemplaren
Met behulp van Service Fabric testbaarheid hulpprogramma's, kunt u een test-scenario waarin u deze situaties testen op verschillende manieren maken:

1. De primaire replica van een stateful service verplaatsen.
   
    De primaire replica van de servicepartitie van een stateful kan worden verplaatst om verschillende redenen. Gebruik deze optie om de primaire replica van een specifieke partitie om te zien hoe uw services op de verplaatsing reageren op een zeer gecontroleerde manier.
   
    ```powershell
   
    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService
   
    ```
2. Een knooppunt stoppen.
   
    Wanneer een knooppunt wordt gestopt, worden Service Fabric verplaatst van de service-exemplaren of partities die op dat knooppunt aan een van de andere beschikbare knooppunten in het cluster zijn. Gebruik deze optie voor het testen van een situatie waarbij een knooppunt verloren is gegaan van uw cluster en alle van de service-exemplaren en replica's op dat knooppunt moeten verplaatsen.
   
    U kunt een knooppunt stoppen met behulp van de PowerShell **Stop-ServiceFabricNode** cmdlet:
   
    ```powershell
   
    PS > Stop-ServiceFabricNode -NodeName Node_1
   
    ```

## <a name="maintain-service-availability"></a>Service-beschikbaarheid
Als een platform, worden Service Fabric is ontworpen voor maximale beschikbaarheid van uw services. Maar in uitzonderlijke gevallen, problemen met de onderliggende netwerkinfrastructuur kunnen nog steeds niet beschikbaar zijn. Het is belangrijk om te testen voor deze scenario's te.

Een systeem op basis van een quorum stateful services gebruiken voor het repliceren van de status voor hoge beschikbaarheid. Dit betekent dat een quorum van replica's beschikbaar zijn moet voor het uitvoeren van bewerkingen voor schrijven. In zeldzame gevallen, zoals een wijdverbreide hardware-uitval, een quorum van replica's mogelijk niet beschikbaar. In dergelijke gevallen kunt u zich geen schrijfbewerkingen uitvoeren, maar kunt u zich nog steeds meer bewerkingen uitvoeren.

### <a name="test-it-write-operation-unavailability"></a>Testen: Schrijven van de bewerking niet beschikbaar zijn
Met behulp van de hulpprogramma's voor testbaarheid in Service Fabric, voert u een fout die quorumverlies als een test induceert. Hoewel dit scenario zeldzaam is, is het belangrijk dat clients en services die afhankelijk van een stateful service zijn bent voorbereid op het verwerken van situaties waarin ze schrijfaanvragen naar deze kunnen niet maken. Het is ook belangrijk dat de stateful service zelf op de hoogte van deze mogelijkheid is en kan zonder problemen voor bellers communiceren.

U kunt quorumverlies veroorzaken met behulp van de PowerShell **Invoke-ServiceFabricPartitionQuorumLoss** cmdlet:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

In dit voorbeeld stellen we `QuorumLossMode` naar `QuorumReplicas` om aan te geven dat we quorumverlies willen zonder uitschakelt alle replica's veroorzaken. Op deze manier leesbewerkingen zijn nog steeds mogelijk. Als u wilt testen in een scenario waarbij een hele partitie niet beschikbaar is, kunt u deze switch instellen op `AllReplicas`.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over testbaarheidsacties](service-fabric-testability-actions.md)

[Meer informatie over testbaarheidsscenario 's](service-fabric-testability-scenarios.md)

