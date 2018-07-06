---
title: Consistentieniveaus in Azure Cosmos DB | Microsoft Docs
description: Azure Cosmos DB biedt vijf consistentieniveaus te verdelen over uiteindelijke consistentie, beschikbaarheid en latentie-en nadelen.
keywords: uiteindelijke consistentie, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: sngun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 20edcd5e8e3ec3a9d3d294f7a81a2e97b4958f50
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857181"
---
# <a name="tunable-data-consistency-levels-in-azure-cosmos-db"></a>Gegevens instelbare consistentieniveaus in Azure Cosmos DB
Azure Cosmos DB is ontworpen vanaf het begin van met wereldwijde distributie in rekening voor elk gegevensmodel. Het is ontworpen om gegarandeerde voorspelbare lage latentie en meerdere duidelijk gedefinieerde beperkte consistentiemodellen te bieden. Op dit moment biedt Azure Cosmos DB vijf consistentieniveaus: sterk, gebonden veroudering, sessie, consistent voorvoegsel en mogelijk. Gebonden veroudering, sessie, consistent voorvoegsel en uiteindelijk worden aangeduid als "beperkte consistentiemodellen" Als deze minder consistentie dan sterk, die het meeste zeer consistent model beschikbaar is. 

Naast de **sterke** en **uiteindelijke consistentie** modellen meestal aangeboden door gedistribueerde databases, Azure Cosmos DB biedt drie meer zorgvuldig worden gecodeerd en ingezette consistentiemodellen:  **gebonden veroudering**, **sessie**, en **consistent voorvoegsel**. Het nut van elk van deze consistentieniveaus is gevalideerd tegen de echte wereld van use cases. Gezamenlijk kunnen deze vijf consistentieniveaus u goed gemotiveerd wisselwerking tussen consistentie, beschikbaarheid en latentie te maken. 

In de volgende video toont de Azure Cosmos DB Program Manager Andrew Liu de functies van kant en klare wereldwijde distributie.

>[!VIDEO https://www.youtube.com/embed/-4FsGysVD14]

## <a name="distributed-databases-and-consistency"></a>Gedistribueerde databases en consistentie
Commerciële gedistribueerde databases kunnen worden onderverdeeld in twee categorieën: databases die duidelijk gedefinieerde consistentiekeuzen voor waarschijnlijke consistentiekeuzes niet helemaal bieden en databases die zijn er twee mogelijkheden voor extreme programmeerbaarheid mogelijkheden (sterke versus mogelijke consistentie). 

In het eerste geval moeten ontwikkelaars van toepassingen gedetailleerde instellingen opgeven voor hun replicatieprotocollen en moeten ze lastige keuzes maken om een juiste balans te vinden tussen consistentie, beschikbaarheid, latentie en doorvoer. In het laatste geval moet een keuze worden gemaakt tussen twee extreme varianten. Ondanks de overvloed aan onderzoek en voorstellen voor meer dan 50 consistentiemodellen, is de community van gedistribueerde databases er niet in geslaagd om consistentieniveaus voorbij 'sterk' en 'mogelijk' commercieel geaccepteerd te krijgen. Cosmos DB biedt ontwikkelaars de mogelijkheid om te kiezen tussen vijf duidelijk gedefinieerde consistentiemodellen langs de consistentie spectrum: sterk, gebonden veroudering [sessie](http://dl.acm.org/citation.cfm?id=383631), consistent prefix en mogelijk. 

![Azure Cosmos DB biedt een keuze uit verschillende, duidelijk omschreven (minder nauwkeurige) consistentiemodellen](./media/consistency-levels/five-consistency-levels.png)

In de volgende tabel ziet u de specifieke garanties die horen bij elk consistentieniveau.
 
**Consistentieniveaus en bijbehorende garanties**

| Consistentieniveau | Garanties |
| --- | --- |
| Sterk | Verwerkingen. Retourneert de meest recente versie van een item bij leesbewerkingen gegarandeerd.|
| Gebonden veroudering | Consistent prefix. Leesbewerkingen volgen op schrijfbewerkingen met maximaal k prefixen of t interval |
| Sessie   | Consistent prefix. Monotone leesbewerkingen, monotone schrijfbewerkingen, read-your-writes, write-follows-reads |
| Consistent prefix | Geretourneerde updates zijn een prefix van alle updates, zonder hiaten |
| Mogelijk  | Leesbewerkingen vinden niet op volgorde plaats |

U kunt het standaardconsistentieniveau configureren in uw Cosmos DB-account (en de consistentie later voor een specifieke aanvraag overschrijven). Intern, het standaardconsistentieniveau toegepast op gegevens in de partitiesets, die mogelijk regio's omvatten. Liever gebonden veroudering ongeveer 73% van de Azure Cosmos DB-tenants gebruiken sessieconsistentie en 20%. Ongeveer 3% van de Azure Cosmos DB klanten experimenteren met verschillende consistentieniveaus in eerste instantie voordat op een specifieke consistentie keuze voor hun toepassing. Alleen 2% van de tenants van Azure Cosmos DB overschrijven consistentieniveaus op basis van per aanvraag. 

In Cosmos DB worden leesbewerkingen geleverd op sessie, consistent voorvoegsel en uiteindelijke consistentie worden twee keer zo goedkope als leesbewerkingen met sterke of gebonden veroudering consistentie. Cosmos DB heeft toonaangevende uitgebreide Sla's, met inbegrip van garanties voor consistentie, samen met beschikbaarheid, doorvoer en latentie. Maakt gebruik van Azure Cosmos DB een [verwerkingen checker](http://dl.acm.org/citation.cfm?id=1806634), die voortdurend werkt via de service-Telemetrie en openlijk rapporten eventuele schendingen consistentie voor u. Voor gebonden veroudering, Azure Cosmos DB bewaakt en eventuele schendingen voor grenzen k en t-rapporten. Voor alle vijf soepele consistentieniveaus, Azure Cosmos DB ook rapporten de [probabilistically gebonden veroudering metriek](http://dl.acm.org/citation.cfm?id=2212359) rechtstreeks aan u.  

## <a name="service-level-agreements"></a>Serviceovereenkomsten

Azure Cosmos DB biedt uitgebreide 99,99% [Sla's](https://azure.microsoft.com/support/legal/sla/cosmos-db/) welke garantie doorvoer, consistentie, beschikbaarheid en latentie voor Azure Cosmos DB-database accounts binnen het bereik van een enkel Azure-regio geconfigureerd met een van de vijf consistentie niveaus, of database-accounts die meerdere Azure-regio's, geconfigureerd met een van de vier soepele consistentie-niveaus. Bovendien, onafhankelijk van de keuze van een consistentieniveau, Azure Cosmos DB biedt een SLA van 99,999% voor de leesbeschikbaarheid voor database-accounts die twee of meer Azure-regio's.

## <a name="scope-of-consistency"></a>Bereik van consistentie
De granulatie van de consistentie is afgestemd op een aanvraag voor één gebruiker. Een schrijfaanvraag kan overeenkomen met een insert, replace, upsert of te verwijderen van de transactie. Net als bij schrijfbewerkingen, is ook een lezen/query-transactie afgestemd op een aanvraag voor één gebruiker. De gebruiker mogelijk worden gevraagd om u te pagineren via een grote resultatenset, die meerdere partities omvatten, maar elke transactie is binnen het bereik van één pagina en binnen een enkele partitie worden aangeleverd vanuit lezen.

## <a name="consistency-levels"></a>Consistentieniveaus
U kunt een standaardconsistentieniveau configureren op uw databaseaccount die van toepassing op alle containers (en -databases) voor uw Cosmos DB-account. Gebruik het standaardconsistentieniveau opgegeven voor het databaseaccount standaard alle leesbewerkingen en query's die voor de gebruiker gedefinieerde resources worden gegeven. U kunt het consistentieniveau van de van een specifieke lezen/query aanvraag met in elk van de ondersteunde API's versoepelen. Er zijn vijf typen consistentieniveaus ondersteund door het Azure Cosmos DB-replicatie-protocol waarmee een duidelijke balans tussen garanties voor specifieke consistentie en prestaties, zoals beschreven in deze sectie.

<a id="strong"></a>
**Sterke**: 

* Sterke consistentie biedt een [verwerkingen](https://aphyr.com/posts/313-strong-consistency-models) garanderen met de leesbewerkingen gegarandeerd de meest recente versie van een item. 
* Sterke consistentie gegarandeerd een schrijfbewerking is alleen zichtbaar nadat deze is door een meerderheidsquorum het quorum meerderheid van replica's. Een schrijfbewerking is ofwel synchroon door een meerderheidsquorum zowel de primaire en het quorum van secundaire databases, of deze wordt afgebroken. Een leesbewerking is altijd worden bevestigd door de meeste leesquorum, een client kan nooit zien voor het terugschrijven van een niet-doorgevoerde of gedeeltelijke en is altijd gegarandeerd de laatste bevestigde schrijfbewerking leest. 
* Azure Cosmos DB-accounts die zijn geconfigureerd voor het gebruik van sterke consistentie kunnen niet meer dan één Azure-regio koppelen aan hun Azure Cosmos DB-account.  
* De kosten van een leesbewerking (in termen van [aanvraageenheden](request-units.md) verbruikt) met sterke consistentie is hoger dan sessie en uiteindelijk, maar hetzelfde als gebonden veroudering.

<a id="bounded-staleness"></a>
**Gebonden veroudering**: 

* Gebonden veroudering consistentie gegarandeerd dat de leesbewerkingen op schrijfbewerkingen met maximaal volgen kunnen *K* versies of voorvoegsels van een item of *t* -tijdsinterval. 
* Daarom bij het kiezen van gebonden veroudering, de 'veroudering' kan worden geconfigureerd op twee manieren: aantal versies *K* van het item waarmee de leesbewerkingen volgen op de schrijf- en het tijdsinterval *t* 
* Gebonden veroudering aanbiedingen totale globale volgorde, behalve binnen de "veroudering venster." De monotone lezen garanties bestaan binnen een regio binnen en buiten de "veroudering venster." 
* Gebonden veroudering biedt een krachtiger consistentiegarantie dan sessie, consistent voorvoegsel of uiteindelijk consistentie. Voor wereldwijd gedistribueerde toepassingen, raden wij aan u gebonden veroudering voor scenario's waar u graag sterke consistentie hebben, maar ook wilt 99,99% beschikbaarheid en lage latentie.   
* Azure Cosmos DB-accounts die zijn geconfigureerd met consistentie voor gebonden veroudering kunnen u een willekeurig aantal Azure-regio's koppelen aan hun Azure Cosmos DB-account. 
* De kosten van een leesbewerking (voor wat betreft verbruikte ru's) met gebonden veroudering is hoger dan de sessie en uiteindelijke consistentie, maar hetzelfde als sterke consistentie.

<a id="session"></a>
**Sessie**: 

* Sessieconsistentie is in tegenstelling tot het globale consistentiemodellen die worden aangeboden door consistentieniveaus sterke en gebonden veroudering, afgestemd op een clientsessie. 
* Sessieconsistentie is ideaal voor alle scenario's waarbij de sessie van een apparaat of gebruiker betrokken is omdat deze gegarandeerd monotone leesbewerkingen, monotone schrijfbewerkingen en lezen (RYW) van uw eigen schrijfbewerkingen wordt gegarandeerd. 
* Sessieconsistentie voorspelbare consistentie biedt voor een sessie en maximale doorvoer lezen terwijl de laagste latentie van schrijfbewerkingen en leesbewerkingen. 
* Azure Cosmos DB-accounts die zijn geconfigureerd met sessieconsistentie kunnen u een willekeurig aantal Azure-regio's koppelen aan hun Azure Cosmos DB-account. 
* De kosten van een leesbewerking (voor wat betreft verbruikte ru's) met een sessie consistentieniveau is lager dan sterke en gebonden veroudering, maar meer dan uiteindelijke consistentie.

<a id="consistent-prefix"></a>
**Consistent Prefix**: 

* Consistent voorvoegsel zorgt ervoor dat in de afwezigheid van geen schrijfbewerkingen meer, de replica's binnen de groep uiteindelijk geconvergeerd. 
* Consistent voorvoegsel wordt gegarandeerd dat leesbewerkingen nooit niet-geordende schrijfbewerkingen te zien. Als schrijfbewerkingen zijn uitgevoerd in de volgorde `A, B, C`, en vervolgens een client een ziet `A`, `A,B`, of `A,B,C`, maar nooit niet-geordende zoals `A,C` of `B,A,C`.
* Azure Cosmos DB-accounts die zijn geconfigureerd met consistent voorvoegsel consistentie kunnen u een willekeurig aantal Azure-regio's koppelen aan hun Azure Cosmos DB-account. 

<a id="eventual"></a>
**Uiteindelijke**: 

* Uiteindelijke consistentie zorgt ervoor dat in de afwezigheid van geen schrijfbewerkingen meer, de replica's binnen de groep uiteindelijk geconvergeerd. 
* Uiteindelijke consistentie is de zwakste vorm van consistentie waarbij een client de waarden die ouder zijn dan de computer die deze had eerder kan verkrijgen.
* Uiteindelijke consistentie biedt de zwakste lezen van consistentie, maar biedt de laagste latentie voor zowel lees- en schrijfbewerkingen.
* Azure Cosmos DB-accounts die zijn geconfigureerd met de uiteindelijke consistentie kunnen u een willekeurig aantal Azure-regio's koppelen aan hun Azure Cosmos DB-account. 
* De kosten van een leesbewerking (voor wat betreft verbruikte ru's) met de uiteindelijke consistentie is het niveau van de laagste waarde van alle Azure Cosmos DB-consistentieniveaus.

## <a name="configuring-the-default-consistency-level"></a>Het standaardconsistentieniveau configureren
1. In de [Azure-portal](https://portal.azure.com/), klik in de Snelbalk op **Azure Cosmos DB**.
2. Selecteer op de pagina **Azure Cosmos DB** het databaseaccount dat u wilt wijzigen.
3. Klik in de accountpagina op **Standaardconsistentie**.
4. In de **Standaardconsistentie** pagina, selecteert u de nieuwe consistentieniveau en op **opslaan**.
   
    ![Schermafbeelding van het pictogram instellingen en de Standaardconsistentie vermelding markeren](./media/consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>Consistentieniveaus voor query 's
Voor de gebruiker gedefinieerde resources, is het consistentieniveau voor query's standaard hetzelfde als het consistentieniveau voor leesbewerkingen. De index wordt standaard synchroon bijgewerkt op elke invoegen, vervangen of verwijderen van een item naar de Cosmos DB-container. Hierdoor kan de query's op hetzelfde consistentieniveau als dat punt leesbewerkingen in acht neemt. Azure Cosmos DB is geoptimaliseerd voor schrijven en langdurige volumes van schrijfbewerkingen, synchrone indexonderhoud ten behoeve van consistente-query's ondersteunt, kunt u bepaalde containers voor het bijwerken van de index lazily kunt configureren. Vertraagde indexeren verder verhoogt de schrijfprestaties en is ideaal voor bulksgewijs opname-scenario's als een werkbelasting voornamelijk leesintensief is.  

| Modus indexeren | Leesbewerkingen | Query's |
| --- | --- | --- |
| Consistente (standaard) |Selecteer een sterk, gebonden veroudering, sessie, consistent voorvoegsel of uiteindelijk |Selecteer een sterk, gebonden veroudering, sessie, of uiteindelijke |
| Vertraagde |Selecteer een sterk, gebonden veroudering, sessie, consistent voorvoegsel of uiteindelijk |Mogelijk |
| Geen |Selecteer een sterk, gebonden veroudering, sessie, consistent voorvoegsel of uiteindelijk |Niet van toepassing |

Als met meer aanvragen, kunt u het consistentieniveau van de van de aanvraag van een specifieke query in elke API verlagen.

## <a name="consistency-levels-for-the-mongodb-api"></a>Consistentieniveaus voor de MongoDB-API

Azure Cosmos DB implementeert op dit moment MongoDB versie 3.4, met twee consistentie-instellingen, sterke en uiteindelijke. Omdat Azure Cosmos DB meerdere API’s heeft, zijn de consistentieniveaus van toepassing op het accountniveau, en het afdwingen van de consistentie wordt beheerd met elke API.  Tot MongoDB 3.6 was er nog geen sprake van sessieconsistentie. Als u een MongoDB-API-account instelt voor gebruik van sessieconsistentie, wordt bij het gebruik van MongoDB-API’s een downgrade uitgevoerd naar het consistentieniveau: mogelijk. Als u een read-your-own-write-garantie nodig hebt voor een MongoDB-API-account, moet het standaardconsistentieniveau voor het account worden ingesteld op gebonden of gebonden-verouderd.

## <a name="next-steps"></a>Volgende stappen
Als u dit doen meer lezen over de consistentieniveaus voor- en nadelen wilt, raden we de volgende bronnen:

* [Gerepliceerde gegevensconsistentie uitgelegd honkbal (video) door Doug Terry](https://www.youtube.com/watch?v=gluIh8zd26I)
* [Gerepliceerde gegevensconsistentie uitgelegd honkbal (technisch document) door Doug Terry](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
* [Sessie-garanties voor zwak consistente gerepliceerde gegevens](http://dl.acm.org/citation.cfm?id=383631)
* [Optimalisatie van de consistentie in het ontwerp van moderne systemen voor gedistribueerde Database: LIMIET is slechts een deel van het artikel](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Probabilistic gebonden veroudering (PBS) voor praktische gedeeltelijke quorum](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
* [Uiteindelijke consistente - herzien](http://allthingsdistributed.com/2008/12/eventually_consistent.html)
* [De Load, capaciteit en beschikbaarheid van de Quorum-systemen, SIAM logboek op Cloudcomputing](http://epubs.siam.org/doi/abs/10.1137/S0097539795281232)
* [De configuratie: een volledige en automatische verwerkingen checker, verslag van de 2010 ACM SIGPLAN Conferentie over het programmeren van taal ontwerpen en implementeren](http://dl.acm.org/citation.cfm?id=1806634)
* [Probabilistically gebonden veroudering voor praktische gedeeltelijke quorum](http://dl.acm.org/citation.cfm?id=2212359)
