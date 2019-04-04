---
title: Entiteitstypen
titleSuffix: Language Understanding - Azure Cognitive Services
description: 'Entiteiten extraheren gegevens uit de utterance. Entiteitstypen bieden u voorspelbare extractie van gegevens. Er zijn twee soorten entiteiten: machine geleerd en niet-machine-hebt geleerd. Het is belangrijk te weten welk type entiteit die u in uitingen werkt.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: diberry
ms.openlocfilehash: 6e37466145af58a52a86a08a2a873e406c99b9e5
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895542"
---
# <a name="entity-types-and-their-purposes-in-luis"></a>Entiteitstypen en hun ten behoeve van LUIS

Entiteiten extraheren gegevens uit de utterance. Entiteitstypen bieden u voorspelbare extractie van gegevens. Er zijn twee soorten entiteiten: machine geleerd en niet-machine-hebt geleerd. Het is belangrijk te weten welk type entiteit die u in uitingen werkt. 

## <a name="entity-compared-to-intent"></a>Entiteit in vergelijking met opzet

De entiteit vertegenwoordigt een woord of zinsdeel in de utterance die u wilt dat opgehaald. Een utterance kan bevatten veel entiteiten of geen helemaal. Een entiteit vertegenwoordigt een klasse met inbegrip van een verzameling van soortgelijke objecten (plaatsen, dingen, personen, gebeurtenissen en concepten). Entiteiten informatie die relevant zijn voor de bedoeling beschrijven en soms ze zijn essentieel voor uw app de taak uit te voeren. Een app nieuws zoeken kan bijvoorbeeld entiteiten, zoals 'onderwerp', 'bron', 'sleutelwoord' en 'publiceren datum', die belangrijke gegevens om te zoeken naar nieuws. Zijn belangrijke informatie voor vlucht reserveren (relevant zijn voor het doel 'Book flight') in een reservering reis-app, de 'locatie', 'datum', "luchtvaartmaatschappij", "reizen class" en 'tickets'.

Ter vergelijking, vertegenwoordigt de bedoeling de voorspelling van de gehele utterance. 

## <a name="entities-help-with-data-extraction-only"></a>Entiteiten help met alleen gegevensextractie

U een label of entiteiten is ingeschakeld voor de entiteit extractie alleen it niet eenvoudiger door intentie voorspelling.

## <a name="entities-represent-data"></a>Entiteiten vertegenwoordigen gegevens

Entiteiten zijn gegevens die u wilt gebruiken om op te halen uit de utterance. Dit kan een naam, datum, naam van het product of een groep van woorden zijn. 

|Utterance|Entiteit|Gegevens|
|--|--|--|
|3 tickets naar de New York kopen|Vooraf gedefinieerde getal<br>Location.Destination|3<br>New York|
|Een ticket van de New York naar Londen kopen op 5 maart|Location.Origin<br>Location.Destination<br>Vooraf gedefinieerde datetimeV2|New York<br>Londen<br>5 maart 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Entiteiten zijn optioneel, maar sterk aanbevolen

Intents zijn vereist, zijn entiteiten optioneel. U hoeft niet te maken van entiteiten voor elke concept in uw app, maar alleen voor die vereist zijn voor de clienttoepassing om actie te ondernemen. 

Als uw uitingen geen details van die uw bot moet om door te gaan, hoeft u niet wilt toevoegen. Als uw app zich verder ontwikkelt, kunt u deze later toevoegen. 

Als u niet zeker weet hoe u de gegevens wilt gebruiken, voegt u enkele algemene vooraf gemaakte entiteiten zoals [datetimeV2](luis-reference-prebuilt-datetimev2.md), [rangtelwoord](luis-reference-prebuilt-ordinal.md), [e](luis-reference-prebuilt-email.md), en [telefoonnummer ](luis-reference-prebuilt-phonenumber.md).

## <a name="label-for-word-meaning"></a>Label voor de betekenis van woord

Als het woord keuze of word-indeling hetzelfde is, maar geen betekent dit hetzelfde te doen dat, u deze niet labelen met de entiteit. 

De volgende uitingen, het woord `fair` een homograaf is. Deze hetzelfde is gespeld, maar heeft een andere betekenis:

|Utterance|
|--|
|Wat voor soort county beurzen plaatsvinden in het gebied van Seattle zomer van dit jaar?|
|Is de huidige classificatie voor de beoordeling Seattle geoorloofd?|

Als u een entiteit gebeurtenis te vinden van alle gegevens van de gebeurtenis, een label het woord `fair` in de eerste utterance, maar niet in de tweede.

## <a name="entities-are-shared-across-intents"></a>Entiteiten worden gedeeld via intents

Entiteiten worden gedeeld tussen intents. Ze behoren niet tot een enkel doel. Intenties en entiteiten kunnen semantisch gekoppeld, maar het is niet een exclusieve relatie.

In de utterance 'Book me een ticket naar Parijs","Parijs"is een entiteit die verwijzen naar de locatie. Door het herkennen van de entiteiten die worden vermeld in van de gebruiker utterance, kunt LUIS u uw clienttoepassing kiest u de specifieke acties te ondernemen om te voldoen aan de aanvraag van de gebruiker.

## <a name="mark-entities-in-none-intent"></a>Markeren van entiteiten in geen intentie

Alle intents, met inbegrip van de **geen** doel, entiteiten, indien mogelijk moeten zijn gemarkeerd. Dit helpt LUIS meer informatie over waar de entiteiten in de uitingen en wat woorden rond de entiteiten zijn. 

## <a name="entity-status-for-predictions"></a>Status van de entiteit voor voorspellingen

De LUIS-portal wordt aangegeven wanneer de entiteit in een voorbeeld-utterance is anders dan de gemarkeerde entiteit of te sluiten met een andere entiteit en daarom niet duidelijk is. Hiermee wordt aangegeven door een rood onderstreept in de voorbeeld-utterance. 

Zie voor meer informatie, [voorspellingen van de Status van de entiteit](luis-how-to-add-example-utterances.md#entity-status-predictions). 

## <a name="types-of-entities"></a>Typen entiteiten

LUIS biedt veel soorten entiteiten. Kies de entiteit op basis van hoe de gegevens moeten worden geëxtraheerd en hoe deze moet worden weergegeven nadat deze is uitgepakt.

Entiteiten kunnen worden geëxtraheerd met machine-learning, waardoor LUIS om meer over hoe de entiteit wordt weergegeven in de utterance. Entiteiten kunnen worden geëxtraheerd zonder machine learning, die overeenkomt met de exacte tekst of een reguliere expressie. Entiteiten in patronen kunnen worden geëxtraheerd met een gemengde-implementatie. 

Nadat de entiteit is uitgepakt, kunt u de entiteitsgegevens weergegeven als één eenheid van informatie of in combinatie met andere entiteiten om te vormen een eenheid met gegevens die de client-toepassing kunt gebruiken.

|Machine geleerd|Can Mark|Zelfstudie|Voorbeeld<br>Antwoord|Entiteitstype|Doel|
|--|--|--|--|--|--|
|✔|✔|[✔](luis-tutorial-composite-entity.md)|[✔](luis-concept-data-extraction.md#composite-entity-data)|[**Samengestelde**](#composite-entity)|Groepering van entiteiten, ongeacht het entiteitstype.|
|✔|✔|[✔](luis-quickstart-intent-and-hier-entity.md)|[✔](luis-concept-data-extraction.md#hierarchical-entity-data)|[**Hiërarchische**](#hierarchical-entity)|Groepering van eenvoudige entiteiten.|
|||[✔](luis-quickstart-intent-and-list-entity.md)|[✔](luis-concept-data-extraction.md#list-entity-data)|[**Lijst**](#list-entity)|Lijst met items en hun synoniemen geëxtraheerd met exact overeenkomende tekst overeenkomen.|
|Gemengd||[✔](luis-tutorial-pattern.md)|[✔](luis-concept-data-extraction.md#patternany-entity-data)|[**Pattern.any**](#patternany-entity)|De entiteit waarin einde van de entiteit moeilijk is te bepalen.|
|||[✔](luis-tutorial-prebuilt-intents-entities.md)|[✔](luis-concept-data-extraction.md#prebuilt-entity-data)|[**Vooraf gedefinieerde**](#prebuilt-entity)|Al getraind om op te halen van verschillende soorten gegevens.|
|||[✔](luis-quickstart-intents-regex-entity.md)|[✔](luis-concept-data-extraction.md#regular-expression-entity-data)|[**Gewone expressie**](#regular-expression-entity)|Maakt gebruik van reguliere expressie zodat deze overeenkomen met de tekst.|
|✔|✔|[✔](luis-quickstart-primary-and-secondary-data.md)|[✔](luis-concept-data-extraction.md#simple-entity-data)|[**Eenvoudig**](#simple-entity)|Een enkele concept in woord of woordgroep bevat.|

Alleen entiteiten hebt geleerd van een Machine moeten worden gemarkeerd in de voorbeeld-uitingen voor elke doel. Machine geleerde entiteiten werken het beste wanneer getest [eindpunt query's](luis-concept-test.md#endpoint-testing) en [eindpunt uitingen controleren](luis-how-to-review-endpoint-utterances.md). 

Pattern.any entiteiten moeten worden gemarkeerd de [patroon](luis-how-to-model-intent-pattern.md) sjabloonvoorbeelden, niet de bedoeling gebruiker voorbeelden. 

Gemengde entiteiten maken gebruik van een combinatie van detectiemethoden entiteit.

## <a name="composite-entity"></a>Samengestelde entiteit

Een samengestelde entiteit is opgebouwd uit andere entiteiten, zoals de vooraf gemaakte entiteiten eenvoudig, reguliere expressie, lijst en hiërarchische entiteiten. De afzonderlijke entiteiten vormen een hele entiteit. 

Deze entiteit is een goede passen wanneer de gegevens:

* Aan elkaar zijn gerelateerd. 
* Ze zijn aan elkaar gerelateerd in de context van de utterance.
* Verschillende Entiteitstypen gebruiken.
* Moet worden gegroepeerd en verwerkt door de clienttoepassing als een eenheid van gegevens.
* Beschikken over een verscheidenheid aan gebruiker uitingen waarvoor machine learning.

![Samengestelde entiteit](./media/luis-concept-entities/composite-entity.png)

[Zelfstudie](luis-tutorial-composite-entity.md)<br>
[Voorbeeld van JSON-antwoord voor entiteit](luis-concept-data-extraction.md#composite-entity-data)<br>

## <a name="hierarchical-entity"></a>Hiërarchische entiteit

Een hiërarchische entiteit is een categorie contextueel geleerde eenvoudige entiteiten kinderen genoemd.

Deze entiteit is een goede passen wanneer de gegevens:

* Het zijn entiteiten van het type Simple.
* Ze zijn aan elkaar gerelateerd in de context van de utterance.
* Gebruik specifieke word keuze om aan te geven van elke onderliggende entiteit. Voorbeelden van deze woorden zijn: van/naar verlaten/gaan naar, weg van/naar.
* Kinderen zijn vaak in de dezelfde utterance. 
* Moet als eenheid informatie worden gegroepeerd en verwerkt door de client-app.

Gebruik niet als:

* U moet een entiteit die overeenkomt met de exacte tekst voor kinderen, ongeacht de context. Gebruik een [entiteit lijst](#list-entity) in plaats daarvan. 
* U moet een entiteit voor een bovenliggende / onderliggende relatie met andere Entiteitstypen. Gebruik de [samengestelde entiteit](#composite-entity).

![hiërarchische entiteit](./media/luis-concept-entities/hierarchical-entity.png)

[Zelfstudie](luis-quickstart-intent-and-hier-entity.md)<br>
[Voorbeeld van JSON-antwoord voor entiteit](luis-concept-data-extraction.md#hierarchical-entity-data)<br>

### <a name="roles-versus-hierarchical-entities"></a>Functies ten opzichte van hiërarchische entiteiten

[Rollen](luis-concept-roles.md#roles-versus-hierarchical-entities) oplossen van een patroon hetzelfde probleem als hiërarchische entiteiten, maar van toepassing op alle Entiteitstypen zijn. Rollen zijn momenteel alleen beschikbaar in de patronen. Rollen zijn niet beschikbaar in de intents voorbeeld uitingen.  

## <a name="list-entity"></a>Lijstentiteit

Lijst met entiteiten vertegenwoordigen een vaste en gesloten set verwante woorden samen met hun synoniemen. LUIS detecteert geen aanvullende waarden voor de lijst met entiteiten. Gebruik de **raden** functie om te bekijken van suggesties voor nieuwe woorden op basis van de huidige lijst. Als er meer dan één lijst entiteit met dezelfde waarde, wordt elke entiteit in de query eindpunt geretourneerd. 

De entiteit is een goede passen wanneer de gegevens:

* Een bekende set zijn.
* De set maximale [begrenzingen](luis-boundaries.md) van LUIS voor dit entiteitstype niet overschrijdt.
* De tekst in de utterance is een exact overeenkomst met een synoniem of de canonieke naam. LUIS gebruikt de lijst alleen voor exact tekstovereenkomsten. Als gevolg meervouden en andere variaties niet worden opgelost met een lijst met entiteit. Hiervoor kunt u overwegen een [patroon](luis-concept-patterns.md#syntax-to-mark-optional-text-in-a-template-utterance) met de optionele tekstsyntaxis te gebruiken.

![lijst met entiteiten](./media/luis-concept-entities/list-entity.png)

[Zelfstudie](luis-quickstart-intent-and-list-entity.md)<br>
[Voorbeeld van JSON-antwoord voor entiteit](luis-concept-data-extraction.md#list-entity-data)

## <a name="patternany-entity"></a>Pattern.any-entiteit

Pattern.any is een tijdelijke aanduiding voor de variabele lengte gebruikt alleen in een patroon van sjabloon utterance markeren waar de entiteit begint en eindigt.  

De entiteit is een goede aanpassen wanneer:

* Het einde van de entiteit kan worden verward met de resterende tekst van de utterance. 
[Zelfstudie](luis-tutorial-pattern.md)<br>
[Voorbeeld van JSON-antwoord voor entiteit](luis-concept-data-extraction.md#patternany-entity-data)

**Voorbeeld**  
Een clienttoepassing die zoekt naar opgegeven voor rapporten op basis van de titel, haalt de pattern.any de volledige titel. Een sjabloon utterance pattern.any gebruiken voor deze zoekopdracht book is `Was {BookTitle} written by an American this year[?]`. 

In de volgende tabel heeft elke rij twee versies van de utterance. De bovenste utterance is hoe LUIS in eerste instantie zien de utterance, waar het is onduidelijk met de titel van het boek begint en eindigt. De onderste utterance is hoe LUIS weet de titel van het boek wanneer een patroon voor het ophalen van is. 

|Utterance|
|--|
|Is de Man die aangezien zijn vrouw voor een Hat en andere klinische verhalen geschreven door een Amerikaans dit jaar?<br>Is **de Man die aangezien zijn vrouw voor een Hat en andere klinische verhalen** geschreven door een Amerikaans dit jaar?|
|Is de helft slaapstand in kikker Pajamas geschreven door een Amerikaans dit jaar?<br>Is **halve slaapstand in kikker Pajamas** geschreven door een Amerikaans dit jaar?|
|De specifieke verdriet van citroensap taart is: Een nieuwe geschreven door een Amerikaans dit jaar?<br>Is **de bepaalde verdriet van citroensap taart: Een nieuwe** die is geschreven door een Amerikaans dit jaar?|
|Is dat er sprake is van een Wocket In mijn Pocket! die is geschreven door een Amerikaans dit jaar?<br>Is **er is een Wocket In mijn Pocket!** die is geschreven door een Amerikaans dit jaar?|

## <a name="prebuilt-entity"></a>Vooraf gemaakte entiteiten

Vooraf gemaakte entiteiten zijn ingebouwde typen die staan voor algemene concepten, zoals e-mail, URL en telefoonnummer. Namen van vooraf gemaakte entiteiten zijn gereserveerd. [Alle vooraf gedefinieerde entiteiten](luis-prebuilt-entities.md) die zijn toegevoegd aan de toepassing in de voorspelling eindpunt query worden geretourneerd als ze worden gevonden in de utterance. 

De entiteit is een goede aanpassen wanneer:

* De gegevens komt overeen met een algemene use-case wordt ondersteund door vooraf gemaakte entiteiten voor uw taal-cultuur. 

Vooraf gemaakte entiteiten kunnen worden toegevoegd en verwijderd op elk gewenst moment.

![Aantal vooraf gedefinieerde entiteit](./media/luis-concept-entities/number-entity.png)

[Zelfstudie](luis-tutorial-prebuilt-intents-entities.md)<br>
[Voorbeeld van JSON-antwoord voor entiteit](luis-concept-data-extraction.md#prebuilt-entity-data)

Sommige van deze vooraf gemaakte entiteiten zijn gedefinieerd in de open-source [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text) project. Als uw specifieke cultuur of de entiteit wordt momenteel niet ondersteund, dragen bij aan het project. 

### <a name="troubleshooting-prebuilt-entities"></a>Oplossen van problemen met vooraf gemaakte entiteiten

In de portal LUIS als een vooraf gedefinieerde entiteit is gemarkeerd in plaats van uw aangepaste entiteit, hebt u een paar opties van hoe u dit probleem oplossen.

De vooraf gemaakte entiteiten die zijn toegevoegd aan de app wordt _altijd_ worden geretourneerd, zelfs als de utterance aangepaste entiteiten voor dezelfde tekst te extraheren. 

#### <a name="change-tagged-entity-in-example-utterance"></a>Met tags entiteit in voorbeeld utterance wijzigen

Als de vooraf gedefinieerde entiteit de dezelfde tekst of tokens als de aangepaste entiteit is, selecteert u de tekst in het voorbeeld utterance en wijzigen van de gemarkeerde utterance. 

Als de vooraf gedefinieerde entiteit is gemarkeerd met meer tekst- of -tokens dan uw aangepaste entiteit, hebt u een aantal opties van hoe u dit probleem oplossen:

* [Voorbeeld utterance verwijderen](#remove-example-utterance-to-fix-tagging) methode
* [Vooraf gedefinieerde entiteit verwijderen](#remove-prebuilt-entity-to-fix-tagging) methode

#### <a name="remove-example-utterance-to-fix-tagging"></a>Voorbeeld utterance om op te lossen de tags verwijderen 

Uw eerste keuze is voor het verwijderen van de voorbeeld-utterance. 

1. De voorbeeld-utterance verwijderen.
1. Opnieuw trainen de app. 
1. Alleen het woord weer toevoegen of meer woorden die de entiteit die is gemarkeerd als een vooraf gedefinieerde entiteit, als een volledig voorbeeld utterance. Het woord of zinsdeel heeft nog steeds de vooraf gedefinieerde entiteit gemarkeerd. 
1. Selecteer de entiteit in de voorbeeld-utterance op de **bedoeling** pagina en wijzigen met uw aangepaste entiteit en opnieuw trainen. Hierdoor moet LUIS deze exact overeenkomende tekst markeren als de vooraf gedefinieerde entiteit in een voorbeeld-uitingen die gebruikmaken van deze tekst. 
1. De volledige oorspronkelijke voorbeeld utterance terug naar het doel toevoegen De aangepaste entiteit moet nog steeds in plaats van de vooraf gedefinieerde entiteit worden gemarkeerd. Als de aangepaste entiteit niet is gemarkeerd, moet u meer voorbeelden van deze tekst in uitingen toevoegen.

#### <a name="remove-prebuilt-entity-to-fix-tagging"></a>Vooraf gedefinieerde entiteit om op te lossen de tags verwijderen

1. De vooraf gedefinieerde entiteit verwijderen uit de app. 
1. Op de **bedoeling** pagina, de aangepaste entiteit in het voorbeeld utterance markeren.
1. Train de app.
1. De vooraf gedefinieerde entiteit terug naar de app toevoegen en de app trainen. Deze oplossing wordt ervan uitgegaan dat de vooraf gedefinieerde entiteit maakt geen deel uit van een samengestelde entiteit.

## <a name="regular-expression-entity"></a>Een entiteit in de vorm van een reguliere expressie 

Een reguliere expressie wordt aanbevolen voor utterance onbewerkte tekst. Deze aanvraag wordt genegeerd en worden genegeerd culturele variant.  Reguliere expressie die overeenkomt met is na spellingcontrole wijzigingen op het niveau van het teken, niet het niveau van de token toegepast. Als de reguliere expressie te complex is is, zoals het gebruik van veel vierkante haken, bent u niet de expressie toevoegen aan het model. Maakt gebruik van onderdeel maar niet alle van de [.NET Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions) bibliotheek. 

De entiteit is een goede aanpassen wanneer:

* De gegevens worden consistent met een wijziging die ook consistent opgemaakt.
* De reguliere expressie hoeft niet meer dan 2 geneste niveaus. 

![Een entiteit in de vorm van een reguliere expressie](./media/luis-concept-entities/regex-entity.png)

[Zelfstudie](luis-quickstart-intents-regex-entity.md)<br>
[Voorbeeld van JSON-antwoord voor entiteit](luis-concept-data-extraction.md#regular-expression-entity-data)<br>

## <a name="simple-entity"></a>Eenvoudige entiteit 

Een enkele entiteit is een algemene entiteit die beschrijft een enkele concept en in de context hebt geleerd van een machine wordt geleerd. Omdat de eenvoudige entiteiten zijn algemeen namen, zoals namen van bedrijven, productnamen of andere categorieën van namen, toevoegen een [woordgroepenlijst](luis-concept-feature.md) bij het gebruik van een enkele entiteit om het signaal van de namen die worden gebruikt te verbeteren. 

De entiteit is een goede aanpassen wanneer:

* De gegevens consistent zijn niet ingedeeld maar duiden op hetzelfde te doen. 

![eenvoudige entiteit](./media/luis-concept-entities/simple-entity.png)

[Zelfstudie](luis-quickstart-primary-and-secondary-data.md)<br/>
[Voorbeeld van de reactie voor entiteit](luis-concept-data-extraction.md#simple-entity-data)<br/>

## <a name="entity-limits"></a>Limieten voor entiteit

Beoordeling [limieten](luis-boundaries.md#model-boundaries) om te begrijpen hoe vaak elk type entiteit kunt u toevoegen aan een model.

## <a name="composite-vs-hierarchical-entities"></a>Samengestelde vs hiërarchische entiteiten

Samengestelde entiteiten en hiërarchische entiteiten zowel hebben relaties tussen bovenliggende en onderliggende en machine hebt geleerd. De machine learning kunt LUIS om te begrijpen van de entiteiten op basis van verschillende contexten (rangschikking van woorden). Samengestelde entiteiten zijn flexibel omdat ze verschillende Entiteitstypen als onderliggende items toegestaan. Een hiërarchische entiteit kinderen zijn alleen eenvoudige entiteiten. 

|Type|Doel|Voorbeeld|
|--|--|--|
|Hiërarchische|Bovenliggende / onderliggende van eenvoudige entiteiten|Location.Origin=New York<br>Location.Destination=London|
|Samengestelde|Bovenliggende / onderliggende entiteiten: vooraf gemaakte, lijst met eenvoudige, hiërarchische| getal = 3<br>lijst met = first class<br>prebuilt.datetimeV2=March 5|

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Als u meer dan het maximum aantal entiteiten 

U moet mogelijk hiërarchische en samengestelde entiteiten gebruiken. Hiërarchische entiteiten weer de relatie tussen entiteiten die dezelfde kenmerken, of lid zijn van een categorie. De onderliggende entiteiten zijn alle leden van de bovenliggende categorie. Bijvoorbeeld, een hiërarchische entiteit met de naam PlaneTicketClass mogelijk de onderliggende entiteiten EconomyClass en bewegingssensoren. De hiërarchie omvat slechts één niveau van diepte.  

Samengestelde entiteiten vertegenwoordigen onderdelen van een volledig. Een samengestelde entiteit met de naam PlaneTicketOrder kan bijvoorbeeld onderliggende entiteiten luchtvaartmaatschappij, doel, DepartureCity, vertrekdatum en PlaneTicketClass hebben. U maakt een samengestelde entiteit op basis van bestaande eenvoudige entiteiten, onderliggende items van hiërarchische entiteiten of vooraf gemaakte entiteiten.  

LUIS biedt ook het type van de lijst met entiteit die niet hebt geleerd van een machine maar kan uw LUIS-app om op te geven van een vaste lijst met waarden. Zie [LUIS grenzen](luis-boundaries.md) verwijzing naar de grenzen van het type van de entiteit lijst bekijken. 

Als u hebt besloten hiërarchische, composite, en lijst met entiteiten en moet het nog steeds meer dan de limiet, contact op met ondersteuning. Om dit te doen, verzamelt gedetailleerde informatie over uw systeem, gaat u naar de [LUIS](luis-reference-regions.md#luis-website) website, en selecteer vervolgens **ondersteuning**. Als uw Azure-abonnement voor ondersteuningsservices bevat, neem dan contact op met [technische ondersteuning van Azure](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Volgende stappen

Informatie over concepten over goede [uitingen](luis-concept-utterance.md). 

Zie [entiteiten toevoegen](luis-how-to-add-entities.md) voor meer informatie over entiteiten toevoegen aan uw LUIS-app.
