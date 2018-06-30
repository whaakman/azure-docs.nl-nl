---
title: Verklarende woordenlijst voor de API-Service van Language Understanding (LUIS) | Microsoft Docs
description: De verklarende woordenlijst verklaart de termen die u kunt tegenkomen bij het werken met de LUIS API-Service.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: d1b83432e925e4f6fd439ac78acfde56ad31ba52
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128386"
---
# <a name="glossary"></a>Woordenlijst

## <a name="active-version"></a>Actieve versie

De actieve LUIS-versie is de versie die wijzigingen in het model ontvangt. In de [LUIS](luis-reference-regions.md) website, als u wijzigingen wilt aanbrengen in een versie die is niet de versie van het actieve, moet u eerst instellen die versie als actief. 

## <a name="authoring"></a>Ontwerpen

Ontwerp is de mogelijkheid te maken, beheren en implementeren van een [LUIS app](#luis-app), ofwel met behulp van de [LUIS](luis-reference-regions.md) website of de [authoring API's](https://aka.ms/luis-authoring-api). 

## <a name="authoring-key"></a>Sleutel ontwerpen

Eerder met de naam 'Programmatic' sleutel. Gebruikt voor het ontwerpen van de app. Niet gebruikt voor query's productieniveau eindpunt. Zie voor meer informatie [limieten sleutel](luis-boundaries.md#key-limits).   

## <a name="batch-test-json-file"></a>Batch tekst JSON-bestand

Het batchbestand is een JSON-matrix. Elk element in de matrix heeft drie eigenschappen: `text`, `intent`, en `entities`. De `entities` eigenschap is een matrix. De matrix kan niet leeg zijn. Als de `entities` matrix niet leeg is, moet de entiteiten nauwkeurig te identificeren.

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


## <a name="collaborator"></a>Medewerker

Een samenwerker is niet de [eigenaar](#owner) van de app, maar heeft dezelfde machtigingen wilt toevoegen, bewerken en verwijderen de intents, entiteiten, utterances.

## <a name="currently-editing"></a>Momenteel bewerken

Hetzelfde als [actieve versie](#active-version)

## <a name="domain"></a>Domein

In de context LUIS een **domein** is een gebied van kennis. Uw domein is specifiek voor uw app-gebied van kennis. Dit is een algemene gebied, zoals de reizen agent-app. Een app reizen agent kan ook worden specifiek zijn voor de gebieden gegevens voor uw bedrijf zoals specifieke geografische locaties, talen en services. 

## <a name="endpoint"></a>Eindpunt

De [LUIS eindpunt](https://aka.ms/luis-endpoint-apis) -URL is waar het indienen van query's LUIS na de [LUIS app](#luis-app) worden gemaakt en gepubliceerd. De eindpunt-URL bevat de regio van de gepubliceerde app, evenals de app-ID. U vindt het eindpunt op de **[publiceren](publishapp.md)** pagina van uw app om, in de tabel Resources en sleutels of als u krijgt de eindpunt-URL van de [App-Info ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c37) API.

Een voorbeeld-eindpunt ziet eruit als:

`https://<region>.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscriptionID>&verbose=true&timezoneOffset=0&q=<utterance>`

|Parameter QueryString|description|
|--|--|
|regio| [gepubliceerde regio](luis-reference-regions.md#publishing-regions) |
|appID | LUIS app-ID |
|abonnements-id | LUIS (abonnement) eindpuntsleutel gemaakt in Azure-portal |
|q | utterance |
|van timezoneoffset is| minutes|

## <a name="entity"></a>entiteit

[Entiteiten](luis-concept-entity-types.md) zijn belangrijk woorden in [utterances](luis-concept-utterance.md) beschrijvende informatie die relevant zijn voor de [bedoeling](luis-concept-intent.md), en soms ze zijn essentieel voor het. Een entiteit is in wezen een gegevenstype in LUIS. 

## <a name="f-measure"></a>F-meting

In [tests in batch][batch-testing], een meting van de nauwkeurigheid van de test.

## <a name="false-negative"></a>ONWAAR negatief (TN)

In [tests in batch][batch-testing], de gegevenspunten vertegenwoordigen utterances waarin uw app onjuist het ontbreken van de bedoeling/doelentiteit voorspeld.

## <a name="false-positive"></a>Fout-positief (TP)

In [tests in batch][batch-testing], de gegevenspunten vertegenwoordigen utterances waarin uw app onjuist het bestaan van de bedoeling/doelentiteit voorspeld.

## <a name="features"></a>Functies

In machine learning, een [functie](luis-concept-feature.md) is een onderscheidende kenmerk hebben of kenmerk van de gegevens die uw systeem toetsenbordinvoer.

## <a name="intent"></a>Doel

Een [bedoeling](luis-concept-intent.md) vertegenwoordigt een taak of actie die de gebruiker wil uitvoeren. Het is een doel of het doel uitgedrukt in de invoer van een gebruiker, zoals een vlucht reservering, betaalt een factuur of een nieuwsbericht zoeken. In LUIS, is de opzet voorspelling gebaseerd op de hele utterance. Entiteiten, zijn vergelijking: een utterance stukken.

## <a name="labeling"></a>Labels

Labels is het proces van het koppelen van een woord of zinsdeel in de opzet [utterance](#utterance) met een [entiteit](#entity) (gegevenstype). 

## <a name="luis-app"></a>LUIS app

Een app LUIS is een getraind gegevensmodel voor het opnemen van natuurlijke taal verwerking [intents](#intent), [entiteiten](#entity), en gelabeld [utterances](#utterance).

## <a name="owner"></a>Eigenaar

Elke app heeft een eigenaar op die de persoon die de app hebt gemaakt. Kan de eigenaar toevoegen [deelnemers](#collaborator).

## <a name="pattern"></a>Patronen
De functie van het vorige patroon wordt vervangen door [patronen](luis-concept-patterns.md). Gebruik patronen om de nauwkeurigheid verbeteren door minder training voorbeelden. 

## <a name="phrase-list"></a>Lijst van de wachtwoordzin op te geven

Een [woordgroepenlijst](luis-concept-feature.md#what-is-a-phrase-list-feature) bevat een groep waarden (woorden of zinnen) die deel uitmaken van dezelfde klasse en op dezelfde manier (bijvoorbeeld namen van steden of producten) moet worden behandeld. Een lijst met uitwisselbaar wordt beschouwd als synoniemen. 

## <a name="prebuilt-domains"></a>Vooraf gedefinieerde domein

Een [vooraf gemaakte domein](luis-how-to-use-prebuilt-domains.md) LUIS-App is geconfigureerd voor een specifiek domein zoals thuis automation (HomeAutomation) of restaurant reserveringen (RestaurantReservation). De intents, utterances en entiteiten zijn geconfigureerd voor dit domein. 

## <a name="prebuilt-entity"></a>Vooraf gedefinieerde entiteit

Een [vooraf gedefinieerde entiteit](pre-builtentities.md) is een entiteit LUIS voor algemene typen gegevens zoals, de URL en de e-mailbericht biedt. U een vooraf gedefinieerde entiteit toevoegen aan uw toepassing. 

## <a name="precision"></a>Precisie
In [tests in batch][batch-testing], precisie (ook wel voorspellende positief) is het gedeelte van de relevante utterances tussen de opgehaalde utterances.

## <a name="programmatic-key"></a>Programmatische sleutel

Nieuwe naam voor [authoring sleutel](#authoring-key). 

## <a name="publish"></a>Publiceren

Publiceren betekent het maken van een LUIS [actieve versie](#active-version) beschikbaar is op de test- of productieomgeving [eindpunt](#endpoint).  

## <a name="quota"></a>quotum

LUIS quotum is de beperking van de [Azure-abonnement laag](https://aka.ms/luis-price-tier). Het quotum LUIS kan worden beperkt door zowel aanvragen per seconde (http-Status 429) en het totale aantal aanvragen in een maand (http-Status 403). 

## <a name="recall"></a>Intrekken
In [tests in batch][batch-testing], intrekken (ook wel aangeduid als gevoeligheid), is de mogelijkheid voor LUIS generaliseren. 

## <a name="semantic-dictionary"></a>Semantische woordenlijst
Een semantische woordenlijst is beschikbaar op de pagina van de entiteit lijst, evenals de pagina van de lijst met woordgroep. Het semantische woordenboek bevat suggesties van woorden op basis van het huidige bereik.

## <a name="sentiment-analysis"></a>Gevoel analyse
Gevoel analyse biedt positieve of negatieve waarden van de utterances geleverd door [Tekstanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/). 

## <a name="speech-priming"></a>Spraak voorbereiden

Spraak voorbereiden kan uw spraak service gereed worden gemaakt met uw LUIS-model. Zie [inschakelen spraak voorbereiden ](publishapp.md#enable-speech-priming).

## <a name="spelling-correction"></a>Correctie

Schakel op de pagina publiceren [spellingcontrole van Bing](publishapp.md#enable-bing-spell-checker) verkeerd gespelde woorden in de utterances voordat de voorspelling te corrigeren. 

## <a name="starter-key"></a>Starter-sleutel

Hetzelfde als [programmatische sleutel](#programmatic-key), nieuwe naam voor de sleutel ontwerpen.

## <a name="subscription-key"></a>abonnementssleutel

De abonnementssleutel is de **eindpunt** sleutel gekoppeld aan de service LUIS [u hebt gemaakt in Azure](luis-how-to-azure-subscription.md). Deze sleutel is niet de [authoring sleutel](#programmatic-key). Als u een eindpuntsleutel hebt, moet het worden gebruikt voor elk eindpunt aanvragen in plaats van de sleutel ontwerpen. U kunt zien dat uw huidige endpoint-sleutel in de eindpunt-URL aan de onderkant van [ **App publiceren** pagina](publishapp.md) in [LUIS](luis-reference-regions.md) website. Dit is de waarde van **abonnementssleutel** naam/waarde-paar. 

## <a name="test"></a>Test

[Testen](interactive-test.md#test-your-app) een app LUIS betekent een utterance doorgeeft aan LUIS en weergeven van de JSON-resultaten.

## <a name="timezoneoffset"></a>Verschuiving van de tijdzone

Het eindpunt bevat van timezoneoffset is. Dit is het aantal minuten dat u wilt toevoegen of verwijderen uit de datetimeV2 vooraf gedefinieerde entiteit. Bijvoorbeeld, als de utterance is 'welk tijdstip is het nu?', de datetimeV2 geretourneerd de huidige tijd voor de clientaanvraag. Als uw clientaanvraag afkomstig is van een bot- of andere toepassingen die niet dezelfde zijn als uw bot gebruiker, moet u tussen de bot en de gebruiker in de verschuiving doorgeven. 

Zie [de tijdzone wijzigen van vooraf gedefinieerde datetimeV2 entiteit](luis-concept-data-alteration.md?#change-time-zone-of-prebuilt-datetimev2-entity).

## <a name="token"></a>Token
Een token is de kleinste eenheid die kan worden voorzien in een entiteit. Tokeniseren is gebaseerd op de toepassing [cultuur](luis-supported-languages.md#tokenization).

## <a name="train"></a>Train

Training is het proces van onderwijs LUIS over wijzigingen in de [actieve versie](#active-version) sinds de laatste training.

## <a name="true-negative"></a>De waarde True negatief (TN)

In [tests in batch][batch-testing], de gegevenspunten vertegenwoordigen utterances waarin uw app correct het ontbreken van de bedoeling/doelentiteit voorspeld.

## <a name="true-positive"></a>De waarde True positieve (TP)

In [tests in batch][batch-testing], de gegevenspunten vertegenwoordigen utterances waarin uw app correct het bestaan van de bedoeling/doelentiteit voorspeld.

## <a name="utterance"></a>utterance

Een utterance is een wachtwoordzin van natuurlijke taal zoals 'adresboek 2 tickets voor Seattle volgende dinsdag'. Voorbeeld utterances worden toegevoegd aan het doel. 

## <a name="version"></a>Versie

Een LUIS [versie](luis-how-to-manage-versions.md) is een specifieke gegevensmodel dat is gekoppeld aan een LUIS app-ID en het gepubliceerde-eindpunt. Elke app LUIS heeft ten minste één versie.

[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/interactive-test#batch-testing
