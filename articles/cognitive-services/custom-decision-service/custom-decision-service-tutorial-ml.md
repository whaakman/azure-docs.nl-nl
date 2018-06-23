---
title: Machine Learning - cognitieve Azure-Services | Microsoft Docs
description: Een zelfstudie voor machine learning in Azure aangepaste besluit Service een cloud-gebaseerde API voor de contextuele besluitvorming.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/08/2018
ms.author: slivkins;marcozo;alekh
ms.openlocfilehash: 50814d67ee39c6657954610358462d877843416e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345479"
---
# <a name="machine-learning"></a>Machine learning

Deze zelfstudie wordt de geavanceerde machine learning-functionaliteit in aangepaste besluit Service. De zelfstudie bestaat uit twee delen: [featurization](#featurization-concepts-and-implementation) en [functie specificatie](#feature-specification-format-and-apis). Featurization verwijst naar de representatie van uw gegevens als '-onderdelen voor machine learning. Functie-specificatie bevat informatie over de JSON-indeling en de aanvullende API's voor het opgeven van functies.

Machine learning in aangepaste besluit-Service is standaard transparant voor de klant. Onderdelen worden automatisch opgehaald uit de inhoud en een standaard versterking learning-algoritme wordt gebruikt. Uitpakken van de functie maakt gebruik van verschillende andere Azure cognitieve Services: [entiteit koppelen](../entitylinking/home.md), [Tekstanalyse](../text-analytics/overview.md), [Emotion](../emotion/home.md), en [Computer Vision](../computer-vision/home.md). Deze zelfstudie worden overgeslagen als alleen de standaardfunctionaliteit wordt gebruikt.

## <a name="featurization-concepts-and-implementation"></a>Featurization: concepten en implementatie

Aangepaste besluit Service kunt u besluiten één voor één. Elke beslissing omvat het kiezen van de verschillende alternatieven ook, acties. Afhankelijk van de toepassing kiezen de beslissing één actie of een (korte) gerangschikte lijst van acties.

Aanpassen voor het exemplaar van de selectie van artikelen op de voorpagina van een website. Hier acties komen overeen met de artikelen en elke beslissing is welke artikelen om te tonen aan een bepaalde gebruiker.

Elke actie wordt vertegenwoordigd door een vector eigenschappen voortaan aangeroepen *functies*. Nieuwe functies, naast de functies die automatisch worden opgehaald, kunt u opgeven. U kunt ook opgeven dat aangepaste besluit Service melden sommige functies, maar deze voor machine learning negeren.

### <a name="native-vs-internal-features"></a>Systeemeigen versus interne functies

U kunt functies in een indeling meest natuurlijke opgeven voor uw toepassing, een getal, een tekenreeks of een matrix. Deze functies worden genoemd 'native-onderdelen. Aangepaste besluit Service zet elke systeemeigen functie in een of meer numerieke onderdelen, genaamd 'interne-onderdelen.

De vertaling in interne functies is als volgt:

- numerieke onderdelen blijven hetzelfde.
- Er kan een numerieke matrix aan verschillende numerieke functies, één voor elk element van de matrix.
- een tekenreeks-valued functie `"Name":"Value"` is standaard vertaald naar een functie met de naam `"NameValue"` en de waarde 1.
- U kunt desgewenst een tekenreeks kan worden weergegeven als [eigenschappenverzameling van woorden](https://en.wikipedia.org/wiki/Bag-of-words_model). Vervolgens wordt een interne functie gemaakt voor elk woord in de tekenreeks, waarvan de waarde het aantal exemplaren van dit woord is.
- Interne functies nul waarden worden weggelaten.

### <a name="shared-vs-action-dependent-features"></a>Gedeeld versus afhankelijk zijn van het actie-functies

Sommige functies verwijzen naar de volledige beslissing en zijn hetzelfde voor alle acties. We noemen deze *gedeelde onderdelen*. Aantal andere functies zijn specifiek voor een bepaalde actie. We noemen deze *actie-afhankelijke onderdelen* (AD FS).

In het voorbeeld uitgevoerd kunnen gedeelde functies beschrijven de gebruiker en/of de status van de hele wereld. Functies, zoals geolocatie, leeftijd en geslacht van de gebruiker en welke belangrijke gebeurtenissen nu plaatsvinden. Actie-afhankelijke onderdelen kunnen de eigenschappen van een bepaald artikel, zoals de onderwerpen in dit artikel wordt beschreven.

### <a name="interacting-features"></a>Functies met elkaar communicerende

Functies vaak 'communiceren': het effect van een afhankelijk van andere gebruikers. Functie X is bijvoorbeeld of de gebruiker is geïnteresseerd in Sport. Functie Y is of een bepaald artikel gaat over sport. Het effect van functie Y is vervolgens maximaal afhankelijk van het onderdeel X.

Als u wilt voor interactie tussen functies X en Y-account maken een *kwadratische* functie waarvan de waarde X is\*Y. (We ook uitspreken "snijpunt" X en Y.) U kunt kiezen welke paren van functies worden overschreden.

> [!TIP]
> Een gedeelde functie moet worden overschreden met actie-afhankelijke functies om te kunnen van invloed zijn op hun positie. Een afhankelijk zijn van het actie-functie moet worden overschreden met gedeelde onderdelen om bijdragen aan persoonlijke instellingen.

Met andere woorden, een gedeelde functie niet is overschreden met een AD FS van invloed is op elke actie op dezelfde manier. Een ADF niet gepasseerd met een gedeelde functie is van invloed is op elke beslissing te. Deze typen functies kunnen de variantie van derden maakt een schatting te verminderen.

### <a name="implementation-via-namespaces"></a>Implementatie via naamruimten

U kunt kruislingse functies (evenals andere concepten featurization) implementeren via de 'een code opdrachtregel' in de Portal. De syntaxis is gebaseerd op de [Vowpal Wabbit](http://hunch.net/~vw/) vanaf de opdrachtregel.

Centraal staat in de implementatie is het concept van *naamruimte*: een benoemde subset van functies. Elke functie behoort tot één naamruimte. De naamruimte kan expliciet worden opgegeven wanneer de functie-waarde is opgegeven voor aangepaste besluit Service. Dit is de enige manier om te verwijzen naar een functie in de opdrachtregel voor een code.

Een naamruimte is 'gedeeld' of 'actie-afhankelijke': bestaat alleen uit gedeelde functies of bestaat alleen uit afhankelijk zijn van het actie-functies van dezelfde actie.

> [!TIP]
> Het is raadzaam om in te verpakken functies in expliciet opgegeven naamruimten. Groeperen van gerelateerde functies in dezelfde naamruimte.

Als de naamruimte niet is opgegeven, wordt de functie automatisch toegewezen aan de standaardnaamruimte.

> [!IMPORTANT]
> Functies en naamruimten hoeft niet te zijn consistent via acties. Een naamruimte kan met name de verschillende functies voor verschillende acties hebben. Bovendien kan een bepaalde naamruimte worden gedefinieerd voor een aantal acties en niet voor andere.

Meerdere interne functies die afkomstig zijn van dezelfde tekenreeks valued systeemeigen functie zijn gegroepeerd in dezelfde naamruimte. Twee systeemeigen functies die zich in verschillende naamruimten worden behandeld als uniek is, zelfs als ze dezelfde naam van de functie hebben.

> [!IMPORTANT]
> Lange, beschrijvende naamruimte-id's zijn veelvoorkomende, wordt de code-opdrachtregel maakt geen onderscheid tussen naamruimten met de id met de dezelfde letter begint. In welke volgt zijn naamruimte-id's één letters, zoals `x` en `y`.

Details over de implementatie zijn als volgt:

- Naamruimten gekruist `x` en `y`, schrijven `-q xy` of `--quadratic xy`. Vervolgens elke functie in `x` is met elke functie in gekruist `y`. Gebruik `-q x:` gekruist `x` met elke naamruimte en `-q ::` alle paren van naamruimten overschrijden.

- Voor het negeren van alle functies in de naamruimte `x`, schrijven `--ignore x`.

Deze opdrachten worden toegepast op elke actie afzonderlijk, als de naamruimten die zijn gedefinieerd.

### <a name="estimated-average-as-a-feature"></a>Geschatte gemiddelde als een functie

Als een experiment gedachte wat de gemiddelde prijs van een bepaalde actie zou zijn als deze zijn geselecteerd voor de besluitvorming voor alle? Deze gemiddelde derden kan worden gebruikt als een meting van de "totale kwaliteit' van deze actie. Het is niet precies bekend wanneer andere acties zijn gekozen in plaats daarvan in een aantal beslissingen te nemen. Het kan echter worden geschat via versterking learning-technieken. De kwaliteit van deze schatting wordt doorgaans na verloop van tijd verbetert.

U kunt deze 'geschatte gemiddelde derden"bevatten als een functie voor een bepaalde actie. Aangepaste besluit Service zou deze schatting vervolgens automatisch bijgewerkt nieuwe gegevens ontvangt. Deze functie wordt aangeroepen de *marginale functie* van deze actie. Marginale functies kunnen worden gebruikt voor machine learning en voor controle.

Schrijven om toe te voegen marginale functies, `--marginal <namespace>` op de opdrachtregel voor een code. Definieer `<namespace>` in JSON als volgt:

```json
{<namespace>: {"mf_name":1 "action_id":1}
```

Plaats deze samen met andere afhankelijk zijn van het actie-functies van een bepaalde actie-naamruimte. Deze definitie voor elke beslissing, met behulp van dezelfde `mf_name` en `action_id` voor de besluitvorming voor alle.

De functie voor marginale wordt toegevoegd voor elke actie met `<namespace>`. De `action_id` mag de naam van een functie die een unieke identificatie van de actie. De functienaam is ingesteld op `mf_name`. In het bijzonder marginal met verschillende functies `mf_name` worden behandeld als verschillende functies, een ander gewicht wordt geleerd voor elk `mf_name`.

Het gebruik van standaard is dat `mf_name` is hetzelfde voor alle acties. Vervolgens wordt een gewicht geleerd voor alle marginale functies.

U kunt ook meerdere marginale functies voor dezelfde actie met dezelfde waarden, maar met verschillende onderdeelnamen opgeven.

```json
{<namespace>: {"mf_name1":1 "action_id":1 "mf_name2":1 "action_id":1}}
```

### <a name="1-hot-encoding"></a>1 hot codering

U kunt sommige functies als bits vectoren, waar elke bit komt met een bereik van de mogelijke waarden overeen vertegenwoordigt. Deze bit is ingesteld op 1 als de functie in dit bereik ligt. Dus er is een 'hot' bit die is ingesteld op 1 en de andere worden ingesteld op 0. Deze weergave staat bekend als *1 hot codering*.

De hot 1-codering is Typerend voor categorische functies zoals 'geografische regio' waarvoor geen een inherent zinvolle numerieke representatie. U wordt ook aangeraden voor numerieke onderdelen waarvan de invloed op de prijs is waarschijnlijk niet lineair. Een bepaald artikel zijn mogelijk relevant zijn voor een bepaalde groep in de leeftijd en niet relevant zijn voor iedereen oudere of jonger.

Een functie string-waarde is 1 hot gecodeerd standaard: een unieke interne functie is gemaakt voor elke mogelijke waarde. Automatische 1 hot codering voor numerieke onderdelen en/of door aangepaste bereiken zijn momenteel niet opgegeven.

> [!TIP]
> De machine learning-algoritmen behandelen alle mogelijke waarden van een bepaalde functie interne op uniforme wijze: via een gemeenschappelijk 'gewicht'. De hot 1-codering, kunt een afzonderlijke 'gewicht' voor elke waardenbereik. Maken van de bereiken kleinere leidt tot betere voordelen zodra voldoende gegevens verzameld worden, maar mogelijk vergroot de hoeveelheid gegevens die nodig zijn voor betere kunt convergeren.

## <a name="feature-specification-format-and-apis"></a>Functie specificatie: indeling en API's

U kunt functies via verschillende aanvullende API's opgeven. Alle API's gebruiken een gemeenschappelijke JSON-indeling. Hieronder vindt u de API's en de indeling op een conceptuele niveau. De specificatie wordt aangevuld met een Swagger-schema.

Het basic JSON-sjabloon voor de functie specificatie is als volgt:

```json
{
"<name>":<value>, "<name>":<value>, ... ,
"namespace1": {"<name>":<value>, ... },
"namespace2": {"<name>":<value>, ... },
...
}
```

Hier `<name>` en `<value>` staan voor de functienaam en de waarde van de functie, respectievelijk. `<value>` is een tekenreeks, een geheel getal, een float, een Booleaanse waarde of een matrix. Een functie die niet zijn ingepakt in een naamruimte wordt automatisch toegewezen in de standaardnaamruimte.

Een speciale syntaxis gebruiken ter vertegenwoordiging van een tekenreeks als een eigenschappenverzameling van woorden `"_text":"string"` in plaats van `"<name>":<value>`. Effectief, wordt een afzonderlijke interne functie voor elk woord in de tekenreeks gemaakt. De waarde is het aantal exemplaren van dit woord.

Als `<name>` begint met '_' (en niet `"_text"`), wordt de functie genegeerd.

> [!TIP]
> U kunt samenvoegen soms functies uit meerdere bronnen voor JSON. Voor het gemak kunt u ze als volgt weergeven:
>
> ```json
> {
> "source1":<features>,
> "source2":<features>,
> ...
> }
> ```

Hier `<features>` verwijst naar de eerder gedefinieerde basic functie-specificatie. 'Geneste' diepere niveaus zijn te toegestaan. Aangepaste besluit-Service zoekt automatisch de 'diepste' JSON-objecten die kunnen worden geïnterpreteerd als `<features>`.

#### <a name="feature-set-api"></a>Functie Set API

Functie ingesteld API retourneert een lijst met functies in de JSON-indeling die eerder zijn beschreven. U kunt verschillende functie ingesteld API-eindpunten. Elk eindpunt dat wordt geïdentificeerd door de functie set-id en een URL. De toewijzing tussen functie id's ingesteld en de URL's op de Portal.

Functie ingesteld API aanroepen door het invoegen van de bijbehorende onderdeel-id in de juiste plaats in JSON. Actie-afhankelijke onderdelen, wordt automatisch de aanroep van parameters gebruikt door de actie-id. U kunt opgeven dat verschillende id's van de functieset voor dezelfde actie.

#### <a name="action-set-api-json-version"></a>Actie ingesteld API (JSON-versie)

Actie ingesteld API heeft een versie waarin acties en functies in JSON zijn opgegeven. Functies kunnen worden opgegeven expliciet en/of via de functie ingesteld API's. Gedeelde functies kunnen voor alle acties één keer worden opgegeven.

#### <a name="ranking-api-http-post-call"></a>Rangschikking API (HTTP POST-aanroep)

Rangschikking API heeft een versie die gebruikmaakt van HTTP POST-aanroep. De hoofdtekst van deze aanroep specificeert acties en -functies via een flexibele JSON-syntaxis.

Acties kunnen worden expliciet opgegeven en/of via een actie id's toekennen. Als er een actie set-id wordt aangetroffen, wordt een aanroep van de bijbehorende actie ingesteld API-eindpunt wordt uitgevoerd.

Als voor de actie ingesteld API kunnen functies expliciet en/of via de functie ingesteld API's worden opgegeven. Gedeelde functies kunnen voor alle acties één keer worden opgegeven.