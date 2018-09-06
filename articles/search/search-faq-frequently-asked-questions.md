---
title: Veelgestelde vragen (FAQ) over Azure Search | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over Microsoft Azure Search-Service
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 08/03/2017
ms.author: heidist
ms.openlocfilehash: 1491fdb0f208100619e569f9a74d5e697a0065a6
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43841674"
---
# <a name="azure-search---frequently-asked-questions-faq"></a>Azure Search - Veelgestelde vragen (FAQ)

 Vind antwoorden op veelgestelde vragen over concepten, code en scenario's met betrekking tot Azure Search.

## <a name="platform"></a>Platform

### <a name="how-is-azure-search-different-from-full-text-search-in-my-dbms"></a>Wat is Azure Search verschil met zoeken in volledige tekst in mijn DBMS-systemen?

Azure Search biedt ondersteuning voor meerdere gegevensbronnen, [taalkundige analyse voor vele talen](https://docs.microsoft.com/rest/api/searchservice/language-support), [aangepaste analyse voor gegevensinvoer interessante en ongebruikelijke](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), positie zoekbesturingselementen via [scoren profielen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), en gebruikerservaring functies zoals typeahead, markeren en meervoudige navigatie. Bevat ook andere functies, zoals synoniemen en uitgebreide query-syntaxis, maar deze functies zijn in het algemeen niet differentiëren.

### <a name="what-is-the-difference-between-azure-search-and-elasticsearch"></a>Wat is het verschil tussen Azure Search en Elasticsearch?

Bij het vergelijken van zoektechnologieën, vragen klanten vaak voor meer informatie over hoe Azure Search met Elasticsearch vergelijkt. Klanten die Azure Search via Elasticsearch voor hun zoeken projecten van de toepassing doorgaans doen omdat we een belangrijke taak gemakkelijker gemaakt hebben of ze nodig hebben de ingebouwde integratie met andere Microsoft-technologieën:

+ Azure Search is een volledig beheerde cloudservice met 99,9% service level agreements (SLA) ingericht met voldoende redundantie (2 replica's voor lezen, 3 replica's voor lezen / schrijven).
+ Microsofts [natuurlijke taal processors](https://docs.microsoft.com/rest/api/searchservice/language-support) bieden geavanceerde linguïstische analyse.  
+ [Azure Search-indexeerfuncties](search-indexer-overview.md) diverse Azure-gegevensbronnen voor initiële en incrementele indexeren kan worden verkend.
+ Als u snel kun reageren op fluctuaties in de query of het indexeren van volumes, kunt u [schuifregelaars](search-manage.md#scale-up-or-down) in Azure portal of voer een [PowerShell-script](search-manage-powershell.md), shard management rechtstreeks overslaan.  
+ [Scoring- en afstemmingsfuncties](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) bieden de mogelijkheid om invloed zoeken positie scores dan wat alleen de zoekmachine kan bieden.

### <a name="can-i-pause-azure-search-service-and-stop-billing"></a>Kan ik Azure Search-service onderbreken en stoppen facturering?

U kunt de service kan niet onderbreken. Rekenkundige- en opslagresources worden toegewezen voor exclusief gebruik als de service is gemaakt. Het is niet mogelijk is vrij te geven en het vrijmaken van deze resources op aanvraag.

## <a name="indexing-operations"></a>Indexeren van bewerkingen

### <a name="backup-and-restore-or-download-and-move-indexes-or-index-snapshots"></a>Back-up en herstellen (of downloaden en verplaatsen) indexen of index momentopnamen?

Weliswaar u kunt [ophalen van een indexdefinitie](https://docs.microsoft.com/rest/api/searchservice/get-index) op elk gewenst moment is er geen index extractie, snapshot, of back-up herstellen-functie voor het downloaden van een *ingevuld* index uitvoeren in de cloud naar een lokaal systeem, of verplaatsen naar een andere Azure Search-service.

Indexen worden gemaakt en ingevuld op basis van code die u schrijft, en alleen op Azure Search in de cloud worden uitgevoerd. Normaal gesproken klanten die willen verplaatsen van een index op een andere service doen door de code voor het gebruik van een nieuw eindpunt te bewerken en start opnieuw indexeren. Als u wilt dat de mogelijkheid een momentopname of back-up van een index, cast-conversie uitvoeren een stem op [User Voice](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index).

### <a name="can-i-restore-my-index-or-service-once-it-is-deleted"></a>Kan herstel ik mijn index of de service nadat dit is verwijderd?

Nee, u niet terugzetten indexen of services. Als u een Azure Search-index verwijdert, wordt de bewerking is voltooid en de index kan niet worden hersteld. Wanneer u een Azure Search-service verwijdert, worden alle indexen in de service definitief verwijderd. Ook als u een Azure-resourcegroep met een of meer Azure Search-services verwijdert, worden alle services definitief verwijderd.  

Herstellen van resources, zoals indexen, indexeerfuncties, gegevensbronnen en kennis en vaardigheden vereist dat u ze opnieuw van code maken. In het geval van indexen, moet u gegevens uit externe bronnen indexeren. Om deze reden, is het raadzaam dat u een origineel exemplaar of een back-up van de oorspronkelijke gegevens in een ander gegevensarchief, zoals Azure SQL Database of Cosmos DB behoudt.

### <a name="can-i-index-from-sql-database-replicas-applies-to-azure-sql-database-indexershttpsdocsmicrosoftcomazuresearchsearch-howto-connecting-azure-sql-database-to-azure-search-using-indexers"></a>Kan ik indexeren van SQL database-replica's (van toepassing op [indexeerfuncties van Azure SQL Database](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers))

Er zijn geen beperkingen voor het gebruik van de primaire of secundaire replica's als een gegevensbron bij het bouwen van een index maken. Vernieuwen van een index met incrementele updates (op basis van gewijzigde records) vereist echter de primaire replica. Deze vereiste is afkomstig van SQL-Database, welke garanties voor bijhouden op de primaire replica's wijzigingen. Als u met behulp van de secundaire replica's voor de werkbelasting van een index vernieuwen probeert, is er geen garantie dat u over alle van de gegevens.

## <a name="search-operations"></a>Zoekopdrachten

### <a name="can-i-search-across-multiple-indexes"></a>Kan ik meerdere indexen Doorzoek?

Nee, deze bewerking wordt niet ondersteund. Zoeken wordt altijd toegewezen aan één index.

### <a name="can-i-restrict-search-corpus-access-by-user-identity"></a>Kan ik zoeken corpus toegang beperken door gebruikers-id?

U kunt implementeren [beveiligingsfilters](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search) met `search.in()` filter. Het filter stelt goed met het bericht [identity management-services zoals Azure Active Directory(AAD)](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search-with-aad) gedefinieerd waaruit de zoekresultaten op basis van lidmaatschap van gebruikersgroepen.

### <a name="why-are-there-zero-matches-on-terms-i-know-to-be-valid"></a>Waarom zijn er nul komt overeen met op voorwaarden die ik weet om geldig te zijn?

De meeste gevallen is niet weten dat elk querytype verschillende zoekgedrag en niveaus van de linguïstische analyse ondersteunt. Zoeken in volledige tekst, die de overheersende workload, bevat de analysefase van een taal waarin voorwaarden omlaag naar de hoofdmap formulieren. Omdat de tokens term overeenkomt met een groter aantal varianten wordt dit aspect van het parseren van de query een bredere net geworpen op mogelijke overeenkomsten.

Jokertekens, fuzzy regex-query's, zoals reguliere zoekterm of woordgroep query's worden niet geanalyseerd en kunnen leiden tot slechte intrekken als de query komt niet overeen met de geanalyseerde vorm van het woord in de search-index. Zie voor meer informatie over het parseren van de query en -analyse, [query architectuur](https://docs.microsoft.com/azure/search/search-lucene-query-architecture).

### <a name="my-wildcard-searches-are-slow"></a>Mijn zoeken met jokertekens zijn traag.

De meeste jokertekens zoekquery's, zoals voorvoegsel, fuzzy en reguliere expressie, worden alleen intern herschreven met de bijbehorende voorwaarden in de search-index. Deze extra verwerking van het scannen van de search-index wordt toegevoegd aan de latentie. Bovendien kunnen de uitgebreide zoekactie query's, zoals `a*` die zijn bijvoorbeeld waarschijnlijk worden herschreven met veel voorwaarden kan zeer traag zijn. Voor goed presterende zoeken met jokertekens, kunt u overwegen definiëren van een [aangepaste analysefunctie](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search).

### <a name="why-is-the-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Waarom is een constante of gelijk zijn score van 1.0 voor elke drukt u op de positie in zoekresultaten?

Standaard zoekresultaten worden beoordeeld op basis van de [statistische eigenschappen van die overeenkomt met de voorwaarden](search-lucene-query-architecture.md#stage-4-scoring), en een geordende hoog naar laag in het resultaat van de verzameling. Echter enkele querytypen (jokerteken, voorvoegsel, reguliere expressie) hun bijdragen altijd een constante score is de totale score van document. Dit gedrag is standaard. Azure Search legt een constante score om toe te staan van de gevonden via de uitbreiding van de query moet worden opgenomen in de resultaten, zonder gevolgen voor de volgorde.

Stel bijvoorbeeld dat invoer van 'rondleiding *' in een zoeken met jokertekens produceert overeenkomsten op "rondleidingen", "tourettes" en 'tourmaline'. Gezien de aard van deze resultaten, wordt er geen manier om te redelijkerwijs afleiden welke voorwaarden vindt u meer dan andere waardevolle is. Om deze reden negeren we term frequenties wanneer resultaten scoring in query's van typen jokerteken, voorvoegsel en reguliere expressie. Zoekresultaten op basis van een gedeeltelijke invoer een constante score om te voorkomen dat de afwijking op mogelijk onverwachte resultaten krijgt.

## <a name="design-patterns"></a>Ontwerppatronen

### <a name="what-is-the-best-approach-for-implementing-localized-search"></a>Wat is de aanbevolen aanpak voor het implementeren van gelokaliseerde zoeken?

De meeste klanten kiezen speciale velden over een verzameling als het gaat om het ondersteunen van verschillende talen (talen) in dezelfde index. Landspecifieke velden maken het mogelijk om toe te wijzen een juiste analyzer. Bijvoorbeeld, toewijzen de Microsoft Frans Analyzer aan een veld met Franse tekenreeksen. Het vereenvoudigt ook filteren. Als u dat een query wordt gestart op een pagina fr-fr, kunt u zoekresultaten in dit veld kan beperken. Of maak een [scoringprofiel](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) waarin het veld meer relatief gewicht. Azure Search ondersteunt via [50 taalanalyse](https://docs.microsoft.com/azure/search/search-language-support) om uit te kiezen.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? De functie aanvragen op de [User Voice-website](https://feedback.azure.com/forums/263029-azure-search).

## <a name="see-also"></a>Zie ook

 [StackOverflow: Azure Search](https://stackoverflow.com/questions/tagged/azure-search)   
 [Hoe vol tekstzoekopdrachten werkt in Azure Search](search-lucene-query-architecture.md)  
 [Wat is Azure Search?](search-what-is-azure-search.md)
