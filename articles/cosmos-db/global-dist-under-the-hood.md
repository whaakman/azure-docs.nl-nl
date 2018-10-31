---
title: Azure DB Cosmos globale distributie - achter de schermen
description: In dit artikel vindt u technische gegevens met betrekking tot wereldwijde distributie van Azure Cosmos DB
services: cosmos-db
author: dharmas-cosmos
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: 21464ccfbd5712b18e46a271a93232dc3ba7d3c8
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244078"
---
# <a name="global-distribution---under-the-hood"></a>Wereldwijde distributie - achter de schermen

Azure Cosmos DB is een fundamentele service van Azure, zodat deze alle Azure-regio's over de hele wereld, waaronder het publiek, onafhankelijke, ministerie van defensie (DoD) en clouds voor de overheid geïmplementeerd. Binnen een datacenter, we implementeren en beheren van de Azure Cosmos DB-service op enorme 'tijdstempels' van machines, elk met lokale opslag toegewezen. Binnen een datacenter, is Azure Cosmos DB geïmplementeerd op veel clusters is elk mogelijk meerdere verschillende hardwaregeneraties. Machines binnen een cluster worden doorgaans verdeeld over foutdomeinen van 10 tot 20.

![Topologie](./media/global-dist-under-the-hood/distributed-system-topology.png)
 **-topologie**

Wereldwijde distributie in Azure Cosmos DB is een gebruiksklare: op elk gewenst moment, met een paar klikken of via een programma met één API-aanroep klant kunt toevoegen of verwijderen van de geografische regio's die zijn gekoppeld aan hun Cosmos-database. Een Cosmos-database wordt op zijn beurt bestaat uit een verzameling van Cosmos-containers. In Cosmos DB fungeren containers als de logische eenheden van de distributie en schaalbaarheid. De verzamelingen, tabellen en grafieken die u maakt zijn (intern) alleen Cosmos-containers. Containers zijn volledig schema-agnostische en geef een bereik voor een query. Alle gegevens in een Cosmos-container automatisch geïndexeerd bij opname. Automatische indexering, kunnen gebruikers de gegevens op te vragen zonder dat u hoeft te bekommeren om het schema of gedoe van indexbeheer, met name in een wereldwijd gedistribueerde installatie.  

Zoals weergegeven in de volgende afbeelding, worden de gegevens in een container worden gedistribueerd in twee dimensies:  

- In een bepaalde regio, worden gegevens in een container gedistribueerd met behulp van een partitiesleutel, die u opgeeft en is transparant beheerd met behulp van de onderliggende resourcepartities (lokale distributie).  
- Elke resourcepartitie wordt ook gerepliceerd in geografische regio's (wereldwijde distributie). 

Wanneer een app met Cosmos DB elastisch schaalt de doorvoer (of meer opslagruimte verbruikt) voor een container van Cosmos, Cosmos DB transparant bewerkingen verwerkt (splitsen, klonen en verwijderen) in alle regio's. Onafhankelijk van de schaal, distributie of storingen, Cosmos DB altijd één integraal beeld van de gegevens in de containers die wereldwijd worden gedistribueerd naar een willekeurig aantal regio's bieden.  

![Resourcepartities](./media/global-dist-under-the-hood/distribution-of-resource-partitions.png)
**verdeling van de Resourcepartities**

Fysiek, wordt een resourcepartitie geïmplementeerd door een groep van replica's met de naam van een replica-set. Elke machine als host fungeert voor honderden replica's die overeenkomt met de verschillende resourcepartities binnen een vaste set van processen, zoals wordt weergegeven in de vorige afbeelding. Replica's die overeenkomt met de resourcepartities dynamisch worden geplaatst en taakverdeling tussen de computers binnen een cluster en datacentrums binnen een regio.  

Een replica wordt een unieke behoort tot een Azure Cosmos DB-tenant. Elke replica als host fungeert voor een exemplaar van de Cosmos-DB [database-engine](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), die de resources, evenals de bijbehorende indexen beheert. De database-engine van Cosmos DB is van invloed op een atom-record-sequence ' (ARS) gebaseerde typesysteem. De engine is agnostisch ten opzichte van het concept van een schema en de grens tussen de structuur en het exemplaar van de waarden van records vervagen. Cosmos DB realiseert volledige schema agnosticism door alles bij opname automatisch te indexeren op een efficiënte manier, waardoor gebruikers kunnen hun wereldwijd gedistribueerde gegevens op te vragen zonder dat u hoeft te bekommeren om schema's of indexbeheer.

De Cosmos DB-database-engine bestaat uit onderdelen implementatie van meerdere coördinatie primitieven, talen, runtimes, de queryprocessor en de opslag en subsystemen die verantwoordelijk is voor transactionele storage indexeren en het indexeren van gegevens, inclusief respectievelijk. Voor duurzaamheid en hoge beschikbaarheid, de database-engine zich blijft voordoen de gegevens en index op SSD's en repliceert dit met de database-engine-exemplaren in de replica-vormen respectievelijk. Grotere tenants komen overeen met grotere schalen van doorvoer en opslag en grotere of meer aantal replica's of beide hebben. Elk onderdeel van het systeem is volledig asynchroon: Er is geen thread blokkeert ooit en elke thread werkt tijdelijke zonder dat er geen onnodige thread-switches. Frequentielimiet en back-druk zijn in de hele stack van het toegangsbeheer tot alle i/o-paden gekoppeld. Onze database-engine is ontworpen om misbruik te maken van uiterst gedetailleerde gelijktijdigheid en leveren van hoge doorvoer tijdens binnen frugal veel systeembronnen.

Cosmos DB globale distributie is afhankelijk van twee belangrijke abstracties: -replicasets en partitie-sets. Een replica-set is een modulaire Lego-blok voor coördinatie en een partitie-set is een dynamische overlay van een of meer resourcepartities voor geografisch verspreide. Om te begrijpen hoe wereldwijde distributie werkt, moeten we deze twee belangrijke abstracties begrijpen. 

## <a name="replica-sets"></a>Replicasets

Een resourcepartitie is gerealiseerde als een groep zelf worden beheerd en dynamische taakverdeling van replica's die zijn verdeeld over meerdere foutdomeinen, met de naam van een replica-set. Deze set implementeert gezamenlijk het protocol van de machine gerepliceerde status om de gegevens in de resourcepartitie maken maximaal beschikbare, duurzame en consistent. Het lidmaatschap van de replicaset N is dynamisch – dit houdt wisselt tussen Nmin en nmax op basis van de fouten, administratieve bewerkingen en de tijd voor mislukte replica's opnieuw genereren/herstellen. Op basis van de wijzigingen in het lidmaatschap, de replicatie-protocol ook configureren de grootte van lezen en schrijven quorum. Als u wilt distribueren op uniforme wijze de doorvoer die is toegewezen aan een bepaalde bron-partitie, gebruiken we twee ideeën: eerst de kosten van het verwerken van de schrijfaanvragen op de leider is hoger dan de kosten van het toepassen van de updates op de toepassingsgateway beschreven. Dienovereenkomstig, de leider is gebudgetteerde meer systeembronnen dan de Volgers. Ten tweede zoveel mogelijk het quorum lezen voor een bepaalde consistentieniveau bestaat uitsluitend uit de hobbyist replica's. Vermijden we contact opnemen met de leider voor het uitvoeren van leesbewerkingen, tenzij dit vereist. We maken gebruik van een aantal ideeën van het onderzoek uitgevoerd op de relatie van [laden en -capaciteit](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) in de quorum-systemen voor de vijf consistentie die modellen Cosmos DB biedt ondersteuning voor.  

## <a name="partition-sets"></a>Partitie-sets

Een groep van resourcepartities, één van elk van de geconfigureerde met de regio's van Cosmos-database bestaat voor het beheren van dezelfde set sleutels die zijn gerepliceerd naar alle geconfigureerde regio's. Deze hogere coördinatie primitieve heet een partitie-set - een geografisch gedistribueerde dynamische overlay van resourcepartities die een bepaalde set sleutels beheren. Terwijl de partitie van een opgegeven resource (een replica-set) heeft een bereik binnen een cluster, een partitie-set clusters, kan omvatten-datacenters en geografische regio's zoals weergegeven in de volgende afbeelding:  

![Partitiesets](./media/global-dist-under-the-hood/dynamic-overlay-of-resource-partitions.png)
**partitie-set is een dynamische overlay van resourcepartities**

U kunt een partitie-set beschouwen als een geografisch verspreide "super-replicaset', die bestaat uit meerdere replicasets die eigenaar is dezelfde set sleutels. Net als bij een replica-set, lidmaatschap van een partitie-set is ook dynamisch – het fluctueert op basis van de impliciete resourcebewerkingen voor nieuwe partities naar/van een bepaalde partitie-set toevoegen/verwijderen (bijvoorbeeld wanneer u opwaarts schalen doorvoer op een container toevoegen/verwijderen een regio met uw Cosmos-database, of wanneer er fouten optreden) omdat met elk van de partities (van een partitie-set) het lidmaatschap van de partitie is ingesteld in een eigen replicaset beheren, het lidmaatschap is volledig gedecentraliseerde en maximaal beschikbaar. Tijdens de herconfiguratie van een partitie-set, de topologie van de overlay tussen resourcepartities is ook tot stand gebracht. De topologie wordt dynamisch geselecteerd op basis van consistentieniveau, de geografische afstand en de beschikbare netwerkbandbreedte tussen de bron en de doel-resource-partities.  

De service kunt u uw Cosmos-databases configureren met een enkele schrijfregio of meerdere regio's voor schrijven en afhankelijk van uw keuze, partitie-sets zijn geconfigureerd voor het accepteren van schrijfbewerkingen in exact één of alle regio's. Het systeem heeft een twee niveaus, geneste consensus-protocol: één niveau werkt binnen de replica's van een replica-set van een resourcepartitie voor het accepteren van de schrijfbewerkingen en de andere werkt op het niveau van een partitie-is ingesteld op voltooid bestellen garanties bieden voor alle de vastgelegde schrijfbewerkingen binnen de partitie-set. Deze consensus met meerdere lagen, geneste is essentieel voor de implementatie van onze strengere Sla's voor hoge beschikbaarheid, evenals de uitvoering van de consistentiemodellen, Cosmos DB biedt aan klanten.  

## <a name="conflict-resolution"></a>Conflictoplossing

Onze ontwerp voor het doorgeven van de update, conflictoplossing en oorzakelijke bijhouden is laat u inspireren van de voorafgaande werkzaamheden op [epizoötie algoritmen](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) en de [Bayou](http://zoo.cs.yale.edu/classes/cs422/2013/bib/terry95managing.pdf) system. Terwijl de kernels ideeën hebt hebt en een handige referentiekader bieden voor de communicatie van de Cosmos-DB-systeemontwerp, hebben ze ook aanzienlijke transformatie ondergaan omdat we deze op het systeem Cosmos DB toegepast. Dit is nodig omdat de vorige systemen zijn ontworpen met het resourcebeheer noch met de Cosmos DB die moet werken, noch de mogelijkheden (bijvoorbeeld, gebonden veroudering consistentie) en de strenge en biedt uitgebreide schaal Sla's de Cosmos DB zijn klanten biedt.  

Intrekken dat een partitie-set is verdeeld over meerdere regio's en Cosmos DB's (meerdere masters) replicatie protocol om te repliceren van de gegevens tussen de resourcepartities die bestaat uit een bepaalde partitie-set volgt. Elke resourcepartitie (van een partitie-set) accepteert schrijfbewerkingen en leesbewerkingen doorgaans dient aan de clients die lokaal voor deze regio zijn. Schrijfbewerkingen geaccepteerd door de resourcepartitie van een binnen een regio zijn blijvend doorgevoerd en is maximaal beschikbaar zijn in de resourcepartitie gemaakt voordat ze naar de client worden bevestigd. Deze voorlopig schrijfbewerkingen en zijn doorgegeven aan andere resourcepartities binnen de partitie-set met behulp van een kanaal anti entropie. Clients kunnen voorlopig of vastgelegde schrijfbewerkingen aanvragen door een aanvraagheader te verwerken. Het anti entropie doorgeven (met inbegrip van de frequentie van doorgifte) is dynamisch, op basis van de topologie van de partitie-set, regionale nabijheid van de resourcepartities en de consistentie niveau geconfigureerd. Binnen een partitie-set volgt Cosmos DB een primaire commit-schema met een partitie dynamisch geselecteerde instellingen. De instellingen-selectie is dynamische en is een integraal onderdeel van de nieuwe configuratie van de partitie-set op basis van de topologie van de overlay. De vastgelegde schrijfbewerkingen (inclusief meerdere-row/batchgewijs updates) worden worden besteld gegarandeerd. 

We maken gebruik van gecodeerde vector klokken (regio-ID en de logische klokken die overeenkomt met elk niveau van de consensus op de replica-set en partitie-set met respectievelijk) voor oorzakelijke bijhouden en versie vectoren te detecteren en oplossen van conflicten bijwerken. De topologie en de algoritme van de peer-selectie zijn ontworpen om te controleren of de vaste en minimale-opslag en netwerk minimale overhead van vectoren. Het algoritme voor de eigenschap strikte convergentie wordt gegarandeerd.  

Voor de Cosmos-databases is geconfigureerd met meerdere regio's voor schrijven en biedt het systeem een aantal flexibele automatische conflict resolutie beleidsregels voor de ontwikkelaars om de verkeersbelasting, met inbegrip van: 

- Laatste schrijven Wins (LWW) die maakt standaard gebruik van een systeem gedefinieerde tijdstempeleigenschap (die is gebaseerd op het clock voor tijdsynchronisatie protocol). Cosmos DB kunt u een andere aangepaste numerieke eigenschap moet worden gebruikt voor conflictoplossing opgeven.  
- Toepassing gedefinieerde aangepaste conflict resolutie beleid (uitgedrukt via samenvoegen procedures) die is ontworpen voor de toepassing gedefinieerde semantiek afstemming van conflicten. Deze procedures ophalen aangeroepen tijdens de detectie van het schrijven / schrijven veroorzaakt een conflict in het kader van een databasetransactie op de server. Het systeem biedt exact één keer voor het uitvoeren van een procedure samenvoegen als onderdeel van het protocol toezegging garanderen. Er zijn diverse voorbeelden beschikbaar zijn voor u af te spelen met.  

## <a name="consistency-models"></a>Consistentiemodellen

Of u uw Cosmos-database met één of meerdere regio's voor schrijven configureren, kunt u kiezen uit vijf duidelijk gedefinieerde consistentiemodellen. Met de zojuist toegevoegde ondersteuning voor het inschakelen van meerdere regio's voor schrijven, volgen hier enkele belangrijke aspecten van de consistentieniveaus:  

Als vóór de consistentie voor gebonden veroudering zorgt ervoor dat alle leesbewerkingen opgenomen in k prefixen of t seconden van de meest recente schrijven in een van de regio's worden. Bovendien worden leesbewerkingen met consistentie voor gebonden veroudering monotone en consistent voorvoegsel garanties worden gegarandeerd. Het protocol anti entropie werkt op een manier beperkt in de snelheid en zorgt ervoor dat de voorvoegsels niet worden en de tegendruk op de schrijfbewerkingen niet hoeft te worden toegepast. Voordat u, gelezen sessie consistentie gegarandeerd monotone lezen, monotone schrijfbewerkingen, uw eigen schrijfbewerkingen, garandeert lezen van de volgende manier en consistent voorvoegsel over de hele wereld. Voor de databases die zijn geconfigureerd met sterke consistentie en schrijven de systeem-switches terug naar een enkele regio door het toewijzen van een leider wordt erkend in elk van de partitie-sets. 

De semantiek van de vijf consistentiemodellen in Cosmos DB worden beschreven [hier](consistency-levels.md) en wiskundig weergegeven met behulp van een op hoog niveau TLA + specificaties [hier](https://github.com/Azure/azure-cosmos-tla).

## <a name="next-steps"></a>Volgende stappen

Naast informatie over het configureren van wereldwijde distributie met behulp van de volgende artikelen:

* [Clients configureren voor multihoming](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Regio's van uw database toevoegen/verwijderen](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Een aangepaste conflict resolutie beleid voor de SQL API-accounts maken](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
