---
title: Azure Service Fabric CLI - sfctl opstellen | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl opstellen opdrachten.
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
ms.openlocfilehash: cc3d3e35ce3dd457d981dfe9420be765cf9fc45a
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763405"
---
# <a name="sfctl-compose"></a>sfctl compose
Maken, verwijderen en beheren van Docker Compose toepassingen.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| maken | Hiermee maakt u een Service Fabric implementatie opstellen. |
| lijst | Hiermee wordt de lijst met implementaties die zijn gemaakt in het Service Fabric-cluster opstellen opgehaald. |
| verwijderen | Hiermee verwijdert u een bestaande Service Fabric samenstellen van de cluster-implementatie. |
| status | Haalt informatie over een Service Fabric opstellen implementatie. |
| upgrade | Hiermee start u een upgrade van een implementatie opstellen in het Service Fabric-cluster. |
| upgrade-status | Meer informatie opgehaald voor de meest recente upgrade uitgevoerd voor deze Service Fabric opstellen implementatie. |

## <a name="sfctl-compose-create"></a>sfctl opstellen maken
Hiermee maakt u een Service Fabric implementatie opstellen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --implementatienaam [vereist] | De naam van de implementatie. |
| --bestandspad [vereist] | Pad naar het doelbestand voor de Docker Compose. |
| --versleuteld op te geven | Gebruik in plaats van naar een container register wachtwoord wordt gevraagd een al is versleuteld wachtwoordzin. |
| --heeft op te geven | Wordt gevraagd om een wachtwoord in het register van de container. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |
| --user | De naam van de gebruiker verbinding maken met container register. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-compose-list"></a>sfctl lijst opstellen
Hiermee wordt de lijst met implementaties die zijn gemaakt in het Service Fabric-cluster opstellen opgehaald.

Hiermee haalt u de status over de opstellen-implementaties die zijn gemaakt of nog moet worden gemaakt in het Service Fabric-cluster. Het antwoord bevat de naam, status en andere details over de implementaties opstellen. Als de lijst met implementaties niet in een pagina passen, wordt één pagina met resultaten geretourneerd en een vervolgtoken die kan worden gebruikt om op te halen van de volgende pagina.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --vervolgtoken | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal resultaten geretourneerd. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de groottebeperkingen max bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de paged query zo veel resultaten mogelijk die in het retourbericht passen. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-compose-remove"></a>sfctl opstellen verwijderen
Hiermee verwijdert u een bestaande Service Fabric samenstellen van de cluster-implementatie.

Hiermee verwijdert u een bestaande Service Fabric opstellen implementatie.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --implementatienaam [vereist] | De identiteit van de implementatie. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-compose-status"></a>sfctl opstellen status
Haalt informatie over een Service Fabric opstellen implementatie.

Retourneert de status van de implementatie opstellen die is gemaakt of het proces wordt gemaakt in het Service Fabric-cluster en waarvan de naam overeenkomt met de versie die is opgegeven als parameter. Het antwoord bevat de naam, status en andere details over de implementatie.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --implementatienaam [vereist] | De identiteit van de implementatie. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-compose-upgrade"></a>sfctl opstellen upgrade
Hiermee start u een upgrade van een implementatie opstellen in het Service Fabric-cluster.

Evalueert de opgegeven parameters voor het bijwerken en begint met het upgraden van de implementatie als de parameters geldig zijn.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --implementatienaam [vereist] | De naam van de implementatie. |
| --bestandspad [vereist] | Pad naar het doel van Docker compose bestand. |
| --Standaard-svc-type-health-kaart | JSON gecodeerde woordenlijst die het statusbeleid dat wordt gebruikt beschrijft voor het evalueren van de status van services. |
| --versleuteld op te geven | Gebruik in plaats van naar een container register wachtwoord wordt gevraagd een al is versleuteld wachtwoordzin. |
| --actie bij fout | Mogelijke waarden zijn\: 'Ongeldig', 'Terugdraaiactie', 'Manual'. |
| --geforceerd opnieuw opstarten | Geforceerd opnieuw opstarten. |
| --heeft op te geven | Wordt gevraagd om een wachtwoord in het register van de container. |
| --health-controle-probeer het opnieuw | Health selectievakje opnieuw time-out gemeten in milliseconden. |
| --health-controle-stabiel | Serverstatus controleren stabiele duur van de gemeten in milliseconden. |
| --wachtduur | Health controle wacht duur gemeten in milliseconden. |
| --replica set controleren | Upgrade replicaset selectievakje time-out gemeten in seconden. |
| --svc-type-health-kaart | JSON gecodeerde lijst met objecten die een beschrijving van het statusbeleid dat wordt gebruikt voor het evalueren van de status van verschillende typen. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |
| --slecht-app | Het maximaal toegestane percentage van de beschadigde toepassingen voordat u een foutmelding. <br><br> Bijvoorbeeld, zodat 10% van toepassingen beschadigd is deze waarde 10. Het percentage vertegenwoordigt het verdragen maximumpercentage van toepassingen die beschadigd worden kunnen voordat het cluster wordt beschouwd als fout. Als het percentage is voldaan, maar er ten minste één slecht toepassingstype is, worden de status wordt geëvalueerd als de waarschuwing. Dit wordt berekend door het aantal beschadigde toepassingen delen via het totale aantal exemplaren van een toepassing in het cluster. |
| --time-out-domein-upgrade | Time-out upgradedomein gemeten in milliseconden. |
| --upgrade-type | Standaard\: Rolling. |
| --upgrade-modus | Mogelijke waarden zijn\: 'Ongeldig', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitored'.  Standaard\: UnmonitoredAuto. |
| --upgrade-time-out | Time-out van de upgrade, gemeten in milliseconden. |
| --user | De naam van de gebruiker verbinding maken met container register. |
| --waarschuwing als fout | Waarschuwingen worden met de ernst van de dezelfde fouten behandeld. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-compose-upgrade-status"></a>sfctl opstellen upgrade-status
Meer informatie opgehaald voor de meest recente upgrade uitgevoerd voor deze Service Fabric opstellen implementatie.

Retourneert de gegevens over de status van de upgrade van de implementatie opstellen samen met details om u te helpen bij foutopsporing toepassingsproblemen health.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --implementatienaam [vereist] | De identiteit van de implementatie. |
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