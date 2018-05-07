---
title: Proces configureerbare drempelwaarde bereikt op basis van regels in Azure Stream Analytics
description: In dit artikel wordt beschreven hoe een meldingsoplossing met configureerbare drempelwaarde bereikt op basis van regels in Azure Stream Analytics kunt gebruiken voor referentiegegevens.
services: stream-analytics
author: zhongc
ms.author: zhongc
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/30/2018
ms.openlocfilehash: 1c131c2c9ca12556c1d2cd52e7976d2f4272a0c8
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="process-configurable-threshold-based-rules-in-azure-stream-analytics"></a>Processen kunnen worden geconfigureerd op basis van drempelwaarden regels in Azure Stream Analytics
In dit artikel wordt beschreven hoe een meldingsoplossing die gebruikmaakt van de configureerbare regels op basis van drempelwaarden in Azure Stream Analytics kunt gebruiken voor referentiegegevens.

## <a name="scenario-alerting-based-on-adjustable-rule-thresholds"></a>Scenario: Waarschuwingen op basis van aanpasbare regel drempelwaarden
Mogelijk moet u een waarschuwing opleveren als uitvoer wanneer binnenkomende gestreamde gebeurtenissen een bepaalde waarde hebt bereikt, of wanneer een cumulatieve waarde op basis van de binnenkomende gestreamde gebeurtenissen een bepaalde drempelwaarde overschrijdt. Het eenvoudig voor het instellen van een Stream Analytics query die een statische drempelwaarde die is vast en vooraf ingestelde waarde vergeleken. Een vaste drempelwaarde mag hard gecodeerd in de streaming-querysyntaxis met eenvoudige numerieke vergelijkingen (groter dan, kleiner dan en gelijkheid).

In sommige gevallen moeten de drempelwaarden worden eenvoudiger geconfigureerd zonder te bewerken van de querysyntaxis telkens wanneer een drempelwaarde wordt gewijzigd. In andere gevallen moet u mogelijk meerdere apparaten of gebruikers die zijn verwerkt door dezelfde query met elk van deze hebben een verschillende drempelwaarden voor elk type apparaat. 

Dit patroon kan worden gebruikt voor dynamisch drempels, selectief kiezen welk type apparaat dat de drempelwaarde van toepassing is door het filteren van de invoergegevens en selectief kiezen welke velden moeten worden opgenomen in de uitvoer.

## <a name="recommended-design-pattern"></a>Aanbevolen ontwerp
De gegevensinvoer van een verwijzing naar een Stream Analytics-taak als een opzoeken van de drempelwaarden voor waarschuwingen gebruiken:
- De drempelwaarden in de referentiegegevens, één waarde per sleutel opslaan.
- Voeg de invoervelden streaming gegevens toe aan de verwijzing naar gegevens op de sleutelkolom.
- Gebruik de versleutelde waarde van de referentiegegevens als de drempelwaarde.

## <a name="example-data-and-query"></a>Van de voorbeeldgegevens en de query
In het voorbeeld worden de waarschuwingen worden gegenereerd wanneer de statistische functie van gegevens streaming van apparaten in een minuut lang venster overeenkomt met de vastgelegde waarden in de regel die is opgegeven voor referentiegegevens.

In de query voor elk apparaat-id en elke metricName onder de apparaat-id, kunt u configureren van 0 5 dimensies op GROUP BY. Alleen de gebeurtenissen met de overeenkomende filterwaarden zijn gegroepeerd. Eenmaal gegroepeerd, worden functies in vensters van Min, Max, Avg, berekend over een tumblingvenster 60 seconden. Filters uit de geaggregeerde waarden worden vervolgens berekend volgens de geconfigureerde drempelwaarde in de documentatie, voor het genereren van de waarschuwing uitvoergebeurtenis.

Als een voorbeeld wordt ervan uitgegaan er is een Stream Analytics-taak met een verwijzing naar gegevensinvoer met de naam **regels**, en streaming invoer met de naam **metrische gegevens**. 

## <a name="reference-data"></a>Referentiegegevens
De referentiegegevens voor dit voorbeeld ziet u hoe een regel op basis van drempelwaarden kan worden weergegeven. Een JSON-bestand bevat de referentiegegevens en opgeslagen in Azure blob-opslag en die blob storage-container wordt gebruikt als een verwijzing naar gegevensinvoer met de naam **regels**. U kan dit JSON-bestand overschrijven en de regelconfiguratie vervangen als de tijd verstrijkt, zonder stoppen of starten van de streaming-taak.

- De voorbeeldregel wordt gebruikt voor een aanpasbare waarschuwing wanneer CPU overschrijdt (gemiddelde is groter dan of gelijk aan) de waarde `90` procent. De `value` veld configureerbare zo nodig is.
- Let op de regel bevat een **operator** veld dat wordt dynamisch geïnterpreteerd in de querysyntaxis later op `AVGGREATEROREQUAL`. 
- De regel filtert u de gegevens op een bepaalde dimensie sleutel `2` met waarde `C1`. Andere velden zijn leeg, waarmee wordt aangegeven dat niet de invoerstroom door deze velden gebeurtenis filteren. U kan regels instellen om extra CPU voor het filteren van andere overeenkomende velden indien nodig.
- Niet alle kolommen moeten worden opgenomen in de uitvoer waarschuwing gebeurtenis. In dit geval `includedDim` sleutel nummer `2` is ingeschakeld `TRUE` om aan te duiden dat gebeurtenissen dat Veldnummer 2 van de gegevens van gebeurtenissen in de stroom worden opgenomen in de in aanmerking komende uitvoer. De andere velden worden niet opgenomen in de uitvoer van de waarschuwing, maar de lijst met velden kan worden aangepast.


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
In dit voorbeeld Stream Analytics query koppelt de **regels** verwijzen naar gegevens uit het bovenstaande voorbeeld met een invoer stroom van gegevens met de naam **metrische gegevens**.

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

## <a name="example-streaming-input-event-data"></a>Voorbeeld streaming invoer-gebeurtenisgegevens
Hiermee geeft u de JSON-gegevens van dit voorbeeld de **metrische gegevens** invoergegevens die in de bovenstaande streaming query wordt gebruikt. 

- Voorbeeld van drie gebeurtenissen worden vermeld in de waarde van het timespan 1 minuut `T14:50`. 
- Alle drie hebben dezelfde `deviceId` waarde `978648`.
- De CPU metrische waarden variëren in elke gebeurtenis `98`, `95`, `80` respectievelijk. Alleen de eerste twee voorbeelden van gebeurtenissen groter zijn dan de CPU-waarschuwingsregel tot stand gebracht in de regel.
- Het veld includeDim in de waarschuwingsregel is sleutel nummer 2. De naam van het betreffende veld sleutel 2 in de voorbeeld-gebeurtenissen `NodeName`. De gebeurtenissen voorbeeld van de drie waarden hebben `N024`, `N024`, en `N014` respectievelijk. In de uitvoer ziet u alleen het knooppunt `N024` is de enige gegevens die overeenkomt met de criteria voor prestatiewaarschuwingen voor intensief CPU. `N014` niet voldoet aan de hoge drempelwaarde voor de CPU.
- De waarschuwingsregel is geconfigureerd met een `filter` alleen op nummer sleutel 2, wat overeenkomt met de `cluster` veld in de voorbeeld-gebeurtenissen. Waarde van de drie voorbeeld van alle gebeurtenissen hebben `C1` en overeenkomen met de filtercriteria.

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
In dit voorbeeld uitvoer JSON-gegevens bevat één waarschuwing gebeurtenis is gemaakt op basis van een regel voor de CPU drempel gedefinieerd in de referentiegegevens. De uitvoergebeurtenis bevat de naam van de waarschuwing, evenals de geaggregeerde (gemiddelde, min, max) van de velden beschouwd. De gebeurtenis uitvoergegevens bevat belangrijke Veldnummer 2 `NodeName` waarde `N024` vanwege de regelconfiguratie van de. (De JSON is gewijzigd om aan te tonen regeleinden voor de leesbaarheid.)

```JSON
{"time":"2018-05-01T02:03:00.0000000Z","deviceid":"978648","ruleid":1234,"metric":"CPU",
"alert":"hot node AVG CPU over 90","avg":96.5,"min":95.0,"max":98.0,
"dim0":null,"dim1":null,"dim2":"N024","dim3":null,"dim4":null}
```