---
title: Azure Service Fabric CLI sfctl rpm | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl rpm-opdrachten.
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
ms.openlocfilehash: 5e7cf1b28e8f38f0539e0084e1f745e0dab4dec3
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763609"
---
# <a name="sfctl-rpm"></a>sfctl rpm
Vragen en opdrachten naar de herstel-service manager verzenden.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| goedkeuren forceren | Hiermee wordt de goedkeuring van de opgegeven hersteltaak. |
| verwijderen | Hiermee verwijdert u een voltooide hersteltaak. |
| lijst | Hiermee wordt een lijst met herstel taken die overeenkomt met de opgegeven filters opgehaald. |

## <a name="sfctl-rpm-approve-force"></a>goedkeuren sfctl rpm-force
Hiermee wordt de goedkeuring van de opgegeven hersteltaak.

Deze API biedt ondersteuning voor het Service Fabric-platform; Dit is niet bedoeld voor rechtstreeks gebruik vanuit uw code.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --taak-id [vereist] | De ID van de hersteltaak. |
| --versie | Het huidige versienummer van de hersteltaak. Als niet-nul, klikt u vervolgens slagen de aanvraag alleen als deze waarde komt overeen met de werkelijke huidige versie van de hersteltaak. Als nul is, wordt er geen versiecontrole van uitgevoerd. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-rpm-delete"></a>sfctl rpm verwijderen
Hiermee verwijdert u een voltooide hersteltaak.

Deze API biedt ondersteuning voor het Service Fabric-platform; Dit is niet bedoeld voor rechtstreeks gebruik vanuit uw code.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --taak-id [vereist] | De ID van de voltooide hersteltaak moet worden verwijderd. |
| --versie | Het huidige versienummer van de hersteltaak. Als niet-nul, klikt u vervolgens slagen de aanvraag alleen als deze waarde komt overeen met de werkelijke huidige versie van de hersteltaak. Als nul is, wordt er geen versiecontrole van uitgevoerd. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-rpm-list"></a>sfctl rpm lijst
Hiermee wordt een lijst met herstel taken die overeenkomt met de opgegeven filters opgehaald.

Deze API biedt ondersteuning voor het Service Fabric-platform; Dit is niet bedoeld voor rechtstreeks gebruik vanuit uw code.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --executor-filter | De naam van het herstel executor waarvan geclaimde taken moeten worden opgenomen in de lijst. |
| --status-filter | Een Bitsgewijze OR van de volgende waarden, geven aangegeven welke taak moet worden opgenomen in de lijst met resultaten. <br> 1 - gemaakt <br>2 - geclaimd  <br>4 - voorbereiden  <br>8 - goedgekeurd  <br>16 - uitvoeren  <br>32 - herstellen  <br>64 - voltooid |
| --task-id-filter | Het herstel taak-ID-voorvoegsel moet worden gevonden. |

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