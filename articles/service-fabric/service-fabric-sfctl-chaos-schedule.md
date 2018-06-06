---
title: Azure Service Fabric CLI - sfctl chaos schema | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl chaos planning opdrachten.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: 0d09338f71d71d07ab0e037d4736cfaa1f3cff85
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34764007"
---
# <a name="sfctl-chaos-schedule"></a>sfctl chaos planning
Ophalen en instellen van de planning chaos.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| Toevoegen | Haal de Chaos planning definiëren wanneer en hoe Chaos uitvoeren. |
| instellen | Stel de planning Chaos door Chaos moet worden gebruikt. |

## <a name="sfctl-chaos-schedule-get"></a>sfctl chaos schema ophalen
Haal de Chaos planning definiëren wanneer en hoe Chaos uitvoeren.

Hiermee haalt u de versie van de planning Chaos in gebruik en de planning Chaos die bepaalt wanneer en hoe Chaos uitvoert.

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-chaos-schedule-set"></a>sfctl chaos planning instellen
Stel de planning Chaos door Chaos moet worden gebruikt.

Stel de Chaos planning momenteel in gebruik door Chaos. Chaos wordt automatisch wordt uitgevoerd op basis van de planning Chaos gepland. De versie in de opgegeven invoer planning moet overeenkomen met de versie van de planning Chaos op de server. Als de versie die is opgegeven, komt niet overeen met de versie op de server, wordt de planning Chaos niet bijgewerkt. Als de versie geboden overeenkomt met de versie op de server, de planning Chaos wordt bijgewerkt en de versie van de planning Chaos op de server zo wordt verhoogd met één en terugloopt terug naar 0 na 2.147.483.647. Als Chaos wordt uitgevoerd wanneer deze aanroep wordt gedaan, wordt de oproep mislukt.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --parameters-chaos-woordenlijst | JSON gecodeerde lijst als een toewijzing van tekenreeksnamen aan ChaosParameters moet worden gebruikt door taken. |
| --verstrijken-datum-utc | De datum en tijd voor het stoppen met het schema Chaos plannen.  Standaard\: 9999-12-31T23\:59\:59.999Z. |
| --taken | JSON gecodeerde lijst van ChaosScheduleJobs vertegenwoordigt wanneer Chaos uitgevoerd en met welke parameters Chaos uitvoeren. |
| --start-datum-utc | De datum en tijd voor het aan de slag met het schema Chaos plannen.  Standaard\: 1601-01-01T00\:00\:00.000Z. |
| --versie | Het versienummer van de planning. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

### <a name="examples"></a>Voorbeelden

De volgende opdracht stelt een schema (ervan uitgaande dat de huidige planning heeft versie 0) dat begint op 2016-01-01 en verloopt op 2038 01-01-die Chaos 24 uur van de dag, 7 dagen per week wordt uitgevoerd. Chaos gepland op het cluster voor die tijd.

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
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).