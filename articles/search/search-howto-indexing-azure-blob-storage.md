---
title: Azure Blob storage-inhoud voor zoeken in volledige tekst - Azure Search-index
description: Leer hoe u Azure Blob Storage indexeren en haal de tekst van documenten met Azure Search.
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: c73a802cd67c9ecb94482cfcd6aac51fc8bbc19e
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53317471"
---
# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Documenten in Azure Blob-opslag met Azure Search indexeren
Dit artikel wordt beschreven hoe u met Azure Search index documenten (zoals PDF-bestanden en Microsoft Office-documenten en enkele andere algemene indelingen) die zijn opgeslagen in Azure Blob-opslag. Eerst wordt de basisbeginselen van het instellen en configureren van een blob-indexeerfunctie uitgelegd. Vervolgens, biedt een diepergaand onderzoek van problemen en scenario's kunt u waarschijnlijk optreden.

## <a name="supported-document-formats"></a>Ondersteunde documentindelingen voor
De blob-indexeerfunctie kunt tekst extraheren uit de opmaak van de volgende documenten:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="setting-up-blob-indexing"></a>Instellen van blob-indexering
U kunt instellen dat een Azure Blob Storage-indexeerfunctie gebruiken:

* [Azure Portal](https://ms.portal.azure.com)
* Azure Search [REST-API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Search [.NET SDK](https://aka.ms/search-sdk)

> [!NOTE]
> Sommige functies (bijvoorbeeld veldtoewijzingen) zijn nog niet beschikbaar in de portal, en moeten worden gebruikt via een programma.
>
>

Hier ziet de stroom met behulp van de REST-API.

### <a name="step-1-create-a-data-source"></a>Stap 1: Een gegevensbron maken
Een gegevensbron bevat gegevens die u wilt indexeren, referenties die nodig zijn voor toegang tot de gegevens en beleidsregels efficiënt om wijzigingen te bepalen in de gegevens (nieuwe, gewijzigde of verwijderde rijen). Een gegevensbron kan worden gebruikt door meerdere indexeerfuncties in de dezelfde search-service.

Voor blob-indexering, moet de gegevensbron de volgende vereiste eigenschappen hebben:

* **naam** is de unieke naam van de gegevensbron in uw search-service.
* **type** moet `azureblob`.
* **referenties** biedt de verbindingsreeks van de storage-account als de `credentials.connectionString` parameter. Zie [opgeven van referenties](#Credentials) hieronder voor meer informatie.
* **container** Hiermee geeft u een container in uw storage-account. Standaard zijn alle blobs in de container worden opgehaald. Als u wilt dat alleen index blobs in een bepaalde virtuele map, kunt u die map met de optionele **query** parameter.

Een gegevensbron maken:

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Zie voor meer informatie over de gegevensbron maken API's [maken Datasource](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="how-to-specify-credentials"></a>Hoe u referenties opgeven ####

U kunt de referenties opgeven voor de blob-container in een van de volgende manieren:

- **Tekenreeks opslagaccountverbinding volledige toegang tot de**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`. U kunt de verbindingsreeks ophalen uit de Azure-portal door te navigeren naar de blade opslagaccount > Instellingen > sleutels (voor klassieke opslagaccounts) of instellingen > toegangssleutels (voor opslagaccounts van Azure Resource Manager).
- **Storage-account gedeelde-toegangshandtekening** (SAS)-verbindingsreeks: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl` De SAS moet de lijst en lezen-machtigingen voor containers en objecten (in dit geval blobs).
-  **Shared access signature voor containers**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl` De SAS moet de lijst en lezen-machtigingen voor de container.

Voor meer informatie over storage gedeelde toegangshandtekeningen, Zie [handtekeningen voor gedeelde toegang met behulp van](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Als u SAS-referenties gebruikt, moet u de referenties voor gegevensbron regelmatig bijgewerkt met de vernieuwde handtekeningen om te voorkomen dat ze zijn verlopen. Als de referenties van de SAS is verlopen, wordt de indexeerfunctie mislukken met een foutbericht weergegeven die vergelijkbaar is met `Credentials provided in the connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>Stap 2: Een index maken
De index Hiermee geeft u de velden in een document, kenmerken, en andere constructies die de vorm van de zoekopdracht ervaring.

Hier wordt beschreven hoe u een index maken met een doorzoekbare `content` veld voor het opslaan van de tekst die is geëxtraheerd uit blobs:   

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
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

### <a name="step-3-create-an-indexer"></a>Stap 3: Een indexeerfunctie maken
Een indexeerfunctie verbindt een gegevensbron met een doelzoekindex en biedt een schema voor het automatiseren van het vernieuwen van gegevens.

Zodra de index en gegevensbron zijn gemaakt, bent u klaar om te maken van de indexeerfunctie:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Deze indexeerfunctie wordt elke twee uur (schema-interval is ingesteld op 'PT2H') worden uitgevoerd. Als u wilt een indexeerfunctie om de 30 minuten uitvoeren, moet u het interval aan 'PT30M' instellen. Het kortste ondersteunde interval is 5 minuten. Het schema is optioneel: als u dit weglaat, een indexeerfunctie wordt uitgevoerd slechts eenmaal wanneer deze gemaakt. U kunt echter een indexeerfunctie op aanvraag uitvoeren op elk gewenst moment.   

Bekijk voor meer informatie over de indexeerfunctie maken API [indexeerfunctie maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="how-azure-search-indexes-blobs"></a>Hoe blobs worden geïndexeerd in Azure Search

Afhankelijk van de [configuratie van de indexeerfunctie](#PartsOfBlobToIndex), de blob-indexeerfunctie kunt u alleen de opslagmetagegevens indexeren (dit is nuttig wanneer u alleen de metagegevens van de interessante en hoeft niet te indexeren van de inhoud van blobs), opslag en metagegevens, of beide metagegevens en tekstuele inhoud. Standaard haalt de indexeerfunctie zowel de metagegevens als de inhoud.

> [!NOTE]
> Standaard worden de blobs met gestructureerde inhoud, zoals JSON- of CSV geïndexeerd als één segment van de tekst. Als u wilt om te indexeren van JSON en CSV-blobs in een gestructureerd, Zie [indexeren van JSON-blobs](search-howto-index-json-blobs.md) en [indexeren van CSV-blobs](search-howto-index-csv-blobs.md) preview-functies.
>
> Een document samengestelde of ingesloten (zoals een ZIP-archief of een Word-document met ingesloten Outlook e-mail met bijlagen) worden ook geïndexeerd als één document.

* De tekstinhoud van het document wordt geëxtraheerd in een tekenreeksveld met de naam `content`.

> [!NOTE]
> Azure Search beperkt hoeveel tekst worden uitgepakt, afhankelijk van de prijscategorie: 32.000 tekens gratis laag, 64.000 voor Basic en 4 miljoen voor de lagen Standard-, Standard S2- en Standard S3. Een waarschuwing is opgenomen in het antwoord van de status van indexeerfunctie voor afgekapte documenten.  

* Door gebruiker opgegeven metagegevenseigenschappen aanwezig zijn op de blob, worden indien van toepassing, geëxtraheerd bewoordingen.
* Eigenschappen van de standaard blob-metagegevens zijn uitgepakt in de volgende velden:

  * **metagegevens\_opslag\_naam** (Edm.String) - de naam van de blob. Bijvoorbeeld, hebt u een blob-/my-container/my-folder/subfolder/resume.pdf, de waarde van dit veld is `resume.pdf`.
  * **metagegevens\_opslag\_pad** (Edm.String) - de volledige URI van de blob, met inbegrip van het storage-account. Bijvoorbeeld: `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`
  * **metagegevens\_opslag\_inhoud\_type** (Edm.String) - inhoudstype zoals opgegeven door de code die u hebt gebruikt voor het uploaden van de blob. Bijvoorbeeld `application/octet-stream`.
  * **metagegevens\_opslag\_laatste\_gewijzigd** (Edm.DateTimeOffset) - tijdstempel voor de blob voor het laatst gewijzigd. Azure Search gebruikt deze timestamp om te identificeren van gewijzigde blobs, om te voorkomen dat alles indexeren na de eerste indexering.
  * **metagegevens\_opslag\_grootte** (Edm.Int64) - blob-grootte in bytes.
  * **metagegevens\_opslag\_inhoud\_md5** (Edm.String) - MD5-hash van de inhoud van de blob, indien beschikbaar.
* Eigenschappen van metagegevens specifiek zijn voor elk documentindeling zijn uitgepakt in de velden die worden vermeld [hier](#ContentSpecificMetadata).

U hoeft niet te definiëren van velden voor alle van de bovenstaande eigenschappen in uw search-index: alleen vastleggen van de eigenschappen die u nodig hebt voor uw toepassing.

> [!NOTE]
> De veldnamen in uw bestaande index is vaak het geval is, anders dan de veldnamen die zijn gegenereerd tijdens de extractie van document. U kunt **veldtoewijzingen** om toe te wijzen de namen van eigenschappen die door Azure Search de veldnamen in uw search-index. Hier ziet u een voorbeeld van het veld toewijzingen hieronder gebruiken.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Document-sleutels en veldtoewijzingen definiëren
In Azure Search identificatie de documentsleutel unieke van een document. Elke search-index moet exact één sleutelveld van het type Edm.String. Het veld met de sleutel is vereist voor elk document dat wordt toegevoegd aan de index (dit is eigenlijk de enige vereiste veld).  

U moet zorgvuldig overwegen welke uitgepakte veld moet worden toegewezen aan een veld met de sleutel voor de index. De kandidaten zijn:

* **metagegevens\_opslag\_naam** : dit is mogelijk een geschikte kandidaat, maar houd er rekening mee dat (1) de namen mogelijk niet uniek is, als er blobs met dezelfde naam in verschillende mappen, en (2) de naam tekens kan bevatten ongeldig document-sleutels, zoals streepjes bevatten. U kunt omgaan met ongeldige tekens met behulp van de `base64Encode` [veld toewijzing functie](search-indexer-field-mappings.md#base64EncodeFunction) : als u dit doet, houd er rekening mee document sleutels coderen wanneer deze worden doorgegeven in de API-, zoals opzoeken aanroepen. (Bijvoorbeeld in .NET kunt u de [UrlTokenEncode methode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) voor dat doel).
* **metagegevens\_opslag\_pad** : het volledige pad, zorgt u ervoor uniekheid, maar het pad bevat zeker `/` tekens [ongeldig in een documentsleutel](https://docs.microsoft.com/rest/api/searchservice/naming-rules).  Zoals hierboven, hebt u de optie voor het coderen van de sleutels met behulp van de `base64Encode` [functie](search-indexer-field-mappings.md#base64EncodeFunction).
* Als geen van de bovenstaande opties voor u werkt, kunt u een aangepaste metagegevenseigenschap toevoegen aan de blobs. Deze optie is, maar het proces voor het uploaden van blob die metagegevenseigenschap toevoegen aan alle blobs vereist. Omdat de sleutel een vereiste eigenschap is, mislukken alle blobs waarvoor deze eigenschap niet te indexeren.

> [!IMPORTANT]
> Als er geen expliciete toewijzing voor een veld met de sleutel in de index is, gebruikt Azure Search automatisch `metadata_storage_path` als de sleutel en het base-64 gecodeerd sleutelwaarden (de tweede optie hierboven).
>
>

In dit voorbeeld gaan we kiezen de `metadata_storage_name` veld als de documentsleutel. We gaan ook wordt ervan uitgegaan dat de index is een sleutelveld met de naam `key` en een veld `fileSize` voor het opslaan van de grootte van het document. Om te dingen die u naar wens wire, geeft u de volgende veldtoewijzingen bij het maken of bijwerken van de indexeerfunctie:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Om dit allemaal bij elkaar, dit is het toevoegen van veldtoewijzingen en Base64-codering van sleutels voor een bestaande indexeerfunctie:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2017-11-11
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
> Zie voor meer informatie over veldtoewijzingen [in dit artikel](search-indexer-field-mappings.md).
>
>

<a name="WhichBlobsAreIndexed"></a>
## <a name="controlling-which-blobs-are-indexed"></a>Beheren welke blobs worden geïndexeerd.
U kunt bepalen welke blobs worden geïndexeerd en die zijn overgeslagen.

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Alleen de blobs met specifieke extensies indexeren
U kunt alleen de blobs met de bestandsnaamextensies die u met behulp van opgeeft de index de `indexedFileNameExtensions` configuratieparameter indexeerfunctie. De waarde is een tekenreeks met een door komma's gescheiden lijst met bestandsextensies (met een toonaangevende punt). Bijvoorbeeld, voor index alleen de. PDF-bestand en. Blobs, DOCX, doen dit:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions"></a>Blobs met bepaalde bestandsextensies uitsluiten
U kunt blobs met specifieke bestandsnaamextensies uitsluiten van het indexeren met behulp van de `excludedFileNameExtensions` configuratieparameter. De waarde is een tekenreeks met een door komma's gescheiden lijst met bestandsextensies (met een toonaangevende punt). Bijvoorbeeld, voor de index alle blobs met uitzondering van die met de. PNG en. JPEG-extensies, doen dit:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Als beide `indexedFileNameExtensions` en `excludedFileNameExtensions` parameters aanwezig zijn, Azure Search eerst gekeken `indexedFileNameExtensions`, klikt u vervolgens op `excludedFileNameExtensions`. Dit betekent dat als dezelfde bestandsextensie in beide lijsten aanwezig is, het, worden uitgesloten van indexeren.

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-the-blob-are-indexed"></a>Bepalen welke onderdelen van de blob worden geïndexeerd.

U kunt bepalen welke onderdelen van de blobs worden geïndexeerd met behulp van de `dataToExtract` configuratieparameter. Het kan duren voordat de volgende waarden:

* `storageMetadata` -Hiermee geeft u op dat alleen de [standaard blob-eigenschappen en metagegevens door gebruiker opgegeven](../storage/blobs/storage-properties-metadata.md) worden geïndexeerd.
* `allMetadata` -Hiermee geeft u de opslagmetagegevens van deze en de [inhoudstype specifieke metagegevens](#ContentSpecificMetadata) geëxtraheerd uit de blob-inhoud zijn geïndexeerd.
* `contentAndMetadata` -Hiermee geeft u aan dat alle metagegevens en tekstuele inhoud opgehaald uit de blob worden geïndexeerd. Dit is de standaardwaarde.

Bijvoorbeeld: als u alleen de opslagmetagegevens indexeren, wilt gebruiken:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>Met behulp van de metagegevens van de blob om te bepalen hoe blobs worden geïndexeerd

De configuratieparameters die hierboven worden beschreven van toepassing op alle blobs. Soms, kunt u om te bepalen hoe *afzonderlijke blobs* worden geïndexeerd. U kunt dit doen door de volgende eigenschappen van de blob-metagegevens en de waarden toe te voegen:

| Naam van eigenschap | Waarde van eigenschap | Uitleg |
| --- | --- | --- |
| AzureSearch_Skip |"true" |Hiermee geeft u de blob-indexeerfunctie volledig kunt overslaan van de blob. Extractie van metagegevens noch inhoud wordt uitgevoerd. Dit is handig wanneer een bepaalde blob herhaaldelijk mislukt en onderbroken door het indexeringsproces worden geautomatiseerd. |
| AzureSearch_SkipContent |"true" |Dit is het equivalent van `"dataToExtract" : "allMetadata"` instellen beschreven [hierboven](#PartsOfBlobToIndex) binnen het bereik van een specifieke blob. |

<a name="DealingWithErrors"></a>
## <a name="dealing-with-errors"></a>Omgaan met fouten

Standaard stopt de blob-indexeerfunctie zodra er een blob met een niet-ondersteund inhoudstype (bijvoorbeeld een afbeelding). U kunt natuurlijk de `excludedFileNameExtensions` parameter om over te slaan, bepaalde inhoudstypen. Maar mogelijk moet u index blobs zonder dat alle mogelijke inhoudstypen vooraf bekend. Instellen als u wilt doorgaan met het indexeren als een niet-ondersteund inhoudstype is aangetroffen, de `failOnUnsupportedContentType` configuratieparameter op `false`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }

Voor sommige blobs, Azure Search kan niet bepalen van het type inhoud is of niet kan verwerken van een document van anders ondersteund inhoudstype. Instellen als u wilt deze foutmodus negeren, de `failOnUnprocessableDocument` configuratieparameter op false:

      "parameters" : { "configuration" : { "failOnUnprocessableDocument" : false } }

Azure Search beperkt de grootte van blobs die zijn geïndexeerd. Deze limieten zijn gedocumenteerd in [Servicelimieten in Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity). Grote blobs worden behandeld als fouten standaard. Echter, u kunt nog steeds de opslagmetagegevens indexeren van grote blobs als u instelt `indexStorageMetadataOnlyForOversizedDocuments` configuratieparameter op ' True ': 

    "parameters" : { "configuration" : { "indexStorageMetadataOnlyForOversizedDocuments" : true } }

U kunt ook blijven indexeren als er fouten optreden op elk gewenst moment van de verwerking, tijdens het parseren van blobs of tijdens het toevoegen van documenten naar een index. Voor een bepaald aantal fouten negeren, stel de `maxFailedItems` en `maxFailedItemsPerBatch` parameters voor de configuratie op de gewenste waarden. Bijvoorbeeld:

    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }

## <a name="incremental-indexing-and-deletion-detection"></a>Detectie van incrementele indexeren en verwijderen
Bij het instellen van een blob-indexeerfunctie om uit te voeren volgens een schema wordt alleen de gewijzigde blobs, zoals wordt bepaald door een van de blob reindexes `LastModified` timestamp.

> [!NOTE]
> U hoeft niet te geven van een detectiebeleid voor wijzigingen – incrementele indexeren automatisch voor u is ingeschakeld.

Ter ondersteuning van verwijderen van documenten, een 'voorlopig verwijderen'-aanpak te gebruiken. Als u de blobs aanschaffen verwijdert, wordt overeenkomende documenten niet worden verwijderd uit de search-index. Gebruik in plaats daarvan de volgende stappen uit:  

1. Een aangepaste metagegevenseigenschap toevoegen aan de blob om aan te geven naar Azure Search logisch wordt verwijderd
2. Een beleid voor voorlopig verwijderen detectie op de gegevensbron configureren
3. Wanneer de indexeerfunctie is verwerkt de blob (zoals weergegeven door de status van de indexeerfunctie API), kunt u fysiek de blob verwijderen

Bijvoorbeeld, het volgende beleid rekening gehouden met een blob moet worden verwijderd als er een eigenschap metadata `IsDeleted` met de waarde `true`:

    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2017-11-11
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

Indexeren van blobs, kan een tijdrovend proces zijn. In gevallen waarin u werkt met miljoenen blobs indexeren, kunt u sneller indexering door uw gegevens partitioneren en meerdere indexeerfuncties gebruiken om de gegevens parallel te verwerken. Hier ziet hoe u kunt dit instellen:

- Uw gegevens partitioneren in meerdere blob-containers of virtuele mappen
- Instellen van meerdere Azure Search-gegevensbronnen, één per container of de map. Als u wilt verwijzen naar een blob-map, gebruikt u de `query` parameter:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Maak een bijbehorende indexeerfunctie voor elke gegevensbron. Alle Indexeerfuncties kunnen verwijzen naar de hetzelfde doel search-index.  

- Een zoekeenheid in uw service kan een indexeerfunctie uitvoeren op een bepaald moment. Het maken van meerdere indexeerfuncties zoals hierboven beschreven, is alleen nuttig als ze daadwerkelijk parallel worden uitgevoerd. Als u wilt meerdere indexeerfuncties parallel worden uitgevoerd, de schaal vergroten uw search-service door het maken van een geschikt aantal partities en replica's. Bijvoorbeeld, als uw search-service heeft 6 search-eenheden (bijvoorbeeld 2 partities x 3 replica's), vervolgens 6 Indexeerfuncties kunnen tegelijkertijd worden uitgevoerd, wat resulteert in een six-fold toename van de doorvoer voor indexering. Zie voor meer informatie over het schalen en plannen van capaciteit, [schalen resource niveaus voor query's en workloads in Azure Search indexeren](search-capacity-planning.md).

## <a name="indexing-documents-along-with-related-data"></a>Documenten samen met verwante gegevens indexeren

U wilt 'samenstellen' documenten uit meerdere bronnen in uw index. U wilt bijvoorbeeld tekst van blobs samenvoegen met andere metagegevens die zijn opgeslagen in Cosmos DB. U kunt zelfs de push API indexeren, samen met verschillende indexeerfuncties gebruiken om op te bouwen documenten zoeken uit meerdere delen. 

Om dit te werken, moeten alle Indexeerfuncties en andere onderdelen te stemmen op de documentsleutel. Zie voor een gedetailleerd overzicht dit externe artikel: [Documenten worden gecombineerd met andere gegevens in Azure Search ](http://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

<a name="IndexingPlainText"></a>
## <a name="indexing-plain-text"></a>Indexering als tekst zonder opmaak 

Als alle blobs in uw tekst zonder opmaak in dezelfde codering bevatten, kunt u indexering prestaties aanzienlijk verbeteren met behulp van **modus voor het parseren van tekst**. Voor het gebruik van modus voor het parseren van tekst, stel de `parsingMode` configuratie-eigenschap op `text`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }

Standaard de `UTF-8` encoding wordt uitgegaan. Als u wilt een andere codering opgeven, gebruikt u de `encoding` configuratie-eigenschap: 

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }


<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>Eigenschappen voor typespecifieke metagegevens van inhoud
De volgende tabel geeft een overzicht van de verwerking die worden uitgevoerd voor elk documentindeling en beschrijft de eigenschappen van metagegevens geëxtraheerd met Azure Search.

| Documentindeling / type inhoud | Eigenschappen van inhoudstype specifieke metagegevens | Details verwerken |
| --- | --- | --- |
| HTML-CODE (`text/html`) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |HTML-opmaak van strook / en haal de tekst |
| PDF (`application/pdf`) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Haal de tekst, met inbegrip van ingesloten documenten (met uitzondering van installatiekopieën) |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Haal de tekst, met inbegrip van ingesloten documenten |
| Doc-bestand (toepassing/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Haal de tekst, met inbegrip van ingesloten documenten |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Haal de tekst, met inbegrip van ingesloten documenten |
| XLS (toepassing/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Haal de tekst, met inbegrip van ingesloten documenten |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Haal de tekst, met inbegrip van ingesloten documenten |
| PPT (toepassing/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Haal de tekst, met inbegrip van ingesloten documenten |
| MSG (toepassing/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Haal de tekst, met inbegrip van bijlagen |
| ZIP (toepassing/zip) |`metadata_content_type` |Haal de tekst uit alle documenten in het archief |
| XML (application/xml) |`metadata_content_type`</br>`metadata_content_encoding`</br> |XML-opmaak van strook / en haal de tekst |
| JSON (application/json) |`metadata_content_type`</br>`metadata_content_encoding` |Tekst extraheren<br/>OPMERKING: Als u nodig hebt om op te halen van meerdere documentvelden van een JSON-blob, Zie [indexeren van JSON-blobs](search-howto-index-json-blobs.md) voor meer informatie |
| EML (bericht/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Haal de tekst, met inbegrip van bijlagen |
| RTF (toepassing/rtf) |`metadata_content_type`</br>`metadata_author`</br>`metadata_character_count`</br>`metadata_creation_date`</br>`metadata_page_count`</br>`metadata_word_count`</br> | Tekst extraheren|
| Tekst zonder opmaak (text/plain) |`metadata_content_type`</br>`metadata_content_encoding`</br> | Tekst extraheren|


## <a name="help-us-make-azure-search-better"></a>Help ons Azure Search verbeteren
Als u functieverzoeken heeft of suggesties voor verbeteringen hebt, laat het ons weten op onze [UserVoice-site](https://feedback.azure.com/forums/263029-azure-search/).
