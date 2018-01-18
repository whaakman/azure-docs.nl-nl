---
title: Azure Service Fabric CLI - sfctl choas | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl chaos-opdrachten.
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
ms.openlocfilehash: dbea84511c37cf52c3d98f0247e5ce3c0b2a05c3
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-chaos"></a>sfctl chaos
Starten, stoppen en te rapporteren voor de test chaos service.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
|    rapport| Hiermee haalt u het volgende segment van het Chaos rapport op basis van de doorgegeven vervolgtoken of doorgegeven in een tijd-bereik.|
|    start | Als Chaos nog niet wordt uitgevoerd in het cluster wordt gestart met Chaos met de opgegeven in de Chaos parameters.|
|    stoppen  | Hiermee stopt u Chaos in het cluster als het al wordt uitgevoerd, anders er niets gebeurt.|


## <a name="sfctl-chaos-report"></a>sfctl chaos rapport
Hiermee haalt u het volgende segment van het Chaos rapport op basis van de doorgegeven vervolgtoken of doorgegeven in een tijd-bereik.

Kunt u de ContinuationToken als u het volgende segment van het rapport Chaos opgeven of u kunt het tijdsbereik via StartTimeUtc en EndTimeUtc opgeven, maar u niet zowel de ContinuationToken als het tijdsbereik opgeven in dezelfde aanroep. Wanneer er meer dan 100 Chaos gebeurtenissen, wordt het rapport Chaos geretourneerd in segmenten waarbij een segment niet meer dan 100 Chaos gebeurtenissen bevat. 

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --vervolgtoken| De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten bevat vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd.|
| --end--utc-tijd   | Het aantal maatstreepjes dat vertegenwoordigt de eindtijd van de periode waarvoor een Chaos rapport is gegenereerd. Neem contact op met [DateTime.Ticks eigenschap](https://msdn.microsoft.com/en-us/library/system.datetime.ticks%28v=vs.110%29) voor meer informatie over de maatstreepjes.|
| --start-time-utc | Het aantal maatstreepjes dat vertegenwoordigt de starttijd van de periode waarvoor een Chaos rapport is gegenereerd. Neem contact op met [DateTime.Ticks eigenschap](https://msdn.microsoft.com/en-us/library/system.datetime.ticks%28v=vs.110%29) voor meer informatie over de maatstreepjes.|
| --time-out -t     | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug          | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h        | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o      | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query          | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide        | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-chaos-start"></a>sfctl chaos starten
Als Chaos nog niet wordt uitgevoerd in het cluster wordt gestart met Chaos met de opgegeven in de Chaos parameters.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-type-health-beleid-kaart  | JSON gecodeerde lijst met slechte toepassingen voor specifieke toepassingstypen maximale percentage. Elke vermelding wordt als een sleutel van de naam van het toepassingstype en als een waarde een geheel getal dat het percentage MaxPercentUnhealthyApplications is gebruikt geeft voor het evalueren van de toepassingen van het type van de opgegeven toepassing.|
| --disable-move-replica-faults | Schakelt de verplaatsing primaire en secundaire fouten te verplaatsen.|
| --max-cluster-stabilization| De maximale hoeveelheid tijd te wachten totdat alle entiteiten stabiel en orde cluster.  Standaard: 60.|
| --max-concurrent-faults    | Het maximum aantal gelijktijdige fouten veroorzaakte per herhaling.           Standaard: 1.|
| --max-procent-slecht-apps  | Bij het evalueren van de gezondheid van het cluster tijdens Chaos, de de maximaal toegestane percentage van de beschadigde toepassingen voordat een foutbericht.|
| --max procent-slecht-knooppunten | Bij het evalueren van de gezondheid van het cluster tijdens Chaos, de de maximaal toegestane percentage beschadigde knooppunten voordat u een foutmelding.|
| --tijd-en-klaar              | Totale tijd (in seconden) waarvoor Chaos voordat automatisch gestopt wordt uitgevoerd. De maximaal toegestane waarde is 4.294.967.295 (System.UInt32.MaxValue).  Standaard: 4294967295.|
| --time-out -t               | Server time-out in seconden.  Standaard: 60.|
| --Wacht tijd tussen fouten | Wachttijd (in seconden) tussen opeenvolgende fouten binnen een enkele iteratie.  Standaard: 20.|
| --Wacht tijd tussen pogingen| Time-scheiding (in seconden) tussen twee opeenvolgende iteraties dank.  Standaard: 30.|
| --waarschuwing als fout         | Bij het evalueren van de gezondheid van het cluster tijdens Chaos, waarschuwingen met de dezelfde ernst als fouten behandelen.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                    | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                  | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.           Standaard: json.|
| --query                    | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide                  | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-chaos-stop"></a>sfctl chaos stoppen
Hiermee stopt u Chaos in het cluster als het al wordt uitgevoerd, anders er niets gebeurt.

Stopt Chaos uit het plannen van verdere fouten; maar de onderweg fouten worden niet beïnvloed.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --time-out -t| Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug  | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h| Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query  | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide| Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="next-steps"></a>Volgende stappen
- [Setup](service-fabric-cli.md) de Service Fabric CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).