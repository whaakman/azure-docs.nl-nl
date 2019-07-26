---
title: Entiteitstypen
titleSuffix: Language Understanding - Azure Cognitive Services
description: 'Entiteiten halen gegevens op uit de utterance. Entiteits typen bieden u voorspel bare extractie van gegevens. Er zijn twee typen entiteiten: door de machine geleerd en niet-machine geleerde. Het is belang rijk om te weten met welk type entiteit u werkt in uitingen.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: a5a3ba8c25107317e7c47ee358f9a6ebe7d4556f
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479119"
---
# <a name="entity-types-and-their-purposes-in-luis"></a>Entiteits typen en hun doel einden in LUIS

Entiteiten halen gegevens op uit de utterance. Entiteits typen bieden u voorspel bare extractie van gegevens. Er zijn twee typen entiteiten: door de machine geleerd en niet-machine geleerde. Het is belang rijk om te weten met welk type entiteit u werkt in uitingen. 

## <a name="entity-compared-to-intent"></a>Entiteit in vergelijking met opzet

De entiteit vertegenwoordigt een woord of zinsdeel in de utterance die u wilt dat opgehaald. Een utterance kan bevatten veel entiteiten of geen helemaal. Een client toepassing heeft de entiteit mogelijk nodig om de taak uit te voeren of te gebruiken als richt lijn voor een aantal keuzes die aan de gebruiker moeten worden gepresenteerd. 

Een entiteit:

* Vertegenwoordigt een klasse met inbegrip van een verzameling vergelijk bare objecten (plaatsen, dingen, personen, gebeurtenissen of concepten). 
* Hierin wordt informatie beschreven die relevant is voor de bedoeling


Een app nieuws zoeken kan bijvoorbeeld entiteiten, zoals 'onderwerp', 'bron', 'sleutelwoord' en 'publiceren datum', die belangrijke gegevens om te zoeken naar nieuws. In een app voor het reserveren van een reis zijn de ' locatie ', ' datum ', ' luchtvaart maatschappij ', ' Travel Class ' en ' tickets ' belang rijke informatie voor het boeken van vluchten (relevant voor de bedoeling van de "boek vlucht").

Ter vergelijking, vertegenwoordigt de bedoeling de voorspelling van de gehele utterance. 

## <a name="entities-help-with-data-extraction-only"></a>Entiteiten bieden alleen ondersteuning voor gegevens extractie

U kunt entiteiten alleen labelen of markeren voor het uitpakken van de entiteit, maar niet met de voor spelling van de bedoeling.

## <a name="entities-represent-data"></a>Entiteiten vertegenwoordigen gegevens

Entiteiten zijn gegevens die u wilt gebruiken om op te halen uit de utterance. Dit kan een naam, datum, naam van het product of een groep van woorden zijn. 

|Utterance|Entiteit|Gegevens|
|--|--|--|
|3 tickets naar de New York kopen|Vooraf gedefinieerde getal<br>Location.Destination|3<br>New York|
|Een ticket van de New York naar Londen kopen op 5 maart|Location.Origin<br>Location.Destination<br>Vooraf gedefinieerde datetimeV2|New York<br>Londen<br>5 maart 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Entiteiten zijn optioneel, maar sterk aanbevolen

Intents zijn vereist, zijn entiteiten optioneel. U hoeft geen entiteiten te maken voor elk concept in uw app, maar alleen voor de toepassingen die vereist zijn voor het uitvoeren van een actie door de client toepassing. 

Als uw uitingen geen details van die uw bot moet om door te gaan, hoeft u niet wilt toevoegen. Als uw app zich verder ontwikkelt, kunt u deze later toevoegen. 

Als u niet zeker weet hoe u de informatie zou gebruiken, voegt u enkele algemene vooraf gedefinieerde entiteiten toe, zoals [datetimeV2](luis-reference-prebuilt-datetimev2.md), [rang telwoord](luis-reference-prebuilt-ordinal.md), [e-mail adres](luis-reference-prebuilt-email.md)en [telefoon nummer](luis-reference-prebuilt-phonenumber.md).

## <a name="label-for-word-meaning"></a>Label voor de betekenis van woord

Als het woord keuze of word-indeling hetzelfde is, maar geen betekent dit hetzelfde te doen dat, u deze niet labelen met de entiteit. 

De volgende uitingen, het woord `fair` een homograaf is. Deze hetzelfde is gespeld, maar heeft een andere betekenis:

|Utterance|
|--|
|Wat voor soort county beurzen plaatsvinden in het gebied van Seattle zomer van dit jaar?|
|Is de huidige classificatie voor de beoordeling Seattle geoorloofd?|

Als u een entiteit gebeurtenis te vinden van alle gegevens van de gebeurtenis, een label het woord `fair` in de eerste utterance, maar niet in de tweede.

## <a name="entities-are-shared-across-intents"></a>Entiteiten worden gedeeld via intents

Entiteiten worden gedeeld tussen intents. Ze behoren niet tot een enkel doel. Intenties en entiteiten kunnen semantisch worden gekoppeld, maar dit is geen exclusieve relatie.

In het utterance ' Book me a ticket to Parijs ' is "Parijs" een entiteit die verwijst naar locatie. Door de entiteiten die worden vermeld in de utterance van de gebruiker te herkennen, helpt LUIS uw client toepassing te kiezen welke specifieke acties moeten worden uitgevoerd om te voldoen aan de aanvraag van de gebruiker.

## <a name="mark-entities-in-none-intent"></a>Entiteiten markeren in geen intentie

Alle intenties, met inbegrip van de **geen** intentie, moeten indien mogelijk gemarkeerde entiteiten hebben. Dit helpt LUIS meer informatie over waar de entiteiten in de uitingen en wat woorden rond de entiteiten zijn. 

## <a name="entity-status-for-predictions"></a>Status van de entiteit voor voorspellingen

De LUIS-Portal vertelt u wanneer de entiteit in een voor beeld utterance afwijkt van de gemarkeerde entiteit of te dicht bij een andere entiteit ligt en daarom onduidelijk is. Dit wordt aangegeven door een rode onderstreping in het voor beeld utterance. 

Zie voor meer informatie [entiteits status](luis-how-to-add-example-utterances.md#entity-status-predictions)voor spellingen. 

## <a name="types-of-entities"></a>Typen entiteiten

LUIS biedt veel soorten entiteiten. Kies de entiteit op basis van de manier waarop de gegevens moeten worden geëxtraheerd en hoe deze moeten worden weer gegeven nadat deze is geëxtraheerd.

Entiteiten kunnen worden geëxtraheerd met machine learning, zodat LUIS kunt door gaan met het leren hoe de entiteit wordt weer gegeven in de utterance. Entiteiten kunnen worden geëxtraheerd zonder machine learning, die exact overeenkomen met de exacte tekst of een reguliere expressie. Entiteiten in patronen kunnen worden geëxtraheerd met een gemengde implementatie. 

Zodra de entiteit is geëxtraheerd, kunnen de entiteits gegevens worden weer gegeven als één gegevens eenheid of gecombineerd met andere entiteiten om een gegevens eenheid te vormen die de client toepassing kan gebruiken.

|Machine-geleerd|Kan markeren|Zelfstudie|Voorbeeld<br>Antwoord|Entiteitstype|Doel|
|--|--|--|--|--|--|
|✔|✔|[✔](luis-tutorial-composite-entity.md)|[✔](luis-concept-data-extraction.md#composite-entity-data)|[**Werken**](#composite-entity)|Groepering van entiteiten, ongeacht het entiteits type.|
|||[✔](luis-quickstart-intent-and-list-entity.md)|[✔](luis-concept-data-extraction.md#list-entity-data)|[**List**](#list-entity)|Lijst met items en de bijbehorende synoniemen die zijn geëxtraheerd met exact overeenkomende tekst.|
|Gemengd||[✔](luis-tutorial-pattern.md)|[✔](luis-concept-data-extraction.md#patternany-entity-data)|[**Pattern.any**](#patternany-entity)|Entiteit waarvan het einde van de entiteit moeilijk te bepalen is.|
|||[✔](luis-tutorial-prebuilt-intents-entities.md)|[✔](luis-concept-data-extraction.md#prebuilt-entity-data)|[**Vooraf gedefinieerde**](#prebuilt-entity)|Al getraind om verschillende soorten gegevens te extra heren.|
|||[✔](luis-quickstart-intents-regex-entity.md)|[✔](luis-concept-data-extraction.md#regular-expression-entity-data)|[**Reguliere expressie**](#regular-expression-entity)|Gebruikt reguliere expressie om tekst overeen te laten komen.|
|✔|✔|[✔](luis-quickstart-primary-and-secondary-data.md)|[✔](luis-concept-data-extraction.md#simple-entity-data)|[**Simple**](#simple-entity)|Bevat één concept in Word of woord groep.|

Alleen entiteiten die door machines zijn geleerd, moeten worden gemarkeerd in het voor beeld uitingen. Door machines geleerde entiteiten werken het beste wanneer ze worden getest via [eindpunt query's](luis-concept-test.md#endpoint-testing) en het [controleren van eind punt uitingen](luis-how-to-review-endoint-utt.md). 

Patroon. alle entiteiten moeten worden gemarkeerd in de voor beelden van [patroon](luis-how-to-model-intent-pattern.md) sjablonen, niet de intentie gebruikers voorbeelden. 

Gemengde entiteiten gebruiken een combi natie van methoden voor het detecteren van entiteiten.

## <a name="machine-learned-entities-use-context"></a>Door machine geleerde entiteiten gebruiken context

Door de machine geleerd entiteiten leren van de context in het utterance. Dit maakt variatie van de plaatsing in voor beeld uitingen aanzienlijk. 

## <a name="non-machine-learned-entities-dont-use-context"></a>Niet door machines geleerde entiteiten gebruiken geen context

De volgende niet-computer geleerde entiteiten nemen utterance-context niet in aanmerking bij overeenkomende entiteiten: 

* [Vooraf gemaakte entiteiten](#prebuilt-entity)
* [Regex-entiteiten](#regular-expression-entity)
* [Lijst met entiteiten](#list-entity) 

Voor deze entiteiten is het model niet gelabeld of getraind. Wanneer u de entiteit toevoegt of configureert, worden de entiteiten geëxtraheerd. De balans is dat deze entiteiten kunnen worden verzorgd, waarbij de overeenkomst niet in aanmerking komt voor de relatie. 

Dit gebeurt regel matig in nieuwe modellen. U bouwt en test uw model met een lijst entiteit, maar wanneer u uw model publiceert en query's ontvangt van het eind punt, is het mogelijk dat uw model overkomt als gevolg van een gebrek aan context. 

Als u wilt zoeken naar woorden of zinsdelen en u rekening moet houden, hebt u twee opties. De eerste is het gebruik van een eenvoudige entiteit die is gekoppeld aan een woordgroepen lijst. De lijst met zinsdelen wordt niet gebruikt voor een overeenkomende functie, maar in plaats daarvan worden relatief vergelijk bare woorden (Verwissel bare lijst) gesignaleerd. Als u een exacte overeenkomst moet hebben in plaats van de variaties van een woordgroepen lijst, gebruikt u een lijst entiteit met een rol, zoals hieronder wordt beschreven.

### <a name="context-with-non-machine-learned-entities"></a>Context met niet door machines geleerde entiteiten

Als u de context van de utterance voor niet-computer geleerde entiteiten wilt weten, moet u [rollen](luis-concept-roles.md)gebruiken.

Als u een entiteit hebt die niet door een machine is geleerd, zoals [vooraf gemaakte entiteiten](#prebuilt-entity), [regex](#regular-expression-entity) -entiteiten of [lijst](#list-entity) entiteiten die overeenkomen met de instantie die u wilt, kunt u een entiteit maken met twee rollen. Met één rol wordt vastgelegd wat u zoekt, en met één rol wordt vastgelegd wat u niet zoekt. Beide versies moeten worden gelabeld in voor beeld uitingen.  

## <a name="composite-entity"></a>Samengestelde entiteit

Een [samengestelde entiteit](reference-entity-composite.md) bestaat uit andere entiteiten, zoals vooraf gebouwde entiteiten, eenvoudige, reguliere expressies en lijst entiteiten. De afzonderlijke entiteiten vormen een hele entiteit. 

## <a name="list-entity"></a>Lijstentiteit

[Lijst entiteiten](reference-entity-list.md) vertegenwoordigen een vaste, gesloten set verwante woorden samen met hun synoniemen. LUIS detecteert geen aanvullende waarden voor de lijst met entiteiten. Gebruik de **raden** functie om te bekijken van suggesties voor nieuwe woorden op basis van de huidige lijst. Als er meer dan één lijst entiteit met dezelfde waarde, wordt elke entiteit in de query eindpunt geretourneerd. 

## <a name="patternany-entity"></a>Pattern.any-entiteit

[Patroon. any](reference-entity-pattern-any.md) is een tijdelijke aanduiding met variabele lengte die alleen wordt gebruikt in de sjabloon van een patroon utterance om te markeren waar de entiteit begint en eindigt.  

## <a name="prebuilt-entity"></a>Vooraf gemaakte entiteiten

Vooraf gemaakte entiteiten zijn ingebouwde typen die algemene concepten vertegenwoordigen, zoals e-mail, URL en telefoon nummer. Namen van vooraf gemaakte entiteiten zijn gereserveerd. [Alle vooraf gemaakte entiteiten](luis-prebuilt-entities.md) die worden toegevoegd aan de toepassing, worden geretourneerd in de Voorspellings query van het eind punt als ze worden gevonden in de utterance. 

De entiteit is goed geschikt wanneer:

* De gegevens komen overeen met een veelvoorkomende use-case die wordt ondersteund door vooraf gemaakte entiteiten voor uw taal cultuur. 

Vooraf gemaakte entiteiten kunnen op elk gewenst moment worden toegevoegd en verwijderd.

![Aantal vooraf gedefinieerde entiteit](./media/luis-concept-entities/number-entity.png)

[Zelfstudie](luis-tutorial-prebuilt-intents-entities.md)<br>
[Voor beeld van JSON-antwoord voor entiteit](luis-concept-data-extraction.md#prebuilt-entity-data)

Sommige van deze vooraf gemaakte entiteiten worden gedefinieerd in het open source- [tekst](https://github.com/Microsoft/Recognizers-Text) project voor herkenning. Als uw specifieke cultuur of de entiteit wordt momenteel niet ondersteund, dragen bij aan het project. 

### <a name="troubleshooting-prebuilt-entities"></a>Problemen met vooraf gemaakte entiteiten oplossen

Als in de LUIS-Portal een vooraf samengestelde entiteit is gelabeld in plaats van uw aangepaste entiteit, hebt u enkele opties om dit op te lossen.

De vooraf gemaakte entiteiten die aan de app worden toegevoegd, worden _altijd_ geretourneerd, zelfs als de utterance aangepaste entiteiten moet extra heren voor dezelfde tekst. 

#### <a name="change-tagged-entity-in-example-utterance"></a>Een gecodeerde entiteit wijzigen in bijvoorbeeld utterance

Als de vooraf samengestelde entiteit dezelfde tekst of tokens als de aangepaste entiteit is, selecteert u de tekst in het voor beeld utterance en wijzigt u de gecodeerde utterance. 

Als de vooraf samengestelde entiteit is gelabeld met meer tekst of tokens dan uw aangepaste entiteit, hebt u een aantal opties om dit op te lossen:

* [Voor beeld van utterance](#remove-example-utterance-to-fix-tagging) -methode verwijderen
* [Vooraf samengestelde entiteits methode verwijderen](#remove-prebuilt-entity-to-fix-tagging)

#### <a name="remove-example-utterance-to-fix-tagging"></a>Voor beeld-utterance verwijderen om labels te herstellen 

De eerste keus is het verwijderen van het voor beeld utterance. 

1. Verwijder het voor beeld utterance.
1. Train de app opnieuw. 
1. Voeg alleen het woord of de woord groep toe die de entiteit is, die is gemarkeerd als een vooraf gedefinieerde entiteit, als een volledig voor beeld van utterance. Het woord of de woord groep heeft nog steeds de vooraf gemaakte entiteit gemarkeerd. 
1. Selecteer de entiteit in het voor beeld-utterance op de pagina **intentie** en wijzig uw aangepaste entiteit en Train opnieuw. Zo wordt voor komen dat LUIS deze exacte tekst als de vooraf gedefinieerde entiteit markeert in een voor beeld van uitingen die die tekst gebruiken. 
1. Voeg het hele oorspronkelijke voor beeld utterance terug naar het doel. De aangepaste entiteit moet blijven gemarkeerd in plaats van de vooraf samengestelde entiteit. Als de aangepaste entiteit niet is gemarkeerd, moet u meer voor beelden van die tekst toevoegen in uitingen.

#### <a name="remove-prebuilt-entity-to-fix-tagging"></a>Vooraf gedefinieerde entiteit verwijderen om labels te herstellen

1. Verwijder de vooraf opgebouwde entiteit uit de app. 
1. Markeer op de pagina **intentie** de aangepaste entiteit in het voor beeld utterance.
1. Train de app.
1. Voeg de vooraf opgebouwde entiteit weer toe aan de app en train de app. In deze oplossing wordt ervan uitgegaan dat de vooraf samengestelde entiteit geen deel uitmaakt van een samengestelde entiteit.

## <a name="regular-expression-entity"></a>Een entiteit in de vorm van een reguliere expressie 

Een [reguliere expressie-entiteit](reference-entity-regular-expression.md) extraheert een entiteit op basis van een reguliere expressie patroon dat u opgeeft.

## <a name="simple-entity"></a>Eenvoudige entiteit

Een [eenvoudige entiteit](reference-entity-simple.md) is een waarde hebt geleerd van een machine. Een woord of woordgroep kan het zijn.
## <a name="entity-limits"></a>Limieten voor entiteit

Beoordeling [limieten](luis-boundaries.md#model-boundaries) om te begrijpen hoe vaak elk type entiteit kunt u toevoegen aan een model.

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Als u meer dan het maximum aantal entiteiten 

Mogelijk moet u samengestelde entiteiten gebruiken in combi natie met entiteits rollen.

Samengestelde entiteiten vertegenwoordigen onderdelen van een volledig. Een samengestelde entiteit met de naam PlaneTicketOrder kan bijvoorbeeld onderliggende entiteiten luchtvaartmaatschappij, doel, DepartureCity, vertrekdatum en PlaneTicketClass hebben.

LUIS biedt ook het entiteits type van de lijst dat niet door de machine is geleerd, maar de LUIS-app kan een vaste lijst met waarden opgeven. Zie [LUIS grenzen](luis-boundaries.md) verwijzing naar de grenzen van het type van de entiteit lijst bekijken. 

Neem contact op met de ondersteuning als u deze entiteiten hebt gezien en nog steeds meer dan de limiet nodig hebt. Om dit te doen, verzamelt gedetailleerde informatie over uw systeem, gaat u naar de [LUIS](luis-reference-regions.md#luis-website) website, en selecteer vervolgens **ondersteuning**. Als uw Azure-abonnement voor ondersteuningsservices bevat, neem dan contact op met [technische ondersteuning van Azure](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Volgende stappen

Informatie over concepten over goede [uitingen](luis-concept-utterance.md). 

Zie [entiteiten toevoegen](luis-how-to-add-entities.md) voor meer informatie over entiteiten toevoegen aan uw LUIS-app.
