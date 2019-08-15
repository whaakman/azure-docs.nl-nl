---
title: Azure Service Fabric CLI-sfctl mesh secretvalue | Microsoft Docs
description: Beschrijft de Service Fabric CLI sfctl net secretvalue-opdrachten.
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
ms.openlocfilehash: 5200c40afe62ce3a236a3a16b59b64deb8d24ced
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035936"
---
# <a name="sfctl-mesh-secretvalue"></a>sfctl mesh secretvalue
Net secretvalue-resources ophalen en verwijderen.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| verwijderen | Hiermee verwijdert u de opgegeven waarde van de naam van de named Secret-resource. |
| list | Lijst met namen van alle waarden van de opgegeven geheime resource. |
| weergeven | Haal de waarde van een opgegeven versie van een geheime resource op. |

## <a name="sfctl-mesh-secretvalue-delete"></a>sfctl mesh secretvalue verwijderen
Hiermee verwijdert u de opgegeven waarde van de naam van de named Secret-resource.

Hiermee verwijdert u de bron van de geheime waarde die wordt geïdentificeerd door de naam. De naam van de resource is doorgaans de versie die aan die waarde is gekoppeld. Als de opgegeven waarde wordt gebruikt, mislukt de verwijdering.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --geheim-naam-n [vereist] | De naam van de geheime resource. |
| --versie-v [vereist] | De naam van de geheime versie. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-mesh-secretvalue-list"></a>sfctl mesh secretvalue-lijst
Lijst met namen van alle waarden van de opgegeven geheime resource.

Haalt informatie op over alle bronnen van de geheime waarde van de opgegeven geheime resource. De informatie bevat de namen van de resources van de geheime waarde, maar niet de werkelijke waarden.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --geheim-naam-n [vereist] | De naam van de geheime resource. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-mesh-secretvalue-show"></a>sfctl mesh secretvalue show
Haal de waarde van een opgegeven versie van een geheime resource op.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --geheim-naam-n [vereist] | De naam van de geheime resource. |
| --versie-v [vereist] | De naam van de geheime versie. |
| --weer geven-waarde | De werkelijke waarde van de geheime versie weer geven. |

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