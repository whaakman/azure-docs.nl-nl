---
title: Index blobs met meerdere documenten van de index van Azure Blob-indexering voor zoeken in volledige tekst - Azure Search zoeken
description: Verken Azure-blobs voor tekstinhoud met behulp van de indexeerfunctie Azure Search Blob. Elke blob kan een of meer Azure Search-index documenten bevatten.
ms.date: 02/12/2019
author: arv100kri
manager: briansmi
ms.author: arjagann
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seofeb2018
ms.openlocfilehash: e95eff015340659b642dff800a03f615e22c1577
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57220569"
---
# <a name="indexing-blobs-producing-multiple-search-documents"></a>Indexeren van blobs produceren van documenten met meerdere zoeken
Standaard wordt een blob-indexeerfunctie de inhoud van een blob behandelen als een document met één zoekopdracht doorzoeken. Bepaalde **parsingMode** waarden ondersteuning van scenario's waarbij een afzonderlijke blob in meerdere documenten met zoeken resulteren kan. De verschillende typen **parsingMode** waarmee een indexeerfunctie om op te halen van meer dan één zoekopdracht documenten van een blob zijn:
+ `delimitedText`
+ `jsonArray`
+ `jsonLines`

> [!IMPORTANT]
> `jsonLines` parseermodus is in openbare preview en mag niet worden gebruikt in een productieomgeving. Zie voor meer informatie, [REST api-version = 2017-11-11-Preview](search-api-2017-11-11-preview.md). 

## <a name="one-to-many-document-key"></a>Een-op-veel-documentsleutel
Elk document dat weergegeven in een Azure Search-index wordt wordt uniek geïdentificeerd door een documentsleutel. 

Wanneer er geen parseren modus is opgegeven en er is geen expliciete toewijzing voor een veld in de Azure Search-index met de sleutel automatisch [toegewezen](search-indexer-field-mappings.md) de `metadata_storage_path` eigenschap als de sleutel. Deze toewijzing zorgt ervoor dat elke blob wordt weergegeven als een document afzonderlijke zoeken.

Wanneer u een van de hierboven vermelde parseermodi, wordt een blob wordt toegewezen aan 'veel' search-documenten, waardoor de documentsleutel van een op basis van de metagegevens van de blob is niet geschikt. Azure Search is om te strijden tegen deze beperking, die geschikt is voor het genereren van een 'een-op-veel'-documentsleutel voor elke afzonderlijke entiteit opgehaald uit een blob. Deze eigenschap is met de naam `AzureSearch_DocumentKey` en wordt toegevoegd aan elke afzonderlijke entiteit opgehaald uit de blob. De waarde van deze eigenschap kan worden gegarandeerd uniek zijn voor elke afzonderlijke entiteit _voor blobs_ en de entiteiten worden weergegeven als afzonderlijke zoekopdracht documenten.

Standaard, als er geen expliciete veldtoewijzingen voor het veld sleutelindex zijn opgegeven, de `AzureSearch_DocumentKey` is toegewezen, met behulp van de `base64Encode` veldtoewijzing functie.

## <a name="example"></a>Voorbeeld
Wordt ervan uitgegaan dat u hebt een definitie van de index met de volgende velden:
+ `id`
+ `temperature`
+ `pressure`
+ `timestamp`

En uw blob-container blobs met de volgende structuur heeft:

_Blob1.json_

    { "temperature": 100, "pressure": 100, "timestamp": "2019-02-13T00:00:00Z" }
    { "temperature" : 33, "pressure" : 30, "timestamp": "2019-02-14T00:00:00Z" }

_Blob2.json_

    { "temperature": 1, "pressure": 1, "timestamp": "2018-01-12T00:00:00Z" }
    { "temperature" : 120, "pressure" : 3, "timestamp": "2013-05-11T00:00:00Z" }

Wanneer u een indexeerfunctie maken en stel de **parsingMode** naar `jsonLines` - zonder op te geven een expliciete veld toewijzingen voor het sleutelveld, de volgende toewijzing wordt impliciet worden toegepast.
    
    {
        "sourceFieldName" : "AzureSearch_DocumentKey",
        "targetFieldName": "id",
        "mappingFunction": { "name" : "base64Encode" }
    }

Deze configuratie leidt tot de Azure Search-index met de volgende informatie (base64-gecodeerde id verkort voor beknoptheid)

| id | temperatuur | pressure | tijdstempel |
|----|-------------|----------|-----------|
| aHR0... YjEuanNvbjsx | 100 | 100 | 2019-02-13T00:00:00Z |
| aHR0... YjEuanNvbjsy | 33 | 30 | 2019-02-14T00:00:00Z |
| aHR0... YjIuanNvbjsx | 1 | 1 | 2018-01-12T00:00:00Z |
| aHR0... YjIuanNvbjsy | 120 | 3 | 2013-05-11T00:00:00Z |

## <a name="custom-field-mapping-for-index-key-field"></a>Aangepast veldtoewijzing voor index sleutelveld

Ervan uitgaande dat de definitie van de dezelfde index als het vorige voorbeeld, dat u dat uw blob-container blobs met de volgende structuur heeft:

_Blob1.json_

    recordid, temperature, pressure, timestamp
    1, 100, 100,"2019-02-13T00:00:00Z" 
    2, 33, 30,"2019-02-14T00:00:00Z" 

_Blob2.json_

    recordid, temperature, pressure, timestamp
    1, 1, 1,"2018-01-12T00:00:00Z" 
    2, 120, 3,"2013-05-11T00:00:00Z" 

Bij het maken van een indexeerfunctie met `delimitedText` **parsingMode**, deze mogelijk ook natuurlijke voor het instellen van een veldtoewijzing functie voor het sleutelveld als volgt:

    {
        "sourceFieldName" : "recordid",
        "targetFieldName": "id"
    }

Deze toewijzing wordt echter _niet_ leiden tot 4 documenten die worden weergegeven in de index, omdat de `recordid` veld is niet uniek _voor blobs_. Daarom raden we u om het gebruik van de impliciete veldtoewijzing toegepast van de `AzureSearch_DocumentKey` eigenschap aan het veld sleutelindex voor parseermodi "een-op-veel".

Als u instellen van een expliciete veldtoewijzing wilt, zorg ervoor dat de _sourceField_ is niet hetzelfde als voor elke afzonderlijke entiteit **in alle blobs**.

> [!NOTE]
> De benadering die wordt gebruikt door `AzureSearch_DocumentKey` om te garanderen uniekheid per uitgepakte entiteit kan worden gewijzigd en daarom is niet verstandig op de waarde voor de behoeften van uw toepassing.

## <a name="see-also"></a>Zie ook

+ [Indexeerfuncties in Azure Search](search-indexer-overview.md)
+ [Azure Blob Storage met Azure Search indexeren](search-howto-index-json-blobs.md)
+ [Indexeren van CSV-blobs met de indexeerfunctie voor Azure Search blob](search-howto-index-csv-blobs.md)
+ [Indexeren van JSON-blobs met de indexeerfunctie voor Azure Search blob](search-howto-index-csv-blobs.md)

## <a name="NextSteps"></a>Volgende stappen
* Zie voor meer informatie over Azure Search, de [pagina zoekservice](https://azure.microsoft.com/services/search/).