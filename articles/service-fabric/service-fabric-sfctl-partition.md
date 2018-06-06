---
title: Azure Service Fabric CLI - sfctl partition | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl partitie-opdrachten.
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
ms.openlocfilehash: a9455683c5fad7fad4dda62fd967da617d8a8496
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763643"
---
# <a name="sfctl-partition"></a>sfctl partition
Doorzoeken en beheren van partities voor elke service.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| verlies van gegevens | Deze API wordt verlies van gegevens voor de opgegeven partitie veroorzaken. |
| status van gegevens verloren gaan | Hiermee wordt de voortgang van een partitie gegevens verloren gaan bewerking gestart met de API StartDataLoss opgehaald. |
| status | Hiermee haalt u de status van de opgegeven Service Fabric-partitie. |
| informatie | Hiermee haalt u de informatie over een Service Fabric-partitie. |
| lijst | Hiermee wordt de lijst met partities van een Service Fabric-service opgehaald. |
| laden | Hiermee haalt u de informatie laden van de opgegeven Service Fabric-partitie. |
| load-reset | Hiermee stelt u de huidige belasting van een Service Fabric-partitie. |
| quorumverlies | Induceert quorumverlies voor een bepaalde stateful service-partitie. |
| quorum-verlies-status | Hiermee haalt u de voortgang van een bewerking voor het verlies van quorum in een partitie met de API StartQuorumLoss is gestart. |
| herstellen | Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van een specifieke partitie die momenteel is vastgelopen in quorumverlies. |
| herstellen-alle | Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen (inclusief systeemservices) services die momenteel zijn vastgelopen in quorumverlies. |
| rapport-status | Verzendt een statusrapport op de Service Fabric-partitie. |
| opnieuw opstarten | Deze API wordt opnieuw opgestart enkele of alle replica's of exemplaren van de opgegeven partitie. |
| opnieuw opstarten-status | Hiermee wordt de voortgang van een PartitionRestart-bewerking gestart met StartPartitionRestart opgehaald. |
| SVC-naam | Hiermee haalt u de naam van de Service Fabric-service voor een partitie. |

## <a name="sfctl-partition-data-loss"></a>sfctl partitie gegevensverlies
Deze API wordt verlies van gegevens voor de opgegeven partitie veroorzaken.

Activeert een aanroep van de API OnDataLossAsync van de partitie.  Deze API wordt verlies van gegevens voor de opgegeven partitie veroorzaken. Activeert een aanroep van de API OnDataLoss van de partitie. Werkelijke gegevensverlies is afhankelijk van de opgegeven DataLossMode. <br> PartialDataLoss - alleen een quorum van replica's zijn verwijderd en OnDataLoss wordt geactiveerd voor de partitie, maar werkelijke gegevens verloren gaan, is afhankelijk van de aanwezigheid van onderweg replicatie. <br>FullDataLoss - alle replica's verwijderd daarom de gegevens worden verwijderd en OnDataLoss wordt geactiveerd. <br>Deze API moet alleen worden aangeroepen met een stateful service als het doel. Het aanroepen van deze API met een systeemservice als het doel wordt niet aanbevolen. 
> [!NOTE]
> Wanneer deze API is aangeroepen, kan niet ongedaan gemaakt. Aanroepen van CancelOperation alleen uitvoering stoppen en interne systeemstatus opschonen. Deze herstelt gegevens niet als de opdracht is ver genoeg gevorderd om te leiden tot gegevensverlies. De API GetDataLossProgress met de dezelfde OperationId informatie geretourneerd over de bewerking gestart met deze API-aanroep.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --gegevens verloren gaan-modus [vereist] | Deze opsomming is doorgegeven aan de API StartDataLoss om aan te geven welk type gegevens verloren gaan om te veroorzaken. |
| --bewerking-id [vereist] | Een GUID die een aanroep van deze API identificeert.  Dit wordt doorgegeven in de bijbehorende GetProgress-API. |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-data-loss-status"></a>sfctl partitie-verlies-status van gegevens
Hiermee wordt de voortgang van een partitie gegevens verloren gaan bewerking gestart met de API StartDataLoss opgehaald.

Hiermee haalt u de voortgang van een gegevensbewerking verlies met StartDataLoss, met behulp van de OperationId is gestart.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --bewerking-id [vereist] | Een GUID die een aanroep van deze API identificeert.  Dit wordt doorgegeven in de bijbehorende GetProgress-API. |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-health"></a>sfctl partitie health
Hiermee haalt u de status van de opgegeven Service Fabric-partitie.

Hiermee haalt u de statusgegevens van de opgegeven partitie. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen voor de service op basis van de status is gerapporteerd. ReplicasHealthStateFilter gebruiken voor het filteren van de verzameling van objecten ReplicaHealthState op de partitie. Als u een partitie die niet in het archief health bestaat opgeeft, wordt door deze aanvraag een fout geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --uitsluiten-health-statistieken | Hiermee wordt aangegeven of de statistieken van de status moet worden geretourneerd als onderdeel van het queryresultaat. Standaard False. De statistieken tonen het aantal onderliggende items entiteiten in de status Ok, waarschuwing en fout. |
| --replica's-health-status-filter | Hiermee kunt u filteren van de verzameling van objecten ReplicaHealthState op de partitie. De waarde kan worden verkregen van leden of een bitsgewijze bewerkingen op leden van HealthStateFilter. Alleen de replica's die overeenkomen met het filter wordt geretourneerd. Alle replica's wordt gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 wordt vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-info"></a>partitiegegevens sfctl
Hiermee haalt u de informatie over een Service Fabric-partitie.

Hiermee haalt u de informatie over de opgegeven partitie. Het antwoord bevat de partitie-ID, partitionering schema-informatie, sleutels die worden ondersteund door de partitie, status, gezondheid en andere details weer over de partitie.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-list"></a>lijst met sfctl partitie
Hiermee wordt de lijst met partities van een Service Fabric-service opgehaald.

Hiermee wordt de lijst met partities van een Service Fabric-service opgehaald. Het antwoord bevat de partitie-ID, partitionering schema-informatie, sleutels die worden ondersteund door de partitie, status, gezondheid en andere details weer over de partitie.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --vervolgtoken | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-load"></a>belasting van de partitie sfctl
Hiermee haalt u de informatie laden van de opgegeven Service Fabric-partitie.

Retourneert informatie over de belasting van een opgegeven partitie. Het antwoord bevat een lijst met rapporten laden voor een Service Fabric-partitie. Elk rapport bevat de naam van metrische belastinggegevens, de waarde en de laatste gerapporteerde tijd in UTC.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-load-reset"></a>sfctl partitie load-reset
Hiermee stelt u de huidige belasting van een Service Fabric-partitie.

Hiermee stelt de huidige belasting van een Service Fabric-partitie op de standaard belasting voor de service.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-quorum-loss"></a>sfctl partitie quorumverlies
Induceert quorumverlies voor een bepaalde stateful service-partitie.

Induceert quorumverlies voor een bepaalde stateful service-partitie.  Deze API is handig voor een situatie met een tijdelijke quorum verloren gaan in de service. De API GetQuorumLossProgress met de dezelfde OperationId informatie geretourneerd over de bewerking gestart met deze API-aanroep. Dit kan alleen worden aangeroepen voor stateful persistent (HasPersistedState == true) services.  Deze API niet gebruiken op stateless services of stateful in het geheugen wordt alleen services.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --bewerking-id [vereist] | Een GUID die een aanroep van deze API identificeert.  Dit wordt doorgegeven in de bijbehorende GetProgress-API. |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --quorum verlies-duur [vereist] | De hoeveelheid tijd op waarvoor de partitie in quorumverlies worden behouden.  Dit moet worden opgegeven in seconden. |
| --quorum-verlies-modus [vereist] | Deze opsomming is doorgegeven aan de API StartQuorumLoss om aan te geven welk type quorumverlies om te veroorzaken. |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-quorum-loss-status"></a>partitie quorum-verlies sfctl-status
Hiermee haalt u de voortgang van een bewerking voor het verlies van quorum in een partitie met de API StartQuorumLoss is gestart.

Hiermee wordt de voortgang van een quorum verlies bewerking gestart met StartQuorumLoss, met behulp van de opgegeven OperationId opgehaald.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --bewerking-id [vereist] | Een GUID die een aanroep van deze API identificeert.  Dit wordt doorgegeven in de bijbehorende GetProgress-API. |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-recover"></a>sfctl partitie herstellen
Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van een specifieke partitie die momenteel is vastgelopen in quorumverlies.

Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van een specifieke partitie die momenteel is vastgelopen in quorumverlies. Deze bewerking mag alleen worden uitgevoerd als bekend is dat de replica's die niet beschikbaar zijn, kunnen niet worden hersteld. Onjuist gebruik van deze API kan leiden tot verlies van gegevens.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-recover-all"></a>sfctl partitie herstellen-alle
Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen (inclusief systeemservices) services die momenteel zijn vastgelopen in quorumverlies.

Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen (inclusief systeemservices) services die momenteel zijn vastgelopen in quorumverlies. Deze bewerking mag alleen worden uitgevoerd als bekend is dat de replica's die niet beschikbaar zijn, kunnen niet worden hersteld. Onjuist gebruik van deze API kan leiden tot verlies van gegevens.

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

## <a name="sfctl-partition-report-health"></a>sfctl partitie rapport-status
Verzendt een statusrapport op de Service Fabric-partitie.

Meldt de status van de opgegeven Service Fabric-partitie. Het rapport moet bevatten informatie over de bron van de health-rapport en de eigenschap waarop deze wordt gerapporteerd. Het rapport wordt verzonden naar een gateway Service Fabric-partitie, die worden doorgestuurd naar de health store. Het rapport kan worden geaccepteerd door de gateway, maar geweigerd door de health store na extra validatie. De health store kan bijvoorbeeld het rapport weigeren vanwege een ongeldige parameter, zoals een verouderde volgnummer. Om te zien of het rapport in de health store is toegepast, Controleer of het rapport wordt weergegeven in de sectie gebeurtenissen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --health-eigenschap [vereist] | De eigenschap van de health-informatie. <br><br> Een entiteit kan statusrapporten voor de verschillende eigenschappen hebben. De eigenschap is een tekenreeks en niet een vaste opsomming de Rapportagefout flexibiliteit om de status-voorwaarde die het rapport activeert te categoriseren toe te passen. Een Rapportagefout met SourceId 'LocalWatchdog' kunt bijvoorbeeld de status van de beschikbare schijfruimte op een knooppunt, bewaken, zodat deze 'AvailableDisk'-eigenschap op dat knooppunt kunt rapporteren. De dezelfde Rapportagefout kan de connectiviteit knooppunt bewaken, zodat dit een 'Verbinding'-eigenschap op hetzelfde knooppunt kunt rapporteren. Deze rapporten worden in het archief health behandeld als afzonderlijke health-gebeurtenissen voor het opgegeven knooppunt. Samen met de bron-id identificatie de eigenschap een unieke voor de health-informatie. |
| --status [vereist] | Mogelijke waarden zijn\: 'Ongeldig', 'Ok', 'Waarschuwing', 'Fout', 'Onbekend'. |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --bron-id [vereist] | De naam van de bron waarmee de component watchdog-client-systeem dat de statusgegevens gegenereerd. |
| --Beschrijving | De beschrijving van de health-informatie. <br><br> Hiermee geeft u de vrije tekst gebruikt voor het toevoegen van menselijke leesbare informatie over het rapport. De maximale tekenreekslengte voor de beschrijving is tekens (4096). Als de opgegeven tekenreeks langer is, worden deze automatisch afgekapt. Wanneer een afgekapt, de laatste tekens van de beschrijving bevat een markering '[Truncated]' en de tekenreeksgrootte van de totale is tekens (4096). De aanwezigheid van de markering geeft aan gebruikers die moet worden afgekapt is opgetreden. Houd er rekening mee dat wanneer afgekapt, de beschrijving is minder dan 4096 tekens uit de oorspronkelijke reeks. |
| --onmiddellijke | Een vlag die aangeeft of het rapport onmiddellijk moet worden verzonden. <br><br> Een statusrapport wordt verzonden naar een Service Fabric-gateway-toepassing die naar de health store doorstuurt. Als direct is ingesteld op true, het rapport wordt verzonden onmiddellijk door HTTP-Gateway naar de health store, ongeacht de instellingen van de fabric-client die de Gateway HTTP-toepassing wordt gebruikt. Dit is handig voor kritieke rapporten die zo snel mogelijk moeten worden verzonden. Afhankelijk van de timing en andere voorwaarden kan verzenden van het rapport nog steeds mislukken, bijvoorbeeld als de HTTP-Gateway is gesloten of het bericht de Gateway niet bereiken. Als direct is ingesteld op false, wordt het rapport verzonden op basis van de clientinstellingen voor de status van de HTTP-Gateway. Daarom deze wordt in batch worden opgenomen volgens de configuratie HealthReportSendInterval. Dit is de aanbevolen instelling omdat hiermee de health-client de statusrapportage berichten health store als verwerking van statusrapporten optimaliseren. Standaard rapporten niet onmiddellijk verzonden. |
| --verwijderen wanneer verlopen | De waarde die aangeeft of het rapport is verwijderd uit health store als deze verloopt. <br><br> Als is ingesteld op true, wordt het rapport wordt verwijderd uit de store health nadat het is verlopen. Indien ingesteld op false, het rapport wordt behandeld als een fout bij het verlopen. Deze eigenschap is standaard ingesteld op false. Wanneer clients periodiek verslag, moeten ze RemoveWhenExpired false (standaard) ingesteld. Op deze manier is de Rapportagefout heeft problemen (bijvoorbeeld impasse) en kan niet rapporteren, wordt de entiteit bij fout wordt geëvalueerd wanneer het statusrapport is verlopen. Hiermee wordt de entiteit als de status fout. |
| ---volgnummer | Het volgnummer voor dit statusrapport als een reeks cijfers. <br><br> Het volgnummer van het rapport wordt gebruikt door de health store voor het detecteren van verouderde rapporten. Als niet wordt opgegeven, is een volgnummer automatisch gegenereerd door de client health bij het toevoegen van een rapport. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |
| --ttl | De duur waarvoor deze statusrapport geldig is. Dit veld is ISO8601-indeling gebruikt voor het opgeven van de duur. <br><br> Wanneer clients periodiek rapporteert, moeten ze rapporten verzenden met een hogere frequentie dan TTL. Als clients een over overgang rapport, kunnen ze time to live op oneindig ingesteld. Wanneer TTL verloopt, de health-gebeurtenis met de health-informatie verwijderd uit health store als RemoveWhenExpired true of geëvalueerd op fout, is als RemoveWhenExpired false. Als dat niet opgegeven, levensduur standaardwaarden op oneindige waarde. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-restart"></a>sfctl partitie opnieuw opstarten
Deze API wordt opnieuw opgestart enkele of alle replica's of exemplaren van de opgegeven partitie.

Deze API is handig voor het testen van failover. Als gebruikt voor het doel van een partitie staatloze service, moet RestartPartitionMode AllReplicasOrInstances. De GetPartitionRestartProgress-API met behulp van de dezelfde OperationId ophalen van de voortgang van de aanroepen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --bewerking-id [vereist] | Een GUID die een aanroep van deze API identificeert.  Dit wordt doorgegeven in de bijbehorende GetProgress-API. |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --opnieuw opstarten-partitie-modus [vereist] | Beschrijven welke partities te starten. |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-restart-status"></a>sfctl partitie opnieuw opstarten-status
Hiermee wordt de voortgang van een PartitionRestart-bewerking gestart met StartPartitionRestart opgehaald.

Hiermee haalt u de voortgang van een PartitionRestart met StartPartitionRestart met behulp van de opgegeven OperationId is gestart.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --bewerking-id [vereist] | Een GUID die een aanroep van deze API identificeert.  Dit wordt doorgegeven in de bijbehorende GetProgress-API. |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-partition-svc-name"></a>sfctl partitie svc-naam
Hiermee haalt u de naam van de Service Fabric-service voor een partitie.

Hiermee haalt u de naam van de service voor de opgegeven partitie. Een 404-fout wordt geretourneerd als de partitie-ID niet in het cluster bestaat.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist] | De identiteit van de partitie. |
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
- [Setup](service-fabric-cli.md) de Service Fabric CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).
