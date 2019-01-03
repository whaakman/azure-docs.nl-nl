---
title: Voorbeeld van Time Series-Model in Azure Time Series Insights | Microsoft Docs
description: Understanding Azure Time Series Insights Time Series-Model.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: a6eb142cf607c286ccce1282bb5c67a30c040c62
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53716579"
---
# <a name="time-series-model"></a>Time Series-model

Dit artikel beschrijft het deel van de Time Series-Model van Azure Time Series Insights Preview. Hierin worden het model zelf, de mogelijkheden en hoe u aan de slag bouwen en het bijwerken van uw eigen model.

Traditioneel, de gegevens die worden verzameld van IoT-apparaten beschikt niet over contextuele informatie, waardoor het moeilijk zijn te vinden en sensoren snel analyseren. De belangrijkste reden voor Time Series-Model is om te zoeken en analyseren van de IoT-gegevens te vereenvoudigen. Dit doel bereikt door in te schakelen de curatie, onderhoud en verrijking van time series-gegevens kunt voorbereiden op consumenten gegevenssets. 

Time Series-modellen spelen een belangrijke rol in query's en navigatie omdat ze context kunnen worden geplaatst apparaat en niet-device-entiteiten. Gegevens die in Time Series-Model heeft persistente wordt gebruikt door time series query berekeningen door te profiteren van de formules die erin zijn opgeslagen.

![TSM][1]

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

Met het doel zodat deze eenvoudige en moeiteloze voor het beheren van time series contextualization kunt Time Series-Model de volgende mogelijkheden in Time Series Insights Preview. Hiermee kunt u:

* Ontwerpen en beheren van berekeningen of formules, transformeer gegevens gebruik te maken van de scalaire functies, statistische bewerkingen en enzovoort.

* Relaties tussen bovenliggende-onderliggende navigatie en verwijzing inschakelen en een context bieden voor time series telemetrie definiëren.

* Eigenschappen die gekoppeld aan het gedeelte van de exemplaren van zijn definiëren *exemplaar velden* en gebruiken voor het maken van hiërarchieën.

## <a name="times-series-model-key-components"></a>Time-out van de belangrijke onderdelen Series-Model

Time Series-Model heeft drie primaire onderdelen:

* Time Series-Model *typen*
* Time Series-Model *hiërarchieën*
* Time Series-Model *exemplaren*

## <a name="time-series-model-types"></a>Typen van Time Series-Model

Time Series-Model *typen* helpen u bij het definiëren van variabelen of formules voor het uitvoeren van berekeningen. De typen zijn gekoppeld aan een specifieke Time Series Insights-exemplaar. Een type kan een of meer variabelen hebben. Bijvoorbeeld, een Time Series Insights-exemplaar van het type mogelijk *temperatuursensor*, die bestaat uit de variabelen *gemiddelde temperatuur*, *min temperatuur*, en *max temperatuur*. We maken een standaardtype zodra de gegevens doorgestuurd naar de Time Series Insights. Het standaardtype kan worden opgehaald en bijgewerkt van modelinstellingen. Standaardtypen hebben een variabele die het aantal gebeurtenissen telt.

## <a name="time-series-model-type-json-example"></a>Voorbeeld van Time Series-Model type JSON

Voorbeeld:

```JSON
{
    "name": "SampleType",
    "description": "This is sample type",
    "variables": {
        "Avg Temperature": {
            "kind": "numeric",
            "filter": null,
            "value": { "tsx": "$event.temperature.Double" },
            "aggregation": {"tsx": "avg($value)"}
        },
        "Count Temperature": {
            "kind": "aggregate",
            "filter": null,
            "value": null,
            "aggregation": {"tsx": "count()"}
        }
    }
}
``````

Zie voor meer informatie over de typen van Time Series-Model, de [referentiedocumentatie](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api).

## <a name="variables"></a>Variabelen

Time Series Insights typen hebben variabelen, die zijn benoemde berekeningen waarden van de gebeurtenissen. Time Series Insights variabele definities bevatten formule en berekeningen regels. Variabele definities bevatten *soort*, *waarde*, *filter*, *vermindering*, en *grenzen*. Variabelen worden opgeslagen in het definitie in Time Series-Model en inline via API's voor de onderdrukking van de definitie van de opgeslagen in Query kunnen worden opgegeven.

De volgende matrix werkt als een legenda voor definities van de variabele:

![tabel][2]

### <a name="variable-kind"></a>Variabele type

De volgende variabele typen worden ondersteund:

* *numerieke*
* *Statistische functie*

### <a name="variable-filter"></a>Variabele filter

Variabele filters opgeven een optioneel filtercomponent om te beperken van het aantal rijen wordt overwogen voor de berekening op basis van voorwaarden.

### <a name="variable-value"></a>Waarde van variabele

Waarden van variabelen zijn en moeten worden gebruikt in berekeningen. Dit is de kolom in de gebeurtenissen die we naar verwijzen moeten.

### <a name="variable-aggregation"></a>Aggregatie van variabele

De statistische functie van de variabele kan deel uitmaken van de berekening. Time Series Insights biedt ondersteuning voor reguliere statistische functies (dat wil zeggen *min*, *max*, *avg*, *som*, en *aantal*).

## <a name="time-series-model-hierarchies"></a>Time Series-Model-hiërarchieën

Hiërarchieën ordenen exemplaren door de namen van eigenschappen en hun relaties op te geven. Mogelijk hebt u een hiërarchie van één of meerdere hiërarchieën. Niet hoeven te worden van een huidige deel van uw gegevens, maar elk exemplaar moet worden toegewezen aan een hiërarchie. Een exemplaar van de Time Series-Model kunt toewijzen aan een enkele hiërarchie of meerdere hiërarchieën.

Hiërarchieën zijn gedefinieerd door *hiërarchie-ID*, *naam*, en *bron*. Hiërarchieën hebben een pad, dit is een bovenliggende / onderliggende van boven naar beneden volgorde van de hiërarchie die gebruikers wilt maken. De toewijzing van de eigenschappen van bovenliggende / onderliggende *exemplaar velden*.

### <a name="time-series-model-hierarchy-json-example"></a>Voorbeeld van Time Series-Model hiërarchie JSON

Voorbeeld:

```JSON
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
```

Zie voor meer informatie over Time Series-Model hiërarchieën, de [referentiedocumentatie](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api).

### <a name="hierarchy-definition-behavior"></a>Gedrag voor hiërarchie-definitie

Houd rekening met het volgende voorbeeld waarbij hiërarchie H1 heeft *bouwen*, *floor*, en *ruimte* als onderdeel van de definitie ervan:

```plaintext
 H1 = [“building”, “floor”, “room”]
```

Afhankelijk van de *exemplaar velden*, de hiërarchie-kenmerken en waarden worden weergegeven zoals in de volgende tabel:

| Tijdreeks-id | Exemplaarvelden |
| --- | --- |
| ID1 | "bouwen" = "1000", "floor" = "10", 'ruimte' = "55"  |
| ID2 | "bouwen" = "1000", 'ruimte' = "55" |
| ID3 | "floor" = "10" |
| ID4 | "bouwen" = "1000", "floor" = "10"  |
| ID5 | Geen van de "bouwen", 'floor' of 'lokaal' is ingesteld |

In het voorgaande voorbeeld ID1 en ID4 weergegeven als onderdeel van een hiërarchie H1 in Azure Time Series Insights explorer en de rest worden geclassificeerd onder *niet-bovenliggende exemplaren* omdat ze niet aan de opgegeven gegevens-hiërarchie voldoen.

## <a name="time-series-model-instances"></a>Time Series-Model exemplaren

-Exemplaren zijn de tijdreeksen zelf. In de meeste gevallen de *deviceId* of *assetId* is de unieke id van de activa in de omgeving. -Exemplaren beschikken over beschrijvende informatie die is gekoppeld aan deze instantie-eigenschappen aangeroepen. Ten minste bevatten exemplaareigenschappen informatie van de hiërarchie. Ze kunnen ook nuttige, beschrijvende gegevens, zoals de fabrikant, operator of de laatste servicedatum bevatten.

Exemplaren worden gedefinieerd door *timeSeriesId*, *typeId*, *hierarchyId*, en *instanceFields*. Elke instantie wordt toegewezen aan slechts één *type*, en een of meer hiërarchieën. Exemplaren nemen alle eigenschappen van hiërarchieën, en aanvullende *instanceFields* kan worden toegevoegd voor verdere definitie van exemplaar-eigenschap.

*instanceFields* zijn eigenschappen van een exemplaar en alle statische gegevens waarmee een exemplaar. Deze definiëren waarden van eigenschappen of niet-hiërarchie terwijl het tevens ondersteunt indexeren om uit te voeren van zoekbewerkingen.

## <a name="time-series-model-instance-json-example"></a>Voorbeeld van Time Series-Model exemplaar JSON

Voorbeeld:

```JSON
{
    "typeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393",
    "timeSeriesId": ["sampleTimeSeriesId"],
    "description": "Sample Instance",
    "hierarchyIds": [
        "1643004c-0a84-48a5-80e5-7688c5ae9295"
    ],
    "instanceFields": {
        "state": "California",
        "city": "Los Angeles"
    }
}
```

Zie voor meer informatie over exemplaren van de Time Series-Model, de [referentiedocumentatie](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api).

## <a name="time-series-model-settings-example"></a>Voorbeeld van instellingen voor Time Series-Model

Voorbeeld:

```JSON
{
    "modelSettings": {
        "name": "DefaultModel",
        "timeSeriesIdProperties": [
            {
                "name": "id",
                "type": "String"
            }
        ],
        "defaultTypeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393"
    }
}
```

Zie voor meer informatie over instellingen voor Time Series-Model, de [referentiedocumentatie](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api).

## <a name="next-steps"></a>Volgende stappen

Zie [Azure Time Series Insights Preview opslag- en uitgangsclaims](./time-series-insights-update-storage-ingress.md).

Zie de nieuwe [Tijdreeksmodel](https://docs.microsoft.com/rest/api/time-series-insights/preview-model).

<!-- Images -->
[1]: media/v2-update-tsm/tsm.png
[2]: media/v2-update-tsm/table.png
