---
title: Azure Service Fabric CLI - sfctl container | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl container-opdrachten.
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
ms.openlocfilehash: cd3725ac547a1ed1fd9207dc48ba3b6227e85ef1
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34764008"
---
# <a name="sfctl-container"></a>sfctl container
Container-gerelateerde opdrachten uitvoeren op een clusterknooppunt.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| Invoke-api | Container REST-API worden aangeroepen. |
| logboeken | De logboeken van de container ophalen. |

## <a name="sfctl-container-invoke-api"></a>sfctl container invoke-api
Container REST-API worden aangeroepen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | Toepassings-id. |
| --code-pakket-exemplaar-id [vereist] | Code pakket exemplaar-ID, die kan worden opgehaald door 'code-pakket-lijst van de service'. |
| --code pakketnaam van [vereist] | Pakketnaam code. |
| --container-api-uri-pad [vereist] | Container REST API-URI-pad, gebruik {ID} in plaats van naam /-id van de container. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --manifest-servicenaam [vereist] | Naam van de manifest van de service. |
| --container-api-instantie | Hoofdtekst van de HTTP-aanvraag voor de container REST-API. |
| --container-api-content-type | Type inhoud voor de container REST-API standaard ingesteld op 'application/json'. |
| --container-api-http-term | HTTP-term voor de container REST-API standaard ingesteld op GET. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-container-logs"></a>sfctl container Logboeken
De logboeken van de container ophalen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | Toepassings-id. |
| --code-pakket-exemplaar-id [vereist] | Code pakket exemplaar-ID, die kan worden opgehaald door 'code-pakket-lijst van de service'. |
| --code pakketnaam van [vereist] | Pakketnaam code. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --manifest-servicenaam [vereist] | Naam van de manifest van de service. |
| --staart | Dit aantal logboek regels alleen vanaf het einde van de logboeken worden geretourneerd. Als een geheel getal of alle voor uitvoer van alle regels van het logboekbestand opgeven. Standaardwaarden voor alle. |
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