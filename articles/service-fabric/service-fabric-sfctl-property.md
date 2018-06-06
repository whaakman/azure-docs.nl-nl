---
title: Azure Service Fabric CLI - sfctl eigenschap | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl eigenschap-opdrachten.
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
ms.openlocfilehash: acade3d828c785af9468baa30086d3b79542f9b7
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34764006"
---
# <a name="sfctl-property"></a>de eigenschap sfctl
Store en query eigenschappen onder Service Fabric-namen.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| verwijderen | Hiermee verwijdert u de opgegeven Service Fabric-eigenschap. |
| Toevoegen | Hiermee wordt de opgegeven Service Fabric-eigenschap. |
| lijst | Hiermee haalt u informatie over alle Service Fabric-eigenschappen onder een bepaalde naam. |
| Plaatsen | Maken of bijwerken van een Service Fabric-eigenschap. |

## <a name="sfctl-property-delete"></a>sfctl eigenschap verwijderen
Hiermee verwijdert u de opgegeven Service Fabric-eigenschap.

Hiermee verwijdert u de opgegeven Service Fabric-eigenschap onder een bepaalde naam. Een eigenschap moet worden gemaakt voordat het kan worden verwijderd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --naam-id [vereist] | De Service Fabric-naam, zonder de ' fabric\:' URI-schema. |
| --eigenschapsnaam [vereist] | Hiermee geeft u de naam van de eigenschap ophalen. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-property-get"></a>sfctl eigenschap get
Hiermee wordt de opgegeven Service Fabric-eigenschap.

Hiermee wordt de opgegeven Service Fabric-eigenschap onder een bepaalde naam. Dit wordt altijd zowel metagegevens als waarde retourneren.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --naam-id [vereist] | De Service Fabric-naam, zonder de ' fabric\:' URI-schema. |
| --eigenschapsnaam [vereist] | Hiermee geeft u de naam van de eigenschap ophalen. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-property-list"></a>lijst van de eigenschap sfctl
Hiermee haalt u informatie over alle Service Fabric-eigenschappen onder een bepaalde naam.

Een Service Fabric-naam kan hebben een of meer benoemde eigenschappen waarmee aangepaste gegevens worden opgeslagen. Deze bewerking wordt de informatie over deze eigenschappen in een wisselbare lijst. De informatie bevat de naam, waarde en metagegevens over elk van de eigenschappen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --naam-id [vereist] | De Service Fabric-naam, zonder de ' fabric\:' URI-schema. |
| --vervolgtoken | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd. |
| --bevatten waarden | Kunt u opgeven of u wilt opnemen van de waarden van de eigenschappen die worden geretourneerd. De waarde True als de waarden moeten worden geretourneerd met de metagegevens. Als de waarde False is, wordt alleen de metagegevens van de eigenschap retourneren. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-property-put"></a>sfctl eigenschap put
Maken of bijwerken van een Service Fabric-eigenschap.

Maken of bijwerken van de opgegeven Service Fabric-eigenschap onder een bepaalde naam.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --naam-id [vereist] | De Service Fabric-naam, zonder de ' fabric\:' URI-schema. |
| --eigenschapsnaam [vereist] | De naam van de Service Fabric-eigenschap. |
| --waarde [vereist] | Beschrijft de waarde van een Service Fabric-eigenschap. Dit is een JSON-tekenreeks. <br><br> De json-tekenreeks heeft twee velden, het type van de gegevens en de waarde van de gegevens. De waarde 'Type' moet het eerste item in de JSON-tekenreeks wordt weergegeven, en mag de waarden 'Binair', 'Int64', 'Double', 'String' of 'Guid'. De waarde moet niet serialiseren-kunnen voor de opgegeven typen. Waarden voor zowel 'Type' en 'Data' moeten worden opgegeven als tekenreeksen. |
| --aangepast in het type-id | De eigenschap aangepaste Typ-id Met deze eigenschap, kan de gebruiker het type van de waarde van de eigenschap tag. |
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