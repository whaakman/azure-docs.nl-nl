---
title: Azure Service Fabric CLI - sfctl replica | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl replica-opdrachten.
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
ms.openlocfilehash: 422c19dfa9a204d98a898f76bc1af92a05c054d0
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-replica"></a>sfctl replica
De replica's die deel uitmaken van servicepartities beheren.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
|    geïmplementeerd  | Hiermee haalt u de details van replica is geïmplementeerd op een Service Fabric-knooppunt.|
|    geïmplementeerd lijst| Hiermee haalt u de lijst van replica's die zijn geïmplementeerd op een Service Fabric-knooppunt.|
|    status    | Hiermee haalt u de status van een Service Fabric stateful service replica of stateless service-exemplaar.|
|    info      | Hiermee haalt u de informatie over een replica van een Service Fabric-partitie.|
|    lijst      | Hiermee haalt u de informatie over de replica's van de partitie van een Service Fabric-service.|
|    verwijderen    | Hiermee verwijdert u de replica van een service uitgevoerd op een knooppunt.|
|    rapport-status| Verzendt een statusrapport voor de Service Fabric-replica.|
|    opnieuw opstarten   | De replica van een service van een permanente service uitgevoerd op een knooppunt opnieuw is opgestart.|


## <a name="sfctl-replica-deployed"></a>sfctl replica geïmplementeerd
Hiermee haalt u de details van replica is geïmplementeerd op een Service Fabric-knooppunt.

Hiermee haalt u de details van de replica die is geïmplementeerd op een Service Fabric-knooppunt. De gegevens omvatten service type, de servicenaam, de huidige servicebewerking, de huidige servicebewerking start datum / tijd, partitie-ID, replica-exemplaar-ID, gemelde laden en andere informatie.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist]| De naam van het knooppunt.|
| --partitie-id [vereist]| De identiteit van de partitie.|
| --replica-id [vereist]| De id van de replica.|
| --time-out -t          | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h             | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query               | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-replica-health"></a>sfctl replica health
Hiermee haalt u de status van een Service Fabric stateful service replica of stateless service-exemplaar.

Hiermee haalt u de status van een Service Fabric-replica. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen die zijn gerapporteerd voor de replica op basis van de status. .

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist]| De identiteit van de partitie.|
| --replica-id [vereist]| De id van de replica.|
| --gebeurtenissen-health-status-filter| Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarde die is verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535.|
| --time-out -t             | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                  | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o              | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query                  | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide                | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-replica-info"></a>sfctl replica info
Hiermee haalt u de informatie over een replica van een Service Fabric-partitie.

De respons bevatten de ID, rol, status, status, knooppuntnaam, bedrijfstijd en andere details weer over de replica.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist]| De identiteit van de partitie.|
| --replica-id [vereist]| De id van de replica.|
| --vervolgtoken  | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten bevat vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd.|
| --time-out -t          | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h             | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query               | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-replica-list"></a>lijst met sfctl replica 's
Hiermee haalt u de informatie over de replica's van de partitie van een Service Fabric-service.

Het eindpunt GetReplicas retourneert informatie over de replica's van de opgegeven partitie.
De respons bevatten de ID, rol, status, status, knooppuntnaam, bedrijfstijd en andere details weer over de replica.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist]| De identiteit van de partitie.|
| --vervolgtoken  | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten bevat vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd.|
| --time-out -t          | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h             | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query               | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-replica-remove"></a>sfctl replica verwijderen
Hiermee verwijdert u de replica van een service uitgevoerd op een knooppunt.

Deze API wordt gesimuleerd van een Service Fabric-replica-fout door het verwijderen van een replica van een Service Fabric-cluster. De verwijdering wordt de replica wordt gesloten, de replica aan de rol overgangen None, en vervolgens verwijdert alle van de informatie over de status van de replica uit het cluster. Deze API gecontroleerd van het pad naar replica de status verwijderen en simuleert het permanente Rapportpad voor fouttolerantie via de client-API's. Waarschuwing - Er zijn geen controles veiligheid wanneer deze API wordt gebruikt. Onjuist gebruik van deze API kan leiden tot verlies van gegevens voor stateful services. Bovendien is de vlag forceRemove geldt voor alle andere replica's die in hetzelfde proces worden gehost.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist]| De naam van het knooppunt.|
| --partitie-id [vereist]| De identiteit van de partitie.|
| --replica-id [vereist]| De id van de replica.|
| --force-remove        | Verwijder een Service Fabric-toepassing of service geforceerd zonder tussenkomst van de reeks correct afsluiten. Deze parameter kan worden gebruikt geforceerd verwijderen van een toepassing of service voor welke verwijderen krijgt een time-out vanwege problemen met de code die voorkomt dat correcte sluit van replica's.|
| --time-out -t          | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h             | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query               | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-replica-restart"></a>sfctl replica opnieuw opstarten
De replica van een service van een permanente service uitgevoerd op een knooppunt opnieuw is opgestart.

De replica van een service van een permanente service uitgevoerd op een knooppunt opnieuw is opgestart. Waarschuwing - Er zijn geen controles veiligheid wanneer deze API wordt gebruikt. Onjuist gebruik van deze API kan leiden tot verlies van beschikbaarheid voor stateful services.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist]| De naam van het knooppunt.|
| --partitie-id [vereist]| De identiteit van de partitie.|
| --replica-id [vereist]| De id van de replica.|
| --time-out -t          | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h             | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query               | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="next-steps"></a>Volgende stappen
- [Setup](service-fabric-cli.md) de Service Fabric CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).
