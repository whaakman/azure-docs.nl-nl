---
title: Gegevens met Azure Cosmos DB globaal distribueren | Microsoft Docs
description: Meer informatie over de wereld scale geo-replicatie, failover en gegevens herstellen met behulp van globale databases van Azure Cosmos DB, een globaal gedistribueerd, zijn voor het model databaseservice.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: sngun
ms.openlocfilehash: 4f548e180ca315013d5ca91118041cac2e622520
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34611446"
---
# <a name="how-to-distribute-data-globally-with-azure-cosmos-db"></a>Het distribueren van gegevens met Azure Cosmos DB globaal
Azure is alomtegenwoordige - heeft een algemene footprint tussen 50 + geografische regio's en continu groter wordt. Met wereldwijde aanwezigheid is een van de gedifferentieerde mogelijkheden die Azure voor de ontwikkelaars biedt de mogelijkheid om te maken, implementeren en beheren van globaal gedistribueerde toepassingen eenvoudig. 

[Azure Cosmos DB](../cosmos-db/introduction.md) is de wereldwijd gedistribueerde databaseservice met meerdere modellen van Microsoft voor essentiële toepassingen. Azure Cosmos DB biedt klare distributielijsten [elastisch schalen van doorvoer en opslag](../cosmos-db/partition-data.md) overal ter wereld, één cijfer milliseconde latenties op het 99th percentiel [vijf goed gedefinieerde consistentie modellen](consistency-levels.md), hoge beschikbaarheid, alle back-ups door gegarandeerd [toonaangevende uitgebreide serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [indexeert al uw gegevens automatisch](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) zonder u te gaan met het schema of index management. Dit is een service met meerdere modellen en ondersteunt document, sleutel / waarde-grafiek en gegevensmodellen kolom-serie. Als een systeemeigen geboren in de cloudservice, is Azure DB die Cosmos zorgvuldig ontworpen met multi-tenancymodus en globale verdeling van de grond.


![Azure DB Cosmos-verzameling is gepartitioneerd en verdeeld over drie gebieden](./media/distribute-data-globally/global-apps.png)

**Een enkele container Azure Cosmos DB gepartitioneerd en verdeeld over meerdere Azure-regio 's**

Als er tijdens het maken van Azure DB die Cosmos hebt geleerd, niet toe te voegen globale distributie pas achteraf overwogen. Kan niet 'bout eenmalige aanmelding' op een systeem van de database "één site". De mogelijkheden die worden aangeboden door een wereldwijd gedistribueerde database span afgezien van dat van traditionele geografische disaster recovery (geografisch DR) die worden aangeboden door 'één site' databases. Databases van één site Geo-DR-mogelijkheid bieden zijn een strikte subset van globaal gedistribueerde databases. 

Met Azure Cosmos DB klare globale verdeling ontwikkelaars hoeft niet te bouwen van hun eigen steigers replicatie door middel van het patroon Lambda (bijvoorbeeld [AWS DynamoDB replicatie](https://github.com/awslabs/dynamodb-cross-region-library/blob/master/README.md)) via het databaselogboek of door het uitvoeren van 'dubbele schrijfbewerkingen' over meerdere regio's. We doen *niet* raden deze benaderingen, omdat het is niet mogelijk om te controleren of deze methoden juist is en goed Sla's bieden. 

In dit artikel bieden we een overzicht van Azure Cosmos DB algemene distributie-mogelijkheden. We beschrijven ook Azure Cosmos DB unieke aanpak voor het leveren van uitgebreide Sla's. 

## <a id="EnableGlobalDistribution"></a>Klare globale distributiepunt inschakelen
Azure Cosmos DB biedt de volgende mogelijkheden waarmee u eenvoudig schrijven globaal gedistribueerde toepassingen. Deze mogelijkheden zijn beschikbaar via de Azure Cosmos-DB resource provider gebaseerde [REST-API's](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/) en de Azure-portal.

Bekijk de volgende video voor de functie klare globale verdeling in Azure Cosmos DB in te grijpen.

> [!VIDEO https://www.youtube.com/embed/1D06yjTVxt8]
>

### <a id="RegionalPresence"></a>De alomtegenwoordige regionale aanwezigheid 
Azure is de geografische aanwezigheid voortdurend groeiende doordat [nieuwe gebieden](https://azure.microsoft.com/regions/) online. Azure Cosmos DB is geclassificeerd als een *fundamentele service* in Azure en op alle nieuwe Azure-regio's standaard beschikbaar is. Hiermee kunt u een geografische regio koppelen aan uw Azure DB die Cosmos-databaseaccount zodra Azure Hiermee opent u het nieuwe gebied voor bedrijven.

### <a id="UnlimitedRegionsPerAccount"></a>Een onbeperkt aantal regio's koppelen aan uw Azure DB die Cosmos-databaseaccount
Azure Cosmos-database kunt u een willekeurig aantal Azure-regio's aan uw Azure Cosmos DB database-account koppelen. Er zijn geen beperkingen voor het aantal regio's die gekoppeld aan uw Azure DB die Cosmos-databaseaccount worden kunnen buiten geofencing beperkingen (bijvoorbeeld de Duitse, China). De volgende afbeelding ziet een databaseaccount geconfigureerd voor het overbruggen van 25 Azure-regio's.  

![Azure DB Cosmos-databaseaccount spanning 25 Azure-regio 's](./media/distribute-data-globally/spanning-regions.png)

**Een tenant Azure Cosmos DB database account spanning 25 Azure-regio 's**


### <a id="PolicyBasedGeoFencing"></a>Geofencing op basis van beleid
Azure Cosmos DB is ontworpen ter ondersteuning van geofencing op basis van beleid. Geofencing is een belangrijk onderdeel om te controleren of gegevens bestuur en naleving beperkingen waardoor mogelijk een specifiek gebied koppelen aan uw account. Voorbeelden van geofencing bevatten (maar niet beperkt tot) scoping globale distributie naar de regio's binnen een soevereine cloud (bijvoorbeeld China en Duitsland) of binnen een grens van een government belasting (bijvoorbeeld, Australië). Het beleid worden beheerd met behulp van de metagegevens van uw Azure-abonnement.

### <a id="DynamicallyAddRegions"></a>Dynamisch toevoegen en verwijderen van de regio 's
Azure Cosmos-database kunt u (koppelen) toevoegen of verwijderen (ontkoppelen) regio's vanuit uw databaseaccount op elk gewenst moment (Zie [voorgaande afbeelding](#UnlimitedRegionsPerAccount)). Doordat parallelle replicatie van gegevens over partities Azure Cosmos DB zorgt ervoor dat wanneer een nieuw gebied wordt toegevoegd, beschikbaar voor bewerkingen binnen 30 minuten overal ter wereld is (ervan uitgaande dat uw gegevens is 100 TBs of minder). 

### <a id="FailoverPriorities"></a>Failover-prioriteiten
Om te bepalen met exacte reeks van regionale failover in geval van een storing, kunt u om te koppelen met Azure Cosmos DB een *prioriteit* met verschillende regio's die zijn gekoppeld aan de database account (Zie de onderstaande afbeelding). Azure Cosmos DB zorgt ervoor dat de volgorde van de automatische failover plaatsvindt in de volgorde van de prioriteit die u hebt opgegeven. Zie voor meer informatie over regionale failovers [automatische regionale failovers voor bedrijfscontinuïteit in Azure Cosmos DB](regional-failover.md).


![Failover prioriteiten configureren met Azure Cosmos-DB](./media/distribute-data-globally/failover-priorities.png)

**Een tenant van Azure DB die Cosmos kunt failover-volgorde van prioriteit (rechterdeelvenster) voor de regio's die zijn gekoppeld aan een databaseaccount configureren**

### <a id="ConsistencyLevels"></a>Meerdere, goed gedefinieerde consistentie modellen voor algemeen gedistribueerde databases
Biedt ondersteuning voor Azure Cosmos DB [meerdere goed gedefinieerde intuïtieve en praktische consistentie modellen](consistency-levels.md) back-up Sla's. U kunt een specifieke consistentie model (uit de beschikbare lijst met opties), afhankelijk van de werkbelasting/scenario's. 

### <a id="TunableConsistency"></a>Instelbare consistentie voor globaal gerepliceerde databases
Azure Cosmos-database kunt u programmatisch overschrijven en de standaardkeuze voor consistentie op basis van per aanvraag tijdens runtime versoepelen. 

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Dynamisch configureerbare lezen en schrijven regio 's
Azure Cosmos-database kunt u de regio's (gekoppeld aan de database) configureren voor 'lezen', 'schrijven' of ' lezen/schrijven' regio's. 

### <a id="ElasticallyScaleThroughput"></a>Elastisch schalen doorvoer op Azure-regio 's
U kunt schalen een Azure DB die Cosmos-container door inrichting doorvoer programmatisch. De doorvoer wordt toegepast op alle gebieden van die de Azure DB die Cosmos-container wordt gedistribueerd in.

### <a id="GeoLocalReadsAndWrites"></a>Geo-local leest en schrijft
Het belangrijkste voordeel van een globaal gedistribueerde database is die dit toegang tot gegevens overal ter wereld lage latentie biedt. Azure Cosmos DB biedt lage latentie leest en schrijft op het 99th percentiel overal ter wereld. Hiermee zorgt u ervoor dat alle leesbewerkingen worden geleverd vanuit de dichtstbijzijnde regio (lokaal). Voor een leesaanvraag, wordt lokaal op de regio waarin het lezen is uitgegeven quorum gebruikt. Dit geldt ook voor schrijfbewerkingen. Een schrijfbewerking is bevestigd pas nadat een meerderheid van replica's hebben de schrijfbewerking blijvend gepleegd lokaal maar zonder wordt geregeld op externe replica's voor het bevestigen van de geschreven. Als u deze anders, werkt het protocol van de replicatie van Azure DB die Cosmos in de veronderstelling dat het quorum lezen en schrijven altijd lokaal zijn voor de regio waar de aanvraag is uitgegeven.

### <a id="ManualFailover"></a>Handmatige failover
Azure Cosmos-database kunt u voor het activeren van de failover van een databaseaccount valideren van de *complete* eigenschappen van de beschikbaarheid van de gehele toepassing (buiten de database). Omdat zowel de veiligheid en de liveness eigenschappen van de keuze van detectie en opvulteken fout gegarandeerd, wordt gegarandeerd dat Azure Cosmos DB *nul gegevensverlies* voor het uitvoeren van een handmatige failover-tenant is gestart.

### <a id="AutomaticFailover"></a>Automatische failover
Azure Cosmos DB ondersteunt automatische failover tijdens een of meer regionale uitval. Tijdens een regionale failover houdt Azure Cosmos DB de lezen latentie, bedrijfstijd beschikbaarheid, consistentie en doorvoer Sla's. Azure Cosmos DB biedt een bovengrens voor de duur van een automatische failover-bewerking is voltooid. Dit is het venster van mogelijk gegevensverlies tijdens een regionale uitval.

### <a id="GranularFailover"></a>Ontworpen voor verschillende failover granulaties
De mogelijkheden voor automatische en handmatige failover worden momenteel weergegeven op de granulatie van de account van de database. Let op: Azure Cosmos DB intern is ontworpen om u te bieden *automatische* failover fijner samenvattingen van een database, een container of zelfs een partitie (van een container die eigenaar is van een bereik van sleutels). 

### <a id="MultiHomingAPIs"></a>Multihoming in Azure Cosmos DB
Azure Cosmos-database kunt u communiceren met een database met behulp van *logische* (regio networkdirect) of *fysieke* (regiospecifieke)-eindpunten. Met behulp van de logische eindpunten zorgt ervoor dat de toepassing kunt transparant multihomed bij een failover. De laatste een fysieke-eindpunt biedt nauwkeuriger beheer aan de toepassing om te leiden leest en schrijft naar specifieke regio's.

U vindt informatie over het configureren van lezen voorkeuren voor de [SQL-API](../cosmos-db/tutorial-global-distribution-sql-api.md), [Gremlin API](../cosmos-db/tutorial-global-distribution-graph.md), [tabel API](../cosmos-db/tutorial-global-distribution-table.md), en [MongoDB API](../cosmos-db/tutorial-global-distribution-mongodb.md) in deze artikelen.

### <a id="TransparentSchemaMigration"></a>De schema- en migratie transparant en consistent database 
Azure Cosmos DB is volledig [schema networkdirect](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). De unieke ontwerp van de database-engine kunt Azure Cosmos-database moet automatisch en alle gegevens bij opname, synchroon index zonder een schema of secundaire indexen van de gebruiker. Hiermee kunt u snel herhalen van de globaal gedistribueerde toepassing zonder dat u de migratie van de schema- en index van de database of de coördinatie van meerdere fase toepassingsimplementaties van wijzigingen in het schema. Azure Cosmos DB zorgt ervoor dat eventuele wijzigingen in het beleid expliciet door u uitgevoerde indexeren niet in een verslechtering van prestaties of beschikbaarheid resulteren.  

### <a id="ComprehensiveSLAs"></a>Uitgebreide Sla's (afgezien van hoge beschikbaarheid)
Als een globaal gedistribueerde database-service biedt Azure Cosmos DB goed gedefinieerde en uitgebreide serviceovereenkomsten voor **beschikbaarheid**, **latentie**, **doorvoer**, en **consistentie** voor de database op globale schaal, ongeacht het aantal gebieden die zijn gekoppeld aan de database wordt uitgevoerd.  

## <a id="LatencyGuarantees"></a>Latentie garanties
Het belangrijkste voordeel van een globaal gedistribueerde database-service, zoals Azure Cosmos DB is op het aanbod van de lage latentie toegang tot uw gegevens overal ter wereld. Azure Cosmos DB biedt gegarandeerde lage latentie op het 99th percentiel voor verschillende databasebewerkingen. De replicatie-protocol die gebruikmaakt van Azure DB die Cosmos zorgt ervoor dat de databasebewerkingen (zowel lees- en schrijfbewerkingen) altijd worden uitgevoerd in de regio die lokaal op die van de client. De latentie SLA Azure Cosmos DB biedt garanties op het 99th percentiel voor leesbewerkingen, (synchroon) geïndexeerde schrijfbewerkingen en query's voor verschillende grootten voor aanvraag en -antwoord. De garanties latentie voor schrijfbewerkingen bevatten duurzame meerderheid quorum doorvoeracties binnen de lokale regio.

### <a id="LatencyAndConsistency"></a>Latentie van relatie met consistentiecontrole 
Voor een globaal gedistribueerde service op het aanbod van sterke consistentie in de instellingen voor een globaal gedistribueerde moet worden gerepliceerd synchroon de geschreven of synchroon regio-overschrijdende leesbewerkingen uitvoeren. De snelheid van licht en de betrouwbaarheid van WAN-netwerk bepaalt dat sterke consistentie leiden hogere latenties en beperkte beschikbaarheid van databasebewerkingen tot zal. Daarom gegarandeerd lage latenties bij de 99th percentiel en 99,99% beschikbaarheid voor alle accounts voor één regio en alle meerdere landen/regio-accounts met beperkte consistentie en 99,999% beschikbaarheid voor alle accounts van meerdere landen/regio-database, de service om te bieden moet gebruikmaken van asynchrone replicatie. Deze beurt vereist dat de service ook bieden moet [goed gedefinieerde, beperkte consistentie modellen](consistency-levels.md) – zwakkere dan sterk (om het lage latentie en beschikbaarheid garanties bieden) en in het ideale geval sterker is dan 'uiteindelijke' consistentie (met een intuïtieve programmeermodel).

Azure Cosmos DB zorgt ervoor dat er een leesbewerking is vereist voor Neem contact op met de replica's in meerdere regio's voor het leveren van een specifieke consistentie niveau garantie. Ook kan Hiermee zorgt u ervoor dat een schrijfbewerking komt niet ophalen wordt geblokkeerd terwijl de gegevens wordt gerepliceerd in alle regio's dat wil zeggen, schrijfbewerkingen worden asynchroon gerepliceerd tussen regio's). Voor meerdere landen/regio database accounts zijn beide sterk en meerdere beperkte consistentieniveaus beschikbaar. 

### <a id="LatencyAndAvailability"></a>Latentie van relatie met de beschikbaarheid van 
Zijn de twee zijden van dezelfde medaille latentie en beschikbaarheid. Latentie van een bewerking in de actieve status en beschikbaarheid in de aanwezigheid van de fouten en netwerkpartities bespreken. Vanuit het standpunt toepassing wordt langzaam uitgevoerd een databasebewerking te onderscheiden van een database die niet beschikbaar is. 

Als u wilt onderscheiden hoge latentie van niet beschikbaar zijn, biedt Azure Cosmos DB een absolute bovengrens voor latentie van verschillende databasebewerkingen. Als de databasebewerking langer dan de bovengrens duurt worden voltooid, stuurt Azure Cosmos DB een time-outfout. De SLA voor Azure Cosmos DB beschikbaarheid zorgt ervoor dat de time-outs tegen de beschikbaarheids-SLA worden geteld. 

### <a id="LatencyAndThroughput"></a>Latentie van relatie met doorvoer
Azure Cosmos-database maakt geen u kiezen tussen latentie en doorvoer. Deze zich houdt aan de SLA voor beide latentieperiode het 99th percentiel en levert de doorvoer die u hebt ingericht. 

## <a id="ConsistencyGuarantees"></a>Consistentie wordt gegarandeerd
Terwijl de [sterke consistentie model](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) is de standaard goud van gegevens programmeerbaarheid, wordt geleverd voor een ingewikkeld prijs van een hogere latentie (in de actieve status), en lagere beschikbaarheid (met betrekking tot fouten). 

Azure Cosmos DB biedt een goed gedefinieerde programmeermodel voor u tot reden van de consistentie van de gerepliceerde gegevens. Zodat u eenvoudig globaal gedistribueerde toepassingen maken met multihoming-functionaliteit zijn de consistentie modellen die worden weergegeven door Azure Cosmos DB bedoeld als regio networkdirect en onafhankelijk van de regio waar de lees- en schrijfbewerkingen zijn niet geleverd. 

SLA voor Azure Cosmos-DB consistentie wordt gegarandeerd dat 100% van leesaanvragen voldoen aan de consistentie garantie voor de consistentie model dat is opgegeven door u (ofwel op het niveau van de aanvraag of de account van de database). Een leesaanvraag wordt beschouwd als voldaan de consistentie SLA, als alle de consistentie wordt gegarandeerd dat is gekoppeld met het consistentieniveau van de is voldaan. De volgende tabel bevat de consistentie wordt gegarandeerd dat correspondeert met specifieke consistentie modellen die worden aangeboden door Azure Cosmos DB.

<table>
    <tr>
        <td><strong>Consistentie Model</strong></td>
        <td><strong>Consistentie kenmerken</strong></td>
        <td><strong>SLA</strong></td>
    </tr>
        <tr>
        <td>Sterk</td>
        <td>Linearizable</td>
        <td>100%</td>
    </tr>
    <tr>
        <td rowspan="3">Gebonden veroudering</td>
        <td>Monotone gelezen (binnen een regio)</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Consistente voorvoegsel</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Gebonden veroudering &lt; K, T</td>
        <td>100%</td>
    </tr>
<tr>
        <td rowspan="3">Sessie</td>
        <td>Lezen van uw eigen schrijven</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Monotone lezen</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Consistente voorvoegsel</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Consistente voorvoegsel</td>
        <td>Consistente voorvoegsel</td>
        <td>100%</td>
    </tr>
</table>

**Consistentie wordt gegarandeerd die zijn gekoppeld aan een model opgegeven consistentie in Azure Cosmos-DB**


### <a id="ConsistencyAndAvailability"></a>Relatie van de consistentie met beschikbaarheid
De [onmogelijk resultaat](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) van de [CAP stelling van](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) blijkt dat het inderdaad onmogelijk om het systeem beschikbaar blijven en bieden linearizable consistentie met betrekking tot fouten is. De database-service moet kiezen CP of Azië, waarbij CP systemen beschikbaarheid voor linearizable consistentie beheercluster terwijl de systemen Azië beheercluster [linearizable consistentie](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) voor beschikbaarheid. Azure Cosmos DB is nooit in strijd met het model aangevraagde consistentie, waardoor formeel een CP-systeem. In de praktijk is consistentie echter niet een toegevoegde alles of niets; Er zijn meerdere goed gedefinieerde consistentie modellen ligt binnen het spectrum consistentie tussen linearizable en uiteindelijke consistentie. In Azure Cosmos DB identificeert verschillende beperkte consistentie modellen die zijn van toepassing op concrete scenario's en intuïtieve te gebruiken. Azure Cosmos DB navigeert de voor-en nadelen consistentie beschikbaarheid door het aanbieden van een [meerdere versoepeld nog goed gedefinieerde consistentie modellen](consistency-levels.md) en een 99,99% beschikbaarheid voor alle enkele regio database accounts en 99,999% lezen en schrijven beschikbaarheid voor alle accounts voor meerdere landen/regio-database. 

### <a id="ConsistencyAndAvailability"></a>Consistentie van relatie met een latentie
Een uitgebreidere variatie CAP stelling van heet [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), die ook is verantwoordelijk voor de latentie en consistentie-en nadelen in een constante status. Dit geeft aan dat de database-systeem in een constante status tussen de consistentie en latentie kiezen moet. Azure Cosmos DB met meerdere beperkte consistentie modellen (ondersteund door de asynchrone replicatie en lokaal lezen en schrijven quorum), zorgt u ervoor dat alle lees- en schrijfbewerkingen lokaal voor het lezen zijn en regio's respectievelijk schrijven. Hiermee kunt Azure Cosmos-database moet lage latentie garanties binnen de regio voor de opgegeven consistentie-modellen bieden.  

### <a id="ConsistencyAndThroughput"></a>De relatie van de consistentie met doorvoer
Omdat de implementatie van een specifieke consistentie model afhankelijk van de keuze van is een [quorumtype](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), doorvoer varieert op basis van de keuze van een model van de consistentie. Bijvoorbeeld in Azure Cosmos DB de RU kosten voor sterke consistent leesbewerkingen is ongeveer *dubbele* die uiteindelijk consistent is gelezen. In dit geval moet u voor het inrichten van dubbele de RUs zodat dezelfde doorvoer wordt gehaald.


![Relatie tussen de consistentie en doorvoer](./media/distribute-data-globally/consistency-and-throughput.png)

**Relatie van meer capaciteit voor een specifieke consistentie-model in Azure Cosmos-DB**

## <a id="ThroughputGuarantees"></a>Doorvoer garanties 
Azure Cosmos-database kunt u schaal doorvoer (evenals, opslag), elastisch via verschillende regio's, afhankelijk van uw behoeften of de aanvraag. 

![Azure Cosmos DB gedistribueerd en gepartitioneerde verzamelingen](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

**Een enkele Azure DB die Cosmos-container horizontaal gepartitioneerd (in drie resource partities binnen een regio) en vervolgens globaal zijn verdeeld over drie Azure-regio 's**

Een Azure DB die Cosmos-container opgehaald verdeeld in twee dimensies (i) binnen een regio en (ii) tussen regio's. Dit doet u al volgt: 

* **Lokale distributiepunten**: binnen één regio een Azure DB die Cosmos-container is horizontaal uitgebreid in termen van *resource partities*. Elke partitie resource beheert een set van sleutels en sterke consistent en maximaal beschikbare wordt fysiek vertegenwoordigd door vier replica's ook wel een *replicaset* en replicatie van de machine de status van deze replica's. Azure Cosmos-database is een systeem volledig resource geregeld waar een resource-partitie is verantwoordelijk voor het leveren van een deel van de doorvoer voor het budget van systeemresources die zijn toegewezen. De schaal van een Azure DB die Cosmos-container is transparant voor de gebruikers. Azure Cosmos DB de resource-partities beheert en splitst en voegt deze zo nodig als opslag samen en doorvoer vereisten veranderen. 
* **Globale distributie**: als het een database meerdere landen/regio, elk van de partities van de resource vervolgens is verdeeld over de regio's. Resource-partities die eigenaar is van dezelfde reeks sleutels over verschillende regio's formulier *partitieset* (Zie [voorgaande afbeelding](#ThroughputGuarantees)).  Resource partities binnen een partitieset worden gecoördineerd met behulp van replicatie van machine staat over meerdere regio's die zijn gekoppeld aan de database. Afhankelijk van het consistentieniveau is geconfigureerd, worden de resource-partities binnen een partitieset geconfigureerd dynamisch met verschillende topologieën (bijvoorbeeld ster, keten, structuur enz.). 

Doordat een uiterst responsieve partitie management, taakverdeling en strikte resource beheeracties kunt Azure Cosmos DB u elastisch schalen doorvoer over meerdere Azure-regio's die zijn gekoppeld aan een Azure DB die Cosmos-container of de database. Het wijzigen van de ingerichte doorvoer is een Runtimebewerking in Azure Cosmos DB. Net als bij andere databasebewerkingen, Azure Cosmos DB wordt gegarandeerd dat de absolute bovengrens voor latentie voor uw aanvraag voor het wijzigen van de ingerichte doorvoer. De volgende afbeelding ziet u bijvoorbeeld een klant container met elastisch ingerichte doorvoer (variëren van 1-10M aanvragen per seconde tussen twee regio's) op basis van de vraag.

![Elastisch ingerichte doorvoer Azure Cosmos-DB](./media/distribute-data-globally/elastic-throughput.png)

**Een klant-container met elastisch ingerichte doorvoer (variëren van 1-10M aanvragen per seconde)**

### <a id="ThroughputAndConsistency"></a>Doorvoer van relatie met consistentiecontrole 
Dit is hetzelfde zoals beschreven in [relatie van de consistentie met doorvoer](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Relatie met de beschikbaarheid van de doorvoer
Azure Cosmos DB blijft de hoge beschikbaarheid behouden wanneer wijzigingen zijn aangebracht aan de ingerichte doorvoer. Azure Cosmos DB transparant resource partities beheert (en voert gesplitste, samenvoegen en kloon operations) en zorgt ervoor dat de bewerkingen kunnen geen nadelige invloed op prestaties of beschikbaarheid, terwijl de toepassing elastisch verhoogt of verlaagt u Hiermee doorvoer. 

## <a id="AvailabilityGuarantees"></a>Beschikbaarheidsgaranties
Azure Cosmos DB biedt een 99,99% beschikbaarheid SLA voor alle accounts voor één regio-database en alle meerdere landen/regio-accounts met beperkte consistentie en 99,999% beschikbaarheid voor alle accounts voor meerdere landen/regio-database. Zoals eerder beschreven, omvatten beschikbaarheidsgaranties Azure Cosmos DB een absolute bovengrens voor latentie voor elke vlak gegevens en bewerkingen. De beschikbaarheidsgaranties wijzigen niet met het aantal regio's of de geografische afstand tussen regio's. Beschikbaarheidsgaranties zijn van toepassing met betrekking tot zowel handmatige als automatische failovers. Azure Cosmos DB biedt transparante multihoming API's die ervoor zorgen dat uw toepassing kan worden uitgevoerd tegen logische eindpunten en de aanvragen naar een nieuw gebied in geval van een failover kan transparant sturen. Uw toepassing hoeft niet te worden geïmplementeerd voor het geval van een regionale failover en de beschikbaarheid van de SLA's te allen tijde worden behouden.

### <a id="AvailabilityAndConsistency"></a>Beschikbaarheid van relatie met de consistentiecontrole, latentie en doorvoer
Beschikbaarheid van relatie met de consistentiecontrole, latentie en doorvoer wordt beschreven in de secties [relatie met de beschikbaarheid van de consistentie](#ConsistencyAndAvailability), [relatie met de beschikbaarheid van de latentie](#LatencyAndAvailability) en [Relatie met de beschikbaarheid van de doorvoer](#ThroughputAndAvailability). 

## <a id="CustomerFacingSLAMetrics"></a>Klantgerichte serviceovereenkomstmetrieken
Azure Cosmos DB beschrijft transparant de metrische gegevens doorvoer, latentie, consistentie en beschikbaarheid. Deze metrische gegevens zijn toegankelijk via programmacode of via Azure portal (Zie de onderstaande afbeelding). U kunt ook waarschuwingen op verschillende drempelwaarden voor gebruik van Azure Application Insights instellen.
 

![Azure DB Cosmos zichtbaar is voor een klant serviceovereenkomstmetrieken](./media/distribute-data-globally/customer-slas.png)

**Consistentie, latentie, doorvoer en beschikbaarheid metrische gegevens transparant beschikbaar zijn voor elke tenant**

## <a id="Next Steps"></a>Volgende stappen
* Als u wilt implementeren globale replicatie van uw Azure DB die Cosmos-account met de Azure portal, Zie [het uitvoeren van Azure DB die Cosmos globale databasereplicatie met de Azure portal](tutorial-global-distribution-sql-api.md).
* Zie voor meer informatie over het implementeren van meerdere masters architecturen met Azure Cosmos DB, [hoofddatabase meerdere architecturen met Azure Cosmos DB](multi-region-writers.md).
* Zie voor meer informatie over de werking van automatische en handmatige failover werken in Azure Cosmos DB [regionale Failovers in Azure Cosmos DB](regional-failover.md).

## <a id="References"></a>Verwijzingen
1. Eric Brewer. [Voor robuuste gedistribueerde systemen](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric Brewer. [KAPJE twaalf jaar Later – hoe de regels zijn gewijzigd](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Geurts, Lynch. - [Brewer&#39;s gissingen en haalbaarheid van consistente, beschikbare partitie fouttolerante webservices](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Daniel Abadi. [Consistentiecontrole voor-en nadelen in moderne gedistribueerde Database systemen ontwerp](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. To Kleppmann. [Stop het aanroepen van databases CP of Azië](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis et al. [Probabilistische gebonden veroudering (PBS) voor praktische gedeeltelijke quorum](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor en wol. [Laden en de capaciteit en beschikbaarheid in een Quorum-systemen](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy en volgende. [Lineralizability: Een juistheid voorwaarde voor gelijktijdige objecten](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [SLA voor Azure Cosmos-DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
