---
title: Azure Service Fabric CLI - sfctl chaos | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl chaos-opdrachten.
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
ms.openlocfilehash: 68799fe2755d8c0e811ae217f2cf6ceb84c7d869
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494223"
---
# <a name="sfctl-chaos"></a>sfctl chaos
Starten, stoppen en rapporteren over de chaos-test-service.

## <a name="subgroups"></a>Subgroepen
|Subgroep|Beschrijving|
| --- | --- |
| [schedule](service-fabric-sfctl-chaos-schedule.md) | Ophalen en instellen van de planning van chaos. |
## <a name="commands"></a>Opdrachten

|Command|Beschrijving|
| --- | --- |
| gebeurtenissen | Hiermee haalt u het volgende segment van de Chaos-gebeurtenissen op basis van het vervolgtoken of het tijdsbereik. |
| Toevoegen | De status van Chaos ophalen. |
| start | Chaos start in het cluster. |
| Stoppen | Chaos stopt als deze wordt uitgevoerd in het cluster en de planning Chaos in een status ' gestopt plaatsen '. |

## <a name="sfctl-chaos-events"></a>sfctl chaos-gebeurtenissen
Hiermee haalt u het volgende segment van de Chaos-gebeurtenissen op basis van het vervolgtoken of het tijdsbereik.

Als u het volgende segment van de Chaos-gebeurtenissen, kunt u de ContinuationToken opgeven. Als u het begin van een nieuw segment van Chaos-gebeurtenissen, kunt u het tijdsbereik via StartTimeUtc en EndTimeUtc opgeven. U kunt zowel de ContinuationToken en het tijdsbereik niet opgeven in de aanroep van dezelfde. Wanneer er meer dan 100 Chaos-gebeurtenissen zijn, de Chaos gebeurtenissen geretourneerd in meerdere segmenten waarin een segment niet langer zijn dan 100 Chaos-gebeurtenissen bevat, zodat het volgende segment maakt u een aanroep van deze API met het vervolgtoken.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --vervolgtoken | De voortzetting van token-parameter wordt gebruikt om op te halen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in één antwoord passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep retourneert de API volgende set met resultaten. Als er geen verdere resultaten, klikt u vervolgens bevat het vervolgtoken een waarde. De waarde van deze parameter mag geen URL-codering. |
| --utc-time-end | De Windows-bestand tijd voor de eindtijd van de periode waarvoor een Chaos-rapport wordt gegenereerd. Raadpleeg [DateTime.ToFileTimeUtc methode](https\://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) voor meer informatie. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal geretourneerde resultaten. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de beperkingen van de grootte van maximaal bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de query met resultatenpagina's zo veel resultaten als is mogelijk dat de geretourneerde bericht. |
| --utc-time-start | De Windows-bestand tijd voor de begintijd van de periode waarvoor een Chaos-rapport wordt gegenereerd. Raadpleeg [DateTime.ToFileTimeUtc methode](https\://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) voor meer informatie. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-chaos-get"></a>sfctl chaos ophalen
De status van Chaos ophalen.

De status van Chaos waarmee wordt aangegeven of Chaos wordt uitgevoerd, wordt de Chaos-parameters voor het uitvoeren van Chaos en de status van de planning Chaos ophalen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-chaos-start"></a>sfctl chaos start
Chaos start in het cluster.

Als Chaos niet al wordt uitgevoerd in het cluster, het Chaos begint met het doorgegeven in de Chaos-parameters. Als Chaos al wordt uitgevoerd wanneer deze oproep wordt gedaan, wordt de aanroep mislukt met foutcode FABRIC_E_CHAOS_ALREADY_RUNNING.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-type-health-policy-toewijzing | JSON gecodeerd lijst met maximale percentage beschadigde toepassingen voor specifieke typen. Elke vermelding bevat als sleutel de typenaam van de toepassing en als een waarde een geheel getal dat staat voor het percentage MaxPercentUnhealthyApplications is gebruikt voor het evalueren van de toepassingen van het opgegeven type. <br><br> Hiermee definieert u een kaart met het maximale percentage beschadigde toepassingen voor specifieke typen. Elke vermelding bevat als de naam van de toepassing type sleutel en waarde een geheel getal dat staat voor het percentage MaxPercentUnhealthyApplications is gebruikt voor het evalueren van de toepassingen van het opgegeven type. Het toepassingsoverzicht type health beleid kan worden gebruikt tijdens de evaluatie van cluster health om te beschrijven van speciale typen. De soorten toepassingen is opgenomen in de kaart worden geëvalueerd op basis van het percentage dat is opgegeven in de kaart en niet met de globale MaxPercentUnhealthyApplications gedefinieerd in het cluster health-beleid. De toepassingen van toepassingstypen die zijn opgegeven in de kaart worden niet geteld op basis van de algemene groep van toepassingen. Bijvoorbeeld, als sommige toepassingen van een type essentieel zijn, de Clusterbeheerder kunt een vermelding toevoegen aan de kaart voor die toepassing en hieraan een waarde van 0% (dat wil zeggen, doen niet tolereren fouten). Alle andere toepassingen kunnen worden geëvalueerd met MaxPercentUnhealthyApplications ingesteld op 20% te tolereren fouten opgetreden buiten de duizenden exemplaren van de toepassing. Het toepassingsoverzicht type health beleid wordt alleen gebruikt als het clustermanifest kunt type toepassing evalueren met behulp van de configuratie-item voor HealthManager/EnableApplicationTypeHealthEvaluation. |
| --chaos-doel-filter | JSON gecodeerd woordenlijst met twee tekenreeks type sleutels. De twee sleutels zijn NodeTypeInclusionList en ApplicationInclusionList. Waarden voor beide sleutels zijn lijst met tekenreeksen. chaos_target_filter definieert alle filters voor gerichte Chaos fouten, bijvoorbeeld alleen bepaalde typen met fout of alleen bepaalde toepassingen met fout. <br><br> Als chaos_target_filter niet gebruikt wordt, bedrijfsstoringen Chaos alle entiteiten van het cluster. Als chaos_target_filter wordt gebruikt, bedrijfsstoringen Chaos alleen de entiteiten die voldoen aan de specificatie chaos_target_filter. NodeTypeInclusionList en ApplicationInclusionList kunt alleen een union semantiek. Het is niet mogelijk om op te geven van een snijpunt van NodeTypeInclusionList en ApplicationInclusionList. Het is bijvoorbeeld niet mogelijk om op te geven "fault deze toepassing alleen wanneer deze zich op dit knooppunttype." Zodra een entiteit is opgenomen in NodeTypeInclusionList of ApplicationInclusionList, kan niet die entiteit worden uitgesloten chaostargetfilter kunnen gebruiken. Zelfs als applicationX niet wordt weergegeven in ApplicationInclusionList, in bepaalde iteratie Chaos applicationX kunnen worden is mislukt omdat dit gebeurt op een knooppunt van het nodeTypeY die is opgenomen in NodeTypeInclusionList. Als zowel NodeTypeInclusionList en ApplicationInclusionList leeg zijn, is er een ArgumentException gegenereerd. Alle typen fouten (knooppunt opnieuw opstarten, opnieuw opstarten codepakket replica verwijderen, replica opnieuw, verplaatsen primaire en secundaire verplaatsen) zijn ingeschakeld voor de knooppunten van de volgende knooppunttypen. Als een knooppunttype (bijvoorbeeld NodeTypeX) niet wordt weergegeven in de NodeTypeInclusionList en vervolgens het knooppunt niveau fouten (zoals NodeRestart) nooit worden ingeschakeld voor de knooppunten van NodeTypeX, maar het fouten voor het pakket en de replica van code kunnen nog steeds worden ingeschakeld voor NodeTypeX als een toepassing in de ApplicationInclusionList gebeurt zich bevinden op een knooppunt van NodeTypeX. Maximaal 100 knooppunt Typenamen kunnen worden opgenomen in deze lijst, dit aantal vergroten, een upgrade van een configuratie is vereist voor MaxNumberOfNodeTypesInChaosEntityFilter configuratie. Alle replica's die behoren tot de services van deze toepassingen zijn aanmerking voor replica-fouten (opnieuw opstarten replica, replica verwijderen, verplaatsen primaire en secundaire verplaatsen) door Chaos. Chaos mogelijk opnieuw opstarten van een codepakket alleen als het codepakket als host fungeert voor replica's van deze toepassingen alleen. Als een toepassing niet in deze lijst wordt weergegeven, kan het nog steeds mislukt in bepaalde iteratie Chaos als beëindigd door de toepassing van op een knooppunt van een knooppunttype dat is opgenomen in NodeTypeInclusionList. Maar als applicationX is gekoppeld aan nodeTypeY door middel van plaatsingsbeperkingen en applicationX afwezig is van ApplicationInclusionList en nodeTypeY is afwezig van NodeTypeInclusionList, klikt u vervolgens applicationX wordt nooit worden is mislukt. Maximaal 1000 toepassingsnamen kunnen worden opgenomen in deze lijst, dit aantal vergroten, een upgrade van een configuratie is vereist voor MaxNumberOfApplicationsInChaosEntityFilter configuratie. |
| --context | JSON gecodeerd toewijzing van het (string, string) typt u sleutel-waardeparen. De kaart kan worden gebruikt om vast te leggen van de informatie over het uitvoeren van Chaos. Er mag niet meer dan 100 dergelijke paren en elke tekenreeks (sleutel of waarde) mag maximaal 4095 tekens lang zijn. Deze kaart is ingesteld door de starter van de Chaos uitgevoerd voor het opslaan van de context over de specifieke uitvoering (optioneel). |
| --disable-verplaatsen-replica-fouten | Schakelt de verplaatsen primaire en secundaire fouten verplaatsen. |
| --max-cluster-stabilization | De maximale hoeveelheid tijd te wachten totdat alle entiteiten stabiel en in orde cluster.  Standaard\: 60. <br><br> Chaos uitgevoerd in iteraties en aan het begin van elke herhaling valideert de status van de cluster-entiteiten. Als een cluster-entiteit niet stabiel en binnen MaxClusterStabilizationTimeoutInSeconds, gezonde Chaos tijdens de validatie genereert een mislukte validatie-gebeurtenis. |
| --max-gelijktijdige-fouten | Het maximum aantal gelijktijdige fouten veroorzaakte per herhaling. Chaos uitgevoerd in iteraties en twee opeenvolgende pogingen worden gescheiden door een validatiefase. Hoe hoger de waarde voor concurrency, de agressiever het injecteren van fouten--meer complexe reeks statussen om fouten veroorzaken. De aanbeveling is om te beginnen met een waarde 2 of 3 en wees voorzichtig met het tijdens het verplaatsen van.  Standaard\: 1. |
| --max-procent-niet in orde-apps | Bij het evalueren van clusterstatus tijdens Chaos, het maximale percentage van de beschadigde toepassingen toegestaan voordat u een foutmelding. <br><br> De maximaal toegestane percentage van de beschadigde toepassingen voordat u een foutmelding. Bijvoorbeeld, om toe te staan 10% van de toepassingen niet in orde, is deze waarde 10. Het percentage geeft het maximum aantal verdragen percentage van toepassingen die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage in acht wordt genomen, maar er ten minste één beschadigde toepassing is, wordt de status wordt geëvalueerd als waarschuwing. Dit wordt berekend door het aantal beschadigde toepassingen delen via het totale aantal exemplaren van een toepassing in het cluster, met uitzondering van toepassingen van toepassingstypen die zijn opgenomen in de ApplicationTypeHealthPolicyMap. De berekening rondt af naar één tolereren op kleine aantallen toepassingen. Percentage van de standaardwaarde is nul. |
| --max-procent--knooppunten met slechte | Bij het evalueren van clusterstatus tijdens Chaos, mag de maximale percentage van knooppunten met slechte voordat u een foutmelding. <br><br> De maximaal toegestane percentage van knooppunten met slechte voordat u een foutmelding. Bijvoorbeeld, om toe te staan 10% van de knooppunten niet in orde, is deze waarde 10. Het percentage geeft het maximum aantal verdragen percentage van knooppunten die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage in acht wordt genomen, maar er ten minste één beschadigd knooppunt is, wordt de status wordt geëvalueerd als waarschuwing. Het percentage wordt berekend door het aantal knooppunten met slechte delen via het totale aantal knooppunten in het cluster. De berekening rondt af naar één tolereren op kleine aantallen knooppunten. Percentage van de standaardwaarde is nul. In grote clusters sommige knooppunten altijd worden omlaag of uit voor herstellingen, zodat dit percentage moet worden geconfigureerd om te tolereren die. |
| --tijd-en-klaar | Totale tijd (in seconden) waarvoor Chaos voordat automatisch gestopt wordt uitgevoerd. De maximaal toegestane waarde is 4.294.967.295 (System.UInt32.MaxValue).  Standaard\: 4294967295. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --Wacht tijd tussen fouten | Wachttijd (in seconden) tussen opeenvolgende fouten binnen een enkel iteratie.  Standaard\: 20. <br><br> Hoe hoger de waarde, des te lager de overlappende tussen fouten en het eenvoudiger de volgorde van de status verandert dat het cluster wordt verstuurd via. De aanbeveling is om te beginnen met een waarde tussen 1 en 5 en oefening waarschuwing tijdens het verplaatsen van. |
| --Wacht tijd tussen pogingen | Time-scheiding (in seconden) tussen twee opeenvolgende herhalingen van Chaos. Hoe hoger de waarde, des te lager tarief voor fout met betrekking tot injectie.  Standaard\: 30. |
| --waarschuwing als fout | Hiermee stelt u het beleid van de status waarschuwing behandelen als fouten. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-chaos-stop"></a>sfctl chaos stoppen
Chaos stopt als deze wordt uitgevoerd in het cluster en de planning Chaos in een status ' gestopt plaatsen '.

Hiermee stopt u Chaos van nieuwe fouten wordt uitgevoerd. Actieve fouten blijft om uit te voeren totdat ze voltooid zijn. De huidige Chaos-planning wordt geplaatst in een status ' gestopt '. Nadat een schema is gestopt, wordt de status ' gestopt ' blijven en niet worden gebruikt voor nieuwe uitvoeringen van Chaos Chaos-schema. Een nieuw Chaos-schema moet worden ingesteld om te kunnen hervatten plannen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
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
- [Setup](service-fabric-cli.md) de Service Fabric-CLI.
- Meer informatie over het gebruik van de Service Fabric-CLI met behulp van de [voorbeelden van scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).