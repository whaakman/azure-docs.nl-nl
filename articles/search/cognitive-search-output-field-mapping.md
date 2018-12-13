---
title: Cognitieve zoekopdrachten kaart uitgebreid invoervelden naar uitvoervelden - Azure Search
description: Pak en verrijken van velden voor bron en toewijzen aan uitvoervelden in een Azure Search-index.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: a662558deeeb09849a23a361aabea962eb633cd7
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314666"
---
# <a name="how-to-map-enriched-fields-to-a-searchable-index"></a>Verrijkt velden toewijzen aan een doorzoekbare index

In dit artikel leert u hoe u geavanceerde invoervelden toewijzen aan velden in een doorzoekbare index uitvoeren. Als u eenmaal hebt [gedefinieerd van een set vaardigheden](cognitive-search-defining-skillset.md), moet u de uitvoervelden van een kwalificatie die rechtstreeks deel uitmaakt van de waarden aan een bepaald veld in uw search-index worden toegewezen. Veldtoewijzingen zijn vereist voor het verplaatsen van de inhoud van verrijkt documenten in de index.


## <a name="use-outputfieldmappings"></a>Gebruik outputFieldMappings
Toevoegen als velden wilt toewijzen, `outputFieldMappings` aan de definitie van de indexeerfunctie zoals hieronder wordt weergegeven:

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```

De hoofdtekst van de aanvraag is als volgt opgebouwd:

```json
{
    "name": "myIndexer",
    "dataSourceName": "myDataSource",
    "targetIndexName": "myIndex",
    "skillsetName": "myFirstSkillSet",
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction": {
                "name": "base64Encode"
            }
        }
    ],
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/content/organizations/*/description",
            "targetFieldName": "descriptions"
        },
        {
            "sourceFieldName": "/document/content/organizations",
            "targetFieldName": "orgNames"
        },
        {
            "sourceFieldName": "/document/content/sentiment",
            "targetFieldName": "sentiment"
        }
    ]
}
```
Voor elk veld de toewijzing van uitvoer, stelt u de naam van de geavanceerde veld (sourceFieldName) en de naam van het veld waarnaar wordt verwezen in de index (targetFieldName).

Het pad in een sourceFieldName kan één element of meerdere elementen vertegenwoordigen. In het bovenstaande voorbeeld ```/document/content/sentiment``` vertegenwoordigt een numerieke waarde, terwijl ```/document/content/organizations/*/description``` verschillende organisatie-beschrijvingen vertegenwoordigt. In gevallen waarbij er verschillende elementen, ze worden "afgevlakt" in een matrix met de elementen. Meer concrete invulling te geven, voor de ```/document/content/organizations/*/description``` voorbeeld, worden de gegevens in de *beschrijvingen* veld eruit als een platte-matrix van beschrijvingen voordat deze worden geïndexeerd:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```
## <a name="next-steps"></a>Volgende stappen
Als u uw verrijkt velden aan doorzoekbare velden hebt toegewezen, kunt u het veldkenmerken instellen voor elk van de doorzoekbare velden [als onderdeel van de indexdefinitie](search-what-is-an-index.md).

Zie voor meer informatie over veldtoewijzing [veldtoewijzingen in Azure Search-indexeerfuncties](search-indexer-field-mappings.md).