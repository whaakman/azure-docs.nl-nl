---
title: Een Azure Search-index opnieuw maken of vernieuwen van doorzoekbare inhoud - Azure Search
description: Nieuwe elementen toe te voegen, bestaande elementen of documenten bijwerken of verwijderen van verouderde documenten in een volledig opnieuw samenstellen of een gedeeltelijke incrementele indexeren om te vernieuwen van een Azure Search-index.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 9c9af69e45af6a70c5327393a1c10385ba2c2aed
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316893"
---
# <a name="how-to-rebuild-an-azure-search-index"></a>Het opnieuw opbouwen van een Azure Search-index

Opnieuw opbouwen van een index verandert de structuur, het wijzigen van de fysieke expressie van de index in uw Azure Search-service. Vernieuwen van een index is echter een update alleen inhoud van de meest recente wijzigingen uit een bijdragen externe gegevensbron. Dit artikel bevat de richting voor het bijwerken van indexen structureel en inhoudelijk.

Machtigingen voor lezen / schrijven op niveau van de service zijn vereist voor index-updates. U kunt via een programma, REST- of .NET-API's aanroepen voor een volledige opnieuw samenstellen of incrementele indexeren van inhoud, met de parameters op te geven de opties voor het bijwerken. 

Over het algemeen zijn updates naar een index op aanvraag. Echter voor indexen ingevuld met de bron-specifieke [indexeerfuncties](search-indexer-overview.md), kunt u een ingebouwde scheduler. De scheduler ondersteunt document vernieuwen zo vaak als elke 15 minuten, maximaal ongeacht interval en het patroon die u nodig hebt. Een snellere vernieuwingsfrequentie is vereist pushen index-updates handmatig, bijvoorbeeld via een double-schrijfbewerking van transacties, de externe gegevensbron en de Azure Search-index tegelijkertijd bijwerken.

## <a name="full-rebuilds"></a>Volledige opnieuw op te bouwen

Voor veel soorten updates is een volledig opnieuw is gebouwd vereist. Het herbouwen van een volledige verwijst naar het verwijderen van een index, zowel gegevens en metagegevens, gevolgd door de index uit externe gegevensbronnen opnieuw importeert. Via een programma, [verwijderen](https://docs.microsoft.com/rest/api/searchservice/delete-index), [maken](https://docs.microsoft.com/rest/api/searchservice/create-index), en [laden](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) de index opnieuw bouwen. 

Na opnieuw samenstellen, houd er rekening mee dat als u querypatronen is testen en scoreprofielen, u variatie in queryresultaten verwachten kunt als de onderliggende inhoud is gewijzigd.

## <a name="when-to-rebuild"></a>Wanneer u het opnieuw samenstellen

Plan op regelmatige, volledig opnieuw worden opgebouwd tijdens het ontwikkelen van actieve, wanneer de index schema's zijn in een status van lichtstroom.

| Wijziging | Status opnieuw samenstellen|
|--------------|---------------|
| De naam van een veld wijzigen-gegevenstype, of de [indexkenmerken](https://docs.microsoft.com/rest/api/searchservice/create-index) | Wijzigen van de velddefinitie van een doorgaans leidt tot een boete opnieuw maken, met uitzondering van deze [indexkenmerken](https://docs.microsoft.com/rest/api/searchservice/create-index): Ophalen mogelijk, SearchAnalyzer, SynonymMaps. U kunt de kenmerken ophalen mogelijk, SearchAnalyzer en SynonymMaps toevoegen aan een bestaand veld zonder te hoeven de index opnieuw opbouwen.|
| Een veld toevoegen | Er is geen strikte vereiste op opnieuw. Bestaande geïndexeerde documenten worden een null-waarde opgegeven voor het nieuwe veld. In een toekomstige opnieuw indexeren vervangt u waarden uit de brongegevens de null-waarden toegevoegd door Azure Search. |
| Een veld verwijderen | U kunt een veld rechtstreeks uit een Azure Search-index verwijderen. In plaats daarvan moet u uw toepassing het veld 'verwijderd' om te voorkomen met behulp van deze negeren. Fysiek, blijven de velddefinitie van het en de inhoud in de index tot de volgende keer dat u opnieuw opbouwen van uw index met behulp van een schema dat het betreffende veld wordt weggelaten.|

> [!Note]
> Opnieuw opbouwen is ook vereist als u overschakelt van lagen. Als u op meer capaciteit op een bepaald moment besluit, is er geen in-place upgrade. Een nieuwe service moet worden gemaakt op het moment dat nieuwe capaciteit en indexen helemaal moeten worden gebouwd op de nieuwe service. 

## <a name="partial-or-incremental-indexing"></a>Gedeeltelijk of incrementele indexeren

Zodra er een index in de productieomgeving is, wordt de focus verplaatst naar het incrementele indexeren, meestal met geen aanmerkelijke serviceonderbrekingen. Gedeeltelijk of incrementele indexeren is een alleen-inhoud werkbelasting waarmee de inhoud van een zoekindex in overeenstemming met de status van inhoud in een gegevensbron bijdragen worden gesynchroniseerd. Een document toegevoegd of verwijderd uit de bron wordt toegevoegd of verwijderd op de index. In de code roept de [documenten toevoegen, bijwerken of verwijderen](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) bewerking of de .NET-equivalent.

> [!Note]
> Bij het gebruik van indexeerfuncties waarmee externe gegevensbronnen verkennen, worden de mechanismen voor wijzigingen bijhouden in bronsystemen gebruikt voor incrementele indexeren. Voor [Azure Blob-opslag](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection), een `lastModified` veld wordt gebruikt. Op [Azure Table storage](search-howto-indexing-azure-tables.md#incremental-indexing-and-deletion-detection), `timestamp` heeft hetzelfde doel. Op deze manier beide [indexeerfunctie voor Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) en [Azure Cosmos DB-indexeerfunctie](search-howto-index-cosmosdb.md#indexing-changed-documents) velden bevatten voor het markeren van rij-updates. Zie voor meer informatie over indexeerfuncties [overzicht van de indexeerfunctie](search-indexer-overview.md).


## <a name="see-also"></a>Zie ook

+ [Overzicht van de indexeerfunctie](search-indexer-overview.md)
+ [Index grote gegevenssets op schaal](search-howto-large-index.md)
+ [Indexeren in de portal](search-import-data-portal.md)
+ [Indexeerfunctie voor Azure SQL-Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB-indexeerfunctie](search-howto-index-cosmosdb.md)
+ [Indexeerfunctie voor Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Indexeerfunctie voor Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Beveiliging in Azure Search](search-security-overview.md)