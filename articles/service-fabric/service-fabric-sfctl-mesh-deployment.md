---
title: Azure Service Fabric CLI-implementatie van sfctl-net | Microsoft Docs
description: Hierin worden de implementatie-opdrachten voor de Service Fabric CLI sfctl-net beschreven.
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
ms.openlocfilehash: b3f506b46ef563f47fc7c67b759d3fcd08b7509d
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035189"
---
# <a name="sfctl-mesh-deployment"></a>sfctl mesh deployment
Service Fabric netresources maken.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| create | Hiermee maakt u een implementatie van Service Fabric netresources. |

## <a name="sfctl-mesh-deployment-create"></a>implementatie van sfctl-net maken
Hiermee maakt u een implementatie van Service Fabric netresources.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --input-YAML-files [required] | Door komma's gescheiden relatieve/absolute bestands paden van alle YAML-bestanden of relatief/absoluut pad van de directory (recursieve) die YAML-bestanden bevatten. |
| --parameters | Een relatief/absoluut pad naar een yaml-bestand of een JSON-object dat de para meters bevat die moeten worden overschreven. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

### <a name="examples"></a>Voorbeelden

Consolideert en implementeert alle resources naar het cluster door de para meters die worden vermeld in het yaml-bestand te overschrijven.

```
sfctl mesh deployment create --input-yaml-files ./app.yaml,./network.yaml --parameters
./param.yaml
```

Consolideert en implementeert alle resources in een directory naar het cluster door de para meters die worden vermeld in het yaml-bestand te overschrijven.

```
sfctl mesh deployment create --input-yaml-files ./resources --parameters ./param.yaml
```

Consolideert en implementeert alle resources in een directory naar een cluster door de para meters te overschrijven die rechtstreeks als JSON-object worden door gegeven

```
sfctl mesh deployment create --input-yaml-files ./resources --parameters "{ 'my_param' :
{'value' : 'my_value'} }"
```


## <a name="next-steps"></a>Volgende stappen
- [Stel](service-fabric-cli.md) de service Fabric cli in.
- Meer informatie over het gebruik van de Service Fabric CLI met behulp van de [voorbeeld scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).