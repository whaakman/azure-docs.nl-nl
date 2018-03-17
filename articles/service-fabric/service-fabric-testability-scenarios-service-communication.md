---
title: 'Testbaarheid: Service communicatie | Microsoft Docs'
description: Service to service-communicatie is een kritieke integratie van een Service Fabric-toepassing. Dit artikel worden de overwegingen bij het ontwerpen en testen technieken.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 017557df-fb59-4e4a-a65d-2732f29255b8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: c87b5d82b6eef2b1d28a3280cc2fa07c28084f90
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="service-fabric-testability-scenarios-service-communication"></a>Service Fabric-testbaarheid scenario's: Service-communicatie
Microservices en architectuur stijlen service oriented vlak natuurlijk in Azure Service Fabric. In deze soorten architecturen voor gedistribueerde, samengestelde microservice toepassingen doorgaans bestaan uit meerdere services die nodig zijn om met elkaar te communiceren. In zelfs de meest eenvoudige gevallen over het algemeen hebt u ten minste een stateless webservice en een opslagservice stateful gegevens die nodig zijn om te communiceren.

Service to service-communicatie is een kritieke integratie van een toepassing, omdat elke service een externe API met andere services geeft. Werken met een reeks API grenzen waarbij i/o over het algemeen moet zorgvuldig te werk gaan, met een goede hoeveelheid testen en valideren.

Er zijn talrijke aandachtspunten wanneer deze servicegrenzen samen in een gedistribueerde systeem zijn bekabelde:

* *Transportprotocol*. Wilt u HTTP voor verbeterde interoperabiliteit of een aangepaste binaire protocol gebruiken voor maximale doorvoer?
* *Foutafhandeling*. Hoe wordt permanent en tijdelijke fouten worden verwerkt? Wat gebeurt er wanneer een service wordt verplaatst naar een ander knooppunt?
* *Time-outs en latentie*. In een toepassingen, hoe elke servicelaag omgaan latentie via de netwerkstack en aan de gebruiker?

Of u een van de onderdelen van de ingebouwde service-communicatie geleverd door de Service Fabric gebruiken of u uw eigen tests de interacties tussen uw services bouwen is van cruciaal belang om ervoor te zorgen tolerantie in uw toepassing.

## <a name="prepare-for-services-to-move"></a>Voorbereiden op services verplaatsen
Service-exemplaren kunnen navigeren gedurende een bepaalde periode. Dit geldt met name wanneer ze zijn geconfigureerd met metrische gegevens load balancing optimale resource aangepaste afgestemd. Service Fabric worden verplaatst van uw service-exemplaren de beschikbaarheid ervan maximaliseren zelfs tijdens upgrades, failovers, scale-out en andere situaties die tijdens de levensduur van een gedistribueerde systeem optreden.

Als u services zich in het cluster verplaatsen, moeten uw clients en andere services worden voorbereid voor het afhandelen van twee scenario's wanneer ze met een service communiceren:

* De service-exemplaar of partitie replica is verplaatst sinds de laatste keer dat er aan is besproken. Dit is een normaal onderdeel van de levensduur van een service en er gebeurt tijdens de levensduur van uw toepassing moet worden verwacht.
* De service-exemplaar of partitie replica is bezig te verplaatsen. Hoewel failover van een service van het ene knooppunt naar een andere zeer snel in Service Fabric wordt er mogelijk een vertraging in beschikbaarheid als onderdeel van de communicatie van uw service traag starten.

Afhandeling van deze scenario's zonder problemen is belangrijk voor een systeem vloeiend worden uitgevoerd. Om dit te doen, houd rekening met:

* Elke service waarnaar kan worden verbonden met een *adres* die deze luistert op (bijvoorbeeld HTTP of WebSockets). Als u een service-exemplaar of de partitie worden verplaatst, verandert de adres-eindpunt. (Deze wordt verplaatst naar een ander knooppunt met een ander IP-adres.) Als u de ingebouwde communicatie-onderdelen, worden ze opnieuw het omzetten van adressen van de service voor u verwerken.
* Er is mogelijk een tijdelijke toename in latentie van de service als de service wordt gestart van de listener opnieuw. Dit is afhankelijk van hoe snel de listener door de service wordt geopend nadat het service-exemplaar is verplaatst.
* De bestaande verbindingen moeten worden gesloten en heropend nadat de service is geopend op een nieuw knooppunt. Opnieuw opstarten of afsluiten van de correcte knooppunt kunt de tijd voor bestaande verbindingen met de juiste manier worden afgesloten.

### <a name="test-it-move-service-instances"></a>Het testen: service-exemplaren verplaatsen
Met behulp van Service Fabric-testbaarheid hulpprogramma's, kunt u een Testscenario waarin u deze situaties testen op verschillende manieren maken:

1. Verplaats de primaire replica van een stateful service.
   
    De primaire replica van een partitie stateful service kan worden verplaatst voor een aantal redenen. Gebruik deze optie om de primaire replica van een specifieke partitie om te zien hoe uw services op de verplaatsing reageren op een zeer gecontroleerde manier.
   
    ```powershell
   
    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService
   
    ```
2. Een knooppunt stoppen.
   
    Wanneer een knooppunt is gestopt, worden alle service-exemplaren of partities die zich op dat knooppunt naar een van de andere beschikbare knooppunten in het cluster Service Fabric verplaatst. Gebruik dit voor het testen van een situatie waarbij een knooppunt van het cluster en alle van de service-exemplaren verloren gaan en replica's op dat knooppunt moeten verplaatsen.
   
    U kunt een knooppunt met behulp van de PowerShell stoppen **Stop ServiceFabricNode** cmdlet:
   
    ```powershell
   
    PS > Stop-ServiceFabricNode -NodeName Node_1
   
    ```

## <a name="maintain-service-availability"></a>Beschikbaarheid van Services onderhouden
Service Fabric is ontworpen als een platform voor maximale beschikbaarheid van uw services. Maar in uitzonderlijke gevallen onderliggende infrastructuurproblemen kunnen nog steeds niet beschikbaar zijn. Het is belangrijk te testen voor deze scenario's.

Stateful services gebruiken een quorum-systeem voor replicatie van de status voor hoge beschikbaarheid. Dit betekent dat een quorum van replica's beschikbaar zijn moet voor het uitvoeren van schrijfbewerkingen. In zeldzame gevallen, zoals een wijdverbreid hardwarefout een quorum van replica's mogelijk niet beschikbaar. In dergelijke gevallen worden geen schrijfbewerkingen uitvoeren, maar worden nog steeds leesbewerkingen uitvoeren.

### <a name="test-it-write-operation-unavailability"></a>Het testen: bewerking onbeschikbaarheid schrijven
Met de testbaarheid hulpprogramma's in Service Fabric, kunt u een fout die quorumverlies als test induceert invoeren. Hoewel dit scenario zeldzame, is het belangrijk dat clients en -services die afhankelijk van een stateful service zijn zijn voorbereid voor het afhandelen van situaties waar zij aanvragen schrijven kunnen niet maken. Het is ook belangrijk dat de stateful service zelf op de hoogte van deze mogelijkheid is en kan probleemloos voor aanroepers communiceren.

U kunt quorumverlies veroorzaken met behulp van de PowerShell **Invoke-ServiceFabricPartitionQuorumLoss** cmdlet:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

In dit voorbeeld wordt ingesteld `QuorumLossMode` naar `QuorumReplicas` om aan te geven dat we quorumverlies willen zonder omlaag alle replica's veroorzaken. Op deze manier leesbewerkingen zijn nog steeds mogelijk. Als u wilt testen in een scenario waarbij een volledige partitie niet beschikbaar is, kunt u deze switch instellen op `AllReplicas`.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over testbaarheid acties](service-fabric-testability-actions.md)

[Meer informatie over testbaarheid scenario 's](service-fabric-testability-scenarios.md)

