---
title: Configureerbare drempelwaarde op basis van regels in Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u van referentiegegevens voor een oplossing voor waarschuwingen met configureerbare drempelwaarde op basis van regels in Azure Stream Analytics.
services: stream-analytics
author: zhongc
ms.author: zhongc
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/30/2018
ms.openlocfilehash: ce2cf6ebdfd74549114e94e4c7356e387576d3c8
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56731188"
---
# <a name="process-configurable-threshold-based-rules-in-azure-stream-analytics"></a>Verwerken configureerbare drempelwaarde-regels in Azure Stream Analytics
In dit artikel wordt beschreven hoe u van referentiegegevens voor een waarschuwingen oplossing die gebruikmaakt van configureerbare regels op basis van een drempelwaarde in Azure Stream Analytics.

## <a name="scenario-alerting-based-on-adjustable-rule-thresholds"></a>Scenario: Waarschuwingen op basis van aanpasbare regel drempelwaarden
U moet mogelijk een waarschuwing geproduceerd als uitvoer als een bepaalde waarde hebt bereikt van het binnenkomende gestreamde gebeurtenissen, of als een geaggregeerde waarde op basis van de binnenkomende gestreamde gebeurtenissen een bepaalde drempelwaarde overschrijdt. Het eenvoudig voor het instellen van een Stream Analytics-query die in vergelijking met een statische drempelwaarde die is vastgesteld en vooraf ingestelde waarde. Een vaste drempelwaarde kan worden vastgelegd in de streaming-querysyntaxis met behulp van eenvoudige numerieke vergelijkingen (groter is dan, kleiner dan en gelijkheid).

In sommige gevallen moeten de drempelwaarden zijn eenvoudiger kunnen worden geconfigureerd zonder het bewerken van de querysyntaxis van de elke keer dat een waarde voor drempel wordt gewijzigd. In andere gevallen moet u mogelijk talloze apparaten of gebruikers die zijn verwerkt door de dezelfde query aan elk van deze met een verschillende drempelwaarden op elk type apparaat. 

Dit patroon kan worden gebruikt om dynamisch drempels, selectief kiezen welk type apparaat dat de drempelwaarde wordt toegepast door te filteren de invoergegevens en selectief kiezen welke velden u wilt opnemen in de uitvoer.

## <a name="recommended-design-pattern"></a>Aanbevolen ontwerp
Gebruik een referentie-invoer voor gegevens naar een Stream Analytics-taak als een opzoeken van de drempelwaarden:
- De drempelwaarden Store in de referentiegegevens, één waarde per sleutel.
- De invoer gebeurtenissen van streaming gegevens toevoegen aan de referentiegegevens op de sleutelkolom.
- De versleutelde waarde van de referentiegegevens gebruiken als de drempelwaarde.

## <a name="example-data-and-query"></a>Van de voorbeeldgegevens en query's uitvoeren
In het voorbeeld worden de waarschuwingen worden gegenereerd wanneer de statistische functie van de gegevens streaming van apparaten in een minuut lang venster overeenkomt met de vastgelegde waarden in de regel die is opgegeven als referentiegegevens.

In de query voor elk apparaat-id en elke metricName onder de apparaat-id, kunt u configureren van 0 tot 5 dimensies voor GROUP BY. Alleen de gebeurtenissen met de overeenkomende filterwaarden zijn gegroepeerd. Eenmaal gegroepeerd, worden statistische functies in vensters, Min, Max, Gem, berekend over een tumblingvenster 60 seconden. Filters op de geaggregeerde waarden worden vervolgens berekend volgens de geconfigureerde drempelwaarde in de verwijzing voor het genereren van de waarschuwing uitvoergebeurtenis.

Als voorbeeld wordt ervan uitgegaan er is een Stream Analytics-taak waarvoor een verwijzing invoer met de naam **regels**, en het streamen van gegevensinvoer met de naam **metrische gegevens**. 

## <a name="reference-data"></a>Referentiegegevens
De referentiegegevens voor dit voorbeeld laat zien hoe een regel op basis van een drempelwaarde kan worden weergegeven. Een JSON-bestand bevat de referentiegegevens en wordt opgeslagen in Azure blob-opslag en die blob storage-container wordt gebruikt als een verwijzing invoer met de naam **regels**. U kunt deze JSON-bestand overschrijven en de regelconfiguratie vervangen als de tijd verstrijkt, zonder te stoppen of starten van de streaming-taak.

- De voorbeeldregel wordt gebruikt voor een aanpasbare waarschuwing wanneer CPU overschrijdt (gemiddelde is groter dan of gelijk aan) de waarde `90` procent. De `value` veld kan worden geconfigureerd als nodig is.
- U ziet de regel is een **operator** veld, die wordt dynamisch geïnterpreteerd in de query-syntaxis later `AVGGREATEROREQUAL`. 
- De regel filtert de gegevens op een bepaalde dimensie-sleutel `2` met waarde `C1`. Andere velden zijn lege tekenreeks is, waarmee wordt aangegeven dat niet de invoerstroom van die gebeurtenis velden filteren. U kan extra CPU-regels instellen voor het filteren van andere velden indien nodig.
- Niet alle kolommen zijn moeten worden opgenomen in de waarschuwing uitvoergebeurtenis. In dit geval `includedDim` sleutel nummer `2` is ingeschakeld op `TRUE` om weer te geven dat Veldnummer 2 van de gegevens van de gebeurtenis in de stroom worden opgenomen in de uitvoergebeurtenissen die in aanmerking komend. De andere velden zijn niet opgenomen in de uitvoer van de waarschuwing, maar de lijst met velden kan worden aangepast.


```json
{
    "ruleId": 1234, 
    "deviceId" : "978648", 
    "metricName": "CPU", 
    "alertName": "hot node AVG CPU over 90",
    "operator" : "AVGGREATEROREQUAL",
    "value": 90, 
    "includeDim": {
        "0": "FALSE", 
        "1": "FALSE", 
        "2": "TRUE", 
        "3": "FALSE", 
        "4": "FALSE"
    },
    "filter": {
        "0": "", 
        "1": "",
        "2": "C1", 
        "3": "", 
        "4": ""
    }    
}
```

## <a name="example-streaming-query"></a>Voorbeeldquery voor streaming
In dit voorbeeld van de Stream Analytics-query lid wordt van de **regels** verwijzen naar gegevens uit het voorbeeld hierboven, naar een invoerstroom van gegevens met de naam **metrische gegevens**.

```sql
WITH transformedInput AS
(
    SELECT
        dim0 = CASE rules.includeDim.[0] WHEN 'TRUE' THEN metrics.custom.dimensions.[0].value ELSE NULL END,
        dim1 = CASE rules.includeDim.[1] WHEN 'TRUE' THEN metrics.custom.dimensions.[1].value ELSE NULL END,
        dim2 = CASE rules.includeDim.[2] WHEN 'TRUE' THEN metrics.custom.dimensions.[2].value ELSE NULL END,
        dim3 = CASE rules.includeDim.[3] WHEN 'TRUE' THEN metrics.custom.dimensions.[3].value ELSE NULL END,
        dim4 = CASE rules.includeDim.[4] WHEN 'TRUE' THEN metrics.custom.dimensions.[4].value ELSE NULL END,
        metric = metrics.metric.value,
        metricName = metrics.metric.name,
        deviceId = rules.deviceId, 
        ruleId = rules.ruleId, 
        alertName = rules.alertName,
        ruleOperator = rules.operator, 
        ruleValue = rules.value
    FROM 
        metrics
        timestamp by eventTime
    JOIN 
        rules
        ON metrics.deviceId = rules.deviceId AND metrics.metric.name = rules.metricName
    WHERE
        (rules.filter.[0] = '' OR metrics.custom.filters.[0].value = rules.filter.[0]) AND 
        (rules.filter.[1] = '' OR metrics.custom.filters.[1].value = rules.filter.[1]) AND
        (rules.filter.[2] = '' OR metrics.custom.filters.[2].value = rules.filter.[2]) AND
        (rules.filter.[3] = '' OR metrics.custom.filters.[3].value = rules.filter.[3]) AND
        (rules.filter.[4] = '' OR metrics.custom.filters.[4].value = rules.filter.[4])
)

SELECT
    System.Timestamp as time, 
    transformedInput.deviceId as deviceId,
    transformedInput.ruleId as ruleId,
    transformedInput.metricName as metric,
    transformedInput.alertName as alert,
    AVG(metric) as avg,
    MIN(metric) as min, 
    MAX(metric) as max, 
    dim0, dim1, dim2, dim3, dim4
FROM
    transformedInput
GROUP BY
    transformedInput.deviceId,
    transformedInput.ruleId,
    transformedInput.metricName,
    transformedInput.alertName,
    dim0, dim1, dim2, dim3, dim4,
    ruleOperator, 
    ruleValue, 
    TumblingWindow(second, 60)
HAVING
    (
        (ruleOperator = 'AVGGREATEROREQUAL' AND avg(metric) >= ruleValue) OR
        (ruleOperator = 'AVGEQUALORLESS' AND avg(metric) <= ruleValue) 
    )
```

## <a name="example-streaming-input-event-data"></a>Voorbeeld van invoer streaminggegevens
In dit voorbeeld-JSON-gegevens vertegenwoordigt de **metrische gegevens** invoergegevens die wordt gebruikt in de bovenstaande streaming query. 

- Drie voorbeelden van gebeurtenissen worden weergegeven binnen het interval van 1 minuut, waarde `T14:50`. 
- Alle drie hebben dezelfde `deviceId` waarde `978648`.
- De metrische waarden van CPU variëren in elke gebeurtenis `98`, `95`, `80` respectievelijk. Alleen de eerste twee voorbeelden van gebeurtenissen langer zijn dan de CPU-waarschuwingsregel tot stand gebracht in de regel.
- Het veld includeDim in de waarschuwingsregel is nummer sleutel 2. De bijbehorende sleutel 2-veld in de voorbeeld-gebeurtenissen met de naam `NodeName`. De gebeurtenissen voorbeeld van de drie waarden hebben `N024`, `N024`, en `N014` respectievelijk. In de uitvoer ziet u alleen het knooppunt `N024` is de enige gegevens die overeenkomt met de waarschuwingscriteria van hoge CPU. `N014` voldoet niet aan de hoge CPU-drempelwaarde.
- De waarschuwingsregel is geconfigureerd met een `filter` alleen op het nummer van 2, die overeenkomt met de `cluster` veld in de voorbeeldgebeurtenissen. De drie voorbeeld van alle gebeurtenissen hebben die waarde `C1` en overeenkomen met de filtercriteria.

```json
{
    "eventTime": "2018-04-30T14:50:23.1324132Z",
    "deviceId": "978648",
    "custom": {
        "dimensions": {
            "0": {
                "name": "NodeType",
                "value": "N1"
            },
            "1": {
                "name": "Cluster",
                "value": "C1"
            },
            "2": {
                "name": "NodeName",
                "value": "N024"
            }
        },
        "filters": {
            "0": {
                "name": "application",
                "value": "A1"
            },
            "1": {
                "name": "deviceType",
                "value": "T1"
            },
            "2": {
                "name": "cluster",
                "value": "C1"
            },
            "3": {
                "name": "nodeType",
                "value": "N1"
            }
        }
    },
    "metric": {
        "name": "CPU",
        "value": 98,
        "count": 1.0,
        "min": 98,
        "max": 98,
        "stdDev": 0.0
    }
}
{
    "eventTime": "2018-04-30T14:50:24.1324138Z",
    "deviceId": "978648",
    "custom": {
        "dimensions": {
            "0": {
                "name": "NodeType",
                "value": "N2"
            },
            "1": {
                "name": "Cluster",
                "value": "C1"
            },
            "2": {
                "name": "NodeName",
                "value": "N024"
            }
        },
        "filters": {
            "0": {
                "name": "application",
                "value": "A1"
            },
            "1": {
                "name": "deviceType",
                "value": "T1"
            },
            "2": {
                "name": "cluster",
                "value": "C1"
            },
            "3": {
                "name": "nodeType",
                "value": "N2"
            }
        }
    },
    "metric": {
        "name": "CPU",
        "value": 95,
        "count": 1,
        "min": 95,
        "max": 95,
        "stdDev": 0
    }
}
{
    "eventTime": "2018-04-30T14:50:37.1324130Z",
    "deviceId": "978648",
    "custom": {
        "dimensions": {
            "0": {
                "name": "NodeType",
                "value": "N3"
            },
            "1": {
                "name": "Cluster",
                "value": "C1 "
            },
            "2": {
                "name": "NodeName",
                "value": "N014"
            }
        },
        "filters": {
            "0": {
                "name": "application",
                "value": "A1"
            },
            "1": {
                "name": "deviceType",
                "value": "T1"
            },
            "2": {
                "name": "cluster",
                "value": "C1"
            },
            "3": {
                "name": "nodeType",
                "value": "N3"
            }
        }
    },
    "metric": {
        "name": "CPU",
        "value": 80,
        "count": 1,
        "min": 80,
        "max": 80,
        "stdDev": 0
    }
}
```

## <a name="example-output"></a>Voorbeeld van uitvoer
In dit voorbeeld van uitvoer in JSON-gegevens bevat één waarschuwing gebeurtenis is gemaakt op basis van de CPU-drempel regel gedefinieerd in de referentiegegevens. De uitvoergebeurtenis bevat de naam van de waarschuwing, evenals de samengevoegde (gemiddelde, min, max) van de velden beschouwd. De gegevens van de uitvoer-gebeurtenis bevat belangrijke Veldnummer 2 `NodeName` waarde `N024` vanwege de regelconfiguratie. (De JSON is gewijzigd om weer te geven regeleinden voor de leesbaarheid.)

```JSON
{"time":"2018-05-01T02:03:00.0000000Z","deviceid":"978648","ruleid":1234,"metric":"CPU",
"alert":"hot node AVG CPU over 90","avg":96.5,"min":95.0,"max":98.0,
"dim0":null,"dim1":null,"dim2":"N024","dim3":null,"dim4":null}
```