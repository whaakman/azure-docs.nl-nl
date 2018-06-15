---
title: Meer informatie over hoe patronen nauwkeurigheid vergroten | Microsoft Docs
titleSuffix: Azure
description: Informatie over het ontwerpen van patronen te verhogen opzet voorspelling scores en entiteiten te vinden.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr
ms.openlocfilehash: 7ccc8353c91c354c745d52f9fc9663131f2b1c4e
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35350089"
---
# <a name="patterns-improve-prediction-accuracy"></a>Patronen verbeteren nauwkeurigheid
Patronen zijn ontworpen voor betere nauwkeurigheid wanneer verschillende utterances vergelijkbaar zijn. Doordat het patroon voor de utterance zijn LUIS een hoge betrouwbaarheid in de voorspelling. 

## <a name="patterns-solve-low-intent-confidence"></a>Patronen oplossen lage opzet vertrouwen
U kunt een Human Resources-app die over de organisatie-grafiek ten opzichte van een werknemer rapporten. Gezien de naam van een werknemer en de relatie, retourneert LUIS de werknemers die zijn betrokken. Houd rekening met een werknemer, aangepaste, met een manager naam Els en een team van onderliggende niveaus met de naam: Michael Rebecca en Carl.

![Afbeelding van organigram](./media/luis-concept-patterns/org-chart.png)

|Utterances|Bedoeling voorspeld|Opzet score|
|--|--|--|
|Wie is de aangepaste onderliggend niveau?|GetOrgChart|.30|
|Wie is de onderliggend niveau van aangepaste?|GetOrgChart|.30|

Als een app tussen 10 en 20 utterances met verschillende lengten van zin, andere word volgorde en zelfs verschillende woorden (synoniemen van 'onderliggende', 'beheren', 'rapport'), kan LUIS een lage vertrouwen score retourneren. Om u te helpen begrijpen het belang van de volgorde word LUIS, een patroon te maken. 

Patronen oplossen van de volgende situaties: 

* Wanneer de opzet score is laag
* Wanneer de juiste bedoeling is niet de bovenste score maar te dicht bij de bovenste score. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Er zijn geen patronen een garantie van het doel van
Patronen een mengeling van voorspelling technologieën gebruiken. Instellen van een doel voor de utterance van een sjabloon in een patroon is geen garantie van de opzet voorspelling maar het is een sterk signaal. 

## <a name="patterns-do-not-improve-entity-detection"></a>Patronen verbetert niet entiteit detectie
Terwijl patronen entiteiten vereisen, helpt een patroon niet detecteren van de entiteit. Een patroon is alleen bedoeld om u te helpen de voorspelling met intents en rollen.  

## <a name="prediction-scores-with-and-without-patterns"></a>Voorspelling scores met en zonder patronen
Onvoldoende utterances voorbeeld gezien, zou LUIS kunnen meer vertrouwen voorspelling zonder patronen. De score vertrouwen verhogen patronen zonder zoveel utterances bieden.  

## <a name="pattern-matching"></a>Jokertekens
Een patroon wordt vergeleken op basis van de entiteiten in het patroon eerst detecteren en vervolgens de rest van de woorden en word volgorde van het patroon wordt gevalideerd. Entiteiten zijn vereist in het patroon voor een patroon moet worden gezocht. 

## <a name="pattern-syntax"></a>De syntaxis van het patroon
Entiteiten in patronen worden omgeven door accolades. Patronen kunnen bevatten entiteiten en entiteiten met rollen. Pattern.any is een entiteit die alleen wordt gebruikt in patronen. De syntaxis voor elk van deze wordt in de volgende secties uitgelegd.

### <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Syntaxis voor een entiteit toevoegen aan een sjabloon patroon
Als u wilt een entiteit in de sjabloon voor het patroon toevoegen, zoals de entiteitsnaam van de met accolades, rond `Who does {Employee} manage?`. 

```
Who does {Employee} manage?
```

### <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Syntaxis van een entiteit en de rol toevoegen aan een sjabloon patroon
Een entiteitsrol voor een wordt aangeduid als `{entity:role}` met de entiteitsnaam van de gevolgd door een dubbelepunt en vervolgens de naam van de rol. Als u wilt een entity met een rol in de sjabloon voor het patroon toevoegen, zoals rond de naam van de entiteit en de naam van de rol met de accolades, `Book a ticket from {Location:Origin} to {Location:Destination}`. 

```
Book a ticket from {Location:Origin} to {Location:Destination}
```

### <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Syntaxis voor het toevoegen van een pattern.any aan patroon sjabloon
De entiteit Pattern.any kunt u een entiteit met verschillende lengten toevoegen aan het patroon. Als de sjabloon patroon wordt gevolgd, mag de pattern.any een willekeurige lengte. 

Om toe te voegen een **Pattern.any** entiteit in de sjabloon patroon rond de entiteit Pattern.any met de accolades, zoals `How much does {Booktitle} cost and what format is it available in?`.  

```
How much does {Booktitle} cost and what format is it available in?
```

|Titels in het patroon|
|--|
|Wat kost **stelen dit boek** kosten en welke indeling is beschikbaar in?|
|Wat kost **vraag** kosten en welke indeling is beschikbaar in?|
|Wat kost **de weten Incident van het aquaduct in de nacht-tijd** kosten en welke indeling is beschikbaar in?| 

De contextuele woorden van de titel adresboek zijn in deze voorbeelden van de titel adresboek niet verwarring opleveren voor LUIS. LUIS weet waar de titel van boek omdat deze in een patroon is en gemarkeerd met een entiteit Pattern.any eindigt.

### <a name="explicit-lists"></a>Expliciete lijsten
Als het patroon bevat een Pattern.any en de syntaxis van het patroon kunt u de mogelijkheid van een onjuist entiteit extractie op basis van de utterance, maakt u een [expliciete lijst](https://aka.ms/ExplicitList) via de API-uitzondering toestaan voor ontwerpen. 

Stel bijvoorbeeld dat u hebt een patroon met beide optionele syntaxis `[]`, en de syntaxis van de entiteit `{}`gecombineerde op een manier om gegevens te extraheren onjuist.

Houd rekening met het patroon [zoeken] e over {onderwerp} [from {persoon}]. In de volgende utterances de **onderwerp** en **persoon** entiteit zijn uitgepakt correct en onjuist:

|Utterance|Entiteit|Juiste uitpakken|
|--|--|:--:|
|e-mail over honden van Chris|onderwerp honden =<br>persoon Chris =|✔|
|Stuur een e-mail over de man van La-Mancha|onderwerp het man =<br>persoon La Mancha =|X|

In de voorgaande tabel, de utterance `email about the man from La Mancha`, het onderwerp moet `the man from La Mancha` (titel van een boek), maar omdat het onderwerp de optionele word bevat `from`, de titel onjuist wordt voorspeld. 

U kunt deze uitzondering op het patroon oplossen door toevoegen `the man from la mancha` als een expliciete lijst overeenkomst voor de {onderwerp} entiteit met de [API ontwerpen voor expliciete lijst](https://aka.ms/ExplicitList).

### <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>Syntaxis van de optionele tekst in een sjabloon utterance gemarkeerd
Optionele tekst in de utterance met de reguliere expressie vierkant haakje-syntaxis markeert `[]`. De optionele tekst kunt vierkante haken maximaal twee vierkante haken nesten.

```
[find] email about {subject} [from {person}]
```

Leestekens zoals `.`, `!`, en `?` kunnen worden genegeerd met de vierkante haken. Als u wilt dat deze markeringen, moet elke is ingeschakeld in een afzonderlijke patroon. De syntaxis van de optionele ondersteunt momenteel geen een item in een lijst van meerdere items worden genegeerd.

## <a name="patterns-only"></a>Alleen patronen
Een app zonder eventuele utterances voorbeeld kan LUIS in opzet. Dit is alleen toegestaan als patronen die zijn gebruikt. Patronen vereist ten minste één entiteit in elke patroon. Voor een patroon alleen-lezen-app mogen het patroon geen machine geleerde entiteiten omdat deze voorbeeld utterances vereisen. 

## <a name="best-practices"></a>Aanbevolen procedures
Meer informatie over [aanbevolen procedures](luis-concept-best-practices.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het implementeren van patronen in deze zelfstudie](luis-tutorial-pattern.md)

[LUIS]: luis-reference-regions.md
