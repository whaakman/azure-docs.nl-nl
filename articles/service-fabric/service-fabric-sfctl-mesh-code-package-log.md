---
title: Azure Service Fabric CLI-sfctl mesh code-pakket-log | Microsoft Docs
description: Beschrijft de Service Fabric CLI sfctl netcode-package-log-opdrachten.
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
ms.openlocfilehash: b1949f87dcdb1e3d9fe8e7fd08d8d8ba3b8203a0
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036448"
---
# <a name="sfctl-mesh-code-package-log"></a>sfctl mesh code-package-log
De logboeken ophalen voor de container van het opgegeven code pakket voor de opgegeven service replica.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| Toevoegen | Hiermee worden de logboeken van de container opgehaald. |

## <a name="sfctl-mesh-code-package-log-get"></a>sfctl netcode-pakket-logboek ophalen
Hiermee worden de logboeken van de container opgehaald.

Hiermee haalt u de logboeken voor de container van het opgegeven code pakket van de service replica.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --app-naam--Application name [required] | De naam van de toepassing. |
| --code-pakket naam [vereist] | De naam van het code pakket van de service. |
| --replica-naam [vereist] | Service Fabric replica naam. |
| --Service-naam [vereist] | De naam van de service. |
| --staart | Aantal regels dat moet worden weer gegeven aan het einde van de logboeken. De standaard waarde is 100. ' all ' om de volledige logboeken weer te geven. |

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