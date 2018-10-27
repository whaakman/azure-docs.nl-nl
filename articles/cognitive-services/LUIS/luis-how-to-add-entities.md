---
title: Entiteiten in LUIS-apps toevoegen
titleSuffix: Azure Cognitive Services
description: Entiteiten (belangrijke gegevens in het domein van uw toepassing) in Language Understanding (LUIS)-apps toevoegen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 10/24/2018
ms.author: diberry
ms.openlocfilehash: 178f4314f9ede86444ee60fd2a64f85dc283080b
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138525"
---
# <a name="create-entities-without-utterances"></a>Entiteiten zonder uitingen maken

De entiteit vertegenwoordigt een woord of zinsdeel in de utterance die u wilt dat opgehaald. Een utterance kan bevatten veel entiteiten of geen helemaal. Een entiteit vertegenwoordigt een klasse met inbegrip van een verzameling van soortgelijke objecten (plaatsen, dingen, personen, gebeurtenissen en concepten). Entiteiten informatie die relevant zijn voor de bedoeling beschrijven en soms ze zijn essentieel voor uw app de taak uit te voeren. 

U kunt entiteiten maken vóór of naast het maken van intents.

U kunt toevoegen, bewerken of verwijderen van entiteiten in uw LUIS-app via de **entiteitenlijst** op de **entiteiten** pagina. LUIS biedt twee typen entiteiten: [vooraf gemaakte entiteiten](luis-reference-prebuilt-entities.md), en uw eigen [aangepaste entiteiten](luis-concept-entity-types.md#types-of-entities).

Zodra een entiteit is gemaakt, kan deze worden gelabeld in utterance van voorbeeld van een doel van de **bedoeling** pagina met details. 

## <a name="add-prebuilt-entity"></a>Vooraf gedefinieerde entiteit toevoegen

Algemene vooraf gemaakte entiteiten die zijn toegevoegd aan een toepassing worden *getal* en *datetimeV2*. 

1. In uw app uit de **bouwen** sectie, selecteer **entiteiten** in het linkerpaneel.
 
1. Op de **entiteiten** weergeeft, schakelt **vooraf gemaakte entiteiten toevoegen**.

1. In **vooraf gemaakte entiteiten toevoegen** in het dialoogvenster, selecteer de **getal** en **datetimeV2** vooraf gemaakte entiteiten. Selecteer vervolgens **Done**.

    ![Dialoogvenster voor vooraf gedefinieerde entiteit schermopname toevoegen](./media/add-entities/list-of-prebuilt-entities.png)

## <a name="add-simple-entities"></a>Eenvoudige entiteiten toevoegen

Een enkele entiteit beschrijft een enkele concept. De volgende procedure gebruiken om te maken van een entiteit die namen van de afdeling bedrijven zoals extraheert *Human resources* of *Operations*.   

1. Selecteer in uw app, de **bouwen** sectie en selecteer vervolgens **entiteiten** in het linkerpaneel, schakelt u vervolgens **nieuwe entiteit maken**.

1. Typ in het pop-updialoogvenster `Location` in de **entiteitnaam** Schakel **eenvoudige** uit de **entiteitstype** lijst en selecteer vervolgens **gedaan**.

    Zodra deze entiteit is gemaakt, gaat u naar alle intents waarvoor de voorbeeld-uitingen die de entiteit bevatten. Selecteer de tekst in het voorbeeld utterance en de tekst markeren als de entiteit. 

    Een [woordgroepenlijst](luis-concept-feature.md) wordt meestal gebruikt om het signaal van een enkele entiteit te verbeteren.

## <a name="add-regular-expression-entities"></a>Reguliere expressie entiteiten toevoegen

Een entiteit reguliere expressie wordt gebruikt voor het ophalen van gegevens uit de utterance op basis van een reguliere expressie die u opgeeft. 

1. Selecteer in uw app **entiteiten** uit de navigatiebalk aan de linkerkant en selecteer vervolgens **nieuwe entiteit maken**.

1. Voer in het pop-updialoogvenster `Human resources form name` in de **entiteitnaam** Schakel **reguliere expressie** uit de **entiteitstype** , voert u de reguliere expressie `hrf-[0-9]{6}`, en selecteer vervolgens **gedaan**. 

    Deze reguliere expressie letterlijke tekens `hrf-`, klikt u vervolgens 6 cijfers om weer te geven van een formulier nummer voor een formulier Human resources.

## <a name="add-hierarchical-entities"></a>Hiërarchische entiteiten toevoegen

Een hiërarchische entiteit is een soort contextueel geleerde en gerelateerde entiteiten. In het volgende voorbeeld bevat de entiteit bron en doel-locaties. 

In de utterance `Move John Smith from Seattle to Cairo`, Seattle, is de verzendingslocatie en Cairo is de doellocatie. Elke locatie is contextueel verschillende en geleerde van woordvolgorde en word keuze in de utterance.

Hiërarchische entiteiten toevoegen, voert u de volgende stappen uit: 

1. Selecteer in uw app **entiteiten** uit de navigatiebalk aan de linkerkant en selecteer vervolgens **nieuwe entiteit maken**.

1. Typ in het pop-updialoogvenster `Location` in de **entiteitnaam** vak en selecteer vervolgens **hiërarchisch** uit de **entiteitstype** lijst.

    ![Een hiërarchische entiteit toevoegen](./media/add-entities/hier-location-entity-creation.png)

1. Selecteer **onderliggende toevoegen**, en voer vervolgens `Origin` in **onderliggende #1** vak. 

1. Selecteer **onderliggende toevoegen**, en voer vervolgens `Destination` in **onderliggende 2** vak. Selecteer **Done**.

    >[!CAUTION]
    >Namen van de onderliggende entiteiten moeten uniek zijn in alle entiteiten in een enkele app. Twee entiteiten met verschillende hiërarchische mag geen onderliggende entiteiten met dezelfde naam bevatten. 

    Zodra deze entiteit is gemaakt, gaat u naar alle intents waarvoor de voorbeeld-uitingen die de entiteit bevatten. Selecteer de tekst in het voorbeeld utterance en de tekst markeren als de entiteit. 

## <a name="add-composite-entities"></a>Samengestelde entiteiten toevoegen

U kunt de relaties tussen entiteiten van verschillende typen door het maken van een samengestelde entiteit definiëren. In het volgende voorbeeld wordt bevat de entiteit een reguliere expressie en een vooraf gedefinieerde entiteit met naam.  

In de utterance `Send hrf-123456 to John Smith`, de tekst `hrf-123456` wordt vergeleken met een HR [reguliere expressie](#add-regular-expression-entities) en `John Smith` wordt opgehaald met de vooraf gedefinieerde entiteit personName. Elke entiteit maakt deel uit van een grotere, bovenliggende entiteit. 

1. Selecteer in uw app **entiteiten** in de linkernavigatiebalk van de **bouwen** uit en selecteer vervolgens **vooraf gedefinieerde entiteit toevoegen**.

1. De vooraf gedefinieerde entiteit toevoegen **PersonName**. Zie voor instructies [vooraf gemaakte entiteiten toevoegen](#add-prebuilt-entity). 

1. Selecteer **entiteiten** uit de navigatiebalk aan de linkerkant en selecteer vervolgens **nieuwe entiteit maken**.

1. Voer in het pop-updialoogvenster `SendHrForm` in de **entiteitnaam** vak en selecteer vervolgens **samengestelde** uit de **entiteitstype** lijst.

1. Selecteer **onderliggende toevoegen** om toe te voegen een nieuw onderliggend element.

1. In **onderliggende #1**, selecteer de entiteit **getal** in de lijst.

1. In **onderliggende 2**, selecteer de entiteit **Human resources formuliernaam** in de lijst. 

1. Selecteer **Done**.

## <a name="add-patternany-entities"></a>Pattern.any entiteiten toevoegen

[Pattern.ANY](luis-concept-entity-types.md) entiteiten zijn alleen geldig in [patronen](luis-how-to-model-intent-pattern.md), niet intents. Dit type entiteit kunt LUIS vinden het einde van de entiteiten met verschillende lengte en word keuze. Omdat deze entiteit wordt gebruikt in een patroon, LUIS u weet waar het einde van de entiteit zich bevindt in de sjabloon utterance.

Als een app heeft een `FindHumanResourcesForm` intentie, de titel van de uitgepakte formulier kan leiden tot problemen met de intentie voorspelling. Gebruiken om te verduidelijken welke woorden, zijn in de titel van het formulier, een Pattern.any binnen een patroon. De voorspelling LUIS begint met de utterance. De utterance wordt eerst gecontroleerd en afgestemd voor entiteiten, wanneer de entiteiten worden gevonden, wordt het patroon wordt gecontroleerd en afgestemd. 

In de utterance `Where is Request relocation from employee new to the company on the server?`, de titel van het formulier is erg lastig omdat het geen context voor de hand liggende waar de titel wordt beëindigd en waar de rest van de utterance begint. Titels zijn een willekeurige volgorde van woorden met inbegrip van één woord, complexe zinnen met interpunctie en wachtwoord ordening van woorden. Een patroon kunt u een entiteit maken waar de volledige en exacte entiteit kan worden geëxtraheerd. Als de titel wordt gevonden, de `FindHumanResourcesForm` bedoeling omdat dat het doel van het patroon wordt voorspeld.

1. Uit de **bouwen** sectie, selecteer **entiteiten** in het linkerdeelvenster en selecteer vervolgens **nieuwe entiteit maken**.

1. In de **entiteit toevoegen** in het dialoogvenster, Geef in het naamvak entiteit en selecteer **Pattern.any** als de **entiteitstype**.

    Een patroon voor het gebruik van de entiteit pattern.any toevoegt op het **patronen** pagina, in de **verbeterde app-prestaties** sectie, met de syntaxis van de juiste accolade toe, zoals `Where is **{HumanResourcesFormTitle}** on the server?`.

    Als u merkt dat het patroon, wanneer dit een Pattern.any bevat, entiteiten onjuist extraheert, gebruikt u een [expliciete lijst](luis-concept-patterns.md#explicit-lists) om dit probleem te verhelpen. 

## <a name="add-a-role-to-pattern-based-entity"></a>Rol toevoegen aan de entiteit op basis van het patroon

Een rol is een benoemde subtype van een entiteit op basis van context. Het is vergelijkbaar met een [hiërarchische](#add-hierarchical-entities) entiteit, maar functies worden alleen gebruikt [patronen](luis-how-to-model-intent-pattern.md). 

Met behulp van hetzelfde voorbeeld als de hiërarchische entiteit van oorsprong en bestemming steden, het verschil is dat een rol met de naam origin in plaats van een hiërarchische onderliggend element. 

De syntaxis voor een rol is **{Entityname:Rolename}** waar de naam van de entiteit wordt gevolgd door een dubbele punt, gevolgd door de rolnaam. Bijvoorbeeld `Move {personName} from {LocationUsingRoles:Origin} to {LocationUsingRoles:Destination}`.

1. Uit de **bouwen** sectie, selecteer **entiteiten** in het linkerpaneel.

1. Selecteer **Create new intent**. Voer de naam van `LocationUsingRoles`. Selecteer het type **eenvoudige** en selecteer **gedaan**. 

1. Selecteer **entiteiten** in het linkerpaneel, selecteer vervolgens de nieuwe entiteit **LocationUsingRoles** in de vorige stap hebt gemaakt.

1. In de **rolnaam** tekstvak, voer de naam van de rol `Origin` en invoeren. Voeg de naam van een tweede functie van `Destination`. 

    ![Schermafbeelding van de oorsprong rol toe te voegen aan de entiteit locatie](./media/add-entities/roles-enter-role-name-text.png)

## <a name="add-list-entities"></a>Lijst met entiteiten toevoegen

Lijst met entiteiten vertegenwoordigen een vaste en gesloten set verwante woorden. 

U kunt een lijst met alle afdelingen, samen met eventuele synoniemen voor de afdelingen hebben voor een Human Resources-app. U hoeft niet te weten van alle waarden bij het maken van de entiteit. U kunt meer toevoegen nadat u hebt bekeken real-user-uitingen door synoniemen.

1. Uit de **bouwen** sectie, selecteer **entiteiten** in het linkerdeelvenster en selecteer vervolgens **nieuwe entiteit maken**.

1. In de **entiteit toevoegen** in het dialoogvenster, type `Department` in de **entiteitnaam** vak en selecteer **lijst** als de **entiteitstype**. Selecteer **Done**.
  
1. De entiteit-pagina van de lijst kunt u genormaliseerde namen toevoegen. In de **waarden** tekstvak, voer de naam van een afdeling voor de lijst, zoals `HumanResources` druk op Enter op het toetsenbord. 

1. Geef aan de rechterkant van de genormaliseerde waarde, synoniemen, drukt u op Enter na elk item op.

1. Als u meer genormaliseerde items voor de lijst wilt, selecteert u **raden** om te zien van de opties uit de [semantische woordenlijst](luis-glossary.md#semantic-dictionary).

    ![Schermafbeelding van de oorsprong rol toe te voegen aan de entiteit locatie](./media/add-entities/hr-list-2.png)


1. Selecteer een item in de aanbevolen lijst toe te voegen als een genormaliseerde waarde of selecteert **alle** alle items kunnen worden toegevoegd. 
    U kunt waarden kunt importeren in een bestaande lijst entiteit met behulp van de volgende JSON-indeling:

    ```
    [
        {
            "canonicalForm": "Blue",
            "list": [
                "navy",
                "royal",
                "baby"
            ]
        },
        {
            "canonicalForm": "Green",
            "list": [
                "kelly",
                "forest",
                "avacado"
            ]
        }
    ]  
    ```


## <a name="change-entity-type"></a>Entiteitstype wijzigen

LUIS staat niet toe dat u het type van de entiteit niet wijzigen omdat het niet weet wat u moet toevoegen of verwijderen om te bouwen die entiteit. Als u wilt wijzigen, is het beter om een nieuwe entiteit van het juiste type maken met een iets andere naam. Nadat de entiteit is gemaakt, in elke utterance, verwijdert u de oude naam van de gelabelde entiteit en de naam van de nieuwe entiteit toevoegen. Zodra alle uitingen hebben is relabeled, de oude entiteit niet verwijderen. 

## <a name="create-a-pattern-from-an-utterance"></a>Maken van een patroon van een utterance

Zie [toevoegen patroon uit bestaande utterance op de pagina kunt u lezen wat of entiteit](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="train-your-app-after-changing-model-with-entities"></a>Uw app trainen na het wijzigen van het model met entiteiten

Na het toevoegen, bewerken of verwijderen van entiteiten, [trainen](luis-how-to-train.md) en [publiceren](luis-how-to-publish-app.md) uw app voor uw wijzigingen zijn doorgevoerd in eindpunt query's. 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het vooraf gemaakte entiteiten de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text) project. 

Zie voor meer informatie over hoe de entiteit wordt weergegeven in het query-antwoord van de JSON-eindpunt [gegevensextractie](luis-concept-data-extraction.md)

Nu dat u intenties, uitingen en entiteiten hebt toegevoegd, hebt u een eenvoudige LUIS-app. Meer informatie over het [trainen](luis-how-to-train.md), [testen](luis-interactive-test.md), en [publiceren](luis-how-to-publish-app.md) uw app.
 
