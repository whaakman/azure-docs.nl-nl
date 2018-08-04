---
title: Azure Service Fabric CLI - sfctl rpm | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl rpm-opdrachten.
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
ms.openlocfilehash: 57a9f0516175b459723a3dcdb2e3766f0fa039c1
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495421"
---
# <a name="sfctl-rpm"></a>sfctl rpm
Query's uitvoeren en opdrachten worden verzonden naar de manager-service voor herstel.

## <a name="commands"></a>Opdrachten

|Command|Beschrijving|
| --- | --- |
| goedkeuren-force | Hiermee wordt de goedkeuring van de opgegeven hersteltaak. |
| verwijderen | Hiermee verwijdert u een hersteltaak is voltooid. |
| lijst | Hiermee haalt u een lijst met herstellen taken die overeenkomt met de opgegeven filters. |

## <a name="sfctl-rpm-approve-force"></a>sfctl goedkeuren rpm-force
Hiermee wordt de goedkeuring van de opgegeven hersteltaak.

Deze API biedt ondersteuning voor de Service Fabric-platform; het is niet bedoeld voor gebruik rechtstreeks vanuit uw code.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --taak-id (vereist) | De ID van de hersteltaak. |
| --versie | Het huidige versienummer van de hersteltaak. Als niet-nul, klikt u vervolgens de aanvraag worden alleen uitgevoerd als deze waarde komt overeen met de huidige versie van de hersteltaak. Als nul is, wordt er geen versiecontrole uitgevoerd. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-rpm-delete"></a>sfctl rpm verwijderen
Hiermee verwijdert u een hersteltaak is voltooid.

Deze API biedt ondersteuning voor de Service Fabric-platform; het is niet bedoeld voor gebruik rechtstreeks vanuit uw code.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --taak-id (vereist) | De ID van de voltooide hersteltaak moet worden verwijderd. |
| --versie | Het huidige versienummer van de hersteltaak. Als niet-nul, klikt u vervolgens de aanvraag worden alleen uitgevoerd als deze waarde komt overeen met de huidige versie van de hersteltaak. Als nul is, wordt er geen versiecontrole uitgevoerd. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-rpm-list"></a>sfctl rpm-lijst
Hiermee haalt u een lijst met herstellen taken die overeenkomt met de opgegeven filters.

Deze API biedt ondersteuning voor de Service Fabric-platform; het is niet bedoeld voor gebruik rechtstreeks vanuit uw code.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --executor-filter | De naam van de herstel-uitvoerder waarvan de geclaimde taken moeten worden opgenomen in de lijst. |
| --staat-filter | Bitsgewijze OR van de volgende waarden op te geven welke taak Staten moet worden opgenomen in de lijst met resultaten. <br> 1 - gemaakt <br>2 - geclaimd  <br>4 - voorbereiden  <br>8: goedgekeurd  <br>16 - uitvoering  <br>32 - herstellen  <br>64 - voltooid |
| --task-id-filter | Het herstel taak-ID-voorvoegsel worden vergeleken. |

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