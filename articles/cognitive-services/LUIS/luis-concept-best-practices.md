---
title: Aanbevolen procedures LUIS - Azure begrijpen | Microsoft Docs
description: Meer informatie over de LUIS aanbevolen procedures voor het ophalen van de beste resultaten.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: 729f510de59fe27761389fb1f6edb4025021565a
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35350064"
---
# <a name="best-practices"></a>Aanbevolen procedures
De app die authoring proces gebruiken om uw app LUIS te bouwen. 

* Taalmodel maken
* Enkele training voorbeeld utterances (10-15 per bedoeling) toevoegen
* Publiceren 
* Testen van eindpunt 
* Functies toevoegen

Als uw app wordt [gepubliceerd](publishapp.md), gebruikt u de authoring cyclus van functies toevoegen, publiceren en testen van eindpunt. De volgende cyclus authoring niet beginnen met het toevoegen van meer voorbeeld utterances. Die laat niet meer van uw model met echte gebruiker utterances LUIS. 

In de volgorde voor LUIS efficiënt op de taak van learning niet uitbreiden de utterances totdat de huidige set met zowel voorbeeld als eindpunt utterances erop kunnen vertrouwen, hoog voorspelling scores wilt retourneren. Verbeteren met behulp van actieve learning scores [patronen](luis-concept-patterns.md), en [een lijst met woorden](luis-concept-feature.md). 

## <a name="do-and-dont"></a>Doen en niet
De volgende lijst bevat aanbevolen procedures voor LUIS apps:

|Doen|Niet doen|
|--|--|
|[Afzonderlijke intents definiëren](#do-define-distinct-intents) |[Veel voorbeeld utterances toevoegen aan intents](#dont-add-many-example-utterances-to-intents) |
|[De positie van een sweet tussen te algemeen en te specifiek vinden voor elke doel](#do-find-sweet-spot-for-intents)|[LUIS gebruiken als een platform training](#dont-use-luis-as-a-training-platform)|
|[Uw app iteratief maken](#do-build-the-app-iteratively)|[Toevoegen van veel voorbeeld utterances van dezelfde indeling, andere indelingen worden genegeerd](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Woordgroep lijsten en patronen in de volgende herhalingen toevoegen](#do-add-phrase-lists-and-patterns-in-later-iterations)|[Meng de definitie van intents en entiteiten](#dont-mix-the-definition-of-intents-and-entities)|
|[Voorbeeld utterances op None opzet toevoegen](#do-add-example-utterances-to-none-intent)|[Woordgroep lijsten maken met alle mogelijke waarden](#dont-create-phrase-lists-with-all-the-possible-values)|
|[Gebruikmaken van de functie suggesties voor actieve learning](#do-leverage-the-suggest-feature-for-active-learning)|[Zo veel patronen toevoegen](#dont-add-many-patterns)|
|[Bewaking van de prestaties van uw app](#do-monitor-the-performance-of-your-app)|[Trainen en te publiceren met elk één voorbeeld utterance toegevoegd](#dont-train-and-publish-with-every-single-example-utterance)|

## <a name="do-define-distinct-intents"></a>Afzonderlijke intents definiëren
Controleer of dat de woordenlijst voor elk doel is alleen voor die intentie en geen overlappende met een ander doel. Bijvoorbeeld, als u een app hebt die reizen regelingen zoals luchtvaartmaatschappij vlucht en hotels verwerken wilt, kunt u deze als afzonderlijke intents of de dezelfde bedoeling van entiteiten voor specifieke gegevens binnen de utterance.

Als de woordenlijst tussen twee intents hetzelfde is, de bedoeling combineren en entiteiten gebruiken. 

Houd rekening met het volgende voorbeeld utterances:

```
Book a flight
Book a hotel
```

'Een vlucht boek' en 'Een hotel boek' gebruiken de dezelfde vocabulaire van ' adresboek een '. Dit overlapt zodat hetzelfde moet doel met de verschillende woorden vlucht en hotel uitgepakt entiteiten. 

## <a name="do-find-sweet-spot-for-intents"></a>Vind sweet positie voor intents
Gegevens van de voorspelling van LUIS gebruiken om te bepalen of uw intents elkaar overlappen. Overlappende intents confuses LUIS. Het resultaat is dat boven score berekenen voor doel te dicht bij een ander doel. Omdat LUIS geen exact hetzelfde pad via de gegevens voor het trainen van elke keer gebruikt, heeft de overlappende opzet kans van de eerste of tweede in training wordt. Wilt u de utterance score voor elke voornemen worden verder uit elkaar zodat dit niet gebeurt. Goede onderscheid voor intents moet resulteren in de verwachte bovenste bedoeling elke keer. 
 
## <a name="do-build-the-app-iteratively"></a>Bouw de app iteratief
Houd een afzonderlijke *blind* testen set die niet wordt gebruikt als [voorbeeld utterances](luis-concept-utterance.md) of utterances eindpunt. Houd de app voor uw testset verbeteren. De test is ingesteld in overeenstemming met echte gebruiker utterances aanpassen. Gebruik deze blind test ingesteld op elke herhaling evalueren. 

Ontwikkelaars moeten de drie soorten gegevens hebben. De eerste is de utterances voorbeeld voor het bouwen van het model. De tweede is voor het testen van het model op het eindpunt. Het derde is de blind testen gebruikt in [tests in batch](luis-how-to-batch-test.md). Deze laatste set is niet gebruikt in het trainen van de toepassing niet op het eindpunt is verzonden.  

## <a name="do-add-phrase-lists-and-patterns-in-later-iterations"></a>Voeg woordgroep lijsten en patronen in de volgende herhalingen
[Een lijst met woorden](luis-concept-feature.md) kunt u definiëren woordenlijsten van woorden die verwant zijn aan uw app-domein. Seed uw woordgroep lijst met een paar woorden en gebruik vervolgens de functie suggesties zodat LUIS weet over meer woorden in de woordenlijst. Voeg elk woord niet aan de woordenlijst omdat de woordgroepenlijst met niet een exacte overeenkomst. 

Echte gebruiker utterances van het eindpunt is vergelijkbaar met elkaar, waarschijnlijk patronen van word keuze en plaatsing. De [patroon](luis-concept-patterns.md) wordt dit woord keuze en plaatsing samen met de reguliere expressies voor het verbeteren van de nauwkeurigheid. Een reguliere expressie in het patroon kunt woorden en leestekens die u van plan bent te negeren tijdens het nog steeds overeenkomen met het patroon. 

Gebruik van patroon optionele syntaxis voor interpunctie zodat interpunctie kan worden genegeerd.

Deze procedures zijn niet van toepassing voordat u uw app eindpunt aanvragen heeft ontvangen, omdat dat Hiermee laat u het vertrouwen overhellen.  

## <a name="do-add-example-utterances-to-none-intent"></a>Voeg voorbeeld utterances op None opzet
Dit is de terugval hetzelfde doel aangegeven alles buiten uw toepassing. Een voorbeeld utterance toevoegen aan de opzet voor elke 10 voorbeeld utterances in de rest van uw app LUIS geen.

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Gebruikmaken van de functie suggesties voor actieve learning
Gebruik [active learning](label-suggested-utterances.md)van **bekijken eindpunt utterances** regelmatig in plaats van meer voorbeeld utterances aan intents toe te voegen. Omdat de app voortdurend eindpunt utterances ontvangt, wordt deze lijst groeien en wijzigen.

## <a name="do-monitor-the-performance-of-your-app"></a>Bewaak de prestaties van uw app
De nauwkeurigheid met behulp van een testset bewaken. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Veel voorbeeld utterances aan intents niet toevoegen
Nadat de app is gepubliceerd, kunt u alleen utterances van actieve learning in iteratief proces toevoegen. Als utterances te lijken, voegt u een patroon. 

## <a name="dont-use-luis-as-a-training-platform"></a>Gebruik LUIS niet als een platform training
LUIS is specifiek voor een taalmodel domein. Dit is niet bedoeld om te werken als een algemene training-platform. 

## <a name="dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats"></a>Voeg niet veel voorbeeld utterances van dezelfde indeling, andere indelingen worden genegeerd
LUIS verwacht variaties in de opzet utterances. De utterances kunnen variëren en dezelfde algemene betekenis hebben. Variaties kunnen opnemen utterance lengte, word keuze en word plaatsing. 

|Gebruik niet dezelfde indeling|Gebruik verschillende indeling|
|--|--|
|Een ticket voor Seattle kopen<br>Een ticket naar Parijs kopen<br>Een ticket naar Orlando kopen|1 ticket voor Seattle kopen<br>Twee plaatsen op de rode ogen Parijs reserveren volgende maandag<br>Ik wil het adresboek van 3-tickets voor Orlando spring pauze|

De tweede kolom maakt gebruik van verschillende woorden (kopen, reserveren, book), verschillende aantallen (1, twee, 3), en andere regelingen van woorden maar alle hetzelfde doel van het aanschaffen van een luchtvaartmaatschappij tickets voor reizen hebben. 

## <a name="dont-mix-the-definition-of-intents-and-entities"></a>Geen combinatie van de definitie van intents en entiteiten
De opzet voor alle acties die uw bot in werking te laten maken. Entiteiten gebruiken als parameters die deze actie mogelijk te maken. 

Voor een chatbot die luchtvaartmaatschappij vlucht wordt boek, maakt u een **BookFlight** bedoeld. Maak geen de opzet voor elke luchtvaartmaatschappij of elke doelcomputer. Gebruik deze gegevensitems als [entiteiten](luis-concept-entity-types.md) en deze markeren in de voorbeeld-utterances. 

## <a name="dont-create-phrase-lists-with-all-the-possible-values"></a>Maak geen woordgroep lijsten met alle mogelijke waarden
Enkele voorbeelden gegeven in de [een lijst met woorden](luis-concept-feature.md) maar niet elk woord. LUIS generaliseert en context rekening wordt gehouden. 

## <a name="dont-add-many-patterns"></a>Veel patronen niet toevoegen
Voeg niet te veel [patronen](luis-concept-patterns.md). LUIS is bedoeld om meer informatie over snel met minder voorbeelden. Het systeem niet onnodig worden overbelast.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>Geen trainen en publiceren met elk één voorbeeld utterance
10 of 15 utterances toevoegen voordat u training en publiceren. Hierdoor kunt u de impact op de nauwkeurigheid zien. Een enkele utterance toe te voegen, kan niet een zichtbaar invloed hebben op de score. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over hoe [plannen van uw app](plan-your-app.md) in uw app LUIS.
[LUIS]: luis-reference-regions.md #luis-website
