---
title: Over Language Understanding (LUIS) in Azure | Microsoft Docs
description: Informatie over het Language Understanding (LUIS) gebruiken om de status van de kracht van machine learning om uw toepassingen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/22/2017
ms.author: v-geberr
ms.openlocfilehash: bbd0a532e54f9b221739c8ae9ff097fe44fdc4df
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751592"
---
# <a name="what-is-language-understanding-luis"></a>Wat is de taal Understanding (LUIS)?
Language Understanding (LUIS) is een cloud-gebaseerde service die van toepassing is aangepast machine learning eigen, natuurlijke taal tekst van een gebruiker om te voorspellen algehele betekenis en ophalen van de relevante, gedetailleerde informatie. 

Een clienttoepassing voor LUIS mag eigen toepassing die communiceert met een gebruiker in natuurlijke taal om een taak te voltooien. Voorbeelden van clienttoepassingen zijn sociale media apps, chatbots en bureaubladtoepassingen waarvoor spraakherkenning is ingeschakeld.  

![Conceptuele afbeelding van 3-toepassingen informatie info LUIS voeding](./media/luis-overview/luis-entry-point.png)

## <a name="what-is-a-luis-app"></a>Wat is een app LUIS?
Een app LUIS bevat een domeinspecifieke natuurlijke taalmodel ontwerpen. U kunt uw app LUIS beginnen met een vooraf gedefinieerde domeinmodel, bouwen van uw eigen of onderdelen van een vooraf gedefinieerde domein overlopen door uw eigen aangepaste informatie.

[Vooraf gedefinieerde domeinmodellen](luis-how-to-use-prebuilt-domains.md) bevatten van alle onderdelen van deze voor u en een uitstekende manier om aan de slag met LUIS snel zijn.

De app LUIS bevat ook de instellingen voor adreslijstintegratie [deelnemers](luis-concept-collaborator.md), en [versies](luis-concept-version.md).

## <a name="using-a-luis-app"></a>Met behulp van een app LUIS
<a name="Accessing-LUIS"></a> Als uw app LUIS is gepubliceerd, uw clienttoepassing utterances verzendt naar de LUIS [eindpunt API] [ endpoint-apis] en ontvangt van de voorspelling resultaten als JSON-antwoorden.

In het volgende diagram verzendt de client chatbot eerst tekst van de gebruiker van een persoon wil in hun eigen woorden om naar te LUIS in een HTTP-aanvraag. Ten tweede LUIS uw geleerde model geldt voor de natuurlijke taal zinnig input van de gebruikers en een indeling notatie JSON (JavaScript Object) antwoord geretourneerd. Ten slotte uw chatbot client maakt gebruik van de JSON-antwoord om te voldoen aan aanvragen van de gebruiker. 

![Conceptuele beelden van LUIS werken met Chatbot](./media/luis-overview/luis-overview-process-2.png)

### <a name="example-of-json-endpoint-response"></a>Voorbeeld van JSON-antwoord voor eindpunt

Het antwoord van de JSON-eindpunt minimaal bevat de query utterance, en de bovenkant score berekenen voor doel. 

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
Een model begint met een lijst met algemene gebruiker bedoelingen aangeroepen _intents_, zoals 'Adresboek vlucht' of 'Neem contact op met helpdesk'. Bieden van de voorbeeldtekst van gebruiker, aangeroepen _voorbeeld utterances_ voor de intents. Markeert vervolgens aanzienlijke woorden of zinnen in de utterance, aangeroepen _entiteiten_.


Een model omvat:

* **[intents](#intents)**: categorieën van gebruiker bedoelingen (beoogde actie of resultaat)
* **[entiteiten](#entities)**: bepaalde typen gegevens in utterances zoals, de e-mailadres of de naam
* **[voorbeeld utterances](#example-utterances)**: een gebruiker in de clienttoepassing invoert voorbeeldtekst

### <a name="intents"></a>Intents 
Een [bedoeling](luis-how-to-add-intents.md), afkorting voor _voornemen_, is een doel of doel uitgedrukt in utterance van een gebruiker, zoals een vlucht reservering, betaalt een factuur of een nieuwsbericht zoeken. U maakt een doel voor elke actie. Een LUIS reizen app mogen het definiëren van de opzet met de naam 'BookFlight'. De clienttoepassing kunt boven bedoeling score berekenen voor het activeren van een actie. Als 'BookFlight' bedoeling van LUIS wordt geretourneerd, kan uw clienttoepassing een API-aanroep naar een externe service voor het reserveren van een ticket vlak activeren.

### <a name="entities"></a>Entiteiten
Een [entiteit](luis-how-to-add-entities.md) staat voor gedetailleerde informatie gevonden binnen de utterance die relevant is voor de aanvraag van de gebruiker. Bijvoorbeeld, in de utterance 'Adresboek een ticket naar Parijs' een één ticket wordt aangevraagd en 'Parijs' is een locatie. Twee entiteiten zijn gevonden 'een ticket' die een één-ticket en 'Parijs', die wijzen op de bestemming aangeeft. 

Nadat LUIS de entiteiten die zijn gevonden in utterance van de gebruiker retourneert, kan de clienttoepassing gebruik de lijst van entiteiten als parameters voor het activeren van een actie. Bijvoorbeeld, vereist een vlucht reservering entiteiten, zoals de reizen bestemming, de datum en het luchtvaartmaatschappij.

LUIS biedt verschillende manieren om te identificeren en categoriseren entiteiten.

* **Vooraf gedefinieerde entiteiten** LUIS heeft veel vooraf gedefinieerde domeinmodellen intents, utterances, inclusief en [vooraf gemaakte entiteiten](pre-builtentities.md). U kunt de vooraf gedefinieerde entiteiten gebruiken zonder gebruik te maken van de intents en utterances van het vooraf gedefinieerde model. De vooraf gedefinieerde entiteiten u tijd besparen.

* **Aangepaste entiteiten** LUIS biedt u verschillende methoden voor het identificeren van uw eigen aangepaste [entiteiten](luis-concept-entity-types.md) waaronder machine geleerde entiteiten, specifieke of letterlijke waarde entiteiten en een combinatie van machine geleerd en literal.

### <a name="example-utterances"></a>Voorbeeld utterances
Een voorbeeld [utterance](luis-how-to-add-example-utterances.md) tekstinvoer van de gebruiker die de clienttoepassing nodig heeft om te begrijpen is. Het is mogelijk een zin, zoals 'Een ticket naar Parijs boek' of een fragment van een zin, zoals 'Reservering' of "Parijs vlucht." Utterances niet altijd goed ingedeelde en kunnen er veel utterance variaties voor een bepaald doel. Toevoegen van 10 tot 20 voorbeeld utterances aan elk doel en entiteiten in elke utterance markeren.

|Voorbeeld van de gebruiker utterance|Doel|Entiteiten|
|-----------|-----------|-----------|
|"Een vlucht een boek __Seattle__?"|BookFlight|Seattle|
|"Wanneer uw store biedt __openen__?"|StoreHoursAndLocation|openen|
|' Een vergadering op plannen __13: 00__ met __Bob__ in distributie '|ScheduleMeeting|1 uur, Berend|

## <a name="improve-prediction-accuracy"></a>Nauwkeurigheid verbeteren
Nadat uw app LUIS is gepubliceerd en echte gebruiker utterances ontvangt, LUIS voorziet verschillende methodes om de nauwkeurigheid voorspelling te verbeteren: [active learning](#active-learning) van endpoint-utterances [een lijst met woorden](#phrase-lists) voor domein opgenomen, Word en [patronen](#patterns) om het aantal utterances nodig te verminderen.

### <a name="active-learning"></a>Actief leren
In de [active learning](label-suggested-utterances.md) proces LUIS kunt u aan te passen aan uw app LUIS echte utterances door het selecteren van utterances het ontvangen door het eindpunt te controleren. U kunt accepteren of corrigeer de voorspelling eindpunt, retrain en opnieuw publiceren. LUIS aan leert snel met dit proces duurt de minimale hoeveelheid de tijd en moeite. 

### <a name="phrase-lists"></a>Een lijst met woordgroep 
LUIS biedt [woordgroepen met lijsten](luis-concept-feature.md) zodat u belangrijke woorden of zinnen aan uw domein model kunt aangeven. LUIS maakt gebruik van deze lijsten aanvullende significante toevoegen aan deze woorden en woordgroepen die zou anders niet worden gevonden in het model.

### <a name="patterns"></a>Patronen 
Patronen kunnen u de opzet utterance verzameling in algemene [sjablonen](luis-concept-patterns.md) van word keuze en word volgorde. Hierdoor LUIS te leren sneller door hoeven minder voorbeeld utterances voor de intents. Een hybride-systeem van reguliere expressies en machine geleerd expressies zijn. 

<a name="using-luis"></a>

## <a name="authoring-and-accessing-luis"></a>Ontwerp en de toegang tot LUIS
Bouwen van uw app LUIS van de website LUIS of programmatisch met de [ontwerpen](https://aka.ms/luis-authoring-apis) -API's of gebruik afhankelijk van de ontwerpen behoeften. Toegang tot uw gepubliceerde LUIS-app door de query [eindpunt](https://aka.ms/luis-endpoint-apis). 

LUIS biedt drie websites overal ter wereld, afhankelijk van uw regio ontwerpen. De authoring regio bepaalt de Azure-regio waar u uw app LUIS kunt publiceren.
<!--
|Authoring region|Publishing region(s)|
|--|--|
|[www.luis.ai](https://www.luis.ai)|**U.S.**<br>West US<br>West US 2<br>East US<br>East US 2<br>South Central US<br>West Central US<br><br>**Asia**<br>Southeast Asia<br>East Asia<br><br>**South America**<br>Brazil South |
|[au.luis.ai](https://au.luis.ai)|Australia East|
|[eu.luis.ai](https://eu.luis.ai)|West Europe<br>North Europe|
-->

Meer informatie over [meer](luis-reference-regions.md) over het ontwerpen en regio's publiceren.

## <a name="what-technologies-work-with-luis"></a>Welke technologieën met LUIS werken?
Enkele Microsoft-technologieën werken met LUIS:

* [Bing spellen controleren API](../bing-spell-check/proof-text.md) Tekstcorrectie voordat voorspelling biedt. 
* [Bot Framework] [ bot-framework] kunt u een chatbot om te communiceren met een gebruiker via tekstinvoer. Selecteer [3.x](https://github.com/Microsoft/BotBuilder) of [4.x](https://github.com/Microsoft/botbuilder-dotnet) SDK voor een volledige bot-ervaring.
* [De QnA Maker] [ qnamaker] kunt u verschillende soorten tekst te combineren in een vraag en antwoord knowledge base.
* [Spraak](../Speech/home.md) gesproken aanvragen geconverteerd naar tekst. Na de conversie naar tekst LUIS de aanvragen worden verwerkt. Zie [spraak SDK](https://aka.ms/csspeech) voor meer informatie.
* [Tekstanalyse](../text-analytics/overview.md) gevoel analyse en sleutel woordgroep ophalen van gegevens biedt.

## <a name="next-steps"></a>Volgende stappen
Maakt een nieuwe app LUIS met een [vooraf gedefinieerde](luis-get-started-create-app.md) of [aangepaste](luis-quickstart-intents-only.md) domein.

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/