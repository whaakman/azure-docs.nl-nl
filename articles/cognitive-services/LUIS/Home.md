---
title: Informatie over Language Understanding (LUIS) in Azure | Microsoft Docs
description: Informatie over het gebruik van Language Understanding (LUIS) om de vele mogelijkheden van machine learning aan uw toepassingen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/22/2017
ms.author: v-geberr
ms.openlocfilehash: da8ea6dead6b22d97e7338b2aa57a892be475417
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344828"
---
# <a name="what-is-language-understanding-luis"></a>Wat is Language Understanding (LUIS)?
Language Understanding (LUIS) is een cloud-gebaseerde service die aangepaste machine learning is van toepassing op van een gebruiker tekst met betrekking tot een conversatie, natuurlijke taal algehele betekenis te voorspellen en pull-relevante, gedetailleerde informatie. 

Een clienttoepassing voor LUIS mag elke conversatie toepassing die communiceert met een gebruiker in natuurlijke taal om een taak te voltooien. Voorbeelden van clienttoepassingen zijn socialemedia-apps, chatbots en bureaubladtoepassingen spraak ingeschakeld.  

![Conceptuele afbeelding van deze informatie info LUIS uit 3-toepassingen](./media/luis-overview/luis-entry-point.png)

## <a name="what-is-a-luis-app"></a>Wat is een LUIS-app?
Een LUIS-app bevat een domeinspecifieke natuurlijke taalmodel ontwerpen. U kunt uw LUIS-app te starten met een vooraf gedefinieerde domeinmodel, bouw uw eigen of onderdelen van een vooraf gedefinieerde domein met uw eigen aangepaste gegevens combineren.

[Vooraf gedefinieerde domeinmodellen](luis-how-to-use-prebuilt-domains.md) al deze onderdelen zijn voor u en zijn een uitstekende manier om te beginnen met LUIS snel.

De LUIS-app bevat ook de instellingen voor adreslijstintegratie [samenwerkers](luis-concept-collaborator.md), en [versies](luis-concept-version.md).

## <a name="using-a-luis-app"></a>Met behulp van een LUIS-app
<a name="Accessing-LUIS"></a> Zodra uw LUIS-app is gepubliceerd, uw clienttoepassing uitingen verzendt naar de LUIS [eindpunt API] [ endpoint-apis] en ontvangt de voorspellingsresultaten als JSON-antwoorden.

In het volgende diagram verzendt de client-chatbot eerst tekst van de gebruiker van een persoon wil in hun eigen woorden LUIS in een HTTP-aanvraag. Ten tweede LUIS het geleerde model is van toepassing op de natuurlijke taal die zinvol zijn van de invoer van de gebruiker en een JavaScript Object Notation (JSON)-indeling-antwoord geretourneerd. Derde, uw client-chatbot maakt gebruik van het JSON-antwoord om te voldoen aan aanvragen van de gebruiker. 

![Conceptuele beeldmateriaal van LUIS werken met Chatbot](./media/luis-overview/luis-overview-process-2.png)

### <a name="example-of-json-endpoint-response"></a>Voorbeeld van JSON-eindpunt-antwoord

Het antwoord van een JSON-eindpunt, ten minste bevat de query-utterance, en de bovenkant scoring-doel. 

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

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>
## <a name="what-is-a-natural-language-model"></a>Wat is een natuurlijke taalmodel?
Een model begint met een lijst met algemene gebruiker bedoelingen, met de naam _intents_, zoals 'Vlucht' of 'Neem contact op met helpdesk'. U opgeven dat de voorbeeldtekst van de gebruiker, met de naam _voorbeeld uitingen_ voor de intents. Markeert vervolgens aanzienlijke woorden of zinsdelen in de utterance, met de naam _entiteiten_.


Een model omvat:

* **[intents](#intents)**: categorieën van de gebruiker bedoelingen (beoogde actie of resultaat)
* **[entiteiten](#entities)**: bepaalde typen gegevens in uitingen zoals nummer, e-mailadres of naam
* **[voorbeeld-uitingen](#example-utterances)**: een gebruiker moet in de clienttoepassing invoeren voorbeeldtekst

### <a name="intents"></a>Intents 
Een [bedoeling](luis-how-to-add-intents.md), kort voor _voornemen_, is het doel of doel, uitgedrukt in van een gebruiker utterance, zoals een vlucht reserveren, een factuur betaalt of zoeken naar een nieuwsartikel. U maakt een doel voor elke actie. Een LUIS reis-app kan definiëren een doel met de naam "BookFlight." Uw clienttoepassing kan boven scoren doel gebruiken voor het activeren van een actie. Als "BookFlight" intentie wordt geretourneerd van LUIS, kan uw clienttoepassing een API-aanroep naar een externe service voor het reserveren van een ticket vlak activeren.

### <a name="entities"></a>Entiteiten
Een [entiteit](luis-how-to-add-entities.md) staat voor gedetailleerde informatie vindt u in de utterance die relevant is voor de aanvraag van de gebruiker. Bijvoorbeeld, in de utterance "Book een ticket naar Parijs", één ticket wordt aangevraagd, en "Parijs" is een locatie. Twee entiteiten zijn 'een ticket"die wijzen op een enkele ticket en"Parijs", die wijzen op de bestemming gevonden. 

Nadat LUIS de entiteiten in van de gebruiker utterance gevonden retourneert, kunt de lijst met entiteiten als parameters door de clienttoepassing voor het activeren van een actie gebruiken. Bijvoorbeeld: entiteiten, zoals de reis-doel, de datum en de luchtvaartmaatschappij een vlucht reserveren heeft.

LUIS biedt verschillende manieren om te identificeren en categoriseren entiteiten.

* **Vooraf gemaakte entiteiten** LUIS heeft veel vooraf gedefinieerde domeinmodellen intents, uitingen, inclusief en [vooraf gemaakte entiteiten](luis-prebuilt-entities.md). U kunt de vooraf gemaakte entiteiten gebruiken zonder gebruik te maken van de intenties en uitingen van de vooraf gebouwde model. De vooraf gemaakte entiteiten u tijd besparen.

* **Aangepaste entiteiten** LUIS kunt u verschillende manieren om te bepalen van uw eigen aangepaste [entiteiten](luis-concept-entity-types.md) inclusief machine geleerde entiteiten, letterlijke of specifieke entiteiten en een combinatie van machine-geleerd en letterlijke.

### <a name="example-utterances"></a>Voorbeelden van utterances
Een voorbeeld [utterance](luis-how-to-add-example-utterances.md) tekstinvoer van de gebruiker die de clienttoepassing nodig heeft om te begrijpen is. Het is mogelijk een zin, zoals "Book een ticket naar Parijs", of een fragment van een zin, zoals 'Reservering' of "Parijs vlucht." Uitingen niet altijd goed is ingedeeld en kunnen er veel utterance variaties voor een bepaald doel. 10 tot 20 voorbeeld utterances toevoegen aan elke kunt u lezen wat en markeren van entiteiten in elke utterance.

|Voorbeeld van de gebruiker utterance|Doel|Entiteiten|
|-----------|-----------|-----------|
|"Een overdracht naar het adresboek __Seattle__?"|BookFlight|Seattle|
|"Wanneer heeft uw winkel __open__?"|StoreHoursAndLocation|openen|
|"Een vergadering op plannen __1 uur__ met __Bob__ in distributie"|ScheduleMeeting|1 uur, Bob|

## <a name="improve-prediction-accuracy"></a>Nauwkeurigheid van voorspelling verbeteren
Nadat uw LUIS-app is gepubliceerd en real-user-uitingen ontvangt, LUIS, biedt verschillende methoden voor het verbeteren van de nauwkeurigheid: [actief leren](#active-learning) van eindpunt-uitingen [lijsten woordgroep](#phrase-lists) voor domein opgenomen, Word en [patronen](#patterns) te verminderen van het aantal uitingen die nodig zijn.

### <a name="active-learning"></a>Actief leren
In de [actief leren](label-suggested-utterances.md) proces, LUIS kunt u uw LUIS-app op echte uitingen aanpassen door te uitingen die het ontvangen op het eindpunt voor de beoordeling selecteren. U kunt accepteren of corrigeer de voorspelling van het eindpunt, retrain en opnieuw publiceren. LUIS snel aan het aanleren met dit proces duurt de minimale hoeveelheid tijd en moeite. 

### <a name="phrase-lists"></a>Frasenlijsten 
LUIS biedt [woordgroepen met lijsten](luis-concept-feature.md) zodat u belangrijke woorden of zinsdelen aan het domein van uw model aangeven kunt. LUIS maakt gebruik van deze lijsten meer betekenis toevoegen aan deze woorden en zinnen die zouden anders niet worden gevonden in het model.

### <a name="patterns"></a>Patronen 
Patronen kunnen u voor het vereenvoudigen van de opzet utterance verzameling naar algemene [sjablonen](luis-concept-patterns.md) van word keuze en word volgorde. Hiermee wordt LUIS voor meer informatie over snellere door minder voorbeeld uitingen die voor de intents. Patronen zijn een hybride systeem van reguliere expressies en expressies machine geleerd. 

<a name="using-luis"></a>

## <a name="authoring-and-accessing-luis"></a>Ontwerpen en openen van LUIS
Bouw uw LUIS-app van de website van LUIS of programmatisch met de [ontwerpen](https://aka.ms/luis-authoring-apis) API's of gebruik afhankelijk van de behoeften van de ontwerphandleiding. Toegang tot uw gepubliceerde LUIS-app door de query [eindpunt](https://aka.ms/luis-endpoint-apis). 

LUIS biedt drie websites over de hele wereld, afhankelijk van uw regio voor schrijven. De regio voor schrijven bepaalt de Azure-regio waar u uw LUIS-app kunt publiceren.
<!--
|Authoring region|Publishing region(s)|
|--|--|
|[www.luis.ai](https://www.luis.ai)|**U.S.**<br>West US<br>West US 2<br>East US<br>East US 2<br>South Central US<br>West Central US<br><br>**Asia**<br>Southeast Asia<br>East Asia<br><br>**South America**<br>Brazil South |
|[au.luis.ai](https://au.luis.ai)|Australia East|
|[eu.luis.ai](https://eu.luis.ai)|West Europe<br>North Europe|
-->

Informatie over [meer](luis-reference-regions.md) over het ontwerpen en publiceren van regio's.

## <a name="what-technologies-work-with-luis"></a>Welke technologieën werken met LUIS?
Verschillende Microsoft-technologieën werken met LUIS:

* [Bing Spell Check-API](../bing-spell-check/proof-text.md) Tekstcorrectie voor voorspelling biedt. 
* [Botframework] [ bot-framework] kunt u een chatbot om te communiceren met een gebruiker via tekstinvoer. Selecteer [3.x](https://github.com/Microsoft/BotBuilder) of [4.x](https://github.com/Microsoft/botbuilder-dotnet) SDK voor een complete bot-ervaring.
* [QnA Maker] [ qnamaker] kunnen verschillende soorten tekst om te combineren in een kennisdatabase vragen en antwoorden.
* [Spraak](../Speech/home.md) zet gesproken taalaanvragen om in tekst. Na de conversie naar tekst, LUIS de aanvragen worden verwerkt. Zie [spraak SDK](https://aka.ms/csspeech) voor meer informatie.
* [Tekstanalyse](../text-analytics/overview.md) sentiment-analyse en de sleutel woordgroep ophalen van gegevens biedt.

## <a name="next-steps"></a>Volgende stappen
Maak een nieuwe LUIS-app met een [vooraf gedefinieerde](luis-get-started-create-app.md) of [aangepaste](luis-quickstart-intents-only.md) domein.

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/