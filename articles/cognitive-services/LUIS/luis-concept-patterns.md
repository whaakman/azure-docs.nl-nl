---
title: Help voor voor spelling van patronen-LUIS
titleSuffix: Azure Cognitive Services
description: Een patroon kunt u meer nauwkeurigheid voor een doel zonder op te geven veel meer uitingen krijgen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: acbcaa7e5588c0fecf2c20751e69442e1373cbb5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563992"
---
# <a name="patterns-improve-prediction-accuracy"></a>Patronen verbeteren nauwkeurigheid
Patronen zijn ontworpen voor betere nauwkeurigheid wanneer verschillende uitingen vergelijkbaar zijn.  Een patroon kunt u meer nauwkeurigheid voor een doel zonder op te geven veel meer uitingen krijgen. 

## <a name="patterns-solve-low-intent-confidence"></a>Patronen oplossen lage intentie vertrouwen
Houd rekening met een Human Resources-app die in de organisatie-grafiek ten opzichte van een werknemer-rapporten. Gezien de naam en de relatie van een werknemer, retourneert LUIS de werknemers die betrokken zijn. Overweeg een werk nemer, Tom, met de naam van een manager en een team van ondergeschikten met de naam: Michael, Rebecca en Carl.

![Afbeelding van organigram](./media/luis-concept-patterns/org-chart.png)

|Utterances|Bedoeling voorspeld|Intentie score|
|--|--|--|
|Wie is de Tom ondergeschikte?|GetOrgChart|.30|
|Wie is de onderliggend niveau van Tom?|GetOrgChart|.30|

Als een app heeft tussen 10 en 20 uitingen met verschillende lengtes van zin, verschillende woordvolgorde en zelfs verschillende woorden (synoniemen van 'onderliggende', 'beheren', "rapport"), opleveren LUIS met een lage betrouwbaarheidsscore. Maak een patroon om LUIS inzicht te krijgen in het belang van de woord volgorde. 

Patronen oplossen van de volgende situaties: 

* De intentie Score is laag
* De juiste intentie is niet de hoogste score, maar te dicht bij de bovenste Score. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Patronen zijn geen garantie van doel
Patronen gebruiken een combinatie van technologieën voor voorspelling. Instellen van een doel voor de utterance van een sjabloon in een patroon is geen garantie van de intentie voorspelling maar het is een sterk signaal. 

<a name="patterns-do-not-improve-entity-detection"/></a>

## <a name="patterns-do-not-improve-machine-learned-entity-detection"></a>De door de machine gedetecteerde entiteits detectie worden niet door patronen verbeterd

Een patroon is hoofd zakelijk bedoeld om de voor spelling van intenties en rollen te helpen. Het patroon. elke entiteit wordt gebruikt om vrije-vorm entiteiten te extra heren. Hoewel patronen gebruikmaken van entiteiten, kan een patroon niet helpen bij het detecteren van een door de machine geleerde entiteit.  

Het is niet raadzaam om verbeterde entiteits voorspellingen te zien als u meerdere uitingen samenvouwt tot één patroon. Voor eenvoudige entiteiten die moeten worden gestart, moet u uitingen toevoegen of de lijst entiteiten gebruiken. anders wordt uw patroon niet geactiveerd.

## <a name="patterns-use-entity-roles"></a>Patronen entiteit rollen gebruiken
Als twee of meer entiteiten in een patroon contextueel zijn gerelateerd, patronen entiteit gebruiken [rollen](luis-concept-roles.md) contextuele gegevens over entiteiten extraheren.  

## <a name="prediction-scores-with-and-without-patterns"></a>Voorspelling scores met en zonder patronen
Onvoldoende uitingen voorbeeld gezien, zou LUIS kunnen worden vergroot het vertrouwen van voorspelling geen patronen weergegeven. De betrouwbaarheidsscore verhogen patronen zonder deze op te geven zoveel uitingen.  

## <a name="pattern-matching"></a>Jokertekens
Een patroon wordt op basis van de entiteiten in het patroon eerst detecteren en valideren van de rest van de woorden en de woordvolgorde van het patroon vergeleken. Entiteiten zijn vereist in het patroon voor een patroon voor de overeenkomst. Het patroon wordt toegepast op het niveau van token niet het teken-niveau. 

## <a name="pattern-syntax"></a>De syntaxis van het patroon
De syntaxis van het patroon is een sjabloon voor een utterance. De sjabloon moet bevatten woorden en entiteiten die u wilt vergelijken en woorden en leestekens die u wilt negeren. Het is **niet** een reguliere expressie. 

Entiteiten in de patronen zijn omgeven door accolades, `{}`. Patronen kunnen bestaan uit entiteiten en entiteiten met functies. [Patroon. any](luis-concept-entity-types.md#patternany-entity) is een entiteit die alleen in patronen wordt gebruikt. 

De syntaxis van het patroon ondersteunt de volgende syntaxis:

|Function|Syntaxis|Niveau nesten|Voorbeeld|
|--|--|--|--|
|entiteit| {}-accolades|2|Waar is formulier {entity-name}?|
|optioneel|[]-vier Kante haken<BR><BR>Er is een limiet van 3 op geneste niveaus van een combi natie van optioneel en groepering |2|Het vraag teken is optioneel [?]|
|shapes|()-haakjes|2|is (a \| b)|
|of| \|-verticale streep (pipe)<br><br>Er is een limiet van 2 op de verticale balken (of) in één groep |-|Waar is formulier ({Form-name-Short} &#x7c; {Form-name-Long} &#x7c; {Form-Number})| 
|begin en/of einde van utterance|^-caret|-|^ utterance starten<br>de utterance is voltooid.<br>^ strikte letterlijke overeenkomst van de volledige utterance met {Number} entiteit ^|

## <a name="nesting-syntax-in-patterns"></a>Syntaxis nesten in patronen

De **optionele** syntaxis met vier Kante haken kan twee niveaus genest zijn. Bijvoorbeeld: `[[this]is] a new form`. In dit voor beeld worden de volgende uitingen toegestaan: 

|Genest optioneel utterance-voor beeld|Uitleg|
|--|--|
|Dit is een nieuw formulier|komt overeen met alle woorden in het patroon|
|is een nieuw formulier|komt overeen met buitenste optionele woord en niet-optionele woorden in een patroon|
|een nieuw formulier|komt overeen met alleen de vereiste woorden|

De **groeperings** syntaxis met haakjes kan twee niveaus genest zijn. Bijvoorbeeld: `(({Entity1.RoleName1} | {Entity1.RoleName2} ) | {Entity2} )`. Met deze functie kunnen de drie entiteiten overeenkomen. 

Als Entity1 een locatie is met functies zoals origin (Seattle) en doel (Cairo) en entiteit 2 is een bekend gebouw naam van een lijst entiteit (RedWest-C), zou de volgende uitingen worden toegewezen aan dit patroon:

|Voor beeld van geneste groeperings utterance|Uitleg|
|--|--|
|RedWest-C|komt overeen met buitenste groeperings entiteit|
|Seattle|komt overeen met een van de binnenste groeperings entiteiten|
|Cairo|komt overeen met een van de binnenste groeperings entiteiten|

## <a name="nesting-limits-for-groups-with-optional-syntax"></a>Limieten voor groepen met optionele syntaxis nesten

Een combi natie van **groepering** met **optionele** syntaxis heeft een limiet van 3 geneste niveaus.

|Toegestaan|Voorbeeld|
|--|--|
|Ja|( [ ( test1 &#x7c; test2 ) ] &#x7c; test3 )|
|Nee|( [ ( [ test1 ] &#x7c; test2 ) ] &#x7c; test3 )|

## <a name="nesting-limits-for-groups-with-or-ing-syntax"></a>Limieten voor het nesten van groepen met de syntaxis

Een combi natie van **groepering** met **of-ING-** syntaxis heeft een limiet van 2 verticale balken.

|Toegestaan|Voorbeeld|
|--|--|
|Ja|( test1 &#x7c; test2 &#x7c; ( test3 &#x7c; test4 ) )|
|Nee|( test1 &#x7c; test2 &#x7c; test3 &#x7c; ( test4 &#x7c; test5 ) ) |

## <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Syntaxis voor een entiteit toevoegen aan een patroon-sjabloon
Als u wilt toevoegen een entiteit in de sjabloon voor het patroon, zoals de naam van de entiteit met behulp van accolades, rondom `Who does {Employee} manage?`. 

|Patroon met entiteit|
|--|
|`Who does {Employee} manage?`|

## <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Syntaxis voor het toevoegen van een entiteit en een rol aan een patroon-sjabloon
De entiteitsrol van een wordt aangeduid als `{entity:role}` met de naam van de entiteit gevolgd door een dubbele punt, gevolgd door de rolnaam. Als u wilt toevoegen een entiteit met een rol in de sjabloon voor het patroon, moet u de naam van de entiteit en de rolnaam met behulp van accolades, zoals `Book a ticket from {Location:Origin} to {Location:Destination}`. 

|Patroon met behulp van entiteit|
|--|
|`Book a ticket from {Location:Origin} to {Location:Destination}`|

## <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Syntaxis voor het toevoegen van een pattern.any aan patroon sjabloon
De entiteit Pattern.any kunt u een entiteit van de verschillende lengten toevoegen aan het patroon. Als de sjabloon patroon wordt gevolgd, mag de pattern.any een willekeurige lengte. 

Om toe te voegen een **Pattern.any** entiteit in de sjabloon patroon rondom de Pattern.any-entiteit met de accolades, zoals `How much does {Booktitle} cost and what format is it available in?`.  

|Patroon met Pattern.any entiteit|
|--|
|`How much does {Booktitle} cost and what format is it available in?`|

|Titels in het patroon|
|--|
|Wat kost **stelen dit boek** kosten, en welke indeling is beschikbaar in?|
|Wat kost **vragen** kosten, en welke indeling is beschikbaar in?|
|Wat kost **de benieuwd Incident van de hond in de nacht-tijd** kosten, en welke indeling is beschikbaar in?| 

De woorden van de boek titel zijn niet verwarrend voor LUIS, omdat LUIS weet waar de titel van het boek eindigt, op basis van het patroon. elke entiteit.

## <a name="explicit-lists"></a>Expliciete lijsten

Maak een [expliciete lijst](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8) via de ontwerp-API om de uitzonde ring toe te staan wanneer:

* Uw patroon bevat een [patroon.](luis-concept-entity-types.md#patternany-entity)
* En met deze patroon syntaxis kan een onjuiste entiteits extractie worden uitgevoerd op basis van de utterance. 

Stel bijvoorbeeld dat u hebt een patroon met beide optioneel syntaxis `[]`, en de syntaxis van de entiteit, `{}`, gecombineerde op een manier om gegevens te extraheren onjuist.

Houd rekening met het patroon [zoeken] e-mail over {subject} [van {persoon}].

In de volgende uitingen de **onderwerp** en **persoon** entiteit correct en niet correct worden opgehaald:

|Utterance|Entiteit|Juiste extractie|
|--|--|:--:|
|e-mail over honden van Chris|onderwerp = honden<br>persoon Chris =|✔|
|per e-mail over de man in de La Mancha|onderwerp = de man<br>persoon La Mancha =|X|

In de voor gaande tabel moet het onderwerp ( `the man from La Mancha` de titel van een boek) zijn, maar omdat het onderwerp het `from`optionele woord bevat, is de titel onjuist voor speld. 

U kunt met het oplossen van deze uitzondering op het patroon, toevoegen `the man from la mancha` als een lijst met expliciete-overeenkomst voor de {subject} entiteit met de [API ontwerpen voor expliciete lijst](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8).

## <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>Syntaxis voor het markeren van optionele tekst in een sjabloon utterance
Optionele tekst in de utterance met behulp van de syntaxis van reguliere expressie vierkant haakje sluiten, markeert `[]`. De optionele tekst kunt vierkante haken maximaal twee vierkante haken nesten.

|Patroon met optionele tekst|Betekenis|
|--|--|
|`[find] email about {subject} [from {person}]`|`find`en `from {person}` zijn optioneel|
|' Kan ik me helpen [?]|Het lees teken is optioneel|

Lees tekens (`?`, `!`, `.`) moeten worden genegeerd en u moet ze negeren met de syntaxis van het vier Kante haakje in patronen. 

## <a name="pattern-only-apps"></a>Alleen patroon-apps
U kunt een app bouwen met intenties die geen voor beeld uitingen hebben, zolang er voor elke intentie een patroon is. Het patroon mag voor een alleen-patroon app geen door machines geleerde entiteiten bevatten, omdat hiervoor bijvoorbeeld uitingen nodig zijn. 

## <a name="best-practices"></a>Aanbevolen procedures
Informatie over [aanbevolen procedures](luis-concept-best-practices.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het implementeren van patronen in deze zelfstudie](luis-tutorial-pattern.md)
