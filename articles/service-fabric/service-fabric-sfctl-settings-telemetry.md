---
title: Azure Service Fabric CLI - sfctl instellingen telemetrie | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI-opdrachten voor telemetrie sfctl-instellingen.
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
ms.openlocfilehash: 42a82ab0be37f260a48a1da6cecab5120c24d293
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669262"
---
# <a name="sfctl-settings-telemetry"></a>sfctl settings telemetry
Telemetrie-instellingen die lokaal zijn voor dit exemplaar van sfctl configureren.

Sfctl telemetrie verzamelt de naam van de opdracht zonder parameters opgegeven of de waarden, sfctl versie, een type besturingssysteem, python-versie, het slagen of mislukken van de opdracht het foutbericht geretourneerd.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| set-telemetrie | Schakel in- of uitschakelen telemetrie. |

## <a name="sfctl-settings-telemetry-set-telemetry"></a>sfctl instellingen telemetrie set-telemetrie
Schakel in- of uitschakelen telemetrie.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --off | Telemetrie uitschakelen. |
| --op | Schakel telemetrie. Dit is de standaardwaarde. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

### <a name="examples"></a>Voorbeelden

Telemetrie uitschakelen.

```
sfctl settings telemetry set_telemetry --off
```

Schakel telemetrie.

```
sfctl settings telemetry set_telemetry --on
```


## <a name="next-steps"></a>Volgende stappen
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Meer informatie over het gebruik van de Service Fabric-CLI met behulp van de [voorbeelden van scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).