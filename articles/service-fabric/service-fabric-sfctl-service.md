---
title: Azure Service Fabric CLI - sfctl service | Microsoft Docs
description: Beschrijving van de opdrachten van Service Fabric-CLI sfctl-service.
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
ms.openlocfilehash: 84c2faaf137e19d78e7e17527feb50baebf8041b
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494571"
---
# <a name="sfctl-service"></a>sfctl service
Maken, verwijderen en beheren van de service, servicetypen en service-pakketten.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| App-naam | Hiermee haalt u de naam van de Service Fabric-toepassing voor een service. |
| code-pakket-list | Hiermee haalt u de lijst met codepakketten die zijn geïmplementeerd op een Service Fabric-knooppunt. |
| maken | Hiermee maakt u de opgegeven Service Fabric-service. |
| delete | Hiermee verwijdert u een bestaande Service Fabric-service. |
| geïmplementeerd-type | Hiermee haalt u de informatie over een opgegeven service-type van de toepassing die is geïmplementeerd op een knooppunt in een Service Fabric-cluster. |
| geïmplementeerd-type-list | Hiermee haalt u de lijst met de informatie over de servicetypen van de toepassingen die zijn geïmplementeerd op een knooppunt in een Service Fabric-cluster. |
| description | Hiermee haalt u de beschrijving van een bestaande Service Fabric-service. |
| Get-container-Logboeken | Hiermee haalt u de logboeken voor containers voor de container geïmplementeerd op een Service Fabric-knooppunt. |
| status | Hiermee haalt u de status van de opgegeven Service Fabric-service. |
| informatie | Hiermee haalt u de informatie over de specifieke service die behoren tot de Service Fabric-toepassing. |
| lijst | Hiermee haalt u de informatie over alle services die behoren tot de toepassing die is opgegeven door de toepassings-ID. |
| Het manifest | Hiermee haalt u de beschrijving van het servicetype van een manifest. |
| pakket implementeren | Downloadt pakketten die zijn gekoppeld aan de opgegeven servicemanifest voor de cache voor installatiekopieën van het opgegeven knooppunt. |
| package-health | Hiermee haalt de informatie over de status van een servicepakket voor een specifieke toepassing die is geïmplementeerd voor een Service Fabric-knooppunt en de toepassing. |
| package-info | Hiermee haalt u de lijst met service-pakketten die zijn geïmplementeerd op een Service Fabric-knooppunt die overeenkomt met precies de opgegeven naam. |
| pakket-lijst | Hiermee haalt u de lijst met service-pakketten die zijn geïmplementeerd op een Service Fabric-knooppunt. |
| herstellen | Geeft aan dat de Service Fabric-cluster dat het proberen moet te herstellen van de opgegeven service die momenteel sprake van quorumverlies is vastgelopen. |
| rapport-en statusbewaking | Verzendt een statusrapport van de Service Fabric-service. |
| oplossen | Een Service Fabric-partitie worden omgezet. |
| type-lijst | Hiermee haalt u de lijst met de informatie over de servicetypen die worden ondersteund door een ingerichte toepassingstype in een Service Fabric-cluster. |
| update | De opgegeven service met behulp van de beschrijving van de betreffende update-updates. |

## <a name="sfctl-service-app-name"></a>sfctl service app-naam
Hiermee haalt u de naam van de Service Fabric-toepassing voor een service.

Hiermee haalt u de naam van de toepassing voor de opgegeven service. Een 404 FABRIC_E_SERVICE_DOES_NOT_EXIST-fout wordt geretourneerd als een service met de opgegeven ID niet bestaat.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-code-package-list"></a>sfctl service code-pakket-list
Hiermee haalt u de lijst met codepakketten die zijn geïmplementeerd op een Service Fabric-knooppunt.

Hiermee haalt u de lijst met codepakketten die zijn geïmplementeerd op een Service Fabric-knooppunt voor de opgegeven toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --code pakketnaam | De naam van het codepakket opgegeven in het servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --naam van een service manifest | De naam van een servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-create"></a>sfctl-service maken
Hiermee maakt u de opgegeven Service Fabric-service.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id ' Mijntoep\~app1' in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --naam (vereist) | Naam van de service. Dit moet een onderliggend element van de toepassings-id. Dit is de volledige naam waaronder de `fabric\:` URI. Bijvoorbeeld service `fabric\:/A/B` een onderliggend item van application `fabric\:/A`. |
| --servicetype (vereist) | De naam van het type van de service. |
| --activering-modus | De modus voor activering voor het servicepakket. |
| --beperkingen | De plaatsingsbeperkingen als een tekenreeks. Plaatsingsbeperkingen zijn Booleaanse expressies in de eigenschappen van het knooppunt en toestaan voor het beperken van een service voor bepaalde knooppunten op basis van de servicevereisten. Bijvoorbeeld, om een service op de knooppunten waar NodeType blauw is het volgende opgeven\:"NodeColor == blue '. |
| --gecorreleerde-service | De naam van de doelservice correleren met. |
| --correlatie | De service met een bestaande service met behulp van een affiniteit uitlijning correleren. |
| --dns-naam | De DNS-naam van de service moet worden gemaakt. De Service Fabric-DNS-service moet zijn ingeschakeld voor deze instelling. |
| --aantal exemplaren | Het aantal instanties. Dit geldt voor stateless services alleen. |
| --int-schema | Geeft aan dat de service op uniforme wijze moet worden gepartitioneerd op tal van niet-ondertekende gehele getallen. |
| --int-schema-count | Het aantal partities binnen het gehele getal sleutel moet worden gemaakt, als u met behulp van een partitieschema uniform geheel getal zijn. |
| --int-schema-hoog | Het einde van het bereik van belangrijke geheel getal zijn, als een partitieschema uniform geheel getal zijn. |
| --int-schema-laag | Het begin van het bereik van belangrijke geheel getal zijn, als een partitieschema uniform geheel getal zijn. |
| --load-metrische gegevens | JSON gecodeerd overzicht van metrische gegevens die gebruikt wordt wanneer load balancing services over knooppunten. |
| --min-replica-set-grootte | De replica van de minimale instellen grootte als een getal. Dit geldt voor stateful services alleen. |
| --verplaatsen-kosten | Hiermee geeft u de kosten voor het verplaatsen van de service. Mogelijke waarden zijn\: 'Nul', 'Laag', 'Gemiddeld', 'Hoog'. |
| --met de naam schema | Geeft aan dat de service moet meerdere benoemde partities hebben. |
| --met de naam-schema-list | JSON gecodeerd lijst met namen voor het partitioneren van de service in, als de benoemde partitieschema. |
| --niet-persistent-status | Indien waar, geeft dit aan de service heeft geen permanente status opgeslagen op de lokale schijf, of alleen wordt status opgeslagen in het geheugen. |
| --plaatsing beleidslijst | JSON gecodeerd lijst met beleidsregels voor plaatsing voor de service, en alle gekoppelde domeinnamen. Beleidsregels kunnen worden een of meer van\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`. |
| --quorum-verlies-wait | De maximale duur, in seconden, waarvoor een partitie is toegestaan in een status van quorumverlies. Dit geldt voor stateful services alleen. |
| --replica-opnieuw opstarten-wait | De duur, in seconden tussen wanneer een replica uitvalt en wanneer een nieuwe replica wordt gemaakt. Dit geldt voor stateful services alleen. |
| --schalen-beleid | JSON gecodeerd lijst met beleidsregels voor deze service te schalen. |
| --singleton-schema | Geeft aan dat de service moet beschikken over één partitie of een niet-gepartitioneerde-service. |
| --zelfstandige door replica behouden | De maximale duur, in seconden, voor welke stand-by-wordt replica's onderhouden voordat het wordt verwijderd. Dit geldt voor stateful services alleen. |
| --stateful | Geeft aan dat de service is een stateful service. |
| --stateless | Geeft aan dat de service is een staatloze service. |
| --target-replica-set-grootte | Grootte van instellen de doelreplica als een getal. Dit geldt voor stateful services alleen. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-delete"></a>sfctl service verwijderen
Hiermee verwijdert u een bestaande Service Fabric-service.

Een service moet worden gemaakt voordat deze kan worden verwijderd. Standaard probeert Service Fabric te sluiten van service-replica's op correcte wijze en verwijder vervolgens de service. Echter als de service is hebt u problemen bij het sluiten van de replica zonder problemen, de delete-bewerking lang kan duren of zitten. Gebruik de optionele ForceRemove vlag de correcte sluiten van een reeks overslaan en de service geforceerd verwijderen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --force-remove | Een Service Fabric-toepassing of service verwijderen geforceerd zonder tussenkomst van de reeks correct afsluiten. Deze parameter kan worden gebruikt voor het geforceerd verwijderen van een toepassing of service voor welke verwijderen time-out vanwege problemen met de code die voorkomt dat vensters sluit van replica's. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-deployed-type"></a>sfctl service geïmplementeerd-type
Hiermee haalt u de informatie over een opgegeven service-type van de toepassing die is geïmplementeerd op een knooppunt in een Service Fabric-cluster.

Hiermee haalt u de lijst met de informatie over een specifieke service-type van de toepassingen die zijn geïmplementeerd op een knooppunt in een Service Fabric-cluster. Het antwoord bevat de naam van het type van de service, de registratiestatus, het codepakket dat deze en activering geregistreerd-ID van het servicepakket. Elk item vertegenwoordigt één activering van een servicetype onderscheiden door de id van de activering.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --service-type-naam (vereist) | Hiermee geeft u de naam van een type Service Fabric-service. |
| --naam van een service manifest | De naam van het servicemanifest voor het filteren van de lijst met geïmplementeerde service-informatie. Indien opgegeven, is het antwoord bevat alleen de informatie over servicetypen die zijn gedefinieerd in het servicemanifest van deze. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-deployed-type-list"></a>sfctl service geïmplementeerd-type-list
Hiermee haalt u de lijst met de informatie over de servicetypen van de toepassingen die zijn geïmplementeerd op een knooppunt in een Service Fabric-cluster.

Hiermee haalt u de lijst met de informatie over de servicetypen van de toepassingen die zijn geïmplementeerd op een knooppunt in een Service Fabric-cluster. Het antwoord bevat de naam van het type van de service, de registratiestatus, het codepakket dat deze en activering geregistreerd-ID van het servicepakket.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --naam van een service manifest | De naam van het servicemanifest voor het filteren van de lijst met geïmplementeerde service-informatie. Indien opgegeven, is het antwoord bevat alleen de informatie over servicetypen die zijn gedefinieerd in het servicemanifest van deze. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-description"></a>Beschrijving van de service sfctl
Hiermee haalt u de beschrijving van een bestaande Service Fabric-service.

Hiermee haalt u de beschrijving van een bestaande Service Fabric-service. Een service moet worden gemaakt voordat de beschrijving ervan kan worden verkregen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-get-container-logs"></a>sfctl get-container-Logboeken
Hiermee haalt u de logboeken voor containers voor de container geïmplementeerd op een Service Fabric-knooppunt.

Hiermee haalt u de logboeken voor containers voor de container geïmplementeerd op een Service Fabric-knooppunt voor de opgegeven codepakket.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --code-pakket-naam (vereist) | De naam van het codepakket opgegeven in het servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --service-manifest-naam (vereist) | De naam van een servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --vorige | Geeft aan of de containerlogboeken ophalen uit is afgesloten/dead containers van het exemplaar van code-pakket. |
| --staart | Het aantal regels om weer te geven vanaf het einde van de logboeken. De standaardwaarde is 100. 'alle' om weer te geven van de volledige Logboeken. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-health"></a>sfctl-servicestatus
Hiermee haalt u de status van de opgegeven Service Fabric-service.

Hiermee haalt u de statusgegevens van de opgegeven service. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen die zijn gerapporteerd voor de service op basis van de status. Gebruik PartitionsHealthStateFilter voor het filteren van de verzameling van partities die worden geretourneerd. Als u een service die niet in de health-store bestaat opgeeft, wordt in deze aanvraag een fout geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling van HealthEvent-objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden, verkregen met behulp van de bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde 6 is worden vervolgens alle gebeurtenissen met HealthState waarde OK (2) en waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --uitsluiten-health-statistieken | Geeft aan of de health-statistieken moet worden geretourneerd als onderdeel van het queryresultaat. Als de waarde False is, wordt standaard. De statistieken Toon het aantal onderliggende entiteiten in de status Ok, waarschuwing en fout. |
| --partities-health-status-filter | Kan het filteren van de partities health state objecten geretourneerd in het resultaat van de zoekopdracht van de health-service op basis van hun status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen de partities die overeenkomen met het filter worden geretourneerd. Alle partities worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarde die is verkregen met behulp van bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde 6 is wordt vervolgens de status van de partities met HealthState waarde OK (2) en waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-info"></a>sfctl service-informatie
Hiermee haalt u de informatie over de specifieke service die behoren tot de Service Fabric-toepassing.

Retourneert informatie over de opgegeven service die behoren tot de opgegeven Service Fabric-toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-list"></a>lijst van de service sfctl
Hiermee haalt u de informatie over alle services die behoren tot de toepassing die is opgegeven door de toepassings-ID.

Retourneert informatie over alle services die behoren tot de toepassing die is opgegeven door de toepassings-ID.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --vervolgtoken | De voortzetting van token-parameter wordt gebruikt om op te halen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in één antwoord passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep retourneert de API volgende set met resultaten. Als er geen verdere resultaten, klikt u vervolgens bevat het vervolgtoken een waarde. De waarde van deze parameter mag geen URL-codering. |
| --service-type-naam | De naam van de service type is gebruikt voor het filteren van de services aan te vragen voor. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-manifest"></a>sfctl-servicemanifest
Hiermee haalt u de beschrijving van het servicetype van een manifest.

Hiermee haalt u de beschrijving van het servicetype van een manifest. Het antwoord bevat de XML van het servicemanifest als een tekenreeks.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-type-naam (vereist) | De naam van het toepassingstype. |
| --toepassing-type-versie (vereist) | De versie van het toepassingstype. |
| --service-manifest-naam (vereist) | De naam van een servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-package-deploy"></a>sfctl service pakket implementeren
Downloadt pakketten die zijn gekoppeld aan de opgegeven servicemanifest voor de cache voor installatiekopieën van het opgegeven knooppunt.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-type-naam (vereist) | De naam van het manifest van de toepassing voor het bijbehorende aangevraagde servicemanifest. |
| --app-type-versie (vereist) | De versie van het manifest van de toepassing voor het bijbehorende aangevraagde servicemanifest. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --service-manifest-naam (vereist) | De naam van het servicemanifest die zijn gekoppeld aan de pakketten die worden gedownload. |
| --share-beleid | Lijst met JSON gecodeerd van het delen van beleid. Voor elk beleidselement in delen bestaat uit een 'name' en 'bereik'. De naam overeenkomt met de naam van het pakket met code, configuratie of gegevens die moet worden gedeeld. Het bereik mag 'None', 'All', 'Code', 'Config' of 'Gegevens'. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-package-health"></a>sfctl pakket-servicestatus
Hiermee haalt de informatie over de status van een servicepakket voor een specifieke toepassing die is geïmplementeerd voor een Service Fabric-knooppunt en de toepassing.

Hiermee haalt de informatie over de status van een servicepakket voor een specifieke toepassing die is geïmplementeerd op een Service Fabric-knooppunt. Gebruik EventsHealthStateFilter om te filteren op (optioneel) de verzameling van HealthEvent-objecten die zijn gerapporteerd voor de geïmplementeerde service-pakket dat is gebaseerd op de status.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --service-pakket-naam (vereist) | De naam van het servicepakket. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling van HealthEvent-objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden, verkregen met behulp van de bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde 6 is worden vervolgens alle gebeurtenissen met HealthState waarde OK (2) en waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-package-info"></a>sfctl service-pakket-informatie
Hiermee haalt u de lijst met service-pakketten die zijn geïmplementeerd op een Service Fabric-knooppunt die overeenkomt met precies de opgegeven naam.

Retourneert informatie over de servicepakketten dat is geïmplementeerd op een Service Fabric-knooppunt voor de opgegeven toepassing. Deze resultaten zijn van de service-pakketten waarvan de naam overeenkomt met precies de naam van de service-pakket opgegeven als parameter.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --service-pakket-naam (vereist) | De naam van het servicepakket. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-package-list"></a>sfctl service pakketlijst
Hiermee haalt u de lijst met service-pakketten die zijn geïmplementeerd op een Service Fabric-knooppunt.

Retourneert informatie over de servicepakketten dat is geïmplementeerd op een Service Fabric-knooppunt voor de opgegeven toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-recover"></a>sfctl service herstellen
Geeft aan dat de Service Fabric-cluster dat het proberen moet te herstellen van de opgegeven service die momenteel sprake van quorumverlies is vastgelopen.

Geeft aan dat de Service Fabric-cluster dat het proberen moet te herstellen van de opgegeven service die momenteel sprake van quorumverlies is vastgelopen. Met deze bewerking moet alleen worden uitgevoerd als bekend is dat de replica's die niet beschikbaar zijn, kunnen niet worden hersteld. Onjuist gebruik van deze API kan leiden tot verlies van gegevens.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-report-health"></a>sfctl rapport-servicestatus
Verzendt een statusrapport van de Service Fabric-service.

Status van de opgegeven Service Fabric-service-rapporten. Het rapport moet de informatie over de oorzaak van de health-rapport en de eigenschap waarop dit apparaat is gerapporteerd bevatten. Het rapport wordt verzonden naar een Service Fabric-gateway-Service, die wordt doorgestuurd naar de health-store. Het rapport kan worden geaccepteerd door de gateway, maar geweigerd door de health store na extra validatie. Het rapport kan bijvoorbeeld afwijzen, de health store vanwege een ongeldige parameter, zoals een verouderde volgnummer. Om te zien of het rapport in de health-store is toegepast, Controleer of het rapport wordt weergegeven in de health-gebeurtenissen van de service.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --health-eigenschap (vereist) | De eigenschap van de gegevens over de servicestatus. <br><br> Een entiteit kan statusrapporten voor de verschillende eigenschappen hebben. De eigenschap is een tekenreeks en niet een vaste-opsomming waarmee de journalist flexibiliteit voor het categoriseren van de voorwaarde staat dat het rapport wordt geactiveerd. Bijvoorbeeld, een Rapportagefout met SourceId "LocalWatchdog" die de status van de beschikbare schijfruimte op een knooppunt kunt controleren, zodat deze eigenschap "AvailableDisk" op dat knooppunt rapporteren kunt. De dezelfde journalist kan het knooppunt-connectiviteit, bewaken, zodat het een eigenschap 'Connectiviteit' op hetzelfde knooppunt rapporteren kan. In de winkel de gezondheid van worden deze rapporten behandeld als afzonderlijke health-gebeurtenissen voor het opgegeven knooppunt. Samen met de bron-id identificatie de eigenschap unieke van de gegevens over de servicestatus. |
| --status (vereist) | Mogelijke waarden zijn\: 'Ongeldige', 'Ok', 'Waarschuwing', 'Fout', 'Onbekend'. |
| --service-id (vereist) | De identiteit van de service. <br><br> Dit is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1', de service-identiteit is ' Mijntoep\~app1\~svc1' in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
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

## <a name="sfctl-service-resolve"></a>sfctl service oplossen
Een Service Fabric-partitie worden omgezet.

Een partitie van de Service Fabric-service om op te halen van de eindpunten van de service-replica's worden omgezet.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --service-id (vereist) | De identiteit van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1 ', de service-identiteit is ' Mijntoep\~app1\~svc1 "in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --partitie-sleutel-type | Type van de sleutel voor de partitie. Deze parameter is vereist als het partitieschema van de service Int64Range of naam is. De mogelijke waarden zijn te volgen. -Geen (1) - geeft aan dat de parameter PartitionKeyValue niet is opgegeven. Dit is ongeldig voor de partities met het partitieschema als Singleton. Dit is de standaardwaarde. De waarde is 1. -Int64Range (2) - geeft aan dat de parameter PartitionKeyValue de partitiesleutel van een gegevenstype int64. Dit is ongeldig voor de partities met het partitieschema als Int64Range. De waarde is 2. -Met de naam (3) - geeft aan dat de parameter PartitionKeyValue een naam op van de partitie is. Dit is ongeldig voor de partities met het partitieschema als naam. De waarde is 3. |
| --partitie-sleutel / waarde- | Partitiesleutel. Dit is vereist als het partitieschema van de service Int64Range of naam is. |
| --vorige rsp-versie | De waarde in het veld versie van het antwoord dat eerder is ontvangen. Dit is vereist als de gebruiker weet dat het resultaat dat is verkregen eerder verouderd is. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-type-list"></a>sfctl-lijst van service
Hiermee haalt u de lijst met de informatie over de servicetypen die worden ondersteund door een ingerichte toepassingstype in een Service Fabric-cluster.

Hiermee haalt u de lijst met de informatie over de servicetypen die worden ondersteund door een ingerichte toepassingstype in een Service Fabric-cluster. Het opgegeven type moet bestaan. Anders wordt een 404-status geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-type-naam (vereist) | De naam van het toepassingstype. |
| --toepassing-type-versie (vereist) | De versie van het toepassingstype. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-service-update"></a>sfctl service-update
De opgegeven service met behulp van de beschrijving van de betreffende update-updates.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --service-id (vereist) | De identiteit van de service. Dit is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de servicenaam van de is ' fabric\:/myapp/app1/svc1', de service-identiteit is ' Mijntoep\~app1\~svc1' in 6.0 en hoger en ' Mijntoep/app1/svc1' in eerdere versies. |
| --beperkingen | De plaatsingsbeperkingen als een tekenreeks. Plaatsingsbeperkingen zijn Booleaanse expressies in de eigenschappen van het knooppunt en toestaan voor het beperken van een service voor bepaalde knooppunten op basis van de servicevereisten. Bijvoorbeeld, om een service op de knooppunten waar NodeType blauw is het volgende opgeven\: "NodeColor == blue '. |
| --gecorreleerde-service | De naam van de doelservice correleren met. |
| --correlatie | De service met een bestaande service met behulp van een affiniteit uitlijning correleren. |
| --aantal exemplaren | Het aantal instanties. Dit geldt voor stateless services alleen. |
| --load-metrische gegevens | JSON gecodeerd overzicht van metrische gegevens die worden gebruikt wanneer de taakverdeling over meerdere knooppunten. |
| --min-replica-set-grootte | De replica van de minimale instellen grootte als een getal. Dit geldt voor stateful services alleen. |
| --verplaatsen-kosten | Hiermee geeft u de kosten voor het verplaatsen van de service. Mogelijke waarden zijn\: 'Nul', 'Laag', 'Gemiddeld', 'Hoog'. |
| --plaatsing beleidslijst | JSON gecodeerd lijst met beleidsregels voor plaatsing voor de service, en alle gekoppelde domeinnamen. Beleidsregels kunnen worden een of meer van\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`. |
| --quorum-verlies-wait | De maximale duur, in seconden, waarvoor een partitie is toegestaan in een status van quorumverlies. Dit geldt voor stateful services alleen. |
| --replica-opnieuw opstarten-wait | De duur, in seconden tussen wanneer een replica uitvalt en wanneer een nieuwe replica wordt gemaakt. Dit geldt voor stateful services alleen. |
| --schalen-beleid | JSON gecodeerd lijst met beleidsregels voor deze service te schalen. |
| --zelfstandige door replica behouden | De maximale duur, in seconden, voor welke stand-by-wordt replica's onderhouden voordat het wordt verwijderd. Dit geldt voor stateful services alleen. |
| --stateful | Geeft aan dat de doelservice is een stateful service. |
| --stateless | Geeft aan dat de doelservice is een staatloze service. |
| --target-replica-set-grootte | Grootte van instellen de doelreplica als een getal. Dit geldt voor stateful services alleen. |
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
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Meer informatie over het gebruik van de Service Fabric-CLI met behulp van de [voorbeelden van scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).
