---
title: Azure Service Fabric CLI - sfctl-NET-app | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl mesh-app-opdrachten.
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
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 3c2194f6a001e4fc49dcf1694f8a9cda41550ace
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284834"
---
# <a name="sfctl-mesh-app"></a>sfctl mesh-app
Ophalen en verwijderen van de toepassingsresources.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| delete | Hiermee verwijdert u de bron van de toepassing. |
| list | Geeft een lijst van alle toepassingsresources. |
| Show | Hiermee haalt u de bron van de toepassing met de opgegeven naam. |

## <a name="sfctl-mesh-app-delete"></a>sfctl mesh-app verwijderen
Hiermee verwijdert u de bron van de toepassing.

Hiermee verwijdert u de bron van het geïdentificeerd door de naam van de toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam -n (vereist) | De naam van de toepassing. |

### <a name="global-arguments"></a>Algemene argumenten

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

### <a name="global-arguments"></a>Algemene argumenten

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

### <a name="global-arguments"></a>Algemene argumenten

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