---
title: Azure Service Fabric CLI - sfctl eigenschap | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl eigenschap opdrachten.
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
ms.openlocfilehash: 78c04abeea1fdc4771f44d44b973ca0dcd2922ea
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53274986"
---
# <a name="sfctl-property"></a>sfctl property
Eigenschappen van Store en query's uitvoeren onder de namen van de Service Fabric.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| delete | Hiermee verwijdert u de opgegeven Service Fabric-eigenschap. |
| Toevoegen | Hiermee haalt u de opgegeven Service Fabric-eigenschap. |
| list | Hiermee haalt u informatie over alle Service Fabric-eigenschappen onder een opgegeven naam. |
| plaatsen | Hiermee maakt of een eigenschap voor Service Fabric-updates. |

## <a name="sfctl-property-delete"></a>sfctl eigenschap verwijderen
Hiermee verwijdert u de opgegeven Service Fabric-eigenschap.

Hiermee verwijdert u de opgegeven Service Fabric-eigenschap onder een opgegeven naam. Een eigenschap moet worden gemaakt voordat deze kan worden verwijderd.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-id (vereist) | De Service Fabric-naam, zonder de ' fabric\:' URI-schema. |
| --eigenschapsnaam (vereist) | Hiermee geeft u de naam van de eigenschap om op te halen. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-property-get"></a>sfctl eigenschap ophalen
Hiermee haalt u de opgegeven Service Fabric-eigenschap.

Hiermee haalt u de opgegeven Service Fabric-eigenschap onder een opgegeven naam. Zowel de waarde en de metagegevens wordt altijd geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-id (vereist) | De Service Fabric-naam, zonder de ' fabric\:' URI-schema. |
| --eigenschapsnaam (vereist) | Hiermee geeft u de naam van de eigenschap om op te halen. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-property-list"></a>lijst met eigenschappen van sfctl
Hiermee haalt u informatie over alle Service Fabric-eigenschappen onder een opgegeven naam.

De naam van een Service Fabric kan een of meer benoemde eigenschappen die aangepaste gegevens opslaan hebben. Deze bewerking wordt de informatie over deze eigenschappen in een lijst met resultatenpagina's opgehaald. De informatie bevat de naam, waarde en metagegevens over elk van de eigenschappen.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-id (vereist) | De Service Fabric-naam, zonder de ' fabric\:' URI-schema. |
| --vervolgtoken | De voortzetting van token-parameter wordt gebruikt om op te halen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in één antwoord passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep retourneert de API volgende set met resultaten. Als er geen verdere resultaten, klikt u vervolgens bevat het vervolgtoken een waarde. De waarde van deze parameter mag geen URL-codering. |
| --bevatten waarden | Kunt u opgeven of u wilt opnemen van de waarden van de eigenschappen die worden geretourneerd. De waarde True als de waarden moeten worden geretourneerd met de metagegevens. De waarde False retourneert alleen de metagegevens van de eigenschap. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-property-put"></a>sfctl eigenschap put
Hiermee maakt of een eigenschap voor Service Fabric-updates.

Hiermee of bijwerken van de opgegeven Service Fabric-eigenschap onder een opgegeven naam.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-id (vereist) | De Service Fabric-naam, zonder de ' fabric\:' URI-schema. |
| --eigenschapsnaam (vereist) | De naam van de Service Fabric-eigenschap. |
| --waarde (vereist) | Beschrijving van de waarde van een Service Fabric-eigenschap. Dit is een JSON-tekenreeks. <br><br> De json-tekenreeks heeft twee velden, het type van de gegevens en de waarde van de gegevens. De waarde 'Type' kan moet het eerste item in de JSON-tekenreeks wordt weergegeven, en waarden 'Binair', 'Int64', 'Dubbele', 'String' of 'Guid'. De waarde moet worden geserialiseerd-kunnen de opgegeven typen. Waarden voor zowel 'Type' en 'Gegevens' moeten worden opgegeven als tekenreeksen. |
| --aangepaste-id-type | Aangepast type-id van de eigenschap. Met deze eigenschap, kan de gebruiker het type van de waarde van de eigenschap tag. |
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