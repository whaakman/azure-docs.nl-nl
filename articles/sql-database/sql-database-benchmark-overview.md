---
title: Overzicht van Azure SQL Database-benchmark
description: Dit onderwerp beschrijft de Azure SQL Database Benchmark in het meten van de prestaties van Azure SQL Database gebruikt.
services: sql-database
documentationcenter: na
author: jan-eng
manager: jhubbard
editor: monicar
ms.assetid: e26f8a66-2c12-49d7-8297-45b4d48a5c01
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 06/21/2016
ms.author: janeng
ms.openlocfilehash: fb8a5f205ddc143dc47349829048f46f88963d05
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="azure-sql-database-benchmark-overview"></a>Overzicht van Azure SQL Database-benchmark
## <a name="overview"></a>Overzicht
Microsoft Azure SQL Database biedt drie [Servicelagen](sql-database-service-tiers.md) met meerdere prestatieniveaus. Elke prestatieniveau biedt een toenemende verzameling resources of power, ontworpen voor steeds meer hogere doorvoer.

Het is belangrijk om te bepalen hoe de toenemende macht van elke prestatieniveau zet in verbeterde databaseprestaties. U doet dit Microsoft heeft ontwikkeld Azure SQL Database Benchmark (ASDB). De benchmark oefent een combinatie van basisbewerkingen gevonden in alle OLTP-werkbelastingen. We meten van de doorvoer voor databases die worden uitgevoerd op elk prestatieniveau worden gerealiseerd.

De resources en de kracht van elke prijscategorie en prestatieniveau serviceniveau worden uitgedrukt in termen van ['s (Database Transaction Units)](sql-database-what-is-a-dtu.md). Dtu's bieden een manier om te beschrijven de relatieve capaciteit van een prestatieniveau op basis van een gecombineerde meting van CPU, geheugen, en lezen en schrijven tarieven die worden aangeboden door elke prestatieniveau. De DTU-classificatie van een database verdubbeling is gelijk aan verdubbeling van het vermogen van de database. De benchmark kunt u de impact op de prestaties van de database van de toenemende macht aangeboden op elk prestatieniveau door de werkelijke databasebewerkingen, die tijdens het schalen van de grootte van de database, het aantal gebruikers, beoordelen en transactie classificeert in verhouding staan tot de resources die worden geleverd met de database.

Doordat de doorvoer van de Basic servicelaag met behulp van transacties per uur, de standaard servicelaag met behulp van transacties per minuut en de Premium servicecategorie met behulp van transacties per seconde, is het eenvoudiger om snel de prestaties de mogelijkheden van elke servicelaag met de vereisten van een toepassing.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Benchmarkresultaten werkelijkheid databaseprestaties correleren
Het is belangrijk te begrijpen dat ASDB, zoals alle benchmarks representatief en indicatief alleen. Deze transactie worden verkregen met de toepassing benchmark niet hetzelfde als die kunnen worden behaald met andere toepassingen. De benchmark bestaat uit een verzameling van een andere transactie typen uitvoeren op een schema met een bereik van tabellen en gegevenstypen. Terwijl de benchmark de dezelfde basisbewerkingen die gemeenschappelijk voor alle OLTP-werkbelastingen oefent zijn, vormt een specifieke klasse van de database of de toepassing niet meer. Het doel van de benchmark is om een redelijke handleiding voor de relatieve prestaties van een database die kan worden verwacht voor het omhoog of omlaag schalen tussen prestatieniveaus. Verschillende combinaties van werkbelastingen optreden en reageert op verschillende manieren in werkelijkheid databases zijn van verschillende grootte en complexiteit. Bijvoorbeeld, een i/o-intensieve toepassingen mogelijk sneller i/o-drempelwaarden bereikt of een toepassing CPU-intensief CPU-limieten sneller mogelijk bereikt. Er is geen garantie dat een bepaalde database kunnen worden geschaald op dezelfde manier als de benchmark onder een toenemende belasting.

De benchmark en de methoden worden hieronder in detail beschreven.

## <a name="benchmark-summary"></a>Samenvatting van de benchmark
ASDB meet de prestaties van een combinatie van basic databasebewerkingen waarvoor een treden het vaakst in online transactieverwerking (OLTP)-workloads. Hoewel de benchmark is ontworpen met cloud computing in gedachten, het databaseschema, vullen met gegevens en transacties zijn ontworpen voor grote schaal representatief zijn voor de basiselementen meest worden gebruikt in een OLTP-werkbelastingen.

## <a name="schema"></a>Schema
Het schema is ontworpen om voldoende diverse en complexiteit ter ondersteuning van een breed scala aan bewerkingen. De benchmark wordt uitgevoerd op basis van een database bestaat uit zes tabellen. De tabellen die zijn onderverdeeld in drie categorieën: vaste grootte, schaalbaarheid en groeit. Er zijn twee tabellen van vaste grootte. drie vergroten/verkleinen tabellen. en één groeiende tabel. Vaste grootte tabellen hebben een constant aantal rijen. Schalen tabellen hebben een kardinaliteit die evenredig is met de prestaties van de database is, maar niet wijzigen tijdens de benchmark. De groeiende tabel wordt aangepast als een vergroten/verkleinen tabel op de eerste keer wordt geladen, maar de kardinaliteit wijzigingen tijdens het uitvoeren van de benchmark als rijen worden ingevoegd of verwijderd.

Het schema bevat een combinatie van gegevenstypen, met inbegrip van geheel getal, numerieke tekens en datum/tijd. Het schema bevat primaire en secundaire sleutels, maar geen refererende sleutels - dat wil zeggen, er zijn geen referentiële integriteitsbeperkingen tussen tabellen.

Een programma van de generatie gegevens genereert de gegevens voor de oorspronkelijke database. Geheel getal en numerieke gegevens wordt met verschillende strategieën gegenereerd. In sommige gevallen worden waarden willekeurig verdeeld over een bereik. In andere gevallen is een set waarden is willekeurig gepermuteerde om ervoor te zorgen dat een specifiek distributiepunt behouden blijft. Tekstvelden worden gegenereerd vanuit een lijst met woorden om naar te realistisch ogende gegevens produceren.

De database wordt aangepast op basis van een 'schaalfactor'. De schaalfactor (afgekort als SF) bepaalt de kardinaliteit van de schaal en groei van tabellen. Zoals hieronder wordt beschreven in de sectie gebruikers en Pacing, de grootte van de database, aantal gebruikers en de maximale prestaties alle schalen in verhouding tot elkaar.

## <a name="transactions"></a>Transacties
De werkbelasting bestaat uit negen transactietypen, zoals weergegeven in de onderstaande tabel. Elke transactie is ontworpen voor het markeren van een bepaalde set kenmerken in de database-engine en system hardware, met een hoog contrast uit de andere transacties. Deze aanpak gemakkelijker om de impact van de verschillende onderdelen van de algehele prestaties vast te stellen. De transactie 'Zware lezen' levert bijvoorbeeld een groot aantal leesbewerkingen van de schijf.

| Transactietype | Beschrijving |
| --- | --- |
| Lite lezen |SELECTEER; in het geheugen; alleen-lezen |
| Gemiddeld lezen |SELECTEER; voornamelijk in het geheugen; alleen-lezen |
| Lees zware |SELECTEER; meestal niet in het geheugen; alleen-lezen |
| Update Lite |UPDATE; in het geheugen; lezen / schrijven |
| Zware bijwerken |UPDATE; meestal niet in het geheugen; lezen / schrijven |
| INSERT Lite |INSERT; in het geheugen; lezen / schrijven |
| Zware invoegen |INSERT; meestal niet in het geheugen; lezen / schrijven |
| Verwijderen |VERWIJDEREN; combinatie van in het geheugen en niet in het geheugen; lezen / schrijven |
| CPU-zware |SELECTEER; in het geheugen; relatief intensief CPU-belasting; alleen-lezen |

## <a name="workload-mix"></a>Combinatie van de werkbelasting
Transacties worden willekeurig geselecteerd uit een gewogen distributiepunt met de volgende algemene combinatie. De algehele mix heeft een verhouding lezen/schrijven van ongeveer 2:1.

| Transactietype | % van de combinatie |
| --- | --- |
| Lite lezen |35 |
| Gemiddeld lezen |20 |
| Lees zware |5 |
| Update Lite |20 |
| Zware bijwerken |3 |
| INSERT Lite |3 |
| Zware invoegen |2 |
| Verwijderen |2 |
| CPU-zware |10 |

## <a name="users-and-pacing"></a>Gebruikers- en pacing
De benchmark werkbelasting wordt aangedreven vanuit een hulpprogramma waarmee transacties worden verzonden over een set van verbindingen met het gedrag van een aantal gelijktijdige gebruikers simuleren. Hoewel alle verbindingen en transacties machine gegenereerd zijn, voor het gemak noemen we deze verbindingen 'gebruikers'. Hoewel elke gebruiker onafhankelijk van alle andere gebruikers werkt, wordt in alle gebruikers de dezelfde cyclus van de onderstaande stappen uitvoeren:

1. Een databaseverbinding tot stand brengen.
2. Herhaal totdat een signaal ontvangen om af te sluiten:
   * Selecteer een transactie in willekeurige volgorde (uit een gewogen distributiepunt).
   * De geselecteerde transactie uitvoeren en de reactietijd meten.
   * Wachten op een pacing vertraging.
3. Sluit de verbinding met de database.
4. Afsluiten.

De pacing vertraging (in stap 2c) willekeurig is geselecteerd, maar met een distributiepunt die een gemiddelde van tweede 1.0 heeft. Elke gebruiker kan dus gemiddeld genereren voor maximaal één transactie per seconde.

## <a name="scaling-rules"></a>Regels voor vergroten/verkleinen
Het aantal gebruikers wordt bepaald door de grootte van de database (in schaalfactor units). Er is een gebruiker voor elke vijf schaalfactor eenheden. Vanwege de pacing vertraging één gebruiker gemiddeld maximaal één transactie per seconde genereren.

Bijvoorbeeld, een scale-factor van 500 (SF = 500)-database heeft 100 gebruikers en een maximale snelheid van 100 TPS kunt bereiken. Om een hogere TPS vereist snelheid meer gebruikers en een grotere database.

De onderstaande tabel ziet het aantal gebruikers daadwerkelijk aanhoudend voor elke prijscategorie en prestatieniveau serviceniveau.

| Servicelaag (prestatieniveau) | Gebruikers | Databaseomvang |
| --- | --- | --- |
| Basic |5 |720 MB |
| Standard (S0) |10 |1 GB |
| Standard (S1) |20 |2.1 GB |
| Standard (S2) |50 |7.1 GB |
| Premium (P1) |100 |14 GB |
| Premium (P2) |200 |28 GB |
| Premium (P6) |800 |114 GB |

## <a name="measurement-duration"></a>Duur van de meting
Een geldig benchmark uitvoeren vereist een actieve status meting duur van ten minste één uur.

## <a name="metrics"></a>Metrische gegevens
De belangrijkste metrische gegevens in de benchmark zijn doorvoer en reactietijd.

* Doorvoer is de essentiële prestatiemeting die in de benchmark. Doorvoer wordt vermeld in transacties per eenheid-van-time, waarbij alle transactietypen wordt geteld.
* Reactietijd is een meting van prestaties, voorspelbaarheid. De beperking van de tijd antwoord verschillen afhankelijk van de klasse van de service met hogere klassen van de service met een strengere antwoord tijd vergt, zoals hieronder wordt weergegeven.

| Klasse van Service | Doorvoer meting | Antwoord tijd vergt |
| --- | --- | --- |
| Premium |Transacties per seconde |95e percentiel op 0,5 seconden |
| Standard |Transacties per minuut |90 percentiel op 1,0 seconden |
| Basic |Transacties per uur |80e percentiel op 2.0 seconden |

## <a name="conclusion"></a>Conclusie
De Azure SQL Database-Benchmark meet de relatieve prestaties van Azure SQL Database uitvoert via het bereik van beschikbare Servicelagen en prestatieniveaus. De benchmark oefent een combinatie van basic databasebewerkingen waarvoor een treden het vaakst in online transactieverwerking (OLTP)-workloads. Door het meten van de werkelijke prestaties, biedt de benchmark een duidelijker beoordeling van de impact op de doorvoer van het wijzigen van het prestatieniveau dan mogelijk is door alleen de resources die worden geleverd op elk niveau zoals CPU-snelheid, geheugengrootte en IOP's aan te bieden. Er wordt in de toekomst nog de benchmark in het bereik vergroten en de gegevens die zijn opgegeven om te ontwikkelen.

## <a name="resources"></a>Resources
[Inleiding tot SQL Database](sql-database-technical-overview.md)

[Servicelagen en prestatieniveaus](sql-database-service-tiers.md)

[Prestaties richtlijnen voor individuele databases](sql-database-performance-guidance.md)
