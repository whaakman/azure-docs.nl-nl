---
title: Azure Service Fabric CLI-sfctl is | Microsoft Docs
description: Beschrijft de Service Fabric CLI sfctl is-opdrachten.
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
ms.openlocfilehash: 998a94350250402d4face64f64e12f32cf5b2a36
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036500"
---
# <a name="sfctl-is"></a>sfctl is
Query's uitvoeren en opdrachten verzenden naar de infrastructuur service.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| Opdracht | Hiermee wordt een beheer opdracht aangeroepen voor het opgegeven infrastructuur service-exemplaar. |
| query | Hiermee wordt een alleen-lezen query aangeroepen voor het opgegeven infrastructuur service-exemplaar. |

## <a name="sfctl-is-command"></a>sfctl is opdracht
Hiermee wordt een beheer opdracht aangeroepen voor het opgegeven infrastructuur service-exemplaar.

Voor clusters waarvoor een of meer exemplaren van de infrastructuur service zijn geconfigureerd, biedt deze API een manier om met een infra structuur-specifieke opdracht naar een bepaald exemplaar van de infrastructuur service te verzenden. Beschik bare opdrachten en de bijbehorende antwoord indelingen variëren, afhankelijk van de infra structuur waarop het cluster wordt uitgevoerd. Deze API ondersteunt het Service Fabric-platform. het is niet bedoeld om rechtstreeks vanuit uw code te worden gebruikt.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --opdracht [vereist] | De tekst van de opdracht die moet worden aangeroepen. De inhoud van de opdracht is specifiek voor een infra structuur. |
| --Service-id | De identiteit van de infrastructuur service. <br><br> Dit is de volledige naam van de infrastructuur service zonder het URI-\:schema ' fabric '. Deze para meter is alleen vereist voor het cluster waarop meer dan één exemplaar van de infrastructuur service wordt uitgevoerd. |
| --time-out-t | Time-out van server in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-is-query"></a>sfctl is query
Hiermee wordt een alleen-lezen query aangeroepen voor het opgegeven infrastructuur service-exemplaar.

Voor clusters waarvoor een of meer exemplaren van de infrastructuur service zijn geconfigureerd, biedt deze API een manier om infrastructuur query's te verzenden naar een bepaald exemplaar van de infrastructuur service. Beschik bare opdrachten en de bijbehorende antwoord indelingen variëren, afhankelijk van de infra structuur waarop het cluster wordt uitgevoerd. Deze API ondersteunt het Service Fabric-platform. het is niet bedoeld om rechtstreeks vanuit uw code te worden gebruikt.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --opdracht [vereist] | De tekst van de opdracht die moet worden aangeroepen. De inhoud van de opdracht is specifiek voor een infra structuur. |
| --Service-id | De identiteit van de infrastructuur service. <br><br> Dit is de volledige naam van de infrastructuur service zonder het URI-\:schema ' fabric '. Deze para meter is alleen vereist voor het cluster waarop meer dan één exemplaar van de infrastructuur service wordt uitgevoerd. |
| --time-out-t | Time-out van server in seconden.  Standaard\: 60. |

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