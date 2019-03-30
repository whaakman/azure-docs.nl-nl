---
title: Azure Service Fabric CLI - sfctl-NET-app | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl mesh-app-opdrachten.
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
ms.openlocfilehash: fb812412c7dd07800c1e2231c9472a122ab7d7d4
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661833"
---
# <a name="sfctl-mesh-app"></a>sfctl mesh app
Ophalen en verwijderen van de toepassingsresources.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| delete | Hiermee verwijdert u de bron van de toepassing. |
| list | Geeft een lijst van alle toepassingsresources. |
| weergeven | Hiermee haalt u de bron van de toepassing met de opgegeven naam. |

## <a name="sfctl-mesh-app-delete"></a>sfctl mesh-app verwijderen
Hiermee verwijdert u de bron van de toepassing.

Hiermee verwijdert u de bron van het geïdentificeerd door de naam van de toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam -n (vereist) | De naam van de toepassing. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-mesh-app-list"></a>sfctl NET app-lijst
Geeft een lijst van alle toepassingsresources.

Hiermee haalt de informatie over alle toepassingsbronnen in een bepaalde resourcegroep. De informatie omvat de beschrijving en andere eigenschappen van de toepassing.

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-mesh-app-show"></a>sfctl mesh-app weergeven
Hiermee haalt u de bron van de toepassing met de opgegeven naam.

Hiermee haalt u de informatie over de bron van de toepassing met de opgegeven naam. De informatie omvat de beschrijving en andere eigenschappen van de toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam -n (vereist) | De naam van de toepassing. |

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