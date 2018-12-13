---
title: Gegevens importeren voor opname van gegevens naar een search-index - Azure Search
description: Vul en gegevens uploaden naar een index in Azure Search uit externe gegevensbronnen.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 01/05/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 731519b4e099bd696002af3aa08ada145e490260
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314853"
---
# <a name="indexing-external-data-for-queries-in-azure-search"></a>Externe gegevens voor query's in Azure Search indexeren
> [!div class="op_single_selector"]
> * [Overzicht](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

In Azure Search worden query's uitgevoerd over uw inhoud in geladen en opgeslagen in een [search-index](search-what-is-an-index.md). Dit artikel worden de twee basismethoden voor het invullen van een index behandeld: *push* uw gegevens in de index via een programma, of wijs een [Azure Search-indexeerfunctie](search-indexer-overview.md) op een ondersteunde gegevensbron om  *pull* in de gegevens.

## <a name="pushing-data-to-an-index"></a>Gegevens naar een index pushen
Het pushmodel, dat wordt gebruikt voor het programmatisch verzenden van uw gegevens naar Azure Search, is de meest flexibele methode. Ten eerste heeft dit model geen beperkingen met betrekking tot het gegevensbrontype. Alle gegevenssets die bestaan uit JSON-documenten kunnen naar een Azure Search-index worden gepusht. Hierbij wordt ervan uitgegaan dat elk document in de gegevensset velden toewijst aan velden die in uw indexschema zijn gedefinieerd. Ten tweede heeft dit model geen beperkingen met betrekking tot de frequentie van de uitvoering. U kunt wijzigingen naar een index pushen zo vaak als u wilt. Voor toepassingen die een zeer lage latentie vereisen (bijvoorbeeld als zoekopdrachten gesynchroniseerd moeten zijn met dynamische inventarisatiedatabases) is het pushmodel de enige mogelijkheid.

Deze aanpak is flexibeler dan het pull-model, omdat u documenten afzonderlijk of in batches kunt uploaden (maximaal 1000 per batch of 16 MB, afhankelijk van waar de limiet ligt). Met het pushmodel kunt u documenten uploaden naar Azure Search, ongeacht waar uw gegevens zich bevinden.

### <a name="how-to-push-data-to-an-azure-search-index"></a>Gegevens naar een Azure Search-index pushen

U kunt de volgende API's gebruiken om één of meerdere documenten in een index te laden:

+ [Documenten toevoegen, bijwerken of verwijderen (REST API)](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents)
+ [indexAction-klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction?view=azure-dotnet) of [indexBatch-klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) 

Er is momenteel geen ondersteuning voor het pushen van gegevens via de portal.

Zie [Gegevens importeren met REST](search-import-data-rest-api.md) of [Gegevens importeren met .NET](search-import-data-dotnet.md) voor een inleiding tot elke methodologie.


## <a name="pulling-data-into-an-index"></a>Gegevens in een index ophalen
Het pullmodel verkent een ondersteunde gegevensbron en uploadt de gegevens automatisch naar uw index. In Azure Search is deze mogelijkheid geïmplementeerd via *indexeerfuncties*, die momenteel beschikbaar zijn voor de volgende platforms:

+ [Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Table Storage](search-howto-indexing-azure-tables.md)
+ [Azure Cosmos DB](https://aka.ms/documentdb-search-indexer)
+ [Azure SQL Database en SQL Server op Azure VM's](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

Indexeerfuncties verbinden een index met een gegevensbron (meestal een tabel, weergave of equivalente structuur) en wijzen bronvelden toe aan equivalente velden in de index. Tijdens de uitvoering wordt de rijenset automatisch omgezet naar JSON en in de opgegeven index geladen. Alle indexeerfuncties ondersteunen planning. U kunt dus opgeven hoe vaak de gegevens moeten worden vernieuwd. Met de meeste indexeerfuncties kunt u wijzigingen bijhouden als dit door de gegevensbron wordt ondersteund. Als u naast het herkennen van nieuwe documenten, wijzigingen en verwijderingen in een bestaand document bijhoudt, hoeft u de gegevens in een index via indexeerfuncties niet actief bij te houden. 


### <a name="how-to-pull-data-into-an-azure-search-index"></a>Gegevens in een Azure Search-index ophalen

Functionaliteit van de indexeerfunctie wordt weergegeven in [Azure Portal](search-import-data-portal.md), de [REST API](/rest/api/searchservice/Indexer-operations) en de [.NET SDK](/dotnet/api/microsoft.azure.search.indexersoperationsextensions). 

Een voordeel van het gebruik van de portal is dat Azure Search meestal een standaardindexschema voor u kan genereren door de metagegevens van de brongegevensset te lezen. U kunt de gegenereerde index wijzigen totdat de index wordt verwerkt. Daarna zijn alleen schemabewerkingen toegestaan waarvoor de index niet opnieuw gegenereerd hoeft te worden. Als de wijzigingen die u doorvoert, rechtstreeks invloed hebben op het schema, moet u de index opnieuw opbouwen. 

## <a name="verify-data-import-with-search-explorer"></a>Gegevensimport met Search explorer controleren

Een snelle manier om uit te voeren van een voorlopige controle op het document uploaden, is met **Search explorer** in de portal. Met de explorer kunt u gegevens uit een index opvragen zonder code te schrijven. De zoekervaring is gebaseerd op standaardinstellingen, zoals de [eenvoudige syntaxis](/rest/api/searchservice/simple-query-syntax-in-azure-search) en de standaard-queryparameter [searchMode](/rest/api/searchservice/search-documents). Resultaten worden geretourneerd in JSON, zodat u het hele document kunt inspecteren.

> [!TIP]
> Tal van [Azure Search-codevoorbeelden](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) bevatten ingesloten of kant-en-klare gegevenssets, zodat u gemakkelijk aan de slag kunt gaan. De portal bevat ook een voorbeeldindexeerfunctie en een gegevensbron, bestaande uit een kleine vastgoedgegevensset ('realestate-us-sample'). Wanneer u de vooraf geconfigureerde indexeerfunctie op de voorbeeld-gegevensbron uitvoert, wordt een index gemaakt en geladen met documenten die vervolgens kunnen worden doorzocht in Search explorer of met code die u schrijft.

## <a name="see-also"></a>Zie ook

+ [Overzicht van de indexeerfunctie](search-indexer-overview.md)
+ [Portaloverzicht: een index maken, gegevens laden, een query in een index uitvoeren](search-get-started-portal.md)
