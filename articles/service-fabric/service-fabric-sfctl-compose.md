---
title: Azure Service Fabric CLI - sfctl compose | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl compose opdrachten.
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
ms.openlocfilehash: d71b0c020fb9ceb305b56216d466bacb42ad21e8
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53278148"
---
# <a name="sfctl-compose"></a>sfctl compose
Maken, verwijderen en beheren van Docker Compose-toepassingen.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| maken | Hiermee maakt u een Service Fabric implementatie voor opstellen. |
| list | Haalt de lijst met implementaties die zijn gemaakt in de Service Fabric-cluster vormen. |
| verwijderen | Verwijdert een bestaande Service Fabric vormen van de cluster-implementatie. |
| status | Haalt informatie over een Service Fabric implementatie voor opstellen. |
| upgrade | Hiermee start u een upgrade van een implementatie voor opstellen in de Service Fabric-cluster. |
| upgrade terugdraaien | Start een implementatie voor opstellen terugdraaien upgraden via de Service Fabric-cluster. |
| upgrade-status | Meer informatie opgehaald voor de meest recente upgrade uitgevoerd voor deze Service Fabric implementatie voor opstellen. |

## <a name="sfctl-compose-create"></a>sfctl compose maken
Hiermee maakt u een Service Fabric implementatie voor opstellen.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-implementatie (vereist) | De naam van de implementatie. |
| --bestandspad (vereist) | Pad naar het doelbestand voor de Docker Compose. |
| --versleuteld pass | Gebruik in plaats van dat wordt gevraagd om een wachtwoord van containerregister, een al is versleuteld wachtwoordzin. |
| --is geslaagd | Wordt gevraagd om een wachtwoord voor het containerregister. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --user | De naam van de gebruiker verbinding maken met container registry. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-compose-list"></a>sfctl lijst opstellen
Haalt de lijst met implementaties die zijn gemaakt in de Service Fabric-cluster vormen.

Hiermee haalt u de status van de compose-implementaties die zijn gemaakt of dat u momenteel in de Service Fabric-cluster wordt gemaakt. Het antwoord bevat de naam, status en andere details over de implementaties opstellen. Als de lijst met implementaties niet in een pagina passen, wordt een pagina met resultaten geretourneerd en een vervolgtoken, die kan worden gebruikt om de volgende pagina.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --vervolgtoken | De voortzetting van token-parameter wordt gebruikt om op te halen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in één antwoord passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep retourneert de API volgende set met resultaten. Als er geen verdere resultaten, klikt u vervolgens bevat het vervolgtoken een waarde. De waarde van deze parameter mag geen URL-codering. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal geretourneerde resultaten. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de beperkingen van de grootte van maximaal bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de query met resultatenpagina's zo veel resultaten als is mogelijk dat de geretourneerde bericht. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-compose-remove"></a>sfctl compose remove
Verwijdert een bestaande Service Fabric vormen van de cluster-implementatie.

Verwijdert een bestaande Service Fabric implementatie voor opstellen.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-implementatie (vereist) | De identiteit van de implementatie. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-compose-status"></a>sfctl compose status
Haalt informatie over een Service Fabric implementatie voor opstellen.

Retourneert de status van de implementatie voor opstellen die is gemaakt of momenteel wordt gemaakt in de Service Fabric-cluster en waarvan de naam overeenkomt met de versie die is opgegeven als parameter. Het antwoord bevat de naam, status en andere details over de implementatie.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-implementatie (vereist) | De identiteit van de implementatie. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-compose-upgrade"></a>sfctl compose upgrade
Hiermee start u een upgrade van een implementatie voor opstellen in de Service Fabric-cluster.

Evalueert de opgegeven parameters voor het bijwerken en begint met een upgrade van de implementatie als de parameters geldig zijn.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-implementatie (vereist) | De naam van de implementatie. |
| --bestandspad (vereist) | Pad naar het doel van Docker compose bestand. |
| --Standaard-svc-type-health-map | JSON gecodeerd woordenlijst die beschrijven het statusbeleid dat wordt gebruikt voor het evalueren van de status van services. |
| --versleuteld pass | Gebruik in plaats van dat wordt gevraagd om een wachtwoord van containerregister, een al is versleuteld wachtwoordzin. |
| --actie bij fout | Mogelijke waarden zijn\: 'Ongeldig', 'Terugdraaien', 'Manual'. |
| --geforceerd opnieuw opstarten | Processen worden geforceerd opnieuw opgestart tijdens de upgrade, zelfs wanneer de codeversie is niet gewijzigd. <br><br> De upgrade wordt alleen gewijzigd configuratie of gegevens. |
| --is geslaagd | Wordt gevraagd om een wachtwoord voor het containerregister. |
| --health-controle-opnieuw proberen | De hoeveelheid tijd tussen pogingen tot het uitvoeren van statuscontroles als de toepassing of het cluster niet in orde is. |
| --health-controle-stabiel | De hoeveelheid tijd dat de toepassing of het cluster moet in orde blijft voordat de upgrade wordt uitgevoerd op het volgende upgradedomein. <br><br> Eerst wordt dit geïnterpreteerd als een tekenreeks voor de duur van een ISO 8601. Als dat mislukt, wordt klikt u vervolgens dit geïnterpreteerd als een getal voor het totale aantal milliseconden. |
| --wachtduur | De hoeveelheid tijd moet wachten na het voltooien van een upgradedomein voordat u begint met de status controleert proces. |
| --replica-set-controle | De maximale hoeveelheid tijd voor het verwerken van een upgradedomein blokkeren en voorkom het verlies van beschikbaarheid wanneer er onverwachte problemen zijn. <br><br> Wanneer deze time-out is verlopen, wordt de verwerking van het upgradedomein ongeacht beschikbaarheidsproblemen verlies voortgezet. De time-out is opnieuw ingesteld aan het begin van elke upgradedomein. Geldige waarden liggen tussen 0 en 42949672925 liggen. |
| --svc-type-health-map | JSON gecodeerd lijst met objecten die het statusbeleid dat wordt gebruikt voor het evalueren van de status van de verschillende servicetypen beschrijven. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --niet in orde-app | De maximaal toegestane percentage van de beschadigde toepassingen voordat u een foutmelding. <br><br> Bijvoorbeeld, om toe te staan 10% van de toepassingen niet in orde, is deze waarde 10. Het percentage geeft het maximum aantal verdragen percentage van toepassingen die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage in acht wordt genomen, maar er ten minste één beschadigde toepassing is, wordt de status wordt geëvalueerd als waarschuwing. Dit wordt berekend door het aantal beschadigde toepassingen delen via het totale aantal exemplaren van een toepassing in het cluster. |
| --upgrade-domein-time-out | De hoeveelheid tijd elk upgradedomein is voltooid voordat FailureAction wordt uitgevoerd. <br><br> Eerst wordt dit geïnterpreteerd als een tekenreeks voor de duur van een ISO 8601. Als dat mislukt, wordt klikt u vervolgens dit geïnterpreteerd als een getal voor het totale aantal milliseconden. |
| --upgrade-type | Standaard\: Rolling. |
| --upgrade-modus | Mogelijke waarden zijn\: 'Ongeldig', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitored'.  Standaard\: UnmonitoredAuto. |
| --upgrade-time-out | De hoeveelheid tijd de algehele upgrade is voltooid voordat FailureAction wordt uitgevoerd. <br><br> Eerst wordt dit geïnterpreteerd als een tekenreeks voor de duur van een ISO 8601. Als dat mislukt, wordt klikt u vervolgens dit geïnterpreteerd als een getal voor het totale aantal milliseconden. |
| --user | De naam van de gebruiker verbinding maken met container registry. |
| --waarschuwing als fout | Geeft aan of waarschuwingen met de dezelfde ernst als fouten worden behandeld. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-compose-upgrade-rollback"></a>sfctl compose upgrade terugdraaien
Start een implementatie voor opstellen terugdraaien upgraden via de Service Fabric-cluster.

Terugdraaien van een service fabric vormen een implementatie-upgrade.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-implementatie (vereist) | De identiteit van de implementatie. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-compose-upgrade-status"></a>sfctl compose upgrade-status
Meer informatie opgehaald voor de meest recente upgrade uitgevoerd voor deze Service Fabric implementatie voor opstellen.

Retourneert informatie over de status van de upgrade van de implementatie opstellen, samen met details kunnen helpen een foutopsporing statusproblemen van toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam-implementatie (vereist) | De identiteit van de implementatie. |
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