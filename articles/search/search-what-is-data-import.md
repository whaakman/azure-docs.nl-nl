---
title: Gegevens uploaden in Azure Search | Microsoft Docs
description: Informatie over het uploaden van gegevens naar een index in Azure Search
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: aa8d47c1-4ae6-4209-a8ce-48d5a9474707
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/09/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 72cc0d9ff35ff656a6134b52812b64c39a295a6f
ms.openlocfilehash: 53786d60d9971d9f976bf0f3ef4e40346c3101f4


---
# <a name="upload-data-to-azure-search"></a>Gegevens uploaden naar Azure Search
> [!div class="op_single_selector"]
> * [Overzicht](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

## <a name="data-upload-models-in-azure-search"></a>Modellen voor het uploaden van gegevens in Azure Search
Er zijn twee manieren om uw Azure Search-index te vullen met uw gegevens. De eerste optie is het handmatig pushen van de gegevens in de index met behulp van de Azure Search [REST-API](search-import-data-rest-api.md) of [.NET SDK](search-import-data-dotnet.md). De tweede optie is om [een ondersteunde gegevensbron](search-indexer-overview.md) te wijzen naar uw Azure Search-index en ervoor te zorgen dat Azure Search automatisch gegevens naar de zoekservice pusht.

Deze handleiding heeft alleen betrekking op instructies voor het gebruik van het pushmodel voor het uploaden van gegevens (dat alleen wordt ondersteund in de [REST-API](search-import-data-rest-api.md) en [.NET SDK](search-import-data-dotnet.md)). U kunt echter meer informatie opvragen over het onderstaande pull-model.

## <a name="push-data-to-an-index"></a>Gegevens naar een index verzenden
Deze aanpak verwijst naar het programmatisch verzenden van uw gegevens naar Azure Search, zodat deze beschikbaar zijn voor een zoekopdracht. Voor toepassingen waarvoor een lage latentie is vereist (bijvoorbeeld wanneer en zoekopdracht gesynchroniseerd is met dynamische inventarisatiedatabases) is de pushoptie de enige mogelijkheid.

U kunt de [REST-API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) of [.NET SDK](search-import-data-dotnet.md) gebruiken om gegevens naar een index te pushen. Er is momenteel geen ondersteuning voor het pushen van gegevens via de portal.

Deze aanpak is flexibeler dan het pull-model, omdat u documenten afzonderlijk of in batches kunt uploaden (maximaal 1000 per batch of 16 MB, afhankelijk van waar de limiet ligt). Met het pushmodel kunt u documenten uploaden naar Azure Search, ongeacht waar uw gegevens zich bevinden.

## <a name="pull-data-into-an-index"></a>Gegevens in een index ophalen
Het pullmodel verkent een ondersteunde gegevensbron en uploadt automatisch de gegevens in uw Azure Search-index. Als u naast het herkennen van nieuwe documenten, wijzigingen en verwijderingen in een bestaand document bijhoudt, hoeft u de gegevens in een index via indexeerfuncties niet actief bij te houden.

In Azure Search is deze mogelijkheid geïmplementeerd via *indexeerfuncties*, die momenteel beschikbaar zijn voor [Blob Storage (preview-versie)](search-howto-indexing-azure-blob-storage.md), [DocumentDB](http://aka.ms/documentdb-search-indexer), [Azure SQL database en SQL Server op Azure VM’s](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md).

De functionaliteit van de indexeerfunctie wordt weergegeven in [Azure Portal](search-import-data-portal.md) en de [REST-API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations).




<!--HONumber=Dec16_HO2-->


