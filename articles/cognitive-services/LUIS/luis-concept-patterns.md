---
title: Patronen te voorspellen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Een patroon kunt u meer nauwkeurigheid voor een doel zonder op te geven veel meer uitingen krijgen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: diberry
ms.openlocfilehash: af73d78b911d59f68c5e824931b5634d8c69f309
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57448999"
---
# <a name="patterns-improve-prediction-accuracy"></a>Patronen verbeteren nauwkeurigheid
Patronen zijn ontworpen voor betere nauwkeurigheid wanneer verschillende uitingen vergelijkbaar zijn.  Een patroon kunt u meer nauwkeurigheid voor een doel zonder op te geven veel meer uitingen krijgen. 

## <a name="patterns-solve-low-intent-confidence"></a>Patronen oplossen lage intentie vertrouwen
Houd rekening met een Human Resources-app die in de organisatie-grafiek ten opzichte van een werknemer-rapporten. Gezien de naam en de relatie van een werknemer, retourneert LUIS de werknemers die betrokken zijn. Houd rekening met een werknemer, Tom, met een manager naam Els en een team van onderliggende niveaus met de naam: Michael, Rebecca en Carl.

![Afbeelding van organigram](./media/luis-concept-patterns/org-chart.png)

|Utterances|Bedoeling voorspeld|Intentie score|
|--|--|--|
|Wie is de Tom ondergeschikte?|GetOrgChart|.30|
|Wie is de onderliggend niveau van Tom?|GetOrgChart|.30|

Als een app heeft tussen 10 en 20 uitingen met verschillende lengtes van zin, verschillende woordvolgorde en zelfs verschillende woorden (synoniemen van 'onderliggende', 'beheren', "rapport"), opleveren LUIS met een lage betrouwbaarheidsscore. Een patroon om te helpen begrijpen het belang van de woordvolgorde LUIS maken. 

Patronen oplossen van de volgende situaties: 

* De intentie score is laag
* Het juiste doel is niet de hoogste score maar te dicht bij de hoogste score. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Patronen zijn geen garantie van doel
Patronen gebruiken een combinatie van technologieën voor voorspelling. Instellen van een doel voor de utterance van een sjabloon in een patroon is geen garantie van de intentie voorspelling maar het is een sterk signaal. 

## <a name="patterns-do-not-improve-entity-detection"></a>Patronen de detectie van de entiteit niet zijn verbeteren
Hoewel patronen entiteiten, een patroon helpt niet bij het detecteren van de entiteit. Een patroon is alleen bedoeld om u te helpen bij het voorspellen met intents en rollen.  

Niet verwacht te zien verbeterde entiteit voorspelling als u meerdere uitingen in een enkel patroon samenvouwen. Voor eenvoudige entiteiten moet worden gestart, moet u utterances toevoegen of lijst met entiteiten gebruiken anders die het patroon wordt niet gestart.

## <a name="patterns-use-entity-roles"></a>Patronen entiteit rollen gebruiken
Als twee of meer entiteiten in een patroon contextueel zijn gerelateerd, patronen entiteit gebruiken [rollen](luis-concept-roles.md) contextuele gegevens over entiteiten extraheren. Dit is gelijk aan het hiërarchische entiteit kinderen, maar is **alleen** beschikbaar in de patronen. 

## <a name="prediction-scores-with-and-without-patterns"></a>Voorspelling scores met en zonder patronen
Onvoldoende uitingen voorbeeld gezien, zou LUIS kunnen worden vergroot het vertrouwen van voorspelling geen patronen weergegeven. De betrouwbaarheidsscore verhogen patronen zonder deze op te geven zoveel uitingen.  

## <a name="pattern-matching"></a>Jokertekens
Een patroon wordt op basis van de entiteiten in het patroon eerst detecteren en valideren van de rest van de woorden en de woordvolgorde van het patroon vergeleken. Entiteiten zijn vereist in het patroon voor een patroon voor de overeenkomst. Het patroon wordt toegepast op het niveau van token niet het teken-niveau. 

## <a name="pattern-syntax"></a>De syntaxis van het patroon
De syntaxis van het patroon is een sjabloon voor een utterance. De sjabloon moet bevatten woorden en entiteiten die u wilt vergelijken en woorden en leestekens die u wilt negeren. Het is **niet** een reguliere expressie. 

Entiteiten in de patronen zijn omgeven door accolades, `{}`. Patronen kunnen bestaan uit entiteiten en entiteiten met functies. [Pattern.ANY](luis-concept-entity-types.md#patternany-entity) is een entiteit die alleen wordt gebruikt in patronen. 

Patroon-syntaxis ondersteunt de volgende syntaxis:

|Function|Syntaxis|Genest niveau|Voorbeeld|
|--|--|--|--|
|entiteit| {} -accolades|2|Waar bevindt zich formulier {entiteitnaam}?|
|optioneel|[] - vierkante haken<BR><BR>Er is een limiet van 3 voor geneste niveaus van een combinatie van optioneel en groeperen |2|Het vraagteken is optioneel [?]|
|Groeperen|() - haakjes|2|is (a \| b)|
|of| \| -verticale streep (pipe)<br><br>Er is een limiet van 2 op de verticale balken (of) in één groep |-|Waar is vorm ({naam-formulier-korte} &#x7c; {naam-formulier-jaren} &#x7c; {formulier-number})| 
|begin-en/of einde van utterance|^-caret-teken|-|^ begin van de utterance<br>de utterance gebeurt ^<br>^ strikte letterlijke overeenkomen met de volledige utterance met de entiteit {number} ^|

## <a name="nesting-syntax-in-patterns"></a>De syntaxis nesten van patronen

De **optionele** syntaxis, tussen vierkante haken staan, kan geneste twee niveaus zijn. Bijvoorbeeld: `[[this]is] a new form`. In dit voorbeeld kan de volgende uitingen: 

|Voorbeeld van de geneste optioneel utterance|Uitleg|
|--|--|
|Dit is een nieuw formulier|komt overeen met alle woorden in het patroon|
|is een nieuw formulier|komt overeen met de buitenste optioneel word en niet-optionele woorden in het patroon|
|een nieuw formulier|komt overeen met de vereiste alleen woorden|

De **groeperen** syntaxis, met behulp van haakjes, kan de geneste twee niveaus zijn. Bijvoorbeeld: `(({Entity1.RoleName1} | {Entity1.RoleName2} ) | {Entity2} )`. Hiermee wordt een van de drie entiteiten worden vergeleken. 

Als Entity1 een locatie met functies zoals de oorsprong (Amsterdam) en de bestemming (Cairo is) en 2 van de entiteit de naam van een bekende het bouwen van een lijst met entiteit (RedWest-C is), worden de volgende uitingen wordt toegewezen aan dit patroon:

|Voorbeeld van de geneste groepering utterance|Uitleg|
|--|--|
|RedWest-C|komt overeen met de buitenste groep entiteit|
|Seattle|komt overeen met een van de binnenste groep-entiteiten|
|Cairo|komt overeen met een van de binnenste groep-entiteiten|

## <a name="nesting-limits-for-groups-with-optional-syntax"></a>Limieten voor groepen met optionele syntaxis nesten

Een combinatie van **groeperen** met **optionele** syntaxis heeft een limiet van 3 geneste niveaus.

|Toegestaan|Voorbeeld|
|--|--|
|Ja|( [ ( test1 &#x7c; test2 ) ] &#x7c; test3 )|
|Nee|( [ ( [ test1 ] &#x7c; test2 ) ] &#x7c; test3 )|

## <a name="nesting-limits-for-groups-with-or-ing-syntax"></a>Nesten limieten voor groepen met de syntaxis van of doorsturen

Een combinatie van **groeperen** met **of doorsturen** syntaxis heeft een limiet van 2 verticale balken.

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

De woorden van titel van het boek zijn niet lastig te LUIS omdat LUIS waar de titel van het boek eindigt weet, op basis van de entiteit Pattern.any.

## <a name="explicit-lists"></a>Expliciete lijsten

Maak een [expliciete lijst](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8) via de API-uitzondering toestaan voor ontwerpen wanneer:

* Het patroon bevat een [Pattern.any](luis-concept-entity-types.md#patternany-entity)
* En dat de syntaxis van het patroon voor de mogelijkheid van de extractie van een onjuist entiteit op basis van de utterance toestaat. 

Stel bijvoorbeeld dat u hebt een patroon met beide optioneel syntaxis `[]`, en de syntaxis van de entiteit, `{}`, gecombineerde op een manier om gegevens te extraheren onjuist.

Houd rekening met het patroon [zoeken] e-mail over {subject} [van {persoon}].

In de volgende uitingen de **onderwerp** en **persoon** entiteit correct en niet correct worden opgehaald:

|Utterance|Entiteit|Juiste extractie|
|--|--|:--:|
|e-mail over honden van Chris|onderwerp = honden<br>persoon Chris =|✔|
|per e-mail over de man in de La Mancha|onderwerp = de man<br>persoon La Mancha =|X|

In de voorgaande tabel, het onderwerp moet `the man from La Mancha` (titel van een boek), maar omdat het onderwerp de optionele word bevat `from`, de titel onjuist wordt voorspeld. 

U kunt met het oplossen van deze uitzondering op het patroon, toevoegen `the man from la mancha` als een lijst met expliciete-overeenkomst voor de {subject} entiteit met de [API ontwerpen voor expliciete lijst](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8).

## <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>Syntaxis voor het markeren van optionele tekst in een sjabloon utterance
Optionele tekst in de utterance met behulp van de syntaxis van reguliere expressie vierkant haakje sluiten, markeert `[]`. De optionele tekst kunt vierkante haken maximaal twee vierkante haken nesten.

|Patroon met optionele tekst|Betekenis|
|--|--|
|`[find] email about {subject} [from {person}]`|`find` en `from {person}` zijn optioneel|
|' U me kan helpen [?]|De markering interpunctie is optioneel|

Leestekens (`?`, `!`, `.`) moeten worden genegeerd en u moet deze met behulp van de syntaxis van de vierkant haakje in patronen genegeerd. 

## <a name="pattern-only-apps"></a>Apps met alleen-patroon
U kunt een toepassing bouwen met intents waarvoor geen uitingen voorbeeld, zolang er een patroon voor elk doel is. Voor een app alleen-patroon mag niet het patroon omdat deze voorbeeld-uitingen hoeven machine geleerde entiteiten bevatten. 

## <a name="best-practices"></a>Aanbevolen procedures
Informatie over [aanbevolen procedures](luis-concept-best-practices.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het implementeren van patronen in deze zelfstudie](luis-tutorial-pattern.md)
