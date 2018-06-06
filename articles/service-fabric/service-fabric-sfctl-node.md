---
title: Azure Service Fabric CLI - sfctl knooppunt | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl knooppunt-opdrachten.
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
ms.openlocfilehash: fb8a310a131938e95f3d21b3962dbbd1944a57ed
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763422"
---
# <a name="sfctl-node"></a>sfctl node
Beheren van de knooppunten die fungeren als een cluster wordt gevormd.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| uitschakelen | Een Service Fabric-clusterknooppunt met de opgegeven deactivering wordt intentie deactiveren. |
| inschakelen | Activeren van een Service Fabric-cluster-knooppunt dat is gedeactiveerd. |
| status | Hiermee haalt u de status van een Service Fabric-knooppunt. |
| informatie | Hiermee haalt de informatie over een specifiek knooppunt in het Service Fabric-cluster. |
| lijst | Hiermee haalt de lijst met knooppunten in het Service Fabric-cluster. |
| laden | Hiermee haalt u de informatie laden van een Service Fabric-knooppunt. |
| status verwijderen | Service Fabric waarschuwt dat de permanente status op een knooppunt is permanent verwijderd of verloren. |
| rapport-status | Verzendt een statusrapport van het Service Fabric-knooppunt. |
| opnieuw opstarten | Start opnieuw op het knooppunt van een Service Fabric-cluster. |
| overgang | Start of stopt een clusterknooppunt. |
| overgang-status | Hiermee haalt u de voortgang van een bewerking met StartNodeTransition gestart. |

## <a name="sfctl-node-disable"></a>sfctl knooppunt uitschakelen
Een Service Fabric-clusterknooppunt met de opgegeven deactivering wordt intentie deactiveren.

Een Service Fabric-clusterknooppunt met de opgegeven deactivering wordt intentie deactiveren. Zodra de deactivering uitgevoerd wordt, de intent voor deactivering van kan worden verhoogd, maar niet wordt verlaagd (bijvoorbeeld een knooppunt dat is geïntegreerd met de bedoeling onderbreken worden gedeactiveerd verder met opnieuw opstarten, maar niet andersom. Knooppunten kunnen opnieuw worden geactiveerd met behulp van het activeren van een knooppunt bewerking elk gewenst moment nadat ze zijn gedeactiveerd. Als de deactivering niet voltooid is, wordt hiermee de deactivering geannuleerd. Een knooppunt dat uitvalt en opnieuw wordt weergegeven, terwijl gedeactiveerd nog steeds worden geactiveerd voordat services op dat knooppunt worden geplaatst.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --intent voor deactivering | Het doel of de reden voor het deactiveren van het knooppunt beschreven. De mogelijke waarden zijn volgt. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-node-enable"></a>sfctl knooppunt inschakelen
Activeren van een Service Fabric-cluster-knooppunt dat is gedeactiveerd.

Hiermee activeert u een Service Fabric-cluster-knooppunt dat is gedeactiveerd. Zodra geactiveerd, het knooppunt wordt opnieuw een praktische doel voor de plaatsing van de nieuwe replica's, en een gedeactiveerde replica's op het knooppunt resterende zal opnieuw worden geactiveerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-node-health"></a>sfctl knooppunt health
Hiermee haalt u de status van een Service Fabric-knooppunt.

Hiermee haalt u de status van een Service Fabric-knooppunt. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen op het knooppunt op basis van de status is gerapporteerd. Als het knooppunt dat u met de naam opgeeft niet in de health store bestaat, moet dit een fout geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-node-info"></a>sfctl knooppunt info
Hiermee haalt de informatie over een specifiek knooppunt in het Service Fabric-cluster.

Hiermee haalt de informatie over een specifiek knooppunt in het Service Fabric-Cluster. Het antwoord bevat de naam, status-ID, status, bedrijfstijd en andere details over het knooppunt.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-node-list"></a>lijst met sfctl knooppunten
Hiermee haalt de lijst met knooppunten in het Service Fabric-cluster.

Hiermee haalt de lijst met knooppunten in het Service Fabric-cluster. Het antwoord bevat de naam, status-ID, status, bedrijfstijd en andere details over het knooppunt.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --vervolgtoken | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd. |
| --knooppunt Statusfilter | Hiermee kunt u op de knooppunten op basis van de NodeStatus filteren. Alleen de knooppunten die zijn die overeenkomt met de opgegeven filterwaarde geretourneerd. De filterwaarde zijn.  Standaard\: standaard. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-node-load"></a>sfctl knooppunt laden
Hiermee haalt u de informatie laden van een Service Fabric-knooppunt.

Haalt de informatie laden van een Service Fabric-knooppunt van de metrische gegevens die load of capaciteit gedefinieerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-node-remove-state"></a>sfctl knooppunt-status verwijderen
Service Fabric waarschuwt dat de permanente status op een knooppunt is permanent verwijderd of verloren.

Service Fabric waarschuwt dat de permanente status op een knooppunt is permanent verwijderd of verloren.  Dit betekent dat het is niet mogelijk voor het herstellen van de permanente status van dat knooppunt. Dit gebeurt doorgaans als een harde schijf is gewist schone, of als een harde schijf is vastgelopen. Het knooppunt heeft als niet beschikbaar voor deze bewerking kan alleen slagen. Deze bewerking kunt Service Fabric weten dat de replica's op dat knooppunt niet meer bestaat en dat het Service Fabric in afwachting van die replica weer actief moet worden gestopt. Voer deze cmdlet niet als de status op het knooppunt is niet verwijderd en het knooppunt om met de status behouden terugkeren kunt.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-node-report-health"></a>sfctl knooppunt rapport-status
Verzendt een statusrapport van het Service Fabric-knooppunt.

Meldt de status van de opgegeven Service Fabric-knooppunt. Het rapport moet bevatten informatie over de bron van de health-rapport en de eigenschap waarop deze wordt gerapporteerd. Het rapport wordt verzonden naar een Service Fabric gateway-knooppunt, die worden doorgestuurd naar de health store. Het rapport kan worden geaccepteerd door de gateway, maar geweigerd door de health store na extra validatie. De health store kan bijvoorbeeld het rapport weigeren vanwege een ongeldige parameter, zoals een verouderde volgnummer. Om te zien of het rapport in de health store is toegepast, Controleer of het rapport wordt weergegeven in de sectie HealthEvents.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --health-eigenschap [vereist] | De eigenschap van de health-informatie. <br><br> Een entiteit kan statusrapporten voor de verschillende eigenschappen hebben. De eigenschap is een tekenreeks en niet een vaste opsomming de Rapportagefout flexibiliteit om de status-voorwaarde die het rapport activeert te categoriseren toe te passen. Een Rapportagefout met SourceId 'LocalWatchdog' kunt bijvoorbeeld de status van de beschikbare schijfruimte op een knooppunt, bewaken, zodat deze 'AvailableDisk'-eigenschap op dat knooppunt kunt rapporteren. De dezelfde Rapportagefout kan de connectiviteit knooppunt bewaken, zodat dit een 'Verbinding'-eigenschap op hetzelfde knooppunt kunt rapporteren. Deze rapporten worden in het archief health behandeld als afzonderlijke health-gebeurtenissen voor het opgegeven knooppunt. Samen met de bron-id identificatie de eigenschap een unieke voor de health-informatie. |
| --status [vereist] | Mogelijke waarden zijn\: 'Ongeldig', 'Ok', 'Waarschuwing', 'Fout', 'Onbekend'. |
| --knooppuntnaam [vereist] | De knooppuntnaam voor het rapporteren van. |
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

## <a name="sfctl-node-restart"></a>sfctl knooppunt opnieuw opstarten
Start opnieuw op het knooppunt van een Service Fabric-cluster.

Start opnieuw op het knooppunt van een Service Fabric-cluster die al is gestart.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --maken fabric dump | Geef waar voor het maken van een dump van het proces fabric-knooppunt. Dit is hoofdlettergevoelig.  Standaard\: False. |
| --knooppunt-exemplaar-id | De exemplaar-ID van het doelknooppunt. Als exemplaar-ID van het knooppunt opnieuw wordt gestart alleen als deze met het huidige exemplaar van het knooppunt overeenkomt is opgegeven. Een standaardwaarde van '0' overeenkomen met een exemplaar-ID. De exemplaar-ID kan worden verkregen met behulp van de query voor get-knooppunt.  Standaard\: 0. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-node-transition"></a>sfctl knooppunt overgang
Start of stopt een clusterknooppunt.

Start of stopt een clusterknooppunt.  Een clusterknooppunt is een proces, niet de OS-exemplaar zelf.  In 'Start' voor de parameter NodeTransitionType doorgeven voor het starten van een knooppunt. Om te stoppen op een knooppunt, doorgeven in 'Stop' voor de parameter NodeTransitionType. Deze API wordt de bewerking - gestart wanneer de API-retourneert het knooppunt kan niet klaar bent met nog een overgang. Aanroepen GetNodeTransitionProgress met de dezelfde OperationId ophalen van de voortgang van de bewerking.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppunt-exemplaar-id [vereist] | De exemplaar-ID voor het knooppunt van het doelknooppunt. Dit kan worden bepaald via GetNodeInfo API. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| ---overgang-knooppunttype [vereist] | Geeft het type van overgang om uit te voeren.  NodeTransitionType.Start, een gestopte knooppunt wordt gestart. NodeTransitionType.Stop wordt een knooppunt dat is gestopt. |
| --bewerking-id [vereist] | Een GUID die een aanroep van deze API identificeert.  Dit wordt doorgegeven in de bijbehorende GetProgress-API. |
| --stop-duur-in-seconden [vereist] | De duur in seconden, dat het knooppunt is gestopt.  De minimumwaarde is 600, het maximum is 14400.  Nadat deze tijd is verstreken, het knooppunt wordt automatisch geactiveerd zijn. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-node-transition-status"></a>sfctl knooppunt overgang-status
Hiermee haalt u de voortgang van een bewerking met StartNodeTransition gestart.

Hiermee haalt u de voortgang van een bewerking met StartNodeTransition met behulp van de opgegeven OperationId is gestart.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --bewerking-id [vereist] | Een GUID die een aanroep van deze API identificeert.  Dit wordt doorgegeven in de bijbehorende GetProgress-API. |
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