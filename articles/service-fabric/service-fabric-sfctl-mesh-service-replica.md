---
title: Azure Service Fabric CLI-sfctl-mesh service-replica | Microsoft Docs
description: Beschrijft de Service Fabric CLI sfctl mesh service-replica-opdrachten.
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
ms.openlocfilehash: 6819bb32eecf8477e2c0727b50641858db21c784
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035914"
---
# <a name="sfctl-mesh-service-replica"></a>sfctl mesh service-replica
Replica Details ophalen en replica's van een bepaalde service in een toepassings bron weer geven.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| list | Een lijst met alle replica's van een service. |
| weergeven | Hiermee wordt de opgegeven replica van de service van een toepassing opgehaald. |

## <a name="sfctl-mesh-service-replica-list"></a>sfctl-mesh service-replica lijst
Een lijst met alle replica's van een service.

Hiermee wordt de informatie opgehaald over alle replica's van een service. De informatie bevat de beschrijving en andere eigenschappen van de service replica.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --app-naam--Application name [required] | De naam van de toepassing. |
| --Service-naam [vereist] | De naam van de service. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-mesh-service-replica-show"></a>sfctl-mesh service-replica show
Hiermee wordt de opgegeven replica van de service van een toepassing opgehaald.

Hiermee wordt de informatie opgehaald over de service replica met de opgegeven naam. De informatie bevat de beschrijving en andere eigenschappen van de service replica.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --app-naam--Application name [required] | De naam van de toepassing. |
| --naam-n [vereist] | De naam van de service replica. |
| --Service-naam [vereist] | De naam van de service. |

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