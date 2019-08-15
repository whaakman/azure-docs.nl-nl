---
title: Azure Service Fabric CLI-sfctl chaos-schema | Microsoft Docs
description: Beschrijft de Service Fabric CLI sfctl chaos schema-opdrachten.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 1664978110b7c700906cbf4e6c80806ac70f1f05
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036534"
---
# <a name="sfctl-chaos-schedule"></a>sfctl chaos schedule
De chaos-planning ophalen en instellen.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| Toevoegen | Het chaos-schema ophalen dat definieert wanneer en hoe chaos moet worden uitgevoerd. |
| set | Het schema instellen dat door chaos wordt gebruikt. |

## <a name="sfctl-chaos-schedule-get"></a>sfctl chaos planning ophalen
Het chaos-schema ophalen dat definieert wanneer en hoe chaos moet worden uitgevoerd.

Hiermee haalt u de versie van de chaos-planning in gebruik en het chaos-schema dat definieert wanneer en hoe chaos moet worden uitgevoerd.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --time-out-t | Time-out van server in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-chaos-schedule-set"></a>sfctl chaos-schema instellen
Het schema instellen dat door chaos wordt gebruikt.

Chaos wordt automatisch gepland op basis van het chaos-schema. De versie in de opgegeven invoer planning moet overeenkomen met de versie van het chaos-schema op de server. Als de opgegeven versie niet overeenkomt met de versie op de server, wordt het chaos-schema niet bijgewerkt. Als de geleverde versie overeenkomt met de versie op de server, wordt de chaos-planning bijgewerkt en wordt de versie van het chaos-schema op de server verhoogd met één en teruggestuurd naar 0 na 2.147.483.647. Als chaos wordt uitgevoerd wanneer deze aanroep wordt gedaan, mislukt de aanroep.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --chaos-para meters-Dictionary | JSON-gecodeerde lijst die een toewijzing van teken reeks namen vertegenwoordigt aan ChaosParameters die moeten worden gebruikt door taken. |
| --expiry-date-utc | De datum en tijd waarop wordt gebruikgemaakt van het schema om chaos te plannen.  Standaard\: : 9999-12-\:31T23\:59 59.999 z. |
| --jobs | JSON-gecodeerde lijst met ChaosScheduleJobs die aangeeft wanneer chaos moet worden uitgevoerd en met welke para meters chaos moeten worden uitgevoerd. |
| --begin datum-UTC | De datum en tijd waarop het schema moet worden gebruikt om chaos te plannen.  Standaard\: 1601-01-01T00\:00\:00.000 z. |
| --time-out-t | Time-out van server in seconden.  Standaard\: 60. |
| --versie | Het versie nummer van het schema. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

### <a name="examples"></a>Voorbeelden

Met de volgende opdracht stelt u een planning in (ervan uitgaande dat het huidige schema versie 0 heeft) die begint op 2016-01-01 en verloopt op 2038-01-01 dat chaos 24 uur van de dag, 7 dagen per week uitvoert. Chaos wordt voor die tijd gepland op het cluster.

    sfctl chaos schedule set --version 0 --start-date-utc "2016-01-01T00:00:00.000Z" --expiry-date-utc "2038-01-01T00:00:00.000Z"
    --chaos-parameters-dictionary
    [
    {
        "Key":"adhoc",
        "Value":{
            "MaxConcurrentFaults":3,
            "EnableMoveReplicaFaults":true,
            "ChaosTargetFilter":{
                "NodeTypeInclusionList":[
                "N0010Ref",
                "N0020Ref",
                "N0030Ref",
                "N0040Ref",
                "N0050Ref"
                ]
            },
            "MaxClusterStabilizationTimeoutInSeconds":60,
            "WaitTimeBetweenIterationsInSeconds":15,
            "WaitTimeBetweenFaultsInSeconds":30,
            "TimeToRunInSeconds":"600",
            "Context":{
                "Map":{
                "test":"value"
                }
            },
            "ClusterHealthPolicy":{
                "MaxPercentUnhealthyNodes":0,
                "ConsiderWarningAsError":true,
                "MaxPercentUnhealthyApplications":0
            }
        }
    }
    ]
    --jobs
    [
    {
        "ChaosParameters":"adhoc",
        "Days":{
            "Sunday":true,
            "Monday":true,
            "Tuesday":true,
            "Wednesday":true,
            "Thursday":true,
            "Friday":true,
            "Saturday":true
        },
        "Times":[
            {
                "StartTime":{
                "Hour":0,
                "Minute":0
                },
                "EndTime":{
                "Hour":23,
                "Minute":59
                }
            }
        ]
    }
    ]


## <a name="next-steps"></a>Volgende stappen
- [Stel](service-fabric-cli.md) de service Fabric cli in.
- Meer informatie over het gebruik van de Service Fabric CLI met behulp van de [voorbeeld scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).
