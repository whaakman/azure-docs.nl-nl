---
title: Azure Service Fabric CLI-sfctl Store | Microsoft Docs
description: Beschrijft de Service Fabric CLI sfctl Store-opdrachten.
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
ms.openlocfilehash: ccd9e8323f6e0de7b81c7600e7828e4858c51201
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035868"
---
# <a name="sfctl-store"></a>sfctl store
Voer elementaire bewerkingen op bestands niveau uit in het cluster installatie kopie archief.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| verwijderen | Hiermee verwijdert u de bestaande inhoud van het installatie kopie archief. |
| root-info | Hiermee haalt u de inhouds informatie op in de hoofdmap van het archief met installatie kopieën. |
| gave | Hiermee wordt de informatie over de inhoud van de installatie kopie opgehaald. |

## <a name="sfctl-store-delete"></a>sfctl Store verwijderen
Hiermee verwijdert u de bestaande inhoud van het installatie kopie archief.

Hiermee verwijdert u de bestaande inhoud van het archief van de installatie kopie in het relatieve pad van de opgegeven afbeeldings opslag. Deze opdracht kan worden gebruikt om Geüploade toepassings pakketten te verwijderen nadat deze zijn ingericht.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --content-path [required] | Het relatieve pad naar het bestand of de map in het archief van de installatie kopie van de hoofdmap. |
| --time-out-t | Time-out van server in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-store-root-info"></a>hoofdmap van sfctl-archief-info
Hiermee haalt u de inhouds informatie op in de hoofdmap van het archief met installatie kopieën.

Hiermee wordt de informatie over de inhoud van de installatie kopie in de hoofdmap van de installatie kopie-Archief geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --time-out-t | Time-out van server in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-store-stat"></a>sfctl Store-stat
Hiermee wordt de informatie over de inhoud van de installatie kopie opgehaald.

Retourneert informatie over de inhoud van de afbeeldings opslag in de opgegeven contentPath. De contentPath is relatief ten opzichte van de hoofdmap van de opslag voor installatie kopieën.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --content-path [required] | Het relatieve pad naar het bestand of de map in het archief van de installatie kopie van de hoofdmap. |
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