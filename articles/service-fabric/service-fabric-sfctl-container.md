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
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: 27108d27ee27346e4cba44e6778faff56df70a36
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495125"
---
# <a name="sfctl-container"></a>sfctl container
Voer container gerelateerde opdrachten op een clusterknooppunt.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| api's aanroepen | Container REST-API aanroepen. |
| logboeken | Bij het ophalen van Logboeken voor containers. |

## <a name="sfctl-container-invoke-api"></a>sfctl aanroepen container-api
Container REST-API aanroepen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --aanvraag-id (vereist) | Toepassings-id. |
| --code-pakket-exemplaar-id (vereist) | Pakket exemplaar-ID, die kan worden opgehaald door 'code-pakket-lijst van de service'-code. |
| --code-pakket-naam (vereist) | Naam van de code-pakket. |
| --container-api-uri-pad (vereist) | Container REST API-URI-pad, gebruik {id} in plaats van de naam /-id van de container. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --service-manifest-naam (vereist) | Naam van de manifest van de service. |
| --container-api-instantie | Hoofdtekst van de HTTP-aanvraag voor de container met REST-API. |
| --container-api-content-type | Inhoudstype voor de container met REST-API standaard ingesteld op 'application/json'. |
| --container-api-http-term | HTTP-term voor de container met REST-API standaard GET. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-container-logs"></a>de containerlogboeken sfctl
Bij het ophalen van Logboeken voor containers.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --aanvraag-id (vereist) | Toepassings-id. |
| --code-pakket-exemplaar-id (vereist) | Pakket exemplaar-ID, die kan worden opgehaald door 'code-pakket-lijst van de service'-code. |
| --code-pakket-naam (vereist) | Naam van de code-pakket. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --service-manifest-naam (vereist) | Naam van de manifest van de service. |
| --staart | Retourneert dit getal log regels alleen vanaf het einde van de logboeken. Als een geheel getal of alle voor de uitvoer van alle logboek-regels opgeven. Standaard ingesteld op 'alle'. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |


## <a name="next-steps"></a>Volgende stappen
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Meer informatie over het gebruik van de Service Fabric-CLI met behulp van de [voorbeelden van scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).