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
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: 5eeff271fea67cd859dff598cae0010cf3b8e13f
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-cluster"></a>sfctl cluster
Selecteer, beheren en gebruiken van Service Fabric-clusters.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
|    code-versies| Haalt een lijst van fabric code-versies die zijn ingericht op een Service Fabric-cluster.|
|    configuratie-versies | Hiermee haalt een lijst van fabric config-versies die zijn ingericht op een Service Fabric-cluster.|
|    status       | Hiermee haalt u de status van een Service Fabric-cluster.|
|    manifest     | Haal het manifest Service Fabric-cluster.|
|    bewerking annuleren| Een gebruiker veroorzaakte fout geannuleerd.|
|    operationgit | Hiermee haalt u een lijst van fouttolerantie gebruiker veroorzaakte bewerkingen die zijn gefilterd op de opgegeven invoer.|
|    Inrichten     | De pakketten code of configuratie van een Service Fabric-cluster inrichten.|
|    herstellen-systeem  | Hiermee wordt aangegeven met het Service Fabric-cluster dat deze proberen moet te herstellen van de systeemservices die momenteel zijn vastgelopen in quorumverlies.|
|rapport-status   | Verzendt een statusrapport op het Service Fabric-cluster.|
|    selecteren       | Maakt verbinding met een Service Fabric-cluster-eindpunt.|
| Inrichting verwijderen     | Inrichting van de pakketten code of configuratie van een Service Fabric-cluster.|
|    upgrade         | Start de code of upgrade van een Service Fabric-cluster.|
|    upgrade hervatten  | Controleer het upgraden van cluster gaat u verder met het volgende upgradedomein.|
|    upgrade-rollback| De upgrade van een Service Fabric-cluster terugdraaien.|
|    upgrade-status  | Hiermee haalt u de voortgang van de huidige upgrade van de cluster.|
|upgrade-update  | Bijwerken van de parameters voor het bijwerken van de upgrade van een Service Fabric-cluster.|


## <a name="sfctl-cluster-health"></a>sfctl cluster health
Hiermee haalt u de status van een Service Fabric-cluster.

Hiermee haalt u de status van een Service Fabric-cluster. EventsHealthStateFilter gebruiken voor het filteren van de verzameling van health-gebeurtenissen op het cluster op basis van de status is gerapporteerd. NodesHealthStateFilter en ApplicationsHealthStateFilter op dezelfde manier gebruiken voor het filteren van de verzameling van knooppunten en toepassingen die worden geretourneerd op basis van hun geaggregeerde status. 

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassingen-health-status-filter| Hiermee kunt u filteren van de toepassing health status objecten geretourneerd in het resultaat van het cluster health query op basis van hun status. De mogelijke waarden voor deze parameter zijn geheel getal is verkregen van leden of een bitsgewijze bewerkingen op leden van HealthStateFilter opsomming. Alleen toepassingen die overeenkomen met het filter worden geretourneerd.  Alle toepassingen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag op basis van opsomming, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator zijn. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 wordt de status van toepassingen met HealthState waarde OK (2) en de waarschuwing (4) zijn geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535.|
| --gebeurtenissen-health-status-filter   | Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag op basis van opsomming, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator zijn. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde.  De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535.|
|--uitsluiten-health-statistieken                   | Hiermee wordt aangegeven of de statistieken van de status moet worden geretourneerd als onderdeel van het queryresultaat. Standaard False. De statistieken tonen het aantal onderliggende items entiteiten in de status Ok, waarschuwing en fout.|
 |   --opnemen-systeem-toepassing-health-statistieken| Hiermee wordt aangegeven of de gezondheidsstatistieken van de fabric moet bevatten: / System application health statistieken. Standaard False. Als IncludeSystemApplicationHealthStatistics is ingesteld op true, wordt de status statistieken omvatten de entiteiten die deel uitmaken van de fabric: / System-toepassing. Anders wordt het queryresultaat health statistische gegevens bevat, alleen voor toepassingen. De health-statistieken moeten worden opgenomen in de resultaten voor deze parameter moet worden toegepast.|
| --knooppunten-health-status-filter    | Hiermee kunt u filteren van het knooppunt health status objecten geretourneerd in het resultaat van het cluster health query op basis van hun status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen knooppunten die overeenkomen met het filter worden geretourneerd. Alle knooppunten worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is '6' wordt de status van knooppunten met HealthState waarde OK (2) en de waarschuwing (4) zijn geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde.  De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535.|
| --time-out -t                   | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                        | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
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
| --debug  | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h| Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query  | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide| Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-cluster-provision"></a>sfctl cluster inrichten
De pakketten code of configuratie van een Service Fabric-cluster inrichten.
Valideren en de pakketten code of configuratie van een Service Fabric-cluster inrichten.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
|--cluster-manifest-file-path| Het pad van het manifestbestand voor het cluster.|
|    --code bestandspad            | Het cluster code pakket bestandspad.|
|    --time-out -t                | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
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
| --debug           | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h         | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o       | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query           | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide         | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-cluster-unprovision"></a>sfctl cluster verwijderen
Inrichting van de pakketten code of configuratie van een Service Fabric-cluster.

Inrichting van de pakketten code of configuratie van een Service Fabric-cluster.

### <a name="arguments"></a>Argumenten
|Argument|Beschrijving|
| --- | --- |
|--code-versie  | De code van pakketversie van de cluster.|
|    --config-versie| De versie van het clustermanifest.|
|    --time-out -t    | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten
|Argument|Beschrijving|
| --- | --- |
|--debug         | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
 |   --help -h       | Deze help-bericht en afsluiten weergeven.|
 |   --uitvoer -o     | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
 |   --query         | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
 |   --uitgebreide       | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|


## <a name="sfctl-cluster-upgrade"></a>clusterupgrades sfctl
Start de code of upgrade van een Service Fabric-cluster.
Valideren van de opgegeven parameters voor de upgrade en start de upgrade of de configuratie van een Service Fabric-cluster als de parameters geldig zijn.

### <a name="arguments"></a>Argumenten
|Argument|Beschrijving|
| --- | --- |
|    --app-health-map                      | JSON gecodeerde woordenlijst van paren toepassingsnaam en het maximale percentage slecht voordat de fout.|
 |   --app-type-health-map                 | JSON gecodeerde woordenlijst van paren met de naam van het toepassingstype en het maximale percentage slecht voordat de fout.|
 |   --code-versie                        | De versie van de cluster.|
 |   --config-versie                      | De versie van het cluster-configuratie.|
 |   --delta-health-evaluatie             | Hiermee schakelt u delta de statusevaluatie in plaats van absolute health evaluatie na voltooiing van elk upgradedomein.|
 |   --delta slecht knooppunten               | Het maximaal toegestane percentage van de knooppunten verslechtering van de status toegestaan tijdens upgrades van de cluster.  Standaard: 10. De verschillen wordt tussen de status van de knooppunten aan het begin van de upgrade en de status van de knooppunten op het moment van de statusevaluatie gemeten. De controle wordt uitgevoerd na de upgrade voltooiing van elke upgradedomein om te controleren of dat de globale status van het cluster is binnen de limieten voor verdragen.|
 |   --actie bij fout                      | Mogelijke waarden zijn: 'Ongeldig', 'Terugdraaiactie', 'Manual'.|
 |   --force-restart                       | Geforceerd opnieuw opstarten.|
 |   --health-check-retry                  | Health selectievakje opnieuw time-out gemeten in milliseconden.|
 |   --health-controle-stabiel                 | Serverstatus controleren stabiele duur van de gemeten in milliseconden.|
  |  --wachtduur                   | Health controle wacht duur gemeten in milliseconden.|
  |  --replica-set-check-timeout           | Upgrade replicaset selectievakje time-out gemeten in seconden.|
 |   --rolling-upgrade-modus                | Mogelijke waarden zijn: 'Ongeldig', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitored'.  Standaard: UnmonitoredAuto.|
  |  --time-out -t                          | Server time-out in seconden.  Standaard: 60.|
  |  --slecht toepassingen              | Het maximaal toegestane percentage van de beschadigde toepassingen voordat u een foutmelding. Bijvoorbeeld, zodat 10% van toepassingen beschadigd is deze waarde 10. Het percentage vertegenwoordigt het verdragen maximumpercentage van toepassingen die beschadigd worden kunnen voordat het cluster wordt beschouwd als fout. Als het percentage is voldaan, maar er ten minste één slecht toepassingstype is, worden de status wordt geëvalueerd als de waarschuwing. Dit wordt berekend door het aantal beschadigde toepassingen delen via het totale aantal exemplaren van een toepassing in het cluster, met uitzondering van toepassingen van toepassingstypen die in de applicationtypehealthpolicymap; deze zijn opgenomen. De berekening rondt af naar boven op een storing op een klein aantal toepassingen tolereren.|
 |   --slecht knooppunten                     | Het maximaal toegestane percentage van slecht knooppunten voordat u een foutmelding. Bijvoorbeeld, zodat 10% van de knooppunten beschadigd is deze waarde 10. Het percentage geeft de maximale verdragen percentage van de knooppunten die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. Als het percentage is voldaan, maar er ten minste één slecht knooppunt is, wordt de status wordt geëvalueerd als waarschuwing. Het percentage wordt berekend door het aantal beschadigde knooppunten delen via het totale aantal knooppunten in het cluster. De berekening rondt af naar boven op een storing op een klein aantal knooppunten tolereren. In grote clusters sommige knooppunten altijd worden omlaag of uit voor reparaties, zodat dit percentage moet worden geconfigureerd voor die tolereren.|
 |   --upgrade-domain-delta-slecht-knooppunten| Het maximaal toegestane percentage van de knooppunten van het upgradedomein verslechtering van de status toegestaan tijdens upgrades van de cluster. Standaard: 15. De verschillen wordt tussen de status van de upgradedomein-knooppunten aan het begin van de upgrade en de status van de upgradedomein-knooppunten op het moment van de statusevaluatie gemeten. De controle wordt uitgevoerd na elke opwaarderen upgradedomein is voltooid voor alle upgradedomeinen om ervoor te zorgen dat de status van de upgradedomeinen is binnen de limieten voor verdragen.|
 |   --upgrade-domain-timeout              | Time-out upgradedomein gemeten in milliseconden.|
 |   --upgrade-timeout                     | Time-out van de upgrade, gemeten in milliseconden.|
 |   --waarschuwing als fout                    | Waarschuwingen worden met de ernst van de dezelfde fouten behandeld.|

### <a name="global-arguments"></a>Algemene argumenten
|Argument|Beschrijving|
| --- | --- |
|--debug                               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
|    --help -h                             | Deze help-bericht en afsluiten weergeven.|
|    --uitvoer -o                           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv. Standaard: json.|
|    --query                               | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
|    --uitgebreide                             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="next-steps"></a>Volgende stappen
- [Setup](service-fabric-cli.md) de Service Fabric CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).