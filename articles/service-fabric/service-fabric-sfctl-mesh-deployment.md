---
title: Azure Service Fabric CLI - sfctl mesh-implementatie | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI-opdrachten voor de implementatie sfctl mesh.
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
ms.openlocfilehash: e6b484dabd77a142961db2d97242896790fa3d8b
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668463"
---
# <a name="sfctl-mesh-deployment"></a>sfctl mesh deployment
Net van Service Fabric-resources maken.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| maken | Hiermee maakt u een implementatie van Service Fabric NET-Resources. |

## <a name="sfctl-mesh-deployment-create"></a>sfctl mesh-implementatie maken
Hiermee maakt u een implementatie van Service Fabric NET-Resources.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --input-yaml-bestanden (vereist) | Door komma's gescheiden relatieve/absolute paden van alle yaml-bestanden of relatieve/absolute pad van de map (recursieve) die yaml-bestanden bevatten. |
| --parameters | Een relatief/absoluut pad naar yaml-bestand of een json-object met de parameters die moeten worden overschreven. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

### <a name="examples"></a>Voorbeelden

Consolideert en implementeert u de resources aan het cluster door de parameters die worden vermeld in het yaml-bestand te overschrijven

```
sfctl mesh deployment create --input-yaml-files ./app.yaml,./network.yaml --parameters
./param.yaml
```

Consolideert en implementeert alle resources in een map in cluster door de parameters die worden vermeld in het yaml-bestand te overschrijven

```
sfctl mesh deployment create --input-yaml-files ./resources --parameters ./param.yaml
```

Consolideert en implementeert alle resources in een map in cluster door de parameters worden doorgegeven rechtstreeks als json-object te overschrijven

```
sfctl mesh deployment create --input-yaml-files ./resources --parameters "{ 'my_param' :
{'value' : 'my_value'} }"
```


## <a name="next-steps"></a>Volgende stappen
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Meer informatie over het gebruik van de Service Fabric-CLI met behulp van de [voorbeelden van scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).