<properties
    pageTitle="Indexeerfuncties in Azure Search | Microsoft Azure | Gehoste service voor zoeken in de cloud"
    description="Azure SQL database, DocumentDB of Azure-opslag verkennen om doorzoekbare gegevens op te halen en een Azure Search-index te vullen."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/08/2016"
    ms.author="heidist"/>


# Indexeerfuncties in Azure Search
> [AZURE.SELECTOR]
- [Overzicht](search-indexer-overview.md)
- [Portal](search-import-data-portal.md)
- [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Blob Storage (voorbeeld)](search-howto-indexing-azure-blob-storage.md)
- [Table Storage (voorbeeld)](search-howto-indexing-azure-tables.md)

Een **indexeerfunctie** in Azure Search is een verkenner die doorzoekbare gegevens en metagegevens uit een externe gegevensbron ophaalt en een index vult op basis van veld-naar-veld-verwijzingen tussen de index en de gegevensbron. Deze aanpak wordt ook wel het 'pull-model' genoemd, omdat de service de gegevens ophaalt zonder dat u code hoeft te schrijven om de gegevens naar de index te pushen.

U kunt een indexeerfunctie gebruiken voor de opname van gegevens of een combinatie van technieken gebruiken, waaronder een indexeerfunctie voor het laden van slechts enkele velden in de index.

U kunt indexeerfuncties op verzoek uitvoeren of op basis van een terugkerend schema voor gegevensvernieuwing, dat zo vaak als elke 15 minuten kan worden uitgevoerd. Als u vaker updates wilt uitvoeren, hebt u een pushmodel nodig dat tegelijkertijd gegevens in Azure Search en uw externe gegevensbron bijwerkt.

## Strategieën voor het maken en beheren van indexeerfuncties

U kunt voor algemeen beschikbare indexeerfuncties zoals Azure SQL of DocumentDB met behulp van deze methoden indexeerfuncties maken en beheren:

- [Portal > Wizard Gegevens importeren ](search-get-started-portal.md)
- [Service REST API](https://msdn.microsoft.com/library/azure/dn946891.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx)

Voor previewindexeerfuncties, zoals Azure Blob of Table Storage, zijn code en preview-API's vereist zoals [Azure Search Preview REST API voor indexeerfuncties](search-api-indexers-2015-02-28-preview.md). Portal-tools zijn doorgaans niet beschikbaar voor preview-functies.

## Basisconfiguratiestappen

Indexeerfuncties kunnen functies bieden die uniek voor de gegevensbron zijn. In dit opzicht variëren bepaalde aspecten van de configuratie van de indexeerfunctie of de gegevensbron al naar gelang het type indexeerfunctie. Alle indexeerfuncties hebben echter dezelfde basissamenstelling en voor alle indexeerfuncties gelden dezelfde vereisten. Hieronder vindt u de stappen die voor alle indexeerfuncties gemeenschappelijk zijn.

### Stap 1: een index maken

Een indexeerfunctie automatiseert bepaalde taken met betrekking tot de opname van gegevens, maar het maken van een index behoort hier niet toe. Een vereiste is dat u een vooraf gedefinieerde index moet hebben met velden die overeenkomen met de velden in uw externe gegevensbron. Zie [Create an Index (Azure Search REST API)](https://msdn.microsoft.com/library/azure/dn798941.aspx) (Een index maken (Azure Search REST API)) voor meer informatie over het structureren van een index.

### Stap 2: een gegevensbron maken

Een indexeerfunctie haalt gegevens op uit een **gegevensbron** die informatie, zoals een verbindingsreeks, bevat. Momenteel worden de volgende gegevensbronnen ondersteund:

- [Azure SQL Database of SQL Server op een virtuele Azure-machine](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Azure Blob Storage (preview)](search-howto-indexing-azure-blob-storage.md), wordt gebruikt om tekst op te halen uit PDF’s, Office-documenten, HTML of XML
- [Azure Table Storage (preview)](search-howto-indexing-azure-tables.md)

Gegevensbronnen worden geconfigureerd en onafhankelijk van de indexeerfuncties beheerd die gebruikmaken van de gegevensbronnen. Dit betekent dat een gegevensbron door meerdere indexeerfuncties kan worden gebruikt om tegelijkertijd meer dan één index te laden. 

### Stap 3: de indexeerfunctie maken en plannen

De definitie van de indexeerfunctie is een constructie die de index, gegevensbron en een planning specificeert. Een indexeerfunctie kan verwijzen naar een gegevensbron van een andere service, zolang die gegevensbron maar uit hetzelfde abonnement komt. Zie [Create Indexer (Azure Search REST API)](https://msdn.microsoft.com/library/azure/dn946899.aspx) (Een indexeerfunctie maken (Azure Search REST API)) voor meer informatie over het structureren van een indexeerfunctie.

## Volgende stappen

Nu u het uitgangspunt hebt begrepen, is de volgende stap de vereisten en taken te bekijken die specifiek zijn voor elk gegevensbrontype.

- [Azure SQL Database of SQL Server op een virtuele Azure-machine](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Azure Blob Storage (preview)](search-howto-indexing-azure-blob-storage.md), wordt gebruikt om tekst op te halen uit PDF’s, Office-documenten, HTML of XML
- [Azure Table Storage (preview)](search-howto-indexing-azure-tables.md)
- [Indexeren van CSV-blobs met de indexeerfunctie Azure Search Blob (preview)](search-howto-index-csv-blobs.md)
- [Indexeren van JSON-blobs met de indexeerfunctie Azure Search Blob (preview)](search-howto-index-json-blobs.md)




<!--HONumber=Sep16_HO3-->


