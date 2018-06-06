---
title: Azure Service Fabric CLI - sfctl replica | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl replica-opdrachten.
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
ms.openlocfilehash: cd09fe906f77bb06f0ac7afaa6c6cce326dbfa5c
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763575"
---
# <a name="sfctl-replica"></a>sfctl replica
De replica's die deel uitmaken van servicepartities beheren.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| geïmplementeerd | Hiermee haalt u de details van replica is geïmplementeerd op een Service Fabric-knooppunt. |
| geïmplementeerd lijst | Hiermee haalt u de lijst van replica's die zijn geïmplementeerd op een Service Fabric-knooppunt. |
| status | Hiermee haalt u de status van een Service Fabric stateful service replica of stateless service-exemplaar. |
| informatie | Hiermee haalt u de informatie over een replica van een Service Fabric-partitie. |
| lijst | Hiermee haalt u de informatie over de replica's van de partitie van een Service Fabric-service. |
| verwijderen | Hiermee verwijdert u de replica van een service uitgevoerd op een knooppunt. |
| rapport-status | Verzendt een statusrapport voor de Service Fabric-replica. |
| opnieuw opstarten | De replica van een service van een permanente service uitgevoerd op een knooppunt opnieuw is opgestart. |

## <a name="sfctl-replica-deployed"></a>sfctl replica geïmplementeerd
Hiermee haalt u de details van replica is geïmplementeerd op een Service Fabric-knooppunt.

Hiermee haalt u de details van de replica die is geïmplementeerd op een Service Fabric-knooppunt. De gegevens omvatten service type, de servicenaam, de huidige servicebewerking, de huidige servicebewerking start datum / tijd, partitie-ID, replica-exemplaar-ID, gemelde laden en andere informatie.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --replica-id [vereist] | De id van de replica. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-replica-deployed-list"></a>geïmplementeerd sfctl replica-lijst
Hiermee haalt u de lijst van replica's die zijn geïmplementeerd op een Service Fabric-knooppunt.

Haalt de lijst met de informatie over de replica's die zijn geïmplementeerd op een Service Fabric-knooppunt. De informatie omvat partitie-ID, de replica-ID, de status van de replica, de naam van de service, de naam van het type van de service en andere informatie. PartitionId of ServiceManifestName query-parameters gebruiken om informatie over de geïmplementeerde replica's die overeenkomt met de opgegeven waarden voor die parameters te retourneren.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --partitie-id | De identiteit van de partitie. |
| --naam van een service manifest | De naam van een servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-replica-health"></a>sfctl replica health
Hiermee haalt u de status van een Service Fabric stateful service replica of stateless service-exemplaar.

Hiermee haalt u de status van een Service Fabric-replica. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen die zijn gerapporteerd voor de replica op basis van de status.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --replica-id [vereist] | De id van de replica. |
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

## <a name="sfctl-replica-info"></a>sfctl replica info
Hiermee haalt u de informatie over een replica van een Service Fabric-partitie.

Het antwoord bevat de ID, rol, status, status, knooppuntnaam, bedrijfstijd en andere details weer over de replica.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --replica-id [vereist] | De id van de replica. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-replica-list"></a>lijst met sfctl replica 's
Hiermee haalt u de informatie over de replica's van de partitie van een Service Fabric-service.

Het eindpunt GetReplicas retourneert informatie over de replica's van de opgegeven partitie. Het antwoord bevat de ID, rol, status, status, knooppuntnaam, bedrijfstijd en andere details weer over de replica.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --vervolgtoken | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-replica-remove"></a>sfctl replica verwijderen
Hiermee verwijdert u de replica van een service uitgevoerd op een knooppunt.

Deze API wordt gesimuleerd van een Service Fabric-replica-fout door het verwijderen van een replica van een Service Fabric-cluster. De verwijdering wordt de replica wordt gesloten, de replica aan de rol overgangen None, en vervolgens verwijdert alle van de informatie over de status van de replica uit het cluster. Deze API gecontroleerd van het pad naar replica de status verwijderen en simuleert het permanente Rapportpad voor fouttolerantie via de client-API's. Waarschuwing - Er zijn geen controles veiligheid wanneer deze API wordt gebruikt. Onjuist gebruik van deze API kan leiden tot verlies van gegevens voor stateful services. Bovendien is de vlag forceRemove geldt voor alle andere replica's die in hetzelfde proces worden gehost.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --replica-id [vereist] | De id van de replica. |
| --force-remove | Verwijder een Service Fabric-toepassing of service geforceerd zonder tussenkomst van de reeks correct afsluiten. Deze parameter kan worden gebruikt geforceerd verwijderen van een toepassing of service voor welke verwijderen krijgt een time-out vanwege problemen met de code die voorkomt dat correcte sluit van replica's. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-replica-report-health"></a>sfctl replica rapport-status
Verzendt een statusrapport voor de Service Fabric-replica.

Meldt de status van de opgegeven Service Fabric-replica. Het rapport moet bevatten informatie over de bron van de health-rapport en de eigenschap waarop deze wordt gerapporteerd. Het rapport wordt verzonden naar een Service Fabric-gateway Replica, die worden doorgestuurd naar de health store. Het rapport kan worden geaccepteerd door de gateway, maar geweigerd door de health store na extra validatie. De health store kan bijvoorbeeld het rapport weigeren vanwege een ongeldige parameter, zoals een verouderde volgnummer. Om te zien of het rapport in de health store is toegepast, Controleer of het rapport wordt weergegeven in de sectie gebeurtenissen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --health-eigenschap [vereist] | De eigenschap van de health-informatie. <br><br> Een entiteit kan statusrapporten voor de verschillende eigenschappen hebben. De eigenschap is een tekenreeks en niet een vaste opsomming de Rapportagefout flexibiliteit om de status-voorwaarde die het rapport activeert te categoriseren toe te passen. Een Rapportagefout met SourceId 'LocalWatchdog' kunt bijvoorbeeld de status van de beschikbare schijfruimte op een knooppunt, bewaken, zodat deze 'AvailableDisk'-eigenschap op dat knooppunt kunt rapporteren. De dezelfde Rapportagefout kan de connectiviteit knooppunt bewaken, zodat dit een 'Verbinding'-eigenschap op hetzelfde knooppunt kunt rapporteren. Deze rapporten worden in het archief health behandeld als afzonderlijke health-gebeurtenissen voor het opgegeven knooppunt. Samen met de bron-id identificatie de eigenschap een unieke voor de health-informatie. |
| --status [vereist] | Mogelijke waarden zijn\: 'Ongeldig', 'Ok', 'Waarschuwing', 'Fout', 'Onbekend'. |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --replica-id [vereist] | De identiteit van de partitie. |
| --bron-id [vereist] | De naam van de bron waarmee de component watchdog-client-systeem dat de statusgegevens gegenereerd. |
| --Beschrijving | De beschrijving van de health-informatie. <br><br> Hiermee geeft u de vrije tekst gebruikt voor het toevoegen van menselijke leesbare informatie over het rapport. De maximale tekenreekslengte voor de beschrijving is tekens (4096). Als de opgegeven tekenreeks langer is, worden deze automatisch afgekapt. Wanneer een afgekapt, de laatste tekens van de beschrijving bevat een markering '[Truncated]' en de tekenreeksgrootte van de totale is tekens (4096). De aanwezigheid van de markering geeft aan gebruikers die moet worden afgekapt is opgetreden. Houd er rekening mee dat wanneer afgekapt, de beschrijving is minder dan 4096 tekens uit de oorspronkelijke reeks. |
| --onmiddellijke | Een vlag die aangeeft of het rapport onmiddellijk moet worden verzonden. <br><br> Een statusrapport wordt verzonden naar een Service Fabric-gateway-toepassing die naar de health store doorstuurt. Als direct is ingesteld op true, het rapport wordt verzonden onmiddellijk door HTTP-Gateway naar de health store, ongeacht de instellingen van de fabric-client die de Gateway HTTP-toepassing wordt gebruikt. Dit is handig voor kritieke rapporten die zo snel mogelijk moeten worden verzonden. Afhankelijk van de timing en andere voorwaarden kan verzenden van het rapport nog steeds mislukken, bijvoorbeeld als de HTTP-Gateway is gesloten of het bericht de Gateway niet bereiken. Als direct is ingesteld op false, wordt het rapport verzonden op basis van de clientinstellingen voor de status van de HTTP-Gateway. Daarom deze wordt in batch worden opgenomen volgens de configuratie HealthReportSendInterval. Dit is de aanbevolen instelling omdat hiermee de health-client de statusrapportage berichten health store als verwerking van statusrapporten optimaliseren. Standaard rapporten niet onmiddellijk verzonden. |
| --verwijderen wanneer verlopen | De waarde die aangeeft of het rapport is verwijderd uit health store als deze verloopt. <br><br> Als is ingesteld op true, wordt het rapport wordt verwijderd uit de store health nadat het is verlopen. Indien ingesteld op false, het rapport wordt behandeld als een fout bij het verlopen. Deze eigenschap is standaard ingesteld op false. Wanneer clients periodiek verslag, moeten ze RemoveWhenExpired false (standaard) ingesteld. Op deze manier is de Rapportagefout heeft problemen (bijvoorbeeld impasse) en kan niet rapporteren, wordt de entiteit bij fout wordt geëvalueerd wanneer het statusrapport is verlopen. Hiermee wordt de entiteit als de status fout. |
| ---volgnummer | Het volgnummer voor dit statusrapport als een reeks cijfers. <br><br> Het volgnummer van het rapport wordt gebruikt door de health store voor het detecteren van verouderde rapporten. Als niet wordt opgegeven, is een volgnummer automatisch gegenereerd door de client health bij het toevoegen van een rapport. |
| --service type | Het soort service replica (stateless of stateful) waarvoor de status wordt gerapporteerd. Hieronder volgen de mogelijke waarden\: 'Stateless', 'Stateful'.  Standaard\: Stateful. |
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

## <a name="sfctl-replica-restart"></a>sfctl replica opnieuw opstarten
De replica van een service van een permanente service uitgevoerd op een knooppunt opnieuw is opgestart.

De replica van een service van een permanente service uitgevoerd op een knooppunt opnieuw is opgestart. Waarschuwing - Er zijn geen controles veiligheid wanneer deze API wordt gebruikt. Onjuist gebruik van deze API kan leiden tot verlies van beschikbaarheid voor stateful services.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --partitie-id [vereist] | De identiteit van de partitie. |
| --replica-id [vereist] | De id van de replica. |
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
