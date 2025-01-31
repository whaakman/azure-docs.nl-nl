---
title: Algemene problemen search indexer - Azure Search
description: Los de fouten en bekende problemen met de indexeerfuncties in Azure Search, met inbegrip van de verbinding met de gegevensbron, firewall en ontbrekende documenten.
author: mgottein
manager: cgronlun
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: magottei
ms.custom: seodec2018
ms.openlocfilehash: 1cb3260fa11354de963318a023fec912d082eae4
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67653405"
---
# <a name="troubleshooting-common-indexer-issues-in-azure-search"></a>Het oplossen van veelvoorkomende problemen met indexeerfunctie in Azure Search

Indexeerfuncties kunnen een aantal problemen ondervindt bij het indexeren van gegevens in Azure Search. De belangrijkste categorieën van de fout zijn onder andere:

* [Verbinding maken met een gegevensbron](#data-source-connection-errors)
* [Verwerking van documenten](#document-processing-errors)
* [Opname van document naar een index](#index-errors)

## <a name="data-source-connection-errors"></a>Data Source-verbindingsfouten

### <a name="blob-storage"></a>Blob Storage

#### <a name="storage-account-firewall"></a>Storage-account firewall

Azure Storage biedt een configureerbare firewall. De firewall is standaard uitgeschakeld, zodat Azure Search verbinding met uw storage-account maken kunt.

Er is geen specifiek foutbericht weergegeven wanneer de firewall is ingeschakeld. Normaal gesproken fouten firewall eruit `The remote server returned an error: (403) Forbidden`.

U kunt controleren dat de firewall is ingeschakeld in de [portal](https://docs.microsoft.com/azure/storage/common/storage-network-security#azure-portal). De enige ondersteunde oplossing is om uit te schakelen van de firewall door te kiezen zodat toegang vanaf ['Alle netwerken'](https://docs.microsoft.com/azure/storage/common/storage-network-security#azure-portal).

Als uw indexeerfunctie beschikt niet over een gekoppelde vaardigheden u _kan_ probeert te [een uitzondering toevoegen](https://docs.microsoft.com/azure/storage/common/storage-network-security#managing-ip-network-rules) voor IP-adressen van uw search-service. Echter, in dit scenario wordt niet ondersteund en is niet noodzakelijkerwijs om te werken.

U vindt het IP-adres van uw search-service door het pingen van de FQDN-naam (`<your-search-service-name>.search.windows.net`).

### <a name="cosmos-db"></a>Cosmos DB

#### <a name="indexing-isnt-enabled"></a>Indexeren is niet ingeschakeld

Azure Search is een impliciete afhankelijkheid van Cosmos DB indexeren. Als u automatische indexering in Cosmos DB uitschakelt, wordt Azure Search retourneert een geslaagde staat, maar naar inhoud van de container index is mislukt. Zie voor instructies over hoe u instellingen controleren en schakel indexeren [beheren in Azure Cosmos DB indexeren](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-azure-portal).

## <a name="document-processing-errors"></a>Fouten in verwerking van document

### <a name="unprocessable-or-unsupported-documents"></a>Onverwerkbare of niet-ondersteunde documenten

De blob-indexeerfunctie [documenten dat indelingen document expliciet worden ondersteund.](search-howto-indexing-azure-blob-storage.md#supported-document-formats). Soms bevat een blob storage-container niet-ondersteunde documenten. In andere gevallen mogelijk zijn er problematische documenten. U kunt voorkomen dat de indexeerfunctie stoppen op deze documenten door [wijzigen van configuratie-opties](search-howto-indexing-azure-blob-storage.md#dealing-with-errors):

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false, "failOnUnprocessableDocument" : false } }
}
```

### <a name="missing-document-content"></a>Ontbrekende documentinhoud

De blob-indexeerfunctie [gevonden en haalt tekst op uit de blobs in een container](search-howto-indexing-azure-blob-storage.md#how-azure-search-indexes-blobs). Er zijn enkele problemen met de tekst te extraheren:

* Het document bevat alleen gescande afbeeldingen. Resultaten in een standaard blob-indexering pijplijn niet leiden tot een PDF-blobs die niet-tekstuele inhoud, zoals gescande afbeeldingen (JPG-bestanden), hebben. Als u de inhoud van de afbeelding met tekstelementen hebt, kunt u [cognitief zoeken](cognitive-search-concept-image-scenarios.md) om te zoeken en ophalen van de tekst.
* De blob-indexeerfunctie is geconfigureerd om alleen de metagegevens van de index. Als u wilt uitpakken van inhoud, de blob-indexeerfunctie moet worden geconfigureerd om [zowel inhoud en metagegevens extraheren](search-howto-indexing-azure-blob-storage.md#controlling-which-parts-of-the-blob-are-indexed):

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "dataToExtract" : "contentAndMetadata" } }
}
```

## <a name="index-errors"></a>Index-fouten

### <a name="missing-documents"></a>Ontbrekende documenten

Indexeerfuncties zoeken naar documenten van een [gegevensbron](https://docs.microsoft.com/rest/api/searchservice/create-data-source). Soms lijkt een document uit de gegevensbron die moet zijn geïndexeerd te ontbreken in een index. Er zijn een aantal veelvoorkomende oorzaken van dat deze fouten kunnen optreden:

* Het document nog niet zijn geïndexeerd. Controleer de portal voor het uitvoeren van geslaagde indexeerfunctie.
* Het document is bijgewerkt nadat de indexeerfunctie worden uitgevoerd. Als uw indexeerfunctie zich op een [planning](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-schedule), uiteindelijk wordt opnieuw uitgevoerd en kies het document.
* De [query](https://docs.microsoft.com/rest/api/searchservice/create-data-source#request-body-syntax) opgegeven in de gegevens bron niet van toepassing op het document. Indexeerfuncties kunnen geen documenten die geen deel uitmaken van de gegevensbron te indexeren.
* [Veldtoewijzingen](https://docs.microsoft.com/rest/api/searchservice/create-indexer#fieldmappings) of [cognitief zoeken](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro) het document zijn gewijzigd en ziet er anders uit dan u verwacht.
* Gebruik de [lookup document API](https://docs.microsoft.com/rest/api/searchservice/lookup-document) aan uw document vinden.
