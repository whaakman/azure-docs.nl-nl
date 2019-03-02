---
title: JSON-blobs indexeren van Azure Blob-indexering voor zoeken in volledige tekst - Azure Search
description: Verken Azure JSON-blobs voor tekstinhoud met behulp van de indexeerfunctie Azure Search Blob. Indexeerfuncties automatiseren opname van gegevens voor bepaalde gegevensbronnen, zoals Azure Blob-opslag.
ms.date: 02/28/2019
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: f287648758d2883226132c0f45418dacaaf27652
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57216319"
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexeren van JSON-blobs met de indexeerfunctie Azure Search Blob
In dit artikel leest u hoe het configureren van een indexeerfunctie Azure Search blob gestructureerde inhoud ophalen van JSON-documenten in Azure Blob-opslag en doorzoekbare in Azure Search. Deze werkstroom wordt een Azure Search-index gemaakt en wordt geladen met de bestaande tekst die is geëxtraheerd uit JSON-blobs. 

U kunt de [portal](#json-indexer-portal), [REST-API's](#json-indexer-rest), of [.NET SDK](#json-indexer-dotnet) naar JSON-inhoud indexeren. Gemeenschappelijke alle methoden is dat de JSON-documenten bevinden zich in een blob-container in een Azure Storage-account. Zie voor instructies over het pushen van JSON-documenten in andere niet-Azure-platforms, [gegevens importeren in Azure Search](search-what-is-data-import.md).

JSON-blobs in Azure Blob-opslag zijn doorgaans een enkele JSON-document of een verzameling van JSON-entiteiten. Voor JSON-verzamelingen, de blob kan hebben een **matrix** van opgemaakte JSON-elementen. BLOBs kunnen ook bestaan uit meerdere afzonderlijke JSON-entiteiten van elkaar gescheiden door een nieuwe regel. Dergelijke constructie, afhankelijk van hoe u ingesteld kan worden geparseerd in de blob-indexeerfunctie in Azure Search de **parsingMode** parameter voor de aanvraag.

> [!IMPORTANT]
> `json` en `jsonArray` parseermodi zijn algemeen beschikbaar, maar `jsonLines` parseermodus is in openbare preview en mag niet worden gebruikt in een productieomgeving. Zie voor meer informatie, [REST api-version = 2017-11-11-Preview](search-api-2017-11-11-preview.md). 

> [!NOTE]
> Volg de aanbevelingen van de configuratie van indexeerfunctie in [een-op-veel indexeren](search-howto-index-one-to-many-blobs.md) voor uitvoer van meerdere documenten zoeken van een Azure-blob.

<a name="json-indexer-portal"></a>

## <a name="use-the-portal"></a>Gebruik de portal

De eenvoudigste methode voor het indexeren van JSON-documenten, is het gebruik van een wizard in de [Azure-portal](https://portal.azure.com/). Tijdens het parseren van metagegevens in de Azure blob-container, de [ **gegevens importeren** ](search-import-data-portal.md) wizard kunt maken van een standaardindex, wijzen bronvelden toe aan indexvelden doel en laden van de index in één bewerking. U kunt een operationele volledige tekst search-index in minuten hebben, afhankelijk van de grootte en complexiteit van de brongegevens.

Het is raadzaam om met behulp van hetzelfde Azure-abonnement voor Azure Search en Azure-opslag, bij voorkeur in dezelfde regio.

### <a name="1---prepare-source-data"></a>1 - brongegevens voorbereiden

U hebt een Azure storage-account, met Blob-opslag, en een container van JSON-documenten. Als u niet bekend met een van deze taken bent, controleert u de vereiste 'Set up Azure Blob-service en de belasting voorbeeldgegevens' de [cognitive search-quickstart](cognitive-search-quickstart-blob.md#set-up-azure-blob-service-and-load-sample-data).

> [!Important]
> Op de container, zorg dat **openbaar toegangsniveau** is ingesteld op 'Container (anonieme leestoegang voor containers en blobs)'.

### <a name="2---start-import-data-wizard"></a>2 - de wizard gegevens importeren starten

U kunt [start de wizard](search-import-data-portal.md) vanuit de opdrachtbalk op de pagina met Azure Search-service of door te klikken op **Azure Search toevoegen** in de **Blob-service** sectie van uw storage-account navigatiedeelvenster links.

   ![De gegevensopdracht in de portal importeren](./media/search-import-data-portal/import-data-cmd2.png "Start de wizard gegevens importeren")

### <a name="3---set-the-data-source"></a>3 - de gegevensbron instellen

In de **gegevensbron** pagina, de bron moet **Azure Blob Storage**, met de volgende specificaties:

+ **Gegevens uitpakken** moet *inhoud en metagegevens*. Als u deze optie kiest, kan de wizard een indexschema afleiden en wijs de velden voor het importeren.
   
+ **Bij het parseren modus** moet worden ingesteld op *JSON*, *JSON-matrix* of *JSON regels*. 

  *JSON* articulates van elke blob als één zoekopdracht documenten, weergegeven als een onafhankelijke item in de zoekresultaten. 

  *JSON-matrix* is voor blobs met opgemaakte JSON-gegevens - goed-opgemaakte JSON komt overeen met een matrix met objecten of heeft een eigenschap die een matrix met objecten is en u wilt dat elk element te worden gelede als een zelfstandige, document onafhankelijke zoeken. Als u blobs zijn complex en u niet kiezen *JSON-matrix* de gehele blob wordt opgenomen als één document.

  *JSON-regels* is voor blobs bestaan uit meerdere JSON-entiteiten, gescheiden door een nieuwe regel, waar u elke entiteit moet worden gelede als een zelfstandige onafhankelijke search-document. Als u blobs zijn complex en u niet kiezen *JSON regels* als één document parseren modus en de volledige blob wordt opgenomen.
   
+ **Storage-container** moet uw opslagaccount en container of een verbindingsreeks die wordt omgezet naar de container opgeven. Op de portalpagina van Blob service krijgt u verbindingsreeksen.

   ![BLOB-gegevensbrondefinitie](media/search-howto-index-json/import-wizard-json-data-source.png)

### <a name="4---skip-the-add-cognitive-search-page-in-the-wizard"></a>4 - slaat u de pagina 'Cognitief zoeken toevoegen' in de wizard

Cognitieve vaardigheden toevoegen is niet nodig voor het importeren van JSON-document. Tenzij u specifiek behoefte aan hebt [zijn onder andere Cognitive Services API's en transformaties](cognitive-search-concept-intro.md) aan uw pijplijn indexering, moet u deze stap overslaan.

Als u wilt de stap overslaat, gaat u eerst naar de volgende pagina.

   ![De knop volgende pagina voor Cognitive Search](media/search-get-started-portal/next-button-add-cog-search.png)

Vanaf deze pagina kunt u verder naar de aanpassing van de index.

   ![Stap voor cognitieve vaardigheden overslaan](media/search-get-started-portal/skip-cog-skill-step.png)

### <a name="5---set-index-attributes"></a>5 - set indexkenmerken

In de **Index** pagina, ziet u een lijst met velden met een gegevenstype en een reeks selectievakjes in voor het instellen van indexkenmerken. De wizard kan een standaardindex op basis van metagegevens en door steekproeven van de brongegevens te genereren. 

De standaardinstellingen produceren vaak een werkbare oplossing: als u een veld (cast als tekenreeks) om te fungeren als de sleutel of het document-ID als unieke identificatie van elk document, evenals de velden die goede kandidaten zijn voor zoeken in volledige tekst en worden opgehaald in een resultatenset. Voor blobs, de `content` veld is de beste kandidaat voor doorzoekbare inhoud.

U kunt de standaardwaarden accepteren of lees de beschrijving van [indexkenmerken](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib) en [taalanalyse](https://docs.microsoft.com/rest/api/searchservice/language-support) overschrijven of vormen een aanvulling op de beginwaarden. 

Neem even de tijd om te controleren van uw selecties. Zodra u de wizard uitvoert, fysieke gegevensstructuren worden gemaakt en kunt u zich niet aan het bewerken van deze velden zonder te verwijderen en opnieuw maken van alle objecten.

   ![De definitie van de blob-index](media/search-howto-index-json/import-wizard-json-index.png)

### <a name="6---create-indexer"></a>6 - indexeerfunctie maken

De wizard maakt volledig is opgegeven, drie afzonderlijke objecten in uw zoekservice. Een gegevensbron en index-object worden opgeslagen als benoemde resources in uw Azure Search-service. De laatste stap maakt u een indexeerfunctie-object. Naamgeving van de indexeerfunctie kan bestaan als een zelfstandige-resource die u kunt plannen en beheren onafhankelijk van het bronobject index en gegevens, die zijn gemaakt in dezelfde volgorde van de wizard.

Als u niet bekend met indexeerfuncties bent, een *indexeerfunctie* is een resource in Azure Search, een externe gegevensbron voor doorzoekbare inhoud te verkennen. De uitvoer van de **gegevens importeren** wizard wordt een indexeerfunctie die uw JSON-gegevensbron te verkennen, doorzoekbare inhoud worden uitgepakt en in een Azure Search-index importeert.

   ![De definitie van de BLOB-indexeerfunctie](media/search-howto-index-json/import-wizard-json-indexer.png)

Klik op **OK** uitvoeren van de wizard en alle objecten maken. Indexeren begint onmiddellijk.

U kunt gegevens importeren in de portal-pagina's bewaken. Voortgang van meldingen geven indexering status en het aantal documenten worden geüpload. 

Wanneer het indexeren is voltooid, kunt u [Search explorer](search-explorer.md) query uitvoeren op uw index.

> [!NOTE]
> Als u de verwachte gegevens niet ziet, moet u mogelijk meer kenmerken instellen op meer velden. Verwijder de index en indexeerfunctie die u zojuist hebt gemaakt en doorloopt u de wizard opnieuw wijzigen van de selecties voor indexkenmerken in stap 5. 

<a name="json-indexer-rest"></a>

## <a name="use-rest-apis"></a>REST-API's gebruiken

U kunt de REST-API gebruiken om te indexeren van JSON-blobs, een gemeenschappelijke driedelige-werkstroom te volgen voor alle indexeerfuncties in Azure Search: maken van een gegevensbron, een index maken, een indexeerfunctie maken. Ophalen van gegevens uit blob storage treedt op wanneer u de indexeerfunctie maken-aanvraag indienen. Nadat deze aanvraag is voltooid, hebt u een index waarin u kunt zoeken. Voorbeeld van de aanvragen die alle drie objecten maken, Zie [REST voorbeeld](#rest-example) aan het einde van deze sectie.

Gebruik voor de code op basis van JSON indexeren, [Postman](search-fiddler.md) en de REST-API voor het maken van deze objecten:

+ [index](https://docs.microsoft.com/rest/api/searchservice/create-index)
+ [Gegevensbron](https://docs.microsoft.com/rest/api/searchservice/create-data-source)
+ [indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

In tegenstelling tot de wizard beheerportal een code-benadering vereist dat u een index hebt in-place, klaar om te accepteren van de JSON-documenten bij het verzenden van de **indexeerfunctie maken** aanvraag.

JSON-blobs in Azure Blob-opslag zijn doorgaans een enkele JSON-document of een JSON 'matrix'. De bouw, afhankelijk van hoe u ingesteld kan worden geparseerd in de blob-indexeerfunctie in Azure Search de **parsingMode** parameter voor de aanvraag.

| JSON-document | parsingMode | Description | Beschikbaarheid |
|--------------|-------------|--------------|--------------|
| Een per-blob | `json` | Geparseerd JSON-blobs als één segment van de tekst. Elk JSON-blob wordt één Azure Search-document. | Algemeen beschikbaar in zowel [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) API en [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) SDK. |
| Meerdere per blob | `jsonArray` | Een JSON-matrix in de blob, waarbij elk element van de matrix een afzonderlijke Azure Search-document wordt geparseerd.  | Beschikbaar in preview in beide [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) API en [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) SDK. |
| Meerdere per blob | `jsonLines` | Een blob die meerdere JSON-entiteiten (een ' matrix' bevat) gescheiden door een nieuwe regel, waarbij elke entiteit een afzonderlijke Azure Search-document wordt worden geparseerd. | Beschikbaar in preview in beide [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) API en [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) SDK. |

### <a name="1---assemble-inputs-for-the-request"></a>1 - invoer voor de aanvraag samenvoegen

U moet de servicenaam en de beheersleutel voor Azure Search (in de bericht-header) en de naam van het opslagaccount en de sleutel voor blob-opslag opgeven voor elke aanvraag. U kunt [Postman](search-fiddler.md) HTTP-aanvragen verzenden naar Azure Search.

Kopieer de volgende vier waarden in Kladblok, zodat u ze in een aanvraag kunt plakken:

+ Naam van een Azure Search-service
+ Azure Search-administratorsleutel
+ Naam van het Azure-opslagaccount
+ Azure storage-accountsleutel

U kunt deze waarden vinden in de portal:

1. In de portal-pagina's voor Azure Search, kopieert u de search service-URL van de pagina overzicht.

2. Klik in het linkernavigatiedeelvenster op **sleutels** en kopieer het primaire of secundaire sleutel (ze zijn equivalent).

3. Schakel over naar de portal-pagina's voor uw opslagaccount. In het navigatiedeelvenster links onder **instellingen**, klikt u op **toegangssleutels**. Deze pagina bevat de accountnaam en de sleutel. Kopieer de naam van het opslagaccount en een van de sleutels naar Kladblok.

### <a name="2---create-a-data-source"></a>2 - een gegevensbron maken

Deze stap voorziet in informatie van de gegevensbronverbinding die worden gebruikt door de indexeerfunctie. De gegevensbron is een object met de naam in Azure Search dat de verbindingsgegevens zich blijft voordoen. Het type, gegevensbron `azureblob`, bepaalt welke gegevens extractie gedrag worden aangeroepen door de indexeerfunctie. 

Vervangen door geldige waarden voor de servicenaam van de, beheersleutel en storage-account en sleutels tijdelijke aanduidingen account.

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

### <a name="3---create-a-target-search-index"></a>3 - een doel search-index maken 

Indexeerfuncties zijn gekoppeld aan een indexschema. Als u gebruikmaakt van de API (in plaats van de portal), bereid een index op voorhand zodat u kunt deze op de indexbewerking opgeven.

De index slaat doorzoekbare inhoud samenvoegt in Azure Search. Geef voor het maken van een index, een schema dat Hiermee geeft u de velden in een document, kenmerken en andere constructies die de zoekervaring vorm. Als u een index met de namen van dezelfde velden en gegevenstypen als de bron maakt, wordt de bron- en velden, zodat u het werk om toe te wijzen expliciet de velden die overeenkomen met de indexeerfunctie.

Het volgende voorbeeld wordt een [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) aanvraag. De index heeft een doorzoekbare `content` veld voor het opslaan van de tekst die is geëxtraheerd uit blobs:   

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="4---configure-and-run-the-indexer"></a>4 - configureren en de indexeerfunctie uitvoeren

Als met een index en een gegevensbron en indexeerfunctie zijn ook een benoemd exemplaar object te maken en gebruiken op een Azure Search-service. Een volledig opgegeven aanvraag voor het maken van een indexeerfunctie kan er als volgt uitzien:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Configuratie van de indexeerfunctie is in de hoofdtekst van de aanvraag. Hiervoor is een gegevensbron en een lege doelindex die al in Azure Search bestaat. 

Planning en parameters zijn optioneel. Als u ze niet opgeeft, de indexeerfunctie wordt uitgevoerd onmiddellijk, met behulp van `json` als de modus voor het parseren.

Deze specifieke indexeerfunctie bevat geen [veldtoewijzingen](#field-mappings). In het definitie van de indexeerfunctie, u kunt weglaten **veldtoewijzingen** als de eigenschappen van de bron-JSON-document overeenkomt met de velden van uw doel search-index. 

### <a name="parsing-modes"></a>Parseermodi

Tot nu toe zijn definities voor de gegevensbron en index parsingMode neutraal. Echter, in de stap voor de configuratie van de indexeerfunctie wordt het pad afwijkt, afhankelijk van hoe u wilt dat de JSON-blob inhoud zijn geparseerd en structuur in een Azure Search-index. U kunt kiezen uit `json` of `jsonArray`:

+ Stel **parsingMode** naar `json` indexeren van elke blob als één document.

+ Stel **parsingMode** naar `jsonArray` als uw blobs bestaan uit JSON-matrices en moet u elk element van de matrix om te worden van een afzonderlijke document in Azure Search. 

+ Stel **parsingMode** naar `jsonLines` als uw blobs bestaan uit meerdere JSON-entiteiten, die worden gescheiden door een nieuwe regel, en moet u elke entiteit om te worden van een afzonderlijke document in Azure Search.

U kunt een document zien als één item in de zoekresultaten. Als u wilt dat elk element in de matrix worden weergegeven in zoekresultaten als onafhankelijk item, gebruikt u de `jsonArray` of `jsonLines` optie waar nodig.

In het definitie van de indexeerfunctie, kunt u eventueel gebruiken [veldtoewijzingen](search-indexer-field-mappings.md) om te kiezen welke eigenschappen van de bron-JSON-document worden gebruikt om uw doel search-index te vullen. Voor `jsonArray` parseermodus, als de matrix als een eigenschap op lager niveau bestaat, kunt u een document-basis waarmee wordt aangegeven waar de matrix wordt geplaatst in de blob instellen.

> [!IMPORTANT]
> Bij het gebruik `json`, `jsonArray` of `jsonLines` parseermodus, Azure Search wordt ervan uitgegaan dat alle blobs in uw gegevensbron JSON bevatten. Als u nodig hebt ter ondersteuning van een combinatie van JSON en niet-JSON-blobs in de dezelfde gegevensbron, laat het ons weten op [onze UserVoice-site](https://feedback.azure.com/forums/263029-azure-search).


### <a name="how-to-parse-single-json-blobs"></a>Het parseren van één JSON-blobs

Standaard [indexeerfunctie voor Azure Search blob](search-howto-indexing-azure-blob-storage.md) JSON-blobs worden geparseerd als een enkel segment van de tekst. Vaak wilt u de structuur van uw JSON-documenten behouden. Stel dat u hebt de volgende JSON-document in Azure Blob-opslag:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13",
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

De blob-indexeerfunctie parseert het JSON-document in een Azure Search-document. De indexeerfunctie wordt een index geladen door die overeenkomen met 'tekst', 'datePublished' en 'tags' van de bron op basis van de index met dezelfde naam en getypte doelvelden.

Veldtoewijzingen zijn niet vereist, zoals is vermeld. Gegeven van een index met 'tekst', ' datePublished en 'tags' velden en de blob indexeerfunctie kunt afleiden van de juiste toewijzing zonder een veldtoewijzing aanwezig zijn in de aanvraag.

### <a name="how-to-parse-json-arrays-in-a-well-formed-json-document"></a>Het parseren van JSON-matrices in een opgemaakte JSON-document

U kunt ook kiezen voor de functie voor JSON-matrix. Deze mogelijkheid is nuttig wanneer blobs bevatten een *matrix met JSON-objecten*, en u wilt dat elk element om te worden van een afzonderlijke Azure Search-document. Bijvoorbeeld, kunt uitgaande van de volgende JSON-blob, u vullen uw Azure Search-index met drie afzonderlijke documenten, elk met velden 'id' en 'tekst'.  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Voor een JSON-matrix, moet definitie van de indexeerfunctie lijken op het volgende voorbeeld. U ziet dat de parameter parsingMode geeft u de `jsonArray` parser. De juiste parser op te geven en met de juiste gegevens zijn invoer de slechts twee matrix-specifieke vereisten voor het indexeren van JSON-blobs.

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }

Nogmaals, zoals u ziet dat veldtoewijzingen kunnen worden weggelaten. Ervan uitgaande dat een index met dezelfde naam 'id' en 'tekst' velden, kan de blob-indexeerfunctie de juiste toewijzing zonder een expliciete toewijzing van de veldenlijst afleiden.

<a name="nested-json-arrays"></a>

### <a name="nested-json-arrays"></a>Geneste JSON-matrices
Wat gebeurt er als u wilt een matrix met JSON-objecten, maar die matrix indexeren ergens is ingesloten in het document? U kunt kiezen welke eigenschap bevat de matrix via de `documentRoot` configuratie-eigenschap. Bijvoorbeeld, als uw blobs als volgt uitzien:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

Deze configuratie gebruiken om te indexeren van de matrix die is opgenomen in de `level2` eigenschap:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

### <a name="how-to-parse-blobs-with-multiple-json-entities-separated-by-newlines"></a>Het parseren van blobs met meerdere JSON-entiteiten, gescheiden door nieuwe regels

Als uw blob meerdere JSON-entiteiten bevat, gescheiden door een nieuwe regel en u voor elk element in om te worden van een afzonderlijke Azure Search-document wilt, kunt u zich voor de functie van de JSON-regels. Bijvoorbeeld, krijgt de volgende blob (indien er drie verschillende entiteiten in de JSON zijn), kunt u uw Azure Search-index met drie seprate documenten, elk met 'id' en 'tekst' velden invullen.

    { "id" : "1", "text" : "example 1" }
    { "id" : "2", "text" : "example 2" }
    { "id" : "3", "text" : "example 3" }

Voor een JSON-regels, moet definitie van de indexeerfunctie lijken op het volgende voorbeeld. U ziet dat de parameter parsingMode geeft u de `jsonLines` parser. 

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonLines" } }
    }

Nogmaals, ziet u dat veldtoewijzingen weggelaten, vergelijkbaar met worden kunnen de `jsonArray` parseermodus.

### <a name="using-field-mappings-to-build-search-documents"></a>U veldtoewijzingen voor het bouwen van documenten zoeken

Als bron en doel-velden zijn niet perfect uitgelijnd, kunt u een sectie met toewijzing in de hoofdtekst van de aanvraag voor koppelingen met expliciete veld te definiëren.

Op dit moment Azure Search kan niet worden geïndexeerd willekeurige JSON-documenten rechtstreeks omdat alleen primitieve gegevenstypen, tekenreeksmatrices en GeoJSON-punten ondersteunt. U kunt echter **veldtoewijzingen** te selecteren van onderdelen van uw JSON-document 'lift"deze in op het hoogste niveau van de velden van het zoekdocument. Zie voor meer informatie over de basisprincipes van veld toewijzingen, [veldtoewijzingen in Azure Search-indexeerfuncties](search-indexer-field-mappings.md).

Opnieuw bezoeken onze voorbeeld-JSON-document:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Wordt ervan uitgegaan dat een search-index met de volgende velden: `text` van het type `Edm.String`, `date` van het type `Edm.DateTimeOffset`, en `tags` van het type `Collection(Edm.String)`. U ziet het verschil tussen 'datePublished' in de bron en `date` veld in de index. Om toe te wijzen uw JSON de gewenste vorm te geven, gebruikt u de volgende veldtoewijzingen:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

De bron-veldnamen in de toewijzingen zijn opgegeven met behulp van de [JSON-aanwijzer](https://tools.ietf.org/html/rfc6901) notatie. U start met een slash om te verwijzen naar de hoofdmap van uw JSON-document en vervolgens de gewenste eigenschap (op het niveau van het nesten van willekeurige) kiezen met behulp van forward slash gescheiden pad.

U kunt ook afzonderlijke matrixelementen verwijzen met behulp van een op nul gebaseerde index. Bijvoorbeeld, zodat het eerste element van de matrix 'tags' uit het bovenstaande voorbeeld, gebruikt u een veldtoewijzing als volgt:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Als naam van een bron in het pad naar een veld toewijzing naar een eigenschap die niet bestaat in JSON verwijst, wordt deze toewijzing overgeslagen zonder fouten. Dit wordt gedaan zodat we kunnen documenten met een ander schema (dit is een gebruikelijk) ondersteunen. Omdat er geen validatie is, moet u voorzichtig zijn om te voorkomen dat typfouten in het veld toewijzing-specificatie.
>
>

### <a name="rest-example"></a>REST-voorbeeld

In deze sectie vindt u een samenvatting van alle aanvragen die worden gebruikt voor het maken van objecten. Zie de voorgaande secties in dit artikel voor een bespreking van onderdelen.

### <a name="data-source-request"></a>Aanvraag voor bron

Alle indexeerfuncties vereisen object voor een gegevensbron met de contactgegevens van verbinding met bestaande gegevens. 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }  


### <a name="index-request"></a>Index-aanvraag

Alle indexeerfuncties vereisen een doelindex waarin de gegevens worden ontvangen. De hoofdtekst van de aanvraag definieert het indexschema, die bestaan uit velden, ter ondersteuning van het gewenste gedrag in een doorzoekbare index toegeschreven. Deze index mag niet leeg zijn wanneer u de indexeerfunctie worden uitgevoerd. 

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="indexer-request"></a>Indexeerfunctie voor aanvraag

Deze aanvraag bevat een indexeerfunctie volledig opgegeven. Het bevat [veldtoewijzingen](#field-mappings), die in de vorige voorbeelden zijn weggelaten. Intrekken van 'schema', 'parameters' en 'fieldMappings' zijn optioneel, zolang er een standaardinstelling beschikbaar is. Als u weglaat 'plannen' zorgt ervoor dat de indexeerfunctie onmiddellijk wilt uitvoeren. Als "parsingMode" zorgt ervoor dat de index van de standaard 'json'.

Het maken van de indexeerfunctie voor Azure Search, activeert het importeren van gegevens. Deze wordt uitgevoerd onmiddellijk, en daarna volgens een schema als u een hebt opgegeven.

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }


<a name="json-indexer-dotnet"></a>

## <a name="use-net-sdk"></a>.NET SDK gebruiken

De .NET SDK is volledig pariteit met de REST-API. Het is raadzaam om de vorige sectie van de REST-API om te leren van concepten, werkstromen en vereisten te controleren. U kunt vervolgens verwijzen naar de volgende .NET API-referentiedocumentatie voor het implementeren van een JSON-indexeerfunctie in beheerde code.

+ [microsoft.azure.search.models.datasource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet)
+ [microsoft.azure.search.models.datasourcetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) 
+ [microsoft.azure.search.models.index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 
+ [microsoft.azure.search.models.indexer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)

## <a name="see-also"></a>Zie ook

+ [Indexeerfuncties in Azure Search](search-indexer-overview.md)
+ [Azure Blob Storage met Azure Search indexeren](search-howto-index-json-blobs.md)
+ [Indexeren van CSV-blobs met de indexeerfunctie voor Azure Search blob](search-howto-index-csv-blobs.md)
+ [Zelfstudie: Semi-gestructureerde gegevens uit Azure Blob-opslag doorzoeken](search-semi-structured-data.md)
