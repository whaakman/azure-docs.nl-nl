---
title: Opslag van sleutelwaarden voor Azure App Configuration | Microsoft Docs
description: Een overzicht van hoe configuratiegegevens worden opgeslagen in Azure App-configuratie
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 352bc20bb4082dd14b810a6afe85653cfd67e7e1
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224466"
---
# <a name="key-value-store"></a>Sleutel-waardearchief

Azure App-configuratie worden configuratiegegevens opgeslagen als sleutel / waarde-paren. Sleutel-waardeparen zijn een eenvoudige maar flexibele manier om weer te geven van verschillende soorten toepassingsinstellingen die ontwikkelaars bekend met bent.

## <a name="keys"></a>Sleutels

Sleutels fungeren als de naam voor de sleutel / waarde-paren en worden gebruikt voor het opslaan en ophalen van de overeenkomstige waarden ervan. Het is een gebruikelijk om naar het indelen van sleutels in een hiërarchische naamruimte met behulp van een scheidingsteken teken, zoals `/` of `:`. Een te gebruiken die het beste geschikt voor uw toepassing gebruiken. Voor App Configuration zijn sleutels een op zichzelf staand object. Deze parseren sleutels om te achterhalen hoe hun namen zijn gestructureerd of een regel op deze af te dwingen niet.

Het gebruik van de opgeslagen configuratie binnen de App-frameworks kan specifieke naming schema's voor sleutelwaarden bepalen. Als een voorbeeld van Java Spring Cloud framework definieert `Environment` bronnen leveren van instellingen aan een Spring-toepassing voor parameters worden gebruikt door de variabelen die zijn *toepassingsnaam* en *profiel*. Sleutels voor Spring Cloud productspecifieke configuratiegegevens beginnen gewoonlijk met deze twee elementen, gescheiden door een scheidingsteken.

Sleutels die zijn opgeslagen in App Configuration zijn hoofdlettergevoelige tekenreeksen, op basis van Unicode. De sleutels *app1* en *App1* zijn uniek in een configuratiearchief. Houd hier rekening mee wanneer u configuratie-instellingen in een toepassing, omdat bepaalde frameworks configuratiesleutels case-insensitively verwerken. In het ASP.NET Core-configuratiesysteem worden sleutels bijvoorbeeld als niet-hoofdlettergevoelige tekenreeksen behandeld. Om te voorkomen onvoorspelbaar gedrag wanneer u App-configuratie in een ASP.NET Core-toepassing opvraagt, gebruik geen sleutels die alleen qua hoofdlettergebruik verschillen.

U kunt een unicode-teken in ingevoerd in de App-configuratie met uitzondering van sleutelnamen `*`, `,`, en `\`. Deze tekens zijn gereserveerd. Als u een gereserveerd teken bevatten wilt, moet u het escape met behulp van `\{Reserved Character}`. Er is een gecombineerde grootte van een sleutel / waarde-paar 10.000 tekens. Deze limiet bevat alle tekens in de sleutel, de waarde ervan, en alle optionele kenmerken die zijn gekoppeld. Binnen deze limiet kunt u veel niveaus voor sleutels gebruiken.

### <a name="design-key-namespaces"></a>Sleutel-naamruimten ontwerpen

Er zijn twee algemene methoden voor naamgeving van sleutels die worden gebruikt voor configuratiegegevens: plat en hiërarchisch. Deze methoden zijn vergelijkbaar een toepassing gebruik vanuit het oogpunt van, maar hiërarchische naming biedt een aantal voordelen:

* Eenvoudiger te lezen. In plaats van een lange reeks tekens, scheidingstekens in een hiërarchische sleutelnaam functioneren als spaties in een zin. Ze bieden ook natuurlijke regeleinden tussen woorden.
* Eenvoudiger te beheren. Een hiërarchie voor sleutelnamen vertegenwoordigt logische groepen van configuratiegegevens.
* Eenvoudiger te gebruiken. Het is eenvoudiger om een query te schrijven die het patroon volgt van sleutels in een hiërarchische structuur, om zo slechts een gedeelte van de configuratiegegevens op te halen. Ook hebben veel nieuwere programming frameworks systeemeigen ondersteuning voor hiërarchische configuratiegegevens, zoals die uw toepassing kunt maken gebruik van specifieke sets van de configuratie.

U kunt sleutels in App Configuration op allerlei manieren hiërarchisch indelen. Met deze sleutels als [URI's](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier). Elke hiërarchische sleutel is een resource *pad* bestaan uit een of meer onderdelen die samen worden gekoppeld met scheidingstekens. Kies welk teken te gebruiken als een scheidingsteken op basis van wat uw toepassing, programmeertaal of framework behoeften. Gebruik meerdere scheidingstekens voor verschillende sleutels in App-configuratie.

Hier volgen enkele voorbeelden van hoe u sleutelnamen in een hiërarchie kunt indelen:

* Op basis van omgevingen

        AppName:Test:DB:Endpoint
        AppName:Staging:DB:Endpoint
        AppName:Production:DB:Endpoint

* Op basis van componentservices

        AppName:Service1:Test:DB:Endpoint
        AppName:Service1:Staging:DB:Endpoint
        AppName:Service1:Production:DB:Endpoint
        AppName:Service2:Test:DB:Endpoint
        AppName:Service2:Staging:DB:Endpoint
        AppName:Service2:Production:DB:Endpoint

* Op basis van implementatieregio's

        AppName:Production:Region1:DB:Endpoint
        AppName:Production:Region2:DB:Endpoint

### <a name="version-key-values"></a>Versie sleutelwaarden

Sleutelwaarden in App-configuratie kunnen eventueel een label-kenmerk hebben. Labels worden gebruikt om u te onderscheiden van sleutelwaarden met dezelfde sleutel. Een sleutel *app1* met labels *v1* en *v2* vormen twee afzonderlijke waarden in een configuratie van apps. Het label voor waarde van een sleutel is standaard leeg is, of `null`.

App-configuratie niet automatisch versie sleutelwaarden zoals deze zijn gewijzigd. Labels gebruiken als een manier om te maken van meerdere versies van een sleutelwaarde. Bijvoorbeeld, kunt u een versienummer van de toepassing invoeren of een ID van de Git-doorvoer in labels voor het identificeren van sleutelwaarden die zijn gekoppeld aan een bepaalde software-build.

U kunt een unicode-teken in labels, behalve voor `*`, `,`, en `\`. Deze tekens zijn gereserveerd. Als u wilt een gereserveerd teken bevatten, moet u het escape met behulp van `\{Reserved Character}`.

### <a name="query-key-values"></a>Query-sleutelwaarden

De waarde van elke sleutel wordt uniek geïdentificeerd door de sleutel en een label dat kan worden `null`. U opvragen een app-configuratiearchief voor sleutelwaarden door een patroon op te geven. Het opslaan van de app-configuratie retourneert alle sleutelwaarden die overeenkomen met het patroon en hun bijbehorende waarden en kenmerken. Gebruik de volgende belangrijke patronen in REST API-aanroepen naar App-configuratie:

| Sleutel | |
|---|---|
| `key` wordt weggelaten of `key=*` | Komt overeen met alle sleutels |
| `key=abc` | Komt overeen met de sleutelnaam **abc** precies |
| `key=abc*` | Komt overeen met sleutelnamen die beginnen met **abc** |
| `key=*abc` | Komt overeen met sleutelnamen die beginnen met **abc** |
| `key=*abc*` | Komt overeen met sleutelnamen die **abc** bevatten |
| `key=abc,xyz` | Komt overeen met de sleutelnamen **abc** of **xyz**, beperkt tot vijf CSV's |

U kunt ook de volgende label patronen opnemen:

| Label | |
|---|---|
| `label` wordt weggelaten of `label=*` | Komt overeen met een label, waaronder `null` |
| `label=%00` | Komt overeen met `null` label |
| `label=1.0.0` | Komt exact overeen met label **1.0.0** |
| `label=1.0.*` | Komt overeen met labels die beginnen met **1.0.** |
| `label=*.0.0` | Komt overeen met labels die eindigen met **.0.0** |
| `label=*.0.*` | Komt overeen met labels die **.0.** bevatten |
| `label=%00,1.0.0` | Komt overeen met labels `null` of **1.0.1**, beperkt tot vijf CSV's |

## <a name="values"></a>Waarden

Waarden die zijn toegewezen aan sleutels zijn ook Unicode-tekenreeksen. U kunt alle Unicode-tekens gebruiken voor waarden. Er is een optionele gebruiker gedefinieerd type inhoud dat is gekoppeld aan elke waarde. Gebruik dit kenmerk voor het opslaan van informatie, bijvoorbeeld een codering schema over een waarde die uw toepassing correct verwerken.

Configuratiegegevens zijn opgeslagen in een app-configuratiearchief, dat alle sleutels en waarden bevat, worden versleuteld in rust en onderweg zijn. App-configuratie is niet een vervangende-oplossing voor Azure Key Vault. Toepassingsgeheimen niet in deze worden opgeslagen.

## <a name="next-steps"></a>Volgende stappen

* [Concept: Momentopname van een bepaald tijdstip](concept-point-time-snapshot.md)  
