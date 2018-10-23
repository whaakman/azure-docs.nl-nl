---
title: Entiteitstypen in LUIS-apps - Language Understanding
titleSuffix: Azure Cognitive Services
description: Entiteiten (belangrijke gegevens in het domein van uw toepassing) in Language Understanding Intelligent Service (LUIS)-apps toevoegen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: diberry
ms.openlocfilehash: 92b4864f8991380740e6edb498328ce2eea98250
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49650108"
---
# <a name="entities-in-luis"></a>Entiteiten in LUIS

Entiteiten zijn woorden of zinsdelen in uitingen die belangrijke gegevens in het domein van uw toepassing zijn.

## <a name="entity-compared-to-intent"></a>Entiteit in vergelijking met opzet
De entiteit vertegenwoordigt een woord of zinsdeel in de utterance die u wilt dat opgehaald. Een utterance kan bevatten veel entiteiten of geen helemaal. Een entiteit vertegenwoordigt een klasse met inbegrip van een verzameling van soortgelijke objecten (plaatsen, dingen, personen, gebeurtenissen en concepten). Entiteiten informatie die relevant zijn voor de bedoeling beschrijven en soms ze zijn essentieel voor uw app de taak uit te voeren. Een app nieuws zoeken kan bijvoorbeeld entiteiten, zoals 'onderwerp', 'bron', 'sleutelwoord' en 'publiceren datum', die belangrijke gegevens om te zoeken naar nieuws. Zijn belangrijke informatie voor vlucht reserveren (relevant zijn voor het doel "Bookflight") in een reservering reis-app, de 'locatie', 'datum', "luchtvaartmaatschappij", "reizen class" en 'tickets'.

Ter vergelijking, vertegenwoordigt de bedoeling de voorspelling van de gehele utterance. 

## <a name="entities-represent-data"></a>Entiteiten vertegenwoordigen gegevens
Entiteiten zijn gegevens die u wilt gebruiken om op te halen uit de utterance. Dit kan een naam, datum, naam van het product of een groep van woorden zijn. 

|Utterance|Entiteit|Gegevens|
|--|--|--|
|3 tickets naar de New York kopen|Vooraf gedefinieerde getal<br>Location.Destination|3<br>New York|
|Een ticket van de New York naar Londen kopen op 5 maart|Location.Origin<br>Location.Destination<br>Vooraf gedefinieerde datetimeV2|New York<br>Londen<br>5 maart 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Entiteiten zijn optioneel, maar sterk aanbevolen
Intents zijn vereist, zijn entiteiten optioneel. U hoeft niet te maken van entiteiten voor elke concept in uw app, maar alleen voor die nodig zijn voor de app om actie te ondernemen. 

Als uw uitingen geen details van die uw bot moet om door te gaan, hoeft u niet wilt toevoegen. Als uw app zich verder ontwikkelt, kunt u deze later toevoegen. 

Als u niet zeker weet hoe u de gegevens wilt gebruiken, een paar algemene vooraf gemaakte entiteiten zoals datetimeV2, rangtelwoord, e-mailadres en telefoonnummer toevoegen.

## <a name="label-for-word-meaning"></a>Label voor de betekenis van woord
Als het woord keuze of word-indeling hetzelfde is, maar geen betekent dit hetzelfde te doen dat, u deze niet labelen met de entiteit. 

De volgende uitingen, het woord `fair` een homograaf is. Deze hetzelfde is gespeld, maar heeft een andere betekenis:

|Utterance|
|--|
|Wat voor soort county beurzen plaatsvinden in het gebied van Seattle zomer van dit jaar?|
|Is de huidige classificatie voor de beoordeling Seattle geoorloofd?|

Als u een entiteit gebeurtenis te vinden van alle gegevens van de gebeurtenis, een label het woord `fair` in de eerste utterance, maar niet in de tweede.

## <a name="entities-are-shared-across-intents"></a>Entiteiten worden gedeeld via intents
Entiteiten worden gedeeld tussen intents. Ze behoren niet tot een enkel doel. Intenties en entiteiten kunnen worden semantisch gekoppeld, maar het is niet een exclusieve relatie.

In de utterance 'Book me een ticket naar Parijs","Parijs"is een entiteit van het type locatie. Door het herkennen van de entiteiten die worden vermeld in de invoer van de gebruiker, kunt LUIS u de specifieke acties te ondernemen om te voldoen aan een doel kiezen.

## <a name="assign-entities-in-none-intent"></a>Geen intentie-entiteiten toewijzen
Alle intents, met inbegrip van de **geen** doel entiteiten met het label moeten hebben. Dit helpt LUIS meer informatie over waar de entiteiten in de uitingen en wat woorden rond de entiteiten zijn. 

## <a name="entity-status-for-predictions"></a>Status van de entiteit voor voorspellingen

Zie [entiteit Status voorspellingen](luis-how-to-add-example-utterances.md#entity-status-predictions) voor meer informatie. 

## <a name="types-of-entities"></a>Typen entiteiten
LUIS biedt veel soorten entiteiten; vooraf gemaakte entiteiten, aangepaste machine hebt geleerd entiteiten en lijst met entiteiten.

| Naam | Kan label | Beschrijving |
| -- |--|--|
| **Vooraf gedefinieerde** <br/>[Aangepaste](#prebuilt)| |  **Definitie**<br>Ingebouwde typen die algemene concepten vertegenwoordigen. <br><br>**Lijst met**<br/>sleuteluitdrukkingen nummer, het rangtelwoord voor temperatuur, dimensie, geld, leeftijd, percentage, e-mailadres, URL, telefoonnummer en sleuteluitdrukkingen. <br><br>Namen van vooraf gemaakte entiteiten zijn gereserveerd. <br><br>Alle vooraf gedefinieerde entiteiten die zijn toegevoegd aan de toepassing worden geretourneerd in de [eindpunt](luis-glossary.md#endpoint) query. Zie voor meer informatie, [vooraf gemaakte entiteiten](./luis-prebuilt-entities.md). <br/><br/>[Voorbeeld van de reactie voor entiteit](luis-concept-data-extraction.md#prebuilt-entity-data)|
|<!-- added week of 3/21/08 --> **Reguliere expressie**<br/>[Reguliere expressie](#regex)||**Definitie**<br>Aangepaste reguliere expressie voor opmaak onbewerkte utterance tekst. Deze aanvraag wordt genegeerd en worden genegeerd culturele variant.  <br><br>Deze entiteit is handig voor woorden of zinsdelen die consistent zijn geformatteerd met een wijziging die ook consistent is.<br><br>Reguliere expressie die overeenkomt met wordt na spellingcontrole wijzigingen toegepast. <br><br>Als de reguliere expressie te complex is is, zoals het gebruik van veel vierkante haken, bent u niet de expressie toevoegen aan het model. <br><br>**Voorbeeld**<br>`kb[0-9]{6,}` komt overeen met kb123456.<br/><br/>[Snelstartgids](luis-quickstart-intents-regex-entity.md)<br>[Voorbeeld van de reactie voor entiteit](luis-concept-data-extraction.md)|
| **Eenvoudige** <br/>[Machine geleerd](#machine-learned) | ✔ | **Definitie**<br>Een enkele entiteit is een algemene entiteit die een enkele concept wordt beschreven en wordt geleerd van context machine geleerd. Context zijn woord keuze, word plaatsing en utterance lengte.<br/><br/>Dit is een goede entiteit naar woorden of zinsdelen die niet consistent zijn opgemaakt maar wijzen om hetzelfde te doen. <br/><br/>[Snelstartgids](luis-quickstart-primary-and-secondary-data.md)<br/>[Voorbeeld van de reactie voor entiteit](luis-concept-data-extraction.md#simple-entity-data)|  
| **Lijst met** <br/>[Exacte overeenkomst](#exact-match)|| **Definitie**<br>Lijst met entiteiten vertegenwoordigen een vaste en gesloten set verwante woorden samen met hun synoymns in uw systeem. <br><br>Elke entiteit lijst mogelijk een of meer formulieren. Het best worden gebruikt voor een bekende set varianten van de manieren om weer te geven van hetzelfde concept.<br/><br/>LUIS detecteert geen aanvullende waarden voor de lijst met entiteiten. Gebruik de **raden** functie om te bekijken van suggesties voor nieuwe woorden op basis van de huidige lijst.<br/><br>Als er meer dan één lijst entiteit met dezelfde waarde, wordt elke entiteit in de query eindpunt geretourneerd. <br/><br/>[Snelstartgids](luis-quickstart-intent-and-list-entity.md)<br>[Voorbeeld van de reactie voor entiteit](luis-concept-data-extraction.md#list-entity-data)| 
| **Pattern.any** <br/>[Gemengde modus](#mixed) | ✔|**Definitie**<br>Patterns.any is een tijdelijke aanduiding voor de variabele lengte gebruikt alleen in een patroon van sjabloon utterance markeren waar de entiteit begint en eindigt.  <br><br>**Voorbeeld**<br>Uitgaande van een zoekopdracht utterance voor rapporten op basis van de titel, haalt de pattern.any de volledige titel. Een sjabloon utterance met behulp van pattern.any is `Who wrote {BookTitle}[?]`.<br/><br/>[Zelfstudie](luis-tutorial-pattern.md)<br>[Voorbeeld van de reactie voor entiteit](luis-concept-data-extraction.md#composite-entity-data)|  
| **Samengestelde** <br/>[Machine geleerd](#machine-learned) | ✔|**Definitie**<br>Een samengestelde entiteit is opgebouwd uit andere entiteiten, zoals de vooraf gemaakte entiteiten, eenvoudige, regex, hiërarchische lijst. De afzonderlijke entiteiten vormen een hele entiteit. <br><br>**Voorbeeld**<br>Een samengestelde entiteit met de naam PlaneTicketOrder mogelijk onderliggende entiteiten vooraf gedefinieerde `number` en `ToLocation`. <br/><br/>[Zelfstudie](luis-tutorial-composite-entity.md)<br>[Voorbeeld van de reactie voor entiteit](luis-concept-data-extraction.md#composite-entity-data)|  
| **Hiërarchische** <br/>[Machine geleerd](#machine-learned) |✔ | **Definitie**<br>Een hiërarchische entiteit is een categorie contextueel geleerde eenvoudige entiteiten.<br><br>**Voorbeeld**<br>Een hiërarchische entiteit van het gegeven `Location` met onderliggende `ToLocation` en `FromLocation`, elke onderliggende kan worden bepaald op basis van de **context** binnen de utterance. In de utterance `Book 2 tickets from Seattle to New York`, wordt de `ToLocation` en `FromLocation` contextueel verschillen op basis van de tekst eromheen. <br/><br/>**Gebruik niet als**<br>Als u naar een entiteit die overeenkomt met de exacte tekst voor kinderen, ongeacht de context zoekt, moet u een lijst met entiteit. Als u naar een bovenliggende / onderliggende relatie met andere Entiteitstypen zoekt, moet u de samengestelde entiteit.<br/><br/>[Snelstartgids](luis-quickstart-intent-and-hier-entity.md)<br>[Voorbeeld van de reactie voor entiteit](luis-concept-data-extraction.md#hierarchical-entity-data)|

<a name="prebuilt"></a>
**Vooraf gedefinieerde** entiteiten zijn aangepaste entiteiten die worden geleverd door LUIS. Sommige van deze entiteiten zijn gedefinieerd in de open-source [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text) project. Er zijn veel [voorbeelden](https://github.com/Microsoft/Recognizers-Text/tree/master/Specs) in de map /Specs voor de ondersteunde culturen. Als uw specifieke cultuur of de entiteit wordt momenteel niet ondersteund, dragen bij aan het project. 

<a name="machine-learned"></a>
**Machine geleerd** entiteiten werken het beste wanneer getest [eindpunt query's](luis-concept-test.md#endpoint-testing) en [eindpunt uitingen controleren](luis-how-to-review-endoint-utt.md). 

<a name="regex"></a>
**Reguliere expressie entiteiten** worden gedefinieerd met een reguliere expressie die de gebruiker als onderdeel van de entiteitsdefinitie van de biedt. 

<a name="exact-match"></a>
**Exacte overeenkomst** entiteiten maken gebruik van de tekst in de entiteit waarmee een exact overeenkomende tekst overeenkomen.

<a name="mixed"></a>
**Gemengde** entiteiten maken gebruik van een combinatie van detectiemethoden entiteit.

## <a name="entity-limits"></a>Limieten voor entiteit
Beoordeling [limieten](luis-boundaries.md#model-boundaries) om te begrijpen hoe vaak elk type entiteit kunt u toevoegen aan een model.

## <a name="entity-roles"></a>Entiteit-rollen
Entiteit [rollen](luis-concept-roles.md) zijn van toepassing op aangepaste en vooraf gemaakte entiteiten en worden gebruikt in alleen-patronen. 

## <a name="composite-vs-hierarchical-entities"></a>Samengestelde vs hiërarchische entiteiten
Samengestelde entiteiten en hiërarchische entiteiten zowel hebben relaties tussen bovenliggende en onderliggende en machine hebt geleerd. De machine learning kunt LUIS om te begrijpen van de entiteiten op basis van verschillende contexten (rangschikking van woorden). Samengestelde entiteiten zijn flexibel omdat ze verschillende Entiteitstypen als onderliggende items toegestaan. Een hiërarchische entiteit kinderen zijn alleen eenvoudige entiteiten. 

|Type|Doel|Voorbeeld|
|--|--|--|
|Hiërarchische|Bovenliggende / onderliggende van eenvoudige entiteiten|Location.Origin=New York<br>Location.Destination=London|
|Samengestelde|Bovenliggende / onderliggende entiteiten: vooraf gemaakte, lijst met eenvoudige, hiërarchische| getal = 3<br>lijst met = first class<br>prebuilt.datetimeV2=March 5|

## <a name="data-matching-multiple-entities"></a>Gegevens die overeenkomen met meerdere entiteiten
Als een woord of woordgroep komt overeen met meer dan één entiteit, retourneert de query eindpunt elke entiteit. Als u zowel vooraf gedefinieerde numerieke entiteit en prebuild datetimeV2 toevoegen, en een utterance moet `create meeting on 2018/03/12 for lunch with wayne`, LUIS herkent alle entiteiten en retourneert een matrix van entiteiten als onderdeel van het JSON-eindpunt-antwoord: 

```JSON
{
  "query": "create meeting on 2018/03/12 for lunch with wayne",
  "topScoringIntent": {
    "intent": "Calendar.Add",
    "score": 0.9333419
  },
  "entities": [
    {
      "entity": "2018/03/12",
      "type": "builtin.datetimeV2.date",
      "startIndex": 18,
      "endIndex": 27,
      "resolution": {
        "values": [
          {
            "timex": "2018-03-12",
            "type": "date",
            "value": "2018-03-12"
          }
        ]
      }
    },
    {
      "entity": "2018",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 21,
      "resolution": {
        "value": "2018"
      }
    },
    {
      "entity": "03/12",
      "type": "builtin.number",
      "startIndex": 23,
      "endIndex": 27,
      "resolution": {
        "value": "0.25"
      }
    }
  ]
}
```

## <a name="data-matching-multiple-list-entities"></a>Gegevens die overeenkomen met meerdere lijst met entiteiten
Als een woord of woordgroep komt overeen met meer dan één entiteit van de lijst, retourneert de query eindpunt elke entiteit die lijst.

Voor de query `when is the best time to go to red rock?`, en de app is het woord `red` in meer dan één lijst LUIS herkent alle entiteiten en retourneert een matrix van entiteiten als onderdeel van het JSON-eindpunt-antwoord: 

```JSON
{
  "query": "when is the best time to go to red rock?",
  "topScoringIntent": {
    "intent": "Calendar.Find",
    "score": 0.06701678
  },
  "entities": [
    {
      "entity": "red",
      "type": "Colors",
      "startIndex": 31,
      "endIndex": 33,
      "resolution": {
        "values": [
          "Red"
        ]
      }
    },
    {
      "entity": "red rock",
      "type": "Cities",
      "startIndex": 31,
      "endIndex": 38,
      "resolution": {
        "values": [
          "Destinations"
        ]
      }
    }
  ]
}
```

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Als u meer dan het maximum aantal entiteiten 

U moet mogelijk hiërarchische en samengestelde entiteiten gebruiken. Hiërarchische entiteiten weer de relatie tussen entiteiten die dezelfde kenmerken, of lid zijn van een categorie. De onderliggende entiteiten zijn alle leden van de bovenliggende categorie. Bijvoorbeeld, een hiërarchische entiteit met de naam PlaneTicketClass mogelijk de onderliggende entiteiten EconomyClass en bewegingssensoren. De hiërarchie omvat slechts één niveau van diepte.  

Samengestelde entiteiten vertegenwoordigen onderdelen van een volledig. Een samengestelde entiteit met de naam PlaneTicketOrder kan bijvoorbeeld onderliggende entiteiten luchtvaartmaatschappij, doel, DepartureCity, vertrekdatum en PlaneTicketClass hebben. U maakt een samengestelde entiteit op basis van bestaande eenvoudige entiteiten, onderliggende items van hiërarchische entiteiten of vooraf gemaakte entiteiten.  

LUIS biedt ook het type van de lijst met entiteit die niet voor machine geleerd, maar kan uw LUIS-app om op te geven van een vaste lijst met waarden. Zie [LUIS grenzen](luis-boundaries.md) verwijzing naar de grenzen van het type van de entiteit lijst bekijken. 

Als u hebt besloten hiërarchische, composite, en lijst met entiteiten en moet het nog steeds meer dan de limiet, contact op met ondersteuning. Om dit te doen, verzamelt gedetailleerde informatie over uw systeem, gaat u naar de [LUIS](luis-reference-regions.md#luis-website) website, en selecteer vervolgens **ondersteuning**. Als uw Azure-abonnement voor ondersteuningsservices bevat, neem dan contact op met [technische ondersteuning van Azure](https://azure.microsoft.com/support/options/). 

## <a name="best-practices"></a>Aanbevolen procedures

Maak een [entiteit](luis-concept-entity-types.md) wanneer de aanroepende toepassing of bot moet bepaalde parameters of de gegevens uit de utterance vereist voor het uitvoeren van een actie. Een entiteit is een woord of zinsdeel in de utterance die u nodig hebt uitgepakt--misschien als een parameter voor een functie. 

Om het juiste type entiteit toevoegen aan uw toepassing selecteert, moet u weten hoe die gegevens worden ingevoerd door gebruikers. Elk entiteitstype is gevonden met behulp van een ander mechanisme, zoals machine learning, gesloten lijst of reguliere expressie. Als u niet zeker weet, beginnen met een enkele entiteit en het woord of woordgroep die dat de gegevens in alle uitingen over alle intents aangeeft, met inbegrip van de geen intentie te labelen.  

Bekijk eindpunt uitingen regelmatig veelvoorkomend gebruik waar een entiteit kan worden geïdentificeerd als een reguliere expressie of gevonden met een overeenkomst exact overeenkomende tekst zoeken.  

Als onderdeel van de beoordeling, houd rekening met het toevoegen van een woordgroepenlijst om toe te voegen een signaal naar LUIS woorden of zinsdelen die belangrijk zijn om uw domein, maar nog niet precies overeenkomen en LUIS geen waarvoor een hoge betrouwbaarheid.  

Zie [aanbevolen procedures](luis-concept-best-practices.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Informatie over concepten over goede [uitingen](luis-concept-utterance.md). 

Zie [entiteiten toevoegen](luis-how-to-add-entities.md) voor meer informatie over entiteiten toevoegen aan uw LUIS-app.