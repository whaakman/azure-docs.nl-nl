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
ms.date: 02/22/2018
ms.author: ryanwi
ms.openlocfilehash: 34e4d47b1de509c2053996d9d1078733d7055447
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="sfctl-chaos"></a>sfctl chaos
Starten, stoppen en te rapporteren voor de test chaos service.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
|    rapport| Hiermee haalt u het volgende segment van het Chaos rapport op basis van de doorgegeven vervolgtoken of doorgegeven in een tijd-bereik.|
|    start | Start Chaos in het cluster.|
|    stoppen  | Hiermee stopt u Chaos in het cluster als het al wordt uitgevoerd, anders er niets gebeurt.|


## <a name="sfctl-chaos-report"></a>sfctl chaos rapport
Hiermee haalt u het volgende segment van het Chaos rapport op basis van de doorgegeven vervolgtoken of doorgegeven in een tijd-bereik.

Kunt u de ContinuationToken als u het volgende segment van het rapport Chaos opgeven of u kunt het tijdsbereik via StartTimeUtc en EndTimeUtc opgeven, maar u niet zowel de ContinuationToken als het tijdsbereik opgeven in dezelfde aanroep. Wanneer er meer dan 100 Chaos gebeurtenissen, wordt het rapport Chaos geretourneerd in segmenten waarbij een segment niet meer dan 100 Chaos gebeurtenissen bevat. 

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --vervolgtoken| De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten bevat vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd.|
| --end--utc-tijd   | De Windows-bestand van tijd voor de eindtijd van de periode waarvoor een Chaos rapport is gegenereerd. Neem contact op met [DateTime.ToFileTimeUtc methode](https://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) voor meer informatie.|
| --start-time-utc | De Windows-bestand van tijd voor de begintijd van de periode waarvoor een Chaos rapport is gegenereerd. Neem contact op met [DateTime.ToFileTimeUtc methode](https://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) voor meer informatie.|
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
Start Chaos in het cluster.

Als Chaos nog niet in het cluster wordt uitgevoerd, deze Chaos begint met de doorgegeven in parameters Chaos. Als Chaos al wordt uitgevoerd wanneer deze aanroep wordt gedaan, wordt de aanroep mislukt met foutcode FABRIC_E_CHAOS_ALREADY_RUNNING.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-type-health-beleid-kaart  | JSON gecodeerde lijst met slechte toepassingen voor specifieke toepassingstypen maximale percentage. Elke vermelding wordt als een sleutel van de naam van het toepassingstype en als een waarde een geheel getal dat het percentage MaxPercentUnhealthyApplications is gebruikt geeft voor het evalueren van de toepassingen van het type van de opgegeven toepassing. Hiermee definieert u een kaart met slechte toepassingen voor specifieke toepassingstypen maximale percentage. Elke vermelding wordt als de naam van het toepassingstype sleutel en als een geheel getal dat staat voor het percentage MaxPercentUnhealthyApplications is gebruikt voor het evalueren van de toepassingen van het type van de opgegeven waarde. De toepassing type health beleid kaart kan worden gebruikt tijdens de evaluatie van de cluster-status te beschrijven van speciale toepassingstypen. De toepassingstypen die is opgenomen in de kaart worden geëvalueerd op basis van het percentage dat is opgegeven in de kaart en niet met de globale MaxPercentUnhealthyApplications gedefinieerd in het cluster-statusbeleid. De toepassingen van toepassingstypen die zijn opgegeven in de kaart worden niet meegeteld tegen de algemene groep van toepassingen. Bijvoorbeeld, als sommige toepassingen van een type kritiek zijn, Clusterbeheer kan een vermelding toevoegen aan de kaart voor dat toepassingstype en wijs hieraan een waarde van 0% (dat wil zeggen, komen niet tolereren fouten). Alle andere toepassingen kunnen worden geëvalueerd, MaxPercentUnhealthyApplications ingesteld op 20% om een aantal fouten buiten de duizenden toepassingsexemplaren tolereren. De toepassing type health beleid kaart wordt alleen gebruikt als het clustermanifest kunt toepassing type health evaluatie met behulp van de configuratie-item voor HealthManager/EnableApplicationTypeHealthEvaluation.|
|--chaos-target-filter         | JSON gecodeerde woordenlijst met twee tekenreekssleutels type. De twee sleutels zijn NodeTypeInclusionList en ApplicationInclusionList. Waarden voor beide sleutels zijn lijst met tekenreeksen. chaos_target_filter definieert alle filters voor gerichte Chaos fouten, bijvoorbeeld alleen bepaalde knooppunttypen afgesloten of alleen bepaalde toepassingen wordt afgesloten. Als chaos_target_filter niet gebruikt wordt, bedrijfsstoringen Chaos alle entiteiten van het cluster. Als chaos_target_filter wordt gebruikt, bedrijfsstoringen Chaos alleen de entiteiten die voldoen aan de chaos_target_filter-specificatie. NodeTypeInclusionList en ApplicationInclusionList kunt alleen een union semantiek. Het is niet mogelijk om op te geven van een snijpunt van NodeTypeInclusionList en ApplicationInclusionList. Het is bijvoorbeeld niet mogelijk om op te geven "fault deze toepassing alleen wanneer deze zich op dit knooppunttype." Zodra een entiteit is opgenomen in NodeTypeInclusionList of ApplicationInclusionList, kan niet die entiteit worden uitgesloten ChaosTargetFilter gebruiken. Zelfs als applicationX niet wordt weergegeven in ApplicationInclusionList, in sommige iteratie Chaos kunt applicationX mislukt omdat dit gebeurt op een knooppunt van nodeTypeY die is opgenomen in NodeTypeInclusionList. Als zowel NodeTypeInclusionList als ApplicationInclusionList leeg zijn, wordt een ArgumentException gegenereerd. Alle soorten fouten (knooppunt opnieuw opstarten, codepakket niet opnieuw starten, replica verwijderen, opnieuw opstarten replica, verplaatsen primaire en secundaire verplaatsen) zijn ingeschakeld voor de knooppunten van de volgende knooppunttypen. Als een knooppunttype (spreken NodeTypeX) niet wordt weergegeven in de NodeTypeInclusionList en vervolgens niveau fouten knooppunt (zoals NodeRestart) voor de knooppunten van NodeTypeX nooit worden ingeschakeld, maar het pakket en replica fouten code kunnen nog steeds worden ingeschakeld voor NodeTypeX als een toepassing in de ApplicationInclusionList gebeurt zich bevinden op een knooppunt van NodeTypeX. Maximaal 100 knooppunt Typenamen kunnen worden opgenomen in deze lijst, dit aantal vergroten, een upgrade van de configuratie is vereist voor MaxNumberOfNodeTypesInChaosEntityFilter configuratie. Alle replica's die horen bij de services van deze toepassingen zijn voorwerp replica fouten (opnieuw opstarten replica, replica verwijderen, verplaatsen primaire en secundaire verplaatsen) door Chaos. Chaos mogelijk opnieuw opstarten van een codepakket alleen als het codepakket fungeert als host van de replica's van deze toepassingen alleen. Als een toepassing niet in deze lijst wordt weergegeven, kan deze nog steeds mislukt in sommige iteratie Chaos als de toepassing zich op een knooppunt van een knooppunttype dat is opgenomen in NodeTypeInclusionList belandt. Echter als applicationX is gekoppeld aan nodeTypeY via plaatsingsbeperkingen en applicationX afwezig is van ApplicationInclusionList en nodeTypeY is afwezig van NodeTypeInclusionList, klikt u vervolgens applicationX nooit mislukt. Maximaal 1000 toepassingsnamen kunnen worden opgenomen in deze lijst, dit aantal vergroten, een upgrade van de configuratie is vereist voor MaxNumberOfApplicationsInChaosEntityFilter configuratie.|
|--context                     | JSON gecodeerde-kaart van (string, string) typt u sleutel-waardeparen. De kaart kan worden gebruikt voor het vastleggen van informatie over het Chaos-run. Er mag niet meer dan 100 dergelijke paren en elke tekenreeks (sleutel of waarde) mag hoogstens 4095 tekens bevatten. Deze kaart is ingesteld door de starter de dank uitvoeren voor het opslaan van de context over de specifieke uitvoeren (optioneel).|
| --disable-move-replica-faults | Schakelt de verplaatsing primaire en secundaire fouten te verplaatsen.|
| --max-cluster-stabilization| De maximale hoeveelheid tijd te wachten totdat alle entiteiten stabiel en orde cluster.  Standaard: 60. Chaos wordt uitgevoerd in iteraties en aan het begin van elke herhaling wordt de status van de cluster-entiteiten gevalideerd. Als de entiteit van een cluster niet stabiel en orde binnen de MaxClusterStabilizationTimeoutInSeconds is, Chaos tijdens de validatie genereert een mislukte validatie-gebeurtenis.|
| --max-concurrent-faults    | Het maximum aantal gelijktijdige fouten veroorzaakte per herhaling.           Standaard: 1. Chaos wordt uitgevoerd in iteraties en twee opeenvolgende herhalingen worden gescheiden door een validatiefase. Hoe hoger de concurrency, de agressievere injectie storingen--complexere reeks statussen om fouten veroorzaken. De aanbeveling is om te beginnen met een waarde 2 of 3 en wees voorzichtig bij het verplaatsen van.|
| --max-procent-slecht-apps  | Bij het evalueren van de gezondheid van het cluster tijdens Chaos, de de maximaal toegestane percentage van de beschadigde toepassingen voordat een foutbericht. Het maximaal toegestane percentage van de beschadigde toepassingen voordat u een foutmelding. Bijvoorbeeld, zodat 10% van toepassingen beschadigd is deze waarde 10. Het percentage vertegenwoordigt het verdragen maximumpercentage van toepassingen die beschadigd worden kunnen voordat het cluster wordt beschouwd als fout. Als het percentage is voldaan, maar er ten minste één slecht toepassingstype is, worden de status wordt geëvalueerd als de waarschuwing. Dit wordt berekend door het aantal beschadigde toepassingen delen via het totale aantal exemplaren van een toepassing in het cluster, met uitzondering van toepassingen van toepassingstypen die in de applicationtypehealthpolicymap; deze zijn opgenomen. De berekening rondt af naar boven op een storing op een klein aantal toepassingen tolereren. Standaardpercentage gelijk is aan nul.|
| --max procent-slecht-knooppunten | Bij het evalueren van de gezondheid van het cluster tijdens Chaos, de de maximaal toegestane percentage beschadigde knooppunten voordat u een foutmelding. Het maximaal toegestane percentage van slecht knooppunten voordat u een foutmelding. Bijvoorbeeld, zodat 10% van de knooppunten beschadigd is deze waarde 10. Het percentage geeft de maximale verdragen percentage van de knooppunten die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage is voldaan, maar er ten minste één slecht knooppunt is, wordt de status wordt geëvalueerd als waarschuwing. Het percentage wordt berekend door het aantal beschadigde knooppunten delen via het totale aantal knooppunten in het cluster. De berekening rondt af naar boven op een storing op een klein aantal knooppunten tolereren. Standaardpercentage gelijk is aan nul. In grote clusters sommige knooppunten altijd worden omlaag of uit voor reparaties, zodat dit percentage moet worden geconfigureerd voor die tolereren.|
| --tijd-en-klaar              | Totale tijd (in seconden) waarvoor Chaos voordat automatisch gestopt wordt uitgevoerd. De maximaal toegestane waarde is 4.294.967.295 (System.UInt32.MaxValue).  Standaard: 4294967295.|
| --time-out -t               | Server time-out in seconden.  Standaard: 60.|
| --Wacht tijd tussen fouten | Wachttijd (in seconden) tussen opeenvolgende fouten binnen een enkele iteratie.  Standaard: 20. Hoe hoger de waarde, hoe lager de overlappende tussen fouten en de meest eenvoudige de volgorde van de status verandert die het cluster doorloopt. Aangeraden wordt om te beginnen met een waarde tussen 1 en 5 en oefening waarschuwing tijdens het verplaatsen van.|
| --Wacht tijd tussen pogingen| Time-scheiding (in seconden) tussen twee opeenvolgende iteraties dank. Hoe hoger de waarde, hoe lager injectie frequentie van fouten. Standaard: 30.|
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