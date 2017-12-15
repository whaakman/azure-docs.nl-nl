---
title: Gegevens met Azure Cosmos DB globaal distribueren | Microsoft Docs
description: Meer informatie over de wereld scale geo-replicatie, failover en gegevens herstellen met globale databases van Azure Cosmos DB, een globaal gedistribueerd, zijn voor het model database-service.
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: ba5ad0cc-aa1f-4f40-aee9-3364af070725
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: arramac
ms.openlocfilehash: 0be81802996f27a4c063e4e728a3c95ad757bea0
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-distribute-data-globally-with-azure-cosmos-db"></a>Het distribueren van gegevens met Azure Cosmos DB globaal
Azure is alomtegenwoordige - heeft een algemene footprint tussen 30 + geografische regio's en continu groter wordt. Met de wereldwijde aanwezigheid is een van de gedifferentieerde mogelijkheden die Azure voor de ontwikkelaars biedt de mogelijkheid om te maken, implementeren en globaal gedistribueerde toepassingen eenvoudig te beheren. 

[Azure Cosmos DB](../cosmos-db/introduction.md) is de wereldwijd gedistribueerde databaseservice met meerdere modellen van Microsoft voor essentiële toepassingen. Azure Cosmos DB biedt klare distributielijsten [elastisch schalen van doorvoer en opslag](../cosmos-db/partition-data.md) overal ter wereld, één cijfer milliseconde latenties op het 99th percentiel [vijf goed gedefinieerde consistentieniveaus](consistency-levels.md), hoge beschikbaarheid, alle back-ups door gegarandeerd [toonaangevende Sla's](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Met Azure Cosmos DB worden [gegevens automatisch geïndexeerd](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), zonder dat u te maken krijgt met schema- en indexbeheer. Azure Cosmos DB beschikt over meerdere modellen en ondersteunt modellen voor document-, sleutelwaarde-, grafiek- en kolomgegevens. Als een service cloud geboren is Azure DB die Cosmos zorgvuldig ontworpen met multi-tenancymodus en globale verdeling van de grond.

**Één Azure Cosmos DB verzameling gepartitioneerd en verdeeld over meerdere Azure-regio 's**

![Azure DB Cosmos-verzameling is gepartitioneerd en verdeeld over drie gebieden](./media/distribute-data-globally/global-apps.png)

Als er tijdens het maken van Azure DB die Cosmos hebt geleerd, toe te voegen globale verdeling kan niet worden pas achteraf overwogen - deze mag niet 'bout eenmalige aanmelding' op een systeem van de database "één site". De mogelijkheden die worden aangeboden door een wereldwijd gedistribueerde database span afgezien van dat van traditionele geografische disaster recovery (geografisch DR) die worden aangeboden door 'één site' databases. Databases van één site Geo-DR-mogelijkheid bieden zijn een strikte subset van globaal gedistribueerde databases. 

Met Azure Cosmos DB klare globale verdeling ontwikkelaars hoeft niet te bouwen van hun eigen steigers replicatie door middel van het patroon Lambda (bijvoorbeeld [AWS DynamoDB replicatie](https://github.com/awslabs/dynamodb-cross-region-library/blob/master/README.md)) via het databaselogboek of op deze manier ' dubbele schrijft' over meerdere regio's. We raden deze benaderingen niet omdat het is niet mogelijk om te controleren of deze methoden juist is en goed Sla's bieden. 

In dit artikel bieden we een overzicht van Azure Cosmos DB algemene distributie-mogelijkheden. We beschrijven ook Azure Cosmos DB unieke aanpak voor het leveren van uitgebreide Sla's. 

## <a id="EnableGlobalDistribution"></a>Klare globale distributiepunt inschakelen
Azure Cosmos DB biedt de volgende mogelijkheden waarmee u eenvoudig wereld schaal om toepassingen te schrijven. Deze mogelijkheden zijn beschikbaar via de Azure Cosmos-DB resource provider gebaseerde [REST-API's](https://docs.microsoft.com/rest/api/documentdbresourceprovider/) en de Azure-portal.

### <a id="RegionalPresence"></a>De alomtegenwoordige regionale aanwezigheid 
Azure is de geografische aanwezigheid voortdurend groeiende doordat [nieuwe gebieden](https://azure.microsoft.com/regions/) online. Azure Cosmos DB is beschikbaar in alle nieuwe Azure-regio's standaard. Hiermee kunt u een geografische regio koppelen aan uw Azure DB die Cosmos-databaseaccount zodra Azure Hiermee opent u het nieuwe gebied voor bedrijven.

### <a id="UnlimitedRegionsPerAccount"></a>Een onbeperkt aantal regio's koppelen aan uw Azure DB die Cosmos-databaseaccount
Azure Cosmos-database kunt u een willekeurig aantal Azure-regio's aan uw Azure Cosmos DB database-account koppelen. Er zijn geen beperkingen voor het aantal regio's die gekoppeld aan uw Azure DB die Cosmos-databaseaccount worden kunnen buiten geofencing beperkingen (bijvoorbeeld de Duitse, China). De volgende afbeelding ziet een databaseaccount geconfigureerd voor het overbruggen van 25 Azure-regio's.  

**Een tenant Azure Cosmos DB database account spanning 25 Azure-regio 's**

![Azure DB Cosmos-databaseaccount spanning 25 Azure-regio 's](./media/distribute-data-globally/spanning-regions.png)

### <a id="PolicyBasedGeoFencing"></a>Geofencing op basis van beleid
Azure Cosmos-DB is ontworpen voor geofencing-mogelijkheden op basis van beleid. Geofencing is een belangrijk onderdeel om te controleren of gegevens bestuur en naleving beperkingen waardoor mogelijk een specifiek gebied koppelen aan uw account. Voorbeelden van geofencing bevatten (maar niet beperkt tot), scoping globale distributie naar de regio's binnen een soevereine cloud (bijvoorbeeld China en Duitsland) of binnen een grens van een government belasting (bijvoorbeeld, Australië). Het beleid worden beheerd met behulp van de metagegevens van uw Azure-abonnement.

### <a id="DynamicallyAddRegions"></a>Dynamisch toevoegen en verwijderen van de regio 's
Azure Cosmos-database kunt u (koppelen) toevoegen of verwijderen (ontkoppelen)-regio's aan uw databaseaccount op elk gewenst moment (Zie [voorgaande afbeelding](#UnlimitedRegionsPerAccount)). Doordat het repliceren van gegevens meerdere partities parallel Azure Cosmos DB zorgt ervoor dat wanneer een nieuw gebied online wordt gezet, Azure Cosmos DB beschikbaar binnen 30 minuten overal ter wereld voor maximaal 100 TBs. 

### <a id="FailoverPriorities"></a>Failover-prioriteiten
Exacte reeks regionale failovers beheren wanneer er een storing meerdere regionale Azure Cosmos DB schakelt u de prioriteit die tot verschillende regio's die zijn gekoppeld aan de database koppelen account (Zie de volgende afbeelding). Azure Cosmos DB zorgt ervoor dat de volgorde van de automatische failover plaatsvindt in de volgorde van de prioriteit die u hebt opgegeven. Zie voor meer informatie over regionale failovers [automatische regionale failovers voor bedrijfscontinuïteit in Azure Cosmos DB](regional-failover.md).

**Een tenant van Azure DB die Cosmos kunt failover-volgorde van prioriteit (rechterdeelvenster) voor de regio's die zijn gekoppeld aan een databaseaccount configureren**

![Failover prioriteiten configureren met Azure Cosmos-DB](./media/distribute-data-globally/failover-priorities.png)

### <a id="ConsistencyLevels"></a>Meerdere, goed gedefinieerde consistentie modellen voor algemeen gerepliceerde databases
Beschrijft de Azure Cosmos DB [meerdere goed gedefinieerde consistentieniveaus](consistency-levels.md) back-up Sla's. U kunt een specifieke consistentie model (uit de beschikbare lijst met opties), afhankelijk van de werkbelasting/scenario's. 

### <a id="TunableConsistency"></a>Instelbare consistentie voor globaal gerepliceerde databases
Azure Cosmos-database kunt u programmatisch overschrijven en versoepelen standaardkeuze consistentiecontrole op een basis per aanvraag tijdens runtime. 

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Dynamisch configureerbare lezen en schrijven regio 's
Azure Cosmos-database kunt u de regio's (gekoppeld aan de database) configureren voor 'lezen', 'schrijven' of ' lezen/schrijven' regio's. 

### <a id="ElasticallyScaleThroughput"></a>Elastisch schalen doorvoer op Azure-regio 's
U kunt schalen een verzameling Azure Cosmos DB door inrichting doorvoer programmatisch. De doorvoer wordt toegepast op alle gebieden van die de verzameling wordt gedistribueerd in.

### <a id="GeoLocalReadsAndWrites"></a>Geo-local leest en schrijft
Het belangrijkste voordeel van een globaal gedistribueerde database is op het aanbod van de lage latentie toegang tot de gegevens overal ter wereld. Azure Cosmos DB biedt lage latentie garanties op P99 voor verschillende databasebewerkingen. Hiermee zorgt u ervoor dat alle leesbewerkingen worden doorgestuurd naar de dichtstbijzijnde lokale lezen regio. Voor een leesaanvraag, wordt het quorum lokaal op de regio waarin het lezen is uitgegeven gebruikt; Dit geldt ook voor de geschreven. Een schrijfbewerking is bevestigd pas nadat een meerderheid van replica's heeft de schrijfbewerking blijvend toegekend lokaal maar zonder wordt geregeld op externe replica's voor het bevestigen van de geschreven. Anders gezegd, werkt het protocol van de replicatie van Azure DB die Cosmos in de veronderstelling dat het quorum lezen en schrijven altijd lokaal voor het lezen zijn en regio's, respectievelijk schrijven waarin de aanvraag is uitgegeven.

### <a id="ManualFailover"></a>Handmatig starten van regionale failover
Azure Cosmos-database kunt u voor het activeren van de failover van de account van de database om te valideren de *complete* eigenschappen van de beschikbaarheid van de gehele toepassing (buiten de database). Aangezien de veiligheid en de liveness eigenschappen van de keuze van detectie en opvulteken fout gegarandeerd, wordt gegarandeerd dat Azure Cosmos DB *nul gegevensverlies* voor het uitvoeren van een handmatige failover-tenant is gestart.

### <a id="AutomaticFailover"></a>Automatische failover
Azure Cosmos DB ondersteunt automatische failover in geval van een of meer regionale uitval. Tijdens een regionale failover houdt Azure Cosmos DB de lezen latentie, bedrijfstijd beschikbaarheid, consistentie en doorvoer Sla's. Azure Cosmos DB biedt een bovengrens van de duur van een automatische failover-bewerking te voltooien. Dit is het venster van mogelijk gegevensverlies tijdens de regionale onderbreking.

### <a id="GranularFailover"></a>Ontworpen voor verschillende failover granulaties
De mogelijkheden voor automatische en handmatige failover worden momenteel weergegeven op de granulatie van de account van de database. Let op: Azure Cosmos DB intern is ontworpen om u te bieden *automatische* failover fijner samenvattingen van een database, de verzameling of zelfs een partitie (van een verzameling die eigenaar is van een bereik van sleutels). 

### <a id="MultiHomingAPIs"></a>Multihoming API's in Azure Cosmos DB
Azure Cosmos-database kunt u communiceren met de database met een logische (regio networkdirect) of fysieke (regiospecifieke)-eindpunten. Met behulp van de logische eindpunten zorgt ervoor dat de toepassing kunt transparant multihomed in geval van een failover. De eindpunten laatstgenoemde, fysieke bieden nauwkeuriger beheer aan de toepassing om te leiden leest en schrijft naar specifieke regio's.

U vindt informatie over het configureren van lezen voorkeuren voor de [SQL-API](../cosmos-db/tutorial-global-distribution-sql-api.md), [Graph API](../cosmos-db/tutorial-global-distribution-graph.md), [tabel API](../cosmos-db/tutorial-global-distribution-table.md), en [MongoDB API](../cosmos-db/tutorial-global-distribution-mongodb.md) in hun respectieve gekoppelde artikelen.

### <a id="TransparentSchemaMigration"></a>De schema- en migratie transparant en consistent database 
Azure Cosmos DB is volledig [schema networkdirect](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Het ontwerp van de database-engine van de unieke toestaat dat deze automatisch en alle gegevens die deze opgenomen zonder dat een schema of secundaire indexen van u synchroon te indexeren. Hiermee kunt u snel herhalen van de globaal gedistribueerde toepassing zonder dat u de migratie van de schema- en index van de database of de coördinatie van meerdere fase toepassingsimplementaties van wijzigingen in het schema. Azure Cosmos DB zorgt ervoor dat eventuele wijzigingen in het beleid expliciet door u uitgevoerde indexeren resulteert niet in een vermindering van prestaties of beschikbaarheid.  

### <a id="ComprehensiveSLAs"></a>Uitgebreide Sla's (anders dan alleen hoge beschikbaarheid)
Als een globaal gedistribueerde database-service biedt Azure Cosmos DB goed gedefinieerde SLA voor **verlies van gegevens**, **beschikbaarheid**, **latentieperiode P99**, **doorvoer**  en **consistentie** voor de database als geheel, ongeacht het aantal gebieden die zijn gekoppeld aan de database.  

## <a id="LatencyGuarantees"></a>Latentie garanties
Het belangrijkste voordeel van een globaal gedistribueerde database-service, zoals Azure Cosmos DB is op het aanbod van de lage latentie toegang tot uw gegevens overal ter wereld. Azure Cosmos DB biedt gegarandeerde lage latentie op P99 voor verschillende databasebewerkingen. De replicatie-protocol die gebruikmaakt van Azure DB die Cosmos zorgt ervoor dat de databasebewerkingen (in het ideale geval leest en schrijft) worden altijd uitgevoerd in de regio die lokaal op die van de client. De latentie SLA Azure Cosmos DB omvat P99 voor leesbewerkingen, (synchroon) geïndexeerde schrijfbewerkingen en query's voor verschillende grootten voor aanvraag en -antwoord. De garanties latentie voor schrijfbewerkingen bevatten duurzame meerderheid quorum doorvoeringen in het lokale datacenter.

### <a id="LatencyAndConsistency"></a>Latentie van relatie met consistentiecontrole 
Voor een globaal gedistribueerde service op het aanbod van sterke consistentie in de instellingen voor een globaal gedistribueerd die moeten worden synchroon gerepliceerd de geschreven of synchrone regio-overschrijdende leesbewerkingen: uitvoeren van de snelheid van licht en de betrouwbaarheid van WAN-netwerk dicteert dat strong consistentie leiden tot hogere latenties en beperkte beschikbaarheid van databasebewerkingen. Daarom gegarandeerd lage latenties op P99 en 99,99% beschikbaarheid voor alle één regio en alle meerdere landen/regio-accounts met beperkte consistentie om te bieden en 99,999% lezen beschikbaarheid voor alle accounts voor meerdere landen/regio-database, de service moet gebruiken asynchrone replicatie. Deze beurt vereist dat de service ook bieden moet [goed gedefinieerde, beperkte consistentie choice(s)](consistency-levels.md) : zwakkere dan sterk (om het lage latentie en beschikbaarheid garanties bieden) en in het ideale geval sterker is dan 'uiteindelijke' consistentie (tot bieden een intuïtieve programmeermodel).

Azure Cosmos DB zorgt ervoor dat er een leesbewerking is vereist voor Neem contact op met de replica's in meerdere regio's leveren de niveau-garantie van specifieke consistentie. Ook kan Hiermee zorgt u ervoor dat een schrijfbewerking komt niet ophalen wordt geblokkeerd terwijl de gegevens in alle regio's (dat wil zeggen schrijfbewerkingen zijn asynchroon gerepliceerd tussen regio's) wordt gerepliceerd. Meerdere beperkte consistentieniveaus zijn beschikbaar voor accounts voor meerdere landen/regio-database. 

### <a id="LatencyAndAvailability"></a>Latentie van relatie met de beschikbaarheid van 
Zijn de twee zijden van dezelfde medaille latentie en beschikbaarheid. We hebben over de latentie van de bewerking in de actieve status en beschikbaarheid, met betrekking tot fouten. Vanuit het standpunt toepassing wordt langzaam uitgevoerd een databasebewerking te onderscheiden van een database die niet beschikbaar is. 

Als u wilt onderscheiden hoge latentie van niet beschikbaar zijn, biedt Azure Cosmos DB een absolute bovengrens op latentie van verschillende databasebewerkingen. Als de databasebewerking langer dan de bovengrens duurt worden voltooid, stuurt Azure Cosmos DB een time-outfout. De SLA voor Azure Cosmos DB beschikbaarheid zorgt ervoor dat de time-outs tegen de beschikbaarheids-SLA worden geteld. 

### <a id="LatencyAndThroughput"></a>Latentie van relatie met doorvoer
Azure Cosmos-database maakt geen u kiezen tussen latentie en doorvoer. Deze zich houdt aan de SLA voor beide latentieperiode P99 en leveren de doorvoer die u hebt ingericht. 

## <a id="ConsistencyGuarantees"></a>Consistentie wordt gegarandeerd
Terwijl de [sterke consistentie model](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) is de standaard goud van programmeerbaarheid, wordt geleverd voor de steile prijs van hogere latentie (in de actieve status), en lagere beschikbaarheid (met betrekking tot fouten). 

Azure Cosmos DB biedt een goed gedefinieerde programmeermodel u tot reden van de consistentie van de gerepliceerde gegevens. De consistentiecontrole modellen die worden weergegeven door Azure Cosmos DB zijn zodat u multihomed toepassingen bouwen ontworpen regio networkdirect en niet afhankelijk van de regio waar de lees- en schrijfbewerkingen worden geleverd. 

SLA voor Azure Cosmos-DB-consistentie wordt gegarandeerd dat 100% van leesaanvragen voldoen aan de garantie consistentie voor het consistentieniveau van de aangevraagd door u (de consistentie standaardniveau voor de databaseaccount) ofwel de overschreven waarde van de aanvraag. Een leesaanvraag wordt beschouwd als aan de SLA voor consistentie voldaan als alle de consistentie wordt gegarandeerd dat is gekoppeld met het consistentieniveau van de is voldaan. De volgende tabel bevat de consistentie wordt gegarandeerd dat correspondeert met specifieke consistentieniveaus die worden aangeboden door Azure Cosmos DB.

**Consistentie wordt gegarandeerd die zijn gekoppeld aan een bepaalde consistentieniveau in Azure Cosmos-DB**

<table>
    <tr>
        <td><strong>Consistentieniveau</strong></td>
        <td><strong>Consistentie kenmerken</strong></td>
        <td><strong>SLA</strong></td>
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
        <td>Consistente voorvoegsel</td>
        <td>Consistente voorvoegsel</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Sterk</td>
        <td>Linearizable</td>
        <td>100%</td>
    </tr>
</table>

### <a id="ConsistencyAndAvailability"></a>Relatie van de consistentie met beschikbaarheid
De [onmogelijk resultaat](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) van de [CAP stelling van](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) blijkt dat het inderdaad onmogelijk voor het systeem beschikbaar blijven en bieden linearizable consistentie met betrekking tot fouten is. De database-service moet kiezen CP of AP - CP systemen beheercluster beschikbaarheid voor linearizable consistentie terwijl de systemen Azië beheercluster [linearizable consistentie](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) voor beschikbaarheid. Azure Cosmos DB schendt nooit de aangevraagde consistentieniveau, waardoor formeel een CP-systeem. Echter, in de praktijk consistentiecontrole is niet een toegevoegde alles of niets: Er zijn meerdere goed gedefinieerde consistentie modellen ligt binnen het spectrum consistentie tussen linearizable en uiteindelijke consistentie. We hebben geprobeerd in Azure Cosmos DB verschillende van de modellen beperkte consistentie met concrete toepasselijkheid en een intuïtieve programmeermodel te identificeren. Azure Cosmos DB navigeert de voor-en nadelen consistentie beschikbaarheid door het aanbieden van een [meerdere versoepeld nog goed gedefinieerde consistentieniveaus](consistency-levels.md) en een 99,99% beschikbaarheid voor alle accounts voor één regio en alle meerdere landen/regio-accounts met consistentie versoepeld en 99,999% gelezen beschikbaarheid voor alle accounts voor meerdere landen/regio-database. 

### <a id="ConsistencyAndAvailability"></a>Consistentie van relatie met een latentie
Een uitgebreidere variatie CAP is voorgesteld door Prof. Daniel Abadi en deze wordt aangeroepen [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), die ook is verantwoordelijk voor de latentie en consistentie-en nadelen in de actieve status. Dit geeft aan dat in de actieve status, het databasesysteem tussen de consistentie en latentie kiezen moet. Azure Cosmos DB met meerdere beperkte consistentie modellen (ondersteund door de asynchrone replicatie en lokale lezen, schrijven quorum), zorgt u ervoor dat alle lees- en schrijfbewerkingen lokaal voor het lezen zijn en regio's respectievelijk schrijven.  Hiermee kunt Azure Cosmos DB lage latentie wordt gegarandeerd dat binnen de regio voor de consistentieniveaus aanbieden.  

### <a id="ConsistencyAndThroughput"></a>De relatie van de consistentie met doorvoer
Omdat de implementatie van een specifieke consistentie model afhankelijk van de keuze van is een [quorumtype](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), de doorvoer varieert op basis van de keuze van de consistentie. Bijvoorbeeld, is de RU kosten voor sterke consistent leesbewerkingen in Azure Cosmos DB min of meer dubbele dat van uiteindelijk consistent leest. In dit geval moet u voor het inrichten van dubbele RUs op uw verzameling zodat dezelfde doorvoer wordt gehaald.
 
**Relatie van meer capaciteit voor een specifieke consistentieniveau in Azure Cosmos-DB**

![Relatie tussen de consistentie en doorvoer](./media/distribute-data-globally/consistency-and-throughput.png)

## <a id="ThroughputGuarantees"></a>Doorvoer garanties 
Azure Cosmos-database kunt u schaal doorvoer (evenals, opslag), elastisch over verschillende regio's, afhankelijk van de vraag. 

**Één Azure Cosmos DB verzameling gepartitioneerd (in drie shards) en vervolgens verdeeld over drie Azure-regio 's**

![Azure Cosmos DB gedistribueerd en gepartitioneerde verzamelingen](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

Een verzameling Azure Cosmos DB opgehaald gedistribueerd met behulp van twee dimensies – binnen een regio en vervolgens tussen regio's. Dit doet u al volgt: 

* In één regio is een verzameling Azure Cosmos DB uitgebreid in termen van de resource-partities. Elke partitie resource beheert een set van sleutels en sterke consistent en maximaal beschikbaar is op grond van de replicatie van machine staat tussen een set van replica's. Azure Cosmos-database is een systeem volledig resource geregeld waar een resource-partitie is verantwoordelijk voor het leveren van een deel van de doorvoer voor het budget systeembronnen toegewezen. De schaal van een verzameling Azure Cosmos DB is volledig transparant – Azure Cosmos DB beheert de resource-partities en splitst en zo nodig Hiermee worden samengevoegd. 
* Elk van de resource-partities wordt vervolgens verdeeld over meerdere regio's. Resource-partities die eigenaar is van dezelfde reeks sleutels over verschillende regio's vormen partitieset (Zie [voorgaande afbeelding](#ThroughputGuarantees)).  Resource partities binnen een partitieset worden gecoördineerd met behulp van replicatie van machine staat over de meerdere regio's. Afhankelijk van het consistentieniveau is geconfigureerd, worden de resource-partities binnen een partitieset geconfigureerd dynamisch met verschillende topologieën (bijvoorbeeld ster, keten, structuur enz.). 

Doordat een uiterst responsieve partitie management, taakverdeling en strikte resource beheeracties kunt Azure Cosmos DB u elastisch schalen doorvoer over meerdere Azure-regio's op een verzameling Azure Cosmos DB. Veranderende doorvoer op een verzameling is een Runtimebewerking in Azure DB die Cosmos - zoals met andere databasebewerkingen Azure Cosmos DB gegarandeerd dat de absolute bovengrens voor latentie voor uw aanvraag voor het wijzigen van de doorvoer. De volgende afbeelding ziet een voorbeeld van een klant-verzameling in met elastisch ingerichte doorvoer (variëren van 1-10M aanvragen per seconde tussen twee regio's) op basis van de vraag.
 
**Een klant-verzameling met elastisch ingerichte doorvoer (1-10M aanvragen per seconde)**

![Elastisch ingerichte doorvoer Azure Cosmos-DB](./media/distribute-data-globally/elastic-throughput.png)

### <a id="ThroughputAndConsistency"></a>Doorvoer van relatie met consistentiecontrole 
Hetzelfde als [relatie van de consistentie met doorvoer](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Relatie met de beschikbaarheid van de doorvoer
Azure Cosmos DB blijft de beschikbaarheid handhaven wanneer de wijzigingen worden aangebracht in de doorvoer. Azure Cosmos DB beheert transparant partities (bijvoorbeeld gesplitste, samenvoegen, kloon operations) en zorgt ervoor dat de bewerkingen kunnen geen nadelige invloed op prestaties of beschikbaarheid, terwijl de toepassing elastisch verhoogt of verlaagt u Hiermee doorvoer. 

## <a id="AvailabilityGuarantees"></a>Beschikbaarheidsgaranties
Azure Cosmos DB biedt een 99,99% beschikbaarheid SLA voor alle één regio en alle meerdere landen/regio-accounts met beperkte consistentie en 99,999% gelezen beschikbaarheid voor alle accounts voor meerdere landen/regio-database. Zoals eerder beschreven, omvatten beschikbaarheidsgaranties Azure Cosmos DB een absolute bovengrens voor de latentie voor elke vlak gegevens en bewerkingen. De beschikbaarheidsgaranties solide eenheid zijn en niet wijzigen met het aantal gebieden of geografische afstand tussen regio's. Beschikbaarheidsgaranties toepassen met zowel handmatig als, automatische failover. Azure Cosmos DB biedt transparante multihoming API's die ervoor zorgen dat uw toepassing kan worden uitgevoerd tegen logische eindpunten en de aanvragen naar de nieuwe regio in geval van een failover kan transparant sturen. Anders gezegd, uw toepassing hoeft niet te worden geïmplementeerd bij regionale failover en de serviceovereenkomsten beschikbaarheid worden bewaard.

### <a id="AvailabilityAndConsistency"></a>Beschikbaarheid van relatie met de consistentiecontrole, latentie en doorvoer
Beschikbaarheid van relatie met de consistentiecontrole, latentie en doorvoer wordt beschreven in [relatie met de beschikbaarheid van de consistentie](#ConsistencyAndAvailability), [relatie met de beschikbaarheid van de latentie](#LatencyAndAvailability) en [Relatie met de beschikbaarheid van de doorvoer](#ThroughputAndAvailability). 

## <a id="GuaranteesAgainstDataLoss"></a>Garanties en systeemgedrag voor 'gegevensverlies'
In Azure Cosmos DB elke partitie (van een verzameling) is maximaal beschikbaar is gemaakt door een aantal replica's worden verdeeld over ten minste 10-20 domeinen met fouten. Alle schrijfbewerkingen zijn synchroon en blijvend vastgelegd door een quorum meerderheid van replica's voordat ze naar de client zijn bevestigd. Asynchrone replicatie wordt toegepast met coördinatie meerdere partities verdeeld over meerdere regio's. Azure Cosmos DB zorgt ervoor dat er geen verlies van gegevens voor een tenant geïnitieerde handmatige failover is. Tijdens de automatische failover Azure Cosmos DB wordt gegarandeerd dat een bovengrens van het geconfigureerde gebonden veroudering interval in het venster voor het verlies van gegevens als onderdeel van de SERVICEOVEREENKOMST.

## <a id="CustomerFacingSLAMetrics"></a>Klantgerichte serviceovereenkomstmetrieken
Azure Cosmos DB beschrijft transparant de metrische gegevens doorvoer, latentie, consistentie en beschikbaarheid. Deze metrische gegevens zijn toegankelijk via een programma of via Azure portal (Zie de volgende afbeelding). U kunt ook waarschuwingen op verschillende drempelwaarden voor gebruik van Azure Application Insights instellen.
 
**Consistentie, latentie, doorvoer en beschikbaarheid metrische gegevens transparant beschikbaar zijn voor elke tenant**

![Azure DB Cosmos zichtbaar is voor een klant serviceovereenkomstmetrieken](./media/distribute-data-globally/customer-slas.png)

## <a id="Next Steps"></a>Volgende stappen
* Als u wilt implementeren globale replicatie van uw Azure DB die Cosmos-account met de Azure portal, Zie [het uitvoeren van Azure DB die Cosmos globale databasereplicatie met de Azure portal](tutorial-global-distribution-sql-api.md).
* Zie voor meer informatie over het implementeren van meerdere masters architecturen met Azure Cosmos DB, [hoofddatabase meerdere architecturen met Azure Cosmos DB](multi-region-writers.md).
* Zie voor meer informatie over de werking van automatische en handmatige failover werken in Azure Cosmos DB [regionale Failovers in Azure Cosmos DB](regional-failover.md).

## <a id="References"></a>Verwijzingen
1. Eric Brewer. [Voor robuuste gedistribueerde systemen](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric Brewer. [KAPJE twaalf jaar Later – hoe de regels zijn gewijzigd](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Geurts, Lynch. - [Brewer &#39; s gissingen en haalbaarheid van consistente, beschikbaar, partitie fouttolerante webservices](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Daniel Abadi. [Consistentiecontrole voor-en nadelen in moderne gedistribueerde Database systemen ontwerp](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. To Kleppmann. [Stop het aanroepen van databases CP of Azië](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis et al. [Probabilistische gebonden veroudering (PBS) voor praktische gedeeltelijke quorum](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor en wol. [Laden en de capaciteit en beschikbaarheid in een Quorum-systemen](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy en volgende. [Lineralizability: Een juistheid voorwaarde voor gelijktijdige objecten](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [SLA voor Azure Cosmos-DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
