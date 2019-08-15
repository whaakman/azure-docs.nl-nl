---
title: Azure Service Fabric CLI-sfctl-container | Microsoft Docs
description: Hierin worden de sfctl-container opdrachten van Service Fabric CLI beschreven.
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
ms.openlocfilehash: 0ce6cf7c627657cf757b0c1ef9aa22428c17a7e7
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036468"
---
# <a name="sfctl-container"></a>sfctl container
Container gerelateerde opdrachten uitvoeren op een cluster knooppunt.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| invoke-api | Container-API aanroepen voor een container die is geïmplementeerd op een Service Fabric knoop punt voor het gegeven code pakket. |
| logs | Hiermee haalt u de container logboeken op voor een container die is geïmplementeerd op een Service Fabric knoop punt voor het gegeven code pakket. |

## <a name="sfctl-container-invoke-api"></a>sfctl-container aanroepen-API
Container-API aanroepen voor een container die is geïmplementeerd op een Service Fabric knoop punt voor het gegeven code pakket.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --toepassings-id [vereist] | De identiteit van de toepassing. <br><br> Dit is doorgaans de volledige naam van de toepassing zonder het URI-\:schema ' fabric '. Vanaf versie 6,0 worden hiërarchische namen gescheiden met het teken '\~'. Als de naam van de toepassing bijvoorbeeld ' Fabric\:/MyApp/app1 ' is, is de toepassings identiteit ' Mijntoep\~app1 ' in 6.0 + en ' Mijntoep/app1 ' in vorige versies. |
| --code-package-instance-id [vereist] | ID die een unieke identificatie vormt van een code pakket exemplaar dat is geïmplementeerd op een service Fabric-knoop punt. <br><br> Kan worden opgehaald door service code-Package List. |
| --code-pakket naam [vereist] | De naam van het code pakket dat is opgegeven in het service manifest, geregistreerd als onderdeel van een toepassings type in een Service Fabric cluster. |
| --container-API-URI-pad [vereist] | Container REST API URI-pad, gebruik {id} in plaats van container naam/id. |
| --node-name [required] | De naam van het knoop punt. |
| --Service-manifest-naam [vereist] | De naam van een service manifest dat is geregistreerd als onderdeel van een toepassings type in een Service Fabric cluster. |
| --container-API-hoofd tekst | Hoofd tekst van de HTTP-aanvraag voor container REST API. |
| --container-API-inhouds type | Inhouds type voor container REST API, wordt standaard ingesteld op Application/JSON. |
| --container-API-http-verb | HTTP-term voor container REST API, standaard ingesteld op ophalen. |
| --time-out-t | Time-out van server in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-container-logs"></a>sfctl-container logboeken
Hiermee haalt u de container logboeken op voor een container die is geïmplementeerd op een Service Fabric knoop punt voor het gegeven code pakket.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --toepassings-id [vereist] | De identiteit van de toepassing. <br><br> Dit is doorgaans de volledige naam van de toepassing zonder het URI-\:schema ' fabric '. Vanaf versie 6,0 worden hiërarchische namen gescheiden met het teken '\~'. Als de naam van de toepassing bijvoorbeeld ' Fabric\:/MyApp/app1 ' is, is de toepassings identiteit ' Mijntoep\~app1 ' in 6.0 + en ' Mijntoep/app1 ' in vorige versies. |
| --code-package-instance-id [vereist] | Code pakket exemplaar-ID, die kan worden opgehaald door service code-Package List. |
| --code-pakket naam [vereist] | De naam van het code pakket dat is opgegeven in het service manifest, geregistreerd als onderdeel van een toepassings type in een Service Fabric cluster. |
| --node-name [required] | De naam van het knoop punt. |
| --Service-manifest-naam [vereist] | De naam van een service manifest dat is geregistreerd als onderdeel van een toepassings type in een Service Fabric cluster. |
| --staart | Aantal regels dat moet worden weer gegeven aan het einde van de logboeken. De standaard waarde is 100. ' all ' om de volledige logboeken weer te geven. |
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