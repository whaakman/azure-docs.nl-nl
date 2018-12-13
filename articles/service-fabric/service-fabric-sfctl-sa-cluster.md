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
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: ce10e2c24e89140357df3fa6b724a1f89f389a50
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275479"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-cluster
Zelfstandige Service Fabric-clusters beheren.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| configuratie | De configuratie van Service Fabric zelfstandige cluster ophalen. |
| configuratie-upgrade | Start de upgrade van de configuratie van een zelfstandige Service Fabric-cluster. |
| upgrade-status | Ophalen van de upgrade clusterstatus voor configuratie van een zelfstandige Service Fabric-cluster. |

## <a name="sfctl-sa-cluster-config"></a>sfctl sa-cluster config
De configuratie van Service Fabric zelfstandige cluster ophalen.

De clusterconfiguratie bevat eigenschappen van het cluster met verschillende typen in het cluster, beveiligingsconfiguraties, fouten, en upgradedomein topologieën, enzovoort.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --configuratie-api-versie (vereist) | De API-versie van het json-configuratie van het zelfstandige-cluster. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
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

|Argument|Description|
| --- | --- |
| --cluster-config (vereist) | De configuratie van het cluster. |
| --toepassing statusbeleid | JSON gecodeerd woordenlijst van paren van de naam van het toepassingstype en het maximale percentage beschadigde voordat de fout. |
| ----knooppunten met slechte deltastatus | De maximaal toegestane percentage van de delta-status degradatie tijdens de upgrade. Toegestane waarden zijn gehele getallen van 0 tot 100. |
| --health-controle-opnieuw proberen | De hoeveelheid tijd tussen pogingen tot het uitvoeren van statuscontroles als de toepassing of het cluster niet in orde is.  Standaard\: PT0H0M0S. |
| --health-controle-stabiel | De hoeveelheid tijd dat de toepassing of het cluster moet in orde blijft voordat de upgrade wordt uitgevoerd op het volgende upgradedomein.  Standaard\: PT0H0M0S. <br><br> Eerst wordt dit geïnterpreteerd als een tekenreeks voor de duur van een ISO 8601. Als dat mislukt, wordt klikt u vervolgens dit geïnterpreteerd als een getal voor het totale aantal milliseconden. |
| --wachtduur | De hoeveelheid tijd moet wachten na het voltooien van een upgradedomein voordat u begint met de status controleert proces.  Standaard\: PT0H0M0S. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --beschadigde toepassingen | De maximaal toegestane percentage van de beschadigde toepassingen tijdens de upgrade. Toegestane waarden zijn gehele getallen van 0 tot 100. |
| --beschadigde knooppunten | De maximaal toegestane percentage van knooppunten met slechte tijdens de upgrade. Toegestane waarden zijn gehele getallen van 0 tot 100. |
| --upgrade-domein---knooppunten met slechte deltastatus | De maximaal toegestane percentage van het upgradedomein delta health degradatie tijdens de upgrade. Toegestane waarden zijn gehele getallen van 0 tot 100. |
| --upgrade-domein-time-out | De hoeveelheid tijd elk upgradedomein is voltooid voordat FailureAction wordt uitgevoerd.  Standaard\: PT0H0M0S. <br><br> Eerst wordt dit geïnterpreteerd als een tekenreeks voor de duur van een ISO 8601. Als dat mislukt, wordt klikt u vervolgens dit geïnterpreteerd als een getal voor het totale aantal milliseconden. |
| --upgrade-time-out | De hoeveelheid tijd de algehele upgrade is voltooid voordat FailureAction wordt uitgevoerd.  Standaard\: PT0H0M0S. <br><br> Eerst wordt dit geïnterpreteerd als een tekenreeks voor de duur van een ISO 8601. Als dat mislukt, wordt klikt u vervolgens dit geïnterpreteerd als een getal voor het totale aantal milliseconden. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

### <a name="examples"></a>Voorbeelden

Beginnen met een cluster configuratie-update

```
sfctl sa-cluster config-upgrade --cluster-config <YOUR CLUSTER CONFIG> --application-health-
policies "{"fabric:/System":{"ConsiderWarningAsError":true}}"
```

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa-cluster upgrade-status
Ophalen van de upgrade clusterstatus voor configuratie van een zelfstandige Service Fabric-cluster.

De clusterconfiguratie details van de status van de upgrade van een zelfstandige Service Fabric-cluster ophalen.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
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