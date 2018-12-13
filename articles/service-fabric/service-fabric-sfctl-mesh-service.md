---
title: Azure Service Fabric CLI - sfctl mesh-service | Microsoft Docs
description: Beschrijving van de opdrachten voor Service Fabric-CLI sfctl mesh-service.
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
ms.openlocfilehash: 6c40cf2340568e704a5c76ad367c07c85826943b
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284799"
---
# <a name="sfctl-mesh-service"></a>sfctl mesh-service
Servicedetails en lijst met services van de bron van een toepassing ophalen.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| list | Geeft een lijst van alle serviceresources. |
| Show | Hiermee haalt u de resource met de opgegeven naam voor de Service. |

## <a name="sfctl-mesh-service-list"></a>lijst van sfctl mesh-service
Geeft een lijst van alle serviceresources.

Hiermee haalt u de informatie over alle services van de bron van een toepassing. De informatie omvat de beschrijving en andere eigenschappen van de Service.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --app-naam: naam van de toepassing (vereist) | De naam van de toepassing. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-mesh-service-show"></a>sfctl mesh-service weergeven
Hiermee haalt u de resource met de opgegeven naam voor de Service.

Hiermee haalt u de informatie over de resource met de opgegeven naam voor de Service. De informatie omvat de beschrijving en andere eigenschappen van de Service.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --app-naam: naam van de toepassing (vereist) | De naam van de toepassing. |
| --naam -n (vereist) | De naam van de service. |

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