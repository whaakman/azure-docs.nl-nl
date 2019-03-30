---
title: Azure Service Fabric CLI - sfctl NET service-replica | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl mesh-service-replica-opdrachten.
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
ms.openlocfilehash: 1e0955ef2a52a6313f0449b956229e03f0a5b5e8
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662428"
---
# <a name="sfctl-mesh-service-replica"></a>sfctl mesh service-replica
Details van de replica en de lijst met replica's van een bepaalde service in de bron van een toepassing ophalen.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| list | Geeft een lijst van alle replica's van een service. |
| weergeven | Hiermee haalt u de opgegeven replica van de service van een toepassing. |

## <a name="sfctl-mesh-service-replica-list"></a>lijst met sfctl mesh-service-replica 's
Geeft een lijst van alle replica's van een service.

Hiermee haalt u de informatie over alle replica's van een service. De informatie omvat de beschrijving en andere eigenschappen van de service-replica.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --app-naam: naam van de toepassing (vereist) | De naam van de toepassing. |
| --service-name                [Required] | De naam van de service. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-mesh-service-replica-show"></a>sfctl mesh-service-replica weergeven
Hiermee haalt u de opgegeven replica van de service van een toepassing.

Hiermee haalt u de informatie over de service-replica met de opgegeven naam. De informatie omvat de beschrijving en andere eigenschappen van de service-replica.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --app-naam: naam van de toepassing (vereist) | De naam van de toepassing. |
| --naam -n (vereist) | De naam van de service-replica. |
| --service-name                [Required] | De naam van de service. |

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