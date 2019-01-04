---
title: Index grote gegevensset met de ingebouwde indexeerfuncties - Azure Search
description: Meer informatie over strategieën voor grote hoeveelheden gegevens te indexeren of rekenintensief zijn indexering door de batchmodus, te en technieken voor geplande, parallelle en gedistribueerde indexeren.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 2f3d08a32384cea815f096f51b24eea596d0d118
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742232"
---
# <a name="how-to-index-large-data-sets-in-azure-search"></a>Grote gegevenssets in Azure Search indexeren

Als de gegevensvolumes toenemen of verwerking moet wijzigen, is het wellicht standaard indexering strategieën zijn niet meer praktische. Voor Azure Search zijn er verschillende manieren om te bepalen of grotere gegevenssets, variërend van hoe u een aanvraag van de gegevens uploaden voor het gebruik van een bron-specifieke indexeerfunctie voor geplande en gedistribueerde werkbelastingen structureren.

Dezelfde technieken voor grote hoeveelheden gegevens zijn ook van toepassing op langlopende processen. In het bijzonder de stappen die worden beschreven in [parallelle indexeren](#parallel-indexing) zijn handig voor het indexeren van rekenintensief zijn, zoals analyse van de afbeelding of de verwerking van natuurlijke taal [cognitief zoeken pijplijnen](cognitive-search-concept-intro.md).

## <a name="batch-indexing"></a>Indexeren van batch

Een van de eenvoudigste mechanismen voor een grotere set gegevens indexeren is om in te dienen meerdere documenten of records in één aanvraag. Zolang de nettolading van de gehele onder 16 MB is, kan een aanvraag tot 1000 documenten in een bulkbewerking uploaden verwerken. Ervan uitgaande dat de [toevoegen of bijwerken documenten REST-API](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents), zou u 1000 documenten in de hoofdtekst van de aanvraag verpakken.

Indexeren van batch is geïmplementeerd voor afzonderlijke aanvragen met behulp van REST- of .NET of via indexeerfuncties. Een paar indexeerfuncties werken onder andere limieten. Met name stelt Azure Blob-indexering batchgrootte op 10 documenten als erkenning van de grootte van de grotere gemiddelde document. Voor indexeerfuncties op basis van de [indexeerfunctie REST-API maken](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer ), kunt u instellen de `BatchSize` argument voor het aanpassen van deze instelling om beter overeen met de kenmerken van uw gegevens. 

> [!NOTE]
> Houd er rekening mee om te voorkomen dat documentgrootte omlaag, moeten worden uitgesloten om gegevens van de aanvraag. Afbeeldingen en andere binaire gegevens zijn niet rechtstreeks kan worden doorzocht en mag niet worden opgeslagen in de index. Als u wilt integreren om gegevens in de zoekresultaten, moet u een niet-doorzoekbaar veld waarin een URL-verwijzing naar de resource definiëren.

## <a name="add-resources"></a>Resources toevoegen

Services die zijn ingericht op een van de [Standard Prijscategorieën](search-sku-tier.md) dikwijls onvoldoende capaciteit voor zowel opslag als werkbelastingen (query's of indexeren), waardoor hebt gebruikt [verhogen van het aantal partitie en replica's ](search-capacity-planning.md) een duidelijk oplossing voor grotere gegevenssets laten werken. Voor optimale resultaten, moet u beide resources: partities voor opslag en replica's voor de taken die gegevens opnemen.

Steeds meer replica's en partities zijn factureerbare gebeurtenissen die uw kosten te verhogen, maar, tenzij u continu met maximale belasting indexeren worden, kunt u schaal toevoegen voor de duur van het indexeringsproces worden geautomatiseerd en pas vervolgens de resource niveaus omlaag nadat het indexeren is is voltooid.

## <a name="use-indexers"></a>Gebruik van indexeerfuncties

[Indexeerfuncties](search-indexer-overview.md) worden gebruikt voor het verkennen van externe gegevensbronnen voor doorzoekbare inhoud. Hoewel niet specifiek bedoeld voor grootschalige indexeren, zijn verschillende mogelijkheden voor indexeerfunctie bijzonder nuttig voor grotere gegevenssets voor:

+ Planners kunnen u pakket uit het indexeren met regelmatige intervallen, zodat u kunt deze verspreid na verloop van tijd.
+ Geplande indexeren kunt hervatten op het laatste punt van de bekende moet worden gestopt. Als een gegevensbron is niet volledig binnen een 24-uurs tijdvenster verkend, hervat de indexeerfunctie indexeren op dag twee op waar het afgebroken.
+ Partitioneren van gegevens in kleinere afzonderlijke gegevensbronnen kunt u parallelle verwerking. U kunt een grote gegevensset opdelen in kleinere gegevenssets, en maak vervolgens meerdere definities voor gegevensbronnen die kunnen worden geïndexeerd parallel.

> [!NOTE]
> Indexeerfuncties zijn data-source-specifieke, met behulp van een indexeerfunctie benadering is alleen beschikbaar voor geselecteerde gegevensbronnen op Azure: [SQL-Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Blob storage](search-howto-indexing-azure-blob-storage.md), [Table storage](search-howto-indexing-azure-tables.md), [Cosmos DB](search-howto-index-cosmosdb.md).

## <a name="scheduled-indexing"></a>Geplande indexeren

Plannen van de indexeerfunctie is een belangrijk mechanisme voor het verwerken van grote gegevenssets, evenals de trage processen, zoals het analyseren van afbeeldingen in een pijplijn cognitief zoeken. Verwerking van de indexeerfunctie werkt binnen een 24-uurs tijdvenster. Als de verwerking niet kan worden voltooid binnen 24 uur, wordt het gedrag van het plannen van de indexeerfunctie kunnen werken in uw voordeel. 

Standaard gepland indexering begint met een bepaald interval, met een taak die doorgaans voltooien voordat u met de volgende geplande interval wordt hervat. Als de verwerking niet binnen het interval wordt voltooid, stopt de indexeerfunctie echter (vanwege tijd onvoldoende). Op het volgende interval bijhouden verwerking wordt hervat waar deze laatste afgebroken, met het system-bijhouden van waar dat wordt uitgevoerd. 

U kunt de indexeerfunctie volgens een schema 24-uurs plaatsen in de praktijk bij index belastingen spanning enkele dagen. Tijdens het indexeren wordt hervat voor de volgende cyclus van 24 uur per dag, opnieuw wordt gestart op de laatste bekende goede document. Op deze manier kan een indexeerfunctie eraan via een achterstand van het document werken gedurende een reeks dagen totdat alle niet-verwerkte documenten worden verwerkt. Zie voor meer informatie over deze benadering, [grote gegevenssets in Azure Blob storage indexeren](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets). Zie voor meer informatie over het instellen van schema's in het algemeen [indexeerfunctie REST-API maken](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer#request-syntax).

<a name="parallel-indexing"></a>

## <a name="parallel-indexing"></a>Parallelle indexeren

Een parallelle indexeren strategie is gebaseerd op meerdere gegevensbronnen databasetaken, waarbij elke definitie van de gegevensbron Hiermee geeft u een subset van de gegevens te indexeren. 

Voor niet-routine, rekenintensieve indexering vereisten - zoals OCR op gescande documenten in een pijplijn cognitief zoeken, analyse van de afbeelding of verwerking van natuurlijke taal - een parallelle strategie indexeren is vaak de juiste aanpak voor het voltooien van een langdurig proces binnen de kortste tijd. Als u kunt voorkomen of query-aanvragen verminderen, is parallelle indexering van een service die niet tegelijkertijd van query's verwerken is de beste strategie-optie voor het uitvoeren van een grote instantie van de inhoud van de langzame-verwerking. 

Parallelle verwerking heeft de volgende elementen:

+ Verdeel brongegevens tussen meerdere containers of meerdere virtuele mappen in dezelfde container. 
+ Elke mini gegevensset toewijzen aan een eigen [Datumbron](https://docs.microsoft.com/rest/api/searchservice/create-data-source), in een eigen gekoppelde [indexeerfunctie](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ Verwijzen naar hetzelfde voor cognitief zoeken [vaardigheden](https://docs.microsoft.com/rest/api/searchservice/create-skillset) in elke definitie van de indexeerfunctie.
+ Schrijven naar het hetzelfde doel search-index. 
+ Alle indexeerfuncties om uit te voeren op hetzelfde moment plannen.

> [!NOTE]
> Azure Search biedt geen ondersteuning voor uitsluitend replica's of partities op specifieke workloads. Het risico van zware gelijktijdige indexeren is uw systeem ten koste van prestaties van query's overbelasten. Als u een testomgeving hebt, voert u parallelle indexeren er eerst voor meer informatie over het gebruik van systeembronnen.

### <a name="how-to-configure-parallel-indexing"></a>Het configureren van parallelle indexeren

Voor indexeerfuncties, capaciteit voor het verwerken is los op basis van een indexeerfunctie-subsysteem voor elke service-eenheid (SU) die wordt gebruikt door uw search-service. Er zijn meerdere gelijktijdige indexeerfuncties op Azure Search-services die zijn ingericht op basis of standaard lagen hebben van ten minste twee replica's mogelijk. 

1. In de [Azure-portal](https://portal.azure.com), op het servicedashboard van uw zoekopdracht **overzicht** pagina, Controleer de **prijscategorie** om te bevestigen is geschikt voor parallelle indexeren. De lagen basis en standaard bieden meerdere replica's.

2. In **instellingen** > **schaal**, [replica's vergroten](search-capacity-planning.md) voor parallelle verwerking: één extra replica voor elke workload van een indexeerfunctie. Laat voldoende voor bestaande queryvolume. Dit ten koste gaat querywerkbelastingen voor indexeren is niet een goede afweging.

3. Gegevens verdelen over meerdere containers op een niveau dat Azure Search-Indexeerfuncties kunnen bereiken. Dit wordt mogelijk meerdere tabellen in Azure SQL Database, meerdere containers in Azure Blob-opslag of meerdere verzamelingen. Definieer een gegevensbronobject voor elke tabel of de container.

4. Maken en plannen van meerdere indexeerfuncties om parallel uit voeren:

   + Wordt ervan uitgegaan dat een service met zes replica's. Configureer zes indexeerfuncties, elke service die is toegewezen aan een gegevensbron met een zesde van de gegevensset voor een 6-manier splitsing van de volledige gegevensset. 

   + Elke indexeerfunctie verwijzen naar dezelfde index. Voor workloads met cognitief zoeken, in de dezelfde vaardigheden Wijs elke indexeerfunctie.

   + Binnen elke definitie van de indexeerfunctie hetzelfde patroon runtime-uitvoering plannen Bijvoorbeeld, `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` een planning maakt op 2018-05-15 op alle indexeerfuncties, met intervallen van acht uur.

Alle indexeerfuncties begint op het geplande tijdstip wordt uitgevoerd, het laden van gegevens, het toepassen van enrichments (als u een pijplijn cognitief zoeken geconfigureerd) en het schrijven naar de index. Azure Search wordt de index voor de updates niet worden vergrendeld. Gelijktijdige schrijfbewerkingen worden beheerd met nieuwe pogingen als een bepaalde schrijven niet op de eerste poging is gelukt.

> [!Note]
> Overweeg bij het verhogen van replica's, het aantal partities verhogen als de grootte van prognoses aanzienlijk toenemen. Partities kunnen worden opgeslagen segmenten van de geïndexeerde inhoud: de meer partities die u hebt, de kleinere segmenten elkaar heeft om op te slaan.

## <a name="see-also"></a>Zie ook

+ [Overzicht van de indexeerfunctie](search-indexer-overview.md)
+ [Indexeren in de portal](search-import-data-portal.md)
+ [Indexeerfunctie voor Azure SQL-Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB-indexeerfunctie](search-howto-index-cosmosdb.md)
+ [Indexeerfunctie voor Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Indexeerfunctie voor Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Beveiliging in Azure Search](search-security-overview.md)