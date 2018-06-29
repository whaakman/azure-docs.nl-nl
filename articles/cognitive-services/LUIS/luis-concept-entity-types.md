---
title: Entiteit-typen in LUIS apps in Azure | Microsoft Docs
description: Entiteiten (belangrijke gegevens in uw toepassing domein) in de taal Understanding Intelligent Service (LUIS) apps toevoegen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/28/2018
ms.author: v-geberr
ms.openlocfilehash: 01f451f7a3e09aacb029c2194044320717bfae96
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37083242"
---
# <a name="entities-in-luis"></a>Entiteiten in LUIS

Entiteiten zijn woorden of zinnen in utterances die belangrijke gegevens in uw toepassing domein zijn.

## <a name="entity-compared-to-intent"></a>Vergeleken met de bedoeling entiteit
De entiteit vertegenwoordigt een woord of woordgroep binnen de gewenste uitgepakte utterance. Een utterance kunt opnemen veel entiteiten of geen helemaal. Een entiteit vertegenwoordigt een klasse met inbegrip van een verzameling van soortgelijke objecten (plaatsen, dingen, mensen, gebeurtenissen of concepten). Entiteiten informatie die relevant zijn voor de bedoeling beschreven, en soms ze zijn essentieel voor uw app de taak uit te voeren. Een app News Search omvat mogelijk entiteiten zoals 'onderwerp', 'bron', 'sleutelwoord' en 'publishing datum', die belangrijke gegevens om te zoeken naar nieuws. Zijn belangrijke informatie voor vlucht reservering (relevant zijn voor het doel van 'Bookflight') in een app van de reservering reizen 'locatie', 'datum', 'luchtvaartmaatschappij', 'reizen class' en 'tickets'.

Vergelijking: vertegenwoordigt de bedoeling de voorspelling van de hele utterance. 

## <a name="entities-represent-data"></a>Entiteiten vertegenwoordigen gegevens
Entiteiten zijn gegevens die u wilt halen uit de utterance. Dit kan een naam, datum, productnaam of een groep van woorden zijn. 

|Utterance|Entiteit|Gegevens|
|--|--|--|
|3 kaartjes voor Den Haag kopen|Vooraf gedefinieerde getal<br>Location.Destination|3<br>New York|
|Een ticket uit New York naar Londen op 5 maart kopen|Location.Origin<br>Location.Destination<br>Vooraf gedefinieerde datetimeV2|New York<br>Londen<br>5 maart 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Entiteiten zijn optioneel, maar sterk aanbevolen
Intents zijn vereist, zijn entiteiten optioneel. U hoeft niet voor elke concept in uw app, maar alleen voor die nodig zijn voor de app om actie te ondernemen-entiteiten maken. 

Als uw utterances geen details van die uw bot nodig heeft om door te gaan, hoeft u niet wilt toevoegen. Naarmate uw app meer vormt krijgt, kunt u deze later toevoegen. 

Als u niet zeker weet hoe u de informatie wordt gebruikt, enkele algemene vooraf gemaakte entiteiten, zoals datetimeV2 rangtelwoord, e-mailadres en telefoonnummer toevoegen.

## <a name="label-for-word-meaning"></a>Label voor de betekenis van woord
Als het woord keuze of word-indeling hetzelfde is, maar niet dezelfde betekenis, niet label aan de entiteit. 

De volgende utterances, het woord `fair` een homograaf is. Hetzelfde is gespeld, maar heeft een andere betekenis:

```
What kind of county fairs are happening in the Seattle area this summer?
Is the current rating for the Seattle review fair?
```

Als u een entiteit gebeurtenis alle gebeurtenisgegevens vinden, het woord label `fair` in de eerste utterance, maar niet in de tweede.

## <a name="entities-are-shared-across-intents"></a>Entiteiten worden gedeeld door intents
Entiteiten worden gedeeld tussen intents. Ze horen niet bij elke één doel. Intents en entiteiten kunnen worden semantisch gekoppeld, maar het is niet een exclusieve relatie.

In de utterance 'boek mij een ticket naar Parijs', 'Parijs' is een entiteit van het type locatie. Door het herkennen van de entiteiten die worden vermeld in de gebruikersinvoer helpt LUIS u bij de specifieke acties te ondernemen om te voldoen aan de opzet kiezen.

## <a name="assign-entities-in-none-intent"></a>Entiteiten in geen opzet toewijzen
Alle intents, met inbegrip van de **geen** hetzelfde doel entiteiten met het label moeten hebben. Dit helpt LUIS meer informatie over waar de entiteiten zich bevindt in de utterances en woorden zijn rond de entiteiten. 

## <a name="types-of-entities"></a>Typen entiteiten
LUIS biedt veel verschillende soorten entiteiten; vooraf gedefinieerde entiteiten, aangepaste machine geleerde entiteiten en entiteiten van de lijst.

| Naam | Kan label | Beschrijving |
| -- |--|--|
| **Vooraf gedefinieerde** <br/>[Aangepaste](#prebuilt)| |  **Definitie**<br>Ingebouwde typen die algemene begrippen vertegenwoordigen. <br><br>**lijst**<br/>nummer van de belangrijkste woordgroep, rangtelwoord temperatuur, dimensie, geld, leeftijd, percentage, e, URL, telefoonnummer en sleutel woordgroep. <br><br>Namen van vooraf gedefinieerde entiteiten zijn gereserveerd. <br><br>Alle vooraf gedefinieerde entiteiten die zijn toegevoegd aan de toepassing wordt geretourneerd in de [eindpunt](luis-glossary.md#endpoint) query. Zie voor meer informatie [vooraf gemaakte entiteiten](./Pre-builtEntities.md). <br/><br/>[Voorbeeld van een antwoord voor entiteit](luis-concept-data-extraction.md#prebuilt-entity-data)|
|<!-- added week of 3/21/08 --> **Reguliere expressie**<br/>[Reguliere expressie](#regex)||**Definitie**<br>Aangepaste reguliere expressie voor opgemaakte utterance voor onbewerkte tekst. Het letters worden genegeerd en culturele variant worden genegeerd.  <br><br>Deze entiteit is geschikt voor woorden of zinnen die consistent zijn geformatteerd met een wijziging die ook consistent is.<br><br>Reguliere expressie die overeenkomt met toegepast nadat de spelling veranderingen. <br><br>Als de reguliere expressie te complex is is, zoals het gebruik van veel haakjes, bent u niet kunt toevoegen van de expressie aan het model. <br><br>**Voorbeeld**<br>`kb[0-9]{6,}` komt overeen met kb123456.<br/><br/>[Snelstartgids](luis-quickstart-intents-regex-entity.md)<br>[Voorbeeld van een antwoord voor entiteit](luis-concept-data-extraction.md)|
| **Eenvoudige** <br/>[Machine geleerd](#machine-learned) | ✔ | **Definitie**<br>Een enkele entiteit is een algemene entiteit die beschrijft een één concept en uit de context hebt geleerd van een computer is ontdekt. Context bevatten woord keuze, word plaatsing en utterance lengte.<br/><br/>Dit is een goede entiteit voor de woorden of zinnen die niet consistent zijn opgemaakt maar hetzelfde wijzen. <br/><br/>[Snelstartgids](luis-quickstart-primary-and-secondary-data.md)<br/>[Voorbeeld van een antwoord voor entiteit](luis-concept-data-extraction.md#simple-entity-data)|  
| **lijst** <br/>[Exacte overeenkomst](#exact-match)|| **Definitie**<br>Lijst entiteiten vertegenwoordigen een vaste, gesloten reeks verwante woorden samen met hun synoymns in uw systeem. <br><br>Elke entiteit lijst heeft misschien een of meer formulieren. Het meest geschikt voor een bekende set met variaties op manieren naar hetzelfde concept vertegenwoordigen.<br/><br/>LUIS detecteert geen extra waarden voor de lijst entiteiten. Gebruik de om te zien [semantische woordenlijst](luis-glossary.md#semantic-dictionary) voor suggesties voor nieuwe woorden op basis van de huidige lijst.<br/><br>Als er meer dan één entiteit van de lijst met dezelfde waarde, wordt elke entiteit in de query eindpunt geretourneerd. <br/><br/>[Snelstartgids](luis-quickstart-intent-and-list-entity.md)<br>[Voorbeeld van een antwoord voor entiteit](luis-concept-data-extraction.md#list-entity-data)| 
| **Pattern.any** <br/>[Gemengde modus](#mixed) | ✔|**Definitie**<br>Patterns.any is een tijdelijke aanduiding voor de variabele lengte alleen gebruikt in een patroon sjabloon utterance markeren waar de entiteit begint en eindigt.  <br><br>**Voorbeeld**<br>Uitgaande van een zoekopdracht utterance voor rapporten op basis van de titel, haalt de pattern.any de volledige titel. Een sjabloon utterance met pattern.any is `Who wrote {BookTitle}[?]`.<br/><br/>[Zelfstudie](luis-tutorial-pattern.md)<br>[Voorbeeld van een antwoord voor entiteit](luis-concept-data-extraction.md#composite-entity-data)|  
| **Samengestelde** <br/>[Machine geleerd](#machine-learned) | ✔|**Definitie**<br>Een samengestelde entiteit bestaat uit andere entiteiten, zoals een vooraf gedefinieerde entiteiten en eenvoudig. De afzonderlijke entiteiten vormen een hele entiteit. Lijst entiteiten zijn niet toegestaan in samengestelde entiteiten. <br><br>**Voorbeeld**<br>Een samengestelde entiteit met de naam PlaneTicketOrder wellicht onderliggende entiteiten vooraf gedefinieerde `number` en `ToLocation`. <br/><br/>[Zelfstudie](luis-tutorial-composite-entity.md)<br>[Voorbeeld van een antwoord voor entiteit](luis-concept-data-extraction.md#composite-entity-data)|  
| **Hiërarchische** <br/>[Machine geleerd](#machine-learned) |✔ | **Definitie**<br>Een hiërarchische entiteit is een categorie contextueel geleerde entiteiten.<br><br>**Voorbeeld**<br>Een hiërarchische entiteit van het gegeven `Location` met onderliggende `ToLocation` en `FromLocation`, elke onderliggende kan worden bepaald op basis van de **context** binnen de utterance. In de utterance `Book 2 tickets from Seattle to New York`, wordt de `ToLocation` en `FromLocation` contextueel verschillen op basis van de omringende woorden. <br/><br/>**Gebruik geen als**<br>Als u naar een entiteit die overeenkomt met de exacte tekst voor kinderen ongeacht context heeft zoekt, moet u een lijst-entiteit. Als u een bovenliggende / onderliggende relatie met andere Entiteitstypen zoekt, moet u de samengestelde entiteit.<br/><br/>[Snelstartgids](luis-quickstart-intent-and-hier-entity.md)<br>[Voorbeeld van een antwoord voor entiteit](luis-concept-data-extraction.md#hierarchical-entity-data)|

<a name="prebuilt"></a>
**Vooraf gedefinieerde** entiteiten worden aangepaste entiteiten geleverd door LUIS. Sommige van deze entiteiten worden gedefinieerd in de open-source [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text) project. Er zijn veel [voorbeelden](https://github.com/Microsoft/Recognizers-Text/tree/master/Specs) in de map /Specs voor de ondersteunde culturen. Als uw specifieke cultuur of de entiteit wordt momenteel niet ondersteund, dragen bij aan het project. 

<a name="machine-learned"></a>
**Machine geleerd** entiteiten werken het beste als getest [eindpunt query's](luis-concept-test.md#endpoint-testing) en [eindpunt utterances controleren](label-suggested-utterances.md). 

<a name="regex"></a>
**Reguliere expressie entiteiten** zijn gedefinieerd door een reguliere expressie die de gebruiker als onderdeel van de entiteitsdefinitie van de bevat. 

<a name="exact-match"></a>
**Exacte overeenkomst** entiteiten de tekst die is opgegeven in de entiteit gebruiken om een exacte tekst overeen te maken.

<a name="mixed"></a>
**Gemengde** entiteiten gebruik een combinatie van detectiemethoden entiteit.

## <a name="entity-limits"></a>Entiteit-limieten
Bekijk [limieten](luis-boundaries.md#model-boundaries) om te begrijpen hoeveel van elk type entiteit kunt u toevoegen aan een model.

## <a name="entity-roles"></a>Entiteit-functies
Entiteit [rollen](luis-concept-roles.md) in patronen alleen worden gebruikt. 

## <a name="composite-vs-hierarchical-entities"></a>Samengestelde tegenover hiërarchische entiteiten
Samengestelde entiteiten en hiërarchische entiteiten bovenliggende-onderliggende relaties hebben en worden geleerd machine. De machine learning kunt LUIS om te begrijpen van de entiteiten die op basis van verschillende contexten (rangschikking van woorden). Samengestelde entiteiten zijn flexibeler omdat ze verschillende Entiteitstypen als onderliggende is toegestaan. Onderliggende elementen van een hiërarchische entiteit zijn alleen eenvoudige entiteiten. 

|Type|Doel|Voorbeeld|
|--|--|--|
|Hiërarchische|Bovenliggende / onderliggende van eenvoudige entiteiten|Location.Origin=New York<br>Location.Destination=London|
|Samengestelde|Bovenliggende / onderliggende entiteiten: lijst met vooraf gedefinieerde, eenvoudige, hiërarchische| aantal = 3<br>lijst met eerste klasse =<br>prebuilt.datetimeV2=March 5|

## <a name="data-matching-multiple-entities"></a>Gegevens die overeenkomen met meerdere entiteiten
Als een woord of woordgroep overeenkomt met meer dan één entiteit, retourneert de query eindpunt elke entiteit. Als u zowel vooraf gedefinieerde nummer entiteit als prebuild datetimeV2 toevoegen, en een utterance `create meeting on 2018/03/12 for lunch with wayne`, LUIS herkent alle entiteiten en retourneert een matrix van entiteiten als onderdeel van het antwoord van JSON-eindpunt: 

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

## <a name="data-matching-multiple-list-entities"></a>Gegevens die overeenkomen met meerdere entiteiten van de lijst
Als een woord of woordgroep overeenkomt met meer dan één lijst entiteit, retourneert de query eindpunt elke entiteit lijst.

Voor de query `when is the best time to go to red rock?`, en de app heeft het woord `red` in meer dan één lijst LUIS herkent alle entiteiten en retourneert een matrix van entiteiten als onderdeel van het antwoord van JSON-eindpunt: 

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

Mogelijk moet u gebruikmaken van hiërarchische en samengestelde entiteiten. Hiërarchische entiteiten weerspiegelen de relatie tussen de entiteiten die dezelfde kenmerken of lid zijn van een categorie. De onderliggende entiteiten zijn alle leden van hun bovenliggende categorie. Bijvoorbeeld, wellicht een hiërarchische entiteit met de naam PlaneTicketClass de onderliggende entiteiten EconomyClass en bewegingssensoren. De hiërarchie omvat slechts één niveau van diepte.  

Samengestelde entiteiten vertegenwoordigen onderdelen van een volledig. Een samengestelde entiteit met de naam PlaneTicketOrder kan bijvoorbeeld onderliggende entiteiten luchtvaartmaatschappij, bestemming DepartureCity, vertrekdatum en PlaneTicketClass hebben. U maakt een samengestelde entiteit van de vooraf bestaande eenvoudige entiteiten, onderliggende elementen van hiërarchische entiteiten of vooraf gedefinieerde entiteiten.  

LUIS biedt ook het type van de lijst entiteit die niet voor machine geleerd, maar kan uw app LUIS om op te geven van een vaste lijst met waarden. Zie [LUIS grenzen](luis-boundaries.md) verwijzing naar de grenzen van het type van de entiteit lijst bekijken. 

Als u hebt besloten hiërarchische, composite, en entiteiten lijst en nog meer dan de limiet moet, moet u contact op met ondersteuning. Gedetailleerde informatie verzameld over uw systeem om dit te doen, gaat u naar de [LUIS] [ LUIS] website en selecteer vervolgens **ondersteuning**. Als uw Azure-abonnement ondersteuningsservices bevat, neem dan contact op met [Azure technische ondersteuning](https://azure.microsoft.com/support/options/). 

## <a name="best-practices"></a>Aanbevolen procedures

Maak een [entiteit](luis-concept-entity-types.md) wanneer de aanroepende toepassing of bot moet bepaalde parameters of gegevens uit de utterance uitvoeren van een actie vereist. Een entiteit is een woord of zinsdeel in de utterance die u nodig hebt uitgepakt--mogelijk als een parameter voor een functie. 

Om het juiste type entiteit toevoegen aan uw toepassing selecteert, moet u weten hoe deze gegevens worden ingevoerd door gebruikers. Elk entiteitstype is gevonden met een ander mechanisme zoals lijst van machine learning, gesloten of reguliere expressie. Als u niet zeker weet, beginnen met een enkele entiteit en het woord of zinsdeel die dat de gegevens in alle utterances over alle intents aangeeft, met inbegrip van de geen opzet label.  

Bekijk eindpunt utterances regelmatig gebruikt waarbij een entiteit kan worden geïdentificeerd als een reguliere expressie of gevonden met een exacte tekst overeenkomst vinden.  

Overweeg een woordgroepenlijst voor het toevoegen van een signaal aan LUIS voor woorden of zinnen die belangrijk zijn om uw domein, maar nog niet precies overeenkomen, en waarvoor geen LUIS een hoge betrouwbaarheid als onderdeel van de evaluatie.  

Zie [aanbevolen procedures](luis-concept-best-practices.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over concepten over goede [utterances](luis-concept-utterance.md). 

Zie [entiteiten toevoegen](luis-how-to-add-entities.md) voor meer informatie over het toevoegen van entiteiten aan uw app LUIS.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website