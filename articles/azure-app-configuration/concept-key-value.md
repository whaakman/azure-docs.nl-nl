---
title: Opslag van sleutelwaarden voor Azure App Configuration | Microsoft Docs
description: Een overzicht van hoe configuratiegegevens worden opgeslagen in Azure App Configuration
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
ms.openlocfilehash: f04067358b0b2bae745727a5dd7a1f5554f9f70e
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884978"
---
# <a name="key-value-store"></a>Sleutel-waardearchief

In Azure App Configuration worden configuratiegegevens opgeslagen als sleutel-waardeparen. Dit is een eenvoudige maar flexibele manier om verschillende soorten toepassingsinstellingen weer te geven die ontwikkelaars veel gebruiken.

## <a name="keys"></a>Sleutels

Sleutels dienen als naam van sleutel-waardeparen en worden gebruikt om de overeenkomstige waarden op te slaan en op te halen. Vaak worden sleutels ingedeeld in een hiërarchische naamruimte met behulp van een scheidingsteken (zoals `/` of `:`), op basis van de conventie die het meest geschikt is voor uw toepassing. Voor App Configuration zijn sleutels een op zichzelf staand object. Sleutels worden niet geparseerd om te bepalen hoe de namen zijn opgebouwd of om regels af te dwingen.

Het is mogelijk dat er bij het gebruik van een configuratiearchief binnen een toepassingsframework specifieke naamgevingsregels voor sleutelwaarden worden afgedwongen. Het Spring Cloud-framework van Java definieert bijvoorbeeld resources van het type `Environment`. Deze geven instellingen door aan een Spring-toepassing die voor variabelen als parameters moeten worden gebruikt, zoals de *toepassingsnaam* en het *profiel*. Sleutels voor configuratiegegevens met betrekking tot Spring Clould moeten doorgaans beginnen met deze twee elementen, gescheiden door een scheidingsteken.

Sleutels die zijn opgeslagen in App Configuration zijn hoofdlettergevoelige tekenreeksen, op basis van Unicode. De sleutels *app1* en *App1* zijn uniek in een configuratiearchief. Houd hier rekening mee wanneer u configuratie-instellingen in een toepassing gebruikt, omdat bepaalde frameworks niet hoofdlettergevoelig zijn bij het verwerken van configuratiesleutels. In het ASP.NET Core-configuratiesysteem worden sleutels bijvoorbeeld als niet-hoofdlettergevoelige tekenreeksen behandeld. Om onvoorspelbaar gedrag te voorkomen bij het uitvoeren van App Configuration-query's in een ASP.NET Core-toepassing, kunt u het beste geen gebruik maken van sleutels die alleen qua hoofdlettergebruik verschillen.

U mag in sleutelnamen die worden opgeslagen in App Configuration alle unicode-tekens gebruiken, met uitzondering van `*`, `,` en `\`, want deze zijn gereserveerd. Als u toch een gereserveerd teken wilt gebruiken, moet u deze uitsluiten met behulp van `\{Reserved Character}`. Er geldt een gecombineerde grootte van 10.000 tekens voor sleutel-waardeparen. Dit omvat alle tekens in de sleutel, de waarde en alle optionele kenmerken die hieraan zijn gekoppeld. Binnen deze limiet kunt u veel niveaus voor sleutels gebruiken.

### <a name="designing-key-namespaces"></a>Naamruimten voor sleutels ontwerpen

Er zijn twee algemene methoden voor naamgeving van sleutels die worden gebruikt voor configuratiegegevens: plat en hiërarchisch. Deze methoden zijn vergelijkbaar vanuit het oogpunt van toepassingsgebruik, maar de laatste biedt een aantal voordelen:

* Eenvoudiger te lezen. In plaats van een lange reeks tekens, fungeren de scheidingstekens in een hiërarchische sleutelnaam als spaties in een zin en als natuurlijke regeleinden tussen woorden.
* Eenvoudiger te beheren. Een hiërarchie voor sleutelnamen vertegenwoordigt logische groepen van configuratiegegevens.
* Eenvoudiger te gebruiken. Het is eenvoudiger om een query te schrijven die het patroon volgt van sleutels in een hiërarchische structuur, om zo slechts een gedeelte van de configuratiegegevens op te halen. Bovendien hebben veel nieuwere programeerframeworks systeemeigen ondersteuning voor hiërarchische configuratiegegevens, zodat u voor uw toepassing gebruik kunt maken van specifieke configuratiesets.

U kunt sleutels in App Configuration op allerlei manieren hiërarchisch indelen. U kunt zulke sleutels beschouwen als [URI's](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier). Elke hiërarchische sleutel is een *resourcepad* dat bestaat uit een of meer onderdelen, samengevoegd door middel van scheidingstekens. U kunt kiezen welk teken u wilt gebruiken als scheidingsteken, afhankelijk van wat vereist is voor uw toepassing, programmeertaal of framework. U kunt in App Configuration meerdere scheidingstekens gebruiken voor verschillende sleutels.

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

### <a name="versioning-key-values"></a>Versiebeheer van sleutelwaarden

Sleutelwaarden kunnen in App Configuration optioneel een kenmerk **label** hebben. Labels worden gebruikt om onderscheid te maken tussen sleutelwaarden met dezelfde sleutel. De sleutel *app1* met de labels *v1* en *v2* vormen twee aparte sleutelwaarden in een App Configuration-archief. Het label voor een sleutelwaarde is standaard leeg (of `null`).

Er worden in App Configuration niet automatisch oudere versies van sleutelwaarden opgeslagen wanneer deze worden gewijzigd. U kunt labels gebruiken om zo meerdere versies van een sleutelpaar te maken. Zo kunt u bijvoorbeeld een versienummer van de toepassing of een id van de Git-commit als label toevoegen om sleutelwaarden te identificeren die zijn gekoppeld aan een bepaalde software-build.

U mag in labels alle unicode-tekens gebruiken, met uitzondering van `*`, `,` en `\`, want deze zijn gereserveerd. Als u toch een gereserveerd teken wilt gebruiken, moet u deze uitsluiten met behulp van `\{Reserved Character}`.

### <a name="querying-key-values"></a>Query's uitvoeren op sleutelwaarden

Elke sleutelwaarde is uniek te identificeren met de sleutel en een label, welke ook `null` kan zijn. U kunt een query uitvoeren voor een sleutelwaarde in een App Configuration-archief door een patroon op te geven. Vervolgens worden vanuit het App Configuration-archief alle sleutelwaarden geretourneerd die overeenkomen met het patroon, evenals de bijbehorende kenmerken en waarden. U kunt de volgende sleutelpatronen gebruiken in REST API-aanroepen naar App Configuration:

| Sleutel | |
|---|---|
| `key` wordt weggelaten of `key=*` | Komt overeen met alle sleutels |
| `key=abc` | Komt exact overeen met de sleutelnaam **abc**. |
| `key=abc*` | Komt overeen met sleutelnamen die beginnen met **abc** |
| `key=*abc` | Komt overeen met sleutelnamen die beginnen met **abc** |
| `key=*abc*` | Komt overeen met sleutelnamen die **abc** bevatten |
| `key=abc,xyz` | Komt overeen met sleutelnamen **abc** of **xyz**, beperkt tot 5 CSV's |

U kunt ook de volgende labelpatronen in uw query opnemen:

| Label | |
|---|---|
| `label` wordt weggelaten of `label=*` | Komt overeen met een label, met inbegrip van `null` |
| `label=%00` | Komt overeen met label `null`. |
| `label=1.0.0` | Komt exact overeen met label **1.0.0** |
| `label=1.0.*` | Komt overeen met labels die beginnen met **1.0.** |
| `label=*.0.0` | Komt overeen met labels die eindigen met **.0.0** |
| `label=*.0.*` | Komt overeen met labels die **.0.** bevatten |
| `label=%00,1.0.0` | Komt overeen met labels `null` of **1.0.1**, beperkt tot 5 CSV's |

## <a name="values"></a>Waarden

Waarden die zijn toegewezen aan sleutels zijn ook Unicode-tekenreeksen. U kunt alle Unicode-tekens gebruiken voor waarden. Er kan optioneel een door de gebruiker gedefinieerd **inhoudstype** aan elke waarde worden gekoppeld. U kunt dit kenmerk gebruiken om informatie over een waarde op te slaan waarmee uw toepassing deze beter kan verwerken (zoals een coderingsschema).

Configuratiegegevens die zijn opgeslagen in een App Configuration-archief, waaronder alle sleutels en waarden, worden versleuteld in rust en tijdens de overdracht. Desalniettemin is App Configuration geen vervanging voor Azure Key Vault. U moet hier geen toepassingsgeheimen in opslaan.

## <a name="next-steps"></a>Volgende stappen

* [Concept: Momentopname van een bepaald tijdstip](concept-point-time-snapshot.md)  
