---
title: Entiteiten in LUIS-apps toevoegen | Microsoft Docs
titleSuffix: Azure
description: Entiteiten (belangrijke gegevens in het domein van uw toepassing) in Language Understanding (LUIS)-apps toevoegen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: e97f9a5391799849983bd98db5400e0a842627b7
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224123"
---
# <a name="manage-entities"></a>Entiteiten beheren
Nadat u uw app hebt geïdentificeerd [intents](luis-concept-intent.md), moet u [label voorbeeld uitingen](luis-concept-utterance.md) met [entiteiten](luis-concept-entity-types.md). Entiteiten kunnen worden de belangrijke onderdelen van een opdracht of een vraag, en essentieel zijn voor uw client-app de taak uit te voeren. 

U kunt toevoegen, bewerken of verwijderen van entiteiten in uw app met de **entiteitenlijst** op de **entiteiten** pagina. LUIS biedt twee typen entiteiten: [vooraf gemaakte entiteiten](luis-reference-prebuilt-entities.md), en uw eigen aangepaste entiteiten.

De volgende secties zijn alleen beschikbaar is binnen een LUIS-app uit de **bouwen** sectie. De **bouwen** koppeling bevindt zich in de bovenste navigatiebalk. Eenmaal binnen de **bouwen** sectie, selecteer **entiteiten** in het navigatiemenu links. Zodra een entiteit wordt toegevoegd aan de toepassing als de entiteit is de machine geleerd, kunt u [label van de entiteit](luis-how-to-add-example-utterances.md) binnen de utterance. Zodra de app is getraind en gepubliceerd, kunt u entiteitsgegevens ontvangen [geëxtraheerd](luis-concept-data-extraction.md) uit de voorspelling. 

## <a name="add-prebuilt-entity"></a>Vooraf gedefinieerde entiteit toevoegen
Vooraf gemaakte entiteiten zijn gedefinieerd in de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text) project. Algemene vooraf gemaakte entiteiten die zijn toegevoegd aan een toepassing worden *getal* en *datetimeV2*. 

1. In uw app uit de **bouwen** uit en klik vervolgens op **entiteiten** in het linkerpaneel.
 
2. Op de **entiteiten** weergeeft, schakelt **vooraf gemaakte entiteiten beheren**.

    ![Schermafbeelding van het vooraf gedefinieerde entiteit op de pagina entiteiten toevoegen](./media/add-entities/manage-prebuilt-entities-button.png)

3. In **toevoegen of verwijderen van vooraf gemaakte entiteiten** in het dialoogvenster, selecteer de **getal** en **datetimeV2** vooraf gemaakte entiteiten. Selecteer vervolgens **gedaan**.

    ![Dialoogvenster voor vooraf gedefinieerde entiteit schermopname toevoegen](./media/add-entities/list-of-prebuilt-entities.png)

    Zie [gegevensextractie](luis-concept-data-extraction.md#prebuilt-entity-data) voor meer informatie over het uitpakken van de vooraf gedefinieerde entiteit van het eindpunt JSON-query-antwoord.

## <a name="add-simple-entities"></a>Eenvoudige entiteiten toevoegen
Een enkele entiteit is een algemene entiteit die een enkele concept wordt beschreven. 

1. In uw app uit de **bouwen** uit en klik vervolgens op **entiteiten** in het linkerdeelvenster en selecteer vervolgens **nieuwe entiteit maken**.

    ![Schermafbeelding van entiteiten pagina met nieuwe entiteit knop maken gemarkeerd](./media/add-entities/create-new-entity-button.png)

2. Typ in het pop-updialoogvenster `Airline` in de **entiteitnaam** Schakel **eenvoudige** uit de **entiteitstype** lijst en selecteer vervolgens **gedaan**.

    ![Schermafbeelding van het dialoogvenster voor het maken van eenvoudige luchtvaartmaatschappij-entiteit](./media/add-entities/create-simple-airline-entity.png)

    Zie [gegevensextractie](luis-concept-data-extraction.md#simple-entity-data) voor meer informatie over het uitpakken van de eenvoudige entiteit van het eindpunt JSON-query-antwoord. Probeer de eenvoudige entiteit [snelstartgids](luis-quickstart-primary-and-secondary-data.md) voor meer informatie over het gebruik van een enkele entiteit.

## <a name="add-regular-expression-entities"></a>Reguliere expressie entiteiten toevoegen
Een entiteit reguliere expressie wordt gebruikt voor het ophalen van gegevens uit de utterance op basis van een reguliere expressie die u opgeeft. 

1. Selecteer in uw app **entiteiten** uit de navigatiebalk aan de linkerkant en selecteer vervolgens **nieuwe entiteit maken**.

2. In het pop-upvenster in het dialoogvenster, type `AirFrance Flight` in de **entiteitnaam** Schakel **reguliere expressie** uit de **entiteitstype** , voert u de reguliere expressie `AFR[0-9]{3,4}`, en selecteer vervolgens **gedaan**. 

    Deze AirFrance Flight reguliere expressie verwacht drie tekens, letterlijk `AFR`, klikt u vervolgens 3 of 4 cijfers. De cijfers mag een willekeurig getal tussen 0 en 9. De reguliere expressie die overeenkomt met AirFrance flight getallen zoals: "AFR101", "ARF1302" en 'AFR5006'. Zie [gegevensextractie](luis-concept-data-extraction.md) voor meer informatie over het uitpakken van de entiteit van het eindpunt JSON-query-antwoord.

    ![Afbeelding van dialoogvenster entiteit van de reguliere expressie maken](./media/add-entities/regex-entity-create-dialog.png)

    Zie [gegevensextractie](luis-concept-data-extraction.md#regular-expression-entity-data) voor meer informatie over het uitpakken van de entiteit reguliere expressie van het eindpunt JSON-query-antwoord. Probeer de reguliere expressie entiteit [snelstartgids](luis-quickstart-intents-regex-entity.md) voor meer informatie over het gebruik van een entiteit reguliere expressie.

## <a name="add-hierarchical-entities"></a>Hiërarchische entiteiten toevoegen
Een hiërarchische entiteit is een soort contextueel geleerde en gerelateerde entiteiten. In het volgende voorbeeld bevat de entiteit bron en doel-locaties. 

In de utterance `Book 2 tickets from Seattle to Cairo`, Seattle, is de verzendingslocatie en Cairo is de doellocatie. Elke locatie is contextueel verschillende en geleerde van woordvolgorde en word keuze in de utterance.

Hiërarchische entiteiten toevoegen, voert u de volgende stappen uit: 

1. Selecteer in uw app **entiteiten** uit de navigatiebalk aan de linkerkant en selecteer vervolgens **nieuwe entiteit maken**.

2. Typ in het pop-updialoogvenster `Location` in de **entiteitnaam** vak en selecteer vervolgens **hiërarchisch** uit de **entiteitstype** lijst.

    ![Een hiërarchische entiteit toevoegen](./media/add-entities/hier-location-entity-creation.png)

3. Selecteer **onderliggende toevoegen**, en typt u 'Origin' in **onderliggende #1** vak. 

4. Selecteer **onderliggende toevoegen**, en typt u 'Doel' in **onderliggende 2** vak. Selecteer **Done**.
5. 
    >[!NOTE]
    >Selecteer het prullenbakpictogram naast het wilt verwijderen van een onderliggend element.

    >[!CAUTION]
    >Namen van de onderliggende entiteiten moeten uniek zijn in alle entiteiten in een enkele app. Twee entiteiten met verschillende hiërarchische mag geen onderliggende entiteiten met dezelfde naam bevatten. 

    Zie [gegevensextractie](luis-concept-data-extraction.md#hierarchical-entity-data) voor meer informatie over het uitpakken van de hiërarchische entiteit van het eindpunt JSON-query-antwoord. Probeer de hiërarchische entiteit [snelstartgids](luis-quickstart-intent-and-hier-entity.md) voor meer informatie over het gebruik van een hiërarchische entiteit.

## <a name="add-composite-entities"></a>Samengestelde entiteiten toevoegen
U kunt de relaties tussen verschillende bestaande entiteiten met het maken van een samengestelde entiteit definiëren. In het volgende voorbeeld bevat de entiteit aantal tickets, de herkomst en de locaties van de bestemming. 

In de utterance `Book 2 tickets from Seattle to Cairo`, het getal 2 wordt vergeleken met een vooraf gedefinieerde entiteit, Seattle is de locatie van de oorsprong en Cairo is de doellocatie. Elke entiteit is onderdeel van een grotere, bovenliggende entiteit nadat de samengestelde entiteit is gemaakt.

1. Voeg in uw app, de vooraf gedefinieerde entiteit **nummer**. Zie voor instructies [vooraf gemaakte entiteiten toevoegen](#add-prebuilt-entity). 

2. De hiërarchische entiteit toevoegen `Location`, met inbegrip van de subtypen: `origin`, `destination`. Zie voor meer instructies [hiërarchische entiteiten toevoegen](#add-hierarchical-entities). 

3. Selecteer **entiteiten** uit de navigatiebalk aan de linkerkant en selecteer vervolgens **nieuwe entiteit maken**.

4. Typ in het pop-updialoogvenster `TicketsOrder` in de **entiteitnaam** vak en selecteer vervolgens **samengestelde** uit de **entiteitstype** lijst.

5. Selecteer **onderliggende toevoegen** om toe te voegen een nieuw onderliggend element.

6. In **onderliggende #1**, selecteer de entiteit **getal** in de lijst.

7. In **onderliggende 2**, selecteer de entiteit **Location::Origin** in de lijst. 

8. In **onderliggende #3**, selecteer de entiteit **Location::Destination** in de lijst. 

9. Selecteer **Done**.

    ![Afbeelding van dialoogvenster om een samengestelde entiteit te maken](./media/add-entities/ticketsorder-composite-entity.png)

    >[!NOTE]
    >Selecteer de knop Prullenbak naast het wilt verwijderen van een onderliggend element.

    Zie [gegevensextractie](luis-concept-data-extraction.md#composite-entity-data) voor meer informatie over het uitpakken van de samengestelde entiteit van het eindpunt JSON-query-antwoord. Probeer de samengestelde entiteit [zelfstudie](luis-tutorial-composite-entity.md) voor meer informatie over het gebruik van een samengestelde entiteit.


## <a name="add-patternany-entities"></a>Pattern.any entiteiten toevoegen
[Pattern.ANY](luis-concept-entity-types.md) entiteiten zijn alleen geldig in [patronen](luis-how-to-model-intent-pattern.md). Deze entiteit kunt LUIS vinden het einde van de entiteiten met verschillende lengte en word keuze. Omdat deze entiteit wordt gebruikt in een patroon, LUIS u weet waar het einde van de entiteit zich bevindt in de utterance.

Als een app heeft een `FindBookInfo` intentie, de titel van het boek kan leiden tot problemen met de intentie voorspelling. Gebruiken om te verduidelijken welke woorden, zijn in de titel adresboek, een Pattern.any binnen een patroon. De voorspelling LUIS begint met de utterance. De utterance wordt eerst gecontroleerd en afgestemd voor entiteiten, wanneer de entiteiten worden gevonden, wordt het patroon wordt gecontroleerd en afgestemd. 

In de utterance `Who wrote the book Ask and when was it published?`, de titel van het boek, vraag, is erg lastig omdat het geen context voor de hand liggende waar de titel wordt beëindigd en waar de rest van de utterance begint. Titels zijn een willekeurige volgorde van woorden met inbegrip van één woord, complexe zinnen met interpunctie en wachtwoord ordening van woorden. Een patroon kunt u een entiteit maken waar de volledige en exacte entiteit kan worden geëxtraheerd. Als de titel van het boek wordt gevonden, de `FindBookInfo` bedoeling omdat dat het doel van het patroon wordt voorspeld.

1. In uw app uit de **bouwen** uit en klik vervolgens op **entiteiten** in het linkerdeelvenster en selecteer vervolgens **nieuwe entiteit maken**.

2. In de **entiteit toevoegen** in het dialoogvenster, type `BookTitle` in de **entiteitnaam** vak en selecteer **Pattern.any** als de **entiteitstype**.
 
    ![Schermopname van het maken van een entiteit pattern.any](./media/add-entities/create-pattern-any-entity.png)

    Een patroon voor het gebruik van de entiteit pattern.any toevoegt op de **patronen** pagina (onder de **verbeterde app-prestaties** sectie) met de syntaxis van de juiste accolade toe, zoals ' voor **{BookTitle}** die de auteur is? ".

    Zie [gegevensextractie](luis-concept-data-extraction.md#patternany-entity-data) voor meer informatie over de entiteit Pattern.any extraheren uit het eindpunt van de JSON-query-antwoord. Probeer de [patroon](luis-tutorial-pattern.md) zelfstudie voor meer informatie over het gebruik van een entiteit Pattern.any.

Als u dat het patroon zien wanneer deze een Pattern.any bevat, extraheert entiteiten onjuist, gebruikt u een [expliciete lijst](luis-concept-patterns.md#explicit-lists) om dit probleem te verhelpen. 

## <a name="add-role-to-pattern-based-entity"></a>Functie toevoegen aan entiteit op basis van het patroon
Een rol is een benoemde subtype van een entiteit op basis van context. Het is vergelijkbaar met een [hiërarchische](#add-hierarchical-entities) entiteit, maar functies worden alleen gebruikt [patronen](luis-how-to-model-intent-pattern.md). 

Een ticket gegevenslaag heeft bijvoorbeeld een *oorsprong plaats* en een *plaats van bestemming*, maar zijn beide steden. LUIS bepaalt dat beide steden zijn en de bron en doel steden op basis van de context van de woordvolgorde en word keuze kunnen bepalen. 

De syntaxis voor een rol is **{naam: rol entiteitnaam}** waar de naam van de entiteit wordt gevolgd door een dubbele punt, gevolgd door de rolnaam. Bijvoorbeeld 'Book een ticket bij {locatie: oorsprong} naar {locatie: Destination}'.

1. In uw app uit de **bouwen** uit en selecteer vervolgens **entiteiten** in het linkerpaneel.

2. Selecteer **Create new intent**. Voer de naam van `Location`. Selecteer het type **eenvoudige** en selecteer **gedaan**

3. Selecteer **entiteiten** in het linkerpaneel, selecteer vervolgens de nieuwe entiteit **locatie** in stap 2 hebt gemaakt.

4. In de **rolnaam** tekstvak, voer de naam van de rol `Origin` en invoeren. Voeg de naam van een tweede functie van `Destination`. Bijvoorbeeld kan een reis vlak een oorsprong en de plaats van een bestemming hebben. De twee rollen zijn 'Origin' en 'Doel'.

    ![Schermafbeelding van de oorsprong rol toe te voegen aan de entiteit locatie](./media/add-entities/roles-enter-role-name-text.png)

    Zie [gegevensextractie](luis-concept-data-extraction.md) voor meer informatie over het uitpakken van de functies van het eindpunt JSON-query-antwoord. Raadpleeg de zelfstudie patroon voor meer informatie over het gebruik van een entiteit Pattern.any.

## <a name="add-list-entities"></a>Lijst met entiteiten toevoegen
Lijst met entiteiten vertegenwoordigen een vaste en gesloten set verwante woorden in uw systeem. 

U kunt twee genormaliseerde waarden hebben voor een entiteit uit de lijst met drank: water en soda pop-server. De naam van elke genormaliseerde heeft synoniemen. Voor water worden synoniemen H20, gas, zonder submappen. Soda-pop zijn synoniemen, er vruchten cola gember. U hoeft niet te weten van alle waarden bij het maken van de entiteit. U kunt meer toevoegen nadat u hebt bekeken real-user-uitingen door synoniemen.

|De naam van genormaliseerde|Synoniemen|
|--|--|
|Water|H20, gas, vast|
|Soda-pop-server|Fruit, cola, gember|

1. In uw app uit de **bouwen** uit en klik vervolgens op **entiteiten** in het linkerdeelvenster en selecteer vervolgens **nieuwe entiteit maken**.

2. In de **entiteit toevoegen** in het dialoogvenster, type `Drinks` in de **entiteitnaam** vak en selecteer **lijst** als de **entiteitstype**. Selecteer **Done**.
 
    ![Afbeelding van dialoogvenster drank lijst entiteit maken](./media/add-entities/menu-list-dialog.png)
  
3.  De entiteit-pagina van de lijst kunt u genormaliseerde namen toevoegen. In de **waarden** tekstvak, voert u een item voor de lijst, zoals `Water` voor de drank lijst en druk op Enter op het toetsenbord. 

    ![Schermafbeelding van drank lijst entiteit met water genormaliseerd waarde in het tekstvak](./media/add-entities/entity-list-normalized-name.png)

4. Aan de rechterkant van de genormaliseerde waarde **water**, Voer synoniemen `h20`, `flat`, en `gas`, drukt u op Enter na elk item op.

    ![Schermafbeelding van drank lijst entiteit met synoniem-items voor water gemarkeerd](./media/add-entities/menu-list-synonyms.png)

5. Als u meer genormaliseerde items voor de lijst wilt, selecteert u **raden** om te zien van de opties uit de [semantische woordenlijst](luis-glossary.md#semantic-dictionary).

    ![Schermafbeelding van drinken lijst entiteit met de aanbevolen items die zijn gemarkeerd](./media/add-entities/entity-list-recommended-list.png)

6. Selecteer een item in de aanbevolen lijst toe te voegen als een genormaliseerde waarde of selecteert **alle** alle items kunnen worden toegevoegd. 

    ![Schermafbeelding van drinken lijst van de entiteit met de aanbevolen item van smaakt en alle knop gemarkeerd toevoegen](./media/add-entities/list-entity-add-suggestions.png)

    Zie [gegevensextractie](luis-concept-data-extraction.md#list-entity-data) voor meer informatie over de lijst met entiteiten extraheren uit het eindpunt van de JSON-query-antwoord. Probeer de [snelstartgids](luis-quickstart-intent-and-list-entity.md) voor meer informatie over het gebruik van een entiteit lijst.

## <a name="import-list-entity-values"></a>De lijstwaarden entiteit importeren
U kunt waarden kunt importeren in een bestaande entiteit in de lijst.

 1. Selecteer op de pagina van de entiteit lijst **lijsten importeren**.

 2. In **nieuwe vermeldingen van het importeren van** in het dialoogvenster, selecteer **bestand kiezen** en selecteert u het JSON-bestand met de lijst.

    ![Schermafbeelding van importeren eenheid waarden pop-upvenster keuzelijst](./media/add-entities/menu-list-import-json-dialog-with-file.png)

    >[!NOTE]
    >LUIS importeert bestanden met de extensie '.json' alleen.

 3. Voor meer informatie over de syntaxis van de lijst met ondersteunde in JSON, selecteer **meer informatie over de syntaxis van de lijst met ondersteunde** uit te breiden van het dialoogvenster en een voorbeeld van de syntaxis van de toegestane weergeven. Aan het dialoogvenster samenvouwen en verbergen syntaxis, selecteert u de titel van de koppeling opnieuw.

 4. Selecteer **Done**.

    Een voorbeeld van een geldige json op voor een **kleuren** lijst entiteit wordt weergegeven in de volgende JSON-indeling code:

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

## <a name="edit-entity-name"></a>Naam van de entiteit bewerken
1. Op de **entiteiten** pagina, selecteert u de entiteit in de lijst. Deze actie gaat u naar de **entiteit** pagina.

2. Op de **entiteit** pagina u de naam van de entiteit bewerken door het selecteren van het bewerkingspictogram naast de naam van de entiteit. Het entiteitstype kan niet worden bewerkt. 

## <a name="delete-entity"></a>Entiteit verwijderen

Op de **entiteit** weergeeft, schakelt de **entiteit verwijderen** knop. Selecteer **Ok** in het bevestigingsbericht om verwijdering te bevestigen.
 
![Schermafbeelding van de locatie entiteitpagina met entiteit verwijderen knop gemarkeerd](./media/add-entities/entity-delete.png)

>[!NOTE]
>* Een hiërarchische entiteit verwijdert, worden alle onderliggende entiteiten.
>* Het verwijderen van een samengestelde entiteit Hiermee verwijdert u alleen de samengestelde en de samengestelde relatie verbroken, maar de entiteiten die deze niet worden verwijderd.

## <a name="changing-entity-type"></a>Entiteitstype wijzigen
LUIS staat niet toe dat u het type van de entiteit niet wijzigen omdat het niet weet wat u moet toevoegen of verwijderen om te bouwen die entiteit. Als u wilt wijzigen, is het beter om een nieuwe entiteit van het juiste type maken met een iets andere naam. Nadat de entiteit is gemaakt, in elke utterance, verwijdert u de oude naam van de gelabelde entiteit en de naam van de nieuwe entiteit toevoegen. Zodra alle uitingen hebben is relabeled, de oude entiteit niet verwijderen. 

## <a name="create-a-pattern-from-an-utterance"></a>Maken van een patroon van een utterance
Zie [toevoegen patroon uit bestaande utterance op de pagina kunt u lezen wat of entiteit](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="search-utterances"></a>Uitingen zoeken
U kunt zoeken en filteren uitingen met het pictogram met Vergrootglas op de werkbalk. 

## <a name="train-your-app-after-changing-model-with-entities"></a>Uw app trainen na het wijzigen van het model met entiteiten
Na het toevoegen, bewerken of verwijderen van entiteiten, [trainen](luis-how-to-train.md) en [publiceren](luis-how-to-publish-app.md) uw app voor uw wijzigingen zijn doorgevoerd in eindpunt query's. 

## <a name="next-steps"></a>Volgende stappen
Nu dat u intenties, uitingen en entiteiten hebt toegevoegd, hebt u een eenvoudige LUIS-app. Meer informatie over het [trainen](luis-how-to-train.md), [testen](luis-interactive-test.md), en [publiceren](luis-how-to-publish-app.md) uw app.
 
