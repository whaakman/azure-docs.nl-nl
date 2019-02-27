---
title: Indexeerfuncties voor verkennen gegevensbronnen tijdens het indexeren - Azure zoeken
description: Azure SQL database, Azure Cosmos DB of Azure Storage verkennen om doorzoekbare gegevens op te halen en een Azure Search-index te vullen.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 10/17/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 754bd06e6033b49d24112cb20686e1c9b200d0d0
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56875477"
---
# <a name="indexers-in-azure-search"></a>Indexeerfuncties in Azure Search

Een *indexeerfunctie* in Azure Search is een Verkenner die doorzoekbare gegevens en metagegevens geëxtraheerd uit een externe Azure-gegevensbron en een index vult op basis van veld naar toewijzingen tussen de index en uw gegevensbron. Deze aanpak wordt ook wel het 'pull-model' genoemd, omdat de service de gegevens ophaalt zonder dat u code hoeft te schrijven om de gegevens naar de index te pushen.

Indexeerfuncties zijn gebaseerd op typen gegevensbronnen of platforms met afzonderlijke indexeerfuncties voor SQL Server op Azure, Cosmos DB, Azure Table Storage en Blob Storage enzovoort.

U kunt een indexeerfunctie gebruiken voor de opname van gegevens of een combinatie van technieken gebruiken, waaronder een indexeerfunctie voor het laden van slechts enkele velden in de index.

U kunt indexeerfuncties op verzoek uitvoeren of op basis van een terugkerend schema voor gegevensvernieuwing, dat zo vaak als elke 15 minuten kan worden uitgevoerd. Als u vaker updates wilt uitvoeren, hebt u een pushmodel nodig dat tegelijkertijd gegevens in Azure Search en uw externe gegevensbron bijwerkt.

## <a name="approaches-for-creating-and-managing-indexers"></a>Strategieën voor het maken en beheren van indexeerfuncties

U kunt op de volgende manieren indexeerfuncties maken en beheren:

* [Portal > Gegevenswizard importeren](search-import-data-portal.md)
* [Service REST API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations)

Een nieuwe indexeerfunctie wordt in eerste instantie aangekondigd als preview-functie. Preview-functies worden geïntroduceerd in API's (REST en .NET) en vervolgens geïntegreerd in de portal nadat ze geleidelijk algemeen beschikbaar zijn gesteld. Als u een nieuwe indexeerfunctie evalueert, moet u er rekening mee houden dat u code moet schrijven.


<a name="supported-data-sources"></a>

## <a name="supported-data-sources"></a>Ondersteunde gegevensbronnen

Indexeerfuncties verkennen gegevensarchieven in Azure.

* [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md) 
    * Houd er rekening mee dat Azure Table Storage wordt niet ondersteund voor [cognitief zoeken](cognitive-search-concept-intro.md)


## <a name="basic-configuration-steps"></a>Basisconfiguratiestappen
Indexeerfuncties kunnen functies bieden die uniek voor de gegevensbron zijn. In dit opzicht variëren bepaalde aspecten van de configuratie van de indexeerfunctie of de gegevensbron al naar gelang het type indexeerfunctie. Alle indexeerfuncties hebben echter dezelfde basissamenstelling en voor alle indexeerfuncties gelden dezelfde vereisten. Hieronder vindt u de stappen die voor alle indexeerfuncties gemeenschappelijk zijn.

### <a name="step-1-create-a-data-source"></a>Stap 1: Een gegevensbron maken
Een indexeerfunctie haalt gegevens op uit een *gegevensbron* die informatie bevat, zoals een verbindingsreeks en mogelijk aanmeldingsgegevens. Roep de [Datasource maken](https://docs.microsoft.com/rest/api/searchservice/create-data-source) REST-API of [DataSource-klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource) om de resource te maken.

Gegevensbronnen worden geconfigureerd en onafhankelijk van de indexeerfuncties beheerd die gebruikmaken van de gegevensbronnen. Dit betekent dat een gegevensbron door meerdere indexeerfuncties kan worden gebruikt om tegelijkertijd meer dan één index te laden.

### <a name="step-2-create-an-index"></a>Stap 2: Een index maken
Een indexeerfunctie automatiseert bepaalde taken met betrekking tot de opname van gegevens, maar het maken van een index behoort hier niet toe. Een vereiste is dat u een vooraf gedefinieerde index moet hebben met velden die overeenkomen met de velden in uw externe gegevensbron. Zie voor meer informatie over het structureren van een index [maken van een Index (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/Create-Index) of [Index-klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index). Zie [Veldtoewijzingen in Azure Search-indexeerfuncties](search-indexer-field-mappings.md) voor hulp bij veldkoppelingen.

> [!Tip]
> Hoewel indexeerfuncties een index niet voor u kunnen genereren, kan de wizard **Gegevens importeren** in de portal helpen. In de meeste gevallen kan de wizard een indexschema afleiden uit bestaande metagegevens in de bron, met een voorlopig indexschema dat u kunt bewerken terwijl de wizard actief is. Als de index eenmaal is aangemaakt op de service, blijven verdere bewerkingen in de portal meestal beperkt tot het toevoegen van nieuwe velden. Overweeg de wizard voor het maken, maar niet voor het herzien van een index. Doorloop het [Portaloverzicht](search-get-started-portal.md) om aan de slag te gaan.

### <a name="step-3-create-and-schedule-the-indexer"></a>Stap 3: Maken en plannen van de indexeerfunctie
De definitie van de indexeerfunctie is een constructie die de index, gegevensbron en een planning specificeert. Een indexeerfunctie kan verwijzen naar een gegevensbron van een andere service, zolang die gegevensbron maar uit hetzelfde abonnement komt. Zie [Create Indexer (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer) (Een indexeerfunctie maken (Azure Search REST API)) voor meer informatie over het structureren van een indexeerfunctie.

## <a name="next-steps"></a>Volgende stappen
Nu u het uitgangspunt hebt begrepen, is de volgende stap de vereisten en taken te bekijken die specifiek zijn voor elk gegevensbrontype.

* [Azure SQL Database of SQL Server op een virtuele Azure-machine](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Indexeren van CSV-blobs met de indexeerfunctie Azure Search Blob](search-howto-index-csv-blobs.md)
* [Indexeren van JSON-blobs met de indexeerfunctie Azure Search Blob](search-howto-index-json-blobs.md)
