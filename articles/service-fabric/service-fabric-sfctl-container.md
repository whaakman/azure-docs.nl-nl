---
title: Azure Service Fabric CLI - sfctl container | Microsoft Docs
description: Beschrijving van de opdrachten van Service Fabric-CLI sfctl container.
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
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 455b2a70568566bff5b1ea4c185568a1758f7db3
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53274901"
---
# <a name="sfctl-container"></a>sfctl container
Voer container gerelateerde opdrachten op een clusterknooppunt.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| api's aanroepen | Container-API voor een container geïmplementeerd op een Service Fabric-knooppunt voor de opgegeven codepakket worden aangeroepen. |
| logboeken | Hiermee haalt u de logboeken voor containers voor de container geïmplementeerd op een Service Fabric-knooppunt voor de opgegeven codepakket. |

## <a name="sfctl-container-invoke-api"></a>sfctl aanroepen container-api
Container-API voor een container geïmplementeerd op een Service Fabric-knooppunt voor de opgegeven codepakket worden aangeroepen.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. <br><br> Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --code-pakket-exemplaar-id (vereist) | De ID die een code-pakket-exemplaar geïmplementeerd op een service fabric-knooppunt wordt aangeduid. <br><br> Kan worden opgehaald door 'code-pakket-lijst van de service'. |
| --code-pakket-naam (vereist) | De naam van het codepakket opgegeven in het servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --container-api-uri-pad (vereist) | Container REST API-URI-pad, gebruik {id} in plaats van de naam /-id van de container. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --service-manifest-naam (vereist) | De naam van een servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --container-api-instantie | Hoofdtekst van de HTTP-aanvraag voor de container met REST-API. |
| --container-api-content-type | Inhoudstype voor de container met REST-API standaard ingesteld op 'application/json'. |
| --container-api-http-term | HTTP-term voor de container met REST-API standaard GET. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-container-logs"></a>de containerlogboeken sfctl
Hiermee haalt u de logboeken voor containers voor de container geïmplementeerd op een Service Fabric-knooppunt voor de opgegeven codepakket.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. <br><br> Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --code-pakket-exemplaar-id (vereist) | Pakket exemplaar-ID, die kan worden opgehaald door 'code-pakket-lijst van de service'-code. |
| --code-pakket-naam (vereist) | De naam van het codepakket opgegeven in het servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --service-manifest-naam (vereist) | De naam van een servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --staart | Het aantal regels om weer te geven vanaf het einde van de logboeken. De standaardwaarde is 100. 'alle' om weer te geven van de volledige Logboeken. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

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