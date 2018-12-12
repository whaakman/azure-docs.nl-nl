---
title: Aanbevolen procedures
titleSuffix: Language Understanding - Azure Cognitive Services
description: Informatie over de LUIS aanbevolen procedures voor de beste resultaten van uw LUIS-app-model.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: e63d6ed584c0e8afc823bbcdc6e70ceabbf43db6
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085277"
---
# <a name="best-practices-for-building-a-language-understanding-app-with-cognitive-services"></a>Aanbevolen procedures voor het bouwen van een language understanding-app met Cognitive Services
Gebruik Apps kunt maken om uw LUIS-app te bouwen. 

* Taalmodel bouwen
* Een paar training voorbeeld utterances (10-15 per bedoeling) toevoegen
* Publiceren 
* Testen van eindpunt 
* Functies toevoegen

Zodra uw app is [gepubliceerd](luis-how-to-publish-app.md), gebruikt u de authoring cyclus van onderdelen toevoegen, publiceren en testen van eindpunt. De volgende cyclus authoring niet beginnen met het toevoegen van meer voorbeeld uitingen. Die kunt niet meer van uw model met echte gebruiker uitingen LUIS. 

Opdat LUIS efficiënt aan de taak van de training, niet uitbreiden de uitingen totdat de huidige set met zowel voorbeeld als eindpunt uitingen vertrouwen, hoge voorspelling scores zijn geretourneerd. Scores met actief leren, verbeteren [patronen](luis-concept-patterns.md), en [lijsten woordgroep](luis-concept-feature.md). 

## <a name="do-and-dont"></a>Voer en geen
De volgende lijst bevat aanbevolen procedures voor LUIS apps:

|Doen|Niet doen|
|--|--|
|[Verschillende intents definiëren](#do-define-distinct-intents) |[Veel voorbeeld uitingen aan intents toevoegen](#dont-add-many-example-utterances-to-intents) |
|[Een plek sweet tussen te algemeen en te specifiek voor elke doel vinden](#do-find-sweet-spot-for-intents)|[LUIS gebruiken als platform voor training](#dont-use-luis-as-a-training-platform)|
|[Bouw uw app iteratief](#do-build-the-app-iteratively)|[Toevoegen van veel voorbeeld uitingen van dezelfde indeling, andere indelingen worden genegeerd](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Woordgroep lijsten en patronen in latere iteraties toevoegen](#do-add-phrase-lists-and-patterns-in-later-iterations)|[Combineer de definitie van de intenties en entiteiten](#dont-mix-the-definition-of-intents-and-entities)|
|[Voorbeeld utterances toevoegen op None intentie](#do-add-example-utterances-to-none-intent)|[Woordgroep lijsten maken met alle mogelijke waarden](#dont-create-phrase-lists-with-all-the-possible-values)|
|[Maak gebruik van de functie voor suggesties voor actief leren](#do-leverage-the-suggest-feature-for-active-learning)|[Voeg zo veel patronen](#dont-add-many-patterns)|
|[De prestaties van uw app controleren](#do-monitor-the-performance-of-your-app)|[Trainen en publiceren met elk één voorbeeld utterance toegevoegd](#dont-train-and-publish-with-every-single-example-utterance)|

## <a name="do-define-distinct-intents"></a>Verschillende intents definiëren
Zorg ervoor dat de woordenlijst voor elk doel alleen voor dit doel en niet met een ander doel overlappende. Bijvoorbeeld, als u een app hebt die Reisarrangementen zoals vluchten en hotels verwerken wilt, kunt u deze als afzonderlijke intents of het hetzelfde doel met entiteiten voor specifieke gegevens binnen de utterance.

Als het vocabulaire tussen twee intents hetzelfde is, de bedoeling combineren en entiteiten gebruiken. 

Houd rekening met het volgende voorbeeld-uitingen:

|Voorbeelden van utterances|
|--|
|Een vlucht reserveren|
|Boek een hotel|

'Een vlucht reserveren' en 'Book een hotel' gebruikt u de dezelfde vocabulaire van ' book een '. Dit wordt overlappende, dus het mag hetzelfde doel met andere woorden van vertragingen van vluchten en hotels entiteiten hebt uitgepakt. 

## <a name="do-find-sweet-spot-for-intents"></a>Vind sweet positie voor intents
Voorspellingsgegevens van LUIS gebruiken om te bepalen als uw intenties elkaar overlappen. Overlappende intents confuses LUIS. Het resultaat is dat boven scoring-doel te sluiten bij een ander doel. LUIS gebruikt niet precies hetzelfde pad van de gegevens voor het trainen van elke keer, is overlappende opzet een kans dat het eerste of tweede in Trainingen. U wilt dat de score van de utterance voor elke voornemen verder uit elkaar liggen, zodat dit gebeurt. Goede onderscheidende factor zijn voor intents moet resulteren in het verwachte belangrijkste doel telkens. 
 
## <a name="do-build-the-app-iteratively"></a>Maak de app iteratief
Houd een afzonderlijke *blind* testen die niet wordt gebruikt als [voorbeeld uitingen](luis-concept-utterance.md) of eindpunt uitingen. Houd de verbetering van de app voor de test-verzameling. Aanpassing van de test instellen om real-user-uitingen weer te geven. Gebruik deze blind test instellen om te evalueren elke iteratie. 

Ontwikkelaars moeten drie gegevenssets hebben. De eerste is de voorbeeld-uitingen voor het bouwen van het model. De tweede is voor het testen van het model op het eindpunt. De derde is de blind gegevens die worden gebruikt test [batch testen](luis-how-to-batch-test.md). Deze laatste set is niet gebruikt in de toepassing training en verzonden op het eindpunt.  

## <a name="do-add-phrase-lists-and-patterns-in-later-iterations"></a>Voeg woordgroep lijsten en -patronen in de volgende herhalingen
[Lijsten met zin](luis-concept-feature.md) kunt u voor het definiëren van woordenlijsten van woorden met betrekking tot uw app-domein. Seed uw woordgroep lijst met een paar woorden en gebruik vervolgens de functie voorstellen zodat LUIS weet over meer woorden in de woordenlijst. Elke woord niet worden toegevoegd aan het vocabulaire, omdat de woordgroepenlijst met niet een exacte overeenkomst. 

Real-user-uitingen van het eindpunt, vergelijkbaar met elkaar worden verbonden, waarschijnlijk patronen van word keuze en plaatsing. De [patroon](luis-concept-patterns.md) functie neemt dit woord keuze en plaatsing samen met de reguliere expressies voor het verbeteren van de nauwkeurigheid van de voorspelling. Een reguliere expressie in het patroon kan woorden en leestekens die u van plan bent om te negeren terwijl nog steeds die overeenkomt met het patroon. 

Gebruik van patroon optionele syntaxis voor interpunctie zodat interpunctie kan worden genegeerd.

Deze procedures zijn niet van toepassing voordat uw app eindpunt aanvragen ontvangen heeft omdat die Hiermee laat u het vertrouwen overhellen.  

## <a name="do-add-example-utterances-to-none-intent"></a>Voeg voorbeeld uitingen op None intentie
Dit is de terugval doel aangegeven alles buiten uw toepassing. Een voorbeeld utterance toevoegen aan de intentie voor elke uitingen 10 voorbeeld in de rest van uw LUIS-app geen.

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Gebruikmaken van de functie voor suggesties voor actief leren
Gebruik [actief leren](luis-how-to-review-endoint-utt.md)van **bekijken eindpunt uitingen** regelmatig, in plaats van meer voorbeeld uitingen aan intents toe te voegen. Omdat de app voortdurend eindpunt uitingen ontvangt, wordt deze lijst groeit en wijzigen.

## <a name="do-monitor-the-performance-of-your-app"></a>De prestaties van uw app controleren
Controleer de nauwkeurigheid met behulp van een testset. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Veel voorbeeld uitingen aan intents niet toevoegen
Nadat de app is gepubliceerd, kunt u alleen uitingen van actief leren in een iteratief proces toevoegen. Als uitingen te lijken, voegt u een patroon toe. 

## <a name="dont-use-luis-as-a-training-platform"></a>LUIS niet gebruiken als platform voor training
LUIS is specifiek voor een taalmodel-domein. Het is niet bedoeld om te werken als een algemene training-platform. 

## <a name="dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats"></a>Voeg niet veel voorbeeld uitingen van dezelfde indeling, andere indelingen worden genegeerd
LUIS verwacht variaties in uitingen van een doel. De uitingen kunnen variëren terwijl dezelfde algemene betekenis. Variaties kunnen opnemen utterance lengte, word keuze en word plaatsing. 

|Gebruik niet dezelfde indeling|Gebruik verschillende opmaak|
|--|--|
|Een ticket naar Seattle kopen<br>Een ticket naar Parijs kopen<br>Een ticket naar Orlando kopen|1 ticket naar Seattle kopen<br>Reserveren van twee seats op de rode ogen naar Parijs volgende maandag<br>Ik wil graag 3 tickets te Orlando spring pauze boek|

De tweede kolom maakt gebruik van verschillende bewerkingen (kopen, reserveren, book), verschillende aantallen (1, twee, 3), en verschillende structuren van woorden, maar alle ook hetzelfde doel van het aanschaffen van vliegtickets voor reizen hebben. 

## <a name="dont-mix-the-definition-of-intents-and-entities"></a>De definitie van de intenties en entiteiten niet combineren
Maak een doel voor alle acties die uw bot duurt. Entiteiten gebruiken als parameters die deze actie mogelijk maken. 

Voor een chatbot die vluchten wordt het adresboek, maakt u een **BookFlight** intentie. Maak geen een doel voor elke luchtvaartmaatschappij of elke bestemming. Gebruik van deze onderdelen van de gegevens als [entiteiten](luis-concept-entity-types.md) en deze te markeren in de voorbeeld-uitingen. 

## <a name="dont-create-phrase-lists-with-all-the-possible-values"></a>Maak geen zin lijsten met alle mogelijke waarden
Vindt u enkele voorbeelden in de [lijsten zin](luis-concept-feature.md) , maar niet elk woord. LUIS generaliseert en context rekening wordt gehouden. 

## <a name="dont-add-many-patterns"></a>Veel patronen niet toevoegen
Voeg niet te veel [patronen](luis-concept-patterns.md). LUIS is bedoeld voor meer informatie over snel met minder voorbeelden. Het systeem niet onnodig worden overbelast.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>Geen trainen en publiceren met elk één voorbeeld utterance
10 of 15 utterances toevoegen voordat u training en publiceren. Hierdoor kunt u de impact op de nauwkeurigheid zien. Een enkele utterance toe te voegen, kan niet een zichtbaar invloed hebben op de score. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [uw app plannen](luis-how-plan-your-app.md) in uw LUIS-app.