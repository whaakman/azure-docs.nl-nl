---
title: Inhoud van de index van Azure Table storage voor zoeken in volledige tekst - Azure Search
description: Leer hoe u gegevens die zijn opgeslagen in Azure Table storage met een indexeerfunctie Azure Search-index.
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 9532f4331b4dbbc8d687778b29f63179cca6b4d4
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53632956"
---
# <a name="index-azure-table-storage-with-azure-search"></a>Azure Table storage met Azure Search-index
In dit artikel laat zien hoe Azure Search gebruiken om gegevens te indexeren die zijn opgeslagen in Azure Table storage.

## <a name="set-up-azure-table-storage-indexing"></a>Instellen van Azure Table storage indexeren

U kunt een indexeerfunctie voor Azure Table storage instellen met behulp van deze resources:

* [Azure Portal](https://ms.portal.azure.com)
* Azure Search [REST-API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Search [.NET SDK](https://aka.ms/search-sdk)

Hier ziet de stroom met behulp van de REST-API. 

### <a name="step-1-create-a-datasource"></a>Stap 1: Een gegevensbron maken

Een gegevensbron bevat gegevens die u wilt indexeren, de referenties die nodig zijn voor toegang tot de gegevens en de beleidsregels die in Azure kunnen zoeken efficiënt om wijzigingen te bepalen in de gegevens.

Voor tabel indexeren, moet de gegevensbron de volgende eigenschappen hebben:

- **naam** is de unieke naam van de gegevensbron in uw search-service.
- **type** moet `azuretable`.
- **referenties** parameter bevat de verbindingsreeks van de storage-account. Zie de [opgeven van referenties](#Credentials) sectie voor meer informatie.
- **container** Hiermee stelt u de tabelnaam van de en een optionele-query.
    - Geef de naam van de tabel met behulp van de `name` parameter.
    - (Optioneel) Geef een query met behulp van de `query` parameter. 

> [!IMPORTANT] 
> Gebruik indien mogelijk een filter voor PartitionKey voor betere prestaties. Elke andere query biedt een volledige tabelcontrole uitgevoerd, wat resulteert in slechte prestaties voor grote tabellen. Zie de [Prestatieoverwegingen](#Performance) sectie.


Een gegevensbron maken:

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Zie voor meer informatie over de gegevensbron maken API [maken Datasource](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="ways-to-specify-credentials"></a>Manieren om op te geven van referenties ####

U kunt de referenties opgeven voor de tabel in een van de volgende manieren: 

- **Tekenreeks opslagaccountverbinding volledige toegang tot de**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` U kunt de verbindingsreeks ophalen uit de Azure-portal door te gaan naar de **blade Opslagaccount** > **instellingen** > **sleutels** (voor klassiek Storage-accounts) of **instellingen** > **toegangssleutels** (voor opslagaccounts van Azure Resource Manager).
- **Storage-account shared access signature-verbindingsreeks**: `TableEndpoint=https://<your account>.table.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=t&sp=rl` De shared access signature moet de lijst en leesrechten op containers (tabellen in dit geval) en -objecten (rijen).
-  **Tabel gedeelde-toegangshandtekening**: `ContainerSharedAccessUri=https://<your storage account>.table.core.windows.net/<table name>?tn=<table name>&sv=2016-05-31&sig=<the signature>&se=<the validity end time>&sp=r` De shared access signature moet query (lezen) machtigingen hebben voor de tabel.

Voor meer informatie over storage gedeelde toegangshandtekeningen, Zie [voor gedeelde toegangshandtekeningen](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Als u referenties van handtekening voor gedeelde toegang, moet u de referenties voor gegevensbron regelmatig bijgewerkt met de vernieuwde handtekeningen om te voorkomen dat ze zijn verlopen. Als de referenties van handtekening voor gedeelde toegang is verlopen, de indexeerfunctie is mislukt met een foutbericht weergegeven die vergelijkbaar is met "Referenties die zijn opgegeven in de connection string zijn ongeldig of verlopen."  

### <a name="step-2-create-an-index"></a>Stap 2: Een index maken
De index Hiermee geeft u de velden in een document, de kenmerken en andere constructies die de vorm van de zoekopdracht ervaring.

Een index te maken:

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
          ]
    }

Zie voor meer informatie over het maken van indexen [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index).

### <a name="step-3-create-an-indexer"></a>Stap 3: Een indexeerfunctie maken
Een indexeerfunctie verbindt een gegevensbron met een doelzoekindex en biedt een schema voor het automatiseren van het vernieuwen van gegevens. 

Nadat de index en gegevensbron zijn gemaakt, bent u klaar om te maken van de indexeerfunctie:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Deze indexeerfunctie wordt elke twee uur uitgevoerd. (De schema-interval is ingesteld op 'PT2H'.) Als u wilt een indexeerfunctie om de 30 minuten uitvoeren, moet u het interval aan 'PT30M' instellen. Het kortste ondersteunde interval is vijf minuten. Het schema is optioneel. Als u dit weglaat, wordt er een indexeerfunctie uitgevoerd slechts eenmaal wanneer deze gemaakt. U kunt echter een indexeerfunctie uitvoeren op aanvraag op elk gewenst moment.   

Zie voor meer informatie over de indexeerfunctie maken API [indexeerfunctie maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="deal-with-different-field-names"></a>Omgaan met verschillende veldnamen
De namen van de velden in uw bestaande index zijn soms verschillen van de namen van eigenschappen in de tabel. Veldtoewijzingen kunt u de namen van eigenschappen uit de tabel aan de veldnamen worden toegewezen in uw search-index. Zie voor meer informatie over veldtoewijzingen [veldtoewijzingen voor Azure Search-indexeerfunctie overbruggen de verschillen tussen gegevensbronnen en zoekindexen](search-indexer-field-mappings.md).

## <a name="handle-document-keys"></a>Document-sleutels worden verwerkt
In Azure Search identificatie de documentsleutel unieke van een document. Elke search-index moet exact één sleutelveld van het type `Edm.String`. Het veld met de sleutel is vereist voor elk document dat wordt toegevoegd aan de index. (In feite, dit is de enige vereiste veld.)

Aangezien rijen een samengestelde sleutel hebben, een synthetische veld met de naam wordt gegenereerd door Azure Search `Key` die een samenvoeging van de partitie en recordsleutelwaarden sleutelwaarden is. Bijvoorbeeld, als een rij van het PartitionKey is `PK1` en RowKey `RK1`, dan zal de `Key` is de waarde van veld `PK1RK1`.

> [!NOTE]
> De `Key` waarde mag de tekens die ongeldig in document sleutels, zoals streepjes zijn bevatten. U kunt omgaan met ongeldige tekens met behulp van de `base64Encode` [veld toewijzing functie](search-indexer-field-mappings.md#base64EncodeFunction). Als u dit doet, vergeet dan niet te gebruiken URL-safe Base64-codering bij het doorgeven van een document sleutels in API-aanroepen, zoals opzoeken.
>
>

## <a name="incremental-indexing-and-deletion-detection"></a>Detectie van incrementele indexeren en verwijderen
Bij het instellen van een tabelindexeerfunctie uit te voeren op een planning, het alleen nieuwe of bijgewerkte rijen, zoals wordt bepaald door een rij reindexes `Timestamp` waarde. U hoeft niet te geven van een detectiebeleid voor wijzigingen. Incrementele indexeren is automatisch ingeschakeld.

Om aan te geven dat bepaalde documenten uit de index moeten worden verwijderd, kunt u een strategie voor voorlopig verwijderen. In plaats van een rij te verwijderen, voegt u de eigenschap om aan te geven dat deze is verwijderd en een beleid voor detectie van voorlopig verwijderen op de gegevensbron instellen. Bijvoorbeeld, het volgende beleid van oordeel is dat een rij worden verwijderd als de rij een eigenschap heeft `IsDeleted` met de waarde `"true"`:

    PUT https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "<query>" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   

<a name="Performance"></a>
## <a name="performance-considerations"></a>Prestatieoverwegingen

Azure Search gebruikt standaard de volgende query-filter: `Timestamp >= HighWaterMarkValue`. Omdat Azure-tabellen hoeft een secundaire index op de `Timestamp` veld, dit type query vereist een volledige tabelcontrole uitgevoerd en is daarom langzaam is voor grote tabellen.


Hier volgen twee mogelijke methoden voor het verbeteren van de tabel indexering prestaties. Beide van deze methoden zijn afhankelijk van het gebruik van Tabelpartities: 

- Als uw gegevens kunnen in verschillende bereiken van de partitie op een natuurlijke manier worden gepartitioneerd, maakt u een gegevensbron en een bijbehorende indexeerfunctie voor het partitiebereik van elke. Elke indexeerfunctie heeft nu alleen een specifieke partitiebereik, wat resulteert in betere prestaties van query's verwerken. Als de gegevens die moeten worden geïndexeerd een klein aantal vaste partities, nog beter heeft: de indexeerfunctie wordt alleen een scan partitie. Bijvoorbeeld, om te maken van een gegevensbron voor het verwerken van een partitiebereik met sleutels uit `000` naar `100`, gebruikt u een query als volgt: 
    ```
    "container" : { "name" : "my-table", "query" : "PartitionKey ge '000' and PartitionKey lt '100' " }
    ```

- Als uw gegevens is gepartitioneerd op basis van tijd (bijvoorbeeld, u geen nieuwe partitie maken elke dag of week), houd rekening met de volgende benadering: 
    - Gebruik van een query van het formulier: `(PartitionKey ge <TimeStamp>) and (other filters)`. 
    - Indexeerfunctie voortgang controleren met behulp van [ophalen indexeerfunctie Status API](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status), en periodiek bijwerken de `<TimeStamp>` voorwaarde van de query op basis van de meest recente geslaagde hoge watermerk waarde. 
    - Als u nodig hebt voor het activeren van een volledige indexeren met deze methode moet u de query datasource naast het opnieuw instellen van de indexeerfunctie opnieuw instellen. 


## <a name="help-us-make-azure-search-better"></a>Help ons Azure Search verbeteren
Als u functieverzoeken heeft of suggesties voor verbeteringen, dient u ze op onze [UserVoice-site](https://feedback.azure.com/forums/263029-azure-search/).
