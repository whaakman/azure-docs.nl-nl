---
title: Consistentieniveaus in Azure Cosmos DB | Microsoft Docs
description: Azure Cosmos DB heeft vijf consistentieniveaus te verdelen uiteindelijke consistentie, beschikbaarheid en latentie-en nadelen.
keywords: uiteindelijke consistentie azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: mimig1
manager: jhubbard
editor: cgronlun
documentationcenter: 
ms.assetid: 3fe51cfa-a889-4a4a-b320-16bf871fe74c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 303a36fc966cd92399de92b4d52f75c114b75781
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="tunable-data-consistency-levels-in-azure-cosmos-db"></a>Gegevens instelbare consistentieniveaus in Azure Cosmos-DB
Azure Cosmos DB is compleet met globale verdeling in gedachten voor elke gegevensmodel ontworpen. Het is ontworpen om voorspelbare lage latentie garanties en meerdere goed gedefinieerde beperkte consistentie modellen bieden. Op dit moment Azure Cosmos DB bevat vijf consistentieniveaus: sterk, gebonden-verouderd, sessie, consistente voorvoegsel en uiteindelijk. Gebonden-verouderd, sessie, consistente voorvoegsel en uiteindelijke worden aangeduid als 'beperkte consistentie modellen' als ze bieden minder consistentie dan sterk, die de meeste maximaal consistent model beschikbaar is. 

Naast de **sterk** en **uiteindelijke consistentie** modellen vaak die worden aangeboden door gedistribueerde databases Azure Cosmos DB biedt drie meer zorgvuldig gecodeerd en geoperationaliseerd consistentie modellen:  **gebonden veroudering**, **sessie**, en **consistent voorvoegsel**. Het nut van elk van deze consistentieniveaus is gevalideerd tegen werkelijkheid gebruiksvoorbeelden. Gezamenlijk kunnen deze vijf consistentieniveaus u afweging goed gemotiveerd tussen de consistentie, beschikbaarheid en latentie. 

## <a name="distributed-databases-and-consistency"></a>Gedistribueerde databases en consistentie
Commerciële gedistribueerde databases worden onderverdeeld in twee categorieën: databases die niet beschikken over goed gedefinieerde provable consistentie keuzes helemaal en databases die zijn er twee extreme programmeerbaarheid mogelijkheden (sterk versus uiteindelijke consistentie). 

In het eerste geval moeten ontwikkelaars van toepassingen gedetailleerde instellingen opgeven voor hun replicatieprotocollen en moeten ze lastige keuzes maken om een juiste balans te vinden tussen consistentie, beschikbaarheid, latentie en doorvoer. In het laatste geval moet een keuze worden gemaakt tussen twee extreme varianten. Ondanks de overvloed aan onderzoek en voorstellen voor meer dan 50 consistentiemodellen, is de community van gedistribueerde databases er niet in geslaagd om consistentieniveaus voorbij 'sterk' en 'mogelijk' commercieel geaccepteerd te krijgen. Cosmos DB kan ontwikkelaars kiezen tussen vijf goed gedefinieerde consistentie modellen langs de consistentie spectrum – sterk, gebonden veroudering [sessie](http://dl.acm.org/citation.cfm?id=383631), consistente voorvoegsel en uiteindelijk. 

![Azure Cosmos DB biedt een keuze uit verschillende, duidelijk omschreven (minder nauwkeurige) consistentiemodellen](./media/consistency-levels/five-consistency-levels.png)

In de volgende tabel ziet u de specifieke garanties die horen bij elk consistentieniveau.
 
**Consistentieniveaus en bijbehorende garanties**

| Consistentieniveau | Garanties |
| --- | --- |
| Sterk | Linearizability. Retourneren van de meest recente versie van een item worden leesbewerkingen gegarandeerd.|
| Gebonden veroudering | Consistent prefix. Leesbewerkingen volgen op schrijfbewerkingen met k prefixen of t interval |
| Sessie   | Consistent prefix. Monotone leesbewerkingen, monotone schrijfbewerkingen, read-your-writes, write-follows-reads |
| Consistent prefix | Geretourneerde updates zijn een prefix van alle updates, zonder hiaten |
| Mogelijk  | Leesbewerkingen vinden niet op volgorde plaats |

U kunt het standaardconsistentieniveau configureren in uw Cosmos DB-account (en de consistentie later voor een specifieke aanvraag overschrijven). Intern, geldt het standaardniveau voor consistentie voor gegevens in de partitiesets, die mogelijk span regio's. Voorkeur gebonden veroudering ongeveer 73% van de Azure DB die Cosmos tenants gebruik sessieconsistentie en 20%. Azure Cosmos DB klanten van ongeveer 3% experimenteren met verschillende consistentieniveaus in eerste instantie voordat op een specifieke consistentie keuze voor de toepassing. Alleen 2% van tenants van Azure DB die Cosmos overschrijven consistentieniveaus op basis van per aanvraag. 

Leesbewerkingen geleverd op consistente voorvoegsel-sessie en uiteindelijke consistentie zijn twee keer als goedkope als leesbewerkingen met sterke of gebonden veroudering consistentie in Cosmos-database. Cosmos DB heeft toonaangevende uitgebreide Sla's, met inbegrip van de consistentie wordt gegarandeerd samen met de beschikbaarheid, doorvoer en latentie. De veiligheidsmaatregelen voor Azure Cosmos DB een [linearizability checker](http://dl.acm.org/citation.cfm?id=1806634), dat continu werkt via de service Telemetrie en openbaar rapporteert eventuele schendingen consistentie voor u. Voor gebonden veroudering Azure Cosmos DB bewaakt en eventuele schendingen op k en t grenzen rapporteert. Voor alle vijf beperkte consistentieniveaus, Azure Cosmos DB ook rapporten de [probabilistically gebonden veroudering metriek](http://dl.acm.org/citation.cfm?id=2212359) voor u.  

## <a name="service-level-agreements"></a>Serviceovereenkomsten

Azure Cosmos DB biedt uitgebreide 99,99% [serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/cosmos-db/) welke garantie doorvoer, consistentie, beschikbaarheid en latentie voor Azure Cosmos DB accounts die zijn geconfigureerd met een van de vijf consistentie één Azure-regio binnen het bereik van de database niveaus of database-accounts die meerdere Azure-regio's, geconfigureerd met een van de vier beperkte consistentieniveaus-spanning. Bovendien onafhankelijk van de keuze van een consistentiecontrole-niveau, Azure Cosmos DB biedt een SLA 99,999% voor lezen beschikbaarheid voor database-accounts spanning van twee of meer Azure-regio's.

## <a name="scope-of-consistency"></a>Bereik van de consistentie
De granulatie van de consistentie is afgestemd op een aanvraag voor één gebruiker. Een schrijfaanvraag mogelijk komen overeen met een insert, replace, upsert of te verwijderen van de transactie. Net als bij schrijfbewerkingen en is ook een lezen/query transactie afgestemd op een aanvraag voor één gebruiker. De gebruiker mogelijk moet pagineren via een grote resultatenset, spanning meerdere partities, maar elke transactie is afgestemd op één pagina en aangeboden via binnen één partitie gelezen.

## <a name="consistency-levels"></a>Consistentieniveaus
U kunt een standaardniveau voor consistentiecontrole configureren op uw databaseaccount die voor alle verzamelingen (en databases geldt) onder uw account Cosmos DB. Standaard gebruiken alle leesbewerkingen en query's die zijn uitgegeven voor de gebruiker gedefinieerde resources het standaardniveau van de consistentie opgegeven op het account van de database. U kunt het consistentieniveau van een specifieke lezen/query aanvraag met in elk van de ondersteunde API's versoepelen. Er zijn vijf typen ondersteund door het protocol van de replicatie Azure Cosmos DB consistentieniveaus die een duidelijke compromis tussen specifieke consistentie wordt gegarandeerd en prestaties bieden, zoals beschreven in deze sectie.

**Sterke**: 

* Sterke consistentie biedt een [linearizability](https://aphyr.com/posts/313-strong-consistency-models) garanderen met gelezen gegevens gegarandeerd dat de meest recente versie van een item geretourneerd. 
* Sterke consistentie wordt gegarandeerd dat een schrijfbewerking is alleen zichtbaar nadat deze definitief is vastgelegd door het quorum meerderheid van replica's. Een schrijfbewerking ofwel synchroon definitief is vastgelegd door zowel de primaire als het quorum van secundaire replica's of deze is afgebroken. Lees altijd wordt bevestigd door de meerderheid quorum lezen, een client nooit het terugschrijven van een niet-doorgevoerde of gedeeltelijke kan zien en altijd de meest recente bevestigde schrijven lezen kan worden gegarandeerd. 
* Azure DB Cosmos-accounts die zijn geconfigureerd voor gebruik van sterke consistentie kunnen niet meer dan één Azure-regio koppelen aan hun Azure DB die Cosmos-account.  
* De kosten van een leesbewerking (in termen van [aanvraageenheden](request-units.md) verbruikt) met sterke consistentie is hoger dan de sessie en mogelijk, maar dezelfde zijn als gebonden veroudering.

**Gebonden veroudering**: 

* Gebonden veroudering consistentie wordt gegarandeerd dat de leesbewerkingen schrijfbewerkingen door maximaal achterblijven *K* versies of voorvoegsels van een item of *t* tijdsinterval blijft. 
* Daarom bij het kiezen van gebonden veroudering, de 'veroudering' kan worden geconfigureerd op twee manieren: aantal versies *K* van het item waarmee de leesbewerkingen lag achter de geschreven en het tijdsinterval *t* 
* Gebonden veroudering aanbiedingen totale algemene volgorde, behalve binnen het 'veroudering venster'. De monotone lezen garanties bestaan in een gebied binnen en buiten het 'veroudering venster." 
* Gebonden veroudering biedt een betere consistentie garantie dan sessie, consistente-voorvoegsel of uiteindelijke consistentie. Globaal gedistribueerde toepassingen, wordt u aangeraden dat u gebonden veroudering gebruiken voor scenario's waarin u sterke consistentie hebben, maar ook wilt 99,99% beschikbaarheid en lage latentie wilt.   
* Azure DB Cosmos-accounts die zijn geconfigureerd met consistentie voor gebonden veroudering kunnen een onbeperkt aantal Azure-regio's koppelen aan hun Azure DB die Cosmos-account. 
* De kosten van een leesbewerking (in termen van RUs verbruikt) met gebonden veroudering is hoger dan de sessie en uiteindelijke consistentie echter hetzelfde als sterke consistentie.

**Sessie**: 

* In tegenstelling tot de globale consistentie modellen die worden aangeboden door sterke en gebonden veroudering consistentieniveaus, heeft een sessieconsistentie bereik op een clientsessie. 
* Sessieconsistentie is ideaal voor alle scenario's waarbij een apparaat of gebruiker sessie omdat garant monotone leest, monotone schrijfbewerkingen en lezen wordt gegarandeerd dat uw eigen schrijfbewerkingen (RYW) is betrokken. 
* Sessieconsistentie voorziet in voorspelbare consistentie voor een sessie en maximale doorvoer en biedt tegelijkertijd de laagste latentie voor schrijfbewerkingen en leesbewerkingen gelezen. 
* Azure DB Cosmos-accounts die zijn geconfigureerd met de sessieconsistentie kunnen een onbeperkt aantal Azure-regio's koppelen aan hun Azure DB die Cosmos-account. 
* De kosten van een leesbewerking (in termen van RUs verbruikt) met sessie consistentieniveau is kleiner dan sterk en gebonden veroudering, maar meer dan uiteindelijke consistentie.

<a id="consistent-prefix"></a>
**Consistente voorvoegsel**: 

* Consistente voorvoegsel wordt gegarandeerd dat zolang er verder geen schrijfbewerkingen, de replica's binnen de groep uiteindelijk geconvergeerd. 
* Consistente voorvoegsel wordt gegarandeerd dat leesbewerkingen nooit volgorde schrijfbewerkingen te zien. Als schrijfbewerkingen zijn uitgevoerd in de volgorde `A, B, C`, en vervolgens een client een ziet `A`, `A,B`, of `A,B,C`, maar nooit volgorde zoals `A,C` of `B,A,C`.
* Azure DB Cosmos-accounts die zijn geconfigureerd met consistente voorvoegsel consistentiecontrole kunnen een onbeperkt aantal Azure-regio's koppelen aan hun account Azure Cosmos DB. 

**Uiteindelijke**: 

* Uiteindelijke consistentie wordt gegarandeerd dat zolang er verder geen schrijfbewerkingen, de replica's binnen de groep uiteindelijk geconvergeerd. 
* Uiteindelijke consistentie is de zwakste vorm van de consistentie waar de waarden die ouder dan degene die deze had die zijn eerder op een client kan verkrijgen.
* Uiteindelijke consistentie voorziet in de minste leesconsistentie, maar biedt de laagste latentie voor zowel lees- en schrijfbewerkingen.
* Azure DB Cosmos-accounts die zijn geconfigureerd met uiteindelijke consistentie kunnen een onbeperkt aantal Azure-regio's koppelen aan hun Azure DB die Cosmos-account. 
* De kosten van een leesbewerking (in termen van RUs verbruikt) met de uiteindelijke consistentie niveau is de laagste van alle Azure Cosmos DB consistentieniveaus.

## <a name="configuring-the-default-consistency-level"></a>Het standaardniveau voor consistentiecontrole configureren
1. In de [Azure-portal](https://portal.azure.com/), klik in de Snelbalk op **Azure Cosmos DB**.
2. In de **Azure Cosmos DB** pagina, selecteert u het account van de database te wijzigen.
3. Klik op de accountpagina **consistentie standaard**.
4. In de **standaard consistentie** pagina, selecteer het nieuwe consistentieniveau en klikt u op **opslaan**.
   
    ![Schermopname het pictogram instellingen en consistentie standaard vermelding markeren](./media/consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>Consistentieniveaus voor query 's
Voor de gebruiker gedefinieerde resources, is het consistentieniveau van de voor query's standaard hetzelfde zijn als het consistentieniveau van de voor leesbewerkingen. De index wordt standaard synchroon bijgewerkt op elke invoegen, vervangen of verwijderen van een item aan de container Cosmos DB. Hierdoor kan de query's te houden van hetzelfde consistentieniveau als die van punt leesbewerkingen. Terwijl Azure Cosmos DB write is geoptimaliseerd en volgehouden volumes van schrijfbewerkingen, synchrone index onderhoud ten behoeve van consistente query's ondersteunt, kunt u bepaalde verzamelingen voor het bijwerken van de index traag kunt configureren. Vertraagde indexeren verder de prestaties met schrijven en is ideaal voor bulksgewijs opname scenario's wanneer een werkbelasting voornamelijk lezen zware.  

| Indexeren van modus | Leesbewerkingen | Query's |
| --- | --- | --- |
| Consistente (standaard) |Selecteer een van de sterk, gebonden veroudering, sessie, consistente voorvoegsel of uiteindelijke |Selecteer een van de sterk, gebonden-verouderd, sessie of uiteindelijke |
| Vertraagde |Selecteer een van de sterk, gebonden veroudering, sessie, consistente voorvoegsel of uiteindelijke |Mogelijk |
| Geen |Selecteer een van de sterk, gebonden veroudering, sessie, consistente voorvoegsel of uiteindelijke |Niet van toepassing |

Als met leesaanvragen, kunt u het consistentieniveau van de aanvraag van een specifieke query in elke API verlagen.

## <a name="next-steps"></a>Volgende stappen
Als u dit doen meer lezen over consistentieniveaus en -en nadelen wilt, raden we de volgende bronnen:

* Doug Terry. Consistentie van de gerepliceerde gegevens toegelicht door middel van baseball (video).   
  [https://www.YouTube.com/Watch?v=gluIh8zd26I](https://www.youtube.com/watch?v=gluIh8zd26I)
* Doug Terry. Consistentie van de gerepliceerde gegevens worden via baseball beschreven.   
  [http://Research.Microsoft.com/pubs/157411/ConsistencyAndBaseballReport.PDF](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
* Doug Terry. Sessie garanties met betrekking tot zwak consistente gerepliceerde gegevens.   
  [http://DL.ACM.org/Citation.cfm?id=383631](http://dl.acm.org/citation.cfm?id=383631)
* Daniel Abadi. Consistentiecontrole voor-en nadelen in moderne gedistribueerde Database systemen ontwerp: KAPJE wordt slechts een deel van het artikel '.   
  [http://computer.org/CSDL/mags/CO/2012/02/mco2012020037-ABS.HTML](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
* Peter Bailis, Shivaram Venkataraman, Michael J. Franklin, Joseph Dhr Hellerstein, bewaartermijn Stoica. Probabilistische begrensd veroudering (PBS) voor praktische gedeeltelijke quorum.   
  [http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.PDF](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
* Werner Vogels. Uiteindelijke Consistent - herzien.    
  [http://allthingsdistributed.com/2008/12/eventually_consistent.HTML](http://allthingsdistributed.com/2008/12/eventually_consistent.html)
* Monitoring Naor, Avishai wol, de belasting, capaciteit en beschikbaarheid van systemen Quorum, SIAM journaal op Computing, v.27 n.2, p.423-447, April 1998.
  [http://epubs.siam.org/DOI/ABS/10.1137/S0097539795281232](http://epubs.siam.org/doi/abs/10.1137/S0097539795281232)
* Sebastian Burckhardt, Chris Dern, Macanal Musuvathi, Roy Tan, de configuratie: een volledige en automatische linearizability checker, verslag van de 2010 ACM SIGPLAN Conferentie over het programmeren van taal ontwerpen en implementeren, juni 05 10 2010 Toronto, Ontario, Canada [doi > 10.1145/1806596.1806634] [http://dl.acm.org/citation.cfm?id=1806634](http://dl.acm.org/citation.cfm?id=1806634)
* Peter Bailis, Shivaram Venkataraman, Michael J. Franklin, Joseph M. Hellerstein, bewaartermijn Stoica, Probabilistically gebonden veroudering voor praktische gedeeltelijke quorum procedures van de eigen VLDB, v.5 n.8, April 2012 met p.776 787 [http:// DL.ACM.org/Citation.cfm?id=2212359](http://dl.acm.org/citation.cfm?id=2212359)
