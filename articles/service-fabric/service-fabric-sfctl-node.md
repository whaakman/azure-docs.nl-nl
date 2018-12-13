---
title: Azure Service Fabric CLI - sfctl node | Microsoft Docs
description: Beschrijving van de opdrachten van Service Fabric-CLI sfctl knooppunt.
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
ms.openlocfilehash: 5ceda83863d892b84ee4dc272345f760116e5e69
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53278602"
---
# <a name="sfctl-node"></a>sfctl node
Beheren van de knooppunten die een cluster wordt gevormd.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| uitschakelen | Deactiveer het knooppunt van een Service Fabric-cluster met de bedoeling opgegeven deactiveren. |
| inschakelen | Activeren van een Service Fabric-cluster-knooppunt dat is gedeactiveerd. |
| gezondheidszorg | Hiermee haalt u de status van een Service Fabric-knooppunt. |
| informatie | Hiermee haalt de informatie over een bepaald knooppunt in het Service Fabric-cluster. |
| list | Hiermee haalt de lijst met knooppunten in het Service Fabric-cluster. |
| laden | Hiermee haalt u de gegevens laden van een Service Fabric-knooppunt. |
| Remove-status | Service Fabric de hoogte gebracht dat de permanente status op een knooppunt is definitief verwijderd of verloren gaan. |
| rapport-en statusbewaking | Verzendt een statusrapport over de Service Fabric-knooppunt. |
| restart | Start opnieuw op het knooppunt van een Service Fabric-cluster. |
| overgang | Wordt gestart of gestopt een clusterknooppunt. |
| overgang-status | Hiermee haalt u de voortgang van een bewerking aan de slag met StartNodeTransition. |

## <a name="sfctl-node-disable"></a>sfctl knooppunt uitschakelen
Deactiveer het knooppunt van een Service Fabric-cluster met de bedoeling opgegeven deactiveren.

Deactiveer het knooppunt van een Service Fabric-cluster met de bedoeling opgegeven deactiveren. Zodra de deactivering uitgevoerd wordt, kunt u lezen wat de deactivering kan worden verhoogd, maar niet wordt verlaagd tot (bijvoorbeeld een knooppunt dat is geïntegreerd met de bedoeling onderbreken worden gedeactiveerd verder met opnieuw opstarten, maar niet andersom. Knooppunten kunnen worden geactiveerd met behulp van het activeren van de knooppuntbewerking van een elk gewenst moment nadat ze zijn uitgeschakeld. Als de deactivering niet voltooid is, wordt hiermee de deactivering geannuleerd. Een knooppunt dat uitvalt en opnieuw wordt weergegeven als gedeactiveerd moet nog steeds opnieuw worden geactiveerd voordat de service wordt geplaatst op dat knooppunt.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --deactivering-doel | Beschrijving van het doel of de reden voor het deactiveren van het knooppunt. De mogelijke waarden zijn te volgen. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-node-enable"></a>sfctl knooppunt inschakelen
Activeren van een Service Fabric-cluster-knooppunt dat is gedeactiveerd.

Hiermee activeert u een Service Fabric-cluster-knooppunt dat is gedeactiveerd. Zodra geactiveerd, het knooppunt opnieuw een mogelijk doel wordt voor de nieuwe replica's, en een gedeactiveerde replica's op het knooppunt zal opnieuw worden geactiveerd.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-node-health"></a>sfctl knooppuntstatus
Hiermee haalt u de status van een Service Fabric-knooppunt.

Hiermee haalt u de status van een Service Fabric-knooppunt. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van gemeld op het knooppunt op basis van de status van de health-gebeurtenissen. Als het knooppunt dat u met de naam opgeeft niet in de health-store bestaat, wordt een fout geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling van HealthEvent-objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden, verkregen met behulp van de bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde 6 is worden vervolgens alle gebeurtenissen met HealthState waarde OK (2) en waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-node-info"></a>sfctl knooppunt info
Hiermee haalt de informatie over een bepaald knooppunt in het Service Fabric-cluster.

Het antwoord bevat de naam, status, -ID, status, actieve tijdsduur en andere details van het knooppunt.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-node-list"></a>lijst met sfctl knooppunt
Hiermee haalt de lijst met knooppunten in het Service Fabric-cluster.

Het antwoord bevat de naam, status, -ID, status, actieve tijdsduur en andere details weer over de knooppunten.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --vervolgtoken | De voortzetting van token-parameter wordt gebruikt om op te halen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in één antwoord passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep retourneert de API volgende set met resultaten. Als er geen verdere resultaten, klikt u vervolgens bevat het vervolgtoken een waarde. De waarde van deze parameter mag geen URL-codering. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal geretourneerde resultaten. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de beperkingen van de grootte van maximaal bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de query met resultatenpagina's zo veel resultaten als is mogelijk dat de geretourneerde bericht. |
| --knooppunt in het filter status | Hiermee kunt u op de knooppunten op basis van de NodeStatus filteren. Alleen de knooppunten die zijn die overeenkomen met de opgegeven filterwaarde geretourneerd. De filterwaarde kan zijn dat een van de volgende.  Standaard\: standaard. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-node-load"></a>sfctl knooppunt laden
Hiermee haalt u de gegevens laden van een Service Fabric-knooppunt.

Haalt informatie op het laden van een Service Fabric-knooppunt voor de metrische gegevens waarvoor gegevens worden geladen of capaciteit gedefinieerd.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-node-remove-state"></a>sfctl knooppunt remove-status
Service Fabric de hoogte gebracht dat de permanente status op een knooppunt is definitief verwijderd of verloren gaan.

Dit betekent dat het is niet mogelijk om te herstellen van de permanente status van dat knooppunt. Dit gebeurt meestal als een harde schijf is gewist schone, of als een harde schijf vastloopt. Het knooppunt is niet beschikbaar voor deze bewerking om succesvol te zijn. Met deze bewerking kunt Service Fabric weten dat de replica's op dat knooppunt niet meer bestaan en die Service Fabric wacht op de replica's keert u terug van moet worden stopgezet. Voer deze cmdlet niet als de status op het knooppunt is niet verwijderd en het knooppunt van met de status behouden overstappen kunt.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-node-report-health"></a>sfctl knooppunt rapport-en statusbewaking
Verzendt een statusrapport over de Service Fabric-knooppunt.

Rapporteert de status van de opgegeven Service Fabric-knooppunt. Het rapport moet de informatie over de oorzaak van de health-rapport en de eigenschap waarop dit apparaat is gerapporteerd bevatten. Het rapport wordt verzonden naar een Service Fabric gateway-knooppunt, die naar de health-store doorstuurt. Het rapport kan worden geaccepteerd door de gateway, maar geweigerd door de health store na extra validatie. Het rapport kan bijvoorbeeld afwijzen, de health store vanwege een ongeldige parameter, zoals een verouderde volgnummer. Om te zien of het rapport in de health-store is toegepast, Controleer of het rapport wordt weergegeven in de sectie HealthEvents.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --health-eigenschap (vereist) | De eigenschap van de gegevens over de servicestatus. <br><br> Een entiteit kan statusrapporten voor de verschillende eigenschappen hebben. De eigenschap is een tekenreeks en niet een vaste-opsomming waarmee de journalist flexibiliteit voor het categoriseren van de voorwaarde staat dat het rapport wordt geactiveerd. Bijvoorbeeld, een Rapportagefout met SourceId "LocalWatchdog" die de status van de beschikbare schijfruimte op een knooppunt kunt controleren, zodat deze eigenschap "AvailableDisk" op dat knooppunt rapporteren kunt. De dezelfde journalist kan het knooppunt-connectiviteit, bewaken, zodat het een eigenschap 'Connectiviteit' op hetzelfde knooppunt rapporteren kan. In de winkel de gezondheid van worden deze rapporten behandeld als afzonderlijke health-gebeurtenissen voor het opgegeven knooppunt. Samen met de bron-id identificatie de eigenschap unieke van de gegevens over de servicestatus. |
| --status (vereist) | Mogelijke waarden zijn\: 'Ongeldige', 'Ok', 'Waarschuwing', 'Fout', 'Onbekend'. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --bron-id (vereist) | De naam van de bron die de watchdog-client-systeem component aangeduid die de gezondheidsinformatie gegenereerd. |
| --Beschrijving | De beschrijving van de gegevens over de servicestatus. <br><br> Hiermee geeft u vrije tekst die wordt gebruikt om toe te voegen menselijke leesbare informatie over het rapport. De maximumlengte voor de beschrijving is 4096 tekens. Als de opgegeven tekenreeks langer is, worden deze automatisch afgekapt. Wanneer afgekapt, de laatste tekens van de beschrijving van de markering van een '[Truncated]' bevatten en totale grootte is 4096 tekens. De aanwezigheid van de markering geeft aan dat gebruikers die moet worden afgekapt is opgetreden. Houd er rekening mee dat wanneer afgekapt, de beschrijving van minder dan 4096 tekens uit de oorspronkelijke reeks heeft. |
| --direct | Een vlag die aangeeft of het rapport direct moet worden verzonden. <br><br> Een statusrapport wordt verzonden naar een gateway Service Fabric-toepassing, die wordt doorgestuurd naar de health-store. Als direct is ingesteld op true, het rapport wordt direct verzonden van HTTP-Gateway naar de health-store, ongeacht de instellingen van de fabric-client die de HTTP-Gateway-toepassing wordt gebruikt. Dit is handig voor de kritieke rapporten die zo snel mogelijk moeten worden verzonden. Afhankelijk van de timing en andere voorwaarden mislukken verzenden van het rapport nog steeds, bijvoorbeeld als de HTTP-Gateway is gesloten of het bericht de Gateway niet bereiken. Als direct is ingesteld op false, wordt het rapport verzonden op basis van de clientinstellingen voor de status van de HTTP-Gateway. Het wordt daarom worden batchgewijs op basis van de configuratie van de HealthReportSendInterval. Dit is de aanbevolen instelling omdat hierdoor de health-client de gezondheid van berichten in health store, evenals verwerking van statusrapporten reporting optimaliseren. Standaard rapporten niet onmiddellijk verzonden. |
| --verwijderen wanneer verlopen | De waarde die aangeeft of het rapport is verwijderd uit health store wanneer het verloopt. <br><br> Als is ingesteld op true, wordt het rapport wordt verwijderd uit de store health nadat deze is verlopen. Indien ingesteld op false, het rapport wordt behandeld als een fout bij het verlopen. De waarde van deze eigenschap is ingesteld op false standaard. Als clients regelmatig rapporteren, moeten ze RemoveWhenExpired false (standaard) ingesteld. Op deze manier is de journalist heeft problemen (bijvoorbeeld impasse) en kan niet rapporteren, wordt de entiteit bij fout wordt geëvalueerd wanneer het statusrapport is verlopen. Deze vlaggen die de entiteit alsof ze de status fout. |
| --volgnummer: | Het volgnummer voor dit statusrapport als een numerieke tekenreeks. <br><br> Het volgnummer van het rapport wordt gebruikt door de health store voor het detecteren van verouderde rapporten. Indien niet opgegeven, is een volgnummer automatisch gegenereerd door de client health wanneer een rapport wordt toegevoegd. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --ttl | De tijdsduur waarvoor deze statusrapport geldig is. Dit veld wordt ISO8601-notatie gebruikt voor het opgeven van de duur. <br><br> Als clients regelmatig rapporteren, moeten ze rapporten verzenden met een hogere frequentie dan time to live van. Als clients een over de overgang rapport, kunnen ze time to live op oneindig ingesteld. Wanneer time to live van is verlopen, de statusgebeurtenis met de health-gegevens is verwijderd uit health store, als RemoveWhenExpired true, of op een fout, geëvalueerd als onwaar RemoveWhenExpired. Indien niet opgegeven, time to live van oneindige waarde de standaardwaarde van. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-node-restart"></a>sfctl knooppunt opnieuw opstarten
Start opnieuw op het knooppunt van een Service Fabric-cluster.

Start opnieuw op een Service Fabric-clusterknooppunt die al is gestart.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --maken-fabric-dump | Geef op waar voor het maken van een dump van het proces van fabric-knooppunt. Dit is hoofdlettergevoelig.  Standaard\: False. |
| --knooppunt-exemplaar-id | De exemplaar-ID van het doelknooppunt. Als exemplaar-ID van het knooppunt opnieuw wordt gestart alleen als deze met het huidige exemplaar van het knooppunt overeenkomt is opgegeven. Een standaardwaarde van '0' wordt er gezocht naar een exemplaar-ID. De exemplaar-ID kan worden verkregen met behulp van get-knooppunt query.  Standaard\: 0. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-node-transition"></a>sfctl voor knooppuntovergang gebruiken
Wordt gestart of gestopt een clusterknooppunt.

Wordt gestart of gestopt een clusterknooppunt.  Een clusterknooppunt is een proces, niet de instantie van een besturingssysteem zelf.  In 'Start' voor de parameter NodeTransitionType doorgeven voor het starten van een knooppunt. Als u wilt stoppen van een knooppunt, doorgeven in 'Stop' voor de parameter NodeTransitionType. Deze API wordt de bewerking - gestart wanneer de API retourneert dat het knooppunt kan niet klaar bent met het nog overstappen. GetNodeTransitionProgress met dezelfde bewerkings-id ophalen van de voortgang van de bewerking aanroepen.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppunt-exemplaar-id (vereist) | De exemplaar-ID voor het knooppunt van het doelknooppunt. Dit kan worden bepaald via GetNodeInfo API. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --knooppunt-overgang-type (vereist) | Geeft het type overgang uit te voeren.  NodeTransitionType.Start, een gestopte knooppunt wordt gestart. NodeTransitionType.Stop stopt een knooppunt of actief is. |
| --bewerking-id (vereist) | Een GUID die een aanroep van deze API identificeert.  Deze informatie wordt doorgegeven aan de bijbehorende GetProgress-API. |
| --stop-duur-in-seconden (vereist) | De duur, in seconden, zodat het knooppunt is gestopt.  De minimumwaarde is 600, het maximum aantal 14400 is.  Nadat deze tijd is verlopen, wordt het knooppunt automatisch geactiveerd terug. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-node-transition-status"></a>sfctl knooppunt overgang-status
Hiermee haalt u de voortgang van een bewerking aan de slag met StartNodeTransition.

Hiermee haalt u de voortgang van een bewerking aan de slag met StartNodeTransition met behulp van de opgegeven bewerkings-id.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --bewerking-id (vereist) | Een GUID die een aanroep van deze API identificeert.  Deze informatie wordt doorgegeven aan de bijbehorende GetProgress-API. |
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
- [Setup](service-fabric-cli.md) de Service Fabric-CLI.
- Meer informatie over het gebruik van de Service Fabric-CLI met behulp van de [voorbeelden van scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).