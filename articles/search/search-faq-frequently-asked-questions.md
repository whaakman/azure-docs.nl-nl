---
title: Veelgestelde vragen (FAQ) over Azure Search | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over Microsoft Azure Search-Service
services: search
author: HeidiSteen
manager: jhubbard
ms.service: search
ms.technology: search
ms.topic: article
ms.date: 08/03/2017
ms.author: heidist
ms.openlocfilehash: f61fe2930bc70e800e5d79773e0de6827621e845
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="azure-search---frequently-asked-questions-faq"></a>Azure Search - Veelgestelde vragen (FAQ)

 Antwoorden op veelgestelde vragen over de concepten, code en scenario's die zijn gerelateerd aan Azure Search.

## <a name="platform"></a>Platform

### <a name="how-is-azure-search-different-from-full-text-search-in-my-dbms"></a>Hoe wordt Azure Search van zoeken in volledige tekst in mijn DBMS?

Azure Search biedt ondersteuning voor meerdere gegevensbronnen [taalkundige analyse voor vele talen](https://docs.microsoft.com/rest/api/searchservice/language-support), [aangepaste analyse voor interessante en ongebruikelijke gegevens invoer](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), positie besturingselementen via zoeken [score berekenen profielen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), en gebruikerservaring functies zoals typeahead treffers markeren en meervoudige navigatie. Dit omvat ook andere onderdelen, zoals synoniemen en uitgebreide querysyntaxis, maar deze functies zijn niet in het algemeen verschillen.

### <a name="what-is-the-difference-between-azure-search-and-elasticsearch"></a>Wat is het verschil tussen Azure Search en Elasticsearch?

Bij het vergelijken van de zoekopdracht technologieën vragen klanten vaak voor specifieke informatie over op hoe Azure Search worden vergeleken met Elasticsearch. Klanten die Azure Search via Elasticsearch voor hun zoekacties toepassing projecten meestal kiezen doen omdat we een belangrijke taak eenvoudiger aangebracht hebben of de ingebouwde integratie met andere Microsoft-technologieën moeten:

+ Azure Search is een volledig beheerde cloudservice met 99,9% serviceovereenkomsten (SLA) wanneer ingericht met voldoende redundantie (2 replica's voor leestoegang, 3 replica's voor lezen-schrijven).
+ Microsoft [natuurlijke taal processors](https://docs.microsoft.com/rest/api/searchservice/language-support) toonaangevende taalkundige analysis bieden.  
+ [Azure Search indexeerfuncties](search-indexer-overview.md) tal van Azure-gegevensbronnen voor initiële en incrementele indexeren kunt verkennen.
+ Als u snelle reactie op schommelingen in de query of volumes te indexeren nodig hebt, kunt u [schuifregelaars](search-manage.md#scale-up-or-down) in de Azure portal of voer een [PowerShell-script](search-manage-powershell.md), shard management rechtstreeks te omzeilen.  
+ [Score berekenen en afstemmen van functies](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) bieden de mogelijkheid om invloed zoeken positie scores afgezien van wat de zoekmachine alleen kunt opgeven.

### <a name="can-i-pause-azure-search-service-and-stop-billing"></a>Kan ik onderbreken Azure Search-service en facturering stoppen?

U kunt de service kan niet onderbreken. Rekenkracht en opslagbronnen worden toegewezen voor uw exclusief gebruik wanneer de service is gemaakt. Het is niet mogelijk vrij te geven en het vrijmaken van deze bronnen op aanvraag.

## <a name="indexing-operations"></a>Indexbewerkingen

### <a name="backup-and-restore-or-download-and-move-indexes-or-index-snapshots"></a>Back-up en herstellen (of downloaden en verplaatsen) indexen of index momentopnamen?

Weliswaar u kunt [ophalen van een indexdefinitie](https://docs.microsoft.com/rest/api/searchservice/get-index) op elk gewenst moment er is geen index extractie, momentopname of terugzetten van back-up-functie voor het downloaden van een *ingevuld* index uitgevoerd in de cloud naar een lokaal systeem of verplaatsen naar een andere Azure Search-service.

Indexen zijn gebouwd en ingevuld op basis van de code die u schrijft en alleen uitvoeren op Azure Search in de cloud. Normaal gesproken klanten die u wilt een index verplaatsen naar een andere service doen door de code voor het gebruik van een nieuw eindpunt te bewerken en voer het indexeren. Als u de mogelijkheid een momentopname of back-up van een index wilt, een stem geconverteerd op [User Voice](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index).

### <a name="can-i-index-from-sql-database-replicas-applies-to-azure-sql-database-indexershttpsdocsmicrosoftcomazuresearchsearch-howto-connecting-azure-sql-database-to-azure-search-using-indexers"></a>Kan ik index in SQL database-replica's (is van toepassing op [indexeerfuncties in Azure SQL Database](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers))

Er zijn geen beperkingen voor het gebruik van primaire of secundaire replica's als een gegevensbron bij het bouwen van een index maken. Vernieuwen van een index met incrementele updates (op basis van gewijzigde records) vereist echter de primaire replica. Deze vereiste zijn afkomstig van de SQL-Database, welke gegarandeerd op primaire replica's bijhouden. Als u met behulp van de secundaire replica's voor de werkbelasting van een index vernieuwen probeert, is er geen garantie dat u alle gegevens ophalen.

## <a name="search-operations"></a>Zoekopdrachten

### <a name="can-i-search-across-multiple-indexes"></a>Kan ik meerdere indexen doorzoeken?

Nee, deze bewerking wordt niet ondersteund. Zoeken is altijd afgestemd op een enkele index.

### <a name="can-i-restrict-search-corpus-access-by-user-identity"></a>Kan ik zoeken corpus toegang beperken door de gebruikersidentiteit van de?

U kunt implementeren [beveiligingsfilters](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search) met `search.in()` filter. Het filter stelt het bericht goed met [identity management-services zoals Azure Active Directory(AAD)](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search-with-aad) trim zoekresultaten op basis van groepslidmaatschap van de gebruiker gedefinieerd.

### <a name="why-are-there-zero-matches-on-terms-i-know-to-be-valid"></a>Waarom zijn er nul overeenkomt met op voorwaarden die ik weet geldig?

De meeste gevallen is niet te weten dat elk querytype verschillende zoekgedrag en niveaus van de taalkundige analyses ondersteunt. Zoekopdracht in volledige tekst, die de belangrijkste werkbelasting, bevat de analysefase van een taal die wordt verbroken voorwaarden omlaag naar de hoofdmap formulieren. Dit aspect van het parseren van de query cast breder net over mogelijke overeenkomsten, omdat de tokens term overeenkomt met een groter aantal varianten.

Jokertekens fuzzy regex-query's, zoals gewone woorden of woordgroepen query's worden niet geanalyseerd en kunnen leiden tot slechte intrekken als de query komt niet overeen met de geanalyseerde vorm van het woord in de search-index. Zie voor meer informatie over het parseren van de query en -analyse, [architectuur query](https://docs.microsoft.com/azure/search/search-lucene-query-architecture).

### <a name="my-wildcard-searches-are-slow"></a>Zoekopdrachten met jokertekens zijn traag.

De meeste jokertekens zoekquery's, zoals voorvoegsel, fuzzy en regex, herschreven intern met die overeenkomt met de voorwaarden in de search-index. Deze extra verwerking van het scannen van de search-index wordt toegevoegd aan de latentie. Verdere, brede zoeken query's, zoals `a*` die zijn bijvoorbeeld waarschijnlijk worden herschreven met veel voorwaarden kan zeer traag zijn. Voor zoekopdrachten met jokertekens zodat, kunt u definiëren van een [aangepaste analyzer](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search).

### <a name="why-is-the-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Waarom is de positie zoeken van een constante of gelijk zijn score van 1.0 voor elke treffer?

Standaard zoekresultaten worden berekend op basis van de [statistische eigenschappen van die overeenkomt met de voorwaarden](search-lucene-query-architecture.md#stage-4-scoring), en een geordende hoog tot laag in het resultaat van de verzameling. Evenwel sommige querytypen (jokerteken, voorvoegsel, regex) bijdragen altijd een constante score voor de algehele document score. Dit gedrag is standaard. Azure Search gelden een constante score zodat overeenkomsten gevonden via de uitbreiding van de query moet worden opgenomen in de resultaten zonder de volgorde.

Stel bijvoorbeeld dat een invoer van het 'rondleiding *' in een zoeken met jokertekens produceert komt overeen met 'rondleidingen', 'tourettes' en 'tourmaline'. Gezien de aard van deze resultaten, kan er niet worden redelijkerwijs afleiden welke voorwaarden vindt u meer dan andere waardevolle. Om deze reden negeren we term frequenties wanneer resultaten scoren van typen jokerteken, voorvoegsel en regex-query's. Zoekresultaten op basis van een gedeeltelijke invoer krijgen een constante score om te voorkomen dat afwijking naar mogelijk onverwachte resultaten.

## <a name="design-patterns"></a>Ontwerppatronen

### <a name="what-is-the-best-approach-for-implementing-localized-search"></a>Wat is de aanbevolen aanpak voor het implementeren van gelokaliseerde zoeken?

De meeste klanten Kies specifieke velden over een verzameling bij het ondersteunen van verschillende talen (talen) in dezelfde index. Landspecifieke velden maken het mogelijk om een juiste analyzer toewijzen. Bijvoorbeeld: Microsoft Frans Analyzer toewijzen aan een veld met Franse tekenreeksen. Het vereenvoudigt ook filteren. Als u dat een query wordt gestart op een pagina fr-fr, kunt u zoekresultaten in dit veld kan beperken. Of maak een [score berekenen profiel](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) geven het veld meer relatieve gewicht. Azure Search via ondersteunt [50 taalanalyse](https://docs.microsoft.com/azure/search/search-language-support) kiezen.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? Aanvragen van de functie op de [User Voice-website](https://feedback.azure.com/forums/263029-azure-search).

## <a name="see-also"></a>Zie ook

 [StackOverflow: Azure Search](https://stackoverflow.com/questions/tagged/azure-search)   
 [Hoe vol tekst zoeken werkt in Azure Search](search-lucene-query-architecture.md)  
 [Wat is Azure Search?](search-what-is-azure-search.md)
