---
title: Azure Service Fabric CLI-sfctl mesh-volume | Microsoft Docs
description: Beschrijft de Service Fabric CLI sfctl net-volume opdrachten.
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
ms.openlocfilehash: 4e2413a152048e08d6218783891c20b66ac2e618
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035003"
---
# <a name="sfctl-mesh-volume"></a>sfctl mesh volume
Volume bronnen ophalen en verwijderen.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| verwijderen | Hiermee verwijdert u de volume resource. |
| list | Een lijst met alle volume bronnen. |
| weergeven | Hiermee wordt de volume resource met de opgegeven naam opgehaald. |

## <a name="sfctl-mesh-volume-delete"></a>sfctl mesh-volume verwijderen
Hiermee verwijdert u de volume resource.

Hiermee verwijdert u de volume resource die wordt geïdentificeerd door de naam.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-n [vereist] | De naam van het volume. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-mesh-volume-list"></a>sfctl mesh-volume lijst
Een lijst met alle volume bronnen.

Hiermee wordt de informatie opgehaald over alle volume resources in een bepaalde resource groep. De informatie bevat de beschrijving en andere eigenschappen van het volume.

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-mesh-volume-show"></a>sfctl mesh-volume weer geven
Hiermee wordt de volume resource met de opgegeven naam opgehaald.

Hiermee wordt de informatie opgehaald over de volume resource met de opgegeven naam. De informatie bevat de beschrijving en andere eigenschappen van het volume.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-n [vereist] | De naam van het volume. |

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