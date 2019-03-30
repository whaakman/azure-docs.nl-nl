---
title: Azure Service Fabric CLI - sfctl net een code-pakket-logboek | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI-opdrachten voor code-pakket-logboek sfctl mesh.
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
ms.openlocfilehash: e7bc8491071946eaa2e322517e5d36d681a49130
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661068"
---
# <a name="sfctl-mesh-code-package-log"></a>sfctl mesh code-package-log
De logboeken ophalen voor de container van de opgegeven code-pakket voor de opgegeven service-replica.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| Toevoegen | Hiermee haalt u de logboeken van de container. |

## <a name="sfctl-mesh-code-package-log-get"></a>sfctl mesh-code-pakket-logboek ophalen
Hiermee haalt u de logboeken van de container.

Hiermee haalt de logboeken voor de container van de opgegeven code-pakket van de service-replica.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --app-naam: naam van de toepassing (vereist) | De naam van de toepassing. |
| --code-pakket-naam (vereist) | De naam van het codepakket van de service. |
| --replica-name                [Required] | Naam van service Fabric-replica. |
| --service-name                [Required] | De naam van de service. |
| --staart | Het aantal regels om weer te geven vanaf het einde van de logboeken. De standaardwaarde is 100. 'alle' om weer te geven van de volledige Logboeken. |

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