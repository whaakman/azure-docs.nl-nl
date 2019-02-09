---
title: Tumblingvenstertrigger venster trigger afhankelijkheden maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van afhankelijkheid op een tumblingvenstertrigger in Azure Data Factory.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: douglasl
ms.openlocfilehash: c51c1056869cbd7741fae2ed1a554a7c794d1a39
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967212"
---
# <a name="create-a-tumbling-window-trigger-dependency"></a>Een tumblingvenstertrigger venster trigger afhankelijkheid maken

Dit artikel bevat stappen voor het maken van een afhankelijkheid op een tumblingvenstertrigger. Raadpleeg voor algemene informatie over tumblingvenstertriggers [over het maken van tumblingvenstertrigger](how-to-create-tumbling-window-trigger.md).

Als u wilt een afhankelijkheidsketen voor bouwen en zorg ervoor dat een trigger wordt uitgevoerd alleen na de voltooiing van uitvoering van een andere trigger in de data factory, deze geavanceerde functie te gebruiken om te maken van een tumblingvenstertrigger venster afhankelijkheid.

## <a name="create-a-dependency-in-the-data-factory-ui"></a>Een afhankelijkheid in de gebruikersinterface van Data Factory maken

Selecteer voor het maken van afhankelijkheid voor een trigger, **Trigger > Geavanceerd > nieuw**, en kies vervolgens de trigger met de juiste verschuiving en de grootte afhankelijk. Selecteer **voltooien** en publiceren van de data factory-wijzigingen voor de afhankelijkheden van kracht.

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency01.png)

## <a name="tumbling-window-dependency-properties"></a>Tumblingvenstertrigger venster afhankelijkheidseigenschappen

Een tumblingvenstertrigger venster trigger-afhankelijkheid heeft de volgende eigenschappen:

```json
{
    "name": "DemoTrigger",
    "properties": {
        "runtimeState": "Started",
        "pipeline": {
            "pipelineReference": {
                "referenceName": "Demo",
                "type": "PipelineReference"
            }
        },
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2018-10-04T00:00:00.000Z",
            "delay": "00:00:00",
            "maxConcurrency": 50,
            "retryPolicy": {
                "intervalInSeconds": 30
            },
            "dependsOn": [
                {
                    "type": "TumblingWindowTriggerDependencyReference",
                    "size": "-02:00:00",
                    "offset": "-01:00:00",
                    "referenceTrigger": {
                        "referenceName": "DemoDependency1",
                        "type": "TriggerReference"
                    }
                }
            ]
        }
    }
}
```

De volgende tabel bevat de lijst met kenmerken die nodig zijn voor het definiëren van een afhankelijkheid Tumblingvenster.

| **De naam van eigenschap** | **Beschrijving**  | **Type** | **Vereist** |
|---|---|---|---|
| Trigger  | Alle de bestaande tumblingvenstertriggers worden weergegeven in deze vervolgkeuzelijst omlaag. Kies de trigger afhankelijkheid ondernemen.  | TumblingWindowTrigger | Ja |
| Offset | Verschuiving van de trigger afhankelijkheid. Geef een waarde in de reeks tijdnotatie en zowel negatieve als positieve verschuivingen zijn toegestaan. Deze parameter is verplicht dat als de trigger afhankelijk van het zelf en in alle andere gevallen is is optioneel. Self-afhankelijkheid moet altijd een negatieve offset. | Periode | Self-afhankelijkheid: Ja andere: Nee |
| Venstergrootte | Grootte van de afhankelijkheid tumblingvenster. Geef een waarde in de reeks tijdnotatie. Deze parameter is optioneel. | Periode | Nee  |
|||||

## <a name="tumbling-window-self-dependency-properties"></a>Eigenschappen van tumblingvenstertrigger venster self-afhankelijkheid

In de scenario's waarbij de trigger moet niet doorgaan naar volgende totdat de vorige venster is voltooid, bouwt u een self-afhankelijkheid. Self-afhankelijkheid trigger hebben onder eigenschappen:

```json
{
    "name": "DemoSelfDependency",
    "properties": {
        "runtimeState": "Started",
        "pipeline": {
            "pipelineReference": {
                "referenceName": "Demo",
                "type": "PipelineReference"
            }
        },
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2018-10-04T00:00:00Z",
            "delay": "00:01:00",
            "maxConcurrency": 50,
            "retryPolicy": {
                "intervalInSeconds": 30
            },
            "dependsOn": [
                {
                    "type": "SelfDependencyTumblingWindowTriggerReference",
                    "size": "01:00:00",
                    "offset": "-01:00:00"
                }
            ]
        }
    }
}
```

Hieronder vindt u in de afbeeldingen van de scenario's en het gebruik van tumblingvenster afhankelijkheidseigenschappen.

## <a name="dependency-offset"></a>Verschuiving van de afhankelijkheid

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency02.png)

## <a name="dependency-size"></a>Grootte van de afhankelijkheid

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency03.png)

## <a name="self-dependency"></a>Self-afhankelijkheid

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency04.png)

## <a name="usage-scenarios"></a>Gebruiksscenario's

### <a name="dependency-on-another-tumbling-window-trigger"></a>Afhankelijkheid van een andere tumblingvenstertrigger

Bijvoorbeeld, een dagelijkse taak van de verwerking van telemetrie, afhankelijk van een andere dagelijkse taak aggregeren van de afgelopen zeven dagen uitvoer en zeven dagen rolling venster streams genereert:

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency05.png)

### <a name="dependency-on-itself"></a>Afhankelijkheid van zelf

Een dagelijkse taak zonder hiaten in de uitvoerstromen van de taak:

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency06.png)

## <a name="monitor-dependencies"></a>Afhankelijkheden controleren

U kunt afhankelijkheidsketen bewaken en de bijbehorende windows van de trigger uitgevoerd pagina. Navigeer naar **bewaking > activeren uitvoeringen**.

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency07.png)

Klik op het glas gezocht naar om alle afhankelijke van het geselecteerde venster triggeruitvoeringen weer te geven.

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency08.png)

## <a name="next-steps"></a>Volgende stappen

Beoordeling [over het maken van een tumblingvenstertrigger](how-to-create-tumbling-window-trigger.md).