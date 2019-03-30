---
title: Azure Service Fabric CLI - sfctl chaos schedule | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI-opdrachten voor planning sfctl chaos.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: dc3dd06b5feac1f66598cd65fa79f447a1bbd9be
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668310"
---
# <a name="sfctl-chaos-schedule"></a>sfctl chaos schedule
Ophalen en instellen van de planning van chaos.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| Toevoegen | Haal de Chaos-planning definiëren wanneer en hoe u Chaos uitgevoerd. |
| set | Stel de planning die wordt gebruikt door Chaos. |

## <a name="sfctl-chaos-schedule-get"></a>sfctl chaos schema ophalen
Haal de Chaos-planning definiëren wanneer en hoe u Chaos uitgevoerd.

Hiermee haalt u de versie van de planning Chaos in gebruik en de Chaos-planning waarmee wordt gedefinieerd wanneer en hoe u Chaos uitgevoerd.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-chaos-schedule-set"></a>sfctl chaos schema instellen
Stel de planning die wordt gebruikt door Chaos.

Chaos wordt automatisch gepland op basis van de Chaos-planning wordt uitgevoerd. De versie in de opgegeven invoer planning moet overeenkomen met de versie van de planning Chaos op de server. Als de opgegeven versie komt niet overeen met de versie op de server, wordt de Chaos-planning niet bijgewerkt. Als de versie die overeenkomt met de versie op de server, de Chaos-planning wordt bijgewerkt en de versie van de planning Chaos op de server zo wordt verhoogd met één en wordt weer in op 0 na 2.147.483.647. Als Chaos wordt uitgevoerd wanneer deze oproep wordt gedaan, wordt de oproep mislukt.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --chaos-parameters-dictionary | JSON gecodeerd lijst een toewijzing van de tekenreeksnamen van de die naar ChaosParameters moet worden gebruikt door taken. |
| --expiry-date-utc | De datum en tijd voor het stoppen met het gebruik van de planning voor het plannen van Chaos.  Default\: 9999-12-31T23\:59\:59.999Z. |
| --jobs | JSON gecodeerd lijst met ChaosScheduleJobs vertegenwoordigt wanneer Chaos uitgevoerd en met welke parameters om uit te voeren van Chaos. |
| --start-date-utc | De datum en tijd voor wanneer moet worden gestart met behulp van de planning voor het plannen van Chaos.  Standaard\: 1601-01-01T00\:00\:00.000Z. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --versie | Het versienummer van de planning. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

### <a name="examples"></a>Voorbeelden

De volgende opdracht stelt een schema (ervan uitgaande dat het huidige schema heeft versie 0) die begint op 01-01-2016 en verloopt op 2038-01-01 die Chaos 24 uur van de dag, 7 dagen per week wordt uitgevoerd. Chaos wordt voor die tijd op het cluster worden gepland.

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
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Meer informatie over het gebruik van de Service Fabric-CLI met behulp van de [voorbeelden van scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).
