---
title: Azure Blob Storage met Azure Search indexeren
description: Informatie over het indexeren van Azure Blob Storage en haal de tekst uit documenten met Azure Search
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 2a5968f4-6768-4e16-84d0-8b995592f36a
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 12/28/2017
ms.author: eugenesh
ms.openlocfilehash: 286e2b8eddc87a5132fa13468b0cef1b499c3993
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Documenten in Azure Blob Storage met Azure Search indexeren
Dit artikel laat zien hoe u met Azure Search index documenten (zoals PDF-bestanden, Microsoft Office-documenten en enkele andere algemene indelingen) opgeslagen in Azure Blob storage. Eerst wordt de basisprincipes van instellen en configureren van een blob-indexeerfunctie uitgelegd. Vervolgens biedt een meer gedetailleerde uitleg van problemen en scenario's bent u waarschijnlijk tegenkomen.

## <a name="supported-document-formats"></a>Ondersteunde documentindelingen voor
De indexeerfunctie blob kan tekst ophalen uit de volgende documentindelingen voor het:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="setting-up-blob-indexing"></a>Instellen van de blob-indexering
U kunt instellen dat een Azure Blob Storage indexeerfunctie gebruiken:

* [Azure Portal](https://ms.portal.azure.com)
* Azure Search [REST-API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Search [.NET SDK](https://aka.ms/search-sdk)

> [!NOTE]
> Sommige functies (bijvoorbeeld een veld-verwijzingen) nog niet beschikbaar in de portal en programmatisch worden gebruikt.
>
>

Hier ziet u de stroom met de REST API.

### <a name="step-1-create-a-data-source"></a>Stap 1: een gegevensbron maken
Een gegevensbron geeft aan welke gegevens worden indexeren, referenties die nodig zijn voor toegang tot de gegevens en beleidsregels efficiënt om wijzigingen te bepalen in de gegevens (nieuwe, gewijzigde of verwijderde rijen). Een gegevensbron kan worden gebruikt door meerdere indexeerfuncties in de dezelfde search-service.

De gegevensbron moet de volgende vereiste eigenschappen zijn voor blob-indexering:

* **naam** is de unieke naam van de gegevensbron in uw zoekservice.
* **type** moet `azureblob`.
* **referenties** voorziet in de verbindingsreeks voor opslag account als de `credentials.connectionString` parameter. Zie [het opgeven van referenties](#Credentials) hieronder voor meer informatie.
* **container** Hiermee geeft u een container in uw opslagaccount. Standaard zijn alle blobs in de container worden opgehaald. Als u alleen index blobs in een bepaalde virtuele map wilt, kunt u de map met de optionele opgeven **query** parameter.

Een gegevensbron maken:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Zie voor meer informatie over de API-gegevensbron maken [Datasource maken](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="how-to-specify-credentials"></a>Het opgeven van referenties ####

U kunt de referenties opgeven voor de blob-container in een van de volgende manieren:

- **Volledige toegang account opslagverbindingsreeks**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`. De verbindingsreeks kunt u krijgen via de Azure-portal door te navigeren naar de blade opslagaccount > Instellingen > sleutels (voor klassieke opslagaccounts) of instellingen > toegangssleutels (voor Azure Resource Manager storage-accounts).
- **Shared access signature voor Storage account** (SAS)-verbindingsreeks: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl` de SAS moet de lijst en leesrechten op containers en objecten (blobs in dit geval).
-  **Shared access signature voor containers**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl` de SAS moet de lijst en leesrechten op de container.

Toegang tot de handtekeningen voor meer informatie over gedeelde opslag, Zie [handtekeningen voor gedeelde toegang met behulp van](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Als u SAS-referenties van gebruikt, moet u de referenties voor gegevensbron regelmatig bijgewerkt met vernieuwde handtekeningen om te voorkomen dat ze zijn verlopen. Als de SAS-referenties is verlopen, de indexeerfunctie mislukt met een foutbericht weergegeven dat vergelijkbaar is met `Credentials provided in the connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>Stap 2: een index maken
De index geeft de velden in een document, kenmerken, en andere constructies die de vorm van de zoekopdracht ervaring.

Hier volgt een index maken met een doorzoekbare `content` veld voor het opslaan van de tekst opgehaald uit de BLOB's:   

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }

Zie voor meer informatie over het maken van indexen [Index maken](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="step-3-create-an-indexer"></a>Stap 3: Maak een indexeerfunctie
Een indexeerfunctie een gegevensbron is verbonden met een doel search-index en vindt u een planning voor het automatiseren van de gegevens te vernieuwen.

Als de index en gegevensbron hebt gemaakt, bent u klaar voor het maken van de indexeerfunctie:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Deze indexeerfunctie wordt elke twee uur (schema-interval is ingesteld op 'PT2H') worden uitgevoerd. Als u wilt uitvoeren met een indexeerfunctie elke 30 minuten, stelt u het interval voor 'PT30M'. De kortste interval is 5 minuten. Het schema is optioneel - als u dit weglaat, een indexeerfunctie slechts één keer is uitgevoerd wanneer deze gemaakt. U kunt echter een indexeerfunctie op aanvraag uitvoeren op elk gewenst moment.   

Bekijk voor meer informatie over de indexeerfunctie maken API [indexeerfunctie maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="how-azure-search-indexes-blobs"></a>Hoe Azure Search indexeert blobs

Afhankelijk van de [indexeerfunctie configuratie](#PartsOfBlobToIndex), de indexeerfunctie blob storage bestaan alleen uit metagegevens kunt indexeren (is nuttig wanneer u alleen de metagegevens interesseren en hoeven niet te indexeren van de inhoud van BLOB's), opslag en inhoud metagegevens of beide metagegevens en tekstinhoud. Standaard wordt de indexeerfunctie haalt metagegevens en de inhoud.

> [!NOTE]
> Standaard worden de blobs met gestructureerde inhoud, zoals de JSON- of CSV geïndexeerd als een enkel deel van de tekst. Als u wilt een index van een JSON- en CSV blobs gestructureerd, Zie [indexeren JSON-blobs](search-howto-index-json-blobs.md) en [indexeren CSV blobs](search-howto-index-csv-blobs.md) preview-functies.
>
> Een samengestelde of ingesloten document (zoals een ZIP-archief of een Word-document met ingesloten Outlook e-mail met bijlagen) worden ook geïndexeerd als één document.

* De tekstinhoud van het document wordt geëxtraheerd in een tekenreeksveld met de naam `content`.

> [!NOTE]
> Azure Search beperkt hoeveel tekst pakt deze afhankelijk van de prijscategorie: 32.000 tekens gratis laag, 64.000 voor Basic en 4 miljoen voor Standard-, Standard S2- en Standard-S3 lagen. Een waarschuwing is opgenomen in het statusantwoord indexeerfunctie voor ingekorte documenten.  

* De eigenschappen van de gebruiker opgegeven metagegevens is aanwezig op de blob zijn indien aanwezig, uitgepakt woordelijk.
* Eigenschappen van de standaard blob-metagegevens worden uitgepakt in de volgende velden:

  * **metagegevens\_opslag\_naam** (Edm.String) - de bestandsnaam van de blob. Bijvoorbeeld, als u een blob-/my-container/my-folder/subfolder/resume.pdf hebt, de waarde van dit veld is `resume.pdf`.
  * **metagegevens\_opslag\_pad** (Edm.String) - de volledige URI van de blob, met inbegrip van het opslagaccount. Bijvoorbeeld: `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`
  * **metagegevens\_opslag\_inhoud\_type** (Edm.String) - inhoudstype zoals opgegeven door de code die u gebruikt voor het uploaden van de blob. Bijvoorbeeld `application/octet-stream`.
  * **metagegevens\_opslag\_laatste\_gewijzigd** (Edm.DateTimeOffset) - laatst gewijzigd tijdstempel voor de blob. Azure Search gebruikt dit tijdstempel gewijzigde blobs, om te voorkomen dat alles na de eerste indexering indexeren identificeren.
  * **metagegevens\_opslag\_grootte** (Edm.Int64) - blob-grootte in bytes.
  * **metagegevens\_opslag\_inhoud\_md5** (Edm.String) - MD5-hash van de blob-inhoud, indien beschikbaar.
* Eigenschappen van metagegevens is specifiek voor elke documentindeling worden uitgepakt in de velden die worden vermeld [hier](#ContentSpecificMetadata).

U hoeft niet te definiëren van velden voor elk van de bovenstaande eigenschappen in uw zoekindex - alleen vastleggen van de eigenschappen die u nodig hebt voor uw toepassing.

> [!NOTE]
> De veldnamen in uw bestaande index worden vaak, verschilt van de veldnamen die zijn gegenereerd tijdens het uitpakken van het document. U kunt **veld toewijzingen** toewijzen van de namen van de eigenschappen die door Azure Search aan de veldnamen in uw search-index. Hier ziet u een voorbeeld van een veld toewijzingen hieronder gebruiken.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Document sleutels en veldtoewijzingen definiëren
In Azure Search identificatie de documentsleutel unieke van een document. Elke search-index moet exact één sleutelveld van het type Edm.String hebben. Het sleutelveld is vereist voor elk document dat wordt toegevoegd aan de index (dit is feitelijk de enige vereiste veld).  

U moet zorgvuldig overwegen welke uitgepakte veld moet worden toegewezen aan een veld met de sleutel voor de index. De kandidaten zijn:

* **metagegevens\_opslag\_naam** : dit wordt mogelijk een handige kandidaat, maar opmerking dat (1) de namen mogelijk niet uniek zijn, zoals u blobs met dezelfde naam in andere mappen en 2) de naam wellicht mag bevatten ongeldige tekens ongeldig document-sleutels, zoals streepjes. U kunt maken met ongeldige tekens met behulp van de `base64Encode` [veld toewijzing functie](search-indexer-field-mappings.md#base64EncodeFunction) : als u dit doet, voor het coderen van document sleutels wanneer deze worden doorgegeven in de API zoals Lookup aanroept onthouden. (Bijvoorbeeld in .NET kunt u de [UrlTokenEncode methode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) daarvoor).
* **metagegevens\_opslag\_pad** - uniekheid gebruik van het volledige pad zorgt, maar het pad bevat zeker `/` tekens [ongeldig in een documentsleutel](https://docs.microsoft.com/rest/api/searchservice/naming-rules).  Zoals hierboven, hebt u de optie voor het coderen van de sleutels die met behulp van de `base64Encode` [functie](search-indexer-field-mappings.md#base64EncodeFunction).
* Als geen van de bovenstaande opties voor u werkt, kunt u een aangepaste metagegevenseigenschap toevoegen aan de blobs. Deze optie is, maar uw proces voor het uploaden van blob die metagegevenseigenschap toevoegen aan alle blobs vereist. Omdat de sleutel een vereiste eigenschap is, worden alle blobs waarvoor geen die eigenschap niet worden geïndexeerd.

> [!IMPORTANT]
> Als er geen expliciete toewijzing voor het sleutelveld in de index is, gebruikt Azure Search automatisch `metadata_storage_path` als de sleutel en de base 64-gecodeerd sleutelwaarden (de tweede optie hierboven).
>
>

In dit voorbeeld we kiezen de `metadata_storage_name` veld als de documentsleutel. We gaan ook wordt ervan uitgegaan dat uw index heeft een sleutelveld `key` en een veld `fileSize` voor het opslaan van de grootte van het document. Geef de volgende veldtoewijzingen bij het maken of bijwerken van de indexeerfunctie voor proces gewenste wire:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Om dit allemaal samen, hier wordt het veldtoewijzingen toevoegen en base 64-codering met sleutels voor een bestaande indexeerfunctie inschakelen:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [!NOTE]
> Zie voor meer informatie over het toewijzen van veld, [in dit artikel](search-indexer-field-mappings.md).
>
>

<a name="WhichBlobsAreIndexed"></a>
## <a name="controlling-which-blobs-are-indexed"></a>Beheren welke blobs zijn geïndexeerd.
U kunt bepalen welke blobs worden geïndexeerd en die zijn overgeslagen.

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Alleen de blobs met specifieke extensies indexeren
U kunt alleen de blobs met de bestandsnaamextensies die u via opgeeft de index de `indexedFileNameExtensions` configuratieparameter indexeerfunctie. De waarde is een tekenreeks met een door komma's gescheiden lijst met bestandsextensies (met een voorafgaande punt). Bijvoorbeeld, naar de index alleen de. PDF en. DOCX blobs, doen dit:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions"></a>BLOB's met bepaalde bestandsextensies uitsluiten
U kunt BLOB's met specifieke bestandsnaamextensies uitsluiten van het indexeren met behulp van de `excludedFileNameExtensions` configuratieparameter. De waarde is een tekenreeks met een door komma's gescheiden lijst met bestandsextensies (met een voorafgaande punt). Bijvoorbeeld naar index alle blobs, behalve de pakketten met de. PNG en. JPEG-extensies doen dit:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Als beide `indexedFileNameExtensions` en `excludedFileNameExtensions` parameters aanwezig zijn, Azure Search eerst kijkt `indexedFileNameExtensions`, klikt u vervolgens op `excludedFileNameExtensions`. Dit betekent dat als dezelfde bestandsextensie in beide lijsten aanwezig is, wordt uitgesloten van het indexeren.

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-the-blob-are-indexed"></a>Bepalen welke onderdelen van de blob zijn geïndexeerd.

U kunt bepalen welke onderdelen van de blobs zijn geïndexeerd met behulp van de `dataToExtract` configuratieparameter. Het kan duren voordat de volgende waarden:

* `storageMetadata`-Geeft aan dat alleen de [standaard blob-eigenschappen en metagegevens gebruiker opgegeven](../storage/blobs/storage-properties-metadata.md) zijn geïndexeerd.
* `allMetadata`-Hiermee geeft u de opslag metagegevens en de [inhoudstype specifieke metagegevens](#ContentSpecificMetadata) opgehaald van de blob inhoud worden geïndexeerd.
* `contentAndMetadata`-Geeft aan dat alle metagegevens en tekstinhoud opgehaald uit de blob worden geïndexeerd. Dit is de standaardwaarde.

Bijvoorbeeld alleen de metagegevens van de opslag indexeert, gebruiken:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>Met behulp van de blobmetagegevens van de om te bepalen hoe blobs worden geïndexeerd

De hierboven beschreven configuratieparameters van toepassing op alle blobs. Soms moet u mogelijk wilt bepalen hoe *afzonderlijke blobs* zijn geïndexeerd. U kunt dit doen door de volgende eigenschappen van de blob-metagegevens en -waarden toe te voegen:

| Naam van eigenschap | Waarde van eigenschap | Uitleg |
| --- | --- | --- |
| AzureSearch_Skip |'true' |Hiermee geeft u de indexeerfunctie blob volledig kunt overslaan van de blob. Uitpakken van metagegevens noch inhoud wordt uitgevoerd. Dit is handig wanneer een bepaalde blob herhaaldelijk mislukt en het indexing-proces wordt onderbroken. |
| AzureSearch_SkipContent |'true' |Dit is het equivalent van `"dataToExtract" : "allMetadata"` instelling beschreven [hierboven](#PartsOfBlobToIndex) binnen het bereik van een bepaalde blob. |

<a name="DealingWithErrors"></a>
## <a name="dealing-with-errors"></a>Omgaan met fouten

Standaard de blob-indexeerfunctie stopt zodra er een blob met een niet-ondersteunde inhoudstype (bijvoorbeeld een afbeelding) aangetroffen. U kunt uiteraard de `excludedFileNameExtensions` -parameter voor het overslaan van bepaalde typen inhoud. Echter mogelijk moet u index blobs zonder alle mogelijke inhoudstypen van tevoren weten. Om door te gaan wanneer een niet-ondersteund type inhoud is opgetreden indexeren ingesteld de `failOnUnsupportedContentType` configuratieparameter naar `false`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }

Voor sommige blobs Azure Search kan niet bepalen van het type inhoud is of kan niet worden verwerkt een document van anders ondersteund type inhoud. Als u wilt dat deze fout-modus, stel de `failOnUnprocessableDocument` configuratieparameter op false:

      "parameters" : { "configuration" : { "failOnUnprocessableDocument" : false } }

U kunt ook doorwerken indexeren als er fouten optreden op elk gewenst moment wordt verwerkt, tijdens het parseren van blobs of tijdens het toevoegen van documenten naar een index. Als u wilt een bepaald aantal fouten negeren, stelt de `maxFailedItems` en `maxFailedItemsPerBatch` configuratieparameters voor de gewenste waarden. Bijvoorbeeld:

    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }

## <a name="incremental-indexing-and-deletion-detection"></a>Detectie van incrementele indexeren en verwijderen
Bij het instellen van een blob-indexeerfunctie worden uitgevoerd op een planning, het opnieuw indexeren van alleen de gewijzigde blobs, zoals wordt bepaald door de blob `LastModified` timestamp.

> [!NOTE]
> U hoeft niet te geven van een detectie wijzigingsbeleid – incrementele indexeren automatisch voor u is ingeschakeld.

Gebruik een benadering 'soft delete' ter ondersteuning van documenten verwijderen. Als u de blobs rechtstreekse verwijdert, worden overeenkomstige documenten uit de search-index niet worden verwijderd. Gebruik in plaats daarvan de volgende stappen:  

1. De eigenschap van een aangepaste metagegevens toevoegen aan de blob om aan te geven naar Azure Search logisch wordt verwijderd
2. Een beleid voor voorlopig verwijderen op de gegevensbron configureren
3. Wanneer de indexeerfunctie is verwerkt de blob (zoals weergegeven door de status van de indexeerfunctie API), kunt u fysiek de blob verwijderen

Bijvoorbeeld, het volgende beleid overweegt een blob worden verwijderd als er een eigenschap metadata `IsDeleted` met de waarde `true`:

    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",     
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }   

## <a name="indexing-large-datasets"></a>Indexeren van grote gegevenssets

Indexeren van blobs, kan een tijdrovend proces zijn. In gevallen waar u miljoenen blobs index hebt, kunt u sneller indexeren door het partitioneren van uw gegevens en het gebruik van meerdere indexeerfuncties om de gegevens parallel te verwerken. Hier ziet u hoe u kunt dit instellen:

- Partitioneren van uw gegevens in meerdere blob-containers of virtuele mappen
- Instellen van meerdere Azure Search gegevensbronnen, één per container of de map. Om te verwijzen naar een blob-map, gebruiken de `query` parameter:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Maak een bijbehorende indexeerfunctie voor elke gegevensbron. Alle Indexeerfuncties kunnen verwijzen naar hetzelfde doel-search-index.  

- Een zoekeenheid in uw service kan een indexeerfunctie worden uitgevoerd op elk moment. Maken van meerdere indexeerfuncties zoals hierboven beschreven, is alleen nuttig als ze daadwerkelijk parallel worden uitgevoerd. Als u wilt meerdere indexeerfuncties parallel worden uitgevoerd, worden uitgebreid uw search-service door het maken van een geschikt aantal partities en replica's. Bijvoorbeeld, als uw search-service heeft 6 search-eenheden (bijvoorbeeld 2 partities x 3 replica's), vervolgens 6 Indexeerfuncties kunnen tegelijkertijd worden uitgevoerd, wat resulteert in een six-fold toename in doorvoer voor indexering. Zie voor meer informatie over schaalbaarheid en capaciteitsplanning, [schalen niveaus van de bron voor query's en workloads in Azure Search indexeren](search-capacity-planning.md).

## <a name="indexing-documents-along-with-related-data"></a>Indexeren van documenten samen met verwante gegevens

U kunt "samenstellen' documenten uit meerdere bronnen in uw index. U wilt bijvoorbeeld tekst uit blobs samenvoegen met andere metagegevens die zijn opgeslagen in Cosmos-database. U kunt zelfs de API te indexeren samen met verschillende indexeerfuncties push gebruiken om op te bouwen documenten zoeken uit meerdere delen. 

Dit werkt, moeten alle Indexeerfuncties en andere onderdelen te stemmen op de documentsleutel. Zie voor een gedetailleerde procedure in dit artikel externe: [documenten worden gecombineerd met andere gegevens in Azure Search ](http://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

<a name="IndexingPlainText"></a>
## <a name="indexing-plain-text"></a>Indexering als tekst zonder opmaak 

Als alle BLOB's tekst zonder opmaak in dezelfde codering bevatten, kunt u indexering prestaties aanzienlijk verbeteren met behulp van **modus voor het parseren van tekst**. Met de modus voor het parseren van tekst, stelt de `parsingMode` configuratie-eigenschap voor `text`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }

Standaard de `UTF-8` codering wordt verondersteld. Een andere codering, gebruikt u de `encoding` configuratie-eigenschap: 

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }


<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>Eigenschappen voor typespecifieke metagegevens van inhoud
De volgende tabel geeft een overzicht van de verwerking wordt uitgevoerd voor elke documentindeling en beschrijft de eigenschappen van de metagegevens die door Azure Search hebt uitgepakt.

| Documentindeling / type inhoud | Eigenschappen van de specifieke metagegevens inhoudstype | Verwerkingsgegevens |
| --- | --- | --- |
| HTML-CODE (`text/html`) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |HTML-opmaak van strook / en haal de tekst |
| PDF-BESTAND (`application/pdf`) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Haal de tekst, inclusief ingesloten documenten (met uitzondering van installatiekopieën) |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Haal de tekst, inclusief ingesloten documenten |
| DOC (application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Haal de tekst, inclusief ingesloten documenten |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Haal de tekst, inclusief ingesloten documenten |
| XLS (toepassing/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Haal de tekst, inclusief ingesloten documenten |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Haal de tekst, inclusief ingesloten documenten |
| PPT (vnd.ms-toepassing-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Haal de tekst, inclusief ingesloten documenten |
| Bericht (vnd.ms-toepassing-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Haal de tekst, met inbegrip van bijlagen |
| ZIP (toepassing/zip) |`metadata_content_type` |Haal de tekst uit alle documenten in het archief |
| XML (application/xml) |`metadata_content_type`</br>`metadata_content_encoding`</br> |XML-opmaak van strook / en haal de tekst |
| JSON (application/json) |`metadata_content_type`</br>`metadata_content_encoding` |Haal de tekst<br/>Opmerking: Als u meerdere documentvelden extraheren uit een JSON-blob wilt, Zie [indexeren JSON-blobs](search-howto-index-json-blobs.md) voor meer informatie |
| EML (bericht/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Haal de tekst, met inbegrip van bijlagen |
| RTF-indeling (toepassing/rtf) |`metadata_content_type`</br>`metadata_author`</br>`metadata_character_count`</br>`metadata_creation_date`</br>`metadata_page_count`</br>`metadata_word_count`</br> | Haal de tekst|
| Tekst zonder opmaak (text/plain) |`metadata_content_type`</br>`metadata_content_encoding`</br> | Haal de tekst|


## <a name="help-us-make-azure-search-better"></a>Help ons Azure Search te verbeteren
Als u functieaanvragen of suggesties voor verbeteringen hebt, laat ons weten op onze [UserVoice site](https://feedback.azure.com/forums/263029-azure-search/).
