---
title: Azure Service Fabric CLI - sfctl mesh-netwerk | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl mesh-netwerkopdrachten.
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
ms.openlocfilehash: feec5c4796c025c1707b4eb93bfe34b8d384ef3a
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669177"
---
# <a name="sfctl-mesh-network"></a>sfctl mesh network
Ophalen en verwijderen van mesh-netwerkbronnen.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| delete | Hiermee verwijdert u de netwerk-resource. |
| list | Geeft een lijst van alle netwerkbronnen. |
| weergeven | Hiermee haalt u de netwerk-resource met de opgegeven naam. |

## <a name="sfctl-mesh-network-delete"></a>sfctl mesh netwerk verwijderen
Hiermee verwijdert u de netwerk-resource.

Hiermee verwijdert u de netwerkbron aangeduid met de naam.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam -n (vereist) | De naam van het netwerk. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-mesh-network-list"></a>lijst met sfctl mesh-netwerken
Geeft een lijst van alle netwerkbronnen.

Hiermee haalt de informatie over alle netwerkbronnen in een bepaalde resourcegroep. De informatie omvat de beschrijving en andere eigenschappen van het netwerk.

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-mesh-network-show"></a>sfctl mesh netwerk weergeven
Hiermee haalt u de netwerk-resource met de opgegeven naam.

Hiermee haalt u de informatie over de netwerk-resource met de opgegeven naam. De informatie omvat de beschrijving en andere eigenschappen van het netwerk.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam -n (vereist) | De naam van het netwerk. |

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