---
title: Azure Service Fabric CLI - sfctl knooppunt | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl knooppunt-opdrachten.
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
ms.date: 02/22/2018
ms.author: ryanwi
ms.openlocfilehash: 50c7fe38d8bf7b14adf437f85c758e465e7d231d
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="sfctl-node"></a>sfctl node
Beheren van de knooppunten die fungeren als een cluster wordt gevormd.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
|    uitschakelen       | Een Service Fabric-clusterknooppunt met de opgegeven deactivering wordt intentie deactiveren.|
|    inschakelen        | Activeren van een Service Fabric-clusterknooppunt is gedeactiveerd.|
|    status        | Hiermee haalt u de status van een Service Fabric-knooppunt.|
|    info          | Hiermee haalt de informatie over een specifiek knooppunt in het Service Fabric-cluster.|
|    lijst          | Hiermee haalt de lijst met knooppunten in het Service Fabric-cluster.|
|    laden          | Hiermee haalt u de informatie laden van een Service Fabric-knooppunt.|
|    status verwijderen  | Service Fabric waarschuwt dat de permanente status op een knooppunt is permanent verwijderd of verloren.|
|    rapport-status | Verzendt een statusrapport van het Service Fabric-knooppunt.|
|    opnieuw opstarten       | Start opnieuw op het knooppunt van een Service Fabric-cluster.|
|    overgang    | Start of stopt een clusterknooppunt.|
|    overgang-status| Hiermee haalt u de voortgang van een bewerking met StartNodeTransition gestart.|


## <a name="sfctl-node-disable"></a>sfctl knooppunt uitschakelen
Een Service Fabric-clusterknooppunt met de opgegeven deactivering wordt intentie deactiveren.

Een Service Fabric-clusterknooppunt met de opgegeven deactivering wordt intentie deactiveren. Zodra de deactivering uitgevoerd wordt, de intent voor deactivering van kan worden verhoogd, maar niet wordt verlaagd (bijvoorbeeld een knooppunt dat is geïntegreerd met de bedoeling onderbreken worden gedeactiveerd verder met opnieuw opstarten, maar niet andersom. Knooppunten kunnen opnieuw worden geactiveerd met behulp van het activeren van een knooppunt bewerking elk gewenst moment nadat ze zijn gedeactiveerd. Als de deactivering niet voltooid is hierdoor wordt de deactivering geannuleerd. Een knooppunt dat uitvalt en opnieuw wordt weergegeven, terwijl gedeactiveerd moet nog opnieuw worden geactiveerd voordat services kunnen op dat knooppunt worden geplaatst.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist]| De naam van het knooppunt.|
| --intent voor deactivering | Het doel of de reden voor het deactiveren van het knooppunt beschreven. |
| --time-out -t       | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug            | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h          | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o        | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query            | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide          | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-node-enable"></a>sfctl knooppunt inschakelen
Activeren van een Service Fabric-clusterknooppunt die momenteel wordt gedeactiveerd.

Hiermee activeert u een Service Fabric-clusterknooppunt die momenteel wordt gedeactiveerd. Zodra geactiveerd, het knooppunt opnieuw een praktische doel wordt voor de plaatsing van de nieuwe replica's en een gedeactiveerde replica's resterende op het knooppunt opnieuw worden geactiveerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist]| De naam van het knooppunt.|
| --time-out -t       | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug            | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h          | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o        | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query            | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide          | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-node-health"></a>sfctl knooppunt health
Hiermee haalt u de status van een Service Fabric-knooppunt.

Hiermee haalt u de status van een Service Fabric-knooppunt. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen op het knooppunt op basis van de status is gerapporteerd. Als het knooppunt dat u met de naam opgeeft niet in de health store bestaat, moet dit een fout geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist]| De naam van het knooppunt.|
| --gebeurtenissen-health-status-filter| Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535.|
| --time-out -t             | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                  | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o              | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query                  | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide                | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-node-info"></a>sfctl knooppunt info
Hiermee haalt de informatie over een specifiek knooppunt in het Service Fabric-cluster.

Hiermee haalt u de informatie over een bepaald knooppunt in het Service Fabric-Cluster.The antwoord bevatten de naam, status, -id, status, bedrijfstijd en andere details over het knooppunt.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist]| De naam van het knooppunt.|
| --time-out -t       | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug            | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h          | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o        | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query            | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide          | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-node-list"></a>lijst met sfctl knooppunten
Hiermee haalt de lijst met knooppunten in het Service Fabric-cluster.

Hiermee haalt de lijst met knooppunten in het Service Fabric-cluster. Het antwoord bevatten de naam, status, -id, status, bedrijfstijd en andere details over het knooppunt.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --vervolgtoken| De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen.      Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd.|
| --knooppunt Statusfilter| Hiermee kunt u op de knooppunten op basis van de NodeStatus filteren. Alleen de knooppunten die zijn die overeenkomt met de opgegeven filterwaarde worden geretourneerd. De filterwaarde zijn. Standaard: standaard.|
| --time-out -t     | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug          | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h        | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o      | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query          | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide        | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-node-load"></a>sfctl knooppunt laden
Hiermee haalt u de informatie laden van een Service Fabric-knooppunt.

Haalt de informatie laden van een Service Fabric-knooppunt van de metrische gegevens die load of capaciteit gedefinieerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist]| De naam van het knooppunt.|
| --time-out -t       | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug            | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h          | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o        | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query            | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide          | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-node-restart"></a>sfctl knooppunt opnieuw opstarten
Start opnieuw op het knooppunt van een Service Fabric-cluster.

Start opnieuw op het knooppunt van een Service Fabric-cluster die al is gestart.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist]| De naam van het knooppunt.|
| --create-fabric-dump  | Geef waar voor het maken van een dump van het proces fabric-knooppunt. Dit is hoofdlettergevoelig.  Standaard: False.|
| --knooppunt-exemplaar-id | De exemplaar-ID van het doelknooppunt. Als exemplaar-ID van het knooppunt opnieuw wordt gestart alleen als deze met het huidige exemplaar van het knooppunt overeenkomt is opgegeven. Een standaardwaarde van '0' overeenkomen met een exemplaar-ID. De exemplaar-ID kan worden verkregen met behulp van de query voor get-knooppunt.  Standaard: 0.|
| --time-out -t       | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug            | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h          | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o        | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query            | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide          | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-node-transition"></a>sfctl knooppunt overgang
Start of stopt een clusterknooppunt.

Start of stopt een clusterknooppunt.  Een clusterknooppunt is een proces, niet de OS-exemplaar zelf.
In 'Start' voor de parameter NodeTransitionType doorgeven voor het starten van een knooppunt. Om te stoppen op een knooppunt, doorgeven in 'Stop' voor de parameter NodeTransitionType. Deze API wordt de bewerking - gestart wanneer de API-retourneert het knooppunt kan niet klaar bent met nog een overgang. Aanroepen GetNodeTransitionProgress met de dezelfde OperationId ophalen van de voortgang van de bewerking. 

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppunt-exemplaar-id [vereist]| De exemplaar-ID voor het knooppunt van het doelknooppunt. Dit kan worden bepaald via GetNodeInfo API.|
| --knooppuntnaam [vereist]| De naam van het knooppunt.|
| ---overgang-knooppunttype [vereist]| Geeft het type van overgang om uit te voeren.                       NodeTransitionType.Start wordt gestart van een knooppunt gestopt.                       NodeTransitionType. Stop stopt een knooppunt of actief is. |
| --bewerking-id [vereist]| Een GUID die een aanroep van deze API identificeert.  Dit wordt doorgegeven in de bijbehorende GetProgress-API.|
| --stop-duur-in-seconden [vereist]| De duur in seconden, dat het knooppunt is gestopt.  De minimumwaarde is 600, het maximum is 14400. Nadat deze tijd is verstreken, terugkomt het knooppunt automatisch.|
| --time-out -t                      | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                           | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                         | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                       | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.                       Standaard: json.|
| --query                           | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide                         | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="next-steps"></a>Volgende stappen
- [Setup](service-fabric-cli.md) de Service Fabric CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).