---
title: Wat is Language Understanding (LUIS) - Azure Cognitive Services | Microsoft Docs
description: Language Understanding (LUIS) is een API-cloudservice die aangepaste machine-learning-intelligence toepast op tekst in natuurlijke spreektaal van een gebruiker om daar de algemene betekenis en relevante detailinformatie uit te destilleren. Een clienttoepassing voor LUIS is een gesprekstoepassing waarin een gebruiker in natuurlijke taal communiceert om een taak te voltooien. Voorbeelden van clienttoepassingen zijn socialemedia-apps, chatbots en met spraak bestuurde bureaubladtoepassingen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: overview
ms.date: 10/06/2018
ms.author: diberry
ms.openlocfilehash: 0520c00ab20ca7210b3bb13567f9998e7231be43
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867652"
---
# <a name="what-is-language-understanding-luis"></a>Wat is Language Understanding (LUIS)?

Language Understanding (LUIS) is een API-cloudservice die aangepaste machine-learning-intelligence toepast op tekst in natuurlijke spreektaal van een gebruiker om daar de algemene betekenis en relevante detailinformatie uit te destilleren. 

Een clienttoepassing voor LUIS is een gesprekstoepassing waarin een gebruiker in natuurlijke taal communiceert om een taak te voltooien. Voorbeelden van clienttoepassingen zijn socialemedia-apps, chatbots en met spraak bestuurde bureaubladtoepassingen.  

![Conceptafbeelding van drie clienttoepassingen die werken met Cognitive Services Language Understanding (LUIS)](./media/luis-overview/luis-entry-point.png "Conceptafbeelding van drie clienttoepassingen die werken met Cognitive Services Language Understanding (LUIS)")

## <a name="use-luis-in-a-chat-bot"></a>LUIS gebruiken in een chatbot

<a name="Accessing-LUIS"></a>

Als de LUIS-app is gepubliceerd, worden uitingen (tekst) door een clienttoepassing verzonden naar de [API][endpoint-apis] van het eindpunt voor verwerking van natuurlijke taal in LUIS en worden de resultaten ontvangen als JSON-antwoorden. Een algemene clienttoepassing voor LUIS is een chatbot.


![Conceptafbeeldingen van de werking van LUIS met een chatbot om tekst van de gebruiker te voorspellen met behulp van natuurlijke taalverwerking (NLP)](./media/luis-overview/luis-overview-process-2.png "Conceptafbeeldingen van de werking van LUIS met een chatbot om tekst van de gebruiker te voorspellen met behulp van natuurlijke taalverwerking (NLP)")

|Stap|Bewerking|
|:--|:--|
|1|De clienttoepassing verzendt een _uiting_ van de gebruiker (tekst in eigen woorden), zoals "Ik wil mijn contactpersoon van de afdeling HR bellen", naar het eindpunt van LUIS als een HTTP-aanvraag.|
|2|LUIS past het leermodel toe op de tekst in natuurlijke taal en levert intelligente kennis over de invoer van de gebruiker. LUIS retourneert een antwoord in JSON-indeling, met 'HRContact' (HR-contactpersoon) als belangrijkste intentie. Het minimale JSON-antwoord van het eindpunt bevat de query-uiting en de belangrijkste score-intentie. Ook andere gegevens, zoals de entiteit Type contactpersoon, kunnen worden geëxtraheerd.|
|3|De clienttoepassing maakt gebruik van het JSON-antwoord om beslissingen te nemen voor het afhandelen van de aanvragen van de gebruiker. Deze beslissingen kunnen zijn gebaseerd op een beslissingsstructuur in de code van het botframework en aanroepen naar andere services. |

De LUIS-app levert intelligence zodat er slimme keuzen kunnen worden gemaakt in de clienttoepassing. Die keuzen worden niet gemaakt in LUIS. 

<!--

### Example of JSON endpoint response

The minimum JSON endpoint response contains the query utterance, and the top scoring intent. It can also extract data such as the following **Contact Type** entity. 

```JSON
{
  "query": "I want to call my HR rep.",
  "topScoringIntent": {
    "intent": "HRContact",
    "score": 0.921233
  },
  "entities": [
    {
      "entity": "call",
      "type": "Contact Type",
      "startIndex": 10,
      "endIndex": 13,
      "score": 0.7615982
    }
  ]
}
```
-->

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>

## <a name="natural-language-processing"></a>Natuurlijke taalverwerking

Een LUIS-app bevat een specifiek domeinmodel voor natuurlijke taal. U kunt de LUIS-app starten met een vooraf gedefinieerd domeinmodel, uw eigen model bouwen of onderdelen van een vooraf gedefinieerd domein combineren met uw eigen aangepaste gegevens.

* **Vooraf gedefinieerd model** LUIS bevat veel vooraf gedefinieerde domeinmodellen, inclusief intenties, uitingen en vooraf gedefinieerde entiteiten. U kunt de vooraf gedefinieerde entiteiten ook gebruiken zonder de intenties en uitingen van het vooraf gedefinieerde model. [Vooraf gedefinieerde domeinmodellen](luis-how-to-use-prebuilt-domains.md) bevatten alles wat u nodig hebt om LUIS snel te kunnen gaan gebruiken.

* **Aangepaste entiteiten** LUIS biedt u verschillende manieren om uw eigen aangepaste intenties en entiteiten te definiëren, zoals machine-learned-entiteiten, specifieke of letterlijke entiteiten, of een combinatie van machine-learned en letterlijke entiteiten.

## <a name="build-the-luis-model"></a>Het LUIS-model opbouwen
U bouwt het model op met de [ontwerp](https://aka.ms/luis-authoring-apis)-API's of met de LUIS-portal.

Om te beginnen definieert u categorieën gebruikersintenties, ook wel kortweg  **[intenties](luis-concept-intent.md)** genoemd. Voor elke intentie zijn voorbeelden van **[uitingen](luis-concept-utterance.md)** van gebruikers vereist. Elke uiting kan verschillende gegevens bevatten. Deze moeten worden geëxtraheerd met **[entiteiten](luis-concept-entity-types.md)**. 

|Voorbeeld van een uiting van een gebruiker|Intentie|Entiteiten|
|-----------|-----------|-----------|
|"Een vlucht naar __Seattle__ boeken?"|BookFlight|Seattle|
|"Wanneer is uw winkel __open__?"|StoreHoursAndLocation|open|
|"Een vergadering om __13:00 uur__ met __Bob__ van Distributie plannen"|ScheduleMeeting|13:00 uur, Bob|

<!--
## What is a natural language model?

A model begins with a list of general user intentions, called _intents_, such as "Book Flight" or "Contact Help Desk." You provide user's example text, called _example utterances_ for the intents. Then mark significant words or phrases in the utterance, called _entities_.


A model includes:

* **[intents](#intents)**: categories of user intentions (overall intended action or result)
* **[entities](#entities)**: specific types of data in utterances such as number, email, or name contained in text
* **[example utterances](#example-utterances)**: example text a user might enter in the client application

### Intents 

An [intent](luis-how-to-add-intents.md), short for _intention_, is a purpose or goal expressed in a user's utterance, such as booking a flight, paying a bill, or finding a news article. You create an intent for each action. A LUIS travel app may define an intent named "BookFlight." Each prediction query includes the top scored intent. 

The client application can use the top scoring intent to trigger an action. For example, when "BookFlight" intent is returned from LUIS, a client application could trigger an API call to an external service for booking a plane ticket.

### Entities

An [entity](luis-how-to-add-entities.md) represents detailed information found within the utterance that is relevant to the user's request. For example, in the utterance "Book a ticket to Paris",  a single ticket is requested, and "Paris" is a location. Two entities are found "a ticket" indicating a single ticket and "Paris" indicating the destination. 

After LUIS returns the entities found in the user’s utterance, the client application can use the list of entities as parameters to trigger an action. For example, booking a flight requires entities like the travel destination, date, and airline.
-->
<!--
### Example utterances

An example [utterance](luis-how-to-add-example-utterances.md) is text input from the user that the client application needs to understand. It may be a sentence, like "Book a ticket to Paris", or a fragment of a sentence, like "Booking" or "Paris flight." Utterances aren't always well-formed, and there can be many utterance variations for a particular intent. Add 10 to 20 example utterances to each intent and mark entities in every utterance.

|Example user utterance|Intent|Entities|
|-----------|-----------|-----------|
|"Book a flight to __Seattle__?"|BookFlight|Seattle|
|"When does your store __open__?"|StoreHoursAndLocation|open|
|"Schedule a meeting at __1pm__ with __Bob__ in Distribution"|ScheduleMeeting|1pm, Bob|
-->
## <a name="query-prediction-endpoint"></a>Eindpunt queryvoorspelling

Nadat het model is gemaakt en gepubliceerd naar het eindpunt, worden uitingen door de clienttoepassing verzonden naar de gepubliceerde API voor voorspelling van het [eindpunt](https://aka.ms/luis-endpoint-apis). Het model wordt door de API op de tekst toegepast voor analyse. De API reageert met de voorspellingsresultaten in JSON-indeling.  

Het minimale JSON-antwoord van het eindpunt bevat de query-uiting en de belangrijkste score-intentie. Ook andere gegevens kunnen worden geëxtraheerd, zoals de entiteit **Type contactpersoon**. 

```JSON
{
  "query": "I want to call my HR rep.",
  "topScoringIntent": {
    "intent": "HRContact",
    "score": 0.921233
  },
  "entities": [
    {
      "entity": "call",
      "type": "Contact Type",
      "startIndex": 10,
      "endIndex": 13,
      "score": 0.7615982
    }
  ]
}
```

## <a name="improve-model-prediction"></a>Voorspellingen van het model verbeteren

Nadat een LUIS-model is gepubliceerd en daadwerkelijke uitingen van gebruikers ontvangt, zijn er verschillende methoden waarmee u de nauwkeurigheid van voorspellingen kunt verbeteren: [actief leren](luis-concept-review-endpoint-utterances.md) van eindpunt-uitingen, [woordgroepenlijsten](luis-concept-feature.md) voor het opnemen van woorden in een domein, en [patronen](luis-concept-patterns.md) om het aantal benodigde uitingen te verminderen.
<!--
### Active learning

In the [active learning](luis-how-to-review-endoint-utt.md) process, LUIS allows you to adapt the LUIS app to real-world utterances by selecting utterances it received at the endpoint for your review. You can accept or correct the endpoint prediction, retrain, and republish. LUIS learns quickly with this iterative process, taking the minimum amount of your time and effort. 

### Phrase lists 

LUIS provides [phrases lists](luis-concept-feature.md) so you can indicate important words or phrases of the model. LUIS uses these lists to add additional significance to those words and phrases that would otherwise not be found in the model.

### Patterns 

Patterns allow you to simplify an intent's utterance collection into common [templates](luis-concept-patterns.md) of word choice and word order. This allows LUIS to learn quicker by needing fewer example utterances for the intents. Patterns are a hybrid system of regular expressions and machine-learned expressions. 
-->
<a name="using-luis"></a>
<!--
## Authoring and accessing models
Author LUIS from the [authoring](https://aka.ms/luis-authoring-apis) APIs or from the LUIS portal. Query the published prediction endpoint of the model from the [endpoint](https://aka.ms/luis-endpoint-apis) APIs.
-->

## <a name="development-lifecycle"></a>Levenscyclus van ontwikkeling
LUIS biedt hulpprogramma's, versiebeheer en samenwerking met andere LUIS-auteurs voor integratie in de volledige levenscyclus voor ontwikkeling op het niveau van de clienttoepassing en het taalmodel. 

## <a name="implementing-luis"></a>LUIS implementeren
LUIS kan als REST-API worden geïntegreerd met elk product, elke service en elk framework waarmee een HTTP-aanvraag wordt gemaakt. De volgende lijst bevat de belangrijkste Microsoft-producten en services die gebruikmaken van LUIS.

De populairste clienttoepassing voor LUIS is:
* [Web-app-bot](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-3.0) - hiermee maakt u snel een met LUIS compatibele chatbot voor communicatie met een gebruiker via tekstinvoer. Deze maakt gebruik van [Bot Framework][bot-framework] versie [3.x](https://github.com/Microsoft/BotBuilder) of [4.x](https://github.com/Microsoft/botbuilder-dotnet) voor een complete bot-ervaring.

Hulpprogramma's om LUIS snel en eenvoudig met een bot te gebruiken:
* [LUIS CLI](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS) Het NPM-pakket biedt creatie en voorspelling met een zelfstandig opdrachtregelhulpprogramma of een import. 
* [LUISGen](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUISGen) LUISGen is een hulpprogramma voor het genereren van sterk getypeerde broncode voor C# en TypeScript op basis van een geëxporteerd LUIS-model.
* [Dispatcher](https://aka.ms/dispatch-tool) - met behulp van het verdeelprogrammamodel kunnen verschillende LUIS- en QnA Maker-apps worden gebruikt vanuit een bovenliggende app.
* [LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) LUDown is een opdrachtregelprogramma dat helpt om taalmodellen voor uw bot te beheren.

Andere cognitieve services die kunnen worden gebruikt in combinatie met LUIS:
* [QnA Maker] [ qnamaker] - hiermee kunnen verschillende soorten tekst worden gecombineerd in een kennisdatabase met vragen en antwoorden.
* [Spellingcontrole-API voor Bing](../bing-spell-check/proof-text.md) - hiermee kunt u tekst corrigeren vóór het maken van voorspellingen. 
* [Spraakservice](../Speech-Service/overview.md) - hiermee kunt u gesproken taalaanvragen converteren naar tekst. 
* [Conversation Learner](https://docs.microsoft.com/azure/cognitive-services/labs/conversation-learner/overview) - hiermee kunt u sneller bot-gesprekken met LUIS bouwen.
* [Project Personality Chat](https://docs.microsoft.com/azure/cognitive-services/project-personality-chat/overview) - hiermee kunt u bot-smalltalk afhandelen.
<!--
## Other ways of implementing LUIS

A client application for LUIS is:
* [Windows Mixed Reality](https://docs.microsoft.com/windows/mixed-reality/) - learn more with this [Mixed reality course](https://docs.microsoft.com/windows/mixed-reality/mr-azure-303) with LUIS. 


Labs: 

-->
## <a name="next-steps"></a>Volgende stappen

Een nieuwe LUIS-app met een [vooraf gedefinieerd](luis-get-started-create-app.md) of [aangepast](luis-quickstart-intents-only.md) domein maken. [Een query uitvoeren voor het voorspellingseindpunt](luis-get-started-cs-get-intent.md) van een openbare IoT-app.

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/