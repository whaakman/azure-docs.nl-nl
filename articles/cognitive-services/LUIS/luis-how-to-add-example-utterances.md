---
title: Voorbeeld utterances in LUIS-apps toevoegen | Microsoft Docs
titleSuffix: Azure
description: Informatie over het toevoegen van uitingen in Language Understanding (LUIS)-toepassingen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: cf6fc131a1ae2ba0a77bdbeda942c9f85aadd189
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39226284"
---
# <a name="add-example-utterances-and-label-with-entities"></a>Voorbeeld-uitingen en label met entiteiten toevoegen

Voorbeeld uitingen zijn voorbeelden van de tekst van de gebruiker vragen of opdrachten. Om te leren Language Understanding (LUIS), die u nodig hebt om toe te voegen [voorbeeld uitingen](luis-concept-utterance.md) naar een [bedoeling](luis-concept-intent.md).

Over het algemeen een utterance voorbeeld eerst aan een doel toevoegen en vervolgens maken van entiteiten en label uitingen op de pagina intentie. Als u entiteiten in plaats daarvan eerst maakt, Zie [entiteiten toevoegen](luis-how-to-add-entities.md).

## <a name="add-an-utterance"></a>Een utterance toevoegen
Geef een voorbeeld van de relevante utterance die u kunt van uw gebruikers, zoals verwachten op een pagina intentie `book 2 adult business tickets to Paris tomorrow on Air France` in het tekstvak onder de naam van de intentie en druk op Enter. 
 
>[!NOTE]
>LUIS worden alle uitingen geconverteerd naar kleine letters.

![Schermafbeelding van de intenties pagina, met utterance gemarkeerd](./media/luis-how-to-add-example-utterances/add-new-utterance-to-intent.png) 

Uitingen worden toegevoegd aan de lijst met uitingen voor het huidige doel. 

## <a name="ignoring-words-and-punctuation"></a>Woorden en leestekens worden genegeerd
Als u wilt dat specifieke woorden of de interpunctieteken in het voorbeeld utterance negeren, gebruikt u een [patroon](luis-concept-patterns.md#pattern-syntax) met de _negeren_ syntaxis. 

## <a name="add-simple-entity-label"></a>Label voor eenvoudige entiteit toevoegen
In de volgende procedure maakt u maakt en aangepaste entiteiten in de volgende utterance op de intentie pagina labelen:

```
book me 2 adult business tickets to Paris tomorrow on Air France
```

1. Selecteer "Lucht Frankrijk" in de utterance label als een enkele entiteit.

    > [!NOTE]
    > Bij het selecteren van woorden label als entiteiten:
    > * Selecteer op van één woord. 
    > * Selecteer voor een set van twee of meer woorden, aan het begin en vervolgens aan het einde van de set.

2. In het vak van de entiteit vervolgkeuzelijst die wordt weergegeven, kunt u een bestaande entiteit selecteren of een nieuwe entiteit toevoegen. Als u wilt een nieuwe entiteit toevoegen, typt u de naam in het tekstvak in en selecteer vervolgens **nieuwe entiteit maken**. 
 
    ![Schermafbeelding van de intenties pagina, met eenvoudige entiteit labeling-optie is gemarkeerd](./media/luis-how-to-add-example-utterances/create-airline-simple-entity.png)

3. In de **welk type entiteit wilt u maken?** pop-updialoogvenster, Controleer of de naam van de entiteit en selecteer het entiteitstype eenvoudig en selecteer vervolgens **gedaan**.

    ![Afbeelding van dialoogvenster voor bevestiging](./media/luis-how-to-add-example-utterances/create-simple-airline-entity.png)

    Zie [gegevensextractie](luis-concept-data-extraction.md#simple-entity-data) voor meer informatie over het uitpakken van de eenvoudige entiteit van het eindpunt JSON-query-antwoord. Probeer de eenvoudige entiteit [snelstartgids](luis-quickstart-primary-and-secondary-data.md) voor meer informatie over het gebruik van een enkele entiteit.


## <a name="add-list-entity-and-label"></a>Lijst met entiteiten en label toevoegen
Lijst met entiteiten vertegenwoordigen een vaste en gesloten set (exact overeenkomende tekst overeenkomt met) Verwante woorden in uw systeem. 

U kunt twee genormaliseerde waarden hebben voor een entiteit uit de lijst met drank: water en soda pop-server. De naam van elke genormaliseerde heeft synoniemen. Voor water worden synoniemen H20, gas, zonder submappen. Soda-pop zijn synoniemen, er vruchten cola gember. U hoeft niet te weten van alle waarden bij het maken van de entiteit. U kunt meer toevoegen nadat u hebt bekeken real-user-uitingen door synoniemen.

|De naam van genormaliseerde|Synoniemen|
|--|--|
|Water|H20, gas, vast|
|Soda-pop-server|Fruit, cola, gember|

Bij het maken van een nieuwe entiteit in de lijst met de intentie pagina, kunt u doet twee dingen die mogelijk niet duidelijk. Eerst maakt u een nieuwe lijst door het eerste item in de lijst toe te voegen. Ten tweede heet het eerste item in de lijst met het woord of zinsdeel die u hebt geselecteerd in de utterance. Terwijl u deze later vanuit de entiteitpagina wijzigen kunt, kan het zijn sneller een utterance waarin het woord die u wilt gebruiken voor de naam van het item in de lijst selecteren.

Bijvoorbeeld, als u wilt maken van een lijst met typen drinken en u hebt geselecteerd het woord `h2o` uit de utterance om de entiteit te maken, de lijst met één item, waarvan de naam h20 is zou hebben. Als u een meer algemene naam, moet u een utterance die gebruikmaakt van de algemene naam kiezen. 

1. In de utterance, selecteert u het woord dat is het eerste item in de lijst en klikt u vervolgens de naam van de lijst invoeren in het tekstvak en selecteer vervolgens **nieuwe entiteit maken**.   

    ![Schermafbeelding van de intenties pagina, met de nieuwe entiteit maken gemarkeerd](./media/luis-how-to-add-example-utterances/create-drink-list-entity.png)

2. In de **welk type entiteit wilt u maken?** dialoogvenster vak, synoniemen van dit item in de lijst toevoegen. Voor het water-item in een lijst drank toevoegen `h20`, `perrier`, en `waters`, en selecteer **gedaan**. U ziet dat "water" is toegevoegd, omdat de lijst met synoniemen zijn afgestemd op het niveau van het token. In de Engelse cultuur dat niveau op het niveau van word is dus "wateren" zou niet overeen met de "water", tenzij deze in de lijst is. 

    ![Schermopname van het type entiteit wilt u wilt maken in het dialoogvenster](./media/luis-how-to-add-example-utterances/drink-list-ddl.png)

    Deze lijst van drank heeft slechts één drank type water. U kunt meer drank typen toevoegen door andere uitingen labels of door het bewerken van de entiteit op basis van de **entiteiten** in het linkernavigatievenster. [Bewerken](luis-how-to-add-entities.md#add-list-entities) de entiteiten kunt u de opties van extra items met de bijbehorende synoniemen invoeren of [importeren](luis-how-to-add-entities.md#import-list-entity-values) een lijst. 

    Zie [gegevensextractie](luis-concept-data-extraction.md#list-entity-data) voor meer informatie over de lijst met entiteiten extraheren uit het eindpunt van de JSON-query-antwoord. Probeer de [snelstartgids](luis-quickstart-intent-and-list-entity.md) voor meer informatie over het gebruik van een entiteit lijst.

## <a name="add-synonyms-to-the-list-entity"></a>Synoniemen toevoegen aan de lijst-entiteit 
Een synoniem toevoegen aan de lijst met entiteit door te selecteren van het woord of zinsdeel in de utterance. Als u een drank lijst entiteit en wilt toevoegen hebt `agua` als een synoniem voor water, volgt u de stappen uit:

Selecteer in de utterance het synoniem woord, zoals `aqua` voor water, selecteer vervolgens de naam van de entiteit in de vervolgkeuzelijst, zoals **drinken**en selecteer vervolgens **instellen als synoniem**, selecteer de lijst item gelijk aan, zoals is **water**.

![Schermafbeelding van de intenties pagina, met het maken van een nieuwe synoniem gemarkeerd](./media/luis-how-to-add-example-utterances/set-agua-as-synonym.png)

## <a name="create-new-item-for-list-entity"></a>Nieuw item voor een lijst met entiteit maken
Maak een nieuw item voor een bestaande entiteit in de lijst met door het selecteren van het woord of zinsdeel in de utterance. Als u een drank lijst en wilt toevoegen hebt `tea` als een nieuw item, de volgende stappen:

In de utterance, selecteert u het woord voor het nieuwe lijstitem zoals `tea`, selecteer de naam van de entiteit in de vervolgkeuzelijst, zoals **drinken**en selecteer vervolgens **maken van een nieuwe synoniem**. 

![Schermafbeelding van het nieuwe item in de lijst toevoegen](./media/luis-how-to-add-example-utterances/list-entity-create-new-item.png)

Het woord is nu in blauw gemarkeerd. Als u de muisaanwijzer op het woord, wordt er een label wordt weergegeven met de naam van een lijst met item, bijvoorbeeld thee.

![Schermafbeelding van de nieuwe lijst item tag](./media/luis-how-to-add-example-utterances/list-entity-item-name-tag.png)

## <a name="wrap-entities-in-composite-label"></a>Entiteiten in samengestelde label verpakken
Samengestelde entiteiten zijn gemaakt op basis van **entiteiten**. U kunt een samengestelde entiteit vanuit de intentie pagina maken. Zodra de samengestelde entiteit is gemaakt, kunt u de entiteiten in een utterance op de intentie pagina inpakken. 

Ervan uitgaande dat de utterance `book 2 tickets from Seattle to Cairo`, een samengestelde utterance kunt entiteit informatie ophalen van het aantal tickets (2), de oorsprong (Amsterdam) en de bestemming (Cairo)-locaties in een entiteit één bovenliggend item. 

Volg deze [stappen](luis-how-to-add-entities.md#add-prebuilt-entity) om toe te voegen de **getal** vooraf gedefinieerde entiteit. Nadat de entiteit is gemaakt, de `2` in de utterance blauw, aangeeft dat het een gelabelde entiteit is. Vooraf gemaakte entiteiten zijn gelabeld met LUIS. U kan toevoegen of het label van de vooraf gedefinieerde entiteit verwijderen uit een enkele utterance. U kunt alleen toevoegen of verwijderen van de vooraf gedefinieerde labels toevoegen of verwijderen van de vooraf gedefinieerde entiteit van de toepassing.

Volg deze [stappen](#add-hierarchical-entity-and-label) maken een **locatie** hiërarchische entiteit. De bron en doel-locaties in het voorbeeld utterance van label. 

Voordat u de entiteiten in een samengestelde entiteit inpakt, zorg ervoor dat alle onderliggende entiteiten zijn gemarkeerd in blauw, wat betekent dat ze hebben is met het label in de utterance.

1. Als u wilt de afzonderlijke entiteiten in een samengestelde, selecteert u de eerste gelabelde entiteit in de utterance voor de samengestelde entiteit. In de voorbeeld-utterance `book 2 tickets from Seattle to Cairo`, de eerste entiteit is het aantal 2. Een vervolgkeuzelijst wordt weergegeven met de opties voor deze selectie.

    ![Schermopname van het getal dat is geselecteerd en gemarkeerde vervolgkeuzelijst-opties](./media/luis-how-to-add-example-utterances/wrap-1.png)

2. Selecteer **verpakken samengestelde entiteit** uit de vervolgkeuzelijst. 

    ![Schermafbeelding van de vervolgkeuzelijst opties voor samengestelde entiteit wrapping met tekstterugloop in samengestelde entiteit is gemarkeerd](./media/luis-how-to-add-example-utterances/wrap-2.png)

3. Selecteer het laatste woord van de samengestelde entiteit. Selecteer in de utterance van dit voorbeeld 'Location::Destination' (die vertegenwoordigt Cairo). De groene lijn is nu onder de woorden, met inbegrip van niet-entiteit woorden, in de utterance die de samenstelling.

    ![Schermafbeelding van de intentie van de BookFlight-pagina, met nummer gemarkeerd](./media/luis-how-to-add-example-utterances/wrap-composite.png)

4. Selecteer de entiteitsnaam van de samengestelde in de vervolgkeuzelijst. In dit voorbeeld is dat **TicketOrder**.

    ![Schermafbeelding van de wrapping woorden met samengestelde entiteit met de naam van de samengestelde entiteit is gemarkeerd in de vervolgkeuzelijst](./media/luis-how-to-add-example-utterances/wrap-4.png)

    Wanneer u de entiteiten correct inpakt, is er een groene lijn onder de volledige woordgroep.

    ![Schermafbeelding van utterance met samengestelde entiteit is gemarkeerd](./media/luis-how-to-add-example-utterances/wrap-5.png)

    Zie [gegevensextractie](luis-concept-data-extraction.md#composite-entity-data) voor meer informatie over het uitpakken van de samengestelde entiteit van het eindpunt JSON-query-antwoord. Probeer de samengestelde entiteit [zelfstudie](luis-tutorial-composite-entity.md) voor meer informatie over het gebruik van een samengestelde entiteit.

## <a name="add-hierarchical-entity-and-label"></a>Hiërarchische entiteit en label toevoegen
Een hiërarchische entiteit is een soort contextueel geleerde en gerelateerde entiteiten. In het volgende voorbeeld bevat de entiteit bron en doel-locaties. 

In de utterance `Book 2 tickets from Seattle to Cairo`, Seattle, is de verzendingslocatie en Cairo is de doellocatie. Elke locatie is contextueel verschillende en geleerde van woordvolgorde en word keuze in de utterance.

1. Op de intentie pagina, in de utterance, selecteert u "Seattle" en voer de naam van de entiteit ' locatie en selecteer vervolgens **nieuwe entiteit maken**.

    ![Schermafbeelding van maken hiërarchische entiteit labels in het dialoogvenster](./media/luis-how-to-add-example-utterances/create-hier-ent-1.png)

2. Selecteer in het pop-updialoogvenster hiërarchische voor **entiteitstype**, voeg deze `Origin` en `Destination` als onderliggende items en selecteer vervolgens **gedaan**.

    ![Schermafbeelding van de intenties pagina, met ToLocation entiteit is gemarkeerd](./media/luis-how-to-add-example-utterances/create-location-hierarchical-entity.png)

3. Het woord in de utterance is gelabeld met de bovenliggende hiërarchische entiteit. U moet het woord toewijzen aan een onderliggende entiteit. Ga terug naar de utterance op de pagina intentie. Selecteert u het woord en kies de naam van de entiteit die u hebt gemaakt in de vervolgkeuzelijst en volgt u het menu aan de rechterkant de juiste onderliggende entiteit kiezen.

    ![Schermafbeelding van de intenties pagina, met ToLocation entiteit is gemarkeerd](./media/luis-how-to-add-example-utterances/label-tolocation.png)

    >[!CAUTION]
    >Namen van de onderliggende entiteiten moeten uniek zijn in alle entiteiten in een enkele app. Twee entiteiten met verschillende hiërarchische mag geen onderliggende entiteiten met dezelfde naam bevatten. 

    Zie [gegevensextractie](luis-concept-data-extraction.md#hierarchical-entity-data) voor meer informatie over het uitpakken van de hiërarchische entiteit van het eindpunt JSON-query-antwoord. Probeer de hiërarchische entiteit [snelstartgids](luis-quickstart-intent-and-hier-entity.md) voor meer informatie over het gebruik van een hiërarchische entiteit.


## <a name="remove-entity-labels-from-utterances"></a>Entiteit verwijderen van uitingen
U kunt machine geleerd entiteit labels verwijderen uit een utterance op de pagina intentie. Als de entiteit niet machine geleerd, kan deze kan niet worden verwijderd uit een utterance. Als u een niet-machine-geleerd entiteit verwijderen uit de utterance moet, moet u de entiteit verwijderen uit de hele app. 

Als een label hebt geleerd van een machine entiteit uit een utterance verwijderen, selecteert u de entiteit in de utterance. Selecteer vervolgens **Label verwijderen** in de entiteit vervolgkeuzelijst die wordt weergegeven.

![Schermafbeelding van de intenties detailpagina, met verwijdert Label gemarkeerd](./media/luis-how-to-add-example-utterances/remove-label.png) 

## <a name="add-prebuilt-entity-label"></a>Vooraf gedefinieerde entiteit label toevoegen
Als u de vooraf gemaakte entiteiten aan uw LUIS-app toevoegt, moet u geen label uitingen met deze entiteiten. Zie voor meer informatie over het vooraf gemaakte entiteiten en hoe u bestanden toevoegt, [entiteiten toevoegen](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="add-regular-expression-entity-label"></a>Reguliere expressie entiteit label toevoegen
Als u de reguliere expressie entiteiten aan uw LUIS-app toevoegt, moet u geen label uitingen met deze entiteiten. Zie voor meer informatie over de reguliere expressie entiteiten en hoe u bestanden toevoegt, [entiteiten toevoegen](luis-how-to-add-entities.md#add-regular-expression-entities).

## <a name="create-a-pattern-from-an-utterance"></a>Maken van een patroon van een utterance
Zie [toevoegen patroon uit bestaande utterance op de pagina kunt u lezen wat of entiteit](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="add-patternany-entity-label"></a>Pattern.any entiteit label toevoegen
Als u de entiteiten pattern.any aan uw LUIS-app toevoegt, kunt u uitingen met deze entiteiten kan geen label. Ze zijn alleen geldig in patronen. Zie voor meer informatie over pattern.any entiteiten en het toe te voegen, [entiteiten toevoegen](luis-how-to-add-entities.md#add-patternany-entities).

<!--
Fix this - moved to luis-how-to-add-intents.md - how ?

## Search in utterances
## Prediction discrepancy errors
## Filter by intent prediction discrepancy errors
## Filter by entity type
## Switch to token view
## Delete utterances
## Edit an utterance
## Reassign utterances

-->
## <a name="train-your-app-after-changing-model-with-utterances"></a>Uw app na het wijzigen van model met uitingen trainen
Na het toevoegen, bewerken of verwijderen van uitingen, [trainen](luis-how-to-train.md) en [publiceren](luis-how-to-publish-app.md) uw app voor uw wijzigingen zijn doorgevoerd in eindpunt query's. 

## <a name="next-steps"></a>Volgende stappen

Na de labels uitingen in uw intenties, u kunt nu maken een [samengestelde entiteit](luis-how-to-add-entities.md).
