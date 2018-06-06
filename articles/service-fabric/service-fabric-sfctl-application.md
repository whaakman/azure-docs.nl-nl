---
title: Azure Service Fabric CLI - sfctl-toepassing | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl toepassing-opdrachten.
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
ms.openlocfilehash: 3ecc5a03ff1847dc11c5a5047e35566a4e68fec2
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763694"
---
# <a name="sfctl-application"></a>sfctl application
Maken, verwijderen en beheren van toepassingen en die van toepassing.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| maken | Hiermee maakt een Service Fabric-toepassing met behulp van de opgegeven beschrijving. |
| verwijderen | Hiermee verwijdert u een bestaande Service Fabric-toepassing. |
| geïmplementeerd | Hiermee haalt u de informatie over een toepassing is geïmplementeerd op een Service Fabric-knooppunt. |
| geïmplementeerd health | Hiermee haalt u de informatie over de status van een toepassing is geïmplementeerd op een Service Fabric-knooppunt. |
| geïmplementeerd lijst | De lijst met toepassingen die zijn geïmplementeerd op een Service Fabric-knooppunt ophalen. |
| status | Hiermee haalt u de status van de service fabric-toepassing. |
| informatie | Hiermee haalt u informatie over een Service Fabric-toepassing. |
| lijst | Hiermee wordt de lijst met toepassingen die in de Service Fabric-cluster gemaakt die overeenkomen met de opgegeven filters opgehaald. |
| laden | Haalt informatie over een Service Fabric-toepassing worden geladen. |
| Manifest | Het manifest met een beschrijving van een toepassingstype opgehaald. |
| Inrichten | Bepalingen of registers typt u een Service Fabric-toepassing met het cluster met behulp van het pakket .sfpkg in de externe opslag of het gebruik van het toepassingspakket in de image store. |
| rapport-status | Verzendt een statusrapport over de Service Fabric-toepassing. |
| type | Hiermee haalt de lijst met soorten toepassingen in de Service Fabric-cluster die overeenkomt met de opgegeven naam. |
| lijst van type | Hiermee haalt de lijst met soorten toepassingen in de Service Fabric-cluster. |
| Inrichting verwijderen | Verwijdert of heft de registratie van een Service Fabric-toepassingstype uit het cluster. |
| upgrade | Hiermee start u een upgrade van een toepassing in het Service Fabric-cluster. |
| upgrade hervatten | Hervat een upgrade van een toepassing in het Service Fabric-cluster. |
| upgrade terugdraaien | Start het terugdraaien van de momenteel continu upgrade van een toepassing in het Service Fabric-cluster. |
| upgrade-status | Hiermee wordt informatie opgehaald voor de meest recente upgrade uitgevoerd voor deze toepassing. |
| uploaden | Een Service Fabric-toepassing-pakket kopiëren naar de image store. |

## <a name="sfctl-application-create"></a>sfctl toepassing maken
Hiermee maakt een Service Fabric-toepassing met behulp van de opgegeven beschrijving.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-naam [vereist] | De naam van de toepassing, met inbegrip van de ' fabric\:' URI-schema. |
| --app-type [vereist] | De naam van de toepassing type is gevonden in het toepassingsmanifest. |
| --app-versie [vereist] | De versie van het toepassingstype, zoals gedefinieerd in het toepassingsmanifest. |
| --max-node-count | Het maximum aantal knooppunten waar de Service Fabric capaciteit voor deze toepassing wordt gereserveerd. Houd er rekening mee dat dit niet betekent dat de services van deze toepassing op alle knooppunten worden geplaatst. |
| --metrische gegevens | Een JSON gecodeerde lijst met toepassingen capaciteit metrische beschrijvingen. Een metriek is gedefinieerd als een naam, die is gekoppeld aan een set met capaciteiten voor elk knooppunt dat de toepassing bestaat op. |
| --min in het aantal knooppunten | Het minimale aantal knooppunten waar de Service Fabric capaciteit voor deze toepassing wordt gereserveerd. Houd er rekening mee dat dit niet betekent dat de services van deze toepassing op alle knooppunten worden geplaatst. |
| --parameters | Een lijst met JSON gecodeerde van toepassing parameter onderdrukkingen om te worden toegepast bij het maken van de toepassing. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-delete"></a>sfctl toepassing verwijderen
Hiermee verwijdert u een bestaande Service Fabric-toepassing.

Hiermee verwijdert u een bestaande Service Fabric-toepassing. Een toepassing moet worden gemaakt voordat het kan worden verwijderd. Een toepassing verwijdert, worden alle services die deel van die toepassing uitmaken. Standaard probeert Service Fabric service replica's op correcte wijze sluiten en verwijder vervolgens de service. Echter, als een service is met het sluiten van de replica probleemloos problemen, de delete-bewerking kan lang duren of hangen. Gebruik het optionele ForceRemove vlag de correcte sluiten van een reeks overslaan en de toepassing en alle bijbehorende services geforceerd verwijderen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
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

## <a name="sfctl-application-deployed"></a>sfctl toepassing geïmplementeerd
Hiermee haalt u de informatie over een toepassing is geïmplementeerd op een Service Fabric-knooppunt.

Hiermee haalt u de informatie over een toepassing is geïmplementeerd op een Service Fabric-knooppunt.  Deze query retourneert systeemgegevens van toepassing als de toepassings-ID die is opgegeven voor system-toepassing. Resultaten omvatten geïmplementeerde toepassingen in actieve, activeren en het downloaden van statussen. Deze query vereist dat de naam overeenkomt met een knooppunt in het cluster. De query is mislukt als de naam van het opgegeven knooppunt wijst niet naar een actieve Service Fabric-knooppunten op het cluster.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --omvatten de status | De status van een entiteit bevatten. Als deze parameter false of niet opgegeven is, wordt de status geretourneerd 'Onbekend'. Wanneer is ingesteld op true, wordt de query parallel met het knooppunt en de system health service gaat voordat de resultaten worden samengevoegd. Als gevolg hiervan de query is duurder en langere tijd kan duren. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-deployed-health"></a>sfctl toepassing geïmplementeerd-status
Hiermee haalt u de informatie over de status van een toepassing is geïmplementeerd op een Service Fabric-knooppunt.

Hiermee haalt u de informatie over de status van een toepassing is geïmplementeerd op een Service Fabric-knooppunt. EventsHealthStateFilter gebruiken voor het filteren van optioneel voor de verzameling van HealthEvent-objecten die zijn gerapporteerd voor de gedistribueerde toepassing die is gebaseerd op de status. DeployedServicePackagesHealthStateFilter gebruiken voor het filteren van eventueel voor DeployedServicePackageHealth kinderen gebaseerd op de status.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --Deployed-service-Packages-Health-State-filter | Kan de geïmplementeerde service pakket health status objecten geretourneerd in het resultaat van de geïmplementeerde toepassing health query filteren op basis van hun status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen geïmplementeerd service pakketten die overeenkomen met het filter worden geretourneerd. Alle geïmplementeerde servicepakketten worden gebruikt voor het evalueren van de cumulatieve status van de geïmplementeerde toepassing. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator zijn. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 wordt status van servicepakketten met HealthState waarde OK (2) en de waarschuwing (4) worden geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --uitsluiten-health-statistieken | Hiermee wordt aangegeven of de statistieken van de status moet worden geretourneerd als onderdeel van het queryresultaat. Standaard False. De statistieken tonen het aantal onderliggende items entiteiten in de status Ok, waarschuwing en fout. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-deployed-list"></a>sfctl geïmplementeerd-lijst met toepassingen
De lijst met toepassingen die zijn geïmplementeerd op een Service Fabric-knooppunt ophalen.

De lijst met toepassingen die zijn geïmplementeerd op een Service Fabric-knooppunt ophalen. De resultaten bevatten geen informatie over geïmplementeerde systeemtoepassingen, tenzij expliciet opgevraagd op-ID. Resultaten omvatten geïmplementeerde toepassingen in actieve, activeren en het downloaden van statussen. Deze query vereist dat de naam overeenkomt met een knooppunt in het cluster. De query is mislukt als de naam van het opgegeven knooppunt wijst niet naar een actieve Service Fabric-knooppunten op het cluster.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --knooppuntnaam [vereist] | De naam van het knooppunt. |
| --vervolgtoken | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd. |
| --omvatten de status | De status van een entiteit bevatten. Als deze parameter false of niet opgegeven is, is de status geretourneerd 'Onbekend'. Wanneer is ingesteld op true, wordt de query parallel met het knooppunt en de system health service gaat voordat de resultaten worden samengevoegd. Als gevolg hiervan de query is duurder en langere tijd kan duren. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal resultaten geretourneerd. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de groottebeperkingen max bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de paged query zo veel resultaten mogelijk die in het retourbericht passen. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-health"></a>de toepassingsstatus sfctl
Hiermee haalt u de status van de service fabric-toepassing.

Retourneert de Health-status van de service fabric-toepassing. Het antwoord rapporteert de status Ok, fout of waarschuwing. Als de entiteit niet in de health store gevonden is, wordt deze fout geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --geïmplementeerd-toepassingen-health-status-filter | U kunt in het resultaat van toepassing op basis van hun status health-query voor het filteren van de geïmplementeerde toepassingen health status objecten geretourneerd. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen geïmplementeerde toepassingen die overeenkomen met het filter wordt geretourneerd. Alle geïmplementeerde toepassingen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 wordt de status van geïmplementeerde toepassingen met HealthState waarde OK (2) en de waarschuwing (4) zijn geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --uitsluiten-health-statistieken | Hiermee wordt aangegeven of de statistieken van de status moet worden geretourneerd als onderdeel van het queryresultaat. Standaard False. De statistieken tonen het aantal onderliggende items entiteiten in de status Ok, waarschuwing en fout. |
| --services-health-status-filter | U kunt in het resultaat van de services op basis van hun status health-query voor het filteren van de services health status objecten geretourneerd. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen services die overeenkomen met het filter worden geretourneerd. Alle services worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 wordt vervolgens de status van services met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul.  <br> -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1.  <br> -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-info"></a>toepassingsgegevens sfctl
Hiermee haalt u informatie over een Service Fabric-toepassing.

Retourneert de gegevens over de toepassing die is gemaakt of die momenteel wordt gemaakt in het Service Fabric-cluster en waarvan de naam overeenkomt met de versie die is opgegeven als parameter. Het antwoord bevat de naam, type, status, parameters en andere details over de toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --uitsluiten toepassingsparameters | De vlag die aangeeft of de parameters voor de toepassing zal worden uitgesloten van het resultaat. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-list"></a>lijst met sfctl-toepassingen
Hiermee wordt de lijst met toepassingen die in de Service Fabric-cluster gemaakt die overeenkomen met de opgegeven filters opgehaald.

Hiermee wordt de informatie over de toepassingen die zijn gemaakt of het proces wordt gemaakt in de Service Fabric-cluster en overeenkomen met de opgegeven filters opgehaald. Het antwoord bevat de naam, type, status, parameters en andere details over de toepassing. Als de toepassingen niet in een pagina passen, wordt één pagina met resultaten geretourneerd en een vervolgtoken die kan worden gebruikt om op te halen van de volgende pagina. Filters ApplicationTypeName en ApplicationDefinitionKindFilter kunnen niet tegelijkertijd worden opgegeven.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-definitie-type-filter | Gebruikt voor het filteren op ApplicationDefinitionKind, van het mechanisme dat wordt gebruikt voor het definiëren van een Service Fabric-toepassing.  <br> -Standaard - standaardwaarde, die voert dezelfde functie uit als u 'Alle' selecteren. De waarde is 0.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een ApplicationDefinitionKind. De waarde is 65535.  <br> -ServiceFabricApplicationDescription - Filter dat overeenkomt met invoer met ApplicationDefinitionKind waarde ServiceFabricApplicationDescription. De waarde is 1.  <br> -Opstellen - Filter die overeenkomt met invoer met ApplicationDefinitionKind waarde opstellen. De waarde is 2. |
| --toepassing typenaam | De naam van de toepassing type is gebruikt voor het filteren van de toepassingen om te zoeken. Deze waarde moet de versie van het toepassingstype niet bevatten. |
| --vervolgtoken | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd. |
| --uitsluiten toepassingsparameters | De vlag die aangeeft of de parameters voor de toepassing zal worden uitgesloten van het resultaat. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal resultaten geretourneerd. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de groottebeperkingen max bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de paged query zo veel resultaten mogelijk die in het retourbericht passen. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-load"></a>sfctl toepassing werklast
Haalt informatie over een Service Fabric-toepassing worden geladen.

Retourneert de load-informatie over de toepassing die is gemaakt of die momenteel wordt gemaakt in het Service Fabric-cluster en waarvan de naam overeenkomt met de versie die is opgegeven als parameter. Het antwoord bevat de naam, knooppunten minimum, maximum aantal knooppunten, het aantal knooppunten dat de toepassing momenteel wordt ingenomen en toepassing werklast metrische informatie over de toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-manifest"></a>sfctl-toepassingsmanifest
Het manifest met een beschrijving van een toepassingstype opgehaald.

Het manifest met een beschrijving van een toepassingstype opgehaald. Het antwoord bevat de manifest-XML voor de toepassing als een tekenreeks.

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

## <a name="sfctl-application-provision"></a>sfctl toepassing inrichten
Bepalingen of registers typt u een Service Fabric-toepassing met het cluster met behulp van het pakket .sfpkg in de externe opslag of het gebruik van het toepassingspakket in de image store.

Voorziet in een type Service Fabric-toepassing met het cluster. Dit is vereist voordat de nieuwe toepassingen kunnen worden gemaakt. De inrichtingsbewerking kan worden uitgevoerd op het toepassingspakket dat is opgegeven met de relativePathInImageStore, of met behulp van de URI van de externe .sfpkg. Tenzij--externe-provision is ingesteld, wordt deze opdracht image store inrichten verwacht.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --application-pakket-download-uri | Het pad naar het toepassingspakket '.sfpkg' waar het toepassingspakket kan worden gedownload met behulp van HTTP of HTTPS-protocollen. <br><br> Opslaan alleen voor inrichten soort externe. Het toepassingspakket kan worden opgeslagen in een externe winkel waarmee GET-bewerking om het bestand te downloaden. Ondersteunde protocollen zijn HTTP en HTTPS en het pad moet leestoegang toestaan. |
| ---type-build-pad naar toepassing | Voor alleen richten kind image store. Het relatieve pad voor het toepassingspakket in de image store opgegeven tijdens de vorige uploadbewerking. |
| --toepassing typenaam | Opslaan alleen voor inrichten soort externe. De naam van het toepassingstype vertegenwoordigt de naam van het toepassingstype gevonden in het toepassingsmanifest. |
| ----versie van het toepassingstype | Opslaan alleen voor inrichten soort externe. De versie van het toepassingstype vertegenwoordigt de versie van het toepassingstype gevonden in het toepassingsmanifest. |
| --externe inrichten | De locatie vanaf waar toepassingspakket kan worden geregistreerd of ingericht. Geeft aan dat de voorziening voor een toepassingspakket dat eerder is geüpload naar een externe winkel. Het toepassingspakket eindigt met de extensie *.sfpkg. |
| --Er is geen wacht | Hiermee wordt aangegeven of inrichting asynchroon moet plaatsvinden. <br><br> Wanneer is ingesteld op true, retourneert de inrichtingsbewerking blijft wanneer de aanvraag wordt geaccepteerd door het systeem en de inrichtingsbewerking zonder een time-outlimiet. De standaardwaarde is ingesteld op false. Voor grote toepassingpakketten, wordt u aangeraden de waarde instelt op true. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-report-health"></a>sfctl rapport-status
Verzendt een statusrapport over de Service Fabric-toepassing.

Meldt de status van de opgegeven Service Fabric-toepassing. Het rapport moet bevatten informatie over de bron van de health-rapport en de eigenschap waarop deze wordt gerapporteerd. Het rapport wordt verzonden naar een Service Fabric-gateway-toepassing die naar de health store doorstuurt. Het rapport kan worden geaccepteerd door de gateway, maar geweigerd door de health store na extra validatie. De health store kan bijvoorbeeld het rapport weigeren vanwege een ongeldige parameter, zoals een verouderde volgnummer. Toepassingsstatus ophalen om te zien of het rapport in de health store is toegepast, en controleer of het rapport wordt weergegeven.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. <br><br> Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld ' fabric\:/myapp/app1 ", zou de toepassings-id ' myapp\~app1' in 6.0 + en ' myapp/app1' in eerdere versies. |
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

## <a name="sfctl-application-type"></a>toepassingstype sfctl
Hiermee haalt de lijst met soorten toepassingen in de Service Fabric-cluster die overeenkomt met de opgegeven naam.

Retourneert de gegevens over de toepassingstypen die zijn ingericht of die momenteel wordt ingericht in het Service Fabric-cluster. Deze resultaten zijn van toepassingstypen waarvan de naam overeenkomt met een opgegeven als parameter, en die voldoen aan de opgegeven queryparameters. Alle versies van het type van de toepassing die overeenkomt met de naam van het toepassingstype worden geretourneerd met elke versie die wordt geretourneerd als een toepassingstype. Het antwoord bevat de naam, versie, status en andere details over het toepassingstype. Dit is een paged query, wat betekent dat als dat niet alle toepassingstypen in een pagina past één pagina met resultaten geretourneerd en een vervolgtoken die kan worden gebruikt om op te halen van de volgende pagina. Bijvoorbeeld, als er 10 toepassingstypen maar een pagina past alleen de eerste drie toepassingstypen, of als resultaat max is ingesteld op 3, wordt 3 geretourneerd. Voor toegang tot de rest van de resultaten moet de volgende pagina's met behulp van de geretourneerde vervolgtoken in de volgende query worden opgehaald. Een leeg vervolgtoken wordt geretourneerd als er geen verdere's zijn.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| ----naam van het toepassingstype [vereist] | De naam van het toepassingstype. |
| ----versie van het toepassingstype | De versie van het toepassingstype. |
| --vervolgtoken | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd. |
| --uitsluiten toepassingsparameters | De vlag die aangeeft of de parameters voor de toepassing zal worden uitgesloten van het resultaat. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal resultaten geretourneerd. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de groottebeperkingen max bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de paged query zo veel resultaten mogelijk die in het retourbericht passen. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-type-list"></a>sfctl type-lijst met toepassingen
Hiermee haalt de lijst met soorten toepassingen in de Service Fabric-cluster.

Retourneert de gegevens over de toepassingstypen die zijn ingericht of die momenteel wordt ingericht in het Service Fabric-cluster. Elke versie van een toepassingstype wordt geretourneerd als een toepassingstype. Het antwoord bevat de naam, versie, status en andere details over het toepassingstype. Dit is een paged query, wat betekent dat als dat niet alle toepassingstypen in een pagina past één pagina met resultaten geretourneerd en een vervolgtoken die kan worden gebruikt om op te halen van de volgende pagina. Bijvoorbeeld, als er 10 toepassingstypen maar een pagina past alleen de eerste drie toepassingstypen, of als resultaat max is ingesteld op 3, wordt 3 geretourneerd. Voor toegang tot de rest van de resultaten moet de volgende pagina's met behulp van de geretourneerde vervolgtoken in de volgende query worden opgehaald. Een leeg vervolgtoken wordt geretourneerd als er geen verdere's zijn.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-type-definitie-type-filter | Gebruikt voor het filteren op ApplicationTypeDefinitionKind, van het mechanisme dat wordt gebruikt voor het definiëren van het type van een Service Fabric-toepassing.  <br> -Standaard - standaardwaarde, die voert dezelfde functie uit als u 'Alle' selecteren. De waarde is 0.  <br> -All - Filter dat overeenkomt met invoer met de waarde van een ApplicationTypeDefinitionKind. De waarde is 65535.  <br> -ServiceFabricApplicationPackage - Filter dat overeenkomt met invoer met ApplicationTypeDefinitionKind waarde ServiceFabricApplicationPackage. De waarde is 1.  <br> -Opstellen - Filter die overeenkomt met invoer met ApplicationTypeDefinitionKind waarde opstellen. De waarde is 2. |
| --vervolgtoken | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd. |
| --uitsluiten toepassingsparameters | De vlag die aangeeft of de parameters voor de toepassing zal worden uitgesloten van het resultaat. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal resultaten geretourneerd. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de groottebeperkingen max bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de paged query zo veel resultaten mogelijk die in het retourbericht passen. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-unprovision"></a>sfctl toepassing verwijderen
Verwijdert of heft de registratie van een Service Fabric-toepassingstype uit het cluster.

Verwijdert of heft de registratie van een Service Fabric-toepassingstype uit het cluster. Deze bewerking kan alleen worden uitgevoerd als alle exemplaren van een toepassing van het toepassingstype is verwijderd. Zodra het toepassingstype geregistreerd is, kunnen geen nieuwe exemplaren van een toepassing worden gemaakt voor dit type bepaalde toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| ----naam van het toepassingstype [vereist] | De naam van het toepassingstype. |
| ----versie van het toepassingstype [vereist] | De versie van het toepassingstype, zoals gedefinieerd in het toepassingsmanifest. |
| --async-parameter | De vlag die aangeeft of inrichting verwijderen asynchroon moet plaatsvinden. Als is ingesteld op true, wordt de bewerking verwijderen retourneert wanneer de aanvraag wordt geaccepteerd door het systeem en de bewerking verwijderen wordt voortgezet zonder een time-outlimiet. De standaardwaarde is ingesteld op false. We raden echter instellen op true voor grote toepassingspakketten die zijn ingericht. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-upgrade"></a>upgrade van de toepassing sfctl
Hiermee start u een upgrade van een toepassing in het Service Fabric-cluster.

Valideert de parameters voor het bijwerken van opgegeven toepassing en start de toepassing bijwerken als de parameters geldig zijn. Houd er rekening mee dat de beschrijving van de upgrade de beschrijving van de bestaande toepassing vervangt. Dit betekent dat als de parameters niet zijn opgegeven, de bestaande parameters van de toepassingen worden overschreven door de lijst leeg parameters. Dit zou leiden tot de toepassing die gebruikmaakt van de standaardwaarde van de parameters van het toepassingsmanifest.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. <br><br> Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld ' fabric\:/myapp/app1 ", zou de toepassings-id ' myapp\~app1' in 6.0 + en ' myapp/app1' in eerdere versies. |
| --toepassingsversie [vereist] | Versie van de doel-toepassing. |
| --parameters [vereist] | Een lijst met JSON gecodeerde van toepassing parameter onderdrukkingen om te worden toegepast bij de upgrade van de toepassing. |
| ---service-health-standaardbeleid | JSON gecodeerde specificatie van het statusbeleid gebruikt standaard voor het evalueren van de status van een servicetype. |
| --actie bij fout | De actie om uit te voeren wanneer de upgrade van een bewaakte bewaking beleid of health beleidsschendingen optreedt. |
| --geforceerd opnieuw opstarten | Start processen geforceerd opnieuw tijdens de upgrade, zelfs wanneer de codeversie is niet gewijzigd. |
| --health-controle-opnieuw-timeout | De hoeveelheid tijd health evaluaties opnieuw wanneer de toepassing of het cluster slecht vóór de fout actie is wordt uitgevoerd. Gemeten in milliseconden.  Standaard\: PT0H10M0S. |
| --health selectievakje-stabiele duur | De hoeveelheid tijd dat de toepassing of het cluster in orde blijven moet voordat de upgrade wordt uitgevoerd op het volgende upgradedomein. Gemeten in milliseconden.  Standaard\: PT0H2M0S. |
| --health-controle-wait-duur | De hoeveelheid tijd moet worden gewacht na het voltooien van een upgradedomein alvorens statusbeleid toe te passen. Gemeten in milliseconden.  Standaard\: 0. |
| --max-slecht-apps | Het maximaal toegestane percentage van de beschadigde geïmplementeerde toepassingen. Weergegeven als een getal tussen 0 en 100 liggen. |
| --modus | De modus voor de status bewaakt tijdens een rolling upgrade.  Standaard\: UnmonitoredAuto. |
| --replica-set-check-timeout | De maximale hoeveelheid tijd voor het blokkeren van de verwerking van een upgradedomein en verlies van beschikbaarheid voorkomen wanneer er onverwachte problemen zijn. Gemeten in seconden. |
| --service statusbeleid | JSON gecodeerde kaart met het service type statusbeleid per type servicenaam. De kaart is leeg worden standaard. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |
| --time-out-domein-upgrade | De hoeveelheid tijd elk upgradedomein heeft voltooid voordat FailureAction wordt uitgevoerd. Gemeten in milliseconden.  Standaard\: P10675199DT02H48M05.4775807S. |
| --upgrade-time-out | De hoeveelheid tijd die de algehele upgrade heeft voltooid voordat FailureAction wordt uitgevoerd. Gemeten in milliseconden.  Standaard\: P10675199DT02H48M05.4775807S. |
| --waarschuwing als fout | Evaluatie van waarschuwingen met de dezelfde ernst als fouten behandelen. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-upgrade-resume"></a>sfctl toepassing upgrade-hervatten
Hervat een upgrade van een toepassing in het Service Fabric-cluster.

Hervat een niet-bewaakte handmatige Service Fabric-toepassing upgraden. Service Fabric worden één upgradedomein tegelijk bijgewerkt. Voor niet-bewaakte handmatige upgrades, nadat een upgradedomein klaar is met Service Fabric wacht voor u deze API voordat u doorgaat naar de volgende upgradedomein niet aanroepen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --upgrade-domain-name [vereist] | De naam van het upgradedomein waarin u de upgrade te hervatten. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-upgrade-rollback"></a>sfctl toepassing upgrade-terugdraaien
Start het terugdraaien van de momenteel continu upgrade van een toepassing in het Service Fabric-cluster.

Terugdraaien van de huidige toepassing wordt gestart upgraden naar de vorige versie. Deze API kan alleen worden gebruikt voor het terugdraaien van de huidige upgrade wordt uitgevoerd die vooruit te naar de nieuwe versie rollen is. Als de toepassing op dit moment niet worden bijgewerkt gebruik StartApplicationUpgrade API te upgraden naar de gewenste versie, waaronder terugdraaien naar een eerdere versie.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-upgrade-status"></a>sfctl toepassing upgrade-status
Hiermee wordt informatie opgehaald voor de meest recente upgrade uitgevoerd voor deze toepassing.

Retourneert informatie over de status van de upgrade van de meest recente toepassing samen met details om u te helpen bij foutopsporing toepassingsproblemen health.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist] | De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het '\~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric\:/myapp/app1", zou de toepassings-id ' myapp\~app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --time-out -t | Server time-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken. |
| --help -h | Deze help-bericht en afsluiten weergeven. |
| --uitvoer -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath queryreeks. Zie http\://jmespath.org/ voor meer informatie over en voorbeelden. |
| --uitgebreide | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken. |

## <a name="sfctl-application-upload"></a>sfctl toepassing uploaden
Een Service Fabric-toepassing-pakket kopiëren naar de image store.

Bezig met uploaden voor elk bestand eventueel weergeven in het pakket. Uploaden voortgang wordt verzonden naar `stderr`.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --pad [vereist] | Pad naar lokale toepassingspakket. |
| --installatiekopieopslag-tekenreeks | De installatiekopie van het doel voor het uploaden van het toepassingspakket om te worden opgeslagen.  Standaard\: fabric\:Installatiekopieopslag. |
| --weergeven uitgevoerd | Bestand uploadvoortgang voor grote pakketten weergeven. |

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
