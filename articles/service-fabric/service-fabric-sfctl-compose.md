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
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: 3ce0b63c579412d9d8d35b835803becab09f7ef4
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494149"
---
# <a name="sfctl-compose"></a>sfctl compose
Maken, verwijderen en beheren van Docker Compose-toepassingen.

## <a name="commands"></a>Opdrachten

|Command|Beschrijving|
| --- | --- |
| maken | Hiermee maakt u een Service Fabric implementatie voor opstellen. |
| lijst | Haalt de lijst met implementaties die zijn gemaakt in de Service Fabric-cluster vormen. |
| verwijderen | Verwijdert een bestaande Service Fabric vormen van de cluster-implementatie. |
| status | Haalt informatie over een Service Fabric implementatie voor opstellen. |
| upgrade | Hiermee start u een upgrade van een implementatie voor opstellen in de Service Fabric-cluster. |
| upgrade-status | Meer informatie opgehaald voor de meest recente upgrade uitgevoerd voor deze Service Fabric implementatie voor opstellen. |

## <a name="sfctl-compose-create"></a>sfctl compose maken
Hiermee maakt u een Service Fabric implementatie voor opstellen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --naam-implementatie (vereist) | De naam van de implementatie. |
| --bestandspad (vereist) | Pad naar het doelbestand voor de Docker Compose. |
| --versleuteld pass | Gebruik in plaats van dat wordt gevraagd om een wachtwoord van containerregister, een al is versleuteld wachtwoordzin. |
| --is geslaagd | Wordt gevraagd om een wachtwoord voor het containerregister. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --user | De naam van de gebruiker verbinding maken met container registry. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
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

|Argument|Beschrijving|
| --- | --- |
| --vervolgtoken | De voortzetting van token-parameter wordt gebruikt om op te halen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in één antwoord passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep retourneert de API volgende set met resultaten. Als er geen verdere resultaten, klikt u vervolgens bevat het vervolgtoken een waarde. De waarde van deze parameter mag geen URL-codering. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal geretourneerde resultaten. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de beperkingen van de grootte van maximaal bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de query met resultatenpagina's zo veel resultaten als is mogelijk dat de geretourneerde bericht. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
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

|Argument|Beschrijving|
| --- | --- |
| --naam-implementatie (vereist) | De identiteit van de implementatie. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
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

|Argument|Beschrijving|
| --- | --- |
| --naam-implementatie (vereist) | De identiteit van de implementatie. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
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

|Argument|Beschrijving|
| --- | --- |
| --naam-implementatie (vereist) | De naam van de implementatie. |
| --bestandspad (vereist) | Pad naar het doel van Docker compose bestand. |
| --Standaard-svc-type-health-map | JSON gecodeerd woordenlijst die wordt beschreven van het statusbeleid dat wordt gebruikt voor het evalueren van de status van services. |
| --versleuteld pass | Gebruik in plaats van dat wordt gevraagd om een wachtwoord van containerregister, een al is versleuteld wachtwoordzin. |
| --actie bij fout | Mogelijke waarden zijn\: 'Ongeldig', 'Terugdraaien', 'Manual'. |
| --geforceerd opnieuw opstarten | Afdwingen dat opnieuw opstarten. |
| --is geslaagd | Wordt gevraagd om een wachtwoord voor het containerregister. |
| --health-controle-opnieuw proberen | Time-out voor de gezondheid van selectievakje opnieuw proberen, gemeten in milliseconden. |
| --health-controle-stabiel | Statuscontrole duur van stabiele gemeten in milliseconden. |
| --wachtduur | Wachttijd voor Health check gemeten in milliseconden. |
| --replica-set-controle | Upgrade controleren-timeout gemeten in seconden van replicaset. |
| --svc-type-health-map | JSON gecodeerd lijst met objecten die het statusbeleid dat wordt gebruikt voor het evalueren van de status van de verschillende servicetypen beschrijven. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --niet in orde-app | De maximaal toegestane percentage van de beschadigde toepassingen voordat u een foutmelding. <br><br> Bijvoorbeeld, om toe te staan 10% van de toepassingen niet in orde, is deze waarde 10. Het percentage geeft het maximum aantal verdragen percentage van toepassingen die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage in acht wordt genomen, maar er ten minste één beschadigde toepassing is, wordt de status wordt geëvalueerd als waarschuwing. Dit wordt berekend door het aantal beschadigde toepassingen delen via het totale aantal exemplaren van een toepassing in het cluster. |
| --upgrade-domein-time-out | Het upgradedomein gemeten in milliseconden. |
| --upgrade-type | Standaard\: Rolling. |
| --upgrade-modus | Mogelijke waarden zijn\: 'Ongeldig', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitored'.  Standaard\: UnmonitoredAuto. |
| --upgrade-time-out | Time-out voor het upgraden, gemeten in milliseconden. |
| --user | De naam van de gebruiker verbinding maken met container registry. |
| --waarschuwing als fout | Waarschuwingen worden met de dezelfde ernst als fouten behandeld. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
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

|Argument|Beschrijving|
| --- | --- |
| --naam-implementatie (vereist) | De identiteit van de implementatie. |
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