---
title: Azure Service Fabric CLI-sfctl-mesh service | Microsoft Docs
description: Beschrijft de Service Fabric CLI sfctl net-service opdrachten.
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
ms.openlocfilehash: 6afcb891de763f156705bc9825ab7575a272c1db
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035031"
---
# <a name="sfctl-mesh-service"></a>sfctl mesh service
Haal service Details en lijst Services van een toepassings bron op.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| list | Een lijst met alle service bronnen. |
| weergeven | Hiermee wordt de service resource met de opgegeven naam opgehaald. |

## <a name="sfctl-mesh-service-list"></a>sfctl mesh-service lijst
Een lijst met alle service bronnen.

Hiermee haalt u de informatie op over alle services van een toepassings bron. De informatie bevat de beschrijving en andere eigenschappen van de service.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --app-naam--Application name [required] | De naam van de toepassing. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-mesh-service-show"></a>sfctl mesh service show
Hiermee wordt de service resource met de opgegeven naam opgehaald.

Hiermee wordt de informatie opgehaald over de service resource met de opgegeven naam. De informatie bevat de beschrijving en andere eigenschappen van de service.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --app-naam--Application name [required] | De naam van de toepassing. |
| --naam-n [vereist] | De naam van de service. |

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