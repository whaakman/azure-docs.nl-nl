---
title: Een Azure Search-index opnieuw maken of vernieuwen van doorzoekbare inhoud - Azure Search
description: Nieuwe elementen toe te voegen, bestaande elementen of documenten bijwerken of verwijderen van verouderde documenten in een volledig opnieuw samenstellen of een gedeeltelijke incrementele indexeren om te vernieuwen van een Azure Search-index.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 1d9dffe9d311674aeb043fcc4c35110775f420af
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56300802"
---
# <a name="how-to-rebuild-an-azure-search-index"></a>Het opnieuw opbouwen van een Azure Search-index

In dit artikel wordt uitgelegd hoe u een Azure Search-index, de omstandigheden waaronder opnieuw worden opgebouwd vereist zijn en aanbevelingen voor het oplossen van de impact van opnieuw op te bouwen op actieve queryaanvragen opnieuw.

Een *opnieuw* te verwijderen en opnieuw maken van de fysieke gegevensstructuren die zijn gekoppeld aan een index, met inbegrip van alle omgekeerde indexen op basis van het veld verwijst. U kunt geen in Azure Search, verwijderen en opnieuw maken van afzonderlijke velden. Als u wilt een index opnieuw maken, moet alle veld opslag worden verwijderd, opnieuw worden gemaakt op basis van een bestaande of nieuwe indexschema en vervolgens opnieuw worden gevuld met gegevens naar de index gepusht of opgehaald uit externe bronnen. Is het gebruikelijk dat indexen opnieuw samenstellen tijdens de ontwikkeling, maar mogelijk moet u ook opnieuw opbouwen van een index van de productie-niveau om te voldoen aan de structurele wijzigingen, zoals het toevoegen van complexe typen of het toevoegen van velden aan suggesties.

In tegenstelling tot opnieuw op te bouwen die offline nemen van een index *gegevensvernieuwing* wordt uitgevoerd als een achtergrondtaak. U kunt toevoegen, verwijderen en vervang documenten met minimale verstoring voor querywerkbelastingen, hoewel query's doorgaans het langer duren om. Zie voor meer informatie over het bijwerken van inhoud indexeren [documenten toevoegen, bijwerken of verwijderen](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).

## <a name="rebuild-conditions"></a>Opnieuw opbouwen van voorwaarden

| Voorwaarde | Description |
|-----------|-------------|
| De velddefinitie van een wijzigen | Wijzigen van een veldnaam, gegevenstype of specifieke [indexkenmerken](https://docs.microsoft.com/rest/api/searchservice/create-index) (doorzoekbaar Filterbaar, sorteerbaar, geschikt voor facetten) is vereist voor een volledig opnieuw is gebouwd. |
| Een analyzer aan een veld toe te voegen | [Analyzers](search-analyzers.md) zijn gedefinieerd in een index en vervolgens toegewezen aan velden. U kunt een analyzer toevoegen aan een index op elk gewenst moment, maar u kunt alleen een analyzer toewijzen als het veld wordt gemaakt. Dit geldt voor zowel de **analyzer** en **indexAnalyzer** eigenschappen. De **searchAnalyzer** eigenschap is een uitzondering.
| Een veld toevoegt aan een suggestie | Als er al een veld bestaat en u wilt toevoegen aan een [suggesties](index-add-suggesters.md) maken, moet u de index opnieuw opbouwen. |
| Verwijderen van een veld | Als u wilt fysiek verwijdert alle traceringen van een veld, die u moet de index opnieuw opbouwen. Wanneer een direct opnieuw opbouwen niet praktijken, wijzig de meeste ontwikkelaars toepassingscode om uit te schakelen toegang tot het veld 'verwijderd'. Fysiek, blijven de velddefinitie van het en de inhoud in de index tot het volgende opnieuw bouwen met behulp van een schema dat het veld wordt weggelaten in kwestie. |
| Schakelen tussen lagen | Als u meer capaciteit nodig hebt, is er geen in-place upgrade. Een nieuwe service wordt gemaakt op het moment dat nieuwe capaciteit en indexen helemaal moeten worden gebouwd op de nieuwe service. |

Andere wijzigingen kan worden gemaakt zonder gevolgen voor bestaande fysieke structuren. Met name de volgende wijzigingen doen *niet* duiden op een index opnieuw opbouwen:

+ Een nieuw veld toevoegen
+ Stel de **ophalen mogelijk** kenmerk voor een bestaand veld
+ Stel een **searchAnalyzer** op een bestaand veld
+ Toevoegen, bijwerken of verwijderen van een analyzer-constructie in een index
+ Toevoegen, bijwerken of verwijderen van scoreprofielen
+ Toevoegen, bijwerken of verwijderen van CORS-instellingen
+ Toevoegen, bijwerken of verwijderen van synonymMaps

Wanneer u een nieuw veld toevoegt, worden bestaande geïndexeerde documenten een null-waarde opgegeven voor het nieuwe veld. Waarden van gegevens van de externe bron vervangen in een toekomstige gegevensvernieuwing, de null-waarden toegevoegd door Azure Search.

## <a name="partial-or-incremental-indexing"></a>Gedeeltelijk of incrementele indexeren

In Azure Search, kunt niet u bepalen op basis van per veld te indexeren om te verwijderen of opnieuw maken van specifieke velden te kiezen. Er is ook geen ingebouwd mechanisme voor [indexering van documenten op basis van criteria](https://stackoverflow.com/questions/40539019/azure-search-what-is-the-best-way-to-update-a-batch-of-documents). Alle vereisten voor criteria gebaseerde indexering hebt moeten worden voldaan met aangepaste code.

Wat u gemakkelijk kunt doen, is echter *vernieuwen documenten* in een index. Voor veel zoekoplossingen, externe bron de gegevens zijn vluchtig en synchronisatie tussen de brongegevens en een search-index is normaal. In de code roept de [documenten toevoegen, bijwerken of verwijderen](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) bewerking of de [.NET-equivalent](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexesoperationsextensions.createorupdate?view=azure-dotnet) inhoud indexeren bijwerken of het toevoegen van waarden voor een nieuw veld.

## <a name="partial-indexing-with-indexers"></a>Gedeeltelijk indexeren met indexeerfuncties

[Indexeerfuncties](search-indexer-overview.md) vereenvoudigen van de taak voor het vernieuwen van gegevens. Een indexeerfunctie kan slechts één tabel of weergave in de externe gegevensbron indexeren. Om te indexeren meerdere tabellen, is de eenvoudigste manier om te maken van een weergave die lid wordt van tabellen en projecten de kolommen die u wilt om te indexeren. 

Wanneer met de indexeerfuncties die externe gegevensbronnen verkennen, controleert u voor een kolom 'hoge water mark' in de brongegevens. Als er een bestaat, kunt u deze voor de detectie van incrementele wijzigen door het verzamelen van alleen die rijen met nieuwe of gewijzigde inhoud. Voor [Azure Blob-opslag](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection), een `lastModified` veld wordt gebruikt. Op [Azure Table storage](search-howto-indexing-azure-tables.md#incremental-indexing-and-deletion-detection), `timestamp` heeft hetzelfde doel. Op deze manier beide [indexeerfunctie voor Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) en [Azure Cosmos DB-indexeerfunctie](search-howto-index-cosmosdb.md#indexing-changed-documents) velden bevatten voor het markeren van rij-updates. 

Zie voor meer informatie over indexeerfuncties [overzicht van de indexeerfunctie](search-indexer-overview.md) en [Reset indexeerfunctie REST API](https://docs.microsoft.com/rest/api/searchservice/reset-indexer).

## <a name="how-to-rebuild-an-index"></a>Hoe u een index opnieuw maken

Plan op regelmatige, volledig opnieuw worden opgebouwd tijdens het ontwikkelen van actieve, wanneer de index schema's zijn in een status van lichtstroom. Voor toepassingen die al in de productieomgeving, wordt u aangeraden een nieuwe index die wordt uitgevoerd naast elkaar een bestaande index om te voorkomen uitvaltijd van de query te maken.

Als er strenge vereisten voor de SLA, kunt u een nieuwe service speciaal voor dit werk, met de ontwikkeling van inrichting en indexeren die zich voordoen op volle isolatie van een productie-index overwegen. Een afzonderlijke service wordt uitgevoerd op een eigen hardware, een kans op conflicten tussen resources te elimineren. Wanneer ontwikkeling voltooid is, ofwel laat u de nieuwe index aanwezig is, query's omleiden naar het nieuwe eindpunt en de index of u de voltooide code voor het publiceren van een herziene-index in uw oorspronkelijke Azure Search-service wordt uitgevoerd. Er is momenteel geen mechanisme voor het verplaatsen van een kant-en-klare-index op een andere service.

Machtigingen voor lezen / schrijven op niveau van de service zijn vereist voor index-updates. Via een programma, kunt u bellen [Update Index REST-API](https://docs.microsoft.com/rest/api/searchservice/update-index) of .NET-API's voor een volledig opnieuw is gebouwd. De aanvraag is gelijk aan [Index REST-API maken](https://docs.microsoft.com/rest/api/searchservice/create-index), maar heeft een andere context.

1. Als u de naam van de index [de bestaande index](https://docs.microsoft.com/rest/api/searchservice/delete-index). Alle query's die zijn gericht op die index worden onmiddellijk verwijderd. Verwijderen van een index is onomkeerbaar, fysieke opslag voor de Veldenverzameling en andere constructies vernietigen. Zorg ervoor dat u bent wissen op de gevolgen van het verwijderen van een index voordat u deze verwijderen. 

2. Een indexschema voorzien van de gewijzigde of gewijzigde velddefinities. Schemavereisten worden beschreven in [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index).

3. Geef een [administratorsleutel](https://docs.microsoft.com/azure/search/search-security-api-keys) op de aanvraag.

4. Verzendt een [Index bijwerken](https://docs.microsoft.com/rest/api/searchservice/update-index) opdracht voor het opnieuw opbouwen van de fysieke expressie van de index voor Azure Search. Hoofdtekst van de aanvraag bevat het indexschema, evenals constructs voor het scoren van profielen, analyses, suggesties en CORS-opties.

5. [Laden van de index met documenten](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) vanuit een externe bron. U kunt deze API ook gebruiken als u een bestaande, ongewijzigd indexschema met bijgewerkte documenten vernieuwt.

Wanneer u de index maakt, wordt voor elk veld in het indexschema fysieke opslag toegewezen met een omgekeerde index gemaakt voor elk doorzoekbaar veld. Velden die niet kan worden doorzocht kan worden gebruikt in filters of expressies, maar kan geen omgekeerde indexen en zijn geen volledige-tekstindex of fuzzy zijn doorzoekbaar. Op een indexen deze omgekeerde indexen worden verwijderd en opnieuw gemaakt op basis van het indexschema dat u opgeeft.

Wanneer u de index laadt, wordt van elk veld omgekeerde index wordt gevuld met alle van de unieke, tokens woorden uit met een toewijzing aan de bijbehorende document-id's van elk document. Bijvoorbeeld, wanneer een gegevensset hotels indexeren, kan een omgekeerde index gemaakt voor een plaatsveld termen bevatten voor Seattle, Portland, enzovoort. Documenten die Seattle of Portland in het veld Plaats opnemen moet de document-ID weergegeven samen met de term. Op een [toevoegen, bijwerken of verwijderen](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) bewerking, de voorwaarden en de lijst met document-ID worden dienovereenkomstig bijgewerkt.

## <a name="view-updates"></a>Updates weergeven

U kunt beginnen met een index opvragen zodra het eerste document wordt geladen. Als u van een document-ID, weet de [Lookup Document REST-API](https://docs.microsoft.com/rest/api/searchservice/lookup-document) retourneert het specifieke document. Voor bredere testen, moet u wachten totdat de index volledig geladen is en vervolgens query's gebruiken om te controleren of de context die u verwacht te zien.

## <a name="see-also"></a>Zie ook

+ [Overzicht van de indexeerfunctie](search-indexer-overview.md)
+ [Index grote gegevenssets op schaal](search-howto-large-index.md)
+ [Indexeren in de portal](search-import-data-portal.md)
+ [Indexeerfunctie voor Azure SQL-Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB-indexeerfunctie](search-howto-index-cosmosdb.md)
+ [Indexeerfunctie voor Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Indexeerfunctie voor Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Beveiliging in Azure Search](search-security-overview.md)