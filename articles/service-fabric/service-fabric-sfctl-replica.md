---
title: Azure Service Fabric CLI - sfctl replica | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl replica-opdrachten.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: d0a7199ff0e9cb17c3fbc179a9b37a6620f521f9
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666814"
---
# <a name="sfctl-replica"></a>sfctl replica
De replica's die deel uitmaken van servicepartities beheren.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| geïmplementeerd | Hiermee haalt u de details van replica is geïmplementeerd op een Service Fabric-knooppunt. |
| lijst met geïmplementeerd | Hiermee haalt u de lijst met replica's die zijn geïmplementeerd op een Service Fabric-knooppunt. |
| gezondheidszorg | Hiermee haalt u de status van een Service Fabric stateful service-replica of een stateless service-exemplaar. |
| informatie | Hiermee haalt u de informatie over een replica van een Service Fabric-partitie. |
| list | Hiermee haalt u de informatie over de replica's van de partitie van een Service Fabric-service. |
| verwijderen | Hiermee verwijdert u de replica van een service die wordt uitgevoerd op een knooppunt. |
| rapport-en statusbewaking | Verzendt een statusrapport voor de Service Fabric-replica. |
| restart | Een service-replica van een persistente service wordt uitgevoerd op een knooppunt opnieuw is opgestart. |

## <a name="sfctl-replica-deployed"></a>sfctl replica geïmplementeerd
Hiermee haalt u de details van replica is geïmplementeerd op een Service Fabric-knooppunt.

Hiermee haalt u de details van de replica die is geïmplementeerd op een Service Fabric-knooppunt. De informatie bevat service type, de servicenaam van de, de huidige servicebewerking, huidige servicebewerking start datum en tijd, partitie-ID, replica/exemplaar-ID, gerapporteerde laden en andere informatie.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --replica-id (vereist) | De id van de replica. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-replica-deployed-list"></a>sfctl geïmplementeerd replica-lijst
Hiermee haalt u de lijst met replica's die zijn geïmplementeerd op een Service Fabric-knooppunt.

Hiermee haalt u de lijst met de informatie over de replica's die zijn geïmplementeerd op een Service Fabric-knooppunt. De informatie omvat partitie-ID, de replica-ID, de status van de replica, de naam van de service, de naam van het type van de service en andere informatie. PartitionId of ServiceManifestName query-parameters gebruiken om informatie over de geïmplementeerde replica's die overeenkomen met de opgegeven waarden voor deze parameters te retourneren.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --partitie-id | De identiteit van de partitie. |
| --naam van een service manifest | De naam van een servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-replica-health"></a>sfctl replica health
Hiermee haalt u de status van een Service Fabric stateful service-replica of een stateless service-exemplaar.

Hiermee haalt u de status van een Service Fabric-replica. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen die zijn gerapporteerd voor de replica op basis van de status.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --replica-id (vereist) | De id van de replica. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling van HealthEvent-objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden, verkregen met behulp van de bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde 6 is worden vervolgens alle gebeurtenissen met HealthState waarde OK (2) en waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-replica-info"></a>sfctl replica info
Hiermee haalt u de informatie over een replica van een Service Fabric-partitie.

Het antwoord bevat de ID, rol, status, status, de naam van knooppunt, uptime en andere details weer over de replica.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --replica-id (vereist) | De id van de replica. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-replica-list"></a>lijst met sfctl replica 's
Hiermee haalt u de informatie over de replica's van de partitie van een Service Fabric-service.

Het eindpunt GetReplicas retourneert informatie over de replica's van de opgegeven partitie. Het antwoord bevat de ID, rol, status, status, de naam van knooppunt, uptime en andere details weer over de replica.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --vervolgtoken | De voortzetting van token-parameter wordt gebruikt om op te halen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in één antwoord passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep retourneert de API volgende set met resultaten. Als er geen verdere resultaten, klikt u vervolgens bevat het vervolgtoken een waarde. De waarde van deze parameter mag geen URL-codering. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-replica-remove"></a>sfctl replica verwijderen
Hiermee verwijdert u de replica van een service die wordt uitgevoerd op een knooppunt.

Deze API simuleert een Service Fabric-replica-fout door het verwijderen van een replica van een Service Fabric-cluster. Het verwijderen van de replica wordt gesloten, overschakelt op de replica aan de rol geen, en vervolgens verwijdert alle van de informatie over de status van de replica uit het cluster. Deze API het replicapad voor het verwijderen van status gecontroleerd en simuleert de permanente Rapportpad voor fouttolerantie tot en met client-API's. Waarschuwing: Er zijn geen veiligheid controles uitgevoerd wanneer deze API wordt gebruikt. Onjuist gebruik van deze API kan leiden tot verlies van gegevens voor stateful services. Bovendien de vlag forceRemove heeft gevolgen voor alle andere replica's die worden gehost in hetzelfde proces.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --replica-id (vereist) | De id van de replica. |
| --force-remove | Een Service Fabric-toepassing of service verwijderen geforceerd zonder tussenkomst van de reeks correct afsluiten. Deze parameter kan worden gebruikt voor het geforceerd verwijderen van een toepassing of service voor welke verwijderen time-out vanwege problemen met de code die voorkomt dat vensters sluit van replica's. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-replica-report-health"></a>sfctl replica rapport-en statusbewaking
Verzendt een statusrapport voor de Service Fabric-replica.

Rapporteert de status van de opgegeven Service Fabric-replica. Het rapport moet de informatie over de oorzaak van de health-rapport en de eigenschap waarop dit apparaat is gerapporteerd bevatten. Het rapport wordt verzonden naar een Service Fabric-gateway Replica, waarbij wordt doorgestuurd naar de health-store. Het rapport kan worden geaccepteerd door de gateway, maar geweigerd door de health store na extra validatie. Het rapport kan bijvoorbeeld afwijzen, de health store vanwege een ongeldige parameter, zoals een verouderde volgnummer. Run ophalen om te zien of het rapport in de health-store is toegepast, replica-status en controleer of het rapport wordt weergegeven in de sectie HealthEvents.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --health-eigenschap (vereist) | De eigenschap van de gegevens over de servicestatus. <br><br> Een entiteit kan statusrapporten voor de verschillende eigenschappen hebben. De eigenschap is een tekenreeks en niet een vaste-opsomming waarmee de journalist flexibiliteit voor het categoriseren van de voorwaarde staat dat het rapport wordt geactiveerd. Bijvoorbeeld, een Rapportagefout met SourceId "LocalWatchdog" die de status van de beschikbare schijfruimte op een knooppunt kunt controleren, zodat deze eigenschap "AvailableDisk" op dat knooppunt rapporteren kunt. De dezelfde journalist kan het knooppunt-connectiviteit, bewaken, zodat het een eigenschap 'Connectiviteit' op hetzelfde knooppunt rapporteren kan. In de winkel de gezondheid van worden deze rapporten behandeld als afzonderlijke health-gebeurtenissen voor het opgegeven knooppunt. Samen met de bron-id identificatie de eigenschap unieke van de gegevens over de servicestatus. |
| --status (vereist) | Mogelijke waarden zijn\: 'Ongeldige', 'Ok', 'Waarschuwing', 'Fout', 'Onbekend'. |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --replica-id (vereist) | De identiteit van de partitie. |
| --bron-id (vereist) | De naam van de bron die de watchdog-client-systeem component aangeduid die de gezondheidsinformatie gegenereerd. |
| --Beschrijving | De beschrijving van de gegevens over de servicestatus. <br><br> Hiermee geeft u vrije tekst die wordt gebruikt om toe te voegen menselijke leesbare informatie over het rapport. De maximumlengte voor de beschrijving is 4096 tekens. Als de opgegeven tekenreeks langer is, worden deze automatisch afgekapt. Wanneer afgekapt, de laatste tekens van de beschrijving van de markering van een '[Truncated]' bevatten en totale grootte is 4096 tekens. De aanwezigheid van de markering geeft aan dat gebruikers die moet worden afgekapt is opgetreden. Houd er rekening mee dat wanneer afgekapt, de beschrijving van minder dan 4096 tekens uit de oorspronkelijke reeks heeft. |
| --immediate | Een vlag die aangeeft of het rapport direct moet worden verzonden. <br><br> Een statusrapport wordt verzonden naar een gateway Service Fabric-toepassing, die wordt doorgestuurd naar de health-store. Als direct is ingesteld op true, het rapport wordt direct verzonden van HTTP-Gateway naar de health-store, ongeacht de instellingen van de fabric-client die de HTTP-Gateway-toepassing wordt gebruikt. Dit is handig voor de kritieke rapporten die zo snel mogelijk moeten worden verzonden. Afhankelijk van de timing en andere voorwaarden mislukken verzenden van het rapport nog steeds, bijvoorbeeld als de HTTP-Gateway is gesloten of het bericht de Gateway niet bereiken. Als direct is ingesteld op false, wordt het rapport verzonden op basis van de clientinstellingen voor de status van de HTTP-Gateway. Het wordt daarom worden batchgewijs op basis van de configuratie van de HealthReportSendInterval. Dit is de aanbevolen instelling omdat hierdoor de health-client de gezondheid van berichten in health store, evenals verwerking van statusrapporten reporting optimaliseren. Standaard rapporten niet onmiddellijk verzonden. |
| --remove-when-expired | De waarde die aangeeft of het rapport is verwijderd uit health store wanneer het verloopt. <br><br> Als is ingesteld op true, wordt het rapport wordt verwijderd uit de store health nadat deze is verlopen. Indien ingesteld op false, het rapport wordt behandeld als een fout bij het verlopen. De waarde van deze eigenschap is ingesteld op false standaard. Als clients regelmatig rapporteren, moeten ze RemoveWhenExpired false (standaard) ingesteld. Op deze manier is de journalist heeft problemen (bijvoorbeeld impasse) en kan niet rapporteren, wordt de entiteit bij fout wordt geëvalueerd wanneer het statusrapport is verlopen. Deze vlaggen die de entiteit alsof ze de status fout. |
| --volgnummer: | Het volgnummer voor dit statusrapport als een numerieke tekenreeks. <br><br> Het volgnummer van het rapport wordt gebruikt door de health store voor het detecteren van verouderde rapporten. Indien niet opgegeven, is een volgnummer automatisch gegenereerd door de client health wanneer een rapport wordt toegevoegd. |
| --service-type | Het type van de service-replica (stateless of stateful) waarvoor de status wordt gerapporteerd. Hieronder vindt u de mogelijke waarden\: 'Stateless', 'Stateful'.  Standaard\: Stateful. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --ttl | De tijdsduur waarvoor deze statusrapport geldig is. Dit veld wordt ISO8601-notatie gebruikt voor het opgeven van de duur. <br><br> Als clients regelmatig rapporteren, moeten ze rapporten verzenden met een hogere frequentie dan time to live van. Als clients een over de overgang rapport, kunnen ze time to live op oneindig ingesteld. Wanneer time to live van is verlopen, de statusgebeurtenis met de health-gegevens is verwijderd uit health store, als RemoveWhenExpired true, of op een fout, geëvalueerd als onwaar RemoveWhenExpired. Indien niet opgegeven, time to live van oneindige waarde de standaardwaarde van. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-replica-restart"></a>sfctl replica opnieuw opstarten
Een service-replica van een persistente service wordt uitgevoerd op een knooppunt opnieuw is opgestart.

Een service-replica van een persistente service wordt uitgevoerd op een knooppunt opnieuw is opgestart. Waarschuwing: Er zijn geen veiligheid controles uitgevoerd wanneer deze API wordt gebruikt. Onjuist gebruik van deze API kan leiden tot verlies van beschikbaarheid voor stateful services.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --replica-id (vereist) | De id van de replica. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

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
