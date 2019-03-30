---
title: Azure Service Fabric CLI - sfctl mesh secretvalue | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl mesh secretvalue opdrachten.
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
ms.openlocfilehash: 3f8e46f063d3e725e2174fd907169f3e0167586a
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666542"
---
# <a name="sfctl-mesh-secretvalue"></a>sfctl mesh secretvalue
NET secretvalue resources ophalen en verwijderen.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| delete | Hiermee verwijdert u de opgegeven waarde van de benoemde geheime resource. |
| list | Lijst met namen van alle waarden van de opgegeven geheime resource. |
| weergeven | De waarde van een opgegeven versie van een geheime resource ophalen. |

## <a name="sfctl-mesh-secretvalue-delete"></a>sfctl mesh secretvalue verwijderen
Hiermee verwijdert u de opgegeven waarde van de benoemde geheime resource.

Hiermee verwijdert u de geheime waarde resource aangeduid met de naam. De naam van de resource is doorgaans de versie die is gekoppeld aan die waarde. Verwijderen mislukt als de opgegeven waarde in gebruik is.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --geheim-name - n (vereist) | De naam van de geheime resource. |
| --versie - v (vereist) | De naam van de geheime versie. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-mesh-secretvalue-list"></a>sfctl mesh secretvalue lijst
Lijst met namen van alle waarden van de opgegeven geheime resource.

Hiermee haalt u informatie over alle geheime waarde resources van de opgegeven geheime resource. De informatie bevat de namen van de geheime waarde-resources, maar niet de werkelijke waarden.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --geheim-name - n (vereist) | De naam van de geheime resource. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-mesh-secretvalue-show"></a>sfctl mesh secretvalue weergeven
De waarde van een opgegeven versie van een geheime resource ophalen.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --geheim-name - n (vereist) | De naam van de geheime resource. |
| --versie - v (vereist) | De naam van de geheime versie. |
| --show-value | De werkelijke waarde van de geheime versie weergeven. |

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