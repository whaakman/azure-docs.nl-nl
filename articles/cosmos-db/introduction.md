---
title: Inleiding tot Azure Cosmos DB | Microsoft Docs
description: Lees hier alles over Azure Cosmos DB. Deze wereldwijd gedistribueerde database met meerdere modellen is gebouwd voor lage latentie, elastische schaalbaarheid en hoge beschikbaarheid.
services: cosmosdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 1591a7935b10e22164d26a09c4e923101b517227
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017


---

# <a name="welcome-to-azure-cosmos-db"></a>Welkom bij Azure Cosmos DB

Azure Cosmos DB is de wereldwijd gedistribueerde database van Microsoft met meerdere modellen. Azure Cosmos DB maakt het mogelijk om met één muisklik doorvoer en opslag elastisch en onafhankelijk te schalen binnen een onbeperkt aantal geografische regio's van Azure. De oplossing biedt gegarandeerde doorvoer, latentie, beschikbaarheid en consistentie, vastgelegd in uitgebreide [serviceovereenkomsten](https://aka.ms/acdbsla) (SLA's), iets wat geen andere databaseservice kan bieden.

![Azure Cosmos DB is de service van Microsoft voor wereldwijd gedistribueerde databases met mogelijkheden voor elastisch uitschalen, een gegarandeerde lage latentie, vijf consistentiemodellen en uitgebreide, gegarandeerde SLA's](./media/introduction/azure-cosmos-db.png)

Azure Cosmos DB bevat een database-engine die is geoptimaliseerd voor schrijven, en die resourcegestuurd en schemaneutraal is. De engine biedt ingebouwde ondersteuning voor meerdere gegevensmodellen: sleutel-waarde, documenten, grafieken en kolommen. Daarnaast worden allerlei API's ondersteund voor toegang tot gegevens, waaronder [MongoDB](../documentdb/documentdb-protocol-mongodb.md), [DocumentDB SQL](../documentdb/documentdb-introduction.md), [Gremlin](graph-introduction.md) (preview) en [Azure Tables](table-introduction.md) (preview). Deze ondersteuning kan bovendien verder worden uitgebreid. 

Eind 2010 is er een begin gemaakt met Azure Cosmos DB om knelpunten op te lossen waarmee ontwikkelaars van Microsoft werden geconfronteerd bij het werken aan grootschalige toepassingen. Aangezien Microsoft niet het enige bedrijf is dat zich bezighoudt met het bouwen van wereldwijd gedistribueerde toepassingen, werd de service in de vorm van Azure DocumentDB extern beschikbaar gesteld aan alle Azure-ontwikkelaars. Azure Cosmos DB is de volgende grote stap in de ontwikkeling van DocumentDB en de oplossing komt nu ook voor u beschikbaar. Als onderdeel van deze release van Azure Cosmos DB worden klanten van DocumentDB (en hun gegevens) automatisch klanten van Azure Cosmos DB. De overgang is naadloos en deze klanten hebben nu ook toegang tot de nieuwe functionaliteit die is toegevoegd aan Azure Cosmos DB. 

## <a name="capability-comparison"></a>Vergelijking van functionaliteit

Azure Cosmos DB biedt de beste mogelijkheden van relationele en niet-relationele databases.

| Functionaliteit | Relationele databases    | Niet-relationele (NoSQL-) databases |     Azure Cosmos DB |
| --- | --- | --- | --- |
| Wereldwijde distributie | x | x | ✓ Turnkey, 30+ regio's, multihoming |
| Horizontaal schalen | x | ✓ | ✓ Onafhankelijk schalen van doorvoer en opslag | 
| Gegarandeerde latentie | x | ✓ | ✓ < 10 ms voor lezen, < 15 ms voor schrijven bij p99 | 
| Hoge beschikbaarheid | x | ✓ | ✓ Altijd ingeschakeld, PACELC-tradeoffs, automatische en handmatige failover |
| Gegevensmodel + API | Relationeel + SQL | Meerdere modellen + OSS-API | Meerdere modellen + SQL + OSS-API (binnenkort meer) |
| SLA's | ✓ | x | ✓ Uitgebreide SLA's voor latentie, doorvoer, consistentie, beschikbaarheid |

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden
Als een wereldwijd gedistribueerde databaseservice biedt Azure Cosmos DB de volgende mogelijkheden om u te helpen met het bouwen van schaalbare, wereldwijd gedistribueerde, zeer responsieve toepassingen:

* [**Turnkey wereldwijde distributie**](#global-distribution)
    * Uw toepassing is direct beschikbaar voor uw gebruikers, waar die zich ook bevinden. Dit kan nu ook met uw gegevens.
    * U hoeft zich geen zorgen te maken over hardware, het toevoegen van knooppunten, virtuele machines of kerngeheugens. Aanwijzen en klikken is voldoende om uw gegevens op de gewenste bestemming te krijgen. 

* [**Meerdere gegevensmodellen en populaire API's voor het raadplegen en opvragen van gegevens**](#data-models)
    * Ondersteuning voor meerdere gegevensmodellen, waaronder sleutel-waarde, document, grafiek en kolommen.
    * Uitbreidbare API's voor Node.js, Java, .NET, .NET-Core, Python en MongoDB.
    * SQL en Gremlin voor query's. 

* [**Doorvoer en opslag op aanvraag elastisch schalen, waar ook ter wereld**](#horizontal-scale)
    * U kunt doorvoer op eenvoudige wijze schalen met de granulaties [seconde](../documentdb/documentdb-request-units.md) en [minuut](https://aka.ms/acdbrupm), en de instelling wijzigen wanneer dat nodig is. 
    * U kunt opslag [transparant en automatisch](partition-data.md) schalen om aan de opslagbehoeften te voldoen; nu, in de toekomst, altijd.

* [**Zeer responsieve en bedrijfskritische toepassingen bouwen**](#low-latency) 
    * U kunt overal ter wereld toegang krijgen tot uw gegevens met in 99% van de gevallen een latentie van minder dan tien milliseconden. 

* [**Beschikbaarheid op basis van Altijd ingeschakeld**](#high-availability)
    * Beschikbaarheid van 99,99% binnen één regio.
    * U kunt voor een hogere beschikbaarheid implementeren naar een willekeurig aantal [Azure-regio's](https://azure.microsoft.com/regions).
    * U kunt zonder gegevensverlies [een storing simuleren](../documentdb/documentdb-regional-failovers.md) in een of meer regio's. 

* [**Wereldwijd gedistribueerde toepassingen schrijven, op de juiste manier**](#consistency)
    * [Vijf consistentiemodellen](../documentdb/documentdb-consistency-levels.md) omvatten het spectrum van de sterke consistentie van SQL tot de mogelijke consistentie van NoSQL, en alles wat ertussenin ligt. 
  
* [**Niet-goed-geld-teruggarantie**](#sla) 
    * Uw gegevens komen razendsnel op de plaats van bestemming of u krijgt uw geld terug. 
    * [Serviceovereenkomsten](https://aka.ms/acdbsla) voor beschikbaarheid, latentie, doorvoer en consistentie. 

* [**Geen databaseschema/indexbeheer**](#schema-free)
    * U hoeft zich geen zorgen meer te maken of uw databaseschema en indexen nog synchroon zijn met uw toepassingsschema. We maken namelijk geen gebruik van schema's. 

* [**Lage eigendomskosten**](#tco)
    * Vijf tot tien keer [kosteneffectiever](https://aka.ms/documentdb-tco-paper) dan een niet-beheerde oplossing.
    * Drie keer goedkoper dan DynamoDB.

<a id="global-distribution"></a>

## <a name="global-distribution"></a>Wereldwijde distributie
Containers van Azure DB Cosmos worden gedistribueerd in twee dimensies: 

1. Binnen een bepaalde regio worden alle resources horizontaal gepartitioneerd in resourcepartities (lokale distributie). 
2. Elke resourcepartitie wordt ook gerepliceerd in geografische regio's (wereldwijde distributie). 

![Een diagram met de wereldwijde distributie van Azure Cosmos DB](./media/introduction/azure-cosmos-db-global-distribution.png) 

Wanneer uw opslag en doorvoer moeten worden geschaald, worden er binnen alle regio's op een transparante manier bewerkingen voor partitiebeheer uitgevoerd door Cosmos DB. Onafhankelijk van de schaal, distributie of storingen, biedt Cosmos DB altijd één integraal beeld van alle wereldwijd gedistribueerde resources. 

De wereldwijde distributie van resources in Cosmos DB vindt [turnkey](../documentdb/documentdb-distribute-data-globally.md) plaats. U kunt op elk moment met een paar muisklikken (of programmatisch met één API-aanroep) een onbeperkt aantal geografische regio's aan uw databaseaccount koppelen. 

Ongeacht de hoeveelheid gegevens of het aantal regio's, garandeert Microsoft dat in 99% van de gevallen in elke nieuw gekoppelde regio binnen het uur wordt gestart met de verwerking van aanvragen van clients. Dit wordt mogelijk gemaakt door gegevens uit alle bronresourcepartities gelijktijdig te seeden en te kopiëren naar de nieuw gekoppelde regio. Klanten kunnen een bestaande regio ook verwijderen of een regio die eerder was verbonden met hun databaseaccount, offline zetten.

<a id="data-models"></a>
## <a name="multi-model-multi-api-support"></a>Ondersteuning voor meerdere modellen en meerdere API's
 Azure Cosmos DB biedt standaard ondersteuning voor meerdere gegevensmodellen, inclusief documenten, sleutel-waarde, grafiek en kolom-familie. Het essentiële inhoudsmodel van de database-engine van Cosmos-DB is gebaseerd op ARS (atom-record-sequence; reeks van atomaire records). Atomen bestaan uit een kleine set primitieve typen zoals tekenreeks, Boole en getal. Records zijn structuren die zijn samengesteld uit deze typen. Reeksen zijn matrices die bestaan uit atomen, records of reeksen. 

![Azure Cosmos DB; meerdere modellen en meerdere API's](./media/introduction/azure-cosmos-db-multimodel.png) 
 
 De database-engine kan op een efficiënte manier verschillende gegevensmodellen vertalen en projecteren op het ARS-gegevensmodel. Het essentiële gegevensmodel van Cosmos DB is standaard toegankelijk vanuit programmeertalen met dynamische typedefinitie en kan direct worden weergegeven als JSON. 
 
 De service ondersteunt ook populaire database-API's voor het raadplegen en opvragen van gegevens. De database-engine van Cosmos DB biedt nu ondersteuning voor [DocumentDB SQL](../documentdb/documentdb-introduction.md), [MongoDB](../documentdb/documentdb-protocol-mongodb.md), [Azure Tables](table-introduction.md) (preview) en [Gremlin](graph-introduction.md) (preview). U kunt toepassingen blijven bouwen met populaire OSS-API's en tegelijkertijd uw voordeel doen met een uitvoerig geteste en volledig beheerde, wereldwijd gedistribueerde databaseservice. 

<a id="horizontal-scale"></a>
## <a name="horizontal-scaling-of-storage-and-throughput"></a>Opslag en doorvoer horizontaal schalen
Alle gegevens in een container van Cosmos-DB (zoals een documentverzameling, tabel of grafiek) worden horizontaal gepartitioneerd en transparant beheerd met behulp van resourcepartities. Een resourcepartitie is een consistente en zeer beschikbare container met gegevens die worden gepartitioneerd aan de hand van een [door de klant opgegeven partitiesleutel](partition-data.md). De partitie biedt een centraal beeld van een reeks resources die in de partitie worden beheerd en is een fundamentele eenheid van schaalbaarheid en distributie. Cosmos DB is ontworpen om doorvoer elastisch te kunnen schalen op basis van de verkeerspatronen van een toepassing in verschillende geografische regio's, om zo workloads te ondersteunen die zowel geografisch als in de tijd variëren. De partities worden transparant beheerd door de service, zonder dat dit ten koste gaat van de beschikbaarheid, consistentie, latentie of doorvoer van een Cosmos DB-container.  
 
![Azure Cosmos DB is horizontaal schaalbaar](./media/introduction/azure-cosmos-db-partitioning.png) 

U kunt de doorvoer van een Azure Cosmos DB-container elastisch schalen via programmacode door doorvoer te reserveren met behulp van [aanvraageenheden per seconde (RU/s)](../documentdb/documentdb-request-units.md). Intern worden repartities transparant beheerd door de service om de gevraagde doorvoer voor een bepaalde container te realiseren. Cosmos DB zorgt ervoor dat de doorvoer beschikbaar is voor gebruik in alle regio's die aan de container zijn gekoppeld. De nieuwe doorvoer is binnen vijf seconden na wijziging van de geconfigureerde doorvoercapaciteit van kracht. 

U kunt de doorvoer voor een container van Cosmos DB inrichten met de granulaties per seconde en [per minuut (RU/m)](request-units-per-minute.md). De ingerichte doorvoer via de granulatie per minuut wordt gebruikt voor het opvangen van onverwachte pieken in de workload terwijl granulatie per seconde wordt gebruikt. 

<a id="low-latency"></a>
## <a name="low-latency-guarantees-at-the-99th-percentile"></a>Garantie voor lage latentie in het 99e percentiel
Als onderdeel van de SLA's garandeert Microsoft een end-to-end lage latentie in het 99e percentiel voor klanten. Voor een gemiddeld item van 1 kB betekent dit dat in 99% van de gevallen een end-to-end latentie van minder dan 10 ms wordt gegarandeerd voor leesbewerkingen en van minder dan 15 ms voor geïndexeerde schrijfbewerkingen, binnen dezelfde Azure-regio. De gemiddelde latenties zijn aanzienlijk lager (minder dan 5 ms).  Met een bovengrens voor aanvraagverwerking voor elke databasetransactie kunnen clients van Cosmos DB duidelijk onderscheid maken tussen transacties met een hoge latentie en een database die niet beschikbaar is.

<a id="high-availability"></a>
## <a name="transparent-multi-homing-and-9999-high-availability"></a>Transparante multihoming en hoge beschikbaarheid van 99,99%
U kunt dynamisch 'prioriteiten' toewijzen aan de regio's die zijn gekoppeld aan uw databaseaccount van Azure Cosmos DB. Prioriteiten worden gebruikt om de aanvragen in het geval van regionale storingen te verzenden naar specifieke regio's. In het onwaarschijnlijke geval van een noodsituatie in de hele regio, wordt er door Cosmos DB automatisch een failover uitgevoerd in volgorde van de opgegeven prioriteit.

Als u de end-to-end beschikbaarheid van de toepassing wilt testen, kunt u [handmatig een failover activeren](../documentdb/documentdb-regional-failovers.md) (maximaal twee keer per uur). Microsoft garandeert dat er geen gegevensverlies optreedt tijdens handmatige failover naar een andere regio. Als er toch sprake is van een noodsituatie in de hele regio, garandeert Microsoft een bovengrens voor gegevensverlies tijdens de door het systeem gestarte automatische failover. U hoeft de toepassing niet opnieuw te implementeren na een failover naar een andere regio. Verder blijven de beschikbaarheids-SLA's van kracht. 

Voor dit scenario staat Cosmos DB interactie met resources toe via logische (regioneutrale) of fysieke (regiospecifiek) eindpunten. In het eerste geval kan transparante multihoming voor de toepassing worden uitgevoerd als er sprake is van failover. In het tweede geval is fijnmazige aansturing van de toepassing mogelijk, zodat lees- en schrijfbewerkingen kunnen worden omgeleid naar specifieke regio's. Microsoft garandeert via een SLA een beschikbaarheid van 99,99% voor elk databaseaccount. De beschikbaarheidsgaranties staan los van de schaal (ingerichte doorvoer en opslag), het aantal regio's of de geografische afstand tussen regio's die zijn gekoppeld aan een bepaalde database. 

<a id="consistency"></a>
## <a name="multiple-well-defined-consistency-models"></a>Meerdere, duidelijk omschreven consistentiemodellen
Commerciële gedistribueerde databases kunnen worden onderverdeeld in twee categorieën: databases die helemaal geen duidelijk omschreven, waarschijnlijke consistentiekeuzes bieden en databases die twee mogelijkheden voor extreme programmeerbaarheid bieden (sterke versus mogelijke consistentie). In het eerste geval moeten ontwikkelaars van toepassingen gedetailleerde instellingen opgeven voor hun replicatieprotocollen en moeten ze lastige keuzes maken om een juiste balans te vinden tussen consistentie, beschikbaarheid, latentie en doorvoer. In het laatste geval moet een keuze worden gemaakt tussen twee extreme varianten. Ondanks de overvloed aan onderzoek en voorstellen voor meer dan 50 consistentiemodellen, is de community van gedistribueerde databases er niet in geslaagd om consistentieniveaus voorbij 'sterk' en 'mogelijk' commercieel geaccepteerd te krijgen. 

In Cosmos DB kunt u kiezen tussen [vijf duidelijk omschreven consistentiemodellen](../documentdb/documentdb-consistency-levels.md) die het volledige consistentiespectrum omvatten: sterk, gebonden veroudering, [sessie](http://dl.acm.org/citation.cfm?id=383631), consistent prefix en mogelijk. 

![Azure Cosmos DB biedt een keuze uit verschillende, duidelijk omschreven (minder nauwkeurige) consistentiemodellen](media/introduction/azure-cosmos-db-consistency-levels.png)

In de volgende tabel ziet u de specifieke garanties die horen bij elk consistentieniveau.
 
**Consistentieniveaus en bijbehorende garanties**

| Consistentieniveau    | Garanties |
| --- | --- |
| Sterk | Linearisabiliteit |
| Gebonden veroudering    | Consistent prefix. Leesbewerkingen volgen op schrijfbewerkingen met k prefixen of t interval |
| Sessie    | Consistent prefix. Monotone leesbewerkingen, monotone schrijfbewerkingen, read-your-writes, write-follows-reads |
| Consistent prefix    | Geretourneerde updates zijn een prefix van alle updates, zonder hiaten |
| Mogelijk    | Leesbewerkingen vinden niet op volgorde plaats |

U kunt het standaardconsistentieniveau configureren in uw Cosmos DB-account (en de consistentie later voor een specifieke aanvraag overschrijven). Intern wordt het standaardconsistentieniveau toegepast op gegevens in de partitiesets, die mogelijk deel uitmaken van verschillende regio's. 


<a id="sla"></a>
## <a name="guaranteed-service-level-agreements"></a>Gegarandeerde serviceovereenkomsten

Cosmos DB is de eerste beheerde databaseservice met [SLA-garanties](https://aka.ms/acdbsla) van 99,99% voor beschikbaarheid, doorvoer, lage latentie en consistentie.
* Beschikbaarheid: SLA voor beschikbaarheid met een uptime van 99,99% voor alle bewerkingen op gegevens en op besturingsvlak.
* Doorvoer: 99,99% van de aanvragen met succes voltooid 
* Latentie: 99,99% van latenties < 10 ms in het 99e percentiel
* Consistentie: 100% van de leesaanvragen voldoet aan de consistentiegarantie voor het door u aangevraagde consistentieniveau.


<a id="schema-free"></a>
## <a name="schema-free"></a>Geen schema's

Bij zowel relationele databases als NoSQL-databases moet u verplicht aan de slag met schema- en indexbeheer, versiebeheer en migratie. Dit zijn allemaal zaken die zeer complex zijn in een configuratie voor wereldwijde distributie. Als u kiest voor Cosmos DB, hebt u hier echter geen omkijken naar. Met Cosmos DB hoeft u namelijk geen schema's en indexen te beheren, ingewikkeld te doen met schemaversies of u zorgen te maken over de beschikbaarheid van toepassingen als u schema's moet gaan migreren. De database-engine van Cosmos DB is namelijk volledig schemaneutraal. Alle ontvangen gegevens worden automatisch geïndexeerd zonder dat hiervoor schema's of indexen nodig zijn en query's worden razendsnel uitgevoerd. 

<a id="tco"></a>
## <a name="low-cost-of-ownership"></a>Lage eigendomskosten

 Wanneer rekening wordt gehouden met alle overwegingen voor totale eigendomskosten (TCO), kunnen beheerde cloudservices zoals Azure Cosmos DB vijf tot tien keer voordeliger zijn dan hun OSS-equivalenten met on-premises of virtuele machines. Azure Cosmos DB is maximaal twee tot drie keer goedkoper dan DynamoDB voor workloads met hoge volumes. Lees voor meer informatie deze Engelstalige [whitepaper](https://aka.ms/documentdb-tco-paper). 

## <a name="next-steps"></a>Volgende stappen
Lees onze snelstartgidsen om snel aan de slag te gaan met Azure Cosmos DB:

* [Aan de slag met de DocumentDB-API van Azure Cosmos DB](create-documentdb-dotnet.md)
* [Aan de slag met de MongoDB-API van Azure Cosmos DB](create-mongodb-nodejs.md)
* [Aan de slag met de grafiek-API van Azure Cosmos DB](create-graph-dotnet.md)
* [Aan de slag met de tabel-API van Azure Cosmos DB](create-table-dotnet.md)

