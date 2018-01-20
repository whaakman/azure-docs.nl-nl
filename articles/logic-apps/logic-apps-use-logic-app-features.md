---
title: Voorwaarden toevoegen en starten van werkstromen - Azure Logic Apps | Microsoft Docs
description: Bepalen hoe werkstromen worden uitgevoerd in Azure Logic Apps door voorwaardelijke logica, triggers, acties en parameters toe te voegen.
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: 5a3957ffcc149bdaf5c196960c7c7f23913f8b5c
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="use-logic-apps-features"></a>Functies van logische Apps gebruiken

In een [vorige onderwerp](../logic-apps/quickstart-create-first-logic-app-workflow.md), u hebt uw eerste logische app hebt gemaakt. Als u uw logische app werkstroom wilt aanpassen, kunt u verschillende paden voor uw logische app om uit te voeren en het verwerken van gegevens in matrices, verzamelingen en batches. U kunt deze elementen opnemen in uw logische app werkstroom:

* Voorwaarden en [overschakelen instructies](../logic-apps/logic-apps-switch-case.md) kunt u uw logische app uitgevoerd verschillende acties op basis van of bepaalde voorwaarden wordt voldaan.

* [Lussen](../logic-apps/logic-apps-loops-and-scopes.md) kunt u uw logische app herhaaldelijk stappen uitvoeren. Bijvoorbeeld, u kunt acties herhalen via een matrix wanneer u een **For_each** lus. Of u kunt acties herhalen totdat een voorwaarde wordt voldaan, wanneer u een **totdat** lus.

* [Scopes](../logic-apps/logic-apps-loops-and-scopes.md) laat u groeperen reeks acties, bijvoorbeeld voor het implementeren van afhandeling van uitzonderingen.

* [Debatching](../logic-apps/logic-apps-loops-and-scopes.md) kunt u uw logische app afzonderlijke werkstromen voor items in een matrix start wanneer u de **SplitOn** opdracht.

Dit onderwerp biedt informatie over andere concepten voor het bouwen van uw logische app:

* Weergave van de code voor het bewerken van een bestaande logische app
* Opties voor het starten van een werkstroom

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a>Voorwaarden: Stappen uitvoeren na het voldoen aan een voorwaarde

Als u uw logische app stappen alleen uitgevoerd als gegevens aan bepaalde criteria voldoet, kunt u een voorwaarde waarmee wordt vergeleken gegevens in de werkstroom op basis van specifieke velden of waarden toevoegen.

Stel dat u hebt een logische app die u op een website RSS-feed te veel e-mails voor berichten verzendt. U kunt een voorwaarde toevoegen zodat uw logische app stuurt een e-mailbericht alleen als het nieuwe bericht tot een specifieke categorie behoort.

1. In de [Azure-portal](https://portal.azure.com), zoeken en openen van uw logische app in Logic App-ontwerper.

2. Een voorwaarde voor de locatie van de werkstroom die u wilt toevoegen. 

   Als u wilt de voorwaarde tussen bestaande stappen in de logic app-werkstroom toevoegen, de aanwijzer boven de pijl waar u de voorwaarde toevoegen. 
   Kies de **plusteken** (**+**), en kies vervolgens **een voorwaarde toevoegen**. Bijvoorbeeld:

   ![Voorwaarde toevoegen aan logische app](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > Als u een voorwaarde toevoegen aan het einde van uw huidige werkstroom wilt, Ga naar de onderkant van uw logische app en selecteer **+ een nieuwe stap**.

3. Nu de voorwaarde wordt gedefinieerd. Het veld voor de bron die u wilt evalueren, de bewerking uit te voeren en de doelwaarde of veld opgeven. Om de bestaande velden toevoegen aan de voorwaarde, kiezen uit de **dynamische inhoud lijst toevoegen**.

   Bijvoorbeeld:

   ![Voorwaarde in de standaardmodus bewerken](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   Dit is de volledige voorwaarde:

   ![Volledige voorwaarde](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > Als u wilt de voorwaarde wordt gedefinieerd in de code, kies **bewerken in de geavanceerde modus**. Bijvoorbeeld:
   > 
   > ![Voorwaarde in de code bewerken](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. Onder **als Ja** en **als Nee**, voeg de stappen uit te voeren op basis van de vraag of de voorwaarde wordt voldaan.

   Bijvoorbeeld:

   ![Voorwaarde van Ja en er worden geen paden](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > Kunt u bestaande acties in de **als Ja** en **als Nee** paden.

5. Wanneer u bent klaar, slaat u uw logische app.

U krijgt nu e-mailberichten alleen wanneer de berichten aan de voorwaarde voldoen.

## <a name="repeat-actions-over-a-list-with-foreach"></a>Acties over een lijst met forEach herhalen

De lus forEach geeft een matrix als u wilt een via herhalen. Als dit niet een matrix is, mislukt de stroom. Als u action1 die een matrix van berichten hebt en u wilt dat elk bericht te verzenden, kunt u deze instructie forEach opnemen in de eigenschappen van de actie:`forEach : "@action('action1').outputs.messages"`

## <a name="edit-the-code-definition-for-a-logic-app"></a>De definitie van de code voor een logische app bewerken

Hoewel u de ontwerpfunctie voor Logic App hebt, kunt u de code die een logische app bepaalt rechtstreeks bewerken.

1. Kies op de opdrachtbalk **codeweergave**.

    Een volledige-editor wordt geopend en toont de definitie die u bewerkt.

    ![Codeweergave](media/logic-apps-use-logic-app-features/codeview.png)

    U kunt kopiëren en plakken van een willekeurig aantal acties binnen dezelfde logic app of tussen logische apps in de teksteditor. 
    U kunt ook eenvoudig toevoegen of verwijderen van volledige secties van de definitie en u kunt ook definities met anderen delen.

2. Om de wijzigingen opslaan, kies **opslaan**.

## <a name="parameters"></a>Parameters

Enkele mogelijkheden van Logic Apps zijn alleen beschikbaar in de codeweergave, bijvoorbeeld parameters. Parameters kunnen u eenvoudig waarden in uw logische app opnieuw gebruiken. Als u een e-mailadres die u gebruiken in verschillende acties wilt hebt, moet u dat e-mailadres definiëren als een parameter.

Parameters zijn goed voor het ophalen van waarden die u waarschijnlijk veel worden gewijzigd. Ze zijn vooral handig als u nodig hebt voor het onderdrukken van parameters in verschillende omgevingen. Zie voor meer informatie over parameters die zijn gebaseerd op de omgeving overschrijven, [logic app-definities auteur](../logic-apps/logic-apps-author-definitions.md) en [REST API-documentatie](https://docs.microsoft.com/rest/api/logic).

In dit voorbeeld laat zien hoe uw bestaande logische app bijwerken zodat u parameters voor de zoekterm gebruiken kunt.

1. Zoeken in de codeweergave de `parameters : {}` object en voeg een `currentFeedUrl` object:

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. Ga naar de `When_a_feed-item_is_published` actie, vinden de `queries` sectie en vervang de querywaarde met:`"feedUrl": "#@{parameters('currentFeedUrl')}"` 

    Als u wilt deelnemen aan twee of meer tekenreeksen, u kunt ook de `concat` functie. 
    Bijvoorbeeld: `"@concat('#',parameters('currentFeedUrl'))"` werkt hetzelfde als de bovenstaande.

3.  Als u bent klaar, kiest u **opslaan**. 

    Nu kunt u de website RSS-feed door een andere URL via de `currentFeedURL` object.

Meer informatie over [hoe auteur logic app-definities](../logic-apps/logic-apps-author-definitions.md).

## <a name="start-logic-app-workflows"></a>Logic app-werkstromen starten

U hebt verschillende opties voor het starten van de werkstroom die is gedefinieerd in uw logische app. U kunt altijd een werkstroom op verzoek starten in de [Azure-portal].

### <a name="recurrence-triggers"></a>Terugkeerpatroon triggers

Een terugkeerpatroon trigger wordt uitgevoerd met een interval dat u opgeeft. Wanneer de trigger voorwaardelijke logica heeft, bepaalt de trigger of de werkstroom moet worden uitgevoerd. Een trigger geeft aan de werkstroom moet worden uitgevoerd door te retourneren een `200` statuscode. Wanneer de werkstroom niet hoeft te worden uitgevoerd, de trigger retourneert een `202` statuscode.

### <a name="callback-using-rest-apis"></a>Callback met REST API 's

Voor het starten van een werkstroom kunnen services bellen en een logische app-eindpunt. Als u wilt dit soort logic app op de aanvraag, kies **nu uitvoeren** op de opdrachtbalk. Zie [werkstromen starten door het aanroepen van logic app eindpunten als triggers](../logic-apps/logic-apps-http-endpoint.md). 

<!-- Shared links -->
[Azure-portal]: https://portal.azure.com

## <a name="next-steps"></a>Volgende stappen

* [Switch-instructies](../logic-apps/logic-apps-switch-case.md) 
* [Lussen, bereiken en debatching](../logic-apps/logic-apps-loops-and-scopes.md)
* [Definities voor logische apps maken](../logic-apps/logic-apps-author-definitions.md)