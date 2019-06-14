---
title: Azure Service Fabric CLI - sfctl mesh secret | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl mesh geheime opdrachten.
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
ms.openlocfilehash: acc5862a8794da4fde991025eacfcccf6e16916a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60607690"
---
# <a name="sfctl-mesh-secret"></a>sfctl mesh secret
Ophalen en verwijderen van geheime mesh-resources.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| delete | Hiermee verwijdert u de geheime resource. |
| list | Geeft een lijst van alle geheime resources. |
| weergeven | Hiermee haalt u de geheime resource met de opgegeven naam. |

## <a name="sfctl-mesh-secret-delete"></a>sfctl mesh geheim verwijderen
Hiermee verwijdert u de geheime resource.

Hiermee verwijdert u de opgegeven geheime resource en alle bijbehorende naamwaarden.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam -n (vereist) | De naam van de geheime sleutel. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-mesh-secret-list"></a>geheime sfctl mesh-lijst
Geeft een lijst van alle geheime resources.

Hiermee haalt de informatie over alle geheime resources in een bepaalde resourcegroep. De informatie omvat de beschrijving en andere eigenschappen van de geheime sleutel.

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-mesh-secret-show"></a>sfctl mesh geheim weergeven
Hiermee haalt u de geheime resource met de opgegeven naam.

Hiermee haalt u de informatie over de geheime resource met de opgegeven naam. De informatie omvat de beschrijving en andere eigenschappen van de geheime sleutel.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam -n (vereist) | De naam van de geheime sleutel. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |


## <a name="next-steps"></a>Volgende stappen
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Meer informatie over het gebruik van de Service Fabric-CLI met behulp van de [voorbeelden van scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).