---
title: Azure Service Fabric CLI - sfctl is | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl opdrachten is.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: aa1e4d588f367351f0bed10370c7d67d50dd9927
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763048"
---
# <a name="sfctl-is"></a>sfctl is
Vragen en opdrachten naar de service infrastructuur verzenden.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| opdracht | Een administratieve opdracht uitvoeren op het opgegeven exemplaar van de Service-infrastructuur. |
| query | Hiermee wordt een alleen-lezen-query op het opgegeven infrastructuur service-exemplaar. |

## <a name="sfctl-is-command"></a>sfctl is opdracht
Een administratieve opdracht uitvoeren op het opgegeven exemplaar van de Service-infrastructuur.

Deze API biedt voor clusters met een of meer exemplaren van de Service-infrastructuur is geconfigureerd, een manier voor het verzenden van infrastructuur-specifieke opdrachten op een bepaalde instantie van de Service-infrastructuur. Beschikbare opdrachten en hun bijbehorende antwoord indelingen variëren afhankelijk van de infrastructuur waarop het cluster wordt uitgevoerd. Deze API biedt ondersteuning voor het Service Fabric-platform; Dit is niet bedoeld voor rechtstreeks gebruik vanuit uw code.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --opdracht [vereist] | De tekst van de opdracht kan worden aangeroepen. De inhoud van de opdracht is specifiek voor infrastructuur. |
| --service-id | De identiteit van de infrastructuur-service. <br><br> Dit is de volledige naam van de service infrastructuur zonder het 'infrastructuur'-URI-schema. Deze parameter is alleen vereist voor het cluster dat is meer dan één exemplaar van infrastructuur-service wordt uitgevoerd. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-is-query"></a>sfctl is-query
Hiermee wordt een alleen-lezen-query op het opgegeven infrastructuur service-exemplaar.

Deze API biedt voor clusters met een of meer exemplaren van de Service-infrastructuur is geconfigureerd, een manier voor het verzenden van infrastructuur-query's op een bepaalde instantie van de Service-infrastructuur. Beschikbare opdrachten en hun bijbehorende antwoord indelingen variëren afhankelijk van de infrastructuur waarop het cluster wordt uitgevoerd. Deze API biedt ondersteuning voor het Service Fabric-platform; Dit is niet bedoeld voor rechtstreeks gebruik vanuit uw code.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --opdracht [vereist] | De tekst van de opdracht kan worden aangeroepen. De inhoud van de opdracht is specifiek voor infrastructuur. |
| --service-id | De identiteit van de infrastructuur-service. <br><br> Dit is de volledige naam van de service infrastructuur zonder de ' fabric\:' URI-schema. Deze parameter is alleen vereist voor het cluster dat is meer dan één exemplaar van infrastructuur-service wordt uitgevoerd. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="next-steps"></a>Volgende stappen
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).