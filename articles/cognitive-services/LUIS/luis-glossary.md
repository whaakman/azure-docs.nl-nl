---
title: Woordenlijst
titleSuffix: Language Understanding - Azure Cognitive Services
description: De verklarende woordenlijst verklaart de termen die u kunt tegenkomen wanneer u met de LUIS-API-Service werkt.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 8cddea1757b37ca462611874af7ace1af70c9646
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54425170"
---
# <a name="language-understanding-glossary-of-common-vocabulary-and-concepts"></a>Language understanding verklarende woordenlijst van algemene vocabulaire en concepten
De Language Understanding (LUIS) verklarende woordenlijst wordt uitgelegd dat de voorwaarden die u kunt tegenkomen wanneer u met de LUIS-API-Service werkt.

## <a name="active-version"></a>Actieve versie

De actieve LUIS-versie is de versie die wijzigingen in het model ontvangt. In de [LUIS](luis-reference-regions.md) website, als u wijzigingen wilt aanbrengen in een versie die is niet de versie van het actieve, moet u eerst die versie instellen als actief.

## <a name="authoring"></a>Ontwerpen

Schrijven is de mogelijkheid om te maken, beheren en implementeren van een [LUIS-app](#luis-app), mogelijk maken via de [LUIS](luis-reference-regions.md) website of de [API's ontwerpen](https://aka.ms/luis-authoring-api).

## <a name="authoring-key"></a>Sleutel ontwerpen

Eerder met de naam "Programmatic" sleutel. Gebruikt voor het maken van de app. Niet gebruikt voor productie-niveau eindpunt query's. Zie voor meer informatie, [sleutel limieten](luis-boundaries.md#key-limits).   

## <a name="batch-test-json-file"></a>Batch tekst JSON-bestand

De batch-bestand is een JSON-matrix. Elk element in de matrix heeft drie eigenschappen: `text`, `intent`, en `entities`. De `entities` eigenschap is een matrix. De matrix kan niet leeg zijn. Als de `entities` matrix is niet leeg zijn, moet dit de entiteiten nauwkeurig te identificeren.

```JSON
[
    {
        "text": "drive me home",
        "intent": "None",
        "entities": []
    },
    {
        "text": "book a flight to orlando on the 25th",
        "intent": "BookFlight",
        "entities": [
            {
                "entity": "orlando",
                "type": "Location",
                "startIndex": 18,
                "endIndex": 25
            }
        ]
    }
]

```


## <a name="collaborator"></a>De samenwerker

Een samenwerker is niet de [eigenaar](#owner) van de app, maar heeft dezelfde machtigingen toevoegen, bewerken en verwijderen van de intenties, entiteiten, uitingen.

## <a name="currently-editing"></a>Wordt bewerkt

Hetzelfde als [actieve versie](#active-version)

## <a name="domain"></a>Domein

In de context van LUIS, een **domein** is een gebied van kennis. Uw domein is specifiek voor uw app-gebied van kennis. Dit is een algemeen gebied, zoals de agent van de reis-app. Een agent van de reis-app kan ook zijn specifiek voor alleen de delen van gegevens voor uw bedrijf, zoals specifieke geografische locaties, talen en services.

## <a name="endpoint"></a>Eindpunt

De [LUIS eindpunt](https://aka.ms/luis-endpoint-apis) -URL is waar het indienen van LUIS-query's na de [LUIS-app](#luis-app) is geautoriseerd en gepubliceerd. De eindpunt-URL bevat de regio van de gepubliceerde app, evenals de app-ID. U vindt het eindpunt op de **[sleutels en eindpunten](luis-how-to-azure-subscription.md)** pagina van uw app, of u krijgt de eindpunt-URL van de [App-informatie ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c37) API.

Een eindpunt voorbeeld lijkt:

`https://<region>.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscriptionID>&verbose=true&timezoneOffset=0&q=<utterance>`

|QueryString-parameter|description|
|--|--|
|regio| [gepubliceerde regio](luis-reference-regions.md#publishing-regions) |
|toepassings-id | LUIS-app-ID |
|abonnements-id | LUIS-eindpunt (abonnement) sleutel is gemaakt in Azure portal |
|q | Utterance |
|timezoneOffset| minutes|

## <a name="entity"></a>Entiteit

[Entiteiten](luis-concept-entity-types.md) zijn belangrijk woorden in [uitingen](luis-concept-utterance.md) beschrijvende informatie die relevant zijn voor de [bedoeling](luis-concept-intent.md), en soms zijn essentieel voor het. Een entiteit is in feite een gegevenstype in LUIS.

## <a name="f-measure"></a>F-meting

In [batch testen](luis-interactive-test.md#batch-testing), een maateenheid voor de nauwkeurigheid van de test.

## <a name="false-negative"></a>De waarde False negatief (TN)

In [batch testen](luis-interactive-test.md#batch-testing), de gegevenspunten vertegenwoordigen uitingen waarin uw app ten onrechte de afwezigheid van de doel-/ doelentiteit voorspeld.

## <a name="false-positive"></a>Fout-positief (TP)

In [batch testen](luis-interactive-test.md#batch-testing), de gegevenspunten vertegenwoordigen uitingen waarin uw app onjuist sprake is van de doel-/ doelentiteit voorspeld.

## <a name="features"></a>Functies

In machine learning, een [functie](luis-concept-feature.md) is een onderscheidende eigenschap of kenmerk van de gegevens die uw systeem toetsenbordinvoer.

## <a name="intent"></a>Doel

Een [bedoeling](luis-concept-intent.md) vertegenwoordigt een taak of een actie die de gebruiker wil om uit te voeren. Het is een doel of het doel, uitgedrukt in de invoer van een gebruiker, zoals een vlucht reserveren, een factuur betaalt of zoeken naar een nieuwsartikel. De intentie voorspelling is in LUIS gebaseerd op de hele utterance. Entiteiten zijn ter vergelijking: onderdelen van een utterance.

## <a name="labeling"></a>Labels

Labels is het proces van het koppelen van een woord of zinsdeel in van een doel [utterance](#utterance) met een [entiteit](#entity) (gegevenstype).

## <a name="luis-app"></a>LUIS-app

Een LUIS-app is een getraind gegevensmodel voor het opnemen van de verwerking van natuurlijke taal [intents](#intent), [entiteiten](#entity), en gelabelde [uitingen](#utterance).

## <a name="owner"></a>Eigenaar

Elke app heeft een eigenaar op die de persoon die de app hebt gemaakt. Kan de eigenaar toevoegen [samenwerkers](#collaborator).

## <a name="pattern"></a>Patronen
De functie van de vorige patroon wordt vervangen door [patronen](luis-concept-patterns.md). Patronen voor het verbeteren van nauwkeurigheid door minder training voorbeelden gebruiken.

## <a name="phrase-list"></a>Woordgroepenlijst met

Een [woordgroepenlijst](luis-concept-feature.md#what-is-a-phrase-list-feature) bevat een aantal waarden (woorden of zinsdelen) die deel uitmaken van dezelfde klasse en op dezelfde manier (bijvoorbeeld de namen van steden of producten) moet worden behandeld. Een lijst met uitwisselbaar wordt beschouwd als synoniemen.

## <a name="prebuilt-domains"></a>Vooraf gedefinieerde domein

Een [vooraf gemaakte domein](luis-how-to-use-prebuilt-domains.md) is een LUIS-app die is geconfigureerd voor een specifiek domein, zoals thuis automation (HomeAutomation) of restaurant reserveringen (RestaurantReservation). De intenties, uitingen en entiteiten zijn geconfigureerd voor dit domein.

## <a name="prebuilt-entity"></a>Vooraf gemaakte entiteiten

Een [vooraf gedefinieerde entiteit](luis-prebuilt-entities.md) is een entiteit LUIS voor algemene typen gegevens, zoals, de URL en de e-mailbericht biedt. U wilt een vooraf gedefinieerde entiteit toevoegen aan uw toepassing.

## <a name="precision"></a>Precisie
In [batch testen](luis-interactive-test.md#batch-testing), precisie (ook wel een positieve waarde voor de voorspellende genoemd) is het gedeelte van de relevante uitingen onder de opgehaalde uitingen.

## <a name="programmatic-key"></a>Programmatische sleutel

Hernoemd [ontwerpen sleutel](#authoring-key).

## <a name="publish"></a>Publiceren

Publiceren van betekent dat het maken van een LUIS [actieve versie](#active-version) beschikbaar is op de test- of productieomgeving [eindpunt](#endpoint).  

## <a name="quota"></a>Quotum

LUIS quotum is de beperking van de [laag van Azure-abonnement](https://aka.ms/luis-price-tier). Het quotum LUIS kan worden beperkt door beide aanvragen per seconde (http-Status 429) en het totale aantal aanvragen in een maand (http-Status 403).

## <a name="recall"></a>Intrekken
In [batch testen](luis-interactive-test.md#batch-testing), intrekken (ook bekend als gevoeligheid), is de mogelijkheid voor LUIS om te generaliseren.

## <a name="semantic-dictionary"></a>Semantische woordenlijst
Een semantische woordenlijst is beschikbaar op de pagina van de entiteit lijst, evenals de pagina van de lijst met woorden. De functionaliteit voor semantische woordenlijst bevat suggesties van woorden op basis van het huidige bereik.

## <a name="sentiment-analysis"></a>Sentimentanalyse
Sentimentanalyse biedt positieve of negatieve waarden van de uitingen geleverd door [Tekstanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/).

## <a name="speech-priming"></a>Spraak voorbereiden

Spraak voorbereiden kan uw spraakherkenning service gereed worden gemaakt met uw LUIS-model.

## <a name="spelling-correction"></a>Correctie

Schakel de spellingcontrole van Bing om te corrigeren van verkeerd gespelde woorden in de uitingen voordat voorspelling.

## <a name="starter-key"></a>Starter-sleutel

Hetzelfde als [programmatische sleutel](#programmatic-key), hernoemd in Authoring sleutel.

## <a name="subscription-key"></a>Abonnementssleutel

De abonnementssleutel is de **eindpunt** sleutel die is gekoppeld met de service LUIS [u hebt gemaakt in Azure](luis-how-to-azure-subscription.md). Deze sleutel is niet de [ontwerpen sleutel](#programmatic-key). Als u een eindpuntsleutel hebt, moet het worden gebruikt voor een eindpunt-aanvragen in plaats van de sleutel van de ontwerphandleiding. U kunt uw huidige eindpuntsleutel binnen de eindpunt-URL aan de onderkant van zien [ **sleutels en eindpunten** pagina](luis-how-to-azure-subscription.md) in [LUIS](luis-reference-regions.md) website. Dit is de waarde van **-abonnementssleutel** naam/waarde-paar.

## <a name="test"></a>Test

[Testen](luis-interactive-test.md#test-your-app) een LUIS-app betekent een utterance wordt doorgegeven aan LUIS en het weergeven van de JSON-resultaten.

## <a name="timezoneoffset"></a>Verschuiving van de tijdzone

Het eindpunt bevat timezoneOffset. Dit is het aantal minuten dat u wilt toevoegen of verwijderen uit de datetimeV2 vooraf gedefinieerde entiteit. Bijvoorbeeld, als de utterance is 'welk moment is het nu?', de datetimeV2 geretourneerd de huidige tijd voor de clientaanvraag. Als uw clientaanvraag afkomstig is van een bot of andere toepassingen die niet is hetzelfde als de gebruiker van uw bot, moet u tussen de bot en de gebruiker in de verschuiving doorgeven.

Zie [tijdzone wijzigen van vooraf gedefinieerde datetimeV2 entiteit](luis-concept-data-alteration.md?#change-time-zone-of-prebuilt-datetimev2-entity).

## <a name="token"></a>Token
Een token is de kleinste eenheid die kan worden voorzien in een entiteit. Tokeniseren is gebaseerd op van de toepassing [cultuur](luis-language-support.md#tokenization).

## <a name="train"></a>Train

Training is het proces van LUIS lessen over wijzigingen in de [actieve versie](#active-version) sinds de laatste training.

## <a name="true-negative"></a>De waarde True negatief (TN)

In [batch testen](luis-interactive-test.md#batch-testing), de gegevenspunten vertegenwoordigen uitingen waarin uw app juist de afwezigheid van de doel-/ doelentiteit voorspeld.

## <a name="true-positive"></a>Terecht positief (TP)

In [batch testen](luis-interactive-test.md#batch-testing), de gegevenspunten vertegenwoordigen uitingen waarin uw app correct sprake is van de doel-/ doelentiteit voorspeld.

## <a name="utterance"></a>Utterance

Een utterance is een woordgroep natuurlijke taal, zoals 'book 2 tickets naar de volgende dinsdag Seattle'. Voorbeeld-uitingen worden toegevoegd aan het doel.

## <a name="version"></a>Versie

Een LUIS [versie](luis-how-to-manage-versions.md) die zijn gekoppeld aan een LUIS-app-ID en het eindpunt van de gepubliceerde model voor een specifieke gegevens. Elke LUIS-app bestaat uit ten minste één versie.
