---
title: Kaart cognitieve zoeken verrijkt invoervelden aan uitvoervelden in Azure Search-index | Microsoft Docs
description: Pak en bron gegevensvelden sitmuleren en toewijzen aan uitvoervelden in een Azure Search-index.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 67e4798070a73eebb8f61b0b260e3104e9ae6237
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-map-enriched-fields-to-a-searchable-index"></a>Het verrijkt velden toewijzen aan een doorzoekbare index

In dit artikel leert u hoe verrijkt invoervelden toewijzen aan uitvoervelden in een doorzoekbare index. Zodra u hebt [een vaardigheden gedefinieerd](cognitive-search-defining-skillset.md), moet u de uitvoervelden van een kwalificatie die rechtstreeks waarden aan een bepaald veld in uw zoekindex bijdraagt toewijzen. Veld-verwijzingen zijn vereist voor het verplaatsen van de inhoud van verrijkt documenten in de index.


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
Voor elk uitvoerveld ingesteld toewijzing heeft de naam van de verrijkt veld (sourceFieldName) en de naam van het veld waarnaar wordt verwezen in de index (targetFieldName).

Het pad in een sourceFieldName kan één element of meerdere elementen vertegenwoordigen. In het bovenstaande voorbeeld ```/document/content/sentiment``` vertegenwoordigt een numerieke waarde, terwijl ```/document/content/organizations/*/description``` beschrijvingen van verschillende organisatie vertegenwoordigt. In gevallen waar er verschillende elementen zijn, ze worden "afgevlakt" in een matrix met elk van de elementen. Meer concrete invulling te geven, voor de ```/document/content/organizations/*/description``` voorbeeld, de gegevens in de *beschrijvingen* veld eruit als een platte matrix beschrijvingen van voordat deze wordt geïndexeerd:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```
## <a name="next-steps"></a>Volgende stappen
Nadat u uw verrijkt velden aan doorzoekbare velden toegewezen hebt, kunt u het veldkenmerken instellen voor elk van de doorzoekbare velden [als onderdeel van de indexdefinitie](search-what-is-an-index.md).

Zie voor meer informatie over veldtoewijzing [toewijzingen in Azure Search indexeerfuncties veld](search-indexer-field-mappings.md).