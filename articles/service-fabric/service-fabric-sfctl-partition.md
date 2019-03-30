---
title: Azure Service Fabric CLI - sfctl-partition | Microsoft Docs
description: Beschrijving van de opdrachten van Service Fabric-CLI sfctl partitie.
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
ms.openlocfilehash: f7c9bcc51757100cb1fc957dee12213bc8bf2eec
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666763"
---
# <a name="sfctl-partition"></a>sfctl partition
Query's uitvoeren en beheren van partities voor elke service.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| verlies van gegevens | Deze API wordt verlies van gegevens voor de opgegeven partitie veroorzaken. |
| status van gegevens verloren gaan | Hiermee haalt u de voortgang van een partitie gegevens verloren gaan bewerking aan de slag met de API StartDataLoss. |
| gezondheidszorg | Hiermee haalt u de status van de opgegeven Service Fabric-partitie. |
| informatie | Hiermee haalt u de informatie over een Service Fabric-partitie. |
| list | Hiermee haalt u de lijst met partities van een Service Fabric-service. |
| laden | Hiermee haalt u de gegevens laden van de opgegeven Service Fabric-partitie. |
| load-reset | Hiermee stelt u de huidige belasting van een Service Fabric-partitie. |
| quorum-loss | Induceert quorumverlies voor een bepaalde stateful service-partitie. |
| quorum-loss-status | Hiermee haalt u de voortgang van een quorum-verlies-bewerking op een partitie die aan de slag met de API StartQuorumLoss. |
| recover | Geeft aan dat de Service Fabric-cluster dat het proberen moet te herstellen van een specifieke partitie die momenteel sprake van quorumverlies is vastgelopen. |
| herstellen-all | Geeft aan dat de Service Fabric-cluster dat het proberen moet te herstellen van de diensten (inclusief systeemservices) die momenteel sprake van quorumverlies zijn vastgelopen. |
| rapport-en statusbewaking | Verzendt een statusrapport over de Service Fabric-partitie. |
| restart | Deze API wordt opnieuw opgestart sommige of alle replica's of exemplaren van de opgegeven partitie. |
| opnieuw opstarten-status | Hiermee haalt u de voortgang van een aan de slag met StartPartitionRestart PartitionRestart-bewerking. |
| svc-name | Hiermee haalt u de naam van de Service Fabric-service voor een partitie. |

## <a name="sfctl-partition-data-loss"></a>sfctl partitie-verlies van gegevens
Deze API wordt verlies van gegevens voor de opgegeven partitie veroorzaken.

Er wordt een aanroep naar de API OnDataLossAsync van de partitie geactiveerd.  Deze API wordt verlies van gegevens voor de opgegeven partitie veroorzaken. Er wordt een aanroep naar de API OnDataLoss van de partitie geactiveerd. Werkelijke gegevensverlies is afhankelijk van de opgegeven DataLossMode.  <br> -PartialDataLoss - alleen een quorum van replica's zijn verwijderd en OnDataLoss wordt geactiveerd voor de partitie maar feitelijke gegevens verloren gaan, is afhankelijk van de aanwezigheid van de replicatie die onderweg zijn.  <br> -FullDataLoss - alle replica's worden verwijderd zodat alle gegevens verloren zijn gegaan en OnDataLoss wordt geactiveerd. Deze API moet alleen worden aangeroepen met een stateful service als het doel. Aanroepen van deze API met een systeemservice van het als het doel wordt niet aanbevolen.

> [!NOTE] 
> Nadat deze API is aangeroepen, kan niet ongedaan gemaakt. Aanroepen van CancelOperation wordt alleen voorkomen dat kan worden uitgevoerd en het opschonen van interne systeemstatus. Deze opnieuw gegevens niet als de opdracht zich ver genoeg om te leiden tot gegevensverlies. Roep de API GetDataLossProgress met dezelfde bewerkings-ID informatie geretourneerd over de bewerking aan de slag met deze API.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --gegevens-verlies-modus (vereist) | Deze opsomming is doorgegeven aan de API StartDataLoss om aan te geven welk type gegevens verloren gaan om te veroorzaken. |
| --bewerking-id (vereist) | Een GUID die een aanroep van deze API identificeert.  Deze informatie wordt doorgegeven aan de bijbehorende GetProgress-API. |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-partition-data-loss-status"></a>sfctl partitie gegevens-verlies-status
Hiermee haalt u de voortgang van een partitie gegevens verloren gaan bewerking aan de slag met de API StartDataLoss.

Hiermee haalt u de voortgang van een bewerking voor het verlies van gegevens aan de slag met StartDataLoss, met behulp van de bewerkings-id.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --bewerking-id (vereist) | Een GUID die een aanroep van deze API identificeert.  Deze informatie wordt doorgegeven aan de bijbehorende GetProgress-API. |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-partition-health"></a>sfctl partition health
Hiermee haalt u de status van de opgegeven Service Fabric-partitie.

EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen die zijn gerapporteerd voor de service op basis van de status. ReplicasHealthStateFilter gebruiken voor het filteren van de verzameling van ReplicaHealthState objecten op de partitie. Als u een partitie die niet in de health-store bestaat opgeeft, wordt in deze aanvraag een fout geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling van HealthEvent-objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden, verkregen met behulp van de bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde 6 is worden vervolgens alle gebeurtenissen met HealthState waarde OK (2) en waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --uitsluiten-health-statistieken | Geeft aan of de health-statistieken moet worden geretourneerd als onderdeel van het queryresultaat. Als de waarde False is, wordt standaard. De statistieken Toon het aantal onderliggende entiteiten in de status Ok, waarschuwing en fout. |
| --replica's-health-status-filter | Hiermee kunt u op de verzameling van objecten op de partitie ReplicaHealthState filteren. De waarde kan worden verkregen van leden of een bitsgewijze bewerkingen op leden van HealthStateFilter. Alleen de replica's die overeenkomen met het filter wordt geretourneerd. Alle replica's wordt gebruikt voor het evalueren van de geaggregeerde status. Indien niet opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde 6 is wordt vervolgens alle gebeurtenissen met HealthState waarde OK (2) en waarschuwing (4) geretourneerd. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-partition-info"></a>partitiegegevens sfctl
Hiermee haalt u de informatie over een Service Fabric-partitie.

Hiermee haalt u de informatie over de opgegeven partitie. Het antwoord bevat de partitie-ID, partitionering schema-informatie, sleutels die worden ondersteund door de partitie, status, gezondheid en andere details weer over de partitie.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-partition-list"></a>sfctl Partitielijst
Hiermee haalt u de lijst met partities van een Service Fabric-service.

Het antwoord bevat de partitie-ID, partitionering schema-informatie, sleutels die worden ondersteund door de partitie, status, gezondheid en andere details weer over de partitie.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
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

## <a name="sfctl-partition-load"></a>sfctl partitiebelasting
Hiermee haalt u de gegevens laden van de opgegeven Service Fabric-partitie.

Retourneert informatie over de belasting van een opgegeven partitie. Het antwoord bevat een lijst met rapporten laden voor een Service Fabric-partitie. Elk rapport bevat de load metrische naam, waarde en laatst gerapporteerde tijd in UTC.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-partition-load-reset"></a>sfctl partitie load-opnieuw instellen
Hiermee stelt u de huidige belasting van een Service Fabric-partitie.

Hiermee stelt de huidige belasting van een Service Fabric-partitie op de standaard-belasting voor de service.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-partition-quorum-loss"></a>sfctl partition quorum-loss
Induceert quorumverlies voor een bepaalde stateful service-partitie.

Deze API is handig voor een tijdelijke quorum verliezen situatie op uw service. Roep de API GetQuorumLossProgress met dezelfde bewerkings-ID informatie geretourneerd over de bewerking aan de slag met deze API. Dit kan alleen worden aangeroepen voor stateful persistent (HasPersistedState == true) services.  Gebruik deze API niet op stateless services of stateful in het geheugen wordt alleen services.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --bewerking-id (vereist) | Een GUID die een aanroep van deze API identificeert.  Deze informatie wordt doorgegeven aan de bijbehorende GetProgress-API. |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --quorum-verlies-duur (vereist) | De tijdsduur waarvoor de partitie in quorumverlies worden behouden.  Dit moet worden opgegeven in een paar seconden. |
| --quorum-loss-mode     [Required] | Deze opsomming is doorgegeven aan de API StartQuorumLoss om aan te geven welk type quorumverlies om te veroorzaken. |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-partition-quorum-loss-status"></a>sfctl partitie quorum-verlies-status
Hiermee haalt u de voortgang van een quorum-verlies-bewerking op een partitie die aan de slag met de API StartQuorumLoss.

Hiermee haalt u de voortgang van een quorum-verlies-bewerking aan de slag met StartQuorumLoss, met behulp van de opgegeven bewerkings-id.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --bewerking-id (vereist) | Een GUID die een aanroep van deze API identificeert.  Deze informatie wordt doorgegeven aan de bijbehorende GetProgress-API. |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-partition-recover"></a>sfctl partitie herstellen
Geeft aan dat de Service Fabric-cluster dat het proberen moet te herstellen van een specifieke partitie die momenteel sprake van quorumverlies is vastgelopen.

Met deze bewerking moet alleen worden uitgevoerd als bekend is dat de replica's die niet beschikbaar zijn, kunnen niet worden hersteld. Onjuist gebruik van deze API kan leiden tot verlies van gegevens.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-partition-recover-all"></a>sfctl partitie herstellen-all
Geeft aan dat de Service Fabric-cluster dat het proberen moet te herstellen van de diensten (inclusief systeemservices) die momenteel sprake van quorumverlies zijn vastgelopen.

Met deze bewerking moet alleen worden uitgevoerd als bekend is dat de replica's die niet beschikbaar zijn, kunnen niet worden hersteld. Onjuist gebruik van deze API kan leiden tot verlies van gegevens.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-partition-report-health"></a>sfctl partitie rapport-en statusbewaking
Verzendt een statusrapport over de Service Fabric-partitie.

Rapporteert de status van de opgegeven Service Fabric-partitie. Het rapport moet de informatie over de oorzaak van de health-rapport en de eigenschap waarop dit apparaat is gerapporteerd bevatten. Het rapport wordt verzonden naar een gateway Service Fabric-partitie, die wordt doorgestuurd naar de health-store. Het rapport kan worden geaccepteerd door de gateway, maar geweigerd door de health store na extra validatie. Het rapport kan bijvoorbeeld afwijzen, de health store vanwege een ongeldige parameter, zoals een verouderde volgnummer. Om te zien of het rapport in de health-store is toegepast, Controleer of het rapport wordt weergegeven in de sectie gebeurtenissen.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --health-eigenschap (vereist) | De eigenschap van de gegevens over de servicestatus. <br><br> Een entiteit kan statusrapporten voor de verschillende eigenschappen hebben. De eigenschap is een tekenreeks en niet een vaste-opsomming waarmee de journalist flexibiliteit voor het categoriseren van de voorwaarde staat dat het rapport wordt geactiveerd. Bijvoorbeeld, een Rapportagefout met SourceId "LocalWatchdog" die de status van de beschikbare schijfruimte op een knooppunt kunt controleren, zodat deze eigenschap "AvailableDisk" op dat knooppunt rapporteren kunt. De dezelfde journalist kan het knooppunt-connectiviteit, bewaken, zodat het een eigenschap 'Connectiviteit' op hetzelfde knooppunt rapporteren kan. In de winkel de gezondheid van worden deze rapporten behandeld als afzonderlijke health-gebeurtenissen voor het opgegeven knooppunt. Samen met de bron-id identificatie de eigenschap unieke van de gegevens over de servicestatus. |
| --status (vereist) | Mogelijke waarden zijn\: 'Ongeldige', 'Ok', 'Waarschuwing', 'Fout', 'Onbekend'. |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --bron-id (vereist) | De naam van de bron die de watchdog-client-systeem component aangeduid die de gezondheidsinformatie gegenereerd. |
| --Beschrijving | De beschrijving van de gegevens over de servicestatus. <br><br> Hiermee geeft u vrije tekst die wordt gebruikt om toe te voegen menselijke leesbare informatie over het rapport. De maximumlengte voor de beschrijving is 4096 tekens. Als de opgegeven tekenreeks langer is, worden deze automatisch afgekapt. Wanneer afgekapt, de laatste tekens van de beschrijving van de markering van een '[Truncated]' bevatten en totale grootte is 4096 tekens. De aanwezigheid van de markering geeft aan dat gebruikers die moet worden afgekapt is opgetreden. Houd er rekening mee dat wanneer afgekapt, de beschrijving van minder dan 4096 tekens uit de oorspronkelijke reeks heeft. |
| --immediate | Een vlag die aangeeft of het rapport direct moet worden verzonden. <br><br> Een statusrapport wordt verzonden naar een gateway Service Fabric-toepassing, die wordt doorgestuurd naar de health-store. Als direct is ingesteld op true, het rapport wordt direct verzonden van HTTP-Gateway naar de health-store, ongeacht de instellingen van de fabric-client die de HTTP-Gateway-toepassing wordt gebruikt. Dit is handig voor de kritieke rapporten die zo snel mogelijk moeten worden verzonden. Afhankelijk van de timing en andere voorwaarden mislukken verzenden van het rapport nog steeds, bijvoorbeeld als de HTTP-Gateway is gesloten of het bericht de Gateway niet bereiken. Als direct is ingesteld op false, wordt het rapport verzonden op basis van de clientinstellingen voor de status van de HTTP-Gateway. Het wordt daarom worden batchgewijs op basis van de configuratie van de HealthReportSendInterval. Dit is de aanbevolen instelling omdat hierdoor de health-client de gezondheid van berichten in health store, evenals verwerking van statusrapporten reporting optimaliseren. Standaard rapporten niet onmiddellijk verzonden. |
| --remove-when-expired | De waarde die aangeeft of het rapport is verwijderd uit health store wanneer het verloopt. <br><br> Als is ingesteld op true, wordt het rapport wordt verwijderd uit de store health nadat deze is verlopen. Indien ingesteld op false, het rapport wordt behandeld als een fout bij het verlopen. De waarde van deze eigenschap is ingesteld op false standaard. Als clients regelmatig rapporteren, moeten ze RemoveWhenExpired false (standaard) ingesteld. Op deze manier is de journalist heeft problemen (bijvoorbeeld impasse) en kan niet rapporteren, wordt de entiteit bij fout wordt geëvalueerd wanneer het statusrapport is verlopen. Deze vlaggen die de entiteit alsof ze de status fout. |
| --volgnummer: | Het volgnummer voor dit statusrapport als een numerieke tekenreeks. <br><br> Het volgnummer van het rapport wordt gebruikt door de health store voor het detecteren van verouderde rapporten. Indien niet opgegeven, is een volgnummer automatisch gegenereerd door de client health wanneer een rapport wordt toegevoegd. |
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

## <a name="sfctl-partition-restart"></a>sfctl partitie opnieuw opstarten
Deze API wordt opnieuw opgestart sommige of alle replica's of exemplaren van de opgegeven partitie.

Deze API is handig voor het testen van failover. Als u gebruikt voor het doel van de servicepartitie van een staatloze, moet RestartPartitionMode AllReplicasOrInstances. De GetPartitionRestartProgress API oproepen met dezelfde bewerkings-id ophalen van de voortgang.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --bewerking-id (vereist) | Een GUID die een aanroep van deze API identificeert.  Deze informatie wordt doorgegeven aan de bijbehorende GetProgress-API. |
| --partitie-id (vereist) | De identiteit van de partitie. |
| ---partitie-modus voor opnieuw opstarten (vereist) | Beschrijf welke partities opnieuw te starten. |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-partition-restart-status"></a>sfctl partitie opnieuw opstarten-status
Hiermee haalt u de voortgang van een aan de slag met StartPartitionRestart PartitionRestart-bewerking.

Hiermee haalt u de voortgang van een PartitionRestart aan de slag met StartPartitionRestart met behulp van de opgegeven bewerkings-id.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --bewerking-id (vereist) | Een GUID die een aanroep van deze API identificeert.  Deze informatie wordt doorgegeven aan de bijbehorende GetProgress-API. |
| --partitie-id (vereist) | De identiteit van de partitie. |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-partition-svc-name"></a>sfctl partitie svc-naam
Hiermee haalt u de naam van de Service Fabric-service voor een partitie.

Hiermee haalt u de naam van de service voor de opgegeven partitie. Een 404-fout wordt geretourneerd als de partitie-ID niet in het cluster bestaat.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --partitie-id (vereist) | De identiteit van de partitie. |
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
