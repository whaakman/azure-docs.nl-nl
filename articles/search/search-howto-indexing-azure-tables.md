---
title: Azure Table storage met Azure Search indexeren | Microsoft Docs
description: Meer informatie over het opgeslagen gegevens in Azure Table storage met Azure Search-index
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 1cc27411-d0cc-40ed-8aed-c7cb9ab402b9
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/10/2017
ms.author: eugenesh
ms.openlocfilehash: b167f69f853f6ecdfd56179e6ffb946cdf2f45b8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="index-azure-table-storage-with-azure-search"></a>Azure Table storage met Azure Search-index
Dit artikel laat zien hoe u met Azure Search indexgegevens die zijn opgeslagen in Azure Table storage.

## <a name="set-up-azure-table-storage-indexing"></a>Instellen van Azure Table storage indexeren

U kunt een indexeerfunctie Azure Table-opslag instellen met behulp van deze bronnen:

* [Azure Portal](https://ms.portal.azure.com)
* Azure Search [REST-API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Search [.NET SDK](https://aka.ms/search-sdk)

Hier ziet de stroom met behulp van de REST-API. 

### <a name="step-1-create-a-datasource"></a>Stap 1: Een gegevensbron maken

Een gegevensbron geeft aan welke gegevens worden indexeren, de referenties die nodig zijn voor toegang tot de gegevens en de beleidsregels waarmee Azure Search, wijzigingen in de gegevens efficiënt te identificeren.

Voor tabel indexeren, moet de gegevensbron op de volgende eigenschappen hebben:

- **naam** is de unieke naam van de datasource die binnen uw search-service.
- **type** moet `azuretable`.
- **referenties** parameter bevat de verbindingsreeks voor opslag-account. Zie de [referenties opgeven](#Credentials) sectie voor meer informatie.
- **container** stelt de tabelnaam en een optionele query.
    - Geef de naam van de tabel met behulp van de `name` parameter.
    - Geef eventueel een query met behulp van de `query` parameter. 

> [!IMPORTANT] 
> Gebruik indien mogelijk een filter op PartitionKey voor betere prestaties. Een andere query biedt een volledige tabelscan leiden tot slechte prestaties voor grote tabellen. Zie de [Prestatieoverwegingen](#Performance) sectie.


Een gegevensbron maken:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Zie voor meer informatie over de API-gegevensbron maken [Datasource maken](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="ways-to-specify-credentials"></a>Manieren om referenties opgeven ####

U kunt de referenties opgeven voor de tabel in een van de volgende manieren: 

- **Volledige toegang account opslagverbindingsreeks**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` de verbindingsreeks kunt u krijgen via de Azure-portal door te gaan naar de **blade Opslagaccount** > **instellingen**  >  **Sleutels** (voor klassieke opslagaccounts) of **instellingen** > **toegangssleutels** (voor opslag van Azure Resource Manager accounts).
- **Storage-account shared access signature-verbindingsreeks**: `TableEndpoint=https://<your account>.table.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=t&sp=rl` de shared access signature moet de lijst en leesrechten op (tabellen in dit geval) containers en objecten (tabelrijen).
-  **Shared access signature voor tabel**: `ContainerSharedAccessUri=https://<your storage account>.table.core.windows.net/<table name>?tn=<table name>&sv=2016-05-31&sig=<the signature>&se=<the validity end time>&sp=r` de shared access signature moet de query (lezen) machtigingen hebben voor de tabel.

Toegang tot de handtekeningen voor meer informatie over gedeelde opslag, Zie [gedeelde handtekeningen voor toegang](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Als u gedeelde toegang handtekening referenties gebruikt, moet u de referenties voor gegevensbron regelmatig bijgewerkt met vernieuwde handtekeningen om te voorkomen dat ze zijn verlopen. Als shared access signature referenties is verlopen, wordt de indexeerfunctie mislukt met een foutbericht weergegeven dat vergelijkbaar is met "In de verbindingstekenreeks opgegeven referenties zijn ongeldig of verlopen."  

### <a name="step-2-create-an-index"></a>Stap 2: een index maken
De index geeft de velden in een document, de kenmerken en andere constructies die de vorm van de zoekopdracht ervaring.

Een index te maken:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
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

### <a name="step-3-create-an-indexer"></a>Stap 3: Maak een indexeerfunctie
Een indexeerfunctie een gegevensbron is verbonden met een doel search-index en vindt u een planning voor het automatiseren van de gegevens te vernieuwen. 

Nadat de index en gegevensbron zijn gemaakt, bent u klaar voor het maken van de indexeerfunctie:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Deze indexeerfunctie wordt elke twee uur uitgevoerd. (De schema-interval is ingesteld op 'PT2H'). Als u wilt uitvoeren met een indexeerfunctie elke 30 minuten, stelt u het interval voor 'PT30M'. De kortste interval is vijf minuten. Het schema is optioneel. Als u dit weglaat, wordt er een indexeerfunctie uitgevoerd slechts eenmaal wanneer deze gemaakt. U kunt echter een indexeerfunctie uitvoeren op aanvraag op elk gewenst moment.   

Zie voor meer informatie over de indexeerfunctie maken API [indexeerfunctie maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="deal-with-different-field-names"></a>Omgaan met verschillende veldnamen
Soms is wijken de veldnamen in uw bestaande index af van de namen van de eigenschappen in de tabel. Veldtoewijzingen kunt u de eigenschapnamen uit de tabel worden de veldnamen worden toegewezen in uw search-index. Zie voor meer informatie over het toewijzen van veld, [veld-verwijzingen voor Azure Search-indexeerfunctie overbruggen de verschillen tussen de gegevensbronnen en zoekindexen](search-indexer-field-mappings.md).

## <a name="handle-document-keys"></a>Ingang document sleutels
In Azure Search identificatie de documentsleutel unieke van een document. Elke search-index moet exact één sleutelveld van het type `Edm.String`. Het sleutelveld is vereist voor elk document dat wordt toegevoegd aan de index. (In feite dit is het enige verplichte veld.)

Aangezien de tabelrijen hebben een samengestelde sleutel, Azure Search genereert een synthetische veld `Key` die een samenvoeging van de partitie sleutel- en sleutelwaarden is. Bijvoorbeeld, als een rij van het PartitionKey is `PK1` en RowKey `RK1`, wordt de `Key` waarde van veld is `PK1RK1`.

> [!NOTE]
> De `Key` waarde mag de tekens die ongeldig in het document sleutels, zoals streepjes zijn bevatten. U kunt maken met ongeldige tekens met behulp van de `base64Encode` [veld toewijzing functie](search-indexer-field-mappings.md#base64EncodeFunction). Als u dit doet, moet u ook gebruiken URL-safe Base64-codering wanneer het document sleutels doorgeven in een API-aanroepen zoals Lookup.
>
>

## <a name="incremental-indexing-and-deletion-detection"></a>Detectie van incrementele indexeren en verwijderen
Bij het instellen van een tabel indexeerfunctie worden uitgevoerd op een planning, het reindexes alleen nieuwe of bijgewerkte rijen, zoals wordt bepaald door een rij `Timestamp` waarde. U hoeft niet te geven van een wijzigingsbeleid. Incrementele indexeren is automatisch ingeschakeld.

Om aan te geven dat bepaalde documenten van de index moeten worden verwijderd, kunt u een strategie voor voorlopig verwijderen gebruiken. In plaats van een rij verwijdert, moet u een eigenschap om aan te geven dat het is verwijderd en van een voorlopig detectie van het verwijderingsbeleid voor de datasource instellen toevoegen. Bijvoorbeeld, het volgende beleid van oordeel is dat een rij is verwijderd als de rij een eigenschap heeft `IsDeleted` met de waarde `"true"`:

    PUT https://[service name].search.windows.net/datasources?api-version=2016-09-01
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

Azure Search gebruikt standaard de volgende query-filter: `Timestamp >= HighWaterMarkValue`. Omdat Azure-tabellen hoeft een secundaire index op de `Timestamp` veld, dit type query vereist een volledige tabelscan en kan daarom langzaam is voor grote tabellen.


Hier vindt u twee mogelijke methoden voor het verbeteren van de tabel indexering prestaties. Deze beide benaderingen afhankelijk van het gebruik van Tabelpartities: 

- Als uw gegevens kunnen natuurlijk verschillende partitie bereiken worden gepartitioneerd, maakt u een gegevensbron en een bijbehorende indexeerfunctie voor elke partitiebereik. Elke indexeerfunctie heeft nu alleen een specifieke partitiebereik, wat resulteert in betere prestaties van query's te verwerken. Als de gegevens die moet worden geïndexeerd een klein aantal vaste partities nog beter heeft: elke indexeerfunctie biedt alleen een scan van de partitie. Om bijvoorbeeld te maken van een gegevensbron voor het verwerken van een partitiebereik met sleutels uit `000` naar `100`, een query als volgt gebruiken: 
    ```
    "container" : { "name" : "my-table", "query" : "PartitionKey ge '000' and PartitionKey lt '100' " }
    ```

- Als uw gegevens is gepartitioneerd door tijd (bijvoorbeeld: u maakt een nieuwe partitie elke dag of week), houd rekening met de volgende benadering: 
    - Gebruik een query van het formulier: `(PartitionKey ge <TimeStamp>) and (other filters)`. 
    - Indexeerfunctie voortgang controleren met behulp van [ophalen indexeerfunctie Status API](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status), en regelmatig bijwerken de `<TimeStamp>` voorwaarde van de query op basis van de meest recente geslaagde high-water-mark waarde. 
    - Als u nodig hebt voor het activeren van een volledige indexeren met deze methode moet u de query datasource naast opnieuw instellen van de indexeerfunctie opnieuw instellen. 


## <a name="help-us-make-azure-search-better"></a>Help ons Azure Search te verbeteren
Als u functieaanvragen of suggesties voor verbeteringen, dient u ze op onze [UserVoice site](https://feedback.azure.com/forums/263029-azure-search/).
