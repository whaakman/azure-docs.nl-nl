---
title: Azure Service Fabric CLI - sfctl store | Microsoft Docs
description: Beschrijving van de opdrachten van Service Fabric-CLI sfctl store.
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
ms.openlocfilehash: 2f2da5b38ab39e3636ff0ac09ac2c5515e02b0c1
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492332"
---
# <a name="sfctl-store"></a>sfctl store
Basisopdrachten voor bestandsbeheer niveau bewerkingen uitvoeren op de installatiekopieopslag van het cluster.

## <a name="commands"></a>Opdrachten

|Command|Beschrijving|
| --- | --- |
| verwijderen | Verwijdert bestaande inhoud van de installatiekopie van de store. |
| root-info | Hiermee haalt u informatie over de inhoud in de hoofdmap van de installatiekopieopslag. |
| Stat | Hiermee haalt u de informatie van een installatiekopie-store. |

## <a name="sfctl-store-delete"></a>sfctl store delete
Verwijdert bestaande inhoud van de installatiekopie van de store.

Verwijdert bestaande inhoud van de installatiekopie store wordt gevonden in de installatiekopie van het opgegeven opslaan relatieve pad. Dit kan worden gebruikt om het geüploade toepassingspakketten verwijderen wanneer deze zijn ingericht.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --inhoud-pad (vereist) | Relatief pad naar het bestand of map in de installatiekopieopslag vanuit de hoofdmap. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-store-root-info"></a>sfctl store root-info
Hiermee haalt u informatie over de inhoud in de hoofdmap van de installatiekopieopslag.

Retourneert informatie over de installatiekopieopslag inhoud in de hoofdmap van de installatiekopieopslag.

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

## <a name="sfctl-store-stat"></a>sfctl store stat
Hiermee haalt u de informatie van een installatiekopie-store.

Retourneert informatie over de inhoud van de store op de opgegeven contentPath. De contentPath is ten opzichte van de hoofdmap van de installatiekopieopslag.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --inhoud-pad (vereist) | Relatief pad naar het bestand of map in de installatiekopieopslag vanuit de hoofdmap. |
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
- [Setup](service-fabric-cli.md) de Service Fabric-CLI.
- Meer informatie over het gebruik van de Service Fabric-CLI met behulp van de [voorbeelden van scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).