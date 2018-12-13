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
ms.date: 12/04/2018
ms.openlocfilehash: 5d5f94aebcd55474385e903246ce7945586456dd
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2018
ms.locfileid: "52890412"
---
# <a name="time-series-model"></a>Time Series-model

Deze informatie over het document de **Tijdreeksmodel** (TSM) deel van de update voor Azure Time Series Insights (TSI). Het beschrijft het model zelf, de mogelijkheden ervan, en om te beginnen het bouwen en het bijwerken van uw eigen model.

Traditioneel, de gegevens die worden verzameld van IoT-apparaten beschikt niet over contextuele informatie waardoor het moeilijk is om te zoeken en sensoren snel analyseren. De belangrijkste reden voor TSM is om te zoeken en analyseren van de IoT-gegevens door in te schakelen curatie, onderhoud en verrijking van time series-gegevens kunt voorbereiden op het gegevenssets gereed is voor consumenten te vereenvoudigen. TSMs spelen een belangrijke rol in query's en navigatie omdat ze context kunnen worden geplaatst apparaat en niet-device-entiteiten. Gegevens persistent gemaakt in TSM bevoegdheden time series-query's berekeningen door gebruik te maken van de formules die erin zijn opgeslagen.

![TSM][1]

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

Met het doel zodat deze eenvoudige en moeiteloze voor het beheren van time series contextualization kunt TSM in de volgende mogelijkheden in de Azure TSI (Preview):

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

Meer informatie over Time Series-Model-typen van de [referentiedocumentatie](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api).

## <a name="variables"></a>Variabelen

Azure TSI typen hebben variabelen, die zijn benoemde berekeningen waarden van de gebeurtenissen. TSI variabele definities bevatten formule en berekeningen regels. Variabele definities bevatten type, waarde, filteren, beperken en grenzen. Variabelen worden opgeslagen in de definitie van het type in TSM en inline via API's voor de onderdrukking van de definitie van de opgeslagen in Query kunnen worden opgegeven.

De onderstaande matrix werkt als een legenda voor definities van de variabele:

![tabel][2]

### <a name="variable-kind"></a>Variabele type

De volgende variabele typen worden ondersteund:

* Numeriek
* Samenstellen

### <a name="variable-filter"></a>Variabele filter

Variabele filters opgeven een optioneel filtercomponent om te beperken van het aantal rijen wordt overwogen voor de berekening op basis van voorwaarden.

### <a name="variable-value"></a>Waarde van variabele

Waarden van variabelen zijn en moeten worden gebruikt in berekeningen. Dit is de kolom in de gebeurtenissen die we naar verwijzen moeten.

### <a name="variable-aggregation"></a>Aggregatie van variabele

De statistische functie van de variabele kan deel uitmaken van de berekening. TSI biedt ondersteuning voor reguliere statistische functies (dat wil zeggen **min**, **max**, **avg**, **som**, en **aantal**).

## <a name="time-series-model-hierarchies"></a>Time Series-Model-hiërarchieën

Hiërarchieën ordenen exemplaren door de namen van eigenschappen en hun relaties op te geven. Mogelijk hebt u een hiërarchie van één of meerdere hiërarchieën. Bovendien deze huidige onderdeel van uw gegevens hoeven niet te worden, maar elk exemplaar moet worden toegewezen aan een hiërarchie. Een exemplaar TSM kunt toewijzen aan een enkele hiërarchie of meerdere hiërarchieën.

Hiërarchieën zijn gedefinieerd door **hiërarchie-ID**, **naam**, en **bron**. Hiërarchieën hebben paden, een pad naar bovenliggende / onderliggende van boven naar beneden volgorde van de hiërarchie van die de gebruiker wil maken. De eigenschappen van de bovenliggende/onderliggende exemplaarvelden toewijzen.

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

Meer informatie over Time Series-Model hiërarchieën uit de [referentiedocumentatie](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api).

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

-Exemplaren zijn de tijdreeksen zelf. In de meeste gevallen de *deviceId* of *assetId* is de unieke id van de activa in de omgeving. -Exemplaren beschikken over beschrijvende informatie die is gekoppeld aan deze instantie-eigenschappen aangeroepen. Ten minste bevatten exemplaareigenschappen informatie van de hiërarchie. Ze kunnen ook nuttige, beschrijvende gegevens, zoals de fabrikant, operator of de laatste servicedatum bevatten.

Exemplaren worden gedefinieerd door *timeSeriesId*, *typeId*, *hierarchyId*, en *instanceFields*. Elke instantie wordt toegewezen aan slechts één *type*, en een of meer hiërarchieën. Exemplaren overnemen alle eigenschappen van hiërarchieën, terwijl meer *instanceFields* kan worden toegevoegd voor verdere definitie van exemplaar-eigenschap.

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

Meer informatie over Time Series-Model instanties uit de [referentiedocumentatie](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api).

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

Meer informatie over Time Series-Model-instellingen van de [referentiedocumentatie](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api).

## <a name="next-steps"></a>Volgende stappen

Lees de [opslag van Azure TSI (Preview) en ingress](./time-series-insights-update-storage-ingress.md).

Lees de over de nieuwe [tijdreeksmodel](https://docs.microsoft.com/rest/api/time-series-insights/preview-model).

<!-- Images -->
[1]: media/v2-update-tsm/tsm.png
[2]: media/v2-update-tsm/table.png
