---
title: Azure Service Fabric CLI-sfctl rpm | Microsoft Docs
description: Hierin worden de Service Fabric CLI-opdrachten voor sfctl rpm beschreven.
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
ms.openlocfilehash: 3f40451087aba5af5b02625ad3ac1ca6231d976c
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035885"
---
# <a name="sfctl-rpm"></a>sfctl rpm
Query's uitvoeren en opdrachten verzenden naar de service reparatie beheer.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| goed keuren forceren | Hiermee wordt de goed keuring van de opgegeven herstel taak geforceerd. |
| verwijderen | Hiermee verwijdert u een voltooide herstel taak. |
| list | Hiermee wordt een lijst met herstel taken opgehaald die overeenkomen met de opgegeven filters. |

## <a name="sfctl-rpm-approve-force"></a>sfctl rpm goed keuren-forceren
Hiermee wordt de goed keuring van de opgegeven herstel taak geforceerd.

Deze API ondersteunt het Service Fabric-platform. het is niet bedoeld om rechtstreeks vanuit uw code te worden gebruikt.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --taak-id [vereist] | De ID van de herstel taak. |
| --versie | Het huidige versie nummer van de herstel taak. Als dit niet het geval is, wordt de aanvraag alleen uitgevoerd als deze waarde overeenkomt met de werkelijke huidige versie van de herstel taak. Als de waarde nul is, wordt er geen versie controle uitgevoerd. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-rpm-delete"></a>sfctl rpm verwijderen
Hiermee verwijdert u een voltooide herstel taak.

Deze API ondersteunt het Service Fabric-platform. het is niet bedoeld om rechtstreeks vanuit uw code te worden gebruikt.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --taak-id [vereist] | De ID van de voltooide herstel taak die moet worden verwijderd. |
| --versie | Het huidige versie nummer van de herstel taak. Als dit niet het geval is, wordt de aanvraag alleen uitgevoerd als deze waarde overeenkomt met de werkelijke huidige versie van de herstel taak. Als de waarde nul is, wordt er geen versie controle uitgevoerd. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-rpm-list"></a>sfctl rpm-lijst
Hiermee wordt een lijst met herstel taken opgehaald die overeenkomen met de opgegeven filters.

Deze API ondersteunt het Service Fabric-platform. het is niet bedoeld om rechtstreeks vanuit uw code te worden gebruikt.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --uitvoerbaar filter | De naam van de reparatie-uitvoerder waarvan de gedeclareerde taken moeten worden opgenomen in de lijst. |
| --status-filter | Een bitsgewijze-of van de volgende waarden, opgeven welke taak statussen moeten worden opgenomen in de lijst met resultaten. <br> 1-gemaakt <br>2-geclaimd  <br>4: voorbereiden  <br>8-goedgekeurd  <br>16-uitvoeren  <br>32-herstellen  <br>64-voltooid |
| --task-id-filter | Het voor voegsel van de reparatie taak-ID moet worden gevonden. |

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