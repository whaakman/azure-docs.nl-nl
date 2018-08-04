---
title: Azure Service Fabric CLI - sfctl-cluster | Microsoft Docs
description: Beschrijving van de opdrachten van Service Fabric-CLI sfctl-cluster.
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
ms.openlocfilehash: 4b0491d59e4ac495750a338ad743aab69ff47a4e
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494240"
---
# <a name="sfctl-cluster"></a>sfctl cluster
Selecteer, beheren en gebruiken van Service Fabric-clusters.

## <a name="commands"></a>Opdrachten

|Command|Beschrijving|
| --- | --- |
| code-versies | Hiermee haalt u een lijst van fabric code-versies die zijn ingericht in een Service Fabric-cluster. |
| configuratie-versies | Hiermee haalt u een lijst van de fabric config-versies die zijn ingericht in een Service Fabric-cluster. |
| status | Hiermee haalt u de status van een Service Fabric-cluster. |
| Het manifest | Haal het manifest van Service Fabric-cluster. |
| bewerking annuleren | Een gebruiker veroorzaakte fouten geannuleerd. |
| lijst van bewerking | Hiermee haalt u een lijst met gebruikers veroorzaakte fouten bewerkingen die zijn gefilterd op de opgegeven invoer. |
| inrichten | De code of configuratie-pakketten van een Service Fabric-cluster inrichten. |
| herstellen-systeem | Geeft aan dat de Service Fabric-cluster dat het proberen moet te herstellen van de systeemservices die momenteel zijn vastgelopen sprake van quorumverlies. |
| rapport-en statusbewaking | Verzendt een statusrapport over de Service Fabric-cluster. |
| selecteer | Maakt verbinding met het eindpunt van een Service Fabric-cluster. |
| show-connection | Weergeven welke dit sfctl-exemplaar is verbonden met Service Fabric-cluster. |
| inrichting verwijderen | Inrichting verwijderen van de code of configuratie-pakketten van een Service Fabric-cluster. |
| upgrade | Start het bijwerken van de versie van de code of configuratie van een Service Fabric-cluster. |
| upgrade-hervatten | Controleer de upgrade van het cluster gaat u verder met het volgende upgradedomein. |
| upgrade terugdraaien | De upgrade van een Service Fabric-cluster terugdraaien. |
| upgrade-status | Hiermee haalt u de voortgang van de upgrade van het huidige cluster. |
| upgrade-update | Werk de parameters voor het bijwerken van de upgrade van een Service Fabric-cluster. |

## <a name="sfctl-cluster-code-versions"></a>sfctl code-clusterversies
Hiermee haalt u een lijst van fabric code-versies die zijn ingericht in een Service Fabric-cluster.

Hiermee haalt u een lijst met informatie over het fabric code-versies die zijn ingericht in het cluster. De parameter CodeVersion kan worden gebruikt om te filteren (optioneel) de uitvoer naar alleen deze specifieke versie.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --code-versie | De versie van het product van Service Fabric. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-cluster-config-versions"></a>sfctl cluster config-versies
Hiermee haalt u een lijst van de fabric config-versies die zijn ingericht in een Service Fabric-cluster.

Hiermee haalt u een lijst met informatie over de fabric config-versies die zijn ingericht in het cluster. De parameter ConfigVersion kan worden gebruikt om te filteren (optioneel) de uitvoer naar alleen deze specifieke versie.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --config-versie | De config-versie van Service Fabric. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-cluster-health"></a>sfctl cluster health
Hiermee haalt u de status van een Service Fabric-cluster.

EventsHealthStateFilter gebruiken voor het filteren van de verzameling van gemeld op het cluster op basis van de status van de health-gebeurtenissen. NodesHealthStateFilter en ApplicationsHealthStateFilter ook gebruiken voor het filteren van de verzameling van knooppunten en toepassingen die worden geretourneerd op basis van hun cumulatieve status.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassingen-health-status-filter | Hiermee kunt u filteren van de toepassing health state objecten geretourneerd in het resultaat van het cluster health query op basis van hun status. De mogelijke waarden voor deze parameter zijn ontleend aan leden of een bitsgewijze bewerkingen op leden van de opsomming HealthStateFilter integer-waarde. Alleen toepassingen die overeenkomen met het filter worden geretourneerd. Alle toepassingen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde is 6 status van toepassingen met HealthState waarde OK (2) en waarschuwing (4) zijn teruggekeerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling van HealthEvent-objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden, verkregen met behulp van de bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde 6 is worden vervolgens alle gebeurtenissen met HealthState waarde OK (2) en waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --uitsluiten-health-statistieken | Geeft aan of de health-statistieken moet worden geretourneerd als onderdeel van het queryresultaat. Als de waarde False is, wordt standaard. De statistieken Toon het aantal onderliggende entiteiten in de status Ok, waarschuwing en fout. |
| --opnemen--toepassing-health-statistieken van het systeem | Geeft aan of de statistieken van de status van de fabric moet bevatten \: /System application health statistieken. Als de waarde False is, wordt standaard. Als IncludeSystemApplicationHealthStatistics is ingesteld op true, wordt de status statistieken omvatten de entiteiten die deel uitmaken van de infrastructuur \: /System application. Anders bevat het queryresultaat health statistieken alleen voor toepassingen. De health-statistieken moet worden opgenomen in het queryresultaat voor deze parameter moet worden toegepast. |
| --knooppunten-health-status-filter | Hiermee kunt u filteren van het knooppunt status status objecten geretourneerd in het resultaat van het cluster health query op basis van hun status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen de knooppunten die overeenkomen met het filter worden geretourneerd. Alle knooppunten worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde is 6 status van knooppunten met HealthState waarde OK (2) en waarschuwing (4) zijn teruggekeerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-cluster-manifest"></a>sfctl clustermanifest
Haal het manifest van Service Fabric-cluster.

Haal het manifest van Service Fabric-cluster. Het clustermanifest bevat eigenschappen van het cluster met verschillende typen in het cluster, beveiligingsconfiguraties, fouten, en upgradedomein topologieën, enzovoort. Deze eigenschappen worden opgegeven als onderdeel van het bestand ClusterConfig.JSON tijdens het implementeren van een zelfstandige cluster. Maar het merendeel van de informatie in het clustermanifest is intern gegenereerd door service fabric tijdens de Clusterimplementatie van het in andere scenario's voor implementatie (bijvoorbeeld bij het gebruik van Azure portal). De inhoud van het clustermanifest zijn uitsluitend ter informatie bedoeld en gebruikers niet worden geacht binnen een afhankelijkheid op de indeling van de inhoud van het bestand of de vertaling.

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

## <a name="sfctl-cluster-operation-cancel"></a>sfctl cluster bewerking annuleren
Een gebruiker veroorzaakte fouten geannuleerd.

De volgende API's starten fouttolerantie bewerkingen die kunnen worden geannuleerd via CancelOperation: StartDataLoss, StartQuorumLoss, StartPartitionRestart, StartNodeTransition. Als force ONWAAR is, klikt u vervolgens de opgegeven gebruiker veroorzaakte bewerking probleemloos gestopt en opgeschoond.  Als force ingesteld op true is, de opdracht wordt afgesloten en enkele interne status kan blijven staan.  Geforceerd op te geven als waar moet worden gebruikt met zorg. Aanroepen van deze API force is ingesteld op ' True ' is niet toegestaan totdat deze API is al aangeroepen op de dezelfde testopdracht force is ingesteld op false eerst of wanneer de testopdracht heeft al een OperationState OperationState.RollingBack. 

 Uitleg\: OperationState.RollingBack betekent dat het systeem worden/ruimt interne systeem status doordat de opdracht wordt uitgevoerd. Deze opnieuw gegevens niet als de testopdracht was om te leiden tot gegevensverlies.  Bijvoorbeeld, als u StartDataLoss Roep vervolgens deze API aanroepen, wordt het systeem alleen schoon interne status van de opdracht uitvoert. Herstel de doelpartitie gegevens, wordt deze niet als de opdracht vooruitgang geboekt toe om te leiden tot gegevensverlies. 

> [!NOTE]
> Als deze API is aangeroepen met force == ' True ', interne status kan blijven staan.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --bewerking-id (vereist) | Een GUID die een aanroep van deze API identificeert.  Deze informatie wordt doorgegeven aan de bijbehorende GetProgress-API. |
| --forceren | Hiermee wordt aangegeven of probleemloos terugdraaien en het opschonen van de status van het interne systeem gewijzigd door de gebruiker die bewerking kan worden uitgevoerd. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-cluster-operation-list"></a>sfctl cluster bewerking-lijst
Hiermee haalt u een lijst met gebruikers veroorzaakte fouten bewerkingen die zijn gefilterd op de opgegeven invoer.

Hiermee haalt u de lijst met gebruikers veroorzaakte fouten bewerkingen die zijn gefilterd op de opgegeven invoer.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --staat-filter | Gebruikt om te filteren op de OperationState voor gebruiker-configuratieset bewerkingen. <br> 65535 - Alles selecteren <br> 1 - selecteren die wordt uitgevoerd <br> 2 - Selecteer ongedaan maken <br>8 - voltooid selecteren <br>16 - Faulted selecteren <br>32 - geannuleerd selecteren <br>64 - ForceCancelled selecteren.  <br>Standaard\: 65535. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --type filteren | Gebruikt om te filteren op OperationType voor gebruiker-configuratieset bewerkingen. <br> 65535 - Alles selecteren <br> 1 - PartitionDataLoss selecteren. <br> 2 - PartitionQuorumLoss selecteren. <br> 4 - PartitionRestart selecteren. <br> 8 - NodeTransition selecteren.  <br> Standaard\: 65535. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-cluster-provision"></a>sfctl-cluster inrichten
De code of configuratie-pakketten van een Service Fabric-cluster inrichten.

Valideren en de code of configuratie-pakketten van een Service Fabric-cluster inrichten.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --cluster-manifest-bestand-pad | Het pad van het manifestbestand cluster. |
| --code-bestandspad | Het cluster code pakket bestandspad. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-cluster-recover-system"></a>sfctl cluster herstellen-systeem
Geeft aan dat de Service Fabric-cluster dat het proberen moet te herstellen van de systeemservices die momenteel zijn vastgelopen sprake van quorumverlies.

Geeft aan dat de Service Fabric-cluster dat het proberen moet te herstellen van de systeemservices die momenteel zijn vastgelopen sprake van quorumverlies. Met deze bewerking moet alleen worden uitgevoerd als bekend is dat de replica's die niet beschikbaar zijn, kunnen niet worden hersteld. Onjuist gebruik van deze API kan leiden tot verlies van gegevens.

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

## <a name="sfctl-cluster-report-health"></a>sfctl cluster rapport-en statusbewaking
Verzendt een statusrapport over de Service Fabric-cluster.

Het rapport moet de informatie over de oorzaak van de health-rapport en de eigenschap waarop dit apparaat is gerapporteerd bevatten. Het rapport wordt verzonden naar een Service Fabric gateway-knooppunt, die naar de health-store doorstuurt. Het rapport kan worden geaccepteerd door de gateway, maar geweigerd door de health store na extra validatie. Het rapport kan bijvoorbeeld afwijzen, de health store vanwege een ongeldige parameter, zoals een verouderde volgnummer. Om te zien of het rapport in de health-store is toegepast, Controleer of het rapport wordt weergegeven in de HealthEvents van het cluster.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --health-eigenschap (vereist) | De eigenschap van de gegevens over de servicestatus. <br><br> Een entiteit kan statusrapporten voor de verschillende eigenschappen hebben. De eigenschap is een tekenreeks en niet een vaste-opsomming waarmee de journalist flexibiliteit voor het categoriseren van de voorwaarde staat dat het rapport wordt geactiveerd. Bijvoorbeeld, een Rapportagefout met SourceId "LocalWatchdog" die de status van de beschikbare schijfruimte op een knooppunt kunt controleren, zodat deze eigenschap "AvailableDisk" op dat knooppunt rapporteren kunt. De dezelfde journalist kan het knooppunt-connectiviteit, bewaken, zodat het een eigenschap 'Connectiviteit' op hetzelfde knooppunt rapporteren kan. In de winkel de gezondheid van worden deze rapporten behandeld als afzonderlijke health-gebeurtenissen voor het opgegeven knooppunt. Samen met de bron-id identificatie de eigenschap unieke van de gegevens over de servicestatus. |
| --status (vereist) | Mogelijke waarden zijn\: 'Ongeldige', 'Ok', 'Waarschuwing', 'Fout', 'Onbekend'. |
| --bron-id (vereist) | De naam van de bron waarmee het onderdeel watchdog-client-systeem dat de statusgegevens gegenereerd uniek wordt geïdentificeerd. |
| --Beschrijving | De beschrijving van de gegevens over de servicestatus. <br><br> Hiermee geeft u vrije tekst die wordt gebruikt om toe te voegen menselijke leesbare informatie over het rapport. De maximumlengte voor de beschrijving is 4096 tekens. Als de opgegeven tekenreeks langer is, worden deze automatisch afgekapt. Wanneer afgekapt, de laatste tekens van de beschrijving van de markering van een '[Truncated]' bevatten en totale grootte is 4096 tekens. De aanwezigheid van de markering geeft aan dat gebruikers die moet worden afgekapt is opgetreden. Houd er rekening mee dat wanneer afgekapt, de beschrijving van minder dan 4096 tekens uit de oorspronkelijke reeks heeft. |
| --direct | Een markering waarmee wordt aangegeven of het rapport direct moet worden verzonden. <br><br> Een statusrapport wordt verzonden naar een gateway Service Fabric-toepassing, die wordt doorgestuurd naar de health-store. Als direct is ingesteld op true, het rapport wordt direct verzonden van HTTP-Gateway naar de health-store, ongeacht de instellingen van de fabric-client die de HTTP-Gateway-toepassing wordt gebruikt. Dit is handig voor de kritieke rapporten die zo snel mogelijk moeten worden verzonden. Afhankelijk van de timing en andere voorwaarden mislukken verzenden van het rapport nog steeds, bijvoorbeeld als de HTTP-Gateway is gesloten of het bericht de Gateway niet bereiken. Als direct is ingesteld op false, wordt het rapport verzonden op basis van de clientinstellingen voor de status van de HTTP-Gateway. Het wordt daarom worden batchgewijs op basis van de configuratie van de HealthReportSendInterval. Dit is de aanbevolen instelling omdat hierdoor de health-client de gezondheid van berichten in health store, evenals verwerking van statusrapporten reporting optimaliseren. Standaard rapporten niet onmiddellijk verzonden. |
| --verwijderen wanneer verlopen | De waarde die aangeeft of het rapport is verwijderd uit health store wanneer het verloopt. <br><br> Als is ingesteld op true, wordt het rapport wordt verwijderd uit de store health nadat deze is verlopen. Indien ingesteld op false, het rapport wordt behandeld als een fout bij het verlopen. De waarde van deze eigenschap is ingesteld op false standaard. Als clients regelmatig rapporteren, moeten ze RemoveWhenExpired false (standaard) ingesteld. Op deze manier is de journalist heeft problemen (bijvoorbeeld impasse) en kan niet rapporteren, wordt de entiteit bij fout wordt geëvalueerd wanneer het statusrapport is verlopen. Deze vlaggen die de entiteit alsof ze de status fout. |
| --volgnummer: | Het volgnummer voor dit statusrapport als een numerieke tekenreeks. <br><br> Het volgnummer van het rapport wordt gebruikt door de health store voor het detecteren van verouderde rapporten. Indien niet opgegeven, is een volgnummer automatisch gegenereerd door de client health wanneer een rapport wordt toegevoegd. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --ttl | De tijdsduur waarvoor deze statusrapport geldig is. Dit veld gebruikt voor het opgeven van de duur van de ISO8601-notatie. <br><br> Als clients regelmatig rapporteren, moeten ze rapporten verzenden met een hogere frequentie dan time to live van. Als clients een over de overgang rapport, kunnen ze time to live op oneindig ingesteld. Wanneer time to live van is verlopen, de statusgebeurtenis met de health-gegevens is verwijderd uit health store, als RemoveWhenExpired true, of op een fout, geëvalueerd als onwaar RemoveWhenExpired. Indien niet opgegeven, time to live van oneindige waarde de standaardwaarde van. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-cluster-select"></a>sfctl cluster selecteren
Maakt verbinding met het eindpunt van een Service Fabric-cluster.

Als verbinding maakt met een beveiligd cluster, geeft u een absoluut pad naar een certificaat (.crt) en -sleutelbestand (.key) of één bestand met beide (.pem). Geef niet beide. Als u verbinding maakt met een beveiligd cluster, geef desgewenst ook een absoluut pad naar een CA bundle-bestand of map van vertrouwde CA-certificaten.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --eindpunt (vereist) | Eindpunt-URL, met inbegrip van de poort en HTTP of HTTPS-voorvoegsel van het cluster. |
| --aad | Azure Active Directory gebruiken voor verificatie. |
| --ca | Het absolute pad naar de directory van de CA certificaten moet worden behandeld als geldig of CA bundle-bestand. |
| --certificaat | Het absolute pad naar een certificaatbestand. |
| --sleutel | Het absolute pad naar certificaat-sleutelbestand. |
| --niet-controleren | Controle voor certificaten uitschakelen bij het gebruik van HTTPS, houd er rekening mee\: dit is een optie voor onveilig en mag niet worden gebruikt voor productie-omgevingen. |
| --pem | Het absolute pad naar het clientcertificaat als een .pem-bestand. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-cluster-show-connection"></a>sfctl cluster show-connection
Weergeven welke dit sfctl-exemplaar is verbonden met Service Fabric-cluster.

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-cluster-unprovision"></a>sfctl cluster unprovision
Inrichting verwijderen van de code of configuratie-pakketten van een Service Fabric-cluster.

Het wordt ondersteund voor afzonderlijk inrichting van code en configuratie.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --code-versie | De code van pakketversie van de cluster. |
| --config-versie | De versie van de cluster-manifest. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-cluster-upgrade"></a>sfctl cluster upgraden
Start het bijwerken van de versie van de code of configuratie van een Service Fabric-cluster.

Valideren van de opgegeven parameters voor de upgrade en start het bijwerken van de code of configuratie-versie van een Service Fabric-cluster als de parameters geldig zijn.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-status-map | JSON gecodeerd woordenlijst van paren van de toepassingsnaam en het maximale percentage niet in orde voordat de fout. |
| --app-type-health-map | JSON gecodeerd woordenlijst van paren van de naam van het toepassingstype en het maximale percentage beschadigde voordat de fout. |
| --code-versie | De versie van de cluster. |
| --config-versie | De versie van de cluster-configuratie. |
| --delta-health-evaluatie | Hiermee kunt evalueren delta in plaats van absolute evalueren na voltooiing van elk upgradedomein. |
| ----knooppunten met slechte deltastatus | De maximaal toegestane percentage van knooppunten verslechtering van de status toegestaan tijdens het upgraden van clusters.  Standaard\: 10. <br><br> De verschillen wordt tussen de status van de knooppunten aan het begin van de upgrade en de status van de knooppunten op het moment van de evaluatie van de status gemeten. De controle wordt uitgevoerd na elke upgrade upgradedomein is voltooid om ervoor te zorgen dat de algemene status van het cluster is binnen verdragen. |
| --actie bij fout | Mogelijke waarden zijn\: 'Ongeldig', 'Terugdraaien', 'Manual'. |
| --geforceerd opnieuw opstarten | Afdwingen dat opnieuw opstarten. |
| --health-controle-opnieuw proberen | Time-out voor de gezondheid van selectievakje opnieuw proberen, gemeten in milliseconden. |
| --health-controle-stabiel | Statuscontrole duur van stabiele gemeten in milliseconden. |
| --wachtduur | Wachttijd voor Health check gemeten in milliseconden. |
| --replica-set-check-timeout | Upgrade controleren-timeout gemeten in seconden van replicaset. |
| --rolling upgrade-modus | Mogelijke waarden zijn\: 'Ongeldig', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitored'.  Standaard\: UnmonitoredAuto. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --beschadigde toepassingen | De maximaal toegestane percentage van de beschadigde toepassingen voordat u een foutmelding. <br><br> Bijvoorbeeld, om toe te staan 10% van de toepassingen niet in orde, is deze waarde 10. Het percentage geeft het maximum aantal verdragen percentage van toepassingen die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage in acht wordt genomen, maar er ten minste één beschadigde toepassing is, wordt de status wordt geëvalueerd als waarschuwing. Dit wordt berekend door het aantal beschadigde toepassingen delen via het totale aantal exemplaren van een toepassing in het cluster, met uitzondering van toepassingen van toepassingstypen die zijn opgenomen in de ApplicationTypeHealthPolicyMap. De berekening rondt af naar één tolereren op kleine aantallen toepassingen. |
| --beschadigde knooppunten | De maximaal toegestane percentage van knooppunten met slechte voordat u een foutmelding. <br><br> Bijvoorbeeld, om toe te staan 10% van de knooppunten niet in orde, is deze waarde 10. Het percentage geeft het maximum aantal verdragen percentage van knooppunten die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage in acht wordt genomen, maar er ten minste één beschadigd knooppunt is, wordt de status wordt geëvalueerd als waarschuwing. Het percentage wordt berekend door het aantal knooppunten met slechte delen via het totale aantal knooppunten in het cluster. De berekening rondt af naar één tolereren op kleine aantallen knooppunten. In grote clusters sommige knooppunten altijd worden omlaag of uit voor herstellingen, zodat dit percentage moet worden geconfigureerd om te tolereren die. |
| --upgrade-domein---knooppunten met slechte deltastatus | De maximaal toegestane percentage van knooppunten in het upgradedomein verslechtering van de status toegestaan tijdens het upgraden van clusters.  Standaard\: 15. <br><br> De verschillen wordt tussen de status van de knooppunten in het upgradedomein aan het begin van de upgrade en de status van de knooppunten in het upgradedomein op het moment van de evaluatie van de status gemeten. De controle wordt uitgevoerd na elke upgrade upgradedomein is voltooid voor alle upgradedomeinen om te controleren of de status van de upgrade-domeinen binnen verdragen. |
| --upgrade-domein-time-out | Het upgradedomein gemeten in milliseconden. |
| --upgrade-time-out | Time-out voor het upgraden, gemeten in milliseconden. |
| --waarschuwing als fout | Waarschuwingen worden met de dezelfde ernst als fouten behandeld. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-cluster-upgrade-resume"></a>sfctl cluster upgrade-hervatten
Controleer de upgrade van het cluster gaat u verder met het volgende upgradedomein.

Overstappen cluster code of configuratie bijwerken op naar het volgende upgradedomein indien van toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --upgrade-domein (vereist) | Het volgende upgradedomein voor dit een clusterupgrade. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-cluster-upgrade-rollback"></a>sfctl cluster upgrade-terugdraaien
De upgrade van een Service Fabric-cluster terugdraaien.

Terugdraaien van een upgrade van de code of configuratie van een Service Fabric-cluster.

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

## <a name="sfctl-cluster-upgrade-status"></a>sfctl cluster upgrade-status
Hiermee haalt u de voortgang van de upgrade van het huidige cluster.

Hiermee haalt u de huidige voortgang van de lopende clusterupgrade. Als er momenteel geen upgrade uitgevoerd wordt, krijgt u de laatste status van de vorige upgrade van het cluster.

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

## <a name="sfctl-cluster-upgrade-update"></a>sfctl cluster upgrade-update
Werk de parameters voor het bijwerken van de upgrade van een Service Fabric-cluster.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-status-map | JSON gecodeerd woordenlijst van paren van de toepassingsnaam en het maximale percentage niet in orde voordat de fout. |
| --app-type-health-map | JSON gecodeerd woordenlijst van paren van de naam van het toepassingstype en het maximale percentage beschadigde voordat de fout. |
| --delta-health-evaluatie | Hiermee kunt evalueren delta in plaats van absolute evalueren na voltooiing van elk upgradedomein. |
| ----knooppunten met slechte deltastatus | De maximaal toegestane percentage van knooppunten verslechtering van de status toegestaan tijdens het upgraden van clusters.  Standaard\: 10. <br><br> De verschillen wordt tussen de status van de knooppunten aan het begin van de upgrade en de status van de knooppunten op het moment van de evaluatie van de status gemeten. De controle wordt uitgevoerd na elke upgrade upgradedomein is voltooid om ervoor te zorgen dat de algemene status van het cluster is binnen verdragen. |
| --actie bij fout | Mogelijke waarden zijn\: 'Ongeldig', 'Terugdraaien', 'Manual'. |
| --geforceerd opnieuw opstarten | Afdwingen dat opnieuw opstarten. |
| --health-controle-opnieuw proberen | Time-out voor de gezondheid van selectievakje opnieuw proberen, gemeten in milliseconden. |
| --health-controle-stabiel | Statuscontrole duur van stabiele gemeten in milliseconden. |
| --wachtduur | Wachttijd voor Health check gemeten in milliseconden. |
| --replica-set-check-timeout | Upgrade controleren-timeout gemeten in seconden van replicaset. |
| --rolling upgrade-modus | Mogelijke waarden zijn\: 'Ongeldig', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitored'.  Standaard\: UnmonitoredAuto. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --beschadigde toepassingen | De maximaal toegestane percentage van de beschadigde toepassingen voordat u een foutmelding. <br><br> Bijvoorbeeld, om toe te staan 10% van de toepassingen niet in orde, is deze waarde 10. Het percentage geeft het maximum aantal verdragen percentage van toepassingen die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage in acht wordt genomen, maar er ten minste één beschadigde toepassing is, wordt de status wordt geëvalueerd als waarschuwing. Dit wordt berekend door het aantal beschadigde toepassingen delen via het totale aantal exemplaren van een toepassing in het cluster, met uitzondering van toepassingen van toepassingstypen die zijn opgenomen in de ApplicationTypeHealthPolicyMap. De berekening rondt af naar één tolereren op kleine aantallen toepassingen. |
| --beschadigde knooppunten | De maximaal toegestane percentage van knooppunten met slechte voordat u een foutmelding. <br><br> Bijvoorbeeld, om toe te staan 10% van de knooppunten niet in orde, is deze waarde 10. Het percentage geeft het maximum aantal verdragen percentage van knooppunten die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage in acht wordt genomen, maar er ten minste één beschadigd knooppunt is, wordt de status wordt geëvalueerd als waarschuwing. Het percentage wordt berekend door het aantal knooppunten met slechte delen via het totale aantal knooppunten in het cluster. De berekening rondt af naar één tolereren op kleine aantallen knooppunten. In grote clusters sommige knooppunten altijd worden omlaag of uit voor herstellingen, zodat dit percentage moet worden geconfigureerd om te tolereren die. |
| --upgrade-domein---knooppunten met slechte deltastatus | De maximaal toegestane percentage van knooppunten in het upgradedomein verslechtering van de status toegestaan tijdens het upgraden van clusters.  Standaard\: 15. <br><br> De verschillen wordt tussen de status van de knooppunten in het upgradedomein aan het begin van de upgrade en de status van de knooppunten in het upgradedomein op het moment van de evaluatie van de status gemeten. De controle wordt uitgevoerd na elke upgrade upgradedomein is voltooid voor alle upgradedomeinen om te controleren of de status van de upgrade-domeinen binnen verdragen. |
| --upgrade-domein-time-out | Het upgradedomein gemeten in milliseconden. |
| --upgrade-type | Mogelijke waarden zijn\: 'Ongeldig', 'Rolling', 'Rolling_ForceRestart'.  Standaard\: Rolling. |
| --upgrade-time-out | Time-out voor het upgraden, gemeten in milliseconden. |
| --waarschuwing als fout | Waarschuwingen worden met de dezelfde ernst als fouten behandeld. |

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