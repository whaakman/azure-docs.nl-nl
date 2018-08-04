---
title: Azure Service Fabric CLI - sfctl sa-cluster | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl zelfstandige clusteropdrachten.
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
ms.openlocfilehash: d7f33bf0657ca2a6888387b7651706f9de537bb4
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494353"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-cluster
Zelfstandige Service Fabric-clusters beheren.

## <a name="commands"></a>Opdrachten

|Command|Beschrijving|
| --- | --- |
| configuratie | De configuratie van Service Fabric zelfstandige cluster ophalen. |
| configuratie-upgrade | Start de upgrade van de configuratie van een zelfstandige Service Fabric-cluster. |
| upgrade-status | Ophalen van de upgrade clusterstatus voor configuratie van een zelfstandige Service Fabric-cluster. |

## <a name="sfctl-sa-cluster-config"></a>sfctl sa-cluster config
De configuratie van Service Fabric zelfstandige cluster ophalen.

De clusterconfiguratie bevat eigenschappen van het cluster met verschillende typen in het cluster, beveiligingsconfiguraties, fouten, en upgradedomein topologieën, enzovoort.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --configuratie-api-versie (vereist) | De API-versie van het json-configuratie van het zelfstandige-cluster. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>sfctl sa-cluster config-upgrade
Start de upgrade van de configuratie van een zelfstandige Service Fabric-cluster.

De parameters voor het bijwerken van de configuratie van de opgegeven te valideren en start een upgrade van de configuratie van het cluster als de parameters geldig zijn.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --cluster-config (vereist) | De configuratie van het cluster dat wordt toegepast op het cluster. |
| --toepassing statusbeleid | JSON gecodeerd woordenlijst van paren van de naam van het toepassingstype en het maximale percentage beschadigde voordat de fout. |
| ----knooppunten met slechte deltastatus | De maximaal toegestane percentage van de delta-status degradatie tijdens de upgrade. Toegestane waarden zijn gehele getallen van 0 tot 100. |
| --health-controle-opnieuw proberen | De hoeveelheid tijd tussen pogingen om uit te voeren van een health controleert of de toepassing of het cluster niet in orde.  Standaard\: PT0H0M0S. |
| --health-controle-stabiel | De hoeveelheid tijd dat de toepassing of het cluster in orde blijven moet.  Standaard\: PT0H0M0S. |
| --wachtduur | De hoeveelheid tijd moet wachten na het voltooien van een upgradedomein voordat u begint met de status controleert proces.  Standaard\: PT0H0M0S. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --beschadigde toepassingen | De maximaal toegestane percentage van de beschadigde toepassingen tijdens de upgrade. Toegestane waarden zijn gehele getallen van 0 tot 100. |
| --beschadigde knooppunten | De maximaal toegestane percentage van knooppunten met slechte tijdens de upgrade. Toegestane waarden zijn gehele getallen van 0 tot 100. |
| --upgrade-domein---knooppunten met slechte deltastatus | De maximaal toegestane percentage van het upgradedomein delta health degradatie tijdens de upgrade. Toegestane waarden zijn gehele getallen van 0 tot 100. |
| --upgrade-domein-time-out | De time-out voor het upgradedomein.  Standaard\: PT0H0M0S. |
| --upgrade-time-out | De time-out voor het upgraden.  Standaard\: PT0H0M0S. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

### <a name="examples"></a>Voorbeelden

Een cluster configuratie-update sfctl sa-cluster config-upgrade start--cluster-config <YOUR CLUSTER CONFIG> --toepassing statusbeleid ' {' fabric: / System ": {"ConsiderWarningAsError": true}} '

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa-cluster upgrade-status
Ophalen van de upgrade clusterstatus voor configuratie van een zelfstandige Service Fabric-cluster.

De clusterconfiguratie details van de status van de upgrade van een zelfstandige Service Fabric-cluster ophalen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
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