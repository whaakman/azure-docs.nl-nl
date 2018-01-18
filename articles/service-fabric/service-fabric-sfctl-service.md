---
title: Azure Service Fabric CLI - sfctl service | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl service-opdrachten.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: 5c1f485812918397b5b52e650611032c9058e3ee
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-service"></a>sfctl-service
Maken, verwijderen en beheren van de service, servicetypen en servicepakketten.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
|    App-naam       | Hiermee haalt u de naam van de Service Fabric-toepassing voor een service.|
|    code-package-list | Haalt de lijst met code-pakketten die zijn geïmplementeerd op een Service Fabric-knooppunt.|
|    maken         | De opgegeven Service Fabric-service maakt van de beschrijving.|
|    verwijderen         | Hiermee verwijdert u een bestaande Service Fabric-service.|
|    geïmplementeerd type  | Hiermee haalt u de informatie over een opgegeven service-type van de toepassing geïmplementeerd op een knooppunt in een Service Fabric-cluster.|
|    geïmplementeerd type lijst| Hiermee wordt de lijst met de informatie over de servicetypen van de toepassingen die zijn geïmplementeerd op een knooppunt in een Service Fabric-cluster opgehaald.|
|    description    | Hiermee haalt u de beschrijving van een bestaande Service Fabric-service.|
|    status         | Hiermee wordt de status van de opgegeven Service Fabric-service opgehaald.|
|    info           | Hiermee haalt u de informatie over de specifieke service die horen bij een Service Fabric-toepassing.|
|    lijst           | Hiermee haalt u de informatie over alle services die horen bij de toepassing die is opgegeven door de toepassings-ID.|
|    manifest       | Hiermee haalt u de beschrijving van het servicetype van een manifest.|
|    package-deploy | Downloadt pakketten die zijn gekoppeld aan de opgegeven servicemanifest aan het cachegeheugen van de installatiekopie op opgegeven knooppunt.|
|    package-health | Hiermee haalt de informatie over de status van een servicepakket voor een bepaalde toepassing is geïmplementeerd voor een Service Fabric-knooppunt en de toepassing.|
|    package-info   | Haalt de lijst met servicepakketten die zijn geïmplementeerd op een Service Fabric-knooppunt die overeenkomt met de opgegeven naam.|
|    package-list   | Haalt de lijst met servicepakketten die zijn geïmplementeerd op een Service Fabric-knooppunt.|
|    herstellen        | Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van de opgegeven service, die momenteel is vastgelopen in quorumverlies.|
|    rapport-status  | Verzendt een statusrapport voor de Service Fabric-service.|
|    oplossen        | Een Service Fabric-partitie worden omgezet.|
|    type-list      | Hiermee haalt de lijst op met de informatie over servicetypen die worden ondersteund door een ingerichte toepassingstype in een Service Fabric-cluster.|
|    bijwerken         | De opgegeven service met behulp van de updatebeschrijving van de opgegeven updates.|


## <a name="sfctl-service-create"></a>sfctl service maken
De opgegeven Service Fabric-service maakt van de beschrijving.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-id [vereist]| De identiteit van de bovenliggende toepassing. Dit wordt meestal de volledige-ID van de toepassing zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~' teken. Bijvoorbeeld, als de toepassingsnaam van de is 'fabric://myapp/app1', de toepassings-id zou zijn ' myapp ~ app1' in 6.0 + en ' myapp/app1' in eerdere versies.|
| --de naam [vereist]| De naam van de service. Dit moet een onderliggend element van de toepassings-ID.           Dit is de volledige naam waaronder de `fabric:` URI. Bijvoorbeeld service `fabric:/A/B` onderliggend element is van toepassing `fabric:/A`.|
| --servicetype [vereist]| Naam van het type van de service.|
| --activering-modus     | De activering modus voor het servicepakket.|
| --beperkingen         | De plaatsingsbeperkingen als een tekenreeks. Plaatsingsbeperkingen zijn boolean-expressies in de eigenschappen van het knooppunt en toestaan voor een service beperken tot bepaalde knooppunten op basis van de servicevereisten. Bijvoorbeeld, om een service op de knooppunten waar NodeType blauw is het volgende opgeven: ' NodeColor == blue '.|
| --gecorreleerde-service  | Naam van de doelservice correleren met.|
| --correlatie         | De service met een bestaande service met behulp van een affiniteit uitlijning correleren.|
| --dns-naam            | De DNS-naam van de service moet worden gemaakt. De Service Fabric-DNS-service moet zijn ingeschakeld voor deze instelling.|
| --aantal exemplaren      | Het aantal exemplaren. Dit geldt voor alleen stateless services.|
| --int-schema          | Hiermee geeft u dat de service moet op uniforme wijze worden gepartitioneerd in een bereik van niet-ondertekende gehele getallen.|
| --int-scheme-count    | Het aantal partities binnen het gehele getal key (voor een partitieschema uniform geheel getal) te maken.|
| --int-scheme-high     | Het einde van het bereik van sleutel geheel getal, als u een partitieschema uniform geheel getal.|
| --int-scheme-low      | Het begin van het bereik van sleutel geheel getal, als u een partitieschema uniform geheel getal.|
| --load-metrische gegevens        | JSON gecodeerde lijst met metrische gegevens die gebruikt wordt wanneer services taakverdeling over knooppunten.|
| --min-replica-set-grootte| De replica van de minimale instellen grootte als een getal. Dit geldt voor stateful services alleen.|
| --verplaatsen kosten           | Hiermee geeft u de move-kosten voor de service. Mogelijke waarden zijn: 'Nul', 'Laag', 'Gemiddeld', 'Hoog'.|
| --named-scheme        | Hiermee geeft u dat de service moet meerdere benoemde partities hebben.|
| --met de naam-schema-lijst   | JSON gecodeerde lijst met namen voor het partitioneren van de service in, als het benoemde partitieschema gebruikt.|
| --niet-persistent-status  | Indien waar, wordt hiermee de service heeft geen permanente status opgeslagen op de lokale schijf of alleen wordt status opgeslagen in het geheugen.|
| --plaatsing beleidslijst  | JSON gecodeerde lijst met beleidsregels voor plaatsing voor de service, en alle gekoppelde domeinnamen. Beleidsregels kunnen worden een of meer van: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`.|
| --quorum-loss-wait    | De maximale duur in seconden, waarvoor een partitie is toegestaan in een status van quorumverlies. Dit geldt voor stateful services alleen.|
| --replica-restart-wait| De duur in seconden tussen wanneer een replica uitvalt en wanneer een nieuwe replica wordt gemaakt. Dit geldt voor stateful services alleen.|
| --singleton-schema    | Hiermee geeft de service moet één partitie hebben of een niet - gepartitioneerde service moet zijn.|
| --stand door replica behouden  | De maximale duur in seconden, voor welke stand-by worden replica's bewaard voordat het wordt verwijderd. Dit geldt voor stateful services alleen.|
| --stateful            | Geeft aan dat de service is een stateful service.|
| --staatloze           | Geeft aan dat de service is een staatloze service.|
| --target-replica-set-size| De doelreplica instellen grootte als een getal. Dit geldt voor stateful services alleen.|
| --time-out -t          | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h             | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query               | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-service-delete"></a>sfctl service verwijderen
Hiermee verwijdert u een bestaande Service Fabric-service.

Hiermee verwijdert u een bestaande Service Fabric-service. Een service moet worden gemaakt voordat het kan worden verwijderd. Standaard probeert de Service Fabric service replica's op correcte wijze sluiten en verwijder vervolgens de service. Echter als service heeft problemen met de replica probleemloos sluit, de delete-bewerking kan lang duren of hangen. Gebruik het optionele ForceRemove vlag de correcte sluiten van een reeks overslaan en de service geforceerd verwijderen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id| De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Bijvoorbeeld, als de servicenaam is fabric://myapp/app1/svc1 ', de service-identiteit zijn ' myapp ~ app1 ~ svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies.|
| --force-remove      | Verwijder een Service Fabric-toepassing of service geforceerd zonder tussenkomst van de reeks correct afsluiten. Deze parameter kan worden gebruikt geforceerd verwijderen van een toepassing of service voor welke verwijderen krijgt een time-out vanwege problemen met de code die voorkomt dat correcte sluit van replica's.|
| --time-out -t        | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug             | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h           | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o         | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query             | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide           | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-service-description"></a>Beschrijving van de service sfctl
Hiermee haalt u de beschrijving van een bestaande Service Fabric-service.

Hiermee haalt u de beschrijving van een bestaande Service Fabric-service. Een service moet worden gemaakt voordat de beschrijving kan worden verkregen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id| De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Bijvoorbeeld, als de servicenaam is 'fabric://myapp/app1/svc1', de service-identiteit zou zijn ' myapp ~ app1 ~ svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies.|
| --time-out -t        | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug             | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h           | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o         | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query             | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide           | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-service-health"></a>sfctl servicestatus
Hiermee wordt de status van de opgegeven Service Fabric-service opgehaald.

Hiermee haalt u de statusgegevens van de opgegeven service. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen voor de service op basis van de status is gerapporteerd. Gebruik PartitionsHealthStateFilter voor het filteren van de verzameling van partities geretourneerd. Als u een service die niet in het archief health bestaat opgeeft, retourneert deze cmdlet een fout opgetreden. .

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id| De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Bijvoorbeeld, als de servicenaam is 'fabric://myapp/app1/svc1', de service-identiteit zou zijn ' myapp ~ app1 ~ svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies.|
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535.|
|--uitsluiten-health-statistieken     | Hiermee wordt aangegeven of de statistieken van de status moet worden geretourneerd als onderdeel van het queryresultaat. Standaard False. De statistieken tonen het aantal onderliggende items entiteiten in de status Ok, waarschuwing en fout.|
| --partities-health-status-filter| U kunt in het resultaat van de zoekopdracht van de health-service op basis van hun status voor het filteren van de partities health status objecten geretourneerd. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de partities die overeenkomen met het filter worden geretourneerd. Alle partities worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is '6' wordt de status van de partities met HealthState waarde OK (2) en de waarschuwing (4) zijn geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState.                  De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535.|
| --time-out -t                 | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                      | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                    | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                  | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.                  Standaard: json.|
| --query                      | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide                    | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-service-info"></a>sfctl servicegegevens
Hiermee haalt u de informatie over de specifieke service die horen bij een Service Fabric-toepassing.

Retourneert de informatie over de opgegeven service die horen bij de opgegeven Service Fabric-toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist]| De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Bijvoorbeeld, als de toepassingsnaam van de is 'fabric://myapp/app1', de toepassings-id zou zijn ' myapp ~ app1 ' in 6.0 + en "myapp/app1" in eerdere versies.|
| --[vereist]-service-id| De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Bijvoorbeeld, als de servicenaam is 'fabric://myapp/app1/svc1', de service-identiteit zou zijn ' myapp ~ app1 ~ svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies.|
| --time-out -t            | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                 | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h               | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o             | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query                 | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide               | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-service-list"></a>lijst van de service sfctl
Hiermee haalt u de informatie over alle services die horen bij de toepassing die is opgegeven door de toepassings-ID.

Retourneert de informatie over alle services die horen bij de toepassing die is opgegeven door de toepassings-ID.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist]| De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Bijvoorbeeld, als de toepassingsnaam van de is 'fabric://myapp/app1', de toepassings-id zou zijn ' myapp ~ app1 ' in 6.0 + en "myapp/app1" in eerdere versies.|
| --vervolgtoken    | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd.|
| --service-type-name     | De naam van de service type is gebruikt voor het filteren van de services zoeken naar.|
| --time-out -t            | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                 | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h               | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o             | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query                 | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide               | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-service-manifest"></a>sfctl servicemanifest
Hiermee haalt u de beschrijving van het servicetype van een manifest.

Hiermee haalt u de beschrijving van het servicetype van een manifest. Het antwoord bevat de servicemanifest XML als een tekenreeks.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| ----naam van het toepassingstype [vereist]| De naam van het toepassingstype.|
| ----versie van het toepassingstype [vereist]| De versie van het toepassingstype.|
| --manifest-servicenaam [vereist]| De naam van een servicemanifest geregistreerd als onderdeel van een toepassingstype in een Service Fabric-cluster.|
| --time-out -t                      | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                           | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                         | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                       | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.                       Standaard: json.|
| --query                           | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide                         | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-service-recover"></a>sfctl service herstellen
Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van de opgegeven service, die momenteel is vastgelopen in quorumverlies.

Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van de opgegeven service, die momenteel is vastgelopen in quorumverlies. Deze bewerking mag alleen worden uitgevoerd als de replica's die niet beschikbaar zijn, kunnen niet worden hersteld. Onjuist gebruik van deze API kan leiden tot verlies van gegevens.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id| De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Bijvoorbeeld, als de servicenaam is fabric://myapp/app1/svc1 ', de service-identiteit zijn ' myapp ~ app1 ~ svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies.|
| --time-out -t        | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug             | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h           | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o         | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query             | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide           | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-service-resolve"></a>sfctl service oplossen
Een Service Fabric-partitie worden omgezet.

Een partitie van de Service Fabric-service om op te halen van de eindpunten van de service-replica's worden omgezet.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id| De identiteit van de service. Dit wordt meestal de volledige naam van de service zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Bijvoorbeeld, als de servicenaam is 'fabric://myapp/app1/svc1', de service-identiteit zou zijn ' myapp ~ app1 ~ svc1 ' in 6.0 + en ' myapp/app1/svc1' in eerdere versies.|
| --partitie sleuteltype| Het sleuteltype voor de partitie. Deze parameter is vereist als het partitieschema voor de service Int64Range of Named zijn. De mogelijke waarden zijn volgt. -Geen (1) - geeft aan dat de parameter PartitionKeyValue niet is opgegeven. Dit is geldig voor partities met partitieschema als Singleton. Dit is de standaardwaarde. De waarde is 1. -Int64Range (2) - geeft aan dat de parameter PartitionKeyValue een partitiesleutel int64. Dit is geldig voor partities met partitieschema als Int64Range. De waarde is 2. -Namen (3) - geeft aan dat de parameter PartitionKeyValue een naam van de partitie. Dit is geldig voor partities met partitieschema als naam. De waarde is 3.|
| --partitie sleutelwaarde  | De partitiesleutel. Dit is vereist als het partitieschema voor de service Int64Range of Named zijn is.|
| --vorige gegevensbron-versie | De waarde in het veld versie van het antwoord dat u eerder hebt ontvangen. Dit is vereist als de gebruiker weet dat het resultaat die is verkregen eerder verlopen is.|
| --time-out -t        | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug             | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h           | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o         | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query             | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide           | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-service-update"></a>sfctl service-update
De opgegeven service met behulp van de updatebeschrijving van de opgegeven updates.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --[vereist]-service-id| De doelservice om bij te werken. Dit wordt meestal de volledige-ID van de service zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Bijvoorbeeld, als de servicenaam is 'fabric://myapp/app1/svc1', de service-identiteit zou zijn ' myapp ~ app1 ~ svc1' in 6.0 + en ' myapp/app1/svc1' in eerdere versies.|
| --beperkingen         | De plaatsingsbeperkingen als een tekenreeks. Plaatsingsbeperkingen zijn boolean-expressies in de eigenschappen van het knooppunt en toestaan voor een service beperken tot bepaalde knooppunten op basis van de servicevereisten. Bijvoorbeeld, om een service op de knooppunten waar NodeType blauw is het volgende opgeven: ' NodeColor == blue '.|
| --gecorreleerde-service  | Naam van de doelservice correleren met.|
| --correlatie         | De service met een bestaande service met behulp van een affiniteit uitlijning correleren.|
| --aantal exemplaren      | Het aantal exemplaren. Dit geldt voor alleen stateless services.|
| --load-metrische gegevens        | Lijst met JSON gecodeerde van metrische gegevens gebruikt wanneer taakverdeling over knooppunten.|
| --min-replica-set-grootte| De replica van de minimale instellen grootte als een getal. Dit geldt voor stateful services alleen.|
| --verplaatsen kosten           | Hiermee geeft u de move-kosten voor de service. Mogelijke waarden zijn: 'Nul', 'Laag', 'Gemiddeld', 'Hoog'.|
| --plaatsing beleidslijst  | JSON gecodeerde lijst met beleidsregels voor plaatsing voor de service, en alle gekoppelde domeinnamen. Beleidsregels kunnen worden een of meer van: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`.|
| --quorum-loss-wait    | De maximale duur in seconden, waarvoor een partitie is toegestaan in een status van quorumverlies. Dit geldt voor stateful services alleen.|
| --replica-restart-wait| De duur in seconden tussen wanneer een replica uitvalt en wanneer een nieuwe replica wordt gemaakt. Dit geldt voor stateful services alleen.|
| --stand door replica behouden  | De maximale duur in seconden, voor welke stand-by worden replica's bewaard voordat het wordt verwijderd. Dit geldt voor stateful services alleen.|
| --stateful            | Geeft dat de doelservice is een stateful service.|
| --staatloze           | Geeft dat de doelservice is een staatloze service.|
| --target-replica-set-size| De doelreplica instellen grootte als een getal. Dit geldt voor stateful services alleen.|
| --time-out -t          | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h             | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query               | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="next-steps"></a>Volgende stappen
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).