---
title: Azure Service Fabric CLI - sfctl cluster | Microsoft Docs
description: Beschrijving van de Service Fabric CLI-opdrachten voor het cluster van sfctl.
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
ms.openlocfilehash: 60f3f74778f0fb32677c3b87b3140131ccd37bea
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763626"
---
# <a name="sfctl-cluster"></a>sfctl cluster
Selecteer, beheren en gebruiken van Service Fabric-clusters.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| code-versies | Haalt een lijst van fabric code-versies die zijn ingericht op een Service Fabric-cluster. |
| configuratie-versies | Hiermee haalt een lijst van fabric config-versies die zijn ingericht op een Service Fabric-cluster. |
| status | Hiermee haalt u de status van een Service Fabric-cluster. |
| Manifest | Haal het manifest Service Fabric-cluster. |
| bewerking annuleren | Een gebruiker veroorzaakte fout geannuleerd. |
| lijst van bewerkingen | Hiermee haalt u een lijst van fouttolerantie gebruiker veroorzaakte bewerkingen die zijn gefilterd op de opgegeven invoer. |
| Inrichten | De pakketten code of configuratie van een Service Fabric-cluster inrichten. |
| herstellen-systeem | Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van de systeemservices die momenteel zijn vastgelopen in quorumverlies. |
| rapport-status | Verzendt een statusrapport op het Service Fabric-cluster. |
| selecteren | Maakt verbinding met een Service Fabric-cluster-eindpunt. |
| Inrichting verwijderen | Inrichting van de pakketten code of configuratie van een Service Fabric-cluster. |
| upgrade | Start de code of upgrade van een Service Fabric-cluster. |
| upgrade hervatten | Controleer het upgraden van cluster gaat u verder met het volgende upgradedomein. |
| upgrade terugdraaien | De upgrade van een Service Fabric-cluster terugdraaien. |
| upgrade-status | Hiermee haalt u de voortgang van de huidige upgrade van de cluster. |
| upgrade-update | Bijwerken van de parameters voor het bijwerken van de upgrade van een Service Fabric-cluster. |

## <a name="sfctl-cluster-code-versions"></a>sfctl cluster-versies
Haalt een lijst van fabric code-versies die zijn ingericht op een Service Fabric-cluster.

Haalt een lijst met informatie over fabric code-versies die zijn ingericht in het cluster. De parameter CodeVersion kan worden gebruikt voor het filteren van de uitvoer voor alleen die bepaalde versie (optioneel).

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --code-versie | De productversie van Service Fabric. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-cluster-config-versions"></a>sfctl cluster config-versies
Hiermee haalt een lijst van fabric config-versies die zijn ingericht op een Service Fabric-cluster.

Haalt een lijst met informatie over de fabric config-versies die zijn ingericht in het cluster. De parameter ConfigVersion kan worden gebruikt voor het filteren van de uitvoer voor alleen die bepaalde versie (optioneel).

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --config-versie | De config-versie van Service Fabric. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-cluster-health"></a>sfctl cluster health
Hiermee haalt u de status van een Service Fabric-cluster.

Hiermee haalt u de status van een Service Fabric-cluster. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen op het cluster op basis van de status is gerapporteerd. NodesHealthStateFilter en ApplicationsHealthStateFilter op dezelfde manier gebruiken voor het filteren van de verzameling van knooppunten en toepassingen die worden geretourneerd op basis van hun geaggregeerde status.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassingen-health-status-filter | Hiermee kunt u filteren van de toepassing health status objecten geretourneerd in het resultaat van het cluster health query op basis van hun status. De mogelijke waarden voor deze parameter zijn geheel getal is verkregen van leden of een bitsgewijze bewerkingen op leden van HealthStateFilter opsomming. Alleen toepassingen die overeenkomen met het filter worden geretourneerd. Alle toepassingen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 wordt de status van toepassingen met HealthState waarde OK (2) en de waarschuwing (4) zijn geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --uitsluiten-health-statistieken | Hiermee wordt aangegeven of de statistieken van de status moet worden geretourneerd als onderdeel van het queryresultaat. Standaard False. De statistieken tonen het aantal onderliggende items entiteiten in de status Ok, waarschuwing en fout. |
| --opnemen-systeem-toepassing-health-statistieken | Hiermee wordt aangegeven of de gezondheidsstatistieken van de fabric moet bevatten \: /System application health statistieken. Standaard False. Als IncludeSystemApplicationHealthStatistics is ingesteld op true, wordt de status statistieken omvatten de entiteiten die deel uitmaken van de fabric \: /System application. Anders wordt het queryresultaat health statistische gegevens bevat, alleen voor toepassingen. De health-statistieken moeten worden opgenomen in de resultaten voor deze parameter moet worden toegepast. |
| --knooppunten-health-status-filter | Hiermee kunt u filteren van het knooppunt health status objecten geretourneerd in het resultaat van het cluster health query op basis van hun status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen knooppunten die overeenkomen met het filter worden geretourneerd. Alle knooppunten worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 wordt de status van knooppunten met HealthState waarde OK (2) en de waarschuwing (4) zijn geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-cluster-manifest"></a>sfctl clustermanifest
Haal het manifest Service Fabric-cluster.

Haal het manifest Service Fabric-cluster. Het clustermanifest bevat eigenschappen van het cluster met verschillende knooppunttypen op het cluster, beveiligingsconfiguraties, fout, en upgradedomein topologieën, enzovoort. Deze eigenschappen zijn opgegeven als deel van het bestand ClusterConfig.JSON tijdens de implementatie van een zelfstandige cluster. Echter, de meeste van de informatie in het clustermanifest wordt gegenereerd intern door service fabric tijdens de implementatie van het cluster in andere implementatiescenario's (bijvoorbeeld wanneer u Azure portal). De inhoud van het clustermanifest zijn alleen ter informatie en gebruikers wordt niet verwacht dat de indeling van de inhoud van het bestand of de interpretatie van een afhankelijkheid ondernemen.

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

## <a name="sfctl-cluster-operation-cancel"></a>sfctl cluster bewerking annuleren
Een gebruiker veroorzaakte fout geannuleerd.

De volgende API's starten fault-bewerkingen die kunnen worden geannuleerd via CancelOperation: StartDataLoss, StartQuorumLoss, StartPartitionRestart, StartNodeTransition. Als force ONWAAR is, klikt u vervolgens de opgegeven gebruiker veroorzaakte bewerking probleemloos gestopt en opgeschoond.  Als force ingesteld op true is, de opdracht wordt afgebroken en een aantal interne status worden achtergelaten.  Force opgeven als waar moet zorgvuldig worden gebruikt. Het aanroepen van deze API met geforceerd ingesteld op true is niet toegestaan, totdat deze API is al aangeroepen voor dezelfde testopdracht force is ingesteld op false eerste of tenzij de testopdracht heeft al een OperationState OperationState.RollingBack. 

Uitleg\: OperationState.RollingBack betekent dat het systeem worden/ruimt interne system state veroorzaakt door de opdracht wordt uitgevoerd. Deze herstelt gegevens niet als de testopdracht was om te leiden tot gegevensverlies.  Bijvoorbeeld, als u StartDataLoss Roep vervolgens deze API niet aanroepen, wordt het systeem alleen opschonen interne status van de opdracht uit te voeren. Deze herstelt niet de doelpartitie gegevens als de opdracht ver genoeg gevorderd om te leiden tot gegevensverlies. 

> [!NOTE]
> Als deze API wordt aangeroepen met force == true, interne status worden achtergelaten.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --bewerking-id [vereist] | Een GUID die een aanroep van deze API identificeert.  Dit wordt doorgegeven in de bijbehorende GetProgress-API. |
| --forceren | Geeft aan of probleemloos terugdraaien en interne systeemstatus gewijzigd door het uitvoeren van de gebruiker veroorzaakte bewerking opschoont. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-cluster-operation-list"></a>sfctl cluster bewerking-lijst
Hiermee haalt u een lijst van fouttolerantie gebruiker veroorzaakte bewerkingen die zijn gefilterd op de opgegeven invoer.

Hiermee wordt de lijst met fout gebruiker veroorzaakte bewerkingen die zijn gefilterd door de opgegeven invoer opgehaald.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --status-filter | Gebruikt om te filteren op de OperationState voor gebruiker veroorzaakte bewerkingen. <br> 65535 - Alles selecteren <br> 1 - selecteren die wordt uitgevoerd <br> 2 - selecteren ongedaan maken <br>8 - voltooid selecteren <br>16 - Faulted selecteren <br>32 - geannuleerd selecteren <br>64 - ForceCancelled selecteren.  <br>Standaard\: 65535. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |
| --type filter | Gebruikt om te filteren op OperationType voor gebruiker veroorzaakte bewerkingen. <br> 65535 - Alles selecteren <br> 1 - PartitionDataLoss selecteren. <br> 2 - PartitionQuorumLoss selecteren. <br> 4 - PartitionRestart selecteren. <br> 8 - NodeTransition selecteren.  <br> Standaard\: 65535. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-cluster-provision"></a>sfctl cluster inrichten
De pakketten code of configuratie van een Service Fabric-cluster inrichten.

Valideren en de pakketten code of configuratie van een Service Fabric-cluster inrichten.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --cluster-manifest-bestandspad | Het pad van het manifestbestand voor het cluster. |
| --code bestandspad | Het cluster code pakket bestandspad. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-cluster-recover-system"></a>sfctl cluster herstellen-systeem
Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van de systeemservices die momenteel zijn vastgelopen in quorumverlies.

Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van de systeemservices die momenteel zijn vastgelopen in quorumverlies. Deze bewerking mag alleen worden uitgevoerd als bekend is dat de replica's die niet beschikbaar zijn, kunnen niet worden hersteld. Onjuist gebruik van deze API kan leiden tot verlies van gegevens.

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

## <a name="sfctl-cluster-report-health"></a>sfctl cluster rapport-status
Verzendt een statusrapport op het Service Fabric-cluster.

Het rapport moet bevatten informatie over de bron van de health-rapport en de eigenschap waarop deze wordt gerapporteerd. Het rapport wordt verzonden naar een Service Fabric gateway-knooppunt, die worden doorgestuurd naar de health store. Het rapport kan worden geaccepteerd door de gateway, maar geweigerd door de health store na extra validatie. De health store kan bijvoorbeeld het rapport weigeren vanwege een ongeldige parameter, zoals een verouderde volgnummer. Om te zien of het rapport in de health store is toegepast, Controleer of het rapport wordt weergegeven in de HealthEvents van het cluster.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --health-eigenschap [vereist] | De eigenschap van de health-informatie. <br><br> Een entiteit kan statusrapporten voor de verschillende eigenschappen hebben. De eigenschap is een tekenreeks en niet een vaste opsomming de Rapportagefout flexibiliteit om de status-voorwaarde die het rapport activeert te categoriseren toe te passen. Een Rapportagefout met SourceId 'LocalWatchdog' kunt bijvoorbeeld de status van de beschikbare schijfruimte op een knooppunt, bewaken, zodat deze 'AvailableDisk'-eigenschap op dat knooppunt kunt rapporteren. De dezelfde Rapportagefout kan de connectiviteit knooppunt bewaken, zodat dit een 'Verbinding'-eigenschap op hetzelfde knooppunt kunt rapporteren. Deze rapporten worden in het archief health behandeld als afzonderlijke health-gebeurtenissen voor het opgegeven knooppunt. Samen met de bron-id identificatie de eigenschap een unieke voor de health-informatie. |
| --status [vereist] | Mogelijke waarden zijn\: 'Ongeldig', 'Ok', 'Waarschuwing', 'Fout', 'Onbekend'. |
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

## <a name="sfctl-cluster-select"></a>sfctl cluster selecteren
Maakt verbinding met een Service Fabric-cluster-eindpunt.

Als u verbinding maakt met de beveiligde cluster, moet u een absoluut pad naar een certificaat (.crt) en sleutelbestand (.key) of één bestand opgeven met beide (.pem). Beide niet opgeven. Als u verbinding maakt met een beveiligde cluster, geef desgewenst ook een absoluut pad naar een CA bundle-bestand of map van vertrouwde CA-certificaten.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --eindpunt [vereist] | Eindpunt-URL, met inbegrip van de poort en het voorvoegsel HTTP of HTTPS-cluster. |
| --aad | Azure Active Directory voor verificatie gebruiken. |
| --ca | Het absolute pad naar de directory van de CA-certificaten klant op dezelfde manier behandelen als geldig of CA bundle-bestand. |
| --cert | Het absolute pad naar een certificaatbestand van de client. |
| --sleutel | Het absolute pad naar de certificaat-sleutelbestand. |
| --Er is geen controleren | Controle voor certificaten uitschakelen bij gebruik van HTTPS, houd er rekening mee\: dit is een onbeveiligde optie en mag niet worden gebruikt voor productieomgevingen. |
| --pem | Het absolute pad naar het clientcertificaat .pem-bestand. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-cluster-unprovision"></a>sfctl cluster verwijderen
Inrichting van de pakketten code of configuratie van een Service Fabric-cluster.

Inrichting van de pakketten code of configuratie van een Service Fabric-cluster. Deze wordt code en configuratie afzonderlijk inrichting ondersteund.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --code-versie | De code van pakketversie van de cluster. |
| --config-versie | De versie van het clustermanifest. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-cluster-upgrade"></a>clusterupgrades sfctl
Start de code of upgrade van een Service Fabric-cluster.

Valideren van de opgegeven parameters voor de upgrade en start de upgrade of de configuratie van een Service Fabric-cluster als de parameters geldig zijn.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-health-kaart | JSON gecodeerde woordenlijst van paren toepassingsnaam en het maximale percentage slecht voordat de fout. |
| --app-type-health-kaart | JSON gecodeerde woordenlijst van paren met de naam van het toepassingstype en het maximale percentage slecht voordat de fout. |
| --code-versie | De versie van de cluster. |
| --config-versie | De versie van het cluster-configuratie. |
| --delta-health-evaluatie | Hiermee schakelt u delta de statusevaluatie in plaats van absolute health evaluatie na voltooiing van elk upgradedomein. |
| --delta slecht knooppunten | Het maximaal toegestane percentage van de knooppunten verslechtering van de status toegestaan tijdens upgrades van de cluster.  Standaard\: 10. <br><br> De verschillen wordt tussen de status van de knooppunten aan het begin van de upgrade en de status van de knooppunten op het moment van de statusevaluatie gemeten. De controle wordt uitgevoerd na de upgrade voltooiing van elke upgradedomein om te controleren of dat de globale status van het cluster is binnen de limieten voor verdragen. |
| --actie bij fout | Mogelijke waarden zijn\: 'Ongeldig', 'Terugdraaiactie', 'Manual'. |
| --geforceerd opnieuw opstarten | Geforceerd opnieuw opstarten. |
| --health-controle-probeer het opnieuw | Health selectievakje opnieuw time-out gemeten in milliseconden. |
| --health-controle-stabiel | Serverstatus controleren stabiele duur van de gemeten in milliseconden. |
| --wachtduur | Health controle wacht duur gemeten in milliseconden. |
| --replica-set-check-timeout | Upgrade replicaset selectievakje time-out gemeten in seconden. |
| --rolling-upgrade-modus | Mogelijke waarden zijn\: 'Ongeldig', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitored'.  Standaard\: UnmonitoredAuto. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |
| --slecht toepassingen | Het maximaal toegestane percentage van de beschadigde toepassingen voordat u een foutmelding. <br><br> Bijvoorbeeld, zodat 10% van toepassingen beschadigd is deze waarde 10. Het percentage vertegenwoordigt het verdragen maximumpercentage van toepassingen die beschadigd worden kunnen voordat het cluster wordt beschouwd als fout. Als het percentage is voldaan, maar er ten minste één slecht toepassingstype is, worden de status wordt geëvalueerd als de waarschuwing. Dit wordt berekend door het aantal beschadigde toepassingen delen via het totale aantal exemplaren van een toepassing in het cluster, met uitzondering van toepassingen van toepassingstypen die in de applicationtypehealthpolicymap; deze zijn opgenomen. De berekening rondt af naar boven op een storing op een klein aantal toepassingen tolereren. |
| --slecht knooppunten | Het maximaal toegestane percentage van slecht knooppunten voordat u een foutmelding. <br><br> Bijvoorbeeld, zodat 10% van de knooppunten beschadigd is deze waarde 10. Het percentage geeft de maximale verdragen percentage van de knooppunten die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage is voldaan, maar er ten minste één slecht knooppunt is, wordt de status wordt geëvalueerd als waarschuwing. Het percentage wordt berekend door het aantal beschadigde knooppunten delen via het totale aantal knooppunten in het cluster. De berekening rondt af naar boven op een storing op een klein aantal knooppunten tolereren. In grote clusters sommige knooppunten altijd worden omlaag of uit voor reparaties, zodat dit percentage moet worden geconfigureerd voor die tolereren. |
| --upgrade-domain-delta-slecht-knooppunten | Het maximaal toegestane percentage van de knooppunten van het upgradedomein verslechtering van de status toegestaan tijdens upgrades van de cluster.  Standaard\: 15. <br><br> De verschillen wordt tussen de status van de upgradedomein-knooppunten aan het begin van de upgrade en de status van de upgradedomein-knooppunten op het moment van de statusevaluatie gemeten. De controle wordt uitgevoerd na elke opwaarderen upgradedomein is voltooid voor alle upgradedomeinen om ervoor te zorgen dat de status van de upgradedomeinen is binnen de limieten voor verdragen. |
| --time-out-domein-upgrade | Time-out upgradedomein gemeten in milliseconden. |
| --upgrade-time-out | Time-out van de upgrade, gemeten in milliseconden. |
| --waarschuwing als fout | Waarschuwingen worden met de ernst van de dezelfde fouten behandeld. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-cluster-upgrade-resume"></a>sfctl cluster upgrade-hervatten
Controleer het upgraden van cluster gaat u verder met het volgende upgradedomein.

Het cluster code of configuratie upgrade verplaatsen op aanbrengen in het volgende upgradedomein, indien van toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --upgrade-domein [vereist] | Het volgende upgradedomein voor het upgraden van dit cluster. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-cluster-upgrade-rollback"></a>sfctl cluster upgrade-terugdraaien
De upgrade van een Service Fabric-cluster terugdraaien.

De code of upgrade van een Service Fabric-cluster terugdraaien.

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

## <a name="sfctl-cluster-upgrade-status"></a>sfctl cluster upgrade-status
Hiermee haalt u de voortgang van de huidige upgrade van de cluster.

Hiermee wordt de huidige voortgang van de lopende clusterupgrades opgehaald. Als u geen upgrade wordt uitgevoerd, krijgt u de laatste status van de vorige upgrade van de cluster.

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

## <a name="sfctl-cluster-upgrade-update"></a>sfctl cluster upgrade-update
Bijwerken van de parameters voor het bijwerken van de upgrade van een Service Fabric-cluster.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-health-kaart | JSON gecodeerde woordenlijst van paren toepassingsnaam en het maximale percentage slecht voordat de fout. |
| --app-type-health-kaart | JSON gecodeerde woordenlijst van paren met de naam van het toepassingstype en het maximale percentage slecht voordat de fout. |
| --delta-health-evaluatie | Hiermee schakelt u delta de statusevaluatie in plaats van absolute health evaluatie na voltooiing van elk upgradedomein. |
| --delta slecht knooppunten | Het maximaal toegestane percentage van de knooppunten verslechtering van de status toegestaan tijdens upgrades van de cluster.  Standaard\: 10. <br><br> De verschillen wordt tussen de status van de knooppunten aan het begin van de upgrade en de status van de knooppunten op het moment van de statusevaluatie gemeten. De controle wordt uitgevoerd na de upgrade voltooiing van elke upgradedomein om te controleren of dat de globale status van het cluster is binnen de limieten voor verdragen. |
| --actie bij fout | Mogelijke waarden zijn\: 'Ongeldig', 'Terugdraaiactie', 'Manual'. |
| --geforceerd opnieuw opstarten | Geforceerd opnieuw opstarten. |
| --health-controle-probeer het opnieuw | Health selectievakje opnieuw time-out gemeten in milliseconden. |
| --health-controle-stabiel | Serverstatus controleren stabiele duur van de gemeten in milliseconden. |
| --wachtduur | Health controle wacht duur gemeten in milliseconden. |
| --replica-set-check-timeout | Upgrade replicaset selectievakje time-out gemeten in seconden. |
| --rolling-upgrade-modus | Mogelijke waarden zijn\: 'Ongeldig', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitored'.  Standaard\: UnmonitoredAuto. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |
| --slecht toepassingen | Het maximaal toegestane percentage van de beschadigde toepassingen voordat u een foutmelding. <br><br> Bijvoorbeeld, zodat 10% van toepassingen beschadigd is deze waarde 10. Het percentage vertegenwoordigt het verdragen maximumpercentage van toepassingen die beschadigd worden kunnen voordat het cluster wordt beschouwd als fout. Als het percentage is voldaan, maar er ten minste één slecht toepassingstype is, worden de status wordt geëvalueerd als de waarschuwing. Dit wordt berekend door het aantal beschadigde toepassingen delen via het totale aantal exemplaren van een toepassing in het cluster, met uitzondering van toepassingen van toepassingstypen die in de applicationtypehealthpolicymap; deze zijn opgenomen. De berekening rondt af naar boven op een storing op een klein aantal toepassingen tolereren. |
| --slecht knooppunten | Het maximaal toegestane percentage van slecht knooppunten voordat u een foutmelding. <br><br> Bijvoorbeeld, zodat 10% van de knooppunten beschadigd is deze waarde 10. Het percentage geeft de maximale verdragen percentage van de knooppunten die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage is voldaan, maar er ten minste één slecht knooppunt is, wordt de status wordt geëvalueerd als waarschuwing. Het percentage wordt berekend door het aantal beschadigde knooppunten delen via het totale aantal knooppunten in het cluster. De berekening rondt af naar boven op een storing op een klein aantal knooppunten tolereren. In grote clusters sommige knooppunten altijd worden omlaag of uit voor reparaties, zodat dit percentage moet worden geconfigureerd voor die tolereren. |
| --upgrade-domain-delta-slecht-knooppunten | Het maximaal toegestane percentage van de knooppunten van het upgradedomein verslechtering van de status toegestaan tijdens upgrades van de cluster.  Standaard\: 15. <br><br> De verschillen wordt tussen de status van de upgradedomein-knooppunten aan het begin van de upgrade en de status van de upgradedomein-knooppunten op het moment van de statusevaluatie gemeten. De controle wordt uitgevoerd na elke opwaarderen upgradedomein is voltooid voor alle upgradedomeinen om ervoor te zorgen dat de status van de upgradedomeinen is binnen de limieten voor verdragen. |
| --time-out-domein-upgrade | Time-out upgradedomein gemeten in milliseconden. |
| --upgrade-type | Mogelijke waarden zijn\: 'Ongeldig', 'Rolling', 'Rolling_ForceRestart'.  Standaard\: Rolling. |
| --upgrade-time-out | Time-out van de upgrade, gemeten in milliseconden. |
| --waarschuwing als fout | Waarschuwingen worden met de ernst van de dezelfde fouten behandeld. |

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