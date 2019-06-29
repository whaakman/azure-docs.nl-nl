---
title: V2 naar V3 API-migratie
titleSuffix: Azure Cognitive Services
description: Het eindpunt van de versie 3 API's zijn gewijzigd. Gebruik deze handleiding voor meer informatie over het migreren naar versie 3-eindpunt API's.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 06/24/2019
ms.author: diberry
ms.openlocfilehash: 4c08c95a05d4f22e2338a7264409aec0f64a4755
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442526"
---
# <a name="preview-migrate-to-api-version-3x-for-luis-apps"></a>Preview: Migreren naar API-versie 3.x voor LUIS-apps

Het eindpunt van de query voorspelling API's zijn gewijzigd. Gebruik deze handleiding voor meer informatie over het migreren naar versie 3-eindpunt API's. 

Deze API V3 bevat de volgende nieuwe functies, waaronder belangrijke JSON-aanvraag en/of antwoord wijzigingen: 

* [Externe entiteiten](#external-entities-passed-in-at-prediction-time)
* [Dynamische lijsten](#dynamic-lists-passed-in-at-prediction-time)
* [Vooraf gedefinieerde JSON-entiteitwijzigingen](#prebuilt-entities-with-new-json)

<!--
* [Multi-intent detection of utterance](#detect-multiple-intents-within-single-utterance)
-->

Het eindpunt van de query voorspelling [aanvraag](#request-changes) en [antwoord](#response-changes) belangrijke wijzigingen zijn aangebracht ter ondersteuning van de nieuwe functies worden weergegeven, met inbegrip van het volgende:

* [Reactie van objectwijzigingen](#top-level-json-changes)
* [Verwijzingen naar namen van entiteit-rol in plaats van de naam van de entiteit](#entity-role-name-instead-of-entity-name)
* [Eigenschappen van het markeren van entiteiten in uitingen](#marking-placement-of-entities-in-utterances)

De volgende functies van LUIS **niet ondersteund** in de API V3:

* Bing Spell Check-versie 7

[Referentiedocumentatie](https://aka.ms/luis-api-v3) is beschikbaar voor V3.

## <a name="endpoint-url-changes-by-slot-name"></a>Wijzigingen van de eindpunt-URL met de naam van de site

De indeling van de aanroep van de HTTP-eindpunt V3 is gewijzigd.

|METHODE|URL|
|--|--|
|GET|https://<b>{REGION}</b>.api.cognitive.microsoft.com/luis/<b>v3.0-preview</b>/apps/<b>{APP-ID}</b>/slots/<b>{SLOT-NAME}</b>/predict?query=<b>{QUERY}</b>|
|POST|https://<b>{REGION}</b>.api.cognitive.microsoft.com/luis/<b>v3.0-preview</b>/apps/<b>{APP-ID}</b>/slots/<b>{SLOT-NAME}</b>/predict|
|||

## <a name="endpoint-url-changes-by-version-id"></a>Wijzigingen van de eindpunt-URL door de versie-ID

Als u zoeken door versie wilt, moet u eerst [publiceren via de API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3b) met de `"directVersionPublish":true`. Query uitvoeren op het eindpunt verwijst naar de versie-ID in plaats van de naam van de site.


|METHODE|URL|
|--|--|
|GET|https://<b>{REGION}</b>.api.cognitive.microsoft.com/luis/<b>v3.0 preview</b>/apps/<b>{APP-ID}</b>/versions/<b>{versie-ID}</b>/ voorspellen? query =<b>{QUERY}</b>|
|POST|https://<b>{REGION}</b>.api.cognitive.microsoft.com/luis/<b>v3.0-preview</b>/apps/<b>{APP-ID}</b>/versions/<b>{VERSION-ID}</b>/predict|
|||

## <a name="prebuilt-entities-with-new-json"></a>Vooraf gemaakte entiteiten met nieuwe JSON

De V3 antwoord objectwijzigingen omvatten [vooraf gemaakte entiteiten](luis-reference-prebuilt-entities.md). 

## <a name="request-changes"></a>Wijzigingen aanvragen 

### <a name="query-string-parameters"></a>Queryreeksparameters

De API V3 heeft verschillende queryreeksparameters.

|Parameternaam|Type|Version|Standaard|Doel|
|--|--|--|--|--|
|`log`|boolean|V2 & V3|false|Query Store in het logboekbestand.| 
|`query`|string|Alleen v3|Er is geen standaard: dit is vereist in de GET-aanvraag|**In V2**, de utterance om te worden voorspeld is in de `q` parameter. <br><br>**In V3**, de functionaliteit wordt doorgegeven de `query` parameter.|
|`show-all-intents`|boolean|Alleen v3|false|Retourneren van alle intents met de bijbehorende score in de **prediction.intents** object. Intents worden geretourneerd als objecten in een bovenliggende `intents` object. Hiermee wordt toegang op programmeerniveau zonder dat het doel niet vinden in een matrix: `prediction.intents.give`. In V2, zijn deze in een matrix geretourneerd. |
|`verbose`|boolean|V2 & V3|false|**In V2**als is ingesteld op true, alle voorspelde intents zijn geretourneerd. Als u alle voorspelde intents moet, gebruikt u de parameter V3 van `show-all-intents`.<br><br>**In V3**, deze parameter biedt alleen een entiteit details van de metagegevens van de voorspelling van de entiteit.  |



<!--
|`multiple-segments`|boolean|V3 only|Break utterance into segments and predict each segment for intents and entities.|
-->


### <a name="the-query-prediction-json-body-for-the-post-request"></a>De query-voorspelling JSON-hoofdtekst voor de `POST` aanvraag

```JSON
{
    "query":"your utterance here",
    "options":{
        "datetimeReference": "2019-05-05T12:00:00",
        "overridePredictions": true
    },
    "externalEntities":[],
    "dynamicLists":[]
}
```

|Eigenschap|Type|Version|Standaard|Doel|
|--|--|--|--|--|
|`dynamicLists`|array|Alleen v3|Niet vereist.|[Een dynamische lijst met](#dynamic-lists-passed-in-at-prediction-time) kunt u een bestaande lijst met getraind en gepubliceerde entiteit, al in de LUIS-app uitbreiden.|
|`externalEntities`|array|Alleen v3|Niet vereist.|[Externe entiteiten](#external-entities-passed-in-at-prediction-time) bieden uw LUIS-app de mogelijkheid te identificeren en labelen entiteiten tijdens runtime, dat kan worden gebruikt als functies voor bestaande entiteiten. |
|`options.datetimeReference`|string|Alleen v3|Geen standaardwaarde|Gebruikt om te bepalen [datetimeV2 offset](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity).|
|`options.overridePredictions`|boolean|Alleen v3|false|Hiermee geeft u als gebruiker [externe entiteit (met dezelfde naam als de bestaande entiteit)](#override-existing-model-predictions) wordt gebruikt of de bestaande entiteit in het model wordt gebruikt voor de voorspelling. |
|`query`|string|Alleen v3|Vereist.|**In V2**, de utterance om te worden voorspeld is in de `q` parameter. <br><br>**In V3**, de functionaliteit wordt doorgegeven de `query` parameter.|



## <a name="response-changes"></a>Wijzigingen van de reactie

De query-antwoord JSON gewijzigd voor groter programmatische toegang tot de gegevens die het meest gebruikt. 

### <a name="top-level-json-changes"></a>Bovenste niveau JSON-wijzigingen

De bovenste JSON-eigenschappen voor V2 zijn, wanneer `verbose` is ingesteld op true, dat wordt geretourneerd van alle intents en hun scores in de `intents` eigenschap:

```JSON
{
    "query":"this is your utterance you want predicted",
    "topScoringIntent":{},
    "intents":[],
    "entities":[],
    "compositeEntities":[]
}
```

De bovenste JSON-eigenschappen voor V3 zijn:

```JSON
{
    "query": "this is your utterance you want predicted",
    "prediction":{
        "normalizedQuery": "this is your utterance you want predicted - after normalization",
        "topIntent": "intent-name-1",
        "intents": {}, 
        "entities":{}
    }
}
```

De `intents` -object is een niet-geordende lijst. Voer niet wordt ervan uitgegaan dat het eerste onderliggende item in de `intents` komt overeen met de `topIntent`. Gebruik in plaats daarvan de `topIntent` waarde om te vinden van de score is:

```nodejs
const topIntentName = response.prediction.topIntent;
const score = intents[topIntentName];
```

De wijzigingen in het antwoord JSON schema toestaan voor:

* Schakel onderscheid tussen de oorspronkelijke utterance `query`, en voorspelling, geretourneerd `prediction`.
* Eenvoudiger programmatische toegang tot voorspelde gegevens. In plaats van het inventariseren van via een matrix in V2, u kunt toegang krijgen tot waarden door **met de naam** voor zowel intenties en entiteiten. Naam van de rol wordt voor rollen, de voorspelde entiteit geretourneerd omdat deze uniek voor de hele app is.
* Gegevenstypen, worden als bepaald, gerespecteerd. Numerieke waarden zijn niet meer worden geretourneerd als tekenreeksen.
* Onderscheid tussen de eerste prioriteit voorspelling informatie en aanvullende metagegevens geretourneerd in de `$instance` object. 

### <a name="access-instance-for-entity-metadata"></a>Toegang tot `$instance` voor metagegevens van een entiteit

Als u de metagegevens van een entiteit nodig, de query-tekenreeks moet gebruiken de `verbose=true` vlag en het antwoord bevat de metagegevens in de `$instance` object. Voorbeelden worden weergegeven in de JSON-antwoorden in de volgende secties.

### <a name="each-predicted-entity-is-represented-as-an-array"></a>Elke voorspelde entiteit wordt weergegeven als een matrix

De `prediction.entities.<entity-name>` -object bevat een matrix, omdat elke entiteit kan meer dan één keer in de utterance worden voorspeld. 

### <a name="list-entity-prediction-changes"></a>Lijst entiteit voorspelling wordt gewijzigd

De JSON voor een voorspelling van de entiteit lijst is gewijzigd in een matrix van matrices:

```JSON
"entities":{
    "my_list_entity":[
        ["canonical-form-1","canonical-form-2"],
        ["canonical-form-2"]
    ]
}
```
Elke binnenkant matrix komt overeen met de tekst in de utterance. Het interior-object is een matrix omdat dezelfde tekst kan worden weergegeven in meer dan één sublijst van een entiteit lijst. 

Bij het toewijzen van tussen de `entities` object toe aan de `$instance` -object, de volgorde van objecten voor de voorspellingen van de entiteit lijst blijft behouden.

```nodejs
const item = 0; // order preserved, use same enumeration for both
const predictedCanonicalForm = entities.my_list_entity[item];
const associatedMetadata = entities.$instance.my_list_entity[item];
```

### <a name="entity-role-name-instead-of-entity-name"></a>Naam van de rol van entiteit in plaats van de naam van de entiteit 

In V2, de `entities` matrix de voorspelde entiteiten geretourneerd met de naam van de entiteit wordt de unieke id. Als de entiteit maakt gebruik van functies en de voorspelling voor de entiteitsrol van een, is de primaire id in V3, de naam van de rol. Dit is mogelijk omdat functienamen entiteit moeten uniek zijn in de hele app met inbegrip van de modelnamen van andere (classificeerders van intentie,).

In het volgende voorbeeld: u kunt een utterance dat de tekst bevat `Yellow Bird Lane`. Deze tekst wordt voorspeld als een aangepaste `Location` rol van de entiteit van `Destination`.

|Utterance tekst|Naam van de entiteit|Rolnaam|
|--|--|--|
|`Yellow Bird Lane`|`Location`|`Destination`|

In V2, de entiteit wordt aangeduid met de _entiteitnaam_ met de rol als een eigenschap van het object:

```JSON
"entities":[
    {
        "entity": "Yellow Bird Lane",
        "type": "Location",
        "startIndex": 13,
        "endIndex": 20,
        "score": 0.786378264,
        "role": "Destination"
    }
]
```

In V3, de entiteit wordt verwezen door de _entiteitsrol_, als de voorspelling voor de rol:

```JSON
"entities":{
    "Destination":[
        "Yellow Bird Lane"
    ]
}
```

In V3, hetzelfde resultaat met de `verbose` vlag om terug te keren van metagegevens van een entiteit:

```JSON
"entities":{
    "Destination":[
        "Yellow Bird Lane"
    ],
    "$instance":{
        "Destination": [
            {
                "role": "Destination",
                "type": "Location",
                "text": "Yellow Bird Lane",
                "startIndex": 25,
                "length":16,
                "score": 0.9837309,
                "modelTypeId": 1,
                "modelType": "Entity Extractor"
            }
        ]
    }
}
```

## <a name="external-entities-passed-in-at-prediction-time"></a>Externe entiteiten doorgegeven op het moment van de voorspelling

Externe entiteiten geven uw LUIS-app de mogelijkheid om te identificeren en labelen entiteiten tijdens runtime, dat kan worden gebruikt als functies voor bestaande entiteiten. Hiermee kunt u uw eigen extractors afzonderlijke en aangepaste entiteit gebruiken voor het verzenden van query's naar uw voorspelling-eindpunt. Omdat deze wordt uitgevoerd op het eindpunt van de voorspelling query, moet u niet opnieuw trainen en publiceren van uw model.

De clienttoepassing is het leveren van een eigen extractor entiteit door het beheer van de entiteit die overeenkomt met en het bepalen van de locatie binnen de utterance van die overeenkomende entiteit en vervolgens die informatie bij de aanvraag te verzenden. 

Externe entiteiten zijn het mechanisme voor het uitbreiden van elk entiteitstype terwijl nog steeds wordt gebruikt als signalen en andere modellen, zoals rollen, samengesteld en anderen.

Dit is handig voor een entiteit met gegevens die beschikbaar zijn alleen tijdens query voorspelling runtime. Voorbeelden van dergelijke gegevens zijn voortdurend gegevens of specifieke per gebruiker. U kunt een LUIS-Neem contact op met entiteit met externe gegevens uit de lijst met contactpersonen van een gebruiker uitbreiden. 

### <a name="entity-already-exists-in-app"></a>Entiteit bestaat al voor in-app

De waarde van `entityName` voor de externe entiteit, die wordt doorgegeven in de eindpunt-aanvraag hoofdtekst van bericht, moet al bestaan in de app getraind en gepubliceerd op het moment dat de aanvraag wordt gedaan. Het type entiteit maakt niet uit, alle typen worden ondersteund.

### <a name="first-turn-in-conversation"></a>Voor het eerst inschakelt in gesprek

Houd rekening met een eerste utterance in een chat bot gesprek waar een gebruiker de volgende onvolledige gegevens invoert:

`Send Hazem a new message`

Het verzoek van de bot chatten om LUIS kan worden doorgegeven in de informatie in de hoofdtekst van bericht over `Hazem` , zodat deze direct als een van de contactpersonen van de gebruiker wordt vergeleken.

```json
    "externalEntities": [
        {
            "entityName":"contacts",
            "startIndex": 5,
            "entityLength": 5,
            "resolution": {
                "employeeID": "05013",
                "preferredContactType": "TeamsChat"
            }
        }
    ]
```

Het antwoord voorspelling bevat die externe entiteit, met alle andere voorspelde entiteiten, omdat deze is gedefinieerd in de aanvraag.  

### <a name="second-turn-in-conversation"></a>Tweede inschakelen in gesprek

De volgende gebruiker utterance in de chatbot maakt gebruik van een termijn van meer vaag zijn:

`Send him a calendar reminder for the party.`

In de vorige utterance gebruikmaakt van de utterance `him` als een verwijzing naar `Hazem`. De conversatie chatbot, in de hoofdtekst van bericht kunt toewijzen `him` aan de entiteitswaarde opgehaald uit de eerste utterance `Hazem`.

```json
    "externalEntities": [
        {
            "entityName":"contacts",
            "startIndex": 5,
            "entityLength": 3,
            "resolution": {
                "employeeID": "05013",
                "preferredContactType": "TeamsChat"
            }
        }
    ]
```

Het antwoord voorspelling bevat die externe entiteit, met alle andere voorspelde entiteiten, omdat deze is gedefinieerd in de aanvraag.  

### <a name="override-existing-model-predictions"></a>Bestaande modelvoorspellingen overschrijven

De `overridePredictions` opties-eigenschap geeft op dat als de gebruiker een externe entiteit die overlapt met een voorspelde entiteit met dezelfde naam verzendt, LUIS de entiteit die wordt doorgegeven kiest in de entiteit in het model bestaan. 

Neem bijvoorbeeld de query `today I'm free`. LUIS detecteert `today` als een datetimeV2 met het volgende antwoord:

```JSON
"datetimeV2": [
    {
        "type": "date",
        "values": [
            {
                "timex": "2019-06-21",
                "value": "2019-06-21"
            }
        ]
    }
]
```

Als de gebruiker het externe entiteit verzendt:

```JSON
{
    "entityName": "datetimeV2",
    "startIndex": 0,
    "entityLength": 5,
    "resolution": {
        "date": "2019-06-21"
    }
}
```

Als de `overridePredictions` is ingesteld op `false`, LUIS een antwoord geretourneerd als de externe entiteit zijn niet verzonden. 

```JSON
"datetimeV2": [
    {
        "type": "date",
        "values": [
            {
                "timex": "2019-06-21",
                "value": "2019-06-21"
            }
        ]
    }
]
```

Als de `overridePredictions` is ingesteld op `true`, LUIS retourneert een antwoord met inbegrip van:

```JSON
"datetimeV2": [
    {
        "date": "2019-06-21"
    }
]
```



#### <a name="resolution"></a>Oplossing

De _optionele_ `resolution` resultaat van de eigenschap in het antwoord voorspellen, zodat u kunt doorgeven in de metagegevens die zijn gekoppeld aan de externe entiteit en vervolgens krijgen deze weer in het antwoord. 

Het primaire doel is om uit te breiden van vooraf gemaakte entiteiten, maar het is niet beperkt tot dat entiteitstype. 

De `resolution` eigenschap is een getal, een tekenreeks, een object of een matrix:

* "Dallas"
* {"text": "value"}
* 12345 
* ["a", "b", "c"]



## <a name="dynamic-lists-passed-in-at-prediction-time"></a>Een dynamische lijst met doorgegeven op het moment van de voorspelling

Dynamische lijsten kunnen u een bestaande lijst met getraind en gepubliceerde entiteit, al in de LUIS-app uitbreiden. 

Deze functie gebruiken als de waarden in de lijst met entiteit moeten worden periodiek gewijzigd. Deze functie kunt u een lijst met al getraind en gepubliceerde entiteit uitbreiden:

* Op het moment van de query-voorspellingsaanvraag-eindpunt.
* Voor een enkele aanvraag.

De lijst met entiteit kan niet leeg zijn in de LUIS-app, maar er bestaan. De lijst met entiteit in de LUIS-app is niet gewijzigd, maar de mogelijkheid voorspelling op het eindpunt is uitgebreid tot maximaal 2 lijsten met ongeveer 1000 items.

### <a name="dynamic-list-json-request-body"></a>Lijst met dynamische JSON-aanvraagtekst

In de volgende JSON-hoofdtekst verzenden naar een nieuwe sublijst met synoniemen toevoegen aan de lijst en de lijst met entiteit voor de tekst, voorspellen `LUIS`, met de `POST` voorspellingsaanvraag query:

```JSON
{
    "query": "Send Hazem a message to add an item to the meeting agenda about LUIS.",
    "options":{
        "timezoneOffset": "-8:00"
    },
    "dynamicLists": [
        {
            "listEntityName":"ProductList",
            "requestLists":[
                {
                    "name": "Azure Cognitive Services",
                    "canonicalForm": "Azure-Cognitive-Services",
                    "synonyms":[
                        "language understanding",
                        "luis",
                        "qna maker"
                    ]
                }
            ]
        }
    ]
}
```

Het antwoord voorspelling bevat die entiteit lijst met alle andere voorspelde entiteiten, omdat deze is gedefinieerd in de aanvraag. 

## <a name="timezoneoffset-renamed-to-datetimereference"></a>TimezoneOffset is gewijzigd in datetimeReference

**In V2**, wordt de `timezoneOffset` [parameter](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) wordt verzonden in de voorspellingsaanvraag als een queryreeks-parameter, ongeacht wanneer de aanvraag als een GET of POST-aanvraag is verzonden. 

**In V3**, dezelfde functionaliteit is opgegeven met de parameter van de hoofdtekst van bericht `datetimeReference`. 

## <a name="marking-placement-of-entities-in-utterances"></a>Plaatsing van entiteiten in uitingen markeren

**In V2**, een entiteit is gemarkeerd in een utterance met de `startIndex` en `endIndex`. 

**In V3**, de entiteit is gemarkeerd met `startIndex` en `entityLength`.

## <a name="deprecation"></a>Afschaffing 

De V2-API worden niet ten minste 9 maanden na de preview van V3 afgeschaft. 

## <a name="next-steps"></a>Volgende stappen

Gebruik de V3 API-documentatie voor het bijwerken van bestaande REST-naar LUIS aanroepen [eindpunt](https://aka.ms/luis-api-v3) API's. 