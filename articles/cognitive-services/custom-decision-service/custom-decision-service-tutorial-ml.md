---
title: 'Zelfstudie: Functionalisatie en functiespecificatie - Custom Decision Service'
titlesuffix: Azure Cognitive Services
description: Een zelfstudie over functionalisatie en functiespecificatie met machine learning in Custom Decision Service.
services: cognitive-services
author: slivkins
manager: cgronlun
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: tutorial
ms.date: 05/08/2018
ms.author: slivkins
ms.openlocfilehash: 1c701cbe1a71ed48c71a9441c05a7fb4b63e3814
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226057"
---
# <a name="tutorial-featurization-and-feature-specification"></a>Zelfstudie: Functionalisatie en functiespecificatie

In deze zelfstudie wordt de geavanceerde machine learning-functionaliteit in Custom Decision Service besproken. De zelfstudie bestaat uit twee delen: [functionalisatie](#featurization-concepts-and-implementation) en [functiespecificatie](#feature-specification-format-and-apis). Functionalisatie verwijst naar de representatie van uw gegevens als 'functies' voor machine learning. Functiespecificatie omvat de JSON-indeling en de aanvullende API's voor het opgeven van functies.

Machine learning in Custom Decision Service is standaard transparant voor de klant. Functies worden automatisch opgehaald uit uw inhoud en er wordt een standaardalgoritme voor bekrachtigend of versterkend leren gebruikt. Bij het ophalen van functies wordt gebruikgemaakt van verschillende andere Azure Cognitive Services: [Entity Linking](../entitylinking/home.md), [Text Analytics](../text-analytics/overview.md), [Emotion](../emotion/home.md) en [Computer Vision](../computer-vision/home.md). U kunt deze zelfstudie overslaan als u alleen de standaardfunctionaliteit gebruikt.

## <a name="featurization-concepts-and-implementation"></a>Functionalisatie: concepten en implementatie

Custom Decision Service neemt beslissingen één voor één. Elke beslissing omvat het kiezen uit verschillende alternatieven, ook wel acties genoemd. Afhankelijk van de toepassing, kan de beslissing zijn om één actie te kiezen of een gerangschikte (korte) lijst met acties.

Stel dat u de selectie van artikelen op de beginpagina van een website wilt aanpassen. Hier komen acties overeen met artikelen en elk beslissing bestaat eruit welke artikelen worden weergegeven aan een bepaalde gebruiker.

Elke actie wordt vertegenwoordigd door een vector van eigenschappen, vanaf nu *functies* genoemd. U kunt nieuwe functies opgeven, naast de functies die automatisch worden geëxtraheerd. U kunt Custom Decision Service ook opdracht geven om bepaalde functies vast te leggen, maar deze te negeren voor machine learning.

### <a name="native-vs-internal-features"></a>Systeemeigen versus interne functies

U kunt functies opgeven in een indeling die het meest natuurlijk is voor uw toepassing, hetzij een getal, een tekenreeks of een matrix. Deze functies worden 'native' of 'systeemeigen functies' genoemd. Custom Decision Service zet elke systeemeigen functie om in een of meer numerieke functies, die 'interne functies' worden genoemd.

De omzetting in interne functies werkt als volgt:

- Numerieke functies blijven hetzelfde.
- Een numerieke matrix wordt omgezet in verschillende numerieke functies, één voor elk element van de matrix.
- Een functie met een tekenreekswaarde `"Name":"Value"` wordt standaard omgezet in een functie met de naam `"NameValue"` en de waarde 1.
- Een tekenreeks kan optioneel worden voorgesteld als een [bag-of-words](https://en.wikipedia.org/wiki/Bag-of-words_model). Vervolgens wordt er voor elk woord in de tekenreeks een interne functie gemaakt, waarvan de waarde overeenkomt met het aantal exemplaren van dit woord.
- Interne functies met nulwaarden worden weggelaten.

### <a name="shared-vs-action-dependent-features"></a>Gedeelde versus actieafhankelijke functies

Sommige functies verwijzen naar de beslissing in zijn geheel, en zijn hetzelfde voor alle acties. We noemen deze *gedeelde functies*. Enkele andere functies zijn specifiek voor een bepaalde actie. Deze noemen we *actieafhankelijke functies* (of ADF voor Action-Dependent Feature).

In het voorbeeld kunnen gedeelde functies de gebruiker en/of de toestand van de wereld beschrijven. Functies zoals geolocatie, leeftijd en geslacht van de gebruiker, en welke belangrijke gebeurtenissen er op dit moment plaatsvinden. Actieafhankelijke functies kunnen de eigenschappen van een bepaald artikel beschrijven, zoals de onderwerpen die in dit artikel worden beschreven.

### <a name="interacting-features"></a>Functies die interactie hebben

Functies hebben vaak 'interactie': het effect van de ene functie is afhankelijk van andere. Functie X geeft bijvoorbeeld aan dat de gebruiker is geïnteresseerd in sport. Functie Y bepaalt of een artikel over sport gaat. Het effect van functie Y is dus sterk afhankelijk van functie X.

Als u rekening wilt houden met interactie tussen functies X en Y, moet u een *kwadratische* functie maken met de waarde X\*Y. (We ook zeggen ook wel dat X en Y elkaar "kruisen".) U kunt aangeven kiezen welke functieparen elkaar kruisen.

> [!TIP]
> Een gedeelde functie kan worden gekruist met actieafhankelijke functies om hun positie te beïnvloeden. Een actieafhankelijke functie moet worden gekruist met gedeelde functies om bij te dragen aan persoonlijke aanpassingen.

Met andere woorden, een gedeelde functie die niet is gekruist met een ADF heeft op alle acties dezelfde invloed. Een ADF die niet is gekruist met een gedeelde functie is ook van invloed op elke beslissing. Deze typen functies kunnen de variantie van schattingen van beloningen verminderen.

### <a name="implementation-via-namespaces"></a>Implementatie via naamruimten

U kunt gekruiste functies (evenals andere concepten van functionalisatie) implementeren via de 'VW-opdrachtregel' in de portal. De syntaxis is gebaseerd op de opdrachtregel van [Vowpal Wabbit](http://hunch.net/~vw/).

Een belangrijke rol bij de implementatie is het concept van *naamruimte*: een benoemde subset van functies. Elke functie behoort tot exact één naamruimte. De naamruimte kan expliciet worden opgegeven als de waarde van de functie wordt doorgegeven aan Custom Decision Service. Dit is de enige manier om te verwijzen naar een functie op de VW-opdrachtregel.

Een naamruimte is 'gedeeld' of 'actieafhankelijk': de naamruimte bestaat alleen uit gedeelde functies of alleen uit actieafhankelijke functies van dezelfde actie.

> [!TIP]
> Het is raadzaam om functies te verpakken in expliciet opgegeven naamruimten. Groepeer gerelateerde functies in dezelfde naamruimte.

Als er geen naamruimte wordt opgegeven, wordt de functie automatisch toegewezen aan de standaardnaamruimte.

> [!IMPORTANT]
> Functies en naamruimten hoeven niet consistent te zijn tussen acties. Zo kan een naamruimte verschillende functies voor verschillende acties hebben. Bovendien kan een bepaalde naamruimte wel zijn gedefinieerd voor sommige acties en niet voor andere.

Meerdere interne functies die afkomstig zijn uit dezelfde systeemeigen functie met tekenreekswaarde worden gegroepeerd in dezelfde naamruimte. Twee systeemeigen functies die zich in verschillende naamruimten bevinden, worden behandeld als verschillende functies, zelfs als ze dezelfde functienaam hebben.

> [!IMPORTANT]
> Hoewel lange beschrijvende naamruimte-id's algemeen worden toegepast, wordt op de VW-opdrachtregel geen onderscheid gemaakt tussen naamruimten waarvan de id met de dezelfde letter begint. In wat volgt bestaan naamruimte-id's uit één letter, zoals `x` en `y`.

De implementatiedetails zijn als volgt:

- Als u de naamruimten `x` en `y` wilt kruisen, schrijft u `-q xy` of `--quadratic xy`. Elke functie in `x` wordt dan gekruist met elke functie in `y`. Gebruik `-q x:` om `x` te kruisen met elke naamruimte en `-q ::` om alle paren van naamruimten te kruisen.

- Als u alle functies in de naamruimte `x` wilt negeren, schrijft u `--ignore x`.

Deze opdrachten worden afzonderlijk toegepast op elke actie, wanneer de naamruimten worden gedefinieerd.

### <a name="estimated-average-as-a-feature"></a>Geschat gemiddelde als een functie

Als een gedachte-experiment, wat zou de gemiddelde beloning van een bepaalde actie zijn als deze voor alle beslissingen wordt gekozen? Een dergelijke gemiddelde beloning kan worden gebruikt als een meting van de 'algehele kwaliteit' van deze actie. Het is niet precies bekend wanneer andere acties zijn gekozen in sommige beslissingen. Hier kan echter een schatting van worden gemaakt via technieken voor bekrachtigend leren. De kwaliteit van deze schatting wordt meestal beter na verloop van tijd.

U kunt ervoor kiezen om deze 'geschatte gemiddelde beloning" als een functie op te nemen voor een bepaalde actie. Deze schatting wordt vervolgens automatisch bijgewerkt door Custom Decision Service wanneer er nieuwe gegevens binnenkomen. Deze functie wordt de *marginale functie* van deze actie genoemd. Marginale functies kunnen worden gebruikt voor machine learning en voor controle.

Als u marginale functies wilt toevoegen, schrijft u `--marginal <namespace>` in op de VW-opdrachtregel. Definieer `<namespace>` als volgt in JSON:

```json
{<namespace>: {"mf_name":1 "action_id":1}
```

Voeg deze naamruimte toe, samen met andere actieafhankelijke functies van een bepaalde actie. Verstrek deze definitie voor elke beslissing, met behulp van dezelfde `mf_name` en `action_id` voor alle beslissingen.

De marginale functie wordt toegevoegd voor elke actie met `<namespace>`. De `action_id` kan elke functienaam zijn die de actie uniek identificeert. De functienaam is ingesteld op `mf_name`. Marginale functies met een andere `mf_name` worden behandeld als andere functies, er wordt een andere weging geleerd voor elke `mf_name`.

Het standaardgebruik is dat `mf_name` hetzelfde is voor alle acties. Vervolgens wordt er één weging geleerd voor alle marginale functies.

U kunt ook meerdere marginale functies voor dezelfde actie opgeven, met dezelfde waarden,maar met verschillende functienamen.

```json
{<namespace>: {"mf_name1":1 "action_id":1 "mf_name2":1 "action_id":1}}
```

### <a name="1-hot-encoding"></a>1-hot codering

U kunt ervoor kiezen om sommige functies als bit-vectoren voor te stellen, waarbij elke bit overeenkomt met een bereik van mogelijke waarden. Deze bit wordt alleen ingesteld op 1 als de functie in dit bereik ligt. Dus er is één 'hot' bit die wordt ingesteld op 1 en de andere op 0. Deze representatie staat bekend als *1-hot codering*.

1-hot-codering wordt vaak gebruikt voor categorische functies zoals 'geografische regio', waarvoor geen inherent zinvolle numerieke representatie beschikbaar is. Deze codering is ook raadzaam voor numerieke functies waarvan de invloed op de beloning waarschijnlijk niet-lineair is. Een bepaald artikel kan bijvoorbeeld relevant zijn voor een bepaalde leeftijdsgroep en niet relevant voor iedereen die ouder of jonger is.

Een functie met een tekenreekswaarde wordt standaard 1-hot gecodeerd: er wordt voor elke mogelijke waarde een afzonderlijke interne functie gemaakt. Automatische 1-hot codering voor numerieke functies en/of met aangepaste bereiken is op dit moment niet mogelijk.

> [!TIP]
> De machine learning-algoritmen behandelen alle mogelijke waarden van een bepaalde interne functie op een uniforme manier: via een algemene 'weging' of 'gewicht'. 1-hot codering maakt het mogelijk om een afzonderlijke 'weging' te hebben voor elk bereik van waarden. Door de bereiken kleiner te maken, is er sprake van betere beloningen zodra er voldoende gegevens zijn verzameld. Een nadeel kan zijn dat er meer gegevens nodig zijn voor convergentie naar betere beloningen.

## <a name="feature-specification-format-and-apis"></a>Functiespecificatie: indeling en API's

U kunt functies opgeven via verschillende aanvullende API's. Alle API's gebruiken een gemeenschappelijke JSON-indeling. Hieronder vindt u de API's en de indeling op een conceptueel niveau. De specificatie wordt aangevuld door een Swagger-schema.

Het basis-JSON-sjabloon voor functiespecificatie is als volgt:

```json
{
"<name>":<value>, "<name>":<value>, ... ,
"namespace1": {"<name>":<value>, ... },
"namespace2": {"<name>":<value>, ... },
...
}
```

Hier staan `<name>` en `<value>` voor respectievelijk de functienaam en de functiewaarde. `<value>` kan een tekenreeks, een geheel getal, een float, een Booleaanse waarde of een matrix zijn. Een functie die niet is verpakt in een naamruimte, wordt automatisch toegewezen aan de standaardnaamruimte.

Als u een tekenreeks wilt voorstellen als een bag-of-words, gebruikt u de speciale syntaxis `"_text":"string"` in plaats van `"<name>":<value>`. In feite wordt er voor elk woord in de tekenreeks een afzonderlijke interne functie gemaakt. De waarde bestaat uit het aantal exemplaren van dit woord.

Als `<name>` begint met "_" (en niet `"_text"` is), wordt de functie genegeerd.

> [!TIP]
> Soms kunt u functies uit meerdere JSON-bronnen samenvoegen. Voor het gemak kunt u deze als volgt weergeven:
>
> ```json
> {
> "source1":<features>,
> "source2":<features>,
> ...
> }
> ```

Hier verwijst `<features>` naar de basisfunctiespecificatie die eerder is gedefinieerd. Diepere niveaus van 'nesten' zijn ook toegestaan. Custom Decision Service vindt automatisch de 'diepste' JSON-objecten die kunnen worden geïnterpreteerd als `<features>`.

#### <a name="feature-set-api"></a>Feature Set-API

De Feature Set-API retourneert een lijst met functies in de JSON-indeling die eerder is beschreven. U kunt verschillende eindpunten voor de Feature Set-API gebruiken. Elk eindpunt wordt geïdentificeerd met de id van de functieset en een URL. De toewijzing tussen de functieset-id's en URL's gebeurt in de portal.

Roep de Feature Set-API aan door de bijbehorende functieset-id op de juiste plaats in JSON in te voegen. Voor actieafhankelijke functies wordt de aanroep automatisch uitgebreid met parameters door de actie-id. U kunt verschillende functieset-id's opgeven voor dezelfde actie.

#### <a name="action-set-api-json-version"></a>Action Set-API (JSON-versie)

De Action Set-API heeft een versie waarin acties en functies worden opgegeven in JSON. Functies kunnen expliciet worden opgegeven en/of via Feature Set-API's. Gedeelde functies kunnen één keer worden opgegeven voor alle acties.

#### <a name="ranking-api-http-post-call"></a>Ranking-API (HTTP POST-aanroep)

De Ranking-API heeft een versie waarin een HTTP POST-aanroep wordt gebruikt. De hoofdtekst van deze aanroep bevat acties en functies via een flexibele JSON-syntaxis.

Acties kunnen expliciet worden opgegeven en/of via Action Set-API's. Wanneer er een actieset-id wordt aangetroffen, wordt er een aanroep naar het bijbehorende eindpunt van de Action Set-API uitgevoerd.

Net als bij de Action Set-API kunnen functies expliciet worden opgegeven en/of via Feature Set-API's. Gedeelde functies kunnen één keer worden opgegeven voor alle acties.
