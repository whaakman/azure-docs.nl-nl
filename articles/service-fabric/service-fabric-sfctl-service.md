---
title: Azure Service Fabric CLI - sfctl service | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl service-opdrachten.
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
ms.openlocfilehash: e027bb7f61d19fc274f7eddd8f28e9e6dac099ce
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763541"
---
# <a name="sfctl-service"></a>sfctl service
Maken, verwijderen en beheren van de service, servicetypen en servicepakketten.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| App-naam | Hiermee haalt u de naam van de Service Fabric-toepassing voor een service. |
| code pakketlijst | Haalt de lijst met code-pakketten die zijn geïmplementeerd op een Service Fabric-knooppunt. |
| maken | De opgegeven Service Fabric-service maakt. |
| verwijderen | Hiermee verwijdert u een bestaande Service Fabric-service. |
| geïmplementeerd type | Hiermee haalt u de informatie over een opgegeven service-type van de toepassing geïmplementeerd op een knooppunt in een Service Fabric-cluster. |
| geïmplementeerd type lijst | Hiermee wordt de lijst met de informatie over de servicetypen van de toepassingen die zijn geïmplementeerd op een knooppunt in een Service Fabric-cluster opgehaald. |
| description | Hiermee haalt u de beschrijving van een bestaande Service Fabric-service. |
| Get-container-Logboeken | Hiermee haalt u de logboeken van de container voor de container geïmplementeerd op een Service Fabric-knooppunt. |
| status | Hiermee wordt de status van de opgegeven Service Fabric-service opgehaald. |
| informatie | Hiermee haalt u de informatie over de specifieke service die horen bij de Service Fabric-toepassing. |
| lijst | Hiermee haalt u de informatie over alle services die horen bij de toepassing die is opgegeven door de toepassings-ID. |
| Manifest | Hiermee haalt u de beschrijving van het servicetype van een manifest. |
| pakket implementeren | Downloadt pakketten die zijn gekoppeld aan de opgegeven servicemanifest aan het cachegeheugen van de installatiekopie op opgegeven knooppunt. |
| package-health | Hiermee haalt de informatie over de status van een servicepakket voor een bepaalde toepassing is geïmplementeerd voor een Service Fabric-knooppunt en de toepassing. |
| package-info | Haalt de lijst met servicepakketten die zijn geïmplementeerd op een Service Fabric-knooppunt die overeenkomt met de opgegeven naam. |
| pakket-lijst | Haalt de lijst met servicepakketten die zijn geïmplementeerd op een Service Fabric-knooppunt. |
| herstellen | Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van de opgegeven service dat momenteel is vastgelopen in quorumverlies. |
| rapport-status | Verzendt een statusrapport voor de Service Fabric-service. |
| oplossen | Een Service Fabric-partitie worden omgezet. |
| lijst van type | Hiermee haalt de lijst op met de informatie over servicetypen die worden ondersteund door een ingerichte toepassingstype in een Service Fabric-cluster. |
| bijwerken | De opgegeven service met behulp van de updatebeschrijving van de opgegeven updates. |

## <a name="sfctl-service-app-name"></a>sfctl service app-naam
Hiermee haalt u de naam van de Service Fabric-toepassing voor een service.

Hiermee haalt u de naam van de toepassing voor de opgegeven service. Een FABRIC_E_SERVICE_DOES_NOT_EXIST 404-fout wordt geretourneerd als een service met de opgegeven service-ID niet bestaat.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-code-package-list"></a>service sfctl code-pakket-lijst
Haalt de lijst met code-pakketten die zijn geïmplementeerd op een Service Fabric-knooppunt.

Haalt de lijst met code-pakketten die zijn geïmplementeerd op een Service Fabric-knooppunt voor de opgegeven toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --code pakketnaam | De naam van codepakket is opgegeven in servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
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

## <a name="sfctl-service-create"></a>sfctl service maken
De opgegeven Service Fabric-service maakt.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld ' fabric\:/myapp/app1 ", zou de toepassings-id ' myapp\~app1' in 6.0 + en ' myapp/app1' in eerdere versies. |
| --de naam [vereist] | De naam van de service. Dit moet een onderliggend element van de toepassings-ID. Dit is de volledige naam waaronder de `fabric\:` URI. Bijvoorbeeld service `fabric\:/A/B` onderliggend element is van toepassing `fabric\:/A`. |
| --servicetype [vereist] | Naam van het type van de service. |
| --activering-modus | De activering modus voor het servicepakket. |
| --beperkingen | De plaatsingsbeperkingen als een tekenreeks. Plaatsingsbeperkingen zijn boolean-expressies in de eigenschappen van het knooppunt en toestaan voor een service beperken tot bepaalde knooppunten op basis van de servicevereisten. Bijvoorbeeld, om een service op de knooppunten waar NodeType blauw is het volgende opgeven\:' NodeColor == blue '. |
| --gecorreleerde-service | Naam van de doelservice correleren met. |
| --correlatie | De service met een bestaande service met behulp van een affiniteit uitlijning correleren. |
| --dns-naam | De DNS-naam van de service moet worden gemaakt. De Service Fabric-DNS-service moet zijn ingeschakeld voor deze instelling. |
| --aantal exemplaren | Het aantal exemplaren. Dit geldt voor alleen stateless services. |
| --int-schema | Hiermee geeft u dat de service moet op uniforme wijze worden gepartitioneerd in een bereik van niet-ondertekende gehele getallen. |
| --int-schema-telling | Het aantal partities binnen het gehele getal sleutel moet worden gemaakt, als u een partitieschema uniform geheel getal. |
| --int-schema-hoog | Het einde van het bereik van sleutel geheel getal, als u een partitieschema uniform geheel getal. |
| --int-schema-laag | Het begin van het bereik van sleutel geheel getal, als u een partitieschema uniform geheel getal. |
| --load-metrische gegevens | JSON gecodeerde lijst met metrische gegevens die gebruikt wordt wanneer services taakverdeling over knooppunten. |
| --min-replica-set-grootte | De replica van de minimale instellen grootte als een getal. Dit geldt voor stateful services alleen. |
| --verplaatsen kosten | Hiermee geeft u de move-kosten voor de service. Mogelijke waarden zijn\: 'Nul', 'Laag', 'Gemiddeld', 'Hoog'. |
| --met de naam schema | Hiermee geeft u dat de service moet meerdere benoemde partities hebben. |
| --met de naam-schema-lijst | JSON gecodeerde lijst met namen voor het partitioneren van de service in, als het benoemde partitieschema gebruikt. |
| --niet-persistent-status | Indien waar, wordt hiermee de service heeft geen permanente status opgeslagen op de lokale schijf of alleen wordt status opgeslagen in het geheugen. |
| --plaatsing beleidslijst | JSON gecodeerde lijst met beleidsregels voor plaatsing voor de service, en alle gekoppelde domeinnamen. Beleidsregels kunnen worden een of meer van\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`. |
| --quorum-verlies-wait | De maximale duur in seconden, waarvoor een partitie is toegestaan in een status van quorumverlies. Dit geldt voor stateful services alleen. |
| --replica-opnieuw opstarten-wait | De duur in seconden tussen wanneer een replica uitvalt en wanneer een nieuwe replica wordt gemaakt. Dit geldt voor stateful services alleen. |
| --schalen beleid | JSON gecodeerde lijst met beleidsregels voor deze service te schalen. |
| --singleton-schema | Geeft aan de service moet één partitie hebben of een niet-gepartitioneerde-service. |
| --stand door replica behouden | De maximale duur in seconden, voor welke stand-by replica's behouden blijft voordat het wordt verwijderd. Dit geldt voor stateful services alleen. |
| --stateful | Geeft aan dat de service is een stateful service. |
| --staatloze | Geeft aan dat de service is een staatloze service. |
| ---replica-set-doelgrootte | De doelreplica instellen grootte als een getal. Dit geldt voor stateful services alleen. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-delete"></a>sfctl service verwijderen
Hiermee verwijdert u een bestaande Service Fabric-service.

Een service moet worden gemaakt voordat het kan worden verwijderd. Standaard probeert Service Fabric service replica's op correcte wijze sluiten en verwijder vervolgens de service. Als de service is met het sluiten van de replica probleemloos problemen, de delete-bewerking kan lang duren of echter hangen. Gebruik het optionele ForceRemove vlag de correcte sluiten van een reeks overslaan en de service geforceerd verwijderen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
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

## <a name="sfctl-service-deployed-type"></a>sfctl service geïmplementeerd type
Hiermee haalt u de informatie over een opgegeven service-type van de toepassing geïmplementeerd op een knooppunt in een Service Fabric-cluster.

Hiermee haalt de lijst op met de informatie over het type van een specifieke service van de toepassingen die zijn geïmplementeerd op een knooppunt in een Service Fabric-cluster. Het antwoord bevat de naam van het type van de service, de registratiestatus, het codepakket die het en -activering geregistreerd-ID van het servicepakket. Elk item vertegenwoordigt een activering van een servicetype, van elkaar onderscheiden door de id van de activering.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| ---type-servicenaam [vereist] | Hiermee geeft u de naam van een Service Fabric-service-type. |
| --naam van een service manifest | De naam van het servicemanifest voor het filteren van de lijst met geïmplementeerde service type-informatie. Indien opgegeven, is het antwoord bevat alleen de informatie over servicetypen die zijn gedefinieerd in deze servicemanifest. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-deployed-type-list"></a>sfctl service geïmplementeerd-type-lijst
Hiermee wordt de lijst met de informatie over de servicetypen van de toepassingen die zijn geïmplementeerd op een knooppunt in een Service Fabric-cluster opgehaald.

Hiermee wordt de lijst met de informatie over de servicetypen van de toepassingen die zijn geïmplementeerd op een knooppunt in een Service Fabric-cluster opgehaald. Het antwoord bevat de naam van het type van de service, de registratiestatus, het codepakket die het en -activering geregistreerd-ID van het servicepakket.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --naam van een service manifest | De naam van het servicemanifest voor het filteren van de lijst met geïmplementeerde service type-informatie. Indien opgegeven, is het antwoord bevat alleen de informatie over servicetypen die zijn gedefinieerd in deze servicemanifest. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-description"></a>Beschrijving van de service sfctl
Hiermee haalt u de beschrijving van een bestaande Service Fabric-service.

Hiermee haalt u de beschrijving van een bestaande Service Fabric-service. Een service moet worden gemaakt voordat de beschrijving kan worden verkregen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-get-container-logs"></a>sfctl-service. get-container-Logboeken
Hiermee haalt u de logboeken van de container voor de container geïmplementeerd op een Service Fabric-knooppunt.

Hiermee haalt u de logboeken van de container voor de container geïmplementeerd op een Service Fabric-knooppunt voor de opgegeven codepakket.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --code pakketnaam van [vereist] | De naam van codepakket is opgegeven in servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --manifest-servicenaam [vereist] | De naam van een servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --vorige | Geeft aan of de container-logboeken ophalen uit afgesloten/onbestelbare containers van het pakket code exemplaar. |
| --staart | Aantal regels om weer te geven vanaf het einde van de logboeken. Standaardwaarde is 100. 'alle' wilt weergeven, de volledige Logboeken. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-health"></a>sfctl servicestatus
Hiermee wordt de status van de opgegeven Service Fabric-service opgehaald.

Hiermee haalt u de statusgegevens van de opgegeven service. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen voor de service op basis van de status is gerapporteerd. Gebruik PartitionsHealthStateFilter voor het filteren van de verzameling van partities geretourneerd. Als u een service die niet in het archief health bestaat opgeeft, wordt door deze aanvraag een fout geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --uitsluiten-health-statistieken | Hiermee wordt aangegeven of de statistieken van de status moet worden geretourneerd als onderdeel van het queryresultaat. Standaard False. De statistieken tonen het aantal onderliggende items entiteiten in de status Ok, waarschuwing en fout. |
| --partities-health-status-filter | U kunt in het resultaat van de zoekopdracht van de health-service op basis van hun status voor het filteren van de partities health status objecten geretourneerd. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de partities die overeenkomen met het filter worden geretourneerd. Alle partities worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 wordt vervolgens de status van de partities met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-info"></a>sfctl servicegegevens
Hiermee haalt u de informatie over de specifieke service die horen bij de Service Fabric-toepassing.

Retourneert de informatie over de opgegeven service die horen bij de opgegeven Service Fabric-toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-list"></a>lijst van de service sfctl
Hiermee haalt u de informatie over alle services die horen bij de toepassing die is opgegeven door de toepassings-ID.

Retourneert de informatie over alle services die horen bij de toepassing die is opgegeven door de toepassings-ID.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --vervolgtoken | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd. |
| --service typenaam | De naam van de service type is gebruikt voor het filteren van de services zoeken naar. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-manifest"></a>sfctl servicemanifest
Hiermee haalt u de beschrijving van het servicetype van een manifest.

Hiermee haalt u de beschrijving van het servicetype van een manifest. Het antwoord bevat de servicemanifest XML als een tekenreeks.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| ----naam van het toepassingstype [vereist] | De naam van het toepassingstype. |
| ----versie van het toepassingstype [vereist] | De versie van het toepassingstype. |
| --manifest-servicenaam [vereist] | De naam van een servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-package-deploy"></a>sfctl service pakket implementeren
Downloadt pakketten die zijn gekoppeld aan de opgegeven servicemanifest aan het cachegeheugen van de installatiekopie op opgegeven knooppunt.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-type-naam [vereist] | De naam van het toepassingsmanifest voor de bijbehorende aangevraagde servicemanifest. |
| --app-type-versie [vereist] | De versie van het toepassingsmanifest voor de bijbehorende aangevraagde servicemanifest. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --manifest-servicenaam [vereist] | De naam van de servicemanifest die zijn gekoppeld aan de pakketten die worden gedownload. |
| --share-beleid | JSON gecodeerde lijst met beleidsregels delen. Elke delen beleidselement bestaat uit een 'name' en 'bereik'. De naam overeenkomt met de naam van het pakket met code, de configuratie of de gegevens die worden gedeeld. Het bereik mag 'None', 'All', 'Code', 'Config' of 'Data'. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-package-health"></a>sfctl pakket-servicestatus
Hiermee haalt de informatie over de status van een servicepakket voor een bepaalde toepassing is geïmplementeerd voor een Service Fabric-knooppunt en de toepassing.

Hiermee wordt de informatie over de status van servicepakket voor een bepaalde toepassing is geïmplementeerd op een Service Fabric-knooppunt. Gebruik EventsHealthStateFilter eventueel filteren op de verzameling van objecten van HealthEvent gemeld op het gebaseerd op de status geïmplementeerde service-pakket.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --service pakketnaam van [vereist] | De naam van het servicepakket. |
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

## <a name="sfctl-service-package-info"></a>sfctl service pakket-info
Haalt de lijst met servicepakketten die zijn geïmplementeerd op een Service Fabric-knooppunt die overeenkomt met de opgegeven naam.

Retourneert de gegevens over de servicepakketten dat is geïmplementeerd op een Service Fabric-knooppunt voor de opgegeven toepassing. Deze resultaten servicepakketten waarvan de naam overeenkomt met precies de pakketnaam service opgegeven als parameter.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --service pakketnaam van [vereist] | De naam van het servicepakket. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-package-list"></a>sfctl service pakketlijst
Haalt de lijst met servicepakketten die zijn geïmplementeerd op een Service Fabric-knooppunt.

Retourneert de gegevens over de servicepakketten dat is geïmplementeerd op een Service Fabric-knooppunt voor de opgegeven toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
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

## <a name="sfctl-service-recover"></a>sfctl service herstellen
Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van de opgegeven service dat momenteel is vastgelopen in quorumverlies.

Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van de opgegeven service dat momenteel is vastgelopen in quorumverlies. Deze bewerking mag alleen worden uitgevoerd als bekend is dat de replica's die niet beschikbaar zijn, kunnen niet worden hersteld. Onjuist gebruik van deze API kan leiden tot verlies van gegevens.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-report-health"></a>sfctl rapport-servicestatus
Verzendt een statusrapport voor de Service Fabric-service.

Status van de opgegeven Service Fabric-service rapporteert. Het rapport moet bevatten informatie over de bron van de health-rapport en de eigenschap waarop deze wordt gerapporteerd. Het rapport wordt verzonden naar een Service Fabric-gateway-Service, die worden doorgestuurd naar de health store. Het rapport kan worden geaccepteerd door de gateway, maar geweigerd door de health store na extra validatie. De health store kan bijvoorbeeld het rapport weigeren vanwege een ongeldige parameter, zoals een verouderde volgnummer. Om te zien of het rapport in de health store is toegepast, Controleer of het rapport wordt weergegeven in de health-gebeurtenissen van de service.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --health-eigenschap [vereist] | De eigenschap van de health-informatie. <br><br> Een entiteit kan statusrapporten voor de verschillende eigenschappen hebben. De eigenschap is een tekenreeks en niet een vaste opsomming de Rapportagefout flexibiliteit om de status-voorwaarde die het rapport activeert te categoriseren toe te passen. Een Rapportagefout met SourceId 'LocalWatchdog' kunt bijvoorbeeld de status van de beschikbare schijfruimte op een knooppunt, bewaken, zodat deze 'AvailableDisk'-eigenschap op dat knooppunt kunt rapporteren. De dezelfde Rapportagefout kan de connectiviteit knooppunt bewaken, zodat dit een 'Verbinding'-eigenschap op hetzelfde knooppunt kunt rapporteren. Deze rapporten worden in het archief health behandeld als afzonderlijke health-gebeurtenissen voor het opgegeven knooppunt. Samen met de bron-id identificatie de eigenschap een unieke voor de health-informatie. |
| --status [vereist] | Mogelijke waarden zijn\: 'Ongeldig', 'Ok', 'Waarschuwing', 'Fout', 'Onbekend'. |
| --[vereist]-service-id | De identiteit van de service. <br><br> Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Bijvoorbeeld, als de servicenaam is ' fabric\:/myapp/app1/svc1', de service-identiteit zijn ' myapp\~app1\~svc1' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --bron-id [vereist] | De naam van de bron waarmee de component watchdog-client-systeem dat de statusgegevens genereert. |
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

## <a name="sfctl-service-resolve"></a>sfctl service oplossen
Een Service Fabric-partitie worden omgezet.

Een partitie van de Service Fabric-service om op te halen van de eindpunten van de service-replica's worden omgezet.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id | De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de servicenaam is bijvoorbeeld "fabric\:/myapp/app1/svc1 ', de service-identiteit is ' myapp\~app1\~svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --partitie sleuteltype | Het sleuteltype voor de partitie. Deze parameter is vereist als het partitieschema voor de service Int64Range of Named zijn. De mogelijke waarden zijn volgt. -Geen (1) - geeft aan dat de parameter PartitionKeyValue niet is opgegeven. Dit is geldig voor partities met partitieschema als Singleton. Dit is de standaardwaarde. De waarde is 1. -Int64Range (2) - geeft aan dat de parameter PartitionKeyValue een partitiesleutel int64. Dit is geldig voor partities met partitieschema als Int64Range. De waarde is 2. -Namen (3) - geeft aan dat de parameter PartitionKeyValue een naam van de partitie. Dit is geldig voor partities met partitieschema als naam. De waarde is 3. |
| --partitie sleutelwaarde | De partitiesleutel. Dit is vereist als het partitieschema voor de service Int64Range of Named zijn is. |
| --vorige gegevensbron-versie | De waarde in het veld versie van het antwoord dat u eerder hebt ontvangen. Dit is vereist als de gebruiker weet dat het resultaat is gegaan eerder verlopen is. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-type-list"></a>sfctl-lijst van service
Hiermee haalt de lijst op met de informatie over servicetypen die worden ondersteund door een ingerichte toepassingstype in een Service Fabric-cluster.

Hiermee haalt de lijst op met de informatie over servicetypen die worden ondersteund door een ingerichte toepassingstype in een Service Fabric-cluster. Het opgegeven type moet bestaan. Anders wordt de status 404 geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| ----naam van het toepassingstype [vereist] | De naam van het toepassingstype. |
| ----versie van het toepassingstype [vereist] | De versie van het toepassingstype. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-service-update"></a>sfctl service-update
De opgegeven service met behulp van de updatebeschrijving van de opgegeven updates.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id | De ID van de service. Deze ID is meestal de volledige naam van de service zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Bijvoorbeeld, als de servicenaam is ' fabric\:/myapp/app1/svc1', de service-identiteit zijn ' myapp\~app1\~svc1' in 6.0 + en ' myapp/app1/svc1' in eerdere versies. |
| --beperkingen | De plaatsingsbeperkingen als een tekenreeks. Plaatsingsbeperkingen zijn boolean-expressies in de eigenschappen van het knooppunt en toestaan voor een service beperken tot bepaalde knooppunten op basis van de servicevereisten. Bijvoorbeeld, om een service op de knooppunten waar NodeType blauw is het volgende opgeven\: ' NodeColor == blue '. |
| --gecorreleerde-service | Naam van de doelservice correleren met. |
| --correlatie | De service met een bestaande service met behulp van een affiniteit uitlijning correleren. |
| --aantal exemplaren | Het aantal exemplaren. Dit geldt voor alleen stateless services. |
| --load-metrische gegevens | Lijst met JSON gecodeerde van metrische gegevens gebruikt wanneer taakverdeling over knooppunten. |
| --min-replica-set-grootte | De replica van de minimale instellen grootte als een getal. Deze grootte is van toepassing op alleen stateful services. |
| --verplaatsen kosten | Hiermee geeft u de move-kosten voor de service. Mogelijke waarden zijn\: 'Nul', 'Laag', 'Gemiddeld', 'Hoog'. |
| --plaatsing beleidslijst | JSON gecodeerde lijst met beleidsregels voor plaatsing voor de service, en alle gekoppelde domeinnamen. Beleidsregels kunnen worden een of meer van\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`. |
| --quorum-verlies-wait | De maximale duur in seconden, waarvoor een partitie is toegestaan in een status van quorumverlies. Deze duur geldt voor stateful services alleen. |
| --replica-opnieuw opstarten-wait | De duur in seconden tussen wanneer een replica uitvalt en wanneer een nieuwe replica wordt gemaakt. Deze duur geldt voor stateful services alleen. |
| --schalen beleid | JSON gecodeerde lijst met beleidsregels voor deze service te schalen. |
| --stand door replica behouden | De maximale duur in seconden, voor welke stand-by replica's behouden blijft voordat het wordt verwijderd. Deze duur geldt voor stateful services alleen. |
| --stateful | Geeft dat de doelservice is een stateful service. |
| --staatloze | Geeft dat de doelservice is een staatloze service. |
| ---replica-set-doelgrootte | De doelreplica instellen grootte als een getal. Deze grootte is van toepassing op alleen stateful services. |
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
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).
