---
title: Gegevens importeren voor opname van gegevens naar een search-index - Azure Search
description: Vul en gegevens uploaden naar een index in Azure Search uit externe gegevensbronnen.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 2c3da9470668fa2987195c26e98eee51f14027f7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58136341"
---
# <a name="data-import-overview---azure-search"></a>Gegevens importeren overzicht - Azure Search

In Azure Search worden query's uitgevoerd over uw inhoud in geladen en opgeslagen in een [search-index](search-what-is-an-index.md). Dit artikel worden de twee basismethoden voor het invullen van een index behandeld: *push* uw gegevens in de index via een programma, of wijs een [Azure Search-indexeerfunctie](search-indexer-overview.md) op een ondersteunde gegevensbron om  *pull* in de gegevens.

Met beide benaderingen, het doel is het *gegevens laden* vanaf een externe gegevensbron in een Azure Search-index. Azure Search kunt u een lege index te maken, maar totdat u push- of gegevens in het ophalen, is het niet waarin u kunt zoeken.

## <a name="pushing-data-to-an-index"></a>Gegevens naar een index pushen
Het pushmodel, dat wordt gebruikt voor het programmatisch verzenden van uw gegevens naar Azure Search, is de meest flexibele methode. Ten eerste heeft dit model geen beperkingen met betrekking tot het gegevensbrontype. Alle gegevenssets die bestaan uit JSON-documenten kunnen naar een Azure Search-index worden gepusht. Hierbij wordt ervan uitgegaan dat elk document in de gegevensset velden toewijst aan velden die in uw indexschema zijn gedefinieerd. Ten tweede heeft dit model geen beperkingen met betrekking tot de frequentie van de uitvoering. U kunt wijzigingen naar een index pushen zo vaak als u wilt. Voor toepassingen die een zeer lage latentie vereisen (bijvoorbeeld als zoekopdrachten gesynchroniseerd moeten zijn met dynamische inventarisatiedatabases) is het pushmodel de enige mogelijkheid.

Deze aanpak is flexibeler dan het pull-model, omdat u documenten afzonderlijk of in batches kunt uploaden (maximaal 1000 per batch of 16 MB, afhankelijk van waar de limiet ligt). Met het pushmodel kunt u documenten uploaden naar Azure Search, ongeacht waar uw gegevens zich bevinden.

### <a name="how-to-push-data-to-an-azure-search-index"></a>Gegevens naar een Azure Search-index pushen

U kunt de volgende API's gebruiken om één of meerdere documenten in een index te laden:

+ [Documenten toevoegen, bijwerken of verwijderen (REST API)](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents)
+ [indexAction-klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction?view=azure-dotnet) of [indexBatch-klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) 

Er is momenteel geen ondersteuning voor het pushen van gegevens via de portal.

Zie voor een inleiding tot elke methodologie [Quick Start: Een Azure Search-index met behulp van PowerShell en de REST-API maken](search-create-index-rest-api.md) of [Quick Start: Maken van een Azure Search-index in C# ](search-import-data-dotnet.md).

<a name="indexing-actions"></a>

### <a name="indexing-actions-upload-merge-uploadormerge-delete"></a>Indexeren van acties: uploaden, samenvoegen, uploadOrMerge, verwijderen

Wanneer u de REST-API gebruikt, doet u HTTP POST-aanvragen met JSON-aanvraagtekst bij de eindpunt-URL van uw Azure Search-index. Het JSON-object in de hoofdtekst van uw HTTP-aanvraag bevat een enkele JSON-matrix met de naam ''waarde''. Deze matrix bevat JSON-objecten die de documenten vertegenwoordigen die u aan uw index wilt toevoegen, wilt bijwerken of wilt wijzigen.

Elk JSON-object in de matrix ''waarde'' vertegenwoordigt een document dat moet worden geïndexeerd. Elk van deze objecten bevat de sleutel van het document en Hiermee geeft u de gewenste indexeringsbewerking (uploaden, samenvoegen, verwijderen). Afhankelijk van welke van de onderstaande bewerkingen u kiest, moet u slechts bepaalde velden voor elk document opnemen:

| @search.action | Description | Vereiste velden voor elk document | Opmerkingen |
| -------------- | ----------- | ---------------------------------- | ----- |
| `upload` |Een `upload`-actie is vergelijkbaar met een "upsert", waarbij het document wordt ingevoegd als het nieuw is en wordt bijgewerkt/vervangen als het al bestaat. |sleutel, plus andere velden die u wilt definiëren |Tijdens het bijwerken/vervangen van een bestaand document wordt elk veld dat niet is opgegeven in de aanvraag ingesteld op `null`. Dit gebeurt zelfs als het veld eerder is ingesteld op een niet-null-waarde. |
| `merge` |Een bestaand document wordt bijgewerkt met de opgegeven velden. Als het document niet in de index bestaat, mislukt de samenvoeging. |sleutel, plus andere velden die u wilt definiëren |Alle velden die u in een samenvoeging opgeeft, vervangen de bestaande velden in het document, ook velden van het type `Collection(Edm.String)`. Als het document bijvoorbeeld een veld `tags` bevat met de waarde `["budget"]` en u een samenvoeging doet met de waarde `["economy", "pool"]` voor `tags`, wordt de uiteindelijke waarde van het veld `tags` `["economy", "pool"]`. Het wordt dus niet `["budget", "economy", "pool"]`. |
| `mergeOrUpload` |Deze bewerking gedraagt zich als `merge` wanneer een document met de opgegeven sleutel al in de index bestaat. Als het document niet bestaat, gedraagt deze bewerking zich als `upload` met een nieuw document. |sleutel, plus andere velden die u wilt definiëren |- |
| `delete` |Het opgegeven document wordt uit de index verwijderd. |alleen sleutel |Alle andere velden worden genegeerd. Als u een afzonderlijk veld uit een document wilt verwijderen, gebruikt u `merge` en stelt u het veld expliciet in op null. |

### <a name="formulate-your-query"></a>Uw query formuleren
Er zijn twee manieren om [in de index te zoeken met behulp van de REST-API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). De ene manier is om een HTTP POST-aanvraag uit te geven waarbij uw queryparameters worden gedefinieerd in een JSON-object in de aanvraagtekst. De andere manier is om een HTTP GET-aanvraag uit te geven waarbij uw queryparameters worden gedefinieerd in de aanvraag-URL. POST heeft [soepelere limieten](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) met betrekking tot de grootte van queryparameters dan GET. Daarom wordt u aangeraden POST te gebruiken, tenzij er speciale omstandigheden zijn waarin het gebruik van GET beter zou zijn.

Voor zowel POST als GET moet u in de aanvraag-URL de *servicenaam*, de *indexnaam* en de juiste *API-versie* (de huidige API-versie is `2017-11-11` op het moment van publicatie van dit document) opgeven. Voor GET geeft u in de *querytekenreeks* aan het einde van de URL de queryparameters op. Hieronder vindt u de URL-indeling:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2017-11-11

De indeling voor POST is hetzelfde, maar met alleen de api-versie in de queryreeksparameters.


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
