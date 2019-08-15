---
title: Azure Service Fabric CLI-sfctl mesh-gateway | Microsoft Docs
description: Beschrijft de Service Fabric CLI-gateway opdrachten voor sfctl.
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
ms.openlocfilehash: b8acb23b274e6062ae3c50ab80220ab93297037e
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036427"
---
# <a name="sfctl-mesh-gateway"></a>sfctl mesh gateway
Netgateway-resources ophalen en verwijderen.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| verwijderen | Hiermee verwijdert u de gateway resource. |
| list | Een lijst met alle gateway-resources. |
| weergeven | Hiermee wordt de gateway resource met de opgegeven naam opgehaald. |

## <a name="sfctl-mesh-gateway-delete"></a>sfctl net gateway verwijderen
Hiermee verwijdert u de gateway resource.

Hiermee verwijdert u de gateway bron geïdentificeerd door de naam.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-n [vereist] | De naam van de gateway resource. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-mesh-gateway-list"></a>sfctl mesh-gateway lijst
Een lijst met alle gateway-resources.

Hiermee wordt de informatie opgehaald over alle gateway-resources in een bepaalde resource groep. De informatie bevat de beschrijving en andere eigenschappen van de gateway.

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-mesh-gateway-show"></a>sfctl net gateway show
Hiermee wordt de gateway resource met de opgegeven naam opgehaald.

Hiermee wordt de informatie opgehaald over de gateway resource met de opgegeven naam. De informatie bevat de beschrijving en andere eigenschappen van de gateway.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-n [vereist] | De naam van de gateway resource. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |


## <a name="next-steps"></a>Volgende stappen
- [Stel](service-fabric-cli.md) de service Fabric cli in.
- Meer informatie over het gebruik van de Service Fabric CLI met behulp van de [voorbeeld scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).