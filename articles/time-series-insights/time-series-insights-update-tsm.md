---
title: Time Series-Model | Microsoft Docs
description: Informatie over Time Series-Model
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: 4cb83b61068922002c0c308f4d129a2e069beadd
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856096"
---
# <a name="time-series-model"></a>Time Series-Model

Deze informatie over het document de **Tijdreeksmodel** (TSM) deel van de update voor Azure Time Series Insights (TSI). Het beschrijft het model zelf, de mogelijkheden ervan, en om te beginnen het bouwen en het bijwerken van uw eigen model.

Traditioneel, de gegevens die worden verzameld van IoT-apparaten beschikt niet over contextuele informatie waardoor het moeilijk is om te zoeken en sensoren snel analyseren. De belangrijkste reden voor TSM is om te zoeken en analyseren van de IoT-gegevens door in te schakelen curatie, onderhoud en verrijking van time series-gegevens kunt voorbereiden op het gegevenssets gereed is voor consumenten te vereenvoudigen. **Time Series-modellen** spelen een belangrijke rol in query's en navigatie omdat ze apparaat en niet-apparaat entiteiten context kunnen worden geplaatst. Gegevens persistent gemaakt in TSM bevoegdheden time series-query's berekeningen door gebruik te maken van de formules die erin zijn opgeslagen.

![TSM][1]

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

Met het doel zodat deze eenvoudige en moeiteloze voor het beheren van time series contextualization kunt TSM in de volgende mogelijkheden in de Azure Time Series Insights (preview):

* De mogelijkheid voor het ontwerpen en beheren van berekeningen of formules en het transformeren van gegevens gebruik te maken van scalaire functies, statistische bewerkingen, enzovoort.
* Het definiëren van relaties tussen bovenliggende-onderliggende navigatie en verwijzing naar een context bieden voor time series-telemetrie inschakelen.
* Eigenschappen die zijn gekoppeld aan het gedeelte exemplaren van exemplaarvelden definiëren en deze te maken van hiërarchieën gebruiken.

## <a name="times-series-model-key-components"></a>Time-out van de belangrijke onderdelen Series-Model

Er zijn drie belangrijke onderdelen van TSM:

* **Time Series-Model** *typen*
* **Time Series-Model** *hiërarchieën*
* **Time Series-Model** *exemplaren*

## <a name="time-series-model-types"></a>Typen van Time Series-Model

**Time Series-Model** *typen* inschakelen definiëren variabelen of formules voor het uitvoeren van berekeningen en zijn gekoppeld aan een bepaald TSI-exemplaar. Een type kan een of meer variabelen hebben. Bijvoorbeeld, een TSI-exemplaar van het type mogelijk **temperatuursensor**, die bestaat uit de variabelen: *gemiddelde temperatuur*, *min temperatuur*, en *max temperatuur*. We maken een standaardtype zodra de gegevens doorgestuurd naar TSI. Het kan worden opgehaald en bijgewerkt van modelinstellingen. Standaardtypen heeft een variabele die het aantal gebeurtenissen telt.

## <a name="time-series-model-type-json-request-and-response-example"></a>Time Series-Model type aanvraag- en voorbeeld van JSON

Gegeven een HTTP POST-aanvraag:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/types/$batch?api-version=API_VERSION
```

| Naam | Beschrijving | Voorbeeld |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  De naam van uw omgeving  | `environment123` |
| API_VERSION  |  De API-specificatie | `2018-11-01-preview` |

Met de volgende JSON hoofdtekst en variabele kenmerken:

| Kenmerk | Vereiste of optionele |
| --- | --- |
| **type**  |  Vereist  |
| **filter**  |  Optioneel |
| **value**  | Null of is niet opgegeven  |
| **interpolatie**  |  Optioneel |
| **Aggregatie**  |  Vereist |

```JSON
{
    "get": null,
    "put": [
        {
            "name": "SampleType",
            "description": "This is type 2",
            "variables": {
                "Avg Temperature": {
                    "kind": "numeric",
                    "filter": null,
                    "value": { "tsx": "$event.temperature.Double" },
                    "interpolation": "None",
                    "aggregation": {"tsx": "avg($value)"}
                },
                "Count Temperature": {
                    "kind": "aggregate",
                    "filter": null,
                    "value": null,
                    "interpolation": "None",
                    "aggregation": {"tsx": "count()"}
                },
                "Min Temperature": {
                    "kind": "aggregate",
                    "filter": null,
                    "value": null,
                    "interpolation": "None",
                    "aggregation": {"tsx": "min($event.temperature)"}
                },
            }
        }
    ]
}
```

Reactie:

```JSON
{
    "get": null,
    "put": [
        {
            "timeSeriesType": {
                "id": "fc4f526c-da6e-4b85-87f7-16f6cf9b69be",
                "name": "type2",
                "description": "This is type 2",
                "variables": {
                    "Avg Temperature": {
                        "kind": "numeric",
                        "filter": null,
                        "value": { "tsx": "$event.temperature.Double" },
                        "interpolation": "None",
                        "aggregation": {"tsx": "avg($value)"}
                    },
                    "Count Temperature": {
                        "kind": "aggregate",
                        "filter": null,
                        "value": null,
                        "interpolation": "None",
                        "aggregation": {"tsx": "count()"}
                    },
                    "Min Temperature": {
                        "kind": "aggregate",
                        "filter": null,
                        "value": null,
                        "interpolation": "None",
                        "aggregation": {"tsx": "min($event.temperature)"}
                    }
                }
            },
            "error": null
        }
    ]
}
``````

Een **standaard** *type* JSON-antwoord:

```JSON
{
    "modelSettings": {
        "name": "DefaultModel",
        "timeSeriesIdProperties": [
            {
                "name": "someType1",
                "type": "String"
            }
        ],
        "defaultTypeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393"
    }
}
```

## <a name="variables"></a>Variabelen

Azure TSI typen variabelen hebben, deze met de naam van berekeningen op de waarden van de gebeurtenissen. TSI variabele definities bevatten formule en berekeningen regels. Variabele definities bevatten type, waarde, filteren, beperken en grenzen. Variabelen worden opgeslagen in de definitie van het type in TSM en inline via API's voor de onderdrukking van de definitie van de opgeslagen in Query kunnen worden opgegeven.

De onderstaande matrix werkt als een legenda voor definities van de variabele:

![tabel][2]

### <a name="variable-kind"></a>Variabele type

De volgende variabele typen worden ondersteund:

* Numerieke: continue

### <a name="variable-filter"></a>Variabele filter

Variabele filters opgeven een optioneel filtercomponent om te beperken van het aantal rijen wordt overwogen voor de berekening op basis van voorwaarden.

### <a name="variable-value"></a>Waarde van variabele

Waarden van variabelen zijn en moeten worden gebruikt in berekeningen. Dit is de kolom in de gebeurtenissen die we naar verwijzen moeten.

### <a name="variable-interpolation"></a>Variabele interpolatie

Het proces van het converteren van een set waarden op een waarde per een interval wordt een variabele vermindering genoemd. Variabele kortingen geaggregeerde opgenomen gegevens uit de bron kunnen zijn of opnieuw opgebouwd signalen met behulp van interpolatie en verzamelen of gereconstrueerde signalen interpolatie gebruiken en steekproeven. Variabele grenzen kunnen worden toegevoegd aan de interpolatie, staat deze berekeningen gebeurtenissen buiten bereik zoeken worden opgenomen.

De Azure TSI (preview) biedt ondersteuning voor de volgende variabele interpolatie: `linear`, `stepright`, en `none`.

### <a name="variable-aggregation"></a>Aggregatie van variabele

De statistische functie van de variabele kan deel uitmaken van de berekening. Als de variabele interpolatie `null` of `none`, vervolgens TSI biedt ondersteuning voor reguliere statistische functies (dat wil zeggen **min**, **max**, **Gem.**, **som** , en **aantal**). Als de variabele interpolatie `stepright` of `linear`, en biedt ondersteuning voor TSI **twmin**, **twmax**, **twavg**, en **twsum**. Aantal kan niet worden opgegeven in de interpolatie.

## <a name="time-series-model-hierarchies"></a>Time Series-Model-hiërarchieën

Hiërarchieën ordenen exemplaren door de namen van eigenschappen en hun relaties op te geven. Mogelijk hebt u een hiërarchie van één of meerdere hiërarchieën. Bovendien deze huidige onderdeel van uw gegevens hoeven niet te worden, maar elk exemplaar moet worden toegewezen aan een hiërarchie. Een exemplaar TSM kunt toewijzen aan een enkele hiërarchie of meerdere hiërarchieën.

Hiërarchieën zijn gedefinieerd door **hiërarchie-ID**, **naam**, en **bron**. Hiërarchieën hebben paden, een pad naar bovenliggende / onderliggende van boven naar beneden volgorde van de hiërarchie van die de gebruiker wil maken. De eigenschappen van de bovenliggende/onderliggende exemplaarvelden toewijzen.

### <a name="time-series-model-hierarchy-json-request-and-response-example"></a>Time Series-Model hiërarchie aanvraag- en voorbeeld van JSON

Gegeven een HTTP POST-aanvraag:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/hierarchies/$batch?api-version=API_VERSION
```

| Naam | Beschrijving | Voorbeeld |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  De naam van uw omgeving  | `environment123` |
| API_VERSION  |  De API-specificatie | `2018-11-01-preview` |

Met JSON-hoofdtekst:

```JSON
{
    "get": null,
    "put": [
        {
            "id": "4c6f1231-f632-4d6f-9b63-e366d04175e3",
            "name": "Location",
            "source": {
                "instanceFieldNames": [
                    "state",
                    "city"
                ]
            }
        }
    ]
}
```

Reactie:

```JSON
{
    "get": null,
    "put": [
        {
            "hierarchy": {
                "id": "4c6f1231-f632-4d6f-9b63-e366d04175e3",
                "name": "Location",
                "source": {
                    "instanceFieldNames": [
                        "state",
                        "city"
                    ]
                }
            },
            "error": null
        }
    ]
}
```

### <a name="hierarchy-definition-behavior"></a>Gedrag voor hiërarchie-definitie

Houd rekening met het volgende voorbeeld waarbij hiërarchie H1 "bouwen", "floor" en 'ruimte' als onderdeel van de definitie ervan heeft:

```plaintext
 H1 = [“building”, “floor”, “room”]
```

Afhankelijk van de exemplaarvelden, de hiërarchie-kenmerken en waarden weergegeven wordt weergegeven: 

| Time Series-ID | Exemplaarvelden |
| --- | --- |
| ID1 | "bouwen" = "1000", "floor" = "10", 'ruimte' = "55"  |
| ID2 | "bouwen" = "1000", 'ruimte' = "55" |
| ID3 |  "floor" = "10" |
| ID4 | "bouwen" = "1000", "floor" = "10"  |
| ID5 | |

In bovenstaande voorbeeld ziet u ID1 als onderdeel van de hiërarchie H1 in de gebruikersinterface/UX, terwijl de rest worden geclassificeerd onder `Unparented Instances` omdat ze niet aan de opgegeven gegevens-hiërarchie voldoen.

## <a name="time-series-model-instances"></a>Time Series-Model exemplaren

-Exemplaren zijn de tijdreeksen zelf. In de meeste gevallen is dit de *deviceId* of *assetId* dat wil zeggen de unieke id van de activa in de omgeving. -Exemplaren beschikken over beschrijvende informatie die is gekoppeld aan deze instantie-eigenschappen aangeroepen. Ten minste bevatten exemplaareigenschappen informatie van de hiërarchie. Ze kunnen ook nuttige, beschrijvende gegevens, zoals de fabrikant, operator of de laatste servicedatum bevatten.

Exemplaren worden gedefinieerd door *timeSeriesId*, *typeId*, *hierarchyId*, en *instanceFields*. Elke instantie wordt toegewezen aan slechts één *type*, en een of meer hiërarchieën. Exemplaren overnemen alle eigenschappen van hiërarchieën, terwijl meer *instanceFields* kan worden toegevoegd voor verdere definitie van exemplaar-eigenschap.

*instanceFields* zijn eigenschappen van een exemplaar en alle statische gegevens waarmee een exemplaar. Deze definiëren waarden van eigenschappen of niet-hiërarchie terwijl het tevens ondersteunt indexeren om uit te voeren van zoekbewerkingen.

## <a name="time-series-model-instance-json-request-and-response-example"></a>Time Series-Model exemplaar aanvraag- en voorbeeld van JSON

Gegeven een HTTP POST-aanvraag:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/instances/$batch?api-version=API_VERSION
```

| Naam | Beschrijving | Voorbeeld |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  De naam van uw omgeving  | `environment123` |
| API_VERSION  |  De API-specificatie | `2018-11-01-preview` |

Met JSON-hoofdtekst:

```JSON
{
    "get": null,
    "put": [
        {
            "name": "SampleName",
            "typeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393",
            "timeSeriesId": [
                "samplePartitionKeyValueOne"
            ],
            "description": "floor 100",
            "hierarchyIds": [
                "e37a4666-9650-42e6-a6d2-788f12d11158"
            ],
            "instanceFields": {
                "state": "California",
                "city": "Los Angeles"
            }
        }
    ]
}
```

Reactie:

```JSON
{
    "get": null,
    "put": [
        {
            "instance": null,
            "error": null
        },
        {
            "instance": null,
            "error": null
        }
    ]
}
```

## <a name="time-series-model-settings-example"></a>Voorbeeld van instellingen voor Time Series-Model

Gegeven een HTTP POST-aanvraag:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/modelSettings?api-version=API_VERSION
```

| Naam | Beschrijving | Voorbeeld |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  De naam van uw omgeving  | `environment123` |
| API_VERSION  |  De API-specificatie | `2018-11-01-preview` |

Reactie:

```JSON
{
    "modelSettings": {
        "name": "DefaultModel",
        "timeSeriesIdProperties": [
            {
                "name": "someType1",
                "type": "String"
            }
        ],
        "defaultTypeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393"
    }
}
```

## <a name="time-series-model-limits"></a>Tijdslimieten Series-Model

| Parameter |   Limieten |
| --- | --- |
| Objectgrootte voor de model-entiteiten (typen, hiërarchieën en exemplaren)|  32 KB bevat eigenschappen |
| Sleutels die zijn toegestaan als TSID-eigenschap geconfigureerd via Azure Portal |   Maximaal 3 |
| Maximum aantal typen in een omgeving |    1000|
| Maximum aantal variabelen in een type |    50|
| Maximum aantal hiërarchieën in een omgeving|   32|
| Max hiërarchie diepte | 32|
| Maximum aantal hiërarchieën die zijn gekoppeld aan 1 exemplaar   |21|
| Maximum aantal exemplaren in een omgeving |    500,000|
| Maximum aantal Instantievelden per exemplaar |   50|
| Model objecten upsert/bijwerken/verwijderen bewerking per seconde   |100 per seconde|
| Timeseries-Model de grootte van de API-aanvraag: Batch |  8 MB of 1000 modelobjecten (afhankelijk van welke situatie zich het eerst voordoet)|
| Time Series-Model aanvraaggrootte: zoeken/voorstellen   | 32 KB|
| Timeseries-Model de grootte van de API-aanvraag: Batch    | 32 MB|
| Zoeken/suggesties is 32 MB|  32 MB|

[!INCLUDE [tsi-update-docs](../../includes/time-series-insights-update-documents.md)]

## <a name="next-steps"></a>Volgende stappen

Lees de [Azure TSI (preview) Storage en binnenkomende](./time-series-insights-update-storage-ingress.md).

Meer informatie over de nieuwe [Tijdreeksmodel](./time-series-insights-update-tsm.md).

<!-- Images -->
[1]: media/v2-update-tsm/tsm.png
[2]: media/v2-update-tsm/table.png
