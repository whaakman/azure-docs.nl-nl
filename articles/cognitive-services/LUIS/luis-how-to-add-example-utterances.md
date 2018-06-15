---
title: Voorbeeld utterances in LUIS apps toevoegen | Microsoft Docs
titleSuffix: Azure
description: Informatie over het toevoegen van utterances in Language Understanding (LUIS)-toepassingen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 12cbffdbd9ab299d4405aa50c0e49db9c20ab0fa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345866"
---
# <a name="add-example-utterances-and-label-with-entities"></a>Voorbeeld utterances en label met entiteiten toevoegen

Utterances zijn voorbeelden van gebruiker vragen of opdrachten. Als u wilt leren Language Understanding (LUIS), moet u toevoegen [voorbeeld utterances](luis-concept-utterance.md) naar een [bedoeling](luis-concept-intent.md).

In het algemeen u eerst een utterance toevoegen en vervolgens entiteiten maken en labelen utterances op de pagina met opzet. Als u entiteiten liever eerst maakt, Zie [entiteiten toevoegen](luis-how-to-add-entities.md).

## <a name="add-an-utterance"></a>Een utterance toevoegen
Voer een relevante utterance u van uw gebruikers, zoals verwachten op een pagina opzet `book 2 adult business tickets to Paris tomorrow on Air France` in het tekstvak onder de naam van de opzet en druk op Enter. 
 
>[!NOTE]
>Alle utterances converteert LUIS naar kleine letters.

![Schermopname van Intents pagina met details met utterance gemarkeerd](./media/luis-how-to-add-example-utterances/add-new-utterance-to-intent.png) 

Utterances worden toegevoegd aan de lijst met utterances voor het huidige doel. 

## <a name="add-simple-entity-label"></a>Eenvoudige entiteit label toevoegen
In de volgende procedure maakt en aangepaste entiteiten in de volgende utterance op de pagina opzet label:

```
book me 2 adult business tickets to Paris tomorrow on Air France
```

1. Selecteer 'Lucht Frankrijk' in de utterance label als een enkele entiteit.

    > [!NOTE]
    > Bij het selecteren van woorden label als entiteiten:
    > * Selecteer op van één woord. 
    > * Selecteer voor een set van twee of meer woorden aan het begin en vervolgens aan het einde van de set.

2. Klik in de vervolgkeuzelijst entiteit kunt u een bestaande entiteit selecteren of toevoegen van een nieuwe entiteit. Een nieuwe entiteit toevoegen, typt u de naam in het tekstvak en selecteer vervolgens **nieuwe entiteit maken**. 
 
    ![Schermopname van Intents pagina met details met eenvoudige entiteit labels optie gemarkeerd](./media/luis-how-to-add-example-utterances/create-airline-simple-entity.png)

3. In de **welk type entiteit wilt u maken?** pop-updialoogvenster, Controleer de entiteitsnaam van de en selecteer het entiteitstype eenvoudig en selecteer vervolgens **gedaan**.

    ![Afbeelding van dialoogvenster voor bevestiging](./media/luis-how-to-add-example-utterances/create-simple-airline-entity.png)

    Zie [ophalen van gegevens](luis-concept-data-extraction.md#simple-entity-data) voor meer informatie over het uitpakken van de eenvoudige entiteit van het eindpunt JSON-query-antwoord. Probeer de eenvoudige entiteit [Quick Start](luis-quickstart-primary-and-secondary-data.md) voor meer informatie over het gebruik van een enkele entiteit.


## <a name="add-list-entity-and-label"></a>Lijst entiteit en label toevoegen
Lijst entiteiten vertegenwoordigen een vaste, gesloten reeks (exacte tekst komt overeen met) Verwante woorden in uw systeem. 

U kunt twee genormaliseerde waarden hebben voor een entiteit van de lijst dranken: water en soda pop. De naam van elke genormaliseerde heeft synoniemen. Voor water zijn synoniemen H20, gas, platte. Synoniemen zijn voor soda-pop fruit, cola, gember. U hoeft niet te weten van alle waarden bij het maken van de entiteit. U kunt meer toevoegen nadat u hebt bekeken echte gebruiker utterances met synoniemen.

|Genormaliseerde naam|Synoniemen|
|--|--|
|Water|H20, gas, platte|
|Soda pop-server|Fruit, cola, gember|

Wanneer u een nieuwe entiteit in de lijst van de opzet pagina maakt, doet u twee dingen die mogelijk niet duidelijk. Eerst maakt u een nieuwe lijst door het eerste item in de lijst toe te voegen. Ten tweede heet het eerste item in de lijst met een of meer woorden die u hebt geselecteerd in de utterance. Terwijl u deze later op de entiteitpagina wijzigen kunt, mogelijk sneller een utterance met het woord dat u voor de naam van het lijstitem wilt selecteren.

Bijvoorbeeld, als u wilt maken van een lijst met typen en drinkwaren en geselecteerd het woord `h2o` van de utterance voor het maken van de entiteit de lijst met één item, waarvan de naam h20 is zou hebben. Als u een meer algemene naam, moet u een utterance die gebruikmaakt van de algemene naam kiezen. 

1. Selecteer in de utterance het woord dat is het eerste item in de lijst en voer de naam van de lijst in het tekstvak, vervolgens selecteert u **nieuwe entiteit maken**.   

    ![Schermopname van Intents pagina met details met de nieuwe entiteit maken gemarkeerd](./media/luis-how-to-add-example-utterances/create-drink-list-entity.png)

2. In de **welk type entiteit wilt u maken?** dialoogvenster Voeg synoniemen van dit item in de lijst. Voor het item water in een lijst drank, voegt u `h20`, `perrier`, en `waters`, en selecteer **gedaan**. U ziet dat 'water' is toegevoegd, omdat de lijst synoniemen zijn afgestemd op het niveau van de token. In de cultuur Engels dat niveau op het niveau van word is dus 'wateren' zou niet overeen met 'water' tenzij deze zich in de lijst. 

    ![Schermopname van wat type entiteit wilt u wilt maken in het dialoogvenster](./media/luis-how-to-add-example-utterances/drink-list-ddl.png)

    Deze lijst dranken heeft slechts één drank type, water. U kunt meer drank typen toevoegen door andere utterances labels of bij het bewerken van de entiteit van de **entiteiten** in de linkernavigatiebalk. [Bewerken](luis-how-to-add-entities.md#add-list-entities) de entiteiten biedt u de mogelijkheden van extra items met de bijbehorende synoniemen invoeren of [importeren](luis-how-to-add-entities.md#import-list-entity-values) een lijst. 

    Zie [ophalen van gegevens](luis-concept-data-extraction.md#list-entity-data) voor meer informatie over het uitpakken van de lijst met entiteiten van het eindpunt JSON-query-antwoord. Probeer de [Quick Start](luis-quickstart-intent-and-list-entity.md) voor meer informatie over het gebruik van een entiteit van de lijst.

## <a name="add-synonyms-to-the-list-entity"></a>Synoniemen toevoegen aan de lijst entiteit 
Een synoniem toevoegen aan de entiteit lijst door te selecteren of meer woorden in de utterance. Als u hebt een drank lijst entiteit en wilt toevoegen `agua` als synoniem voor water, volg de stappen:

Selecteer in de utterance het synoniem woord, zoals `aqua` voor water, selecteer vervolgens de naam van de entiteit in de vervolgkeuzelijst zoals **drinken**, selecteer daarna **ingesteld als synoniem**, selecteert u de lijst item gelijk aan, zoals is **water**.

![Schermopname van Intents pagina met details met maken van een nieuwe synoniem gemarkeerd](./media/luis-how-to-add-example-utterances/set-agua-as-synonym.png)

## <a name="create-new-item-for-list-entity"></a>Nieuw item voor lijst entiteit maken
Een nieuw item voor een bestaande lijst entiteit maken door het woord of zinsdeel in de utterance. Als u hebt een drank lijst en wilt toevoegen `tea` als een nieuw item stappen:

Selecteer in de utterance het woord voor het nieuwe lijstitem zoals `tea`, selecteer vervolgens zoals de naam van de entiteit in de vervolgkeuzelijst **drinken**, selecteer daarna **maken van een nieuwe synoniem**. 

![Schermopname van het nieuwe lijstitem toevoegen](./media/luis-how-to-add-example-utterances/list-entity-create-new-item.png)

Het woord is nu in blauw gemarkeerd. Als u de muisaanwijzer op het woord, wordt er een label wordt weergegeven met de naam van een lijst item, bijvoorbeeld thee.

![Schermopname van nieuw lijst item label](./media/luis-how-to-add-example-utterances/list-entity-item-name-tag.png)

## <a name="wrap-entities-in-composite-label"></a>Entiteiten in samengestelde label laten teruglopen
Samengestelde entiteiten zijn gemaakt op basis van **entiteiten**. U kunt een samengestelde entiteit vanuit de opzet pagina maken. Zodra de samengestelde entiteit is gemaakt, kunt u de entiteiten in een utterance op de pagina opzet inpakken. 

Ervan uitgaande dat de utterance `book 2 tickets from Seattle to Cairo`, een samengestelde utterance kunt entiteit informatie ophalen van de telling van tickets (2), de oorsprong (Haarlem) en doel (Caïro)-locaties in een één bovenliggende entiteit. 

Volg deze [stappen](luis-how-to-add-entities.md#add-prebuilt-entity) toevoegen de **getal** vooraf gedefinieerde entiteit. Nadat de entiteit is gemaakt, de `2` in de utterance blauw, die aangeeft dat het een gelabelde entiteit is. Vooraf gedefinieerde entiteiten zijn gemarkeerd door LUIS. U kan toevoegen of verwijderen van het label van het vooraf gedefinieerde entiteit uit een enkele utterance. U kunt alleen toevoegen of verwijderen van de vooraf gedefinieerde labels toe te voegen of te verwijderen van de vooraf gedefinieerde entiteit van de toepassing.

Volg deze [stappen](#add-hierarchical-entity-and-label) maken een **locatie** hiërarchische entiteit. De bron- en doellocaties in het voorbeeld utterance label. 

Voordat u de entiteiten in een samengestelde entiteit inpakt, zorg ervoor dat alle de onderliggende entiteiten zijn gemarkeerd in blauw, wat betekent dat ze in de utterance hebt zijn gelabeld.

1. Als u wilt de afzonderlijke entiteiten in een samengestelde, selecteert u de eerste gelabelde entiteit in de utterance voor de samengestelde entiteit. In de utterance voorbeeld `book 2 tickets from Seattle to Cairo`, de eerste entiteit is het aantal 2. Een vervolgkeuzelijst wordt weergegeven met de opties voor deze selectie.

    ![Schermopname van het aantal dat is geselecteerd en vervolgkeuzelijst opties gemarkeerd](./media/luis-how-to-add-example-utterances/wrap-1.png)

2. Selecteer **teruglopen samengestelde entiteit** uit de vervolgkeuzelijst. 

    ![Schermafbeelding van de vervolgkeuzelijst opties voor samengestelde entiteit wrapping met automatische terugloop in samengestelde entiteit gemarkeerd](./media/luis-how-to-add-example-utterances/wrap-2.png)

3. Selecteer het laatste woord van de samengestelde entiteit. Selecteer in de utterance van dit voorbeeld is 'Location::Destination' (die vertegenwoordigt Caïro). De groene lijn is nu onder de woorden, inclusief niet-entiteit woorden, in de utterance die de samenstelling.

    ![Schermafbeelding van de intentie van de BookFlight pagina met nummer gemarkeerd](./media/luis-how-to-add-example-utterances/wrap-composite.png)

4. Selecteer de naam van de samengestelde entiteit in de vervolgkeuzelijst. In dit voorbeeld is dat **TicketOrder**.

    ![Schermopname van wrapping woorden met samengestelde entiteit met de naam van de samengestelde entiteit is gemarkeerd in de vervolgkeuzelijst](./media/luis-how-to-add-example-utterances/wrap-4.png)

    Wanneer u de entiteiten correct inpakt, is een groene lijn in de hele woordgroep.

    ![Schermopname van utterance met samengestelde entiteit gemarkeerd](./media/luis-how-to-add-example-utterances/wrap-5.png)

    Zie [ophalen van gegevens](luis-concept-data-extraction.md#composite-entity-data) voor meer informatie over het uitpakken van de samengestelde entiteit van het eindpunt JSON-query-antwoord. Probeer de samengestelde entiteit [zelfstudie](luis-tutorial-composite-entity.md) voor meer informatie over het gebruik van een samengestelde entiteit.

## <a name="add-hierarchical-entity-and-label"></a>Hiërarchische entiteit en label toevoegen
Een hiërarchische entiteit is een categorie contextueel geleerde en gerelateerde entiteiten. In het volgende voorbeeld bevat de entiteit oorsprong- en doellocaties. 

In de utterance `Book 2 tickets from Seattle to Cairo`, Seattle is de locatie van de oorsprong en Caïro is de doellocatie. Elke locatie is contextueel verschillende en geleerde van word volgorde en word keuze in de utterance.

1. Op de pagina opzet in de utterance, selecteer "Seattle" en voer de naam van de entiteit ' locatie en selecteer vervolgens **nieuwe entiteit maken**.

    ![Schermopname van maken hiërarchische entiteit labels in het dialoogvenster](./media/luis-how-to-add-example-utterances/create-hier-ent-1.png)

2. Selecteer in het pop-updialoogvenster hiërarchische voor **entiteitstype**, voegt u `Origin` en `Destination` als onderliggende elementen en selecteer vervolgens **gedaan**.

    ![Schermopname van Intents pagina met details met ToLocation entiteit gemarkeerd](./media/luis-how-to-add-example-utterances/create-location-hierarchical-entity.png)

3. Het woord in de utterance is gelabeld met de bovenliggende hiërarchische entiteit. U moet het woord toewijzen aan een onderliggende entiteit. Terug naar de utterance op de pagina met opzet. Selecteer het woord, en vervolgens Kies de naam van de entiteit die u hebt gemaakt in de vervolgkeuzelijst en volgt u het menu rechts de juiste onderliggende entiteit kiezen.

    ![Schermopname van Intents pagina met details met ToLocation entiteit gemarkeerd](./media/luis-how-to-add-example-utterances/label-tolocation.png)

    >[!CAUTION]
    >Onderliggende Entiteitsnamen moeten uniek zijn in alle entiteiten in een enkele app. Twee entiteiten met verschillende hiërarchische mag geen onderliggende entiteiten met dezelfde naam bevatten. 

    Zie [ophalen van gegevens](luis-concept-data-extraction.md#hierarchical-entity-data) voor meer informatie over het uitpakken van de hiërarchische entiteit van het eindpunt JSON-query-antwoord. Probeer de hiërarchische entiteit [Quick Start](luis-quickstart-intent-and-hier-entity.md) voor meer informatie over het gebruik van een hiërarchische entiteit.


## <a name="remove-entity-labels-from-utterances"></a>Labels van de entiteit van utterances verwijderen
U kunt labels machine geleerd entiteit verwijderen uit een utterance op de pagina met opzet. Als de entiteit niet machine geleerd is, kan deze niet worden verwijderd van een utterance. Als u een niet-machine-geleerd entiteit verwijderen uit de utterance moet, moet u de entiteit verwijderen uit de hele app. 

Als een label machine geleerd entiteit uit een utterance verwijderen, selecteert u de entiteit in de utterance. Selecteer vervolgens **Label verwijderen** in het vervolgkeuzemenu entiteit die wordt weergegeven.

![Schermopname van Intents pagina met details met verwijderen Label gemarkeerd](./media/luis-how-to-add-example-utterances/remove-label.png) 

## <a name="add-prebuilt-entity-label"></a>Vooraf gedefinieerde entiteit label toevoegen
Als u de vooraf gedefinieerde entiteiten aan uw app LUIS toevoegt, hoeft u niet te utterances label met deze entiteiten. Zie voor meer informatie over vooraf gedefinieerde entiteiten en hoe u bestanden toevoegt, [entiteiten toevoegen](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="add-regular-expression-entity-label"></a>Reguliere expressie entiteit label toevoegen
Als u de entiteiten reguliere expressie aan uw app LUIS toevoegt, hoeft u niet te utterances label met deze entiteiten. Zie voor meer informatie over de reguliere expressie entiteiten en hoe u bestanden toevoegt, [entiteiten toevoegen](luis-how-to-add-entities.md#add-regular-expression-entities).

## <a name="create-a-pattern-from-an-utterance"></a>Een patroon van een utterance maken
Zie [toevoegen patroon van bestaande utterance op pagina bedoeling of entiteit](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="add-patternany-entity-label"></a>Pattern.any entiteit label toevoegen
Als u de entiteiten pattern.any aan uw app LUIS toevoegt, kan u utterances met deze entiteiten geen naam. Ze zijn alleen geldig in patronen. Zie voor meer informatie over pattern.any entiteiten en hoe u bestanden toevoegt, [entiteiten toevoegen](luis-how-to-add-entities.md#add-patternany-entities).

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
## <a name="train-your-app-after-changing-model-with-utterances"></a>Training van uw app na het model met utterances wijzigen
Nadat u toevoegen, bewerken of verwijderen van utterances, [trainen](luis-how-to-train.md) en [publiceren](PublishApp.md) uw app om uw wijzigingen te invloed hebben op eindpunt query's. 

## <a name="next-steps"></a>Volgende stappen

Na utterances in uw intents labels, kunt u nu maken een [samengestelde entiteit](luis-how-to-add-entities.md).
