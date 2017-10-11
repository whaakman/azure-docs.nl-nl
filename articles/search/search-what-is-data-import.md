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
ms.date: 05/01/2017
ms.author: ashmaka
ms.openlocfilehash: 5a601b75ec67824e72d8736bc3c45f8e1231ca86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="upload-data-to-azure-search"></a>Gegevens uploaden naar Azure Search
> [!div class="op_single_selector"]
> * [Overzicht](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Er zijn twee manieren om een index te vullen met uw gegevens. De eerste optie is het handmatig pushen van de gegevens in de index met behulp van de Azure Search [REST-API](search-import-data-rest-api.md) of [.NET SDK](search-import-data-dotnet.md). De tweede optie is om [een ondersteunde gegevensbron](search-indexer-overview.md) te laten wijzen naar uw index en ervoor te zorgen dat Azure Search de gegevens automatisch in de zoekservice binnenhaalt.

## <a name="push-data-to-an-index"></a>Gegevens naar een index verzenden
Deze aanpak verwijst naar het programmatisch verzenden van uw gegevens naar Azure Search, zodat deze beschikbaar zijn voor een zoekopdracht. Voor toepassingen die een zeer lage latentie vereisen (bijvoorbeeld als zoekopdrachten gesynchroniseerd moeten zijn met dynamische inventarisatiedatabases) is het pushmodel de enige mogelijkheid.

U kunt de [REST-API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) of [.NET SDK](search-import-data-dotnet.md) gebruiken om gegevens naar een index te pushen. Er is momenteel geen ondersteuning voor het pushen van gegevens via de portal.

Deze aanpak is flexibeler dan het pull-model, omdat u documenten afzonderlijk of in batches kunt uploaden (maximaal 1000 per batch of 16 MB, afhankelijk van waar de limiet ligt). Met het pushmodel kunt u documenten uploaden naar Azure Search, ongeacht waar uw gegevens zich bevinden.

De gegevensindeling die door Azure Search wordt ondersteund is JSON, en alle documenten in de gegevensset moeten velden toewijzen aan velden die in uw indexschema zijn gedefinieerd. 

## <a name="pull-data-into-an-index"></a>Gegevens in een index ophalen
Het pullmodel verkent een ondersteunde gegevensbron en uploadt de gegevens automatisch naar uw index. In Azure Search is deze mogelijkheid geïmplementeerd via *indexeerfuncties*, die momenteel beschikbaar zijn voor [Blob Storage](search-howto-indexing-azure-blob-storage.md), [Table Storage](search-howto-indexing-azure-tables.md), [Azure Cosmos DB](http://aka.ms/documentdb-search-indexer), [Azure SQL-database en SQL Server op Azure VM's](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md). 

Indexeerfuncties verbinden een index met een gegevensbron (meestal een tabel, weergave of equivalente structuur) en wijzen bronvelden toe aan equivalente velden in de index. Tijdens de uitvoering wordt de rijenset automatisch omgezet naar JSON en in de opgegeven index geladen. Alle indexeerfuncties ondersteunen planning. U kunt dus opgeven hoe vaak de gegevens moeten worden vernieuwd. Met de meeste indexeerfuncties kunt u wijzigingen bijhouden als dit door de gegevensbron wordt ondersteund. Als u naast het herkennen van nieuwe documenten, wijzigingen en verwijderingen in een bestaand document bijhoudt, hoeft u de gegevens in een index via indexeerfuncties niet actief bij te houden. 

Functionaliteit van de indexeerfunctie wordt weergegeven in [Azure Portal](search-import-data-portal.md), de [REST API](/rest/api/searchservice/Indexer-operations) en de [.NET SDK](/dotnet/api/microsoft.azure.search.indexersoperations). 

Een voordeel van het gebruik van de portal is dat Azure Search meestal een standaardindexschema voor u kan genereren door de metagegevens van de brongegevensset te lezen. U kunt de gegenereerde index wijzigen totdat de index wordt verwerkt. Daarna zijn alleen schemabewerkingen toegestaan waarvoor de index niet opnieuw gegenereerd hoeft te worden. Als de wijzigingen die u doorvoert, rechtstreeks invloed hebben op het schema, moet u de index opnieuw opbouwen. 

Nadat de index is ingevuld, kunt u **Search Explorer** in de opdrachtbalk van de portal als verificatiestap gebruiken.

## <a name="query-an-index-using-search-explorer"></a>Gegevens uit een index opvragen met Search Explorer

Met **Search Explorer** in de portal kunt u snel een eerste controle van de documentupload uitvoeren. Met de explorer kunt u gegevens uit een index opvragen zonder code te schrijven. De zoekervaring is gebaseerd op standaardinstellingen, zoals de [eenvoudige syntaxis](/rest/api/searchservice/simple-query-syntax-in-azure-search) en de standaard-queryparameter [searchMode](/rest/api/searchservice/search-documents). Resultaten worden geretourneerd in JSON, zodat u het hele document kunt inspecteren.

> [!TIP]
> Tal van [Azure Search-codevoorbeelden](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) bevatten ingesloten of kant-en-klare gegevenssets, zodat u gemakkelijk aan de slag kunt gaan. De portal bevat ook een voorbeeldindexeerfunctie en een gegevensbron, bestaande uit een kleine vastgoedgegevensset ('realestate-us-sample'). Als u de vooraf geconfigureerde indexeerfunctie uitvoert met de voorbeeldgegevensbron, wordt er een index gemaakt. Deze wordt geladen met documenten waarvoor u query’s kunt uitvoeren via Search Explorer of met code die u schrijft.