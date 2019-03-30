---
title: Azure Service Fabric CLI - sfctl is | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl opdrachten is.
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
ms.openlocfilehash: 2039dd9222809d2c05aaeaf01f9d38c51f3b3797
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661587"
---
# <a name="sfctl-is"></a>sfctl is
Query's uitvoeren en opdrachten worden verzonden naar de infrastructuur-service.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| command | Hiermee wordt een opdracht met beheerdersrechten op de opgegeven infrastructuur voor Service-exemplaar. |
| query | Hiermee wordt een alleen-lezen query op het opgegeven infrastructuur service-exemplaar. |

## <a name="sfctl-is-command"></a>sfctl is opdracht
Hiermee wordt een opdracht met beheerdersrechten op de opgegeven infrastructuur voor Service-exemplaar.

Voor clusters die u een of meer exemplaren van de Service-infrastructuur is geconfigureerd hebt, biedt deze API een manier om infrastructuur-specifieke opdrachten worden verzonden naar een bepaalde instantie van de Service-infrastructuur. Beschikbare opdrachten en hun bijbehorende antwoord indelingen variëren, afhankelijk van de infrastructuur waarin het cluster wordt uitgevoerd. Deze API biedt ondersteuning voor de Service Fabric-platform; het is niet bedoeld voor gebruik rechtstreeks vanuit uw code.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --opdracht (vereist) | De tekst van de opdracht om te worden aangeroepen. De inhoud van de opdracht is specifiek voor infrastructuur. |
| --service-id | De identiteit van de infrastructuur-service. <br><br> Dit is de volledige naam van de service infrastructuur zonder de ' fabric\:' URI-schema. Deze parameter is alleen vereist voor het cluster dat is meer dan één exemplaar van de infrastructuur-service die wordt uitgevoerd. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-is-query"></a>sfctl is query
Hiermee wordt een alleen-lezen query op het opgegeven infrastructuur service-exemplaar.

Voor clusters die u een of meer exemplaren van de Service-infrastructuur is geconfigureerd hebt, biedt deze API een manier om infrastructuur-query's verzenden naar een bepaalde instantie van de Service-infrastructuur. Beschikbare opdrachten en hun bijbehorende antwoord indelingen variëren, afhankelijk van de infrastructuur waarin het cluster wordt uitgevoerd. Deze API biedt ondersteuning voor de Service Fabric-platform; het is niet bedoeld voor gebruik rechtstreeks vanuit uw code.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --opdracht (vereist) | De tekst van de opdracht om te worden aangeroepen. De inhoud van de opdracht is specifiek voor infrastructuur. |
| --service-id | De identiteit van de infrastructuur-service. <br><br> Dit is de volledige naam van de service infrastructuur zonder de ' fabric\:' URI-schema. Deze parameter is alleen vereist voor het cluster dat is meer dan één exemplaar van de infrastructuur-service die wordt uitgevoerd. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |


## <a name="next-steps"></a>Volgende stappen
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Meer informatie over het gebruik van de Service Fabric-CLI met behulp van de [voorbeelden van scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).