---
title: Azure Service Fabric CLI - sfctl-toepassing | Microsoft Docs
description: Beschrijving van de opdrachten van Service Fabric-CLI sfctl toepassing.
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
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 0f608dc89d3a9bc8914fc9be142c442246ce13b5
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53278539"
---
# <a name="sfctl-application"></a>sfctl application
Maken, verwijderen en beheren van toepassingen en de soorten toepassingen.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| maken | Hiermee maakt u een Service Fabric-toepassing met behulp van de opgegeven beschrijving. |
| delete | Hiermee verwijdert u een bestaande Service Fabric-toepassing. |
| geïmplementeerd | Hiermee haalt u de informatie over een toepassing is geïmplementeerd op een Service Fabric-knooppunt. |
| -en statusbewaking geïmplementeerd | Hiermee haalt u de informatie over de status van een toepassing is geïmplementeerd op een Service Fabric-knooppunt. |
| lijst met geïmplementeerd | Hiermee haalt u de lijst met toepassingen die zijn geïmplementeerd op een Service Fabric-knooppunt. |
| gezondheidszorg | Hiermee haalt u de status van de service fabric-toepassing. |
| informatie | Hiermee haalt u informatie over een Service Fabric-toepassing. |
| list | Hiermee haalt u de lijst met toepassingen die in de Service Fabric-cluster gemaakt die overeenkomen met de opgegeven filters. |
| laden | Haalt informatie over een Service Fabric-toepassing te laden. |
| Het manifest | Hiermee haalt u het manifest met een beschrijving van een toepassingstype. |
| inrichten | Bepalingen of registers typt u een Service Fabric-toepassing met het cluster met behulp van het pakket .sfpkg in de externe opslag of het toepassingspakket in archief van de installatiekopie. |
| rapport-en statusbewaking | Verzendt een statusrapport over de Service Fabric-toepassing. |
| type | Hiermee haalt u de lijst met typen in het Service Fabric-cluster die overeenkomt met precies de opgegeven naam. |
| type-lijst | Hiermee haalt de lijst met typen in het Service Fabric-cluster. |
| inrichting verwijderen | Hiermee verwijdert u of de registratie van een type Service Fabric-toepassing uit het cluster. |
| upgrade | Hiermee start u een upgrade van een toepassing in de Service Fabric-cluster. |
| upgrade-hervatten | Hervat een upgrade van een toepassing in de Service Fabric-cluster. |
| upgrade terugdraaien | Start het terugdraaien van de momenteel continue upgrade van een toepassing in de Service Fabric-cluster. |
| upgrade-status | Hiermee haalt details voor de meest recente upgrade uitgevoerd voor deze toepassing. |
| uploaden | Een Service Fabric-toepassingspakket kopiëren naar de installatiekopieopslag. |

## <a name="sfctl-application-create"></a>sfctl-toepassing maken
Hiermee maakt u een Service Fabric-toepassing met behulp van de opgegeven beschrijving.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --app-naam (vereist) | De naam van de toepassing, met inbegrip van de ' fabric\:' URI-schema. |
| --app-type (vereist) | De naam van de toepassing type is gevonden in het toepassingsmanifest. |
| --app-versie (vereist) | De versie van het toepassingstype zoals gedefinieerd in het toepassingsmanifest. |
| --max-node-count | Het maximale aantal knooppunten dat is waar Service Fabric-capaciteit voor deze toepassing wordt gereserveerd. Houd er rekening mee dat dit niet betekent dat de services van deze toepassing op alle knooppunten wordt geplaatst. |
| --metrische gegevens | Een JSON gecodeerd lijst van de toepassing capaciteit metrische beschrijvingen. Een metrische waarde wordt gedefinieerd als een naam, die zijn gekoppeld aan een set met capaciteiten voor elk knooppunt dat de toepassing bestaat op. |
| --min in het aantal knooppunten | Het minimum aantal knooppunten dat is waar Service Fabric-capaciteit voor deze toepassing wordt gereserveerd. Houd er rekening mee dat dit niet betekent dat de services van deze toepassing op alle knooppunten wordt geplaatst. |
| --parameters | Een lijst met JSON gecodeerd van de parameter van de toepassing onderdrukt bij het maken van de toepassing moet worden toegepast. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-delete"></a>sfctl toepassing verwijderen
Hiermee verwijdert u een bestaande Service Fabric-toepassing.

Een toepassing moet worden gemaakt voordat deze kan worden verwijderd. Een toepassing verwijdert, worden alle services die deel van die toepassing uitmaken. Standaard probeert Service Fabric te sluiten van service-replica's op correcte wijze en verwijder vervolgens de service. Echter als een service is hebt u problemen bij het sluiten van de replica zonder problemen, de delete-bewerking lang kan duren of zitten. Gebruik de optionele ForceRemove vlag de correcte sluiten van een reeks overslaan en de toepassing en alle bijbehorende services geforceerd verwijderen.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --force-remove | Een Service Fabric-toepassing of service verwijderen geforceerd zonder tussenkomst van de reeks correct afsluiten. Deze parameter kan worden gebruikt voor het geforceerd verwijderen van een toepassing of service voor welke verwijderen time-out vanwege problemen met de code die voorkomt dat vensters sluit van replica's. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-deployed"></a>sfctl toepassing die is geïmplementeerd
Hiermee haalt u de informatie over een toepassing is geïmplementeerd op een Service Fabric-knooppunt.

Deze query retourneert systeemgegevens van de toepassing als de toepassings-ID opgegeven voor de systeemtoepassing is. Resultaten omvatten geïmplementeerde toepassingen in actieve, activeren en downloaden van Staten. Deze query is vereist dat de knooppuntnaam komt met een knooppunt in het cluster overeen. De query is mislukt als de naam van het opgegeven knooppunt wijst niet naar een actieve Service Fabric-knooppunten op het cluster.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --opnemen-health-status | De status van een entiteit bevatten. Als deze parameter of niet opgegeven is, wordt de status van de geretourneerde 'Onbekend'. Wanneer is ingesteld op true, wordt de query parallel om in het knooppunt en de system health service gaat voordat de resultaten worden samengevoegd. Als gevolg hiervan de query is duurder en een langere tijd kan duren. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-deployed-health"></a>sfctl geïmplementeerd-status van de toepassing
Hiermee haalt u de informatie over de status van een toepassing is geïmplementeerd op een Service Fabric-knooppunt.

Hiermee haalt u de informatie over de status van een toepassing is geïmplementeerd op een Service Fabric-knooppunt. Gebruik EventsHealthStateFilter om te filteren op (optioneel) de verzameling van HealthEvent-objecten die zijn gerapporteerd voor de geïmplementeerde toepassing die is gebaseerd op de status. Gebruik DeployedServicePackagesHealthStateFilter om te filteren op (optioneel) DeployedServicePackageHealth onderliggende items die zijn gebaseerd op de status.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --Deployed-service-Packages-Health-State-filter | U kunt filteren van de geïmplementeerde service pakket health state objecten geretourneerd in het resultaat van de geïmplementeerde toepassing health query op basis van hun status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen geïmplementeerd service pakketten die overeenkomen met het filter worden geretourneerd. Alle geïmplementeerde servicepakketten worden gebruikt voor het evalueren van de cumulatieve status van de geïmplementeerde toepassing. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden, verkregen met behulp van de bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde is 6 status van service-pakketten met HealthState waarde OK (2) en waarschuwing (4) zijn teruggekeerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling van HealthEvent-objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden, verkregen met behulp van de bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde 6 is worden vervolgens alle gebeurtenissen met HealthState waarde OK (2) en waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --uitsluiten-health-statistieken | Geeft aan of de health-statistieken moet worden geretourneerd als onderdeel van het queryresultaat. Als de waarde False is, wordt standaard. De statistieken Toon het aantal onderliggende entiteiten in de status Ok, waarschuwing en fout. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-deployed-list"></a>sfctl geïmplementeerd-lijst met toepassingen
Hiermee haalt u de lijst met toepassingen die zijn geïmplementeerd op een Service Fabric-knooppunt.

Hiermee haalt u de lijst met toepassingen die zijn geïmplementeerd op een Service Fabric-knooppunt. De resultaten bevatten geen informatie over het geïmplementeerde systeemtoepassingen, tenzij expliciet een query uitgevoerd voor op ID. Resultaten omvatten geïmplementeerde toepassingen in actieve, activeren en downloaden van Staten. Deze query is vereist dat de knooppuntnaam komt met een knooppunt in het cluster overeen. De query is mislukt als de naam van het opgegeven knooppunt wijst niet naar een actieve Service Fabric-knooppunten op het cluster.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --knooppuntnaam (vereist) | De naam van het knooppunt. |
| --vervolgtoken | De voortzetting van token-parameter wordt gebruikt om op te halen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in één antwoord passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep retourneert de API volgende set met resultaten. Als er geen verdere resultaten, klikt u vervolgens bevat het vervolgtoken een waarde. De waarde van deze parameter mag geen URL-codering. |
| --opnemen-health-status | De status van een entiteit bevatten. Als deze parameter of niet opgegeven is, wordt de status van de geretourneerde 'Onbekend'. Wanneer is ingesteld op true, wordt de query parallel om in het knooppunt en de system health service gaat voordat de resultaten worden samengevoegd. Als gevolg hiervan de query is duurder en een langere tijd kan duren. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal geretourneerde resultaten. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de beperkingen van de grootte van maximaal bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de query met resultatenpagina's zo veel resultaten als is mogelijk dat de geretourneerde bericht. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-health"></a>status van de toepassing sfctl
Hiermee haalt u de status van de service fabric-toepassing.

Retourneert de Health-status van de service fabric-toepassing. Het antwoord rapporteert de status Ok, fout of waarschuwing. Als de entiteit niet in health store gevonden is, wordt deze fout geretourneerd.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --geïmplementeerd-toepassingen-health-status-filter | Kan het filteren van de geïmplementeerde toepassingen health state objecten geretourneerd in het resultaat van application health-query op basis van hun status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen geïmplementeerde toepassingen die overeenkomen met het filter wordt geretourneerd. Alle geïmplementeerde toepassingen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden, verkregen met behulp van bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde is 6 status van geïmplementeerde toepassingen met HealthState waarde OK (2) en waarschuwing (4) zijn teruggekeerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --gebeurtenissen-health-status-filter | Hiermee kunt u filteren van de verzameling van HealthEvent-objecten geretourneerd op basis van status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen de gebeurtenissen die overeenkomen met het filter worden geretourneerd. Alle gebeurtenissen worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden, verkregen met behulp van de bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde 6 is worden vervolgens alle gebeurtenissen met HealthState waarde OK (2) en waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --uitsluiten-health-statistieken | Geeft aan of de health-statistieken moet worden geretourneerd als onderdeel van het queryresultaat. Als de waarde False is, wordt standaard. De statistieken Toon het aantal onderliggende entiteiten in de status Ok, waarschuwing en fout. |
| --services-health-status-filter | Kan het filteren van de services health state objecten geretourneerd in het resultaat van de query voor health services op basis van hun status. De mogelijke waarden voor deze parameter zijn integer-waarde van een van de volgende statussen. Alleen services die overeenkomen met het filter worden geretourneerd. Alle services worden gebruikt voor het evalueren van de geaggregeerde status. Als niet is opgegeven, worden alle vermeldingen geretourneerd. De provincie-waarden zijn vlag gebaseerde inventarisatie, zodat de waarde kan bestaan uit een combinatie van deze waarden, verkregen met behulp van bitwise "OR"-operator. Bijvoorbeeld, als de opgegeven waarde 6 is wordt vervolgens de status van services met HealthState waarde OK (2) en waarschuwing (4) geretourneerd.  <br> -Standaard - standaardwaarde. Komt overeen met alle HealthState. De waarde is nul.  <br> -Geen - Filter op dat komt niet overeen met een willekeurige waarde HealthState. Er zijn geen resultaten geretourneerd bij een bepaalde verzameling van statussen gebruikt. De waarde is 1.  <br> -Ok - filteren dat overeenkomt met op de Ok invoer met HealthState waarde. De waarde is 2.  <br> -Waarschuwing - Filter dat overeenkomt met invoer aan HealthState waarschuwing waarde. De waarde is 4.  <br> -Fout: Filter die overeenkomt met de invoer met HealthState waarde fout. De waarde is 8.  <br> -Alle - Filter op dat overeenkomt met de invoer met een willekeurige waarde HealthState. De waarde is 65535. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-info"></a>sfctl toepassingsinfo
Hiermee haalt u informatie over een Service Fabric-toepassing.

Retourneert de gegevens over de toepassing die is gemaakt of momenteel wordt gemaakt in de Service Fabric-cluster en waarvan de naam overeenkomt met de versie die is opgegeven als parameter. Het antwoord bevat de naam, type, status, parameters en andere details over de toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --uitsluiten toepassingsparameters | De vlag die aangeeft of de parameters voor de toepassing, worden uitgesloten van het resultaat. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-list"></a>lijst met sfctl-toepassingen
Hiermee haalt u de lijst met toepassingen die in de Service Fabric-cluster gemaakt die overeenkomen met de opgegeven filters.

Hiermee haalt u de informatie over de toepassingen die zijn gemaakt of momenteel wordt gemaakt in de Service Fabric-cluster en overeenkomen met de opgegeven filters. Het antwoord bevat de naam, type, status, parameters en andere details over de toepassing. Als de toepassingen niet in een pagina passen, wordt een pagina met resultaten geretourneerd en een vervolgtoken, die kan worden gebruikt om de volgende pagina. Alleen ApplicationTypeName filters en ApplicationDefinitionKindFilter kan niet tegelijkertijd worden opgegeven.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --toepassing-definition-type-filter | Gebruikt om te filteren op ApplicationDefinitionKind, dit is het mechanisme dat wordt gebruikt voor het definiëren van een Service Fabric-toepassing.  <br> -Standaard - standaardwaarde, die dezelfde functie als 'Alle' selecteren. De waarde is 0.  <br> -Alle - filteren die overeenkomt met de invoer met een willekeurige waarde ApplicationDefinitionKind. De waarde is 65535.  <br> -ServiceFabricApplicationDescription - Filter op dat overeenkomt met de invoer met ApplicationDefinitionKind waarde ServiceFabricApplicationDescription. De waarde is 1.  <br> -Compose - Filter op dat overeenkomt met de invoer met ApplicationDefinitionKind waarde opstellen. De waarde is 2. |
| ----naam van het toepassingstype | De naam van de toepassing type is gebruikt voor het filteren van de toepassingen op te vragen voor. Deze waarde moet een versie van het toepassingstype niet bevatten. |
| --vervolgtoken | De voortzetting van token-parameter wordt gebruikt om op te halen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in één antwoord passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep retourneert de API volgende set met resultaten. Als er geen verdere resultaten, klikt u vervolgens bevat het vervolgtoken een waarde. De waarde van deze parameter mag geen URL-codering. |
| --uitsluiten toepassingsparameters | De vlag die aangeeft of de parameters voor de toepassing, worden uitgesloten van het resultaat. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal geretourneerde resultaten. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de beperkingen van de grootte van maximaal bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de query met resultatenpagina's zo veel resultaten als is mogelijk dat de geretourneerde bericht. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-load"></a>sfctl laden
Haalt informatie over een Service Fabric-toepassing te laden.

Retourneert de load-informatie over de toepassing die is gemaakt of momenteel wordt gemaakt in de Service Fabric-cluster en waarvan de naam overeenkomt met de versie die is opgegeven als parameter. Het antwoord bevat de naam, minimum aantal knooppunten, maximum aantal knooppunten, het aantal knooppunten dat de toepassing is momenteel bezet en toepassing laden metrische gegevens over de toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-manifest"></a>sfctl-toepassingsmanifest
Hiermee haalt u het manifest met een beschrijving van een toepassingstype.

Het antwoord bevat de XML-manifest van de toepassing als een tekenreeks.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --toepassing-type-naam (vereist) | De naam van het toepassingstype. |
| --toepassing-type-versie (vereist) | De versie van het toepassingstype. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-provision"></a>sfctl toepassing inrichten
Bepalingen of registers typt u een Service Fabric-toepassing met het cluster met behulp van het pakket .sfpkg in de externe opslag of het toepassingspakket in archief van de installatiekopie.

Richt een type Service Fabric-toepassing met het cluster. Dit is vereist voordat een nieuwe toepassingen kunnen worden gemaakt. De bewerking voor het inrichten kan worden uitgevoerd op het toepassingspakket dat is opgegeven door de relativePathInImageStore, of met behulp van de URI van de externe .sfpkg. Tenzij--externe inrichten is ingesteld, wordt met deze opdracht verwacht dat de installatiekopie store inrichten.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --application-pakket-download-uri | Het pad naar het toepassingspakket '.sfpkg' waaruit het toepassingspakket kan worden gedownload met behulp van HTTP of HTTPS-protocol. <br><br> Voor inrichten van het type externe opslaan alleen. Het toepassingspakket kan worden opgeslagen in een externe opslag waarmee GET-bewerking om het bestand te downloaden. Ondersteunde protocollen zijn HTTP en HTTPS en het pad moet leestoegang toestaan. |
| ---type-build-pad naar toepassing | Voor alleen richten installatiekopie van het soort archief. Het relatieve pad voor het toepassingspakket in het archief van de installatiekopie opgegeven tijdens het uploaden van eerdere. |
| ----naam van het toepassingstype | Voor inrichten van het type externe opslaan alleen. De naam van de toepassing type vertegenwoordigt de naam van het toepassingstype gevonden in het toepassingsmanifest. |
| --versie-type-toepassing | Voor inrichten van het type externe opslaan alleen. De versie van het type toepassing geeft de versie van het toepassingstype gevonden in het toepassingsmanifest. |
| --externe inrichten | De locatie waar toepassingspakket kan worden geregistreerd of ingericht. Geeft aan dat de levering van een toepassingspakket dat eerder is geüpload naar een externe opslag. Het toepassingspakket eindigt met de extensie *.sfpkg. |
| --niet-wait | Geeft aan of inrichting asynchroon moet plaatsvinden. <br><br> Wanneer is ingesteld op true, de bewerking voor het inrichten wordt geretourneerd blijft wanneer de aanvraag is geaccepteerd door het systeem en de bewerking voor het inrichten zonder een time-outlimiet. De standaardwaarde is false. Voor grote toepassingpakketten raden wij de waarde wordt ingesteld op true. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-report-health"></a>sfctl toepassing rapport-en statusbewaking
Verzendt een statusrapport over de Service Fabric-toepassing.

Rapporteert de status van de opgegeven Service Fabric-toepassing. Het rapport moet de informatie over de oorzaak van de health-rapport en de eigenschap waarop dit apparaat is gerapporteerd bevatten. Het rapport wordt verzonden naar een gateway Service Fabric-toepassing, die wordt doorgestuurd naar de health-store. Het rapport kan worden geaccepteerd door de gateway, maar geweigerd door de health store na extra validatie. Het rapport kan bijvoorbeeld afwijzen, de health store vanwege een ongeldige parameter, zoals een verouderde volgnummer. Status van de toepassing ophalen om te zien of het rapport in de health-store is toegepast, en controleer of het rapport wordt weergegeven.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. <br><br> Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id ' Mijntoep\~app1' in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --health-eigenschap (vereist) | De eigenschap van de gegevens over de servicestatus. <br><br> Een entiteit kan statusrapporten voor de verschillende eigenschappen hebben. De eigenschap is een tekenreeks en niet een vaste-opsomming waarmee de journalist flexibiliteit voor het categoriseren van de voorwaarde staat dat het rapport wordt geactiveerd. Bijvoorbeeld, een Rapportagefout met SourceId "LocalWatchdog" die de status van de beschikbare schijfruimte op een knooppunt kunt controleren, zodat deze eigenschap "AvailableDisk" op dat knooppunt rapporteren kunt. De dezelfde journalist kan het knooppunt-connectiviteit, bewaken, zodat het een eigenschap 'Connectiviteit' op hetzelfde knooppunt rapporteren kan. In de winkel de gezondheid van worden deze rapporten behandeld als afzonderlijke health-gebeurtenissen voor het opgegeven knooppunt. Samen met de bron-id identificatie de eigenschap unieke van de gegevens over de servicestatus. |
| --status (vereist) | Mogelijke waarden zijn\: 'Ongeldige', 'Ok', 'Waarschuwing', 'Fout', 'Onbekend'. |
| --bron-id (vereist) | De naam van de bron die de watchdog-client-systeem component aangeduid die de gezondheidsinformatie gegenereerd. |
| --Beschrijving | De beschrijving van de gegevens over de servicestatus. <br><br> Hiermee geeft u vrije tekst die wordt gebruikt om toe te voegen menselijke leesbare informatie over het rapport. De maximumlengte voor de beschrijving is 4096 tekens. Als de opgegeven tekenreeks langer is, worden deze automatisch afgekapt. Wanneer afgekapt, de laatste tekens van de beschrijving van de markering van een '[Truncated]' bevatten en totale grootte is 4096 tekens. De aanwezigheid van de markering geeft aan dat gebruikers die moet worden afgekapt is opgetreden. Houd er rekening mee dat wanneer afgekapt, de beschrijving van minder dan 4096 tekens uit de oorspronkelijke reeks heeft. |
| --direct | Een vlag die aangeeft of het rapport direct moet worden verzonden. <br><br> Een statusrapport wordt verzonden naar een gateway Service Fabric-toepassing, die wordt doorgestuurd naar de health-store. Als direct is ingesteld op true, het rapport wordt direct verzonden van HTTP-Gateway naar de health-store, ongeacht de instellingen van de fabric-client die de HTTP-Gateway-toepassing wordt gebruikt. Dit is handig voor de kritieke rapporten die zo snel mogelijk moeten worden verzonden. Afhankelijk van de timing en andere voorwaarden mislukken verzenden van het rapport nog steeds, bijvoorbeeld als de HTTP-Gateway is gesloten of het bericht de Gateway niet bereiken. Als direct is ingesteld op false, wordt het rapport verzonden op basis van de clientinstellingen voor de status van de HTTP-Gateway. Het wordt daarom worden batchgewijs op basis van de configuratie van de HealthReportSendInterval. Dit is de aanbevolen instelling omdat hierdoor de health-client de gezondheid van berichten in health store, evenals verwerking van statusrapporten reporting optimaliseren. Standaard rapporten niet onmiddellijk verzonden. |
| --verwijderen wanneer verlopen | De waarde die aangeeft of het rapport is verwijderd uit health store wanneer het verloopt. <br><br> Als is ingesteld op true, wordt het rapport wordt verwijderd uit de store health nadat deze is verlopen. Indien ingesteld op false, het rapport wordt behandeld als een fout bij het verlopen. De waarde van deze eigenschap is ingesteld op false standaard. Als clients regelmatig rapporteren, moeten ze RemoveWhenExpired false (standaard) ingesteld. Op deze manier is de journalist heeft problemen (bijvoorbeeld impasse) en kan niet rapporteren, wordt de entiteit bij fout wordt geëvalueerd wanneer het statusrapport is verlopen. Deze vlaggen die de entiteit alsof ze de status fout. |
| --volgnummer: | Het volgnummer voor dit statusrapport als een numerieke tekenreeks. <br><br> Het volgnummer van het rapport wordt gebruikt door de health store voor het detecteren van verouderde rapporten. Indien niet opgegeven, is een volgnummer automatisch gegenereerd door de client health wanneer een rapport wordt toegevoegd. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --ttl | De tijdsduur waarvoor deze statusrapport geldig is. Dit veld wordt ISO8601-notatie gebruikt voor het opgeven van de duur. <br><br> Als clients regelmatig rapporteren, moeten ze rapporten verzenden met een hogere frequentie dan time to live van. Als clients een over de overgang rapport, kunnen ze time to live op oneindig ingesteld. Wanneer time to live van is verlopen, de statusgebeurtenis met de health-gegevens is verwijderd uit health store, als RemoveWhenExpired true, of op een fout, geëvalueerd als onwaar RemoveWhenExpired. Indien niet opgegeven, time to live van oneindige waarde de standaardwaarde van. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-type"></a>sfctl toepassingstype
Hiermee haalt u de lijst met typen in het Service Fabric-cluster die overeenkomt met precies de opgegeven naam.

Retourneert de gegevens over de soorten toepassingen die zijn ingericht of momenteel wordt ingericht in de Service Fabric-cluster. Deze resultaten zijn van de soorten toepassingen waarvan de naam overeenkomt met precies de opgegeven als de parameter en die voldoen aan de opgegeven queryparameters. Alle versies van het type van de toepassing die overeenkomt met de naam van de toepassing-type worden geretourneerd, met elke versie die wordt geretourneerd als een toepassingstype. Het antwoord bevat de naam, versie, status en andere informatie over het toepassingstype. Dit is een gepagineerde query, wat betekent dat als dat niet alle typen van de toepassing in een pagina passen wordt één pagina met resultaten geretourneerd en een vervolgtoken, die kan worden gebruikt om de volgende pagina. Bijvoorbeeld, wordt als er 10 toepassingstypen, maar een pagina past alleen de eerste drie typen, of als het maximum aantal resultaten is ingesteld op 3, klikt u vervolgens drie geretourneerd. Voor toegang tot de rest van de resultaten, de volgende pagina's met behulp van het geretourneerde vervolgtoken in de volgende query worden opgehaald. Een lege vervolgtoken geretourneerd als er geen volgende pagina's.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --toepassing-type-naam (vereist) | De naam van het toepassingstype. |
| --versie-type-toepassing | De versie van het toepassingstype. |
| --vervolgtoken | De voortzetting van token-parameter wordt gebruikt om op te halen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in één antwoord passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep retourneert de API volgende set met resultaten. Als er geen verdere resultaten, klikt u vervolgens bevat het vervolgtoken een waarde. De waarde van deze parameter mag geen URL-codering. |
| --uitsluiten toepassingsparameters | De vlag die aangeeft of de parameters voor de toepassing, worden uitgesloten van het resultaat. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal geretourneerde resultaten. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de beperkingen van de grootte van maximaal bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de query met resultatenpagina's zo veel resultaten als is mogelijk dat de geretourneerde bericht. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-type-list"></a>sfctl-lijst van toepassing
Hiermee haalt de lijst met typen in het Service Fabric-cluster.

Retourneert de gegevens over de soorten toepassingen die zijn ingericht of momenteel wordt ingericht in de Service Fabric-cluster. Elke versie van een toepassingstype wordt geretourneerd als een toepassingstype. Het antwoord bevat de naam, versie, status en andere informatie over het toepassingstype. Dit is een gepagineerde query, wat betekent dat als dat niet alle typen van de toepassing in een pagina passen wordt één pagina met resultaten geretourneerd en een vervolgtoken, die kan worden gebruikt om de volgende pagina. Bijvoorbeeld, wordt als er 10 toepassingstypen, maar een pagina past alleen de eerste drie typen, of als het maximum aantal resultaten is ingesteld op 3, klikt u vervolgens drie geretourneerd. Voor toegang tot de rest van de resultaten, de volgende pagina's met behulp van het geretourneerde vervolgtoken in de volgende query worden opgehaald. Een lege vervolgtoken geretourneerd als er geen volgende pagina's.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --toepassing-type-definition-type-filter | Gebruikt om te filteren op ApplicationTypeDefinitionKind dit is het mechanisme dat wordt gebruikt voor het definiëren van een type Service Fabric-toepassing.  <br> -Standaard - standaardwaarde, die dezelfde functie als 'Alle' selecteren. De waarde is 0.  <br> -Alle - filteren die overeenkomt met de invoer met een willekeurige waarde ApplicationTypeDefinitionKind. De waarde is 65535.  <br> -ServiceFabricApplicationPackage - Filter op dat overeenkomt met de invoer met ApplicationTypeDefinitionKind waarde ServiceFabricApplicationPackage. De waarde is 1.  <br> -Compose - Filter op dat overeenkomt met de invoer met ApplicationTypeDefinitionKind waarde opstellen. De waarde is 2. |
| --vervolgtoken | De voortzetting van token-parameter wordt gebruikt om op te halen van de volgende set resultaten. Een vervolgtoken met een niet-lege waarde is opgenomen in het antwoord van de API wanneer de resultaten van het systeem niet in één antwoord passen. Wanneer deze waarde wordt doorgegeven aan de volgende API-aanroep retourneert de API volgende set met resultaten. Als er geen verdere resultaten, klikt u vervolgens bevat het vervolgtoken een waarde. De waarde van deze parameter mag geen URL-codering. |
| --uitsluiten toepassingsparameters | De vlag die aangeeft of de parameters voor de toepassing, worden uitgesloten van het resultaat. |
| --max-resultaten | Het maximum aantal resultaten moeten worden geretourneerd als onderdeel van de wisselbare query's. Deze parameter bepaalt de bovengrens van het aantal geretourneerde resultaten. De resultaten kan worden kleiner zijn dan het opgegeven maximum aantal resultaten als ze niet in het bericht aan de hand van de beperkingen van de grootte van maximaal bericht passen gedefinieerd in de configuratie. Als deze parameter nul is of niet is opgegeven, bevat de query met resultatenpagina's zo veel resultaten als is mogelijk dat de geretourneerde bericht. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-unprovision"></a>sfctl application unprovision
Hiermee verwijdert u of de registratie van een type Service Fabric-toepassing uit het cluster.

Deze bewerking kan alleen worden uitgevoerd als alle toepassingsexemplaren van het type zijn verwijderd. Zodra het toepassingstype ongedaan is gemaakt, kunnen er geen nieuwe toepassingsexemplaren worden gemaakt voor dit type bepaalde toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --toepassing-type-naam (vereist) | De naam van het toepassingstype. |
| --toepassing-type-versie (vereist) | De versie van het toepassingstype zoals gedefinieerd in het toepassingsmanifest. |
| --async-parameter | De vlag die aangeeft of unprovision asynchroon moet worden uitgevoerd. Wanneer is ingesteld op true, de bewerking unprovision wordt geretourneerd blijft wanneer de aanvraag is geaccepteerd op het systeem en de bewerking unprovision zonder een time-outlimiet. De standaardwaarde is false. We raden echter aan instellen op true voor grote toepassingspakketten die zijn ingericht. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-upgrade"></a>sfctl toepassingsupgrade
Hiermee start u een upgrade van een toepassing in de Service Fabric-cluster.

Evalueert de opgegeven parameters toepassingsupgrade en begint met bijwerken van de toepassing als de parameters geldig zijn. Houd er rekening mee dat upgrade beschrijving de beschrijving van de bestaande toepassing vervangt. Dit betekent dat als de parameters niet opgegeven zijn, de bestaande parameters op de toepassingen wordt overschreven met de lijst met lege parameters. Dit zou leiden tot de toepassing met behulp van de standaardwaarde van de parameters van het toepassingsmanifest.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. <br><br> Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --toepassingsversie (vereist) | De toepassing type doelversie (gevonden in het toepassingsmanifest) voor de upgrade van de toepassing. |
| --parameters (vereist) | Een lijst met JSON gecodeerd van de parameter van de toepassing onderdrukt bij het upgraden van de toepassing moet worden toegepast. |
| ---service-health-standaardbeleid | JSON gecodeerd specificatie van het statusbeleid gebruikt standaard voor het evalueren van de status van een servicetype. |
| --actie bij fout | De actie om uit te voeren wanneer de upgrade van een gecontroleerde controle beleidsschendingen voor beleid of de status. |
| --geforceerd opnieuw opstarten | Start processen geforceerd opnieuw tijdens de upgrade, zelfs wanneer de codeversie is niet gewijzigd. |
| --health-controle-nieuwe poging-timeout | De hoeveelheid tijd tussen pogingen tot het uitvoeren van statuscontroles als de toepassing of het cluster niet in orde is.  Standaard\: PT0H10M0S. |
| --health check-stabiele duur | De hoeveelheid tijd dat de toepassing of het cluster moet in orde blijft voordat de upgrade wordt uitgevoerd op het volgende upgradedomein.  Standaard\: PT0H2M0S. <br><br> Eerst wordt dit geïnterpreteerd als een tekenreeks voor de duur van een ISO 8601. Als dat mislukt, wordt klikt u vervolgens dit geïnterpreteerd als een getal voor het totale aantal milliseconden. |
| --health-controle-wait-duur | De hoeveelheid tijd moet wachten na het voltooien van een upgradedomein voordat u begint met de status controleert proces.  Standaard\: 0. |
| --max-niet in orde-apps | De maximaal toegestane percentage van de beschadigde geïmplementeerde toepassingen. Weergegeven als een getal tussen 0 en 100 liggen. |
| --modus | De modus status gecontroleerd tijdens een rolling upgrade.  Standaard\: UnmonitoredAuto. |
| --replica-set-check-timeout | De maximale hoeveelheid tijd voor het verwerken van een upgradedomein blokkeren en voorkom het verlies van beschikbaarheid wanneer er onverwachte problemen zijn. Gemeten in seconden. |
| --health-service-beleid | JSON gecodeerd kaart met het service type statusbeleid per service-typenaam. De kaart is leeg worden standaard. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |
| --upgrade-domein-time-out | De hoeveelheid tijd elk upgradedomein is voltooid voordat FailureAction wordt uitgevoerd.  Standaard\: P10675199DT02H48M05.4775807S. <br><br> Eerst wordt dit geïnterpreteerd als een tekenreeks voor de duur van een ISO 8601. Als dat mislukt, wordt klikt u vervolgens dit geïnterpreteerd als een getal voor het totale aantal milliseconden. |
| --upgrade-time-out | De hoeveelheid tijd de algehele upgrade is voltooid voordat FailureAction wordt uitgevoerd.  Standaard\: P10675199DT02H48M05.4775807S. <br><br> Eerst wordt dit geïnterpreteerd als een tekenreeks voor de duur van een ISO 8601. Als dat mislukt, wordt klikt u vervolgens dit geïnterpreteerd als een getal voor het totale aantal milliseconden. |
| --waarschuwing als fout | Geeft aan of waarschuwingen met de dezelfde ernst als fouten worden behandeld. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-upgrade-resume"></a>sfctl application upgrade-hervatten
Hervat een upgrade van een toepassing in de Service Fabric-cluster.

Hervat een niet-bewaakte handmatige Service Fabric-toepassing upgraden. Service Fabric wordt één upgradedomein tegelijk bijgewerkt. Voor niet-bewaakte handmatige upgrades, nadat een upgradedomein klaar is met Service Fabric wacht kunt u deze API voordat u doorgaat aanroepen naar het volgende upgradedomein.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --upgrade-domain-name (vereist) | De naam van het upgradedomein waarin u kunt doorgaan met de upgrade. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-upgrade-rollback"></a>sfctl application upgrade-terugdraaien
Start het terugdraaien van de momenteel continue upgrade van een toepassing in de Service Fabric-cluster.

Terugdraaien van de huidige toepassing wordt gestart upgraden naar de vorige versie. Deze API kan alleen worden gebruikt voor het terugdraaien van de huidige upgrade wordt uitgevoerd die doorsturen naar de nieuwe versie is beschikbaar. StartApplicationUpgrade API te upgraden naar de gewenste versie, waaronder terugdraaien naar een eerdere versie gebruiken als de toepassing op dit moment niet worden bijgewerkt.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-upgrade-status"></a>sfctl application upgrade-status
Hiermee haalt details voor de meest recente upgrade uitgevoerd voor deze toepassing.

Retourneert informatie over de status van de upgrade van de meest recente toepassing, samen met details kunnen helpen een foutopsporing statusproblemen van toepassing.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --aanvraag-id (vereist) | De identiteit van de toepassing. Dit is meestal de volledige naam van de toepassing zonder de ' fabric\:' URI-schema. Vanaf versie 6.0, hiërarchische namen worden gescheiden met de '\~' teken. Bijvoorbeeld, als de toepassingsnaam van de is ' fabric\:/Mijntoep/app1 ', is de toepassings-id "mijntoep\~app1" in 6.0 en hoger en ' Mijntoep/app1' in eerdere versies. |
| --time-out -t | Servertime-out in seconden.  Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-application-upload"></a>sfctl toepassing uploaden
Een Service Fabric-toepassingspakket kopiëren naar de installatiekopieopslag.

(Optioneel) van de uploadvoortgang voor elk bestand in het pakket weergeven. Uploaden wordt uitgevoerd, wordt verzonden naar `stderr`.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --pad (vereist) | Pad naar lokale toepassingspakket. |
| --imagestore-tekenreeks | De installatiekopie van doel opslaan om het toepassingspakket te te uploaden.  Standaard\: fabric\:ImageStore. |
| --show-voortgang | Bestand uploadvoortgang voor grote pakketten weergeven. |

### <a name="global-arguments"></a>Algemene argumenten

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
