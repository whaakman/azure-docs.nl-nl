---
title: Azure Service Fabric CLI-sfctl-instellingen telemetrie | Microsoft Docs
description: Hierin worden de telemetrie-opdrachten voor de Service Fabric CLI sfctl-instellingen beschreven.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: cf5ebbeb4d9b4757e0c55eeb1a9268065efb2c7c
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035205"
---
# <a name="sfctl-settings-telemetry"></a>sfctl settings telemetry
Telemetrie-instellingen configureren die lokaal zijn voor dit exemplaar van sfctl.

Sfctl-telemetrie verzamelt opdracht naam zonder opgegeven para meters of hun waarden, Sfctl versie, type besturings systeem, python-versie, het slagen of mislukken van de opdracht, het fout bericht dat wordt geretourneerd.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| set-telemetrie | Telemetrie in-of uitschakelen. |

## <a name="sfctl-settings-telemetry-set-telemetry"></a>sfctl-instellingen telemetrie-set-telemetrie
Telemetrie in-of uitschakelen.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --uit | Schakel telemetrie uit. |
| --aan | Schakel telemetrie in. Dit is de standaardwaarde. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

### <a name="examples"></a>Voorbeelden

Schakel telemetrie uit.

```
sfctl settings telemetry set_telemetry --off
```

Schakel telemetrie in.

```
sfctl settings telemetry set_telemetry --on
```


## <a name="next-steps"></a>Volgende stappen
- [Stel](service-fabric-cli.md) de service Fabric cli in.
- Meer informatie over het gebruik van de Service Fabric CLI met behulp van de [voorbeeld scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).