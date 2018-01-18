---
title: Azure Service Fabric CLI - sfctl partition | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl partitie-opdrachten.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: 9d709a0ec2b7de985ac08fe9ee2935848e7a371c
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-partition"></a>sfctl partitie
Doorzoeken en beheren van partities voor elke service.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
|    verlies van gegevens      | Deze API induceert verlies van gegevens voor de opgegeven partitie.|
|    data-loss-status  | Hiermee wordt de voortgang van een partitie gegevens verloren gaan bewerking gestart met de API StartDataLoss opgehaald.|
|    status         | Hiermee haalt u de status van de opgegeven Service Fabric-partitie.|
|    info           | Hiermee haalt u de informatie over een Service Fabric-partitie.|
|    lijst           | Hiermee wordt de lijst met partities van een Service Fabric-service opgehaald.|
|    laden           | Hiermee haalt u de belasting van de opgegeven Service Fabric-partitie.|
|    load-reset     | Hiermee stelt u de huidige belasting van een Service Fabric-partitie.|
|    quorum-loss    | Induceert quorumverlies voor een bepaalde stateful service-partitie.|
|    quorum-loss-status| Hiermee haalt u de voortgang van een bewerking voor het verlies van quorum in een partitie met de API StartQuorumLoss is gestart.|
|    herstellen        | Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van een specifieke partitie, die momenteel is vastgelopen in quorumverlies.|
|    herstellen-alle    | Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen (inclusief systeemservices) services die momenteel zijn vastgelopen in quorumverlies.|
|    rapport-status  | Verzendt een statusrapport op de Service Fabric-partitie.|
|    opnieuw opstarten        | Deze API wordt opnieuw opgestart enkele of alle replica's of exemplaren van de opgegeven partitie.|
|    restart-status | Hiermee wordt de voortgang van een PartitionRestart-bewerking gestart met StartPartitionRestart opgehaald.|
|    svc-name       | Hiermee haalt u de naam van de Service Fabric-service voor een partitie.|


## <a name="sfctl-partition-health"></a>sfctl partitie health
Hiermee haalt u de status van de opgegeven Service Fabric-partitie.

Hiermee haalt u de statusgegevens van de opgegeven partitie. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen voor de service op basis van de status is gerapporteerd.
ReplicasHealthStateFilter gebruiken voor het filteren van de verzameling van objecten ReplicaHealthState op de partitie. Als u een partitie die niet in het archief health bestaat opgeeft, retourneert deze cmdlet een fout opgetreden. .

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist]| De identiteit van de partitie.|
| --gebeurtenissen-health-status-filter  | Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen.                Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.                -All - Filter dat overeenkomt met invoer met de waarde van een HealthState.                De waarde is 65535.|
|--uitsluiten-health-statistieken   | Hiermee wordt aangegeven of de statistieken van de status moet worden geretourneerd als onderdeel van het queryresultaat. Standaard False. De statistieken tonen het aantal onderliggende items entiteiten in de status Ok, waarschuwing en fout.|
| --replicas-health-state-filter| Hiermee kunt u filteren van de verzameling van objecten ReplicaHealthState op de partitie. De waarde kan worden verkregen van leden of een bitsgewijze bewerkingen op leden van HealthStateFilter. Alleen de replica's die overeenkomen met het filter worden geretourneerd. Alle replica's worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535.|
| --time-out -t               | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                    | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                  | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.                Standaard: json.|
| --query                    | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/. |
| --uitgebreide                  | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-partition-info"></a>partitiegegevens sfctl
Hiermee haalt u de informatie over een Service Fabric-partitie.

Het eindpunt van de partities retourneert informatie over de opgegeven partitie. Het antwoord bevat de partitie-ID, partitionering schema-informatie, sleutels die worden ondersteund door de partitie, status, gezondheid en andere details weer over de partitie.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist]| De identiteit van de partitie.|
| --time-out -t          | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h             | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query               | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-partition-list"></a>lijst met sfctl partitie
Hiermee wordt de lijst met partities van een Service Fabric-service opgehaald.

Hiermee wordt de lijst met partities van een Service Fabric-service opgehaald. Het s de partitie-ID, de partitionering schema-informatie, de sleutels die worden ondersteund door de partitie, status, gezondheid en andere details weer over de partitie.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id| De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Bijvoorbeeld, als de servicenaam is 'fabric://myapp/app1/svc1', de service-identiteit zou zijn ' myapp ~ app1 ~ svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies.|
| --vervolgtoken| De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten.         Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd.|
| --time-out -t        | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug             | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h           | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o         | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query             | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide           | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-partition-load"></a>belasting van de partitie sfctl
Hiermee haalt u de belasting van de opgegeven Service Fabric-partitie.

Retourneert informatie over de opgegeven partitie. Het antwoord bevat een lijst met informatie laden. Elke informatie bevat de naam van metrische belastinggegevens, de waarde en vorige gemelde tijd in UTC.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist]| De identiteit van de partitie.|
| --time-out -t          | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h             | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query               | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-partition-recover"></a>sfctl partitie herstellen
Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van een specifieke partitie die momenteel is vastgelopen in quorumverlies.

Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van een specifieke partitie die momenteel is vastgelopen in quorumverlies. Deze bewerking mag alleen worden uitgevoerd als bekend is dat de replica's die niet beschikbaar zijn, kunnen niet worden hersteld. Onjuist gebruik van deze API kan leiden tot verlies van gegevens.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist]| De identiteit van de partitie.|
| --time-out -t          | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h             | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query               | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-partition-restart"></a>sfctl partitie opnieuw opstarten
Deze API wordt opnieuw opgestart enkele of alle replica's of exemplaren van de opgegeven partitie.

Deze API is handig voor het testen van failover. Als gebruikt voor het doel van een partitie staatloze service, moet RestartPartitionMode AllReplicasOrInstances. De GetPartitionRestartProgress-API met behulp van de dezelfde OperationId ophalen van de voortgang van de aanroepen. .

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --bewerking-id [vereist]| Een GUID die een aanroep van deze API identificeert.  Dit wordt doorgegeven in de bijbehorende GetProgress-API.|
| --partitie-id [vereist]| De identiteit van de partitie.|
| --opnieuw opstarten-partitie-modus [vereist]| -Ongeldige: gereserveerd.  Niet doorgegeven aan de API. -AllReplicasOrInstances - alle replica's of exemplaren in de partitie worden in één keer opnieuw gestart. -OnlyActiveSecondaries - alleen voor de secundaire replica's opnieuw worden gestart. .|
| --[vereist]-service-id| De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Bijvoorbeeld, als de servicenaam is 'fabric://myapp/app1/svc1', de service-identiteit zou zijn ' myapp ~ app1 ~ svc1 ' in 6.0 + en ' myapp/app1/svc1' in de vorige v ersies vergelijken.|
| --time-out -t                    | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                         | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                       | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                     | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.                     Standaard: json.|
| --query                         | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide                       | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="next-steps"></a>Volgende stappen
- [Setup](service-fabric-cli.md) de Service Fabric CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).
