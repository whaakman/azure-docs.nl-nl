---
title: Azure Service Fabric CLI - sfctl-toepassing | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl toepassing-opdrachten.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 02/23/2018
ms.author: ryanwi
ms.openlocfilehash: fe0ef5c81b1ef6bef298e65cde3649c9464089d8
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="sfctl-application"></a>sfctl application
Maken, verwijderen en beheren van toepassingen en die van toepassing.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| maken       | Hiermee maakt een Service Fabric-toepassing met behulp van de opgegeven beschrijving.|
| verwijderen       | Hiermee verwijdert u een bestaande Service Fabric-toepassing.|
| geïmplementeerd     | Hiermee haalt u de informatie over een toepassing is geïmplementeerd op een Service Fabric-knooppunt.|
| geïmplementeerd health | Hiermee haalt u de informatie over de status van een toepassing is geïmplementeerd op een Service Fabric-knooppunt.|
| geïmplementeerd lijst| De lijst met toepassingen die zijn geïmplementeerd op een Service Fabric-knooppunt ophalen.|
| status       | Hiermee haalt u de status van de service fabric-toepassing.|
| info         | Hiermee haalt u informatie over een Service Fabric-toepassing.|
| lijst         | Hiermee wordt de lijst met toepassingen die in de Service Fabric-cluster gemaakt die overeenkomen met opgegeven als parameter filters opgehaald.|
| laden | Haalt informatie over een Service Fabric-toepassing worden geladen. |
| manifest     | Het manifest met een beschrijving van een toepassingstype opgehaald.|
| Inrichten    | Bepalingen of registers typt u een Service Fabric-toepassing met het cluster met behulp van het pakket .sfpkg in de externe opslag of het gebruik van het toepassingspakket in de image store.|
| rapport-status| Verzendt een statusrapport over de Service Fabric-toepassing.|
| type         | Hiermee haalt de lijst met soorten toepassingen in de Service Fabric-cluster die overeenkomt met de opgegeven naam.|
| type-list    | Hiermee haalt de lijst met soorten toepassingen in de Service Fabric-cluster.|
| Inrichting verwijderen  | Verwijdert of heft de registratie van een Service Fabric-toepassingstype uit het cluster.|
| upgrade      | Hiermee start u een upgrade van een toepassing in het Service Fabric-cluster.|
| upgrade hervatten  | Hervat een upgrade van een toepassing in het Service Fabric-cluster.|
| upgrade-rollback| Start het terugdraaien van de momenteel continu upgrade van een toepassing in het Service Fabric-cluster.|
| upgrade-status  | Hiermee wordt informatie opgehaald voor de meest recente upgrade uitgevoerd voor deze toepassing.|
| uploaden       | Een Service Fabric-toepassing-pakket kopiëren naar de image store.|

## <a name="sfctl-application-create"></a>sfctl toepassing maken
Hiermee maakt een Service Fabric-toepassing met behulp van de opgegeven beschrijving.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-naam [vereist]| De naam van de toepassing, met inbegrip van de "fabric:' URI-schema.|
| --app-type [vereist]| De naam van de toepassing type is gevonden in het toepassingsmanifest.|
| --app-versie [vereist]| De versie van het toepassingstype, zoals gedefinieerd in het toepassingsmanifest.|
| --max-node-count     | Het maximum aantal knooppunten dat Service Fabric capaciteit voor deze toepassing reserveert. Dit betekent niet dat de services van deze toepassing op alle knooppunten zijn geplaatst.|
| --metrische gegevens            | Een JSON gecodeerde lijst met toepassingen capaciteit metrische beschrijvingen. Een metriek is gedefinieerd als een naam, die is gekoppeld aan een set met capaciteiten voor elk knooppunt dat de toepassing bestaat op.|
| --min in het aantal knooppunten     | Het minimale aantal knooppunten dat Service Fabric capaciteit voor deze toepassing reserveert. Dit betekent niet dat de services van deze toepassing op alle knooppunten zijn geplaatst.|
| --parameters         | Een lijst met JSON gecodeerde van toepassing parameter onderdrukkingen om te worden toegepast bij het maken van de toepassing.|
| --time-out -t         | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug              | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h            | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o          | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query              | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide            | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-application-delete"></a>sfctl toepassing verwijderen
Hiermee verwijdert u een bestaande Service Fabric-toepassing.

Hiermee verwijdert u een bestaande Service Fabric-toepassing. Een toepassing moet worden gemaakt voordat het kan worden verwijderd. Een toepassing verwijdert, worden alle services die deel van die toepassing uitmaken. Standaard probeert de Service Fabric service replica's op correcte wijze sluiten en verwijder vervolgens de service. Echter als service heeft problemen met de replica probleemloos sluit, de delete-bewerking kan lang duren of hangen. Gebruik het optionele ForceRemove vlag de correcte sluiten van een reeks overslaan en de toepassing en alle bijbehorende services geforceerd verwijderen.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist]| De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric: / myapp/app1", zou de toepassings-id ' myapp ~ app1 ' in 6.0 + en "myapp/app1" in eerdere versies.|
| --force-remove          | Verwijder een Service Fabric-toepassing of service geforceerd zonder tussenkomst van de reeks correct afsluiten. Deze parameter kan worden gebruikt geforceerd verwijderen van een toepassing of service voor welke verwijderen krijgt een time-out vanwege problemen met de code die voorkomt dat correcte sluit van replica's.|
| --time-out -t            | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                 | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h               | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o             | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query                 | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide               | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-application-deployed"></a>sfctl toepassing geïmplementeerd
Hiermee haalt u de informatie over een toepassing is geïmplementeerd op een Service Fabric-knooppunt.

Hiermee haalt u de informatie over een toepassing is geïmplementeerd op een Service Fabric-knooppunt.  Deze query retourneert systeemgegevens van toepassing als de toepassings-ID die is opgegeven voor system-toepassing. Resultaten omvatten geïmplementeerde toepassingen in actieve, activeren en het downloaden van statussen. Deze query vereist dat de naam overeenkomt met een knooppunt in het cluster. De query is mislukt als de naam van het opgegeven knooppunt wijst niet naar een actieve Service Fabric-knooppunten op het cluster.
     
### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist]| De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric: / myapp/app1", zou de toepassings-id ' myapp ~ app1 ' in 6.0 + en "myapp/app1" in eerdere versies.|
| --knooppuntnaam [vereist]| De naam van het knooppunt.|
| --time-out -t            | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                 | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h               | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o             | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query                 | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide               | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-application-health"></a>de toepassingsstatus sfctl
Hiermee haalt u de status van de service fabric-toepassing.

Retourneert de Health-status van de service fabric-toepassing. Het antwoord rapporteert de status Ok, fout of waarschuwing. Als de entiteit niet in de health store gevonden is, deze fout geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist]| De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric: / myapp/app1", zou de toepassings-id ' myapp ~ app1 ' in 6.0 + en "myapp/app1" in eerdere versies.|
| --geïmplementeerd-toepassingen-health-status-filter| U kunt in het resultaat van toepassing op basis van hun status health-query voor het filteren van de geïmplementeerde toepassingen health status objecten geretourneerd. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen geïmplementeerde toepassingen die overeenkomen met het filter wordt geretourneerd. Alle geïmplementeerde toepassingen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 wordt de status van geïmplementeerde toepassingen met HealthState waarde OK (2) en de waarschuwing (4) zijn geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535.|
| --gebeurtenissen-health-status-filter            | Hiermee kunt u filteren van de verzameling HealthEvent objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen die worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 vervolgens alle gebeurtenissen met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535.|
| --uitsluiten-health-statistieken | Hiermee wordt aangegeven of de statistieken van de status moet worden geretourneerd als onderdeel van het queryresultaat. Standaard False. De statistieken tonen het aantal onderliggende items entiteiten in de status Ok, waarschuwing en fout.|
| --services-health-status-filter          | U kunt in het resultaat van de services op basis van hun status health-query voor het filteren van de services health status objecten geretourneerd. De mogelijke waarden voor deze parameter zijn geheel getal van een van de volgende statussen. Alleen services die overeenkomen met het filter worden geretourneerd. Alle services worden gebruikt voor het evalueren van de geaggregeerde status. Als niet wordt opgegeven, worden alle items geretourneerd. De statuswaarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan een combinatie van deze waarden die zijn verkregen met behulp van bitwise "OR"-operator worden. Bijvoorbeeld, als de opgegeven waarde is ingesteld op 6 wordt de status van services met HealthState waarde OK (2) en de waarschuwing (4) geretourneerd. -Standaard - standaardwaarde. Komt overeen met een HealthState. De waarde is nul. -None - filteren die niet overeenkomt met de waarde van een HealthState. Gebruikt om te kunnen geen resultaten geretourneerd bij een bepaalde verzameling van statussen. De waarde is 1. -Ok - Filter dat overeenkomt met invoer-met HealthState waarde Ok. De waarde is 2. -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4. -Fout - Filter dat overeenkomt met invoer met HealthState waarde fout. De waarde is 8. -All - Filter dat overeenkomt met invoer met de waarde van een HealthState. De waarde is 65535.|
| --time-out -t                            | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                                 | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                               | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                             | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query                                 | JMESPath queryreeks. Zie voor meer informatie http://jmespath.org/.|
| --uitgebreide                               | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-application-info"></a>toepassingsgegevens sfctl
Hiermee haalt u informatie over een Service Fabric-toepassing.

Retourneert de gegevens over de toepassing die is gemaakt of die momenteel wordt gemaakt in het Service Fabric-cluster en waarvan de naam overeenkomt met de versie die is opgegeven als parameter. Het antwoord bevat de naam, type, status, parameters en andere details over de toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --toepassing-id [vereist]| De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric: / myapp/app1", zou de toepassings-id ' myapp ~ app1 ' in 6.0 + en "myapp/app1" in eerdere versies.|
| --uitsluiten toepassingsparameters| De vlag die aangeeft of de parameters voor de toepassing zal worden uitgesloten van het resultaat.|
| --time-out -t                 | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                      | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                    | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                  | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.             Standaard: json.|
| --query                      | JMESPath queryreeks. Zie voor meer informatie http://jmespath.org/.|
| --uitgebreide                    | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-application-list"></a>lijst met sfctl-toepassingen
Hiermee wordt de lijst met toepassingen die in de Service Fabric-cluster gemaakt die overeenkomen met opgegeven als parameter filters opgehaald.

Hiermee haalt u de informatie over de toepassingen die zijn gemaakt of het proces wordt gemaakt in de Service Fabric-cluster en overeenkomen met filters opgegeven als parameter. Het antwoord bevat de naam, type, status, parameters en andere details over de toepassing. Als de toepassingen niet in een pagina passen, wordt één pagina met resultaten geretourneerd en een vervolgtoken dat kan worden gebruikt voor het ophalen van de volgende pagina. Filters ApplicationTypeName en ApplicationDefinitionKindFilter kunnen niet tegelijkertijd worden opgegeven.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
|--toepassing-definitie-type-filter| Gebruikt om te filteren op ApplicationDefinitionKind dit het mechanisme dat wordt gebruikt is voor het definiëren van een Service Fabric-toepassing. -Standaard - standaardwaarde, die voert dezelfde functie uit als u 'Alle' selecteren. De waarde is 0. -All - Filter dat overeenkomt met invoer met de waarde van een ApplicationDefinitionKind. De waarde is 65535. -ServiceFabricApplicationDescription - Filter dat overeenkomt met invoer met ApplicationDefinitionKind waarde ServiceFabricApplicationDescription. De waarde is 1. -Opstellen - Filter die overeenkomt met invoer met ApplicationDefinitionKind waarde opstellen. De waarde is 2.|
| --toepassing typenaam      | De naam van de toepassing type is gebruikt voor het filteren van de toepassingen om te zoeken. Deze waarde moet de versie van het toepassingstype niet bevatten.|
| --vervolgtoken         | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd.|
| --uitsluiten toepassingsparameters| De vlag die aangeeft of de toepassingsparameters zijn uitgesloten van het resultaat.|
| --max-results|Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal resultaten geretourneerd. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de groottebeperkingen max bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, wordt in de wisselbare query's zo veel resultaten bevatten mogelijk die in het retourbericht passen.|
| --time-out -t                 | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                      | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                    | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                  | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.             Standaard: json.|
| --query                      | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide                    | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-application-load"></a>sfctl toepassing werklast
Haalt informatie over een Service Fabric-toepassing worden geladen.

Retourneert de load-informatie over de toepassing die is gemaakt of die momenteel wordt gemaakt in het Service Fabric-cluster en waarvan de naam overeenkomt met de versie die is opgegeven als parameter. Het antwoord bevat de naam, knooppunten minimum, maximum aantal knooppunten, het aantal knooppunten dat de app is bepaald dat zich bevindt op dit moment en toepassing werklast metrische informatie over de toepassing.

### <a name="arguments"></a>Argumenten
|Argument|Beschrijving|
| --- | --- |
|--toepassing-id [vereist]| De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~ ' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric: / myapp/app1", zou de toepassings-id ' myapp ~ app1 ' in 6.0 + en "myapp/app1" in eerdere versies. |
| --time-out -t               | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten
|Argument|Beschrijving|
| --- | --- |
|--debug                    | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
    --help -h                  | Deze help-bericht en afsluiten weergeven.|
    --uitvoer -o                | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
    --query                    | JMESPath queryreeks. Zie voor meer informatie http://jmespath.org/.|
    --uitgebreide                  | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-application-manifest"></a>sfctl-toepassingsmanifest
Het manifest met een beschrijving van een toepassingstype opgehaald.
        
Het manifest met een beschrijving van een toepassingstype opgehaald. Het antwoord bevat de manifest-XML voor de toepassing als een tekenreeks.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| ----naam van het toepassingstype [vereist]| De naam van het toepassingstype.|
| ----versie van het toepassingstype [vereist]| De versie van het toepassingstype.|
| --time-out -t                      | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                           | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                         | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                       | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.                  Standaard: json.|
| --query                           | JMESPath queryreeks. Zie voor meer informatie http://jmespath.org/.|
| --uitgebreide                         | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-application-provision"></a>sfctl toepassing inrichten
Bepalingen of registers typt u een Service Fabric-toepassing met het cluster met behulp van het pakket SFPKG in de externe opslag of het gebruik van het toepassingspakket in de image store.

Voorziet in een type Service Fabric-toepassing met het cluster. Dit is vereist voordat de nieuwe toepassingen kunnen worden gemaakt. De inrichtingsbewerking kan worden uitgevoerd op het toepassingspakket dat is opgegeven met de relativePathInImageStore, of met behulp van de URI van de externe SFPKG. Tenzij--externe-provision is ingesteld, levert deze opdracht het toepassingspakket van de image store.
        


### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --application-package-download-uri| Het pad naar het toepassingspakket '.sfpkg' waar het toepassingspakket kan worden gedownload met behulp van HTTP of HTTPS-protocollen. Voor het inrichten van een externe winkel alleen. Het toepassingspakket kan worden opgeslagen in een externe winkel waarmee GET-bewerking om het bestand te downloaden. Ondersteunde protocollen zijn HTTP en HTTPS en het pad moet leestoegang toestaan.|
| ---type-build-pad naar toepassing       | Voor alleen richten kind image store. Het relatieve pad voor het toepassingspakket in de image store opgegeven tijdens de vorige uploadbewerking. |
| --toepassing typenaam| Voor het inrichten van een externe winkel alleen. De naam van het toepassingstype vertegenwoordigt de naam van het toepassingstype gevonden in het toepassingsmanifest.|
| ----versie van het toepassingstype| Voor het inrichten van een externe winkel alleen. De versie van het toepassingstype vertegenwoordigt de versie van het toepassingstype gevonden in het toepassingsmanifest.|
| --externe inrichten| De locatie vanaf waar toepassingspakket kan worden geregistreerd of ingericht. Geeft aan dat de voorziening voor een toepassingspakket dat eerder is geüpload naar een externe winkel. Het toepassingspakket eindigt met de extensie *.sfpkg.|
| --Er is geen wacht| Hiermee wordt aangegeven of inrichting asynchroon moet plaatsvinden.  Wanneer is ingesteld op true, retourneert de inrichtingsbewerking blijft wanneer de aanvraag wordt geaccepteerd door het systeem en de inrichtingsbewerking zonder een time-outlimiet. De standaardwaarde is ingesteld op false. Voor grote toepassingpakketten, wordt u aangeraden de waarde instelt op true.|
| --time-out -t                      | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                              | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                            | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                          | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query                              | JMESPath queryreeks. Zie voor meer informatie http://jmespath.org/.|
| --uitgebreide                            | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-application-type"></a>toepassingstype sfctl

Hiermee haalt de lijst met soorten toepassingen in de Service Fabric-cluster die overeenkomt met de opgegeven naam.

Retourneert de gegevens over de toepassingstypen die zijn ingericht of die momenteel wordt ingericht in het Service Fabric-cluster. Deze resultaten zijn van toepassingstypen waarvan de naam overeenkomt met een opgegeven als parameter, en die voldoen aan de opgegeven queryparameters. Alle versies van het type van de toepassing die overeenkomt met de naam van het toepassingstype worden geretourneerd met elke versie die wordt geretourneerd als een toepassingstype. Het antwoord bevat de naam, versie, status en andere details over het toepassingstype. Dit is een paged query, wat betekent dat als dat niet alle toepassingstypen in een pagina past één pagina met resultaten geretourneerd en een vervolgtoken dat kan worden gebruikt voor het ophalen van de volgende pagina. Bijvoorbeeld, als er 10 toepassingstypen maar een pagina past alleen de eerste 3 toepassingstypen of als max resultaten zijn ingesteld op 3, wordt 3 geretourneerd. Voor toegang tot de rest van de resultaten moet de volgende pagina's met behulp van de geretourneerde vervolgtoken in de volgende query worden opgehaald. Een leeg vervolgtoken wordt geretourneerd als er geen verdere's zijn.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| ----naam van het toepassingstype [vereist]| De naam van het toepassingstype.|
| ----versie van het toepassingstype        | De versie van het toepassingstype.|
| --vervolgtoken           | De token voortzetting-parameter wordt gebruikt voor het verkrijgen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in een enkele antwoordthread passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep, retourneert de API volgende reeks resultaten. Als er geen verdere resultaten, bevat klikt u vervolgens het vervolgtoken geen waarde. De waarde van deze parameter mag geen URL zijn gecodeerd.|
| --uitsluiten toepassingsparameters  | De vlag die aangeeft of de parameters voor de toepassing zal worden uitgesloten van het resultaat.|
| --max-results                  | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal resultaten geretourneerd. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de groottebeperkingen max bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de paged query zo veel resultaten mogelijk die in het retourbericht passen.|
| --time-out -t                   | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                        | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                      | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                    | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.               Standaard: json.|
| --query                        | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide                      | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-application-unprovision"></a>sfctl toepassing verwijderen
Verwijdert of heft de registratie van een Service Fabric-toepassingstype uit het cluster.

Verwijdert of heft de registratie van een Service Fabric-toepassingstype uit het cluster. Deze bewerking kan alleen worden uitgevoerd als alle toepassingsexemplaar van het toepassingstype is verwijderd. Zodra het toepassingstype geregistreerd is, kan geen nieuwe instantie van de toepassing worden gemaakt voor dit type bepaalde toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| ----naam van het toepassingstype [vereist]| De naam van het toepassingstype.|
| ----versie van het toepassingstype [vereist]| De versie van het toepassingstype, zoals gedefinieerd in het toepassingsmanifest.|
|--async-parameter                    | De vlag die aangeeft of inrichting verwijderen asynchroon moet plaatsvinden. Als is ingesteld op true, wordt de bewerking verwijderen retourneert wanneer de aanvraag wordt geaccepteerd door het systeem en de bewerking verwijderen wordt voortgezet zonder een time-outlimiet. De standaardwaarde is ingesteld op false. We raden echter aan dat u deze instellen op true voor grote toepassingspakketten die zijn ingericht.|
| --time-out -t                      | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                           | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                         | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                       | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.                  Standaard: json.|
| --query                           | JMESPath queryreeks. Zie voor meer informatie http://jmespath.org/.|
| --uitgebreide                         | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-application-upgrade"></a>upgrade van de toepassing sfctl
Hiermee start u een upgrade van een toepassing in het Service Fabric-cluster.

Valideert de parameters voor het bijwerken van opgegeven toepassing en start de toepassing bijwerken als de parameters geldig zijn. Beschrijving van de upgrade vervangt de beschrijving van de bestaande toepassing. Dit betekent dat als de parameters niet zijn opgegeven, worden de bestaande parameters van de toepassingen met de parameterlijst leeg overschreven. Dit resulteert in een toepassing met behulp van de standaardwaarde van de parameters van het toepassingsmanifest.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --app-id [vereist]| De identiteit van de toepassing. Dit wordt meestal de volledige naam van de toepassing zonder de "fabric:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden door het ' ~' teken. Als de toepassingsnaam van de is bijvoorbeeld "fabric: / myapp/app1 ', zou de toepassings-id ' myapp ~ app1' in 6.0 + en ' myapp/app1' in eerdere versies.|
| --app-versie [vereist]| Versie van de doel-toepassing.|
| --parameters [vereist]| Een lijst met JSON gecodeerde van toepassing parameter onderdrukkingen om te worden toegepast bij de upgrade van de toepassing.|
| ---service-health-standaardbeleid| JSON gecodeerde specificatie van het statusbeleid gebruikt standaard voor het evalueren van de status van een servicetype.|
| --actie bij fout            | De actie om uit te voeren wanneer de upgrade van een bewaakte bewaking beleid of health beleidsschendingen optreedt.|
| --force-restart             | Start processen geforceerd opnieuw tijdens de upgrade, zelfs wanneer de codeversie is niet gewijzigd.|
| --health-check-retry-timeout| De hoeveelheid tijd health evaluaties opnieuw wanneer de toepassing of het cluster slecht vóór de fout actie is wordt uitgevoerd. Gemeten in milliseconden.  Standaard: PT0H10M0S.|
| --health selectievakje-stabiele duur | De hoeveelheid tijd dat de toepassing of het cluster in orde blijven moet voordat de upgrade wordt uitgevoerd op het volgende upgradedomein.            Gemeten in milliseconden.  Default: PT0H2M0S.|
| --health-controle-wait-duur| De hoeveelheid tijd moet worden gewacht na het voltooien van een upgradedomein alvorens statusbeleid toe te passen. Gemeten in milliseconden.            Standaard: 0.|
| --max-slecht-apps        | Het maximaal toegestane percentage van de beschadigde geïmplementeerde toepassingen. Weergegeven als een getal tussen 0 en 100 liggen.|
| --modus                      | De modus voor de status bewaakt tijdens een rolling upgrade.            Standaard: UnmonitoredAuto.|
| --replica-set-check-timeout | De maximale hoeveelheid tijd voor het blokkeren van de verwerking van een upgradedomein en verlies van beschikbaarheid voorkomen wanneer er onverwachte problemen zijn. Gemeten in seconden.|
| --service statusbeleid     | JSON gecodeerde kaart met het service type statusbeleid per type servicenaam. De kaart is leeg worden standaard.|
| --time-out -t                | Server time-out in seconden.  Standaard: 60.|
| --upgrade-domain-timeout    | De hoeveelheid tijd elk upgradedomein heeft voltooid voordat FailureAction wordt uitgevoerd. Gemeten in milliseconden.  Default:            P10675199DT02H48M05.4775807S.|
| --upgrade-timeout           | De hoeveelheid tijd die de algehele upgrade heeft voltooid voordat FailureAction wordt uitgevoerd. Gemeten in milliseconden.  Default:            P10675199DT02H48M05.4775807S.|
| --waarschuwing als fout          | Evaluatie van waarschuwingen met de dezelfde ernst als fouten behandelen.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug                     | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h                   | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o                 | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.            Standaard: json.|
| --query                     | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide                   | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-application-upload"></a>sfctl toepassing uploaden
Een Service Fabric-toepassing-pakket kopiëren naar de image store.

Bezig met uploaden voor elk bestand eventueel weergeven in het pakket. Uploaden voortgang wordt verzonden naar `stderr`.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --pad [vereist]| Pad naar lokale toepassingspakket.|
|--installatiekopieopslag-tekenreeks| De installatiekopie van het doel voor het uploaden van het toepassingspakket om te worden opgeslagen.  Standaardwaarde: fabric: Installatiekopieopslag.|
| --weergeven uitgevoerd  | Bestand uploadvoortgang voor grote pakketten weergeven.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug       | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h     | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o   | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query       | JMESPath queryreeks. Zie voor meer informatie http://jmespath.org/.|
| --uitgebreide     | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="next-steps"></a>Volgende stappen
- [Setup](service-fabric-cli.md) de Service Fabric CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).
