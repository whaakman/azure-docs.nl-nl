---
title: Azure Service Fabric CLI - sfctl cluster | Microsoft Docs
description: Beschrijving van de Service Fabric CLI-opdrachten voor het cluster van sfctl.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 09/22/2017
ms.author: ryanwi
ms.openlocfilehash: dc2ed59d6adaca97b23dddcb7ec968d90171b483
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-cluster"></a>sfctl cluster
Selecteer, beheren en gebruiken van Service Fabric-clusters.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
|    code-versies| Haalt een lijst van fabric code-versies die zijn ingericht op een Service Fabric-cluster.|
|    configuratie-versies | Hiermee haalt een lijst van fabric config-versies die zijn ingericht op een Service Fabric-cluster.|
|    Status       | Hiermee haalt u de status van een Service Fabric-cluster.|
|    Manifest     | Haal het manifest Service Fabric-cluster.|
|    bewerking annuleren| Een gebruiker veroorzaakte fout geannuleerd.|
|    operationgit | Hiermee haalt u een lijst van fouttolerantie gebruiker veroorzaakte bewerkingen die zijn gefilterd op de opgegeven invoer.|
|    Inrichten     | De pakketten code of configuratie van een Service Fabric-cluster inrichten.|
|    herstellen-systeem  | Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van de systeemservices die momenteel zijn vastgelopen in quorumverlies.|
|rapport-status   | Verzendt een statusrapport op het Service Fabric-cluster.|
|    Selecteer       | Maakt verbinding met een Service Fabric-cluster-eindpunt.|
| Inrichting verwijderen     | Inrichting van de pakketten code of configuratie van een Service Fabric-cluster.|
|    upgrade         | Start de code of upgrade van een Service Fabric-cluster.|
|    upgrade hervatten  | Controleer het upgraden van cluster gaat u verder met het volgende upgradedomein.|
|    upgrade terugdraaien| De upgrade van een Service Fabric-cluster terugdraaien.|
|    upgrade-status  | Hiermee haalt u de voortgang van de huidige upgrade van de cluster.|
|upgrade-update  | Bijwerken van de parameters voor het bijwerken van de upgrade van een Service Fabric-cluster.|


## <a name="sfctl-cluster-health"></a>sfctl cluster health
Hiermee haalt u de status van een Service Fabric-cluster.

Hiermee haalt u de status van een Service Fabric-cluster. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen op het cluster op basis van de status is gerapporteerd. NodesHealthStateFilter en ApplicationsHealthStateFilter op dezelfde manier gebruiken voor het filteren van de verzameling van knooppunten en toepassingen die worden geretourneerd op basis van hun geaggregeerde status. 

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassingen-health-status-filter| Hiermee kunt u filteren van de status van de toepassing
                                                    objects returned in the result of cluster health
                                                    query based on their health state. The possible
                                                    values for this parameter include integer value
                                                    obtained from members or bitwise operations on
                                                    members of HealthStateFilter enumeration. Only
                                                    applications that match the filter are returned.
                                                    All applications are used to evaluate the
                                                    aggregated health state. If not specified, all
                                                    entries are returned. The state values are flag
                                                    based enumeration, so the value could be a
                                                    combination of these values obtained using
                                                    bitwise 'OR' operator. For example, if the
                                                    provided value is 6 then health state of
                                                    applications with HealthState value of OK (2)
                                                    and Warning (4) are returned. - Default -
                                                    Default value. Matches any HealthState. The
                                                    value is zero. - None - Filter that doesn't
                                                    match any HealthState value. Used in order to
                                                    return no results on a given collection of
                                                    states. The value is 1. - Ok - Filter that
                                                    matches input with HealthState value Ok. The
                                                    value is 2. - Warning - Filter that matches
                                                    input with HealthState value Warning. The value
                                                    is 4. - Error - Filter that matches input with
                                                    HealthState value Error. The value is 8. - All -
                                                    Filter that matches input with any HealthState value. The value is 65535.|
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag op basis van opsomming, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator zijn. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde.
De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. | |--uitsluiten-health-statistieken | Hiermee wordt aangegeven of de statistieken van de status moet worden geretourneerd als onderdeel van het queryresultaat. Standaard False. De statistieken zien hoeveel kinderen entiteiten in de status Ok, waarschuwing en fout. | |   --opnemen-systeem-toepassing-health-statistieken | Hiermee wordt aangegeven of de gezondheidsstatistieken van de fabric moet bevatten: / System application health statistieken. Standaard False. Als IncludeSystemApplicationHealthStatistics is ingesteld op true, wordt de status statistieken omvatten de entiteiten die deel uitmaken van de fabric: / System-toepassing. Anders wordt het queryresultaat health statistische gegevens bevat, alleen voor toepassingen. De health-statistieken worden opgenomen in het queryresultaat voor deze parameter moet worden toegepast. | | --knooppunten-health-status-filter | Hiermee kunt u filteren van het knooppunt health status objecten geretourneerd in het resultaat van het cluster health query op basis van hun status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen knooppunten die overeenkomen met het filter worden geretourneerd. Alle knooppunten worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is '6' wordt de status van knooppunten met HealthState waarde OK (2) en de waarschuwing (4) zijn geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde.
De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. | | --time-out -t | Server time-out in seconden.  Standaard: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --fouten opsporen                        | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                      | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                    | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.                    Standaard: json.|
| --query                        | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide                      | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-cluster-manifest"></a>sfctl clustermanifest
Haal het manifest Service Fabric-cluster.

Haal het manifest Service Fabric-cluster. Het clustermanifest bevat eigenschappen van het cluster met verschillende knooppunttypen op het cluster, beveiligingsconfiguraties, fouten en upgradedomein topologieën, enzovoort. Deze eigenschappen zijn opgegeven als deel van het bestand ClusterConfig.JSON tijdens de implementatie van een zelfstandige cluster. Echter, de meeste van de informatie in het clustermanifest wordt gegenereerd intern door service fabric tijdens de implementatie van het cluster in andere implementatiescenario's (bijvoorbeeld bij het gebruik van de [Azure-portal](https://portal.azure.com)). De inhoud van het clustermanifest zijn alleen ter informatie en gebruikers wordt niet verwacht dat de indeling van de inhoud van het bestand of de interpretatie van een afhankelijkheid ondernemen. 

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --time-out -t| Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --fouten opsporen  | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h| Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query  | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide| Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-cluster-provision"></a>sfctl cluster inrichten
De pakketten code of configuratie van een Service Fabric-cluster inrichten.

        Validate and provision the code or configuration packages of a Service Fabric cluster.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
|--cluster-manifest-bestandspad| Het pad van het manifestbestand voor het cluster.|
|    --code bestandspad            | Het cluster code pakket bestandspad.|
|    --time-out -t                | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --fouten opsporen | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h  | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o| De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide  | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-cluster-select"></a>sfctl cluster selecteren
Maakt verbinding met een Service Fabric-cluster-eindpunt.

Als verbinding maken met veilige cluster, geeft u een certificaat (.crt) en sleutelbestand (.key) of één bestand met beide (.pem). Beide niet opgeven. Optioneel als u verbinding maakt met een beveiligde cluster, ook een pad opgeven naar een CA bundle-bestand of map van het vertrouwde CA-certificaten klant.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --eindpunt [vereist]| Eindpunt-URL, met inbegrip van de poort en het voorvoegsel HTTP of HTTPS-cluster.|
| --aad             | Azure Active Directory voor verificatie gebruiken.|
| --ca              | Pad naar de directory van de CA-certificaten klant op dezelfde manier behandelen als geldig of CA bundle-bestand.|
| --cert            | Pad naar een certificaatbestand van de client.|
| --sleutel             | Pad naar de certificaat-sleutelbestand.|
| --Er is geen controleren       | Controle voor certificaten uitschakelen bij gebruik van HTTPS, Let op: dit is een onbeveiligde optie en mag niet worden gebruikt voor productieomgevingen.|
| --pem             | Pad naar het clientcertificaat .pem-bestand.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --fouten opsporen           | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h         | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o       | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query           | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide         | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-cluster-unprovision"></a>sfctl cluster verwijderen
Inrichting van de pakketten code of configuratie van een Service Fabric-cluster.

        Unprovision the code or configuration packages of a Service Fabric cluster.

### <a name="arguments"></a>Argumenten
|Argument|Beschrijving|
| --- | --- |
|--code-versie  | De code van pakketversie van de cluster.|
|    --config-versie| De versie van het clustermanifest.|
|    --time-out -t    | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten
|Argument|Beschrijving|
| --- | --- |
|--fouten opsporen         | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
 |   --help -h       | Deze help-bericht en afsluiten weergeven.|
 |   --uitvoer -o     | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
 |   --query         | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie en
                      voorbeelden.|
 |   --uitgebreide       | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|


## <a name="sfctl-cluster-upgrade"></a>clusterupgrades sfctl
Start de code of upgrade van een Service Fabric-cluster.

        Validate the supplied upgrade parameters and start upgrading the code or configuration
        version of a Service Fabric cluster if the parameters are valid.

### <a name="arguments"></a>Argumenten
|Argument|Beschrijving|
| --- | --- |
|    --app-health-kaart                      | JSON gecodeerde woordenlijst van paren toepassingsnaam en
                                            maximumpercentage slecht voordat de fout.|
 |   --app-type-health-kaart                 | JSON gecodeerde woordenlijst van het toepassingstype paren
                                            name and maximum percentage unhealthy before raising
                                            error.|
 |   --code-versie | De versie van de cluster. | |   --config-versie | De configuratieversie van de cluster. | |   --delta-health-evaluatie | Hiermee delta de statusevaluatie in plaats van absolute health evaluatie na voltooiing van elk upgradedomein. | |   --delta slecht knooppunten | Het maximaal toegestane percentage van de knooppunten verslechtering van de status toegestaan tijdens upgrades van de cluster.  Standaard: 10.
De verschillen wordt tussen de status van de knooppunten aan het begin van de upgrade en de status van de knooppunten op het moment van de statusevaluatie gemeten. De controle wordt uitgevoerd na de upgrade voltooiing van elke upgradedomein om te controleren of de globale status van het cluster is binnen de limieten voor verdragen. | |   --actie bij fout | Mogelijke waarden zijn: 'Ongeldig', 'Terugdraaiactie', 'Manual'. | |   --opnieuw opstarten als force | Forceert herstart. | |   --health-controle-retry | Health selectievakje opnieuw time-out gemeten in milliseconden. | |   --health-controle-stabiel | Statuscontrole stabiele duur in milliseconden gemeten. | |  --wachtduur | Duur gemeten in milliseconden voor de statuscontrole van de wachttijd. | |  --replica-set-controle-timeout | Upgrade replicaset selectievakje time-out gemeten in seconden. | |   --rolling-upgrade-modus | Mogelijke waarden zijn: 'Ongeldig', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitored'.  Standaardwaarde: UnmonitoredAuto. | |  --time-out -t | Server time-out in seconden.  Standaard: 60. | |  --slecht toepassingen | Het maximaal toegestane percentage van de beschadigde toepassingen voordat u een foutmelding.
Bijvoorbeeld, zodat 10% van toepassingen beschadigd is deze waarde 10. Het percentage vertegenwoordigt het verdragen maximumpercentage van toepassingen die beschadigd worden kunnen voordat het cluster wordt beschouwd als fout. Als het percentage is voldaan, maar er ten minste één slecht toepassingstype is, worden de status wordt geëvalueerd als de waarschuwing. Dit wordt berekend door het aantal beschadigde toepassingen delen via het totale aantal exemplaren van een toepassing in het cluster, met uitzondering van toepassingen van toepassingstypen die in de applicationtypehealthpolicymap; deze zijn opgenomen. De berekening rondt af naar boven op een storing op een klein aantal toepassingen tolereren. | |   --slecht knooppunten | Het maximaal toegestane percentage van slecht knooppunten voordat u een foutmelding.
Bijvoorbeeld, zodat 10% van de knooppunten beschadigd is deze waarde 10. Het percentage geeft de maximale verdragen percentage van de knooppunten die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage is voldaan, maar er ten minste één slecht knooppunt is, wordt de status wordt geëvalueerd als waarschuwing. Het percentage wordt berekend door het aantal beschadigde knooppunten delen via het totale aantal knooppunten in het cluster. De berekening rondt af naar boven op een storing op een klein aantal knooppunten tolereren. In grote clusters sommige knooppunten is niet actief of out-reparaties, zodat dit percentage moet worden geconfigureerd voor tolereren die. | |   --upgrade-domain-delta-slecht-knooppunten | Het maximaal toegestane percentage van de knooppunten van het upgradedomein verslechtering van de status toegestaan tijdens upgrades van de cluster.
Standaard: 15.
De verschillen wordt tussen de status van de upgradedomein-knooppunten aan het begin van de upgrade en de status van de upgradedomein-knooppunten op het moment van de statusevaluatie gemeten. De controle wordt uitgevoerd na elke opwaarderen upgradedomein is voltooid voor alle upgradedomeinen om te controleren of de status van de upgradedomeinen voltooid binnen de limieten voor verdragen is. | |   --upgrade-domain-timeout | Time-out upgradedomein gemeten in milliseconden. | |   --upgrade-timeout | Upgrade time-out gemeten in milliseconden. | |   --waarschuwing als fout | Waarschuwingen met de ernst van de dezelfde fouten worden behandeld. |

### <a name="global-arguments"></a>Algemene argumenten
    |Argument|Beschrijving|
| --- | --- |
|--fouten opsporen                               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
|    --help -h                             | Deze help-bericht en afsluiten weergeven.|
|    --uitvoer -o                           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.
                                            Standaard: json.|
|    --query                               | JMESPath queryreeks. Http://jmespath.org/ Zie voor meer informatie
                                            informatie over en voorbeelden.|
|    --uitgebreide                             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporing
                                            de logboeken.|

## <a name="next-steps"></a>Volgende stappen
- [Setup](service-fabric-cli.md) de Service Fabric CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).