---
title: Azure Service Fabric CLI - sfctl sa-cluster | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl zelfstandige clusteropdrachten.
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
ms.openlocfilehash: ffdbff7edc5af187071615c8b1e61790b3a38429
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34764009"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-cluster
Zelfstandige Service Fabric-clusters beheren.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| Configuratie | De clusterconfiguratie van het Service Fabric-zelfstandige ophalen. |
| configuratie-upgrade | Start een upgrade van de configuratie van een zelfstandige Service Fabric-cluster. |
| upgrade-status | Status ophalen voor het cluster configuratie de upgrade van een zelfstandige Service Fabric-cluster. |

## <a name="sfctl-sa-cluster-config"></a>sfctl sa-cluster config
De clusterconfiguratie van het Service Fabric-zelfstandige ophalen.

De clusterconfiguratie van het Service Fabric-zelfstandige ophalen. De clusterconfiguratie bevat eigenschappen van het cluster met verschillende knooppunttypen op het cluster, beveiligingsconfiguraties, fout, en upgradedomein topologieën, enzovoort.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --configuratie-api-versie [vereist] | De API-versie van de zelfstandige cluster json-configuratie. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>sfctl sa-cluster config-upgrade
Start een upgrade van de configuratie van een zelfstandige Service Fabric-cluster.

Valideer de parameters van opgegeven configuratie-upgrade en start u een upgrade van de configuratie van het cluster als de parameters geldig zijn.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --cluster-config [vereist] | De configuratie van het cluster. |
| --delta slecht knooppunten | Het maximaal toegestane percentage van de delta health degradatie tijdens de upgrade. Toegestane waarden zijn integerwaarden van nul tot 100. |
| --health-controle-probeer het opnieuw | De hoeveelheid tijd tussen pogingen tot het uitvoeren van een health controleert als de toepassing of het cluster is niet in orde.  Standaard\: PT0H0M0S. |
| --health-controle-stabiel | De tijdsduur dat de toepassing of het cluster in orde blijven moet.  Standaard\: PT0H0M0S. |
| --wachtduur | De lengte van de wachttijd na het voltooien van een upgradedomein voordat u de status controleert proces.  Standaard\: PT0H0M0S. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |
| --slecht toepassingen | Het maximaal toegestane percentage van de beschadigde toepassingen tijdens de upgrade. Toegestane waarden zijn integerwaarden van nul tot 100. |
| --slecht knooppunten | Het maximaal toegestane percentage van slecht knooppunten tijdens de upgrade. Toegestane waarden zijn integerwaarden van nul tot 100. |
| --upgrade-domain-delta-slecht-knooppunten | Het maximaal toegestane percentage van de upgradedomein delta health degradatie tijdens de upgrade. Toegestane waarden zijn integerwaarden van nul tot 100. |
| --time-out-domein-upgrade | De time-out voor het upgradedomein.  Standaard\: PT0H0M0S. |
| --upgrade-time-out | De upgrade time-out.  Standaard\: PT0H0M0S. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa-cluster upgrade-status
Status ophalen voor het cluster configuratie de upgrade van een zelfstandige Service Fabric-cluster.

Ophalen van de clusterconfiguratie upgradestatus details van een zelfstandige Service Fabric-cluster.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
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