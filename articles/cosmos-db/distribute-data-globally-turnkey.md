---
title: Hoe Azure Cosmos DB kunt kant en klare wereldwijde distributie | Microsoft Docs
description: Meer informatie over wereldwijde geo-replicatie, meerdere masters, failover en gegevens herstellen met behulp van de globale databases van Azure Cosmos DB, een wereldwijd gedistribueerde, multi-model databaseservice.
services: cosmos-db
author: markjbrown
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.openlocfilehash: 1aa0fa3d4d9e1821a6980d9334456a78eba7bfbc
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956911"
---
# <a name="how-azure-cosmos-db-enables-turnkey-global-distribution"></a>Hoe Azure Cosmos DB kunt kant en klare wereldwijde distributie
Azure Cosmos DB biedt de volgende mogelijkheden waarmee u kunt eenvoudig wereldwijd gedistribueerde toepassingen schrijven. Deze mogelijkheden zijn beschikbaar via de Azure Cosmos DB-resource op basis van een provider [REST-API's](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/) en de Azure-portal.

## <a id="GlobalDistribution"></a>Wereldwijde distributie

### <a id="RegionalPresence"></a>Alomtegenwoordige regionale aanwezigheid 
Azure is de geografische aanwezigheid voortdurend groeiende doordat [nieuwe regio's](https://azure.microsoft.com/regions/) online. Azure Cosmos DB is geclassificeerd als een *fundamentele service* in Azure en is beschikbaar in alle nieuwe Azure-regio's standaard. Hiermee kunt u een geografische regio koppelen aan uw Azure Cosmos DB-databaseaccount zodra de nieuwe regio voor bedrijven in Azure geopend.

### <a id="MultiMasterSupport"></a>Ondersteuning voor meerdere masters
Azure Cosmos DB biedt systeemeigen, server-side '-ondersteuning voor meerdere master regio's die evenredig deel uitmaken van een model schrijven overal. Deze ondersteuning levert < 10 ms latentie schrijven en 99,999% beschikbaarheid per schrijven [met financiële Sla's ondersteund](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Meerdere masters is beschikbaar voor de, met inbegrip van alle API [SQL](sql-api-introduction.md), [MongoDB](mongodb-introduction.md), [Cassandra](cassandra-introduction.md), [Graph](graph-introduction.md), en [tabel](table-introduction.md) en vanuit alle SDK-talen voor Cosmos DB. Azure Cosmos DB ondersteunt 4 verschillende consistentieniveaus (gebonden veroudering, sessie, consistent voorvoegsel en uiteindelijk) voor accounts met leren werken met multi mogelijkheid.

### <a id="UnlimitedRegionsPerAccount"></a>Een onbeperkt aantal regio's koppelen aan uw Azure Cosmos DB-databaseaccount
Azure Cosmos DB kunt u een willekeurig aantal Azure-regio's koppelen aan uw Azure Cosmos DB-databaseaccount. Er zijn geen beperkingen op het aantal regio's die gekoppeld aan uw Azure Cosmos DB-databaseaccount worden kunnen buiten geofencing beperkingen (bijvoorbeeld, China, Duitsland). De volgende afbeelding ziet u een databaseaccount dat is geconfigureerd voor het overbruggen van 25 Azure-regio's:

![Azure Cosmos DB-databaseaccount spanning 25 Azure-regio 's](./media/distribute-data-globally-turnkey/spanning-regions.png)


### <a id="PolicyBasedGeoFencing"></a>Op basis van beleid geofencing
Azure Cosmos DB is ontworpen ter ondersteuning van op beleid gebaseerde geofencing. Geofencing is een belangrijk onderdeel om te controleren of beperkingen voor het beheer en naleving van gegevens en kan voorkomen dat een specifieke regio koppelen aan uw account. Voorbeelden van geofencing bevatten (maar niet beperkt tot) scoping van wereldwijde distributie naar de regio's binnen een onafhankelijke Clouds (bijvoorbeeld, China en Duitsland) of binnen een grens van een government belasting (bijvoorbeeld Australië). Het beleid worden beheerd met behulp van de metagegevens van uw Azure-abonnement.

### <a id="DynamicallyAddRegions"></a>Dynamisch toevoegen en verwijderen van regio 's
Azure Cosmos DB kunt u (koppelen) toevoegen of verwijderen (ontkoppelen) regio's van uw databaseaccount op elk gewenst moment in de tijd (Zie [voorgaande afbeelding](#UnlimitedRegionsPerAccount)). Omdat parallelle replicatie van gegevens over meerdere partities, Azure Cosmos DB zorgt ervoor dat wanneer een nieuwe regio wordt toegevoegd, beschikbaar voor bewerkingen binnen 30 minuten overal ter wereld is (ervan uitgaande dat uw gegevens is 100 TB of minder). 

### <a id="FailoverPriorities"></a>Failover-prioriteiten
Als u niet meerdere masters, klanten kunnen de exacte volgorde van de regionale failover in geval van een storing controle, Azure Cosmos DB kunt u koppelen een *prioriteit* met verschillende regio's die zijn gekoppeld aan het account van de database (Zie de de afbeelding hieronder). Azure Cosmos DB zorgt ervoor dat de automatische failover van de takenreeks wordt uitgevoerd in de volgorde van de prioriteit die u hebt opgegeven. Zie voor meer informatie over regionale failovers [automatische regionale failovers voor zakelijke continuïteit in Azure Cosmos DB](regional-failover.md). De volgende afbeelding ziet u hoe een tenant in Azure Cosmos DB de failover-volgorde van prioriteit (rechterdeelvenster) kunt configureren voor regio's die zijn gekoppeld aan een databaseaccount:

![Failover-prioriteiten configureren met Azure Cosmos DB](./media/distribute-data-globally-turnkey/failover-priorities.png)

### <a id="ConsistencyLevels"></a>Meerdere duidelijk gedefinieerde consistentiemodellen voor wereldwijd gedistribueerde databases
Azure Cosmos DB ondersteunt [meerdere duidelijk gedefinieerde, praktische en intuïtieve consistentiemodellen](consistency-levels.md) basis van serviceovereenkomsten. U kunt een specifieke consistentiemodel (in de lijst met opties), afhankelijk van de werkbelasting/scenario's.

### <a id="TunableConsistency"></a>Instelbare consistentie voor wereldwijd gerepliceerde databases
Azure Cosmos DB kunt u programmatisch overschrijven en de standaardoptie voor consistentie op basis van per aanvraag tijdens runtime te versoepelen.

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Dynamisch configureerbare lezen en schrijven regio 's
Azure Cosmos DB kunt u de regio's (die zijn gekoppeld aan de database) configureren voor 'lezen', 'schrijven' of ' lezen/schrijven' regio's.

### <a id="ElasticallyScaleThroughput"></a>Elastisch schalen van doorvoer meerdere Azure-regio 's
U kunt flexibel schalen een Azure Cosmos DB-container door inrichting doorvoer via een programma. De doorvoer wordt toegepast op alle gebieden van die de Azure Cosmos DB-container wordt gedistribueerd in.

### <a id="GeoLocalReadsAndWrites"></a>Geo-lokale lees- en schrijfbewerkingen
Het belangrijkste voordeel van een wereldwijd gedistribueerde database is de lage latentie toegang tot gegevens overal ter wereld. Azure Cosmos DB biedt een lage latentie (< 10 milliseconden) leest en schrijft in het 99e percentiel over de hele wereld. Het zorgt ervoor dat alle leesbewerkingen worden aangeleverd vanuit de dichtstbijzijnde regio (lokaal). Als u wilt een leesaanvraag fungeren, wordt het lokaal in de regio waarin de leesbewerking wordt uitgegeven quorum gebruikt. Dit geldt ook voor schrijfbewerkingen. Een schrijfbewerking is bevestigd nadat een meerderheid van de replica's hebben de schrijven blijvend zijn doorgevoerd lokaal maar zonder wordt geregeld op externe replica's voor het bevestigen van de schrijfbewerkingen. Als u wilt deze anders plaatsen, werkt het protocol van de replicatie van Azure Cosmos DB in de veronderstelling dat het quorum lezen en schrijven altijd lokaal zijn voor de regio waar de aanvraag is uitgegeven.

### <a id="ManualFailover"></a>Handmatige failover
Azure Cosmos DB kunt u voor het activeren van de failover van een databaseaccount dat u wilt valideren de *end-to-end* eigenschappen van de beschikbaarheid van de gehele toepassing (anders dan de database). Omdat zowel de veiligheid en de liveness eigenschappen van de fout detectie en leader election worden gegarandeerd, Azure Cosmos DB garandeert *nul gegevensverlies* voor een handmatige failover tenant geïnitieerde bewerking.

### <a id="AutomaticFailover"></a>Automatische failover
Azure Cosmos DB biedt ondersteuning voor automatische failover tijdens een of meer regionale storingen. Tijdens een regionale failover houdt Azure Cosmos DB de leeslatentie, actieve tijdsduur van beschikbaarheid, consistentie en doorvoer Sla's. Azure Cosmos DB biedt een bovengrens voor de duur van een automatische failover-bewerking is voltooid. Dit is het venster van mogelijk gegevensverlies tijdens een regionale onderbreking.

### <a id="GranularFailover"></a>Ontworpen voor verschillende failover granulaties
De mogelijkheden voor automatische en handmatige failover worden momenteel weergegeven op de granulatie van de account van de database. Let op: intern Azure Cosmos DB is ontworpen om u te bieden *automatische* failover op meer granulatie van een database, een container of zelfs een partitie (van een container die eigenaar is van een bereik van sleutels). 

### <a id="MultiHomingAPIs"></a>Multihoming in Azure Cosmos DB

In een single-master-scenario waarin u hebt een schrijfregio en meerdere lezen-replica's, Azure Cosmos DB kunt u communiceren met een database met behulp van de logische (regioneutrale) of fysieke (regiospecifiek) eindpunten. Met behulp van logische eindpunten zorgt ervoor dat de toepassing kunt transparant multihomed in het geval van een failover. De laatste geen fysiek eindpunt is fijnmazige aansturing van de toepassing om te leiden leest en schrijft naar specifieke regio's. In scenario's met meerdere regio's schrijven is ingeschakeld, moeten gebruikers regiospecifiek-eindpunten met regio's toegevoegd in volgorde van prioriteit voor het gebruik en omleidingen opgeven.

U vindt meer informatie over het configureren van lezen voorkeuren voor de [SQL API](../cosmos-db/tutorial-global-distribution-sql-api.md), [Table-API](../cosmos-db/tutorial-global-distribution-table.md), en [MongoDB-API](../cosmos-db/tutorial-global-distribution-mongodb.md) in deze artikelen.

### <a id="TransparentSchemaMigration"></a>Transparant en consistent schema- en indexbeheer databasemigratie 
Azure Cosmos DB is volledig [schema-agnostische](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Het ontwerp van de database-engine van de unieke Azure Cosmos DB automatisch kunt en Indexeer alle gegevens die bij opname, zonder een schema of secundaire indexen van de gebruiker. Hiermee kunt u uw wereldwijd gedistribueerde toepassing snel herhalen zonder u zorgen te maken over de migratie van de schema- en indexbeheer van de database of het coördineren van meerdere fase toepassingsimplementaties van wijzigingen in het schema. Azure Cosmos DB zorgt ervoor dat eventuele wijzigingen in het indexeringsbeleid expliciet door u zijn gemaakt niet in verslechtering van prestaties of beschikbaarheid resulteren.  

### <a id="ComprehensiveSLAs"></a>Uitgebreide Sla's (naast de hoge beschikbaarheid)
Als een wereldwijd gedistribueerde databaseservice biedt Azure Cosmos DB duidelijk gedefinieerde en uitgebreide Sla's voor **beschikbaarheid**, **latentie**, **doorvoer**, en **consistentie** voor de database die wordt uitgevoerd op wereldwijde schaal, ongeacht het aantal regio's die zijn gekoppeld aan de database.  

## <a id="LatencyGuarantees"></a>Gegarandeerde latentie
Het belangrijkste voordeel van een wereldwijd gedistribueerde databaseservice, zoals Azure Cosmos DB is te bieden van toegang met lage latentie tot uw gegevens overal ter wereld. Azure Cosmos DB biedt gegarandeerde lage latentie in het 99e percentiel voor verschillende databasebewerkingen. De replicatie-protocol met de Azure Cosmos DB zorgt ervoor dat de databasebewerkingen (zowel lees- en schrijfbewerkingen) altijd worden uitgevoerd in de regio die lokaal op die van de client. De latentie SLA van Azure Cosmos DB biedt gegarandeerd in het 99e percentiel voor zowel leesbewerkingen, (synchroon) geïndexeerde schrijfbewerkingen en query's voor verschillende aanvragen en antwoordgroottes. De gegarandeerde latentie voor schrijfbewerkingen zijn duurzame meerderheid quorum doorvoeringen in de lokale regio.

### <a id="LatencyAndConsistency"></a>Latentie van relatie met consistentie 
Voor een wereldwijd gedistribueerde service te bieden van consistentie in een wereldwijd gedistribueerde installatie, moet deze synchroon repliceren schrijfbewerkingen of regio-overschrijdende leesbewerkingen synchroon uitvoeren. De snelheid van licht en de betrouwbaarheid van WAN-netwerk bepaalt dat sterke consistentie tot hogere latentie en verminderde beschikbaarheid van databasebewerkingen leiden zal. Daarom kan gegarandeerde lage latentie bij de 99e percentiel en beschikbaarheid van 99,99% voor alle accounts voor één regio en alle accounts voor meerdere regio's met soepele consistentie en beschikbaarheid van 99,999% voor alle databaseaccounts voor meerdere regio's, de service om te kunnen bieden asynchrone replicatie moet worden gebruikt. Deze beurt vereist dat de service ook moet bieden [goed gedefinieerde, soepele consistentie modellen](consistency-levels.md) – zwakkere dan sterke (om u te bieden met lage latentie en beschikbaarheid garandeert) en in het ideale geval beter dan "" eindconsistentie (met een intuïtief programmeermodel).

Azure Cosmos DB zorgt ervoor dat er een leesbewerking is vereist voor het Neem contact op met de replica's in meerdere regio's aan een specifiek niveau consistentiegarantie leveren. Evenzo zorgt ervoor dat een schrijfbewerking wordt niet geblokkeerd terwijl de gegevens wordt gerepliceerd in alle regio's dat wil zeggen, schrijfbewerkingen zijn asynchroon gerepliceerd in andere regio's). Voor databaseaccounts voor meerdere regio's zijn beide sterke en soepele consistentie op meerdere niveaus beschikbaar. 

### <a id="LatencyAndAvailability"></a>Relatie met de beschikbaarheid van de latentie 
Zijn de beide zijden van dezelfde medaille latentie en beschikbaarheid. Latentie van een bewerking in een stabiele status en beschikbaarheid met fouten en netwerkpartities bespreken. Een trage actieve databasebewerking is vanuit het oogpunt van de toepassing niet te onderscheiden van een database die niet beschikbaar is. 

Als u wilt onderscheiden hoge latentie van niet beschikbaar zijn, biedt Azure Cosmos DB een absolute bovengrens voor latentie van verschillende databasebewerkingen. Als de databasebewerking langer duurt dan de bovengrens om te voltooien, wordt in Azure Cosmos DB een time-outfout retourneert. De SLA van de beschikbaarheid van Azure Cosmos DB zorgt ervoor dat de time-outs zijn geteld op basis van de beschikbaarheids-SLA. 

### <a id="LatencyAndThroughput"></a>Latentie van relatie met doorvoer
Azure Cosmos DB maakt geen u kiezen tussen latentie en doorvoer. Het zich houdt aan de SLA voor beide latentie in het 99e percentiel, en levert de doorvoer die u hebt ingericht. 

## <a id="ConsistencyGuarantees"></a>Garanties voor consistentie
Terwijl de [sterke consistentie-model](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) is de standaard gold van gegevens programmeerbaarheid, wordt geleverd tegen een prijs zonder een straffe van hogere latentie (in onveranderlijke), en tot verminderde beschikbaarheid (bij storingen van). 

Azure Cosmos DB biedt een goed gedefinieerde programmeermodel voor u te beredeneren consistentie van de gerepliceerde gegevens. Zodat u gemakkelijk wereldwijd gedistribueerde toepassingen bouwen met multihoming-functie zijn de van consistentiemodellen die worden weergegeven door Azure Cosmos DB ontworpen om te worden regioneutrale en onafhankelijk van de regio waar de lees- en schrijfbewerkingen zijn niet geleverd. 

Azure Cosmos DB van consistentie SLA wordt gegarandeerd dat 100% van de leesaanvragen voldoet aan de consistentiegarantie voor de consistentiemodel dat is opgegeven door u (hetzij op het account van de database of het aanvraagniveau van de). Een aanvraag wordt beschouwd als om te voldoen aan de consistentie SLA, als alle de consistentiegarantie die zijn gekoppeld aan het consistentieniveau is voldaan. De volgende tabel bevat de consistentiemodel en de staatsgaranties consistentie. Elk van deze consistentiemodel een 100%-SLA wordt gegarandeerd. 

|Consistentiemodel  | Kenmerken  |
|---------|---------|
|Sterk |  Linearizable  |
|Gebonden veroudering  |  Succespercentage voor monotoon lezen (binnen een regio), Consistent voorvoegsel, gebonden veroudering &lt; K, T   |
|Sessie  |  Lezen, uw eigen schrijven, lezen Monotonic, Consistent voorvoegsel    |
|Consistent voorvoegsel  | Consistent voorvoegsel  |

### <a id="ConsistencyAndAvailability"></a>Relatie met de beschikbaarheid van de consistentie
De [onmogelijk resultaat](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) van de [CAP-theorema](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) blijkt dat het inderdaad om het systeem beschikbaar blijven en bieden linearizable consistentie storingen van het onmogelijk is. De database-service moet kiezen CP of Azië en Stille Oceaan, waar CP systemen beschikbaarheid en vervangen door linearizable consistentie beheercluster terwijl de systemen Azië en Stille Oceaan beheercluster [linearizable consistentie](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) en vervangen door de beschikbaarheid. Azure Cosmos DB is nooit in strijd met het aangevraagde consistentiemodel, dat voorheen het een CP-systeem maakt. In de praktijk is consistentie echter niet een alles of niets toegevoegde; Er zijn meerdere duidelijk gedefinieerde consistentiemodellen langs de spectrum van de consistentie tussen linearizable en uiteindelijke consistentie. Hiermee geeft u verschillende soepele consistentiemodellen die van toepassing zijn op praktijkscenario's en zijn gemakkelijk te gebruiken in Azure Cosmos DB. Azure Cosmos DB gaat afwegingen consistentie beschikbaarheid door het aanbieden van een [meerdere soepele nog duidelijk gedefinieerde consistentiemodellen](consistency-levels.md) en een beschikbaarheid van 99,99% voor alle enkele regio database-accounts en 99,999% lezen en schrijven beschikbaarheid voor alle databaseaccounts voor meerdere regio's. 

### <a id="ConsistencyAndAvailability"></a>Relatie met een latentie van de consistentie
Een meer uitgebreide variatie van CAP-theorema heet [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), die ook-accounts voor latentie en consistentie compromissen in geen stabiele status hebben. Er wordt aangegeven dat in een stabiele status, een database-systeem tussen consistentie en latentie kiezen moet. Azure Cosmos DB met meerdere soepele consistentie-modellen (ondersteund door asynchrone replicatie en lokale lezen en schrijven quorum), zorgt u ervoor dat alle lees- en schrijfbewerkingen lokaal voor het lezen zijn en schrijven van regio's respectievelijk. Hiermee kunt Azure Cosmos DB te bieden de garantie voor lage latentie in de regio voor de opgegeven consistentiemodellen.  

### <a id="ConsistencyAndThroughput"></a>De relatie van de consistentie met doorvoer
Omdat de implementatie van een specifieke consistentiemodel afhankelijk van de keuze van is een [quorumtype](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), doorvoer varieert op basis van de keuze van een consistentiemodel. Bijvoorbeeld, in Azure Cosmos DB, de RU-kosten voor sterk consistente leesbewerkingen is ongeveer *dubbele* die van uiteindelijke consistente leesbewerkingen. In dit geval moet u voor het inrichten van dubbele ru's voor het bereiken van de dezelfde doorvoer. De volgende afbeelding ziet u de relatie van meer capaciteit voor een specifieke consistentiemodel in Azure Cosmos DB:

![Relatie tussen de consistentie en doorvoer](./media/distribute-data-globally-turnkey/consistency-and-throughput.png)

## <a id="ThroughputGuarantees"></a>Doorvoer garanties 
Azure Cosmos DB kunt u doorvoer schalen (als opslag), flexibele manier naar een willekeurig aantal regio's, afhankelijk van uw behoeften of de aanvraag. De volgende afbeelding ziet u een enkele Azure Cosmos DB-container horizontaal gepartitioneerd (in drie resourcepartities binnen een regio) en wereldwijd gedistribueerde drie Azure-regio's:

![Azure Cosmos DB gedistribueerd en gepartitioneerd containers](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

Een Azure Cosmos DB-container verdeeld in twee dimensies (i) binnen een regio en (ii) tussen regio's. Dit doet u al volgt: 

* **Lokale distributie**: binnen een enkele regio en een Azure Cosmos DB-container is horizontaal uitgebreid in termen van *resourcepartities*. Elke resourcepartitie beheert een set van sleutels en is sterk consistente en maximaal beschikbare wordt fysiek wordt vertegenwoordigd door vier replica's ook wel een *replicaset* en status machinereplicatie tussen de replica's. Azure Cosmos DB is een systeem volledig resource geregeld waar een resourcepartitie is verantwoordelijk voor het leveren van een deel van de doorvoer voor het budget van systeemresources die zijn toegewezen. De schaal van een Azure Cosmos DB-container is transparant voor de gebruikers. Azure Cosmos DB beheert de resourcepartities en splitst en voegt ze indien nodig als opslag en doorvoer vereisten veranderen. 
* **Wereldwijde distributie**: als het een database voor meerdere regio's, wordt elk van de resourcepartities dan gedistribueerd in deze regio's. Resourcepartities die eigenaar is dezelfde set sleutels in verschillende regio's formulier *verzameling* (Zie [voorgaande afbeelding](#ThroughputGuarantees)).  Resourcepartities binnen een partitie worden gecoördineerd met behulp van de status machinereplicatie over meerdere regio's die zijn gekoppeld aan de database. Afhankelijk van het consistentieniveau geconfigureerd, worden de resourcepartities in een partitieset geconfigureerd dynamisch met behulp van verschillende topologieën (bijvoorbeeld ster, -keten, structuur enz.). 

Een zeer responsieve partitie management, taakverdeling en strikte resource governance kunt Azure Cosmos DB u doorvoer voor elastisch schalen over meerdere Azure-regio's die zijn gekoppeld aan een Azure Cosmos DB-container of de database. Wijzigen van de ingerichte doorvoer is een Runtimebewerking in Azure Cosmos DB. Net als bij andere databasebewerkingen, Azure Cosmos DB garandeert de absolute bovengrens voor latentie voor uw aanvraag voor het wijzigen van de ingerichte doorvoer. De volgende afbeelding ziet u bijvoorbeeld van een klant-container met een flexibel ingerichte doorvoer (1-10M aanvragen per seconde tussen twee regio's) op basis van de vraag.

![Azure Cosmos DB doorvoer elastisch ingericht](./media/distribute-data-globally-turnkey/elastic-throughput.png)

### <a id="ThroughputAndConsistency"></a>Doorvoer van relatie met consistentie 
Dit is dezelfde als beschreven in [consistentie van de relatie met doorvoer](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Relatie met de beschikbaarheid van doorvoer
Azure Cosmos DB blijft de hoge beschikbaarheid behouden wanneer wijzigingen worden aangebracht in de ingerichte doorvoer. Azure Cosmos DB repartities transparant beheerd (en voert splitsen en samenvoegen kloon operations) en zorgt ervoor dat de bewerkingen kunnen geen nadelige invloed op prestaties of beschikbaarheid, terwijl de toepassing elastisch vergroot of verkleint u de doorvoer. 

## <a id="AvailabilityGuarantees"></a>Beschikbaarheidsgaranties
Azure Cosmos DB biedt een beschikbaarheid van 99,99% SLA voor alle databaseaccounts voor één regio en alle accounts voor meerdere regio's met soepele consistentie en beschikbaarheid van 99,999% voor alle databaseaccounts voor meerdere regio's. Zoals eerder beschreven, omvatten van Azure Cosmos DB-beschikbaarheidsgaranties een absolute bovengrens voor latentie voor elke vlak van gegevens en bewerkingen. De beschikbaarheidsgaranties veranderen niet met het aantal regio's of de geografische afstand tussen regio's. Beschikbaarheidsgaranties zijn van toepassing met betrekking tot zowel handmatige als automatische failover. Azure Cosmos DB biedt transparante multihoming-API's die ervoor zorgen dat uw toepassing kan worden uitgevoerd op de eindpunten van logische en de aanvragen kunt transparant te routeren naar een nieuwe regio in geval van een failover of kunt u specifieke regionale eindpunten in volgorde van prioriteit voor het gebruik. Uw toepassing hoeft niet te worden geïmplementeerd in het geval van een regionale failover en de beschikbaarheid van SLA's te allen tijde worden bewaard.

### <a id="AvailabilityAndConsistency"></a>Beschikbaarheid van de relatie met consistentie, latentie en doorvoer
Beschikbaarheid van de relatie met consistentie, latentie en doorvoer wordt beschreven in de secties [relatie met de beschikbaarheid van de consistentie](#ConsistencyAndAvailability), [relatie met de beschikbaarheid van de latentie](#LatencyAndAvailability) en [Relatie met de beschikbaarheid van de doorvoer](#ThroughputAndAvailability). 

## <a id="CustomerFacingSLAMetrics"></a>Klantgerichte metrische SLA-gegevens
Azure Cosmos DB Ontsluit transparant de doorvoer, latentie, consistentie en beschikbaarheid van metrische gegevens. Deze metrische gegevens zijn toegankelijk via programmacode of via de Azure-portal (Zie de onderstaande afbeelding). U kunt ook waarschuwingen over verschillende drempelwaarden bij met behulp van Azure Application Insights instellen. De volgende afbeelding ziet u consistentie, latentie, doorvoer en metrische gegevens over beschikbaarheid die transparant beschikbaar voor elke tenant zijn:
 
![Azure Cosmos DB Klant zichtbaar metrische SLA-gegevens](./media/distribute-data-globally-turnkey/customer-slas.png)

## <a id="Next Steps"></a>Volgende stappen

* [Azure Cosmos DB globale distributie van de belangrijkste voordelen](distribute-data-globally-benefits.md)

* [Azure Cosmos DB globale databasereplicatie configureren](tutorial-global-distribution-sql-api.md)

* [Het inschakelen van meerdere masters voor Azure Cosmos DB-accounts](enable-multi-master.md)

* [Hoe automatische en handmatige failover in Azure Cosmos DB werkt](regional-failover.md)

* [Informatie over conflictoplossing in Azure Cosmos DB](multi-master-conflict-resolution.md)

* [Met behulp van meerdere masters met open-source NoSQL-databases](multi-master-oss-nosql.md)


## <a id="References"></a>Verwijzingen
1. Eric Brewer. [Voor robuuste gedistribueerde systemen](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric Brewer. [KAPJE twaalf jaar Later – hoe de regels zijn gewijzigd](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Gilbert, Lynch. - [Brewer&#39;s gissingen en haalbaarheid van consistente, beschikbare partitie fouttolerante webservices](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Daniel Abadi. [Optimalisatie van de consistentie in moderne gedistribueerde systemen databaseontwerp](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. Martin Kleppmann. [Stop de aanroepen van databases CP of Azië en Stille Oceaan](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis et al. [Probabilistic gebonden veroudering (PBS) voor praktische gedeeltelijke quorum](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor en wol. [Laden, capaciteit en beschikbaarheid in Quorum-systemen](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy en volgende. [Lineralizability: Juistheid voorwaarde voor gelijktijdige objecten](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [SLA van Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
