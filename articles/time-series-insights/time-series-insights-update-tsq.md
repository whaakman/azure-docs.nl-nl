---
title: Time Series-Query | Microsoft Docs
description: Axure Time Series Insights (preview) Time Series-Query
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: f5370d354833e9507d0794d7bc407a7ea2294e1a
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856104"
---
# <a name="time-series-query"></a>Time Series-Query

**Time Series Query** (TSQ), wordt het gemakkelijker te berekenen en ophalen van Time Series-gegevens die zijn opgeslagen in Time Series Insights (TSI) op schaal. TSQ maakt gebruik van rekenkundige definities om te transformeren en ophalen van gegevens van de opslag. Dit gebeurt van **Time Series modellen** (TSM) of via inline variabele definities.

TSQ haalt gegevens op twee verschillende manieren. TSQ kunnen gegevens worden opgehaald omdat deze wordt vastgelegd van de provider van de gegevensbron, of kunt gegevens beperken, of kunt reconstrueren bewerkingen voor de signalen gebruik te maken van de opgegeven methode om klanten in staat om te zetten, combineren, en het uitvoeren van berekeningen op time series-gegevens.

Time Series Insights-gegevens zijn toegankelijk via de systeemeigen TSI update Explorer of de openbare Surface-API. TSQ biedt ook een expressietaal, zodat u u eigen meer geavanceerde TSI-query's kunt maken. Hieronder vindt u de doelstellingen met TSQ:

![Doel][1]

## <a name="core-apis"></a>Core-API 's

Hieronder vindt u de core API's wordt ondersteund.

![tsq][2]

### <a name="getmodelsettings-api"></a>getModelSettings API

De API getModelSettings wordt gebruikt om te retourneren van de automatisch gemaakte model voor de partitiesleutel omgevingen.

De getModelSettings API bevat de volgende parameters:

* *naam*: de naam van het model dat u wilt ophalen.
* *timeSeriesIdProperties*

  * *De naam*
  * *type*

* *defaultTypeID*

#### <a name="getmodelsettings-json-request-and-response-example"></a>aanvraag en antwoord-voorbeeld van JSON getModelSettings

Gegeven een GET HTTP-aanvraag:

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

### <a name="gettypes-api"></a>getTypes API

De getTypes API retourneert alle typen van de Time Series en hun bijbehorende variabelen voor het model.

De getTypes API bevat de volgende parameters:

* *typeId*: de onveranderbare unieke id voor het type.
* *timeSeriesId*: de **Time Series-ID** is de unieke sleutel voor de gegevens in de gebeurtenisstroom en het model. Deze sleutel is TSI wordt gebruikt voor het partitioneren van de gegevens.
* *beschrijving*: de beschrijving van het type.
* *hierarchyIds*: een matrix met bijbehorende hierarchyIds voor het type.
* *instanceFields*:
  * *status*
  * *plaats*

#### <a name="gettypes-json-request-and-response-example"></a>aanvraag en antwoord-voorbeeld van JSON getTypes

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
```

### <a name="gethierarchies-api"></a>getHierarchies API

De getHierarchies API retourneert alle van de Time Series-hiërarchieën en hun bijbehorende veld paden.

De getHierarchies API bevat de volgende parameters:

* *id*: de unieke id-sleutel voor de hiërarchie.
* *naam*: de naam van de hiërarchie
* *Bron*
* *instanceFields*
  * *Jaar*
  * *Maand*

#### <a name="gethierarchies-json-request-and-response-example"></a>aanvraag en antwoord-voorbeeld van JSON getHierarchies

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

### <a name="getinstances-api"></a>getInstances API

De getInstances API wordt gebruikt om alle exemplaren van de Time Series en hun bijbehorende instantievelden te retourneren.

De getInstances API bevat de volgende parameters:

* *naam*: de naam die is gekoppeld aan de instantie, die wordt gebruikt voor het uitvoeren van query's, evenals ter vervanging in UX.
* *typeId*: de unieke id-sleutel voor het type.
* *timeSeriesId*: de **Time Series-ID** is de unieke sleutel voor de gegevens in de gebeurtenisstroom en het model. Deze sleutel is TSI wordt gebruikt voor het partitioneren van de gegevens.
* *beschrijving*: de beschrijving van het exemplaar.
* *hierarchyIds*: een matrix met een of meer van hierarchyIds die unieke elke hiërarchie worden gedefinieerd.
* *instanceFields*:
  * *status*
  * *plaats*

#### <a name="getinstances-json-request-and-response-example"></a>aanvraag en antwoord-voorbeeld van JSON getInstances

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

### <a name="aggregateseries-api"></a>aggregateSeries API

De aggregateSeries die API kunt query's en voor het ophalen van gegevens van TSI van vastgelegde gebeurtenissen door steekproeven en aggregeren van gegevens met behulp van de functies cumulatieve of voorbeeld opgenomen.

De aggregateSeries API bevat de volgende parameters:

* *timeSeriesId*: de **Time Series-ID** is de unieke sleutel voor de gegevens in de gebeurtenisstroom en het model. Deze sleutel is TSI wordt gebruikt voor het partitioneren van de gegevens.
* *searchSpan*: de duur en bucket grootte voor deze statistische expressie.
* *Filter*: optionele predikaatfunctie-component.
* *Interval*: Interval waarmee gegevens moeten worden berekend.
* *ProjectedVariables*: variabelen die in het bereik moeten worden berekend.
* *InlineVariables(optional)*: definities van variabelen die we willen op negeren die niet via TSM of opgegeven inline voor berekeningen.

De getSeries API retourneert een TSV voor elke variabele voor elk interval, op basis van de opgegeven **Time Series-ID** en de set beschikbare variabelen. De getSeries API realiseert verminderen door gebruik te maken van variabelen die zijn opgeslagen in TSM of inline aggregate of voorbeeld gegevens opgegeven.

> [!NOTE]
> Variabele interpolatie wordt momenteel niet ondersteund.

Cumulatieve typen ondersteund:

* `Min`
* `Max`
* `Sum`
* `Count`
* `Average`

#### <a name="aggregateseries-json-request-and-response-example"></a>aanvraag en antwoord-voorbeeld van JSON aggregateSeries

Gegeven een HTTP POST-aanvraag:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/query?api-version=API_VERSION
```

| Naam | Beschrijving | Voorbeeld |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  De naam van uw omgeving  | `environment123` |
| API_VERSION  |  De API-specificatie | `2018-11-01-preview` |

Met JSON-hoofdtekst:

```JSON
{
    "aggregateSeries": {
        "timeSeriesId": ["da2754af-cc51-46b3-b18f-6231f8781a12","PU.98"],
        "searchSpan": {
            "from": {"dateTime": "2016-08-01T00:00:00Z"},
            "to": {"dateTime": "2016-08-01T00:10:00Z"}
        },
        //Optional
        //If provided, Reduction should be specified in Variable Definition
        "interval": "PT1M",
        "filter": null, //Optional
        "projectedVariables": [
            "Count",
            "Average Temperature",
            "Min Temperature"
        ],
        "inlineVariables": {
            "Average Temperature": {
                "kind": "numeric",
                "filter": null,
                "value": { "tsx": "$event.temperature.Double" },
                "interpolation": "None",
                "aggregation": {"tsx": "avg($value)"}
            },
            //Default Type Count Experience
            "Count": {
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
}
```

Reactie:

```JSON
{
    "timestamps": [ "2016-08-01T00:00:00Z","2016-08-01T00:01:00Z","2016-08-01T00:02:00Z","2016-08-01T00:03:00Z","2016-08-01T00:04:00Z",
        "2016-08-01T00:05:00Z","2016-08-01T00:06:00Z","2016-08-01T00:07:00Z","2016-08-01T00:08:00Z","2016-08-01T00:09:00Z" ],
    "properties": [
        {
            "name": "Average Temperature",
            "type": "Double",
            "values": [ 68.699982037970059,68.889287593526234,69.089287593526919,69.28928759352759,69.489287593528246,69.689287593528917,69.8892875935296,70.089287593530273,70.289287593530929,70.4892875935316]
        },
        {
            "name": "Count Temperature",
            "type": "Double",
            "values": [ 60.0,60.0,60.0,60.0,60.0,60.0,60.0,60.0,60.0,60.0 ]
        },
        {
            "name": "Min Temperature",
            "type": "Double",
            "values": [ 68.645954260192127,68.790954260192578,68.990954260193249,69.190954260193919,69.39095426019459,69.590954260195261,69.790954260195932,69.9909542601966,70.190954260197273,70.390954260197944 ]
        }
    ]
}
```

### <a name="getseries-api"></a>getSeries API

De API kunt query's en voor het ophalen van gegevens van TSI van vastgelegde gebeurtenissen door gebruik te maken van gegevens die zijn geregistreerd op de kabel met behulp van de variabelen getSeries definiëren in het model of inline opgegeven.

> [!Note]
> Als interpolatie en aggregatie-component is opgegeven in de variabele of interval is opgegeven, wordt dit genegeerd.

De getSeries API bevat de volgende parameters:

* *timeSeriesId*: de **Time Series-ID** is de unieke sleutel voor de gegevens in de gebeurtenisstroom en het model. Deze sleutel is TSI wordt gebruikt voor het partitioneren van de gegevens.
* *searchSpan*: de duur en bucket grootte voor deze statistische expressie.
* *Filter*: optionele predikaatfunctie-component.
* *ProjectedVariables*: variabelen die in het bereik moeten worden berekend.
* *InlineVariables(optional)*: definities van variabelen die we willen op negeren die niet via TSM of opgegeven inline voor berekeningen.

De getSeries API retourneert een TSV voor elke variabele op basis van de opgegeven **Time Series-ID** en de set beschikbare variabelen. De API behaalt getSeries biedt geen ondersteuning voor intervallen of variabele vermindering/interpolatie.  

#### <a name="getseries-json-request-and-response-example"></a>aanvraag en antwoord-voorbeeld van JSON getSeries

Gegeven een HTTP POST-aanvraag:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/query?api-version=API_VERSION
```

| Naam | Beschrijving | Voorbeeld |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  De naam van uw omgeving  | `environment123` |
| API_VERSION  |  De API-specificatie | `2018-11-01-preview` |

Met JSON-hoofdtekst:

```JSON
{
    "getSeries": {
        "timeSeriesId": ["da2754af-cc51-46b3-b18f-6231f8781a12","PU.98"],
        "searchSpan": {
            "from": {"dateTime": "2016-08-01T00:00:00Z"},
            "to": {"dateTime": "2016-08-01T00:10:00Z"}
        },
        "interval": null, //Null or not specified
        "filter": null, //Optional
        "projectedVariables": [
            "Temperature",
            "Pressure",
        ],
        "inlineVariables": {
            "Temperature": {
                "kind": "numeric",
                "filter": null,
                "value": { "tsx": "$event.temperature.Double" },
                "interpolation": "None", // null, not specified or ignored if specified
                "aggregation": {"tsx": "avg($value)"} //null, not specified or ignored if specified
            },
            "Pressure": {
                "kind": "numeric",
                "filter": null,
                "value": { "tsx": "$event.pressure.Double" },
                "interpolation": "None", // null, not specified or ignored if specified
                "aggregation": {"tsx": "avg($value)"} //null, not specified or ignored if specified
            }
        }
    }
}
```

Reactie:

```JSON
{
    "timestamps": [ "2016-08-01T00:00:00Z","2016-08-01T00:01:00Z","2016-08-01T00:02:00Z","2016-08-01T00:03:00Z","2016-08-01T00:04:00Z",
        "2016-08-01T00:05:00Z","2016-08-01T00:06:00Z","2016-08-01T00:07:00Z","2016-08-01T00:08:00Z","2016-08-01T00:09:00Z" ],
    "properties": [
        {
            "name": "Temperature",
            "type": "Double",
            "values": [ 68.699982037970059,68.889287593526234,69.089287593526919,69.28928759352759,69.489287593528246,69.689287593528917,69.8892875935296,70.089287593530273,70.289287593530929,70.4892875935316 ]
        },
        {
            "name": "Pressure",
            "type": "Double",
            "values": [ 68.645954260192127,68.790954260192578,68.990954260193249,69.190954260193919,69.39095426019459,69.590954260195261,69.790954260195932,69.9909542601966,70.190954260197273,70.390954260197944 ]
        }
    ]
}
```

### <a name="getevents-api"></a>getEvents API

De getEvents API kunt query's en voor het ophalen van gegevens van TSI van gebeurtenissen, zoals ze zijn vastgelegd in Time Series Insights van de provider van de gegevensbron.

De getEvents API bevat de volgende parameters:

* *timeSeriesId*: de **Time Series-ID** is de unieke sleutel voor de gegevens in de gebeurtenisstroom en het model. Deze sleutel is TSI wordt gebruikt voor het partitioneren van de gegevens.
* *searchSpan*: de duur en bucket grootte voor deze statistische expressie.
* *filter*: Hiermee kunt u predicaat waarden op voor het filteren van uw query's, maar u wilt opgeven.
* *(optioneel) projectedProperties*: kunt kolomeigenschappen filteren.

De API onbewerkte retourneert getEvents waarden uit de vastgelegde gebeurtenissen, zoals opgeslagen in de TSI voor een bepaalde **Time Series-ID** en tijdsbereik. Er is geen variabele definities (TSM noch variabele definities worden gebruikt) vereist.

### <a name="getevents-json-request-and-response-example"></a>aanvraag en antwoord-voorbeeld van JSON getEvents

Gegeven een HTTP POST-aanvraag:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/query?api-version=API_VERSION
```

| Naam | Beschrijving | Voorbeeld |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  De naam van uw omgeving  | `environment123` |
| API_VERSION  |  De API-specificatie | `2018-11-01-preview` |

Met JSON-hoofdtekst:

```JSON
{
    "getEvents": {
        // Required and Supports 3 Key as tsId
        "timeSeriesId": [
            "PU.123","W00158","ABN.9890"
        ],
        // Required.
        "searchSpan": {
            "from": "2018-08-01T17:01:00Z",
            "to": "2018-08-20T17:01:00Z",
        },
        // Optional.
        "filter": {
            "tsx": "($event.Value != null) OR ($event.Status.String == 'Good')"
        },
        // Optional – array of simple TSXs that refer exactly one property
        // If not specified, we will return all the properties.
        "projectedProperties": [ 
            {"propertyName" : "Volts", "type": "double" } 
        ]
    }
}
```

Reactie:

```JSON
{
    "timestamps": [ "2016-08-01T00:00:00Z","2016-08-01T00:01:03Z","2016-08-01T00:02:05Z","2016-08-01T00:03:00Z",
        "2016-08-01T00:04:00Z","2016-08-01T00:05:05Z","2016-08-01T00:06:00Z","2016-08-01T00:07:08Z",
        "2016-08-01T00:08:00Z","2016-08-01T00:09:00Z" ],
    "properties": [
        {
            "name": "Volts",
            "type": "Double",
            "values": [ 68.699982037970059,68.889287593526234,69.089287593526919,69.28928759352759,69.489287593528246,69.689287593528917,69.8892875935296,70.089287593530273,70.289287593530929,70.4892875935316 ]
        }
    ]
}
```

### <a name="supported-query-operators"></a>Ondersteunde query-operators

Filteren:

* `HAS`
* `IN`
* `AND`
* `AND NOT`
* `OR`
* `>`
* `<`
* `<=`
* `>=`
* `!=`
* `<>`
* `/`
* `*`

Tijdelijke:

* `From`
* `To`
* `First/Last` (Alleen-API)

Aggregatiesamenvoeging:

* `MEASURE`
* `SUM` (van een meting)
* `COUNT` (gebeurtenissen)
* `AVG` (van een meting)
* `MIN` (van een meting)
* `MAX` (van een meting) "
* `GROUP BY` (categorische kolom)
* `ORDER BY ASC, DSC` (ten opzichte van de tijdstempel)
* `DateHistogram` (Bucketgrootte)

## <a name="tsq-api-limits"></a>TSQ API-limieten

> [!NOTE]
> De onderstaande tabel bevat de limieten van **11/20/18**.

| Parameter | Limieten |
| --- | --- |
| Aanvraaggrootte TSQ | 32 KB |
| TSQ respons beperken | 16 MB |
| TSQ parallelle query's beperken | 20 per omgeving |
| Gebeurtenissen ophalen | 16 MB's of 30 s tijd |
| Token geldigheid | 1 uur |
| Reeks ophalen | 500.000 intervallen of tijdstempels of 16 MB 's |
| Min. | Interval limiet van 1 ms |
| Max. | 50 verwachte variabelen |
| Cumulatieve-serie | 500.000 intervallen of tijdstempels of 16 MB 's |
| Min. | Interval limiet van 1 ms |
| Max. | 50 verwachte variabelen |

## <a name="time-series-query-tutorial"></a>Time Series-Query-zelfstudie

Een volledige TSQ-zelfstudie wordt in de toekomst worden opgegeven.

[!INCLUDE [tsi-update-docs](../../includes/time-series-insights-update-documents.md)]

## <a name="next-steps"></a>Volgende stappen

Lees de [Azure TSI (preview) Storage en binnenkomende](./time-series-insights-update-storage-ingress.md).

Meer informatie over de nieuwe [Tijdreeksmodel](./time-series-insights-update-tsm.md).

Meer informatie over [aanbevolen procedures bij het kiezen van een Time Series-ID](./time-series-insights-update-how-to-id.md).

<!-- Images -->
[1]: media/v2-update-tsq/goal.png
[2]: media/v2-update-tsq/tsq.png
