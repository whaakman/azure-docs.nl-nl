---
title: Voeg voorwaardelijke instructies toe aan de werkstromen - Azure Logic Apps | Microsoft Docs
description: Voorwaarden waarmee acties in werkstromen in Azure Logic Apps maken
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: d4e69d33e07f484b4ccc5343786865230368c7ca
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096373"
---
# <a name="create-conditional-statements-that-control-workflow-actions-in-azure-logic-apps"></a>Voorwaardelijke instructies waarmee de werkstroomacties in Azure Logic Apps maken

Toevoegen om bepaalde acties uitvoeren in uw logische app alleen na een opgegeven voorwaarde wordt doorgegeven, een *voorwaardelijke instructie*. Deze structuur vergelijkt de gegevens in uw werkstroom op basis van bepaalde waarden of velden. Vervolgens kunt u verschillende acties die worden uitgevoerd op basis van of de gegevens aan de voorwaarde voldoet. U kunt voorwaarden in de andere nesten.

Stel dat u hebt een logische app waarmee te veel e-mails worden verzonden wanneer nieuwe items worden weergegeven op een website RSS-feed. U kunt een voorwaarde voor het verzenden van e-mailbericht alleen als het nieuwe item een specifieke tekenreeks omvat toevoegen. 

> [!TIP]
> Gebruiken voor het uitvoeren van andere stappen uitvoeren op basis van andere specifieke waarden, een [ *instructie switch* ](../logic-apps/logic-apps-control-flow-switch-statement.md) in plaats daarvan.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Elementaire kennis over [logic apps maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Te volgen in het voorbeeld in dit artikel [in dit voorbeeld logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md) met een Outlook.com of Outlook van Office 365-account.

## <a name="add-a-condition"></a>Een voorwaarde toevoegen

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, opent u uw logische app in Logic App Designer.

2. Een voorwaarde op de locatie die u wilt toevoegen. 

   Als u wilt een voorwaarde tussen stappen toevoegen, de aanwijzer boven de pijl waar u de voorwaarde toevoegen. Kies de **plusteken** (**+**) dat wordt weergegeven, en kies vervolgens **een voorwaarde toevoegen**. Bijvoorbeeld:

   ![Voorwaarde tussen stappen toevoegen](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

   Als u wilt een voorwaarde toevoegen aan het einde van de werkstroom, aan de onderkant van uw logische app, kiest u **+ een nieuwe stap** > **een voorwaarde toevoegen**.

3. Onder **voorwaarde**, de voorwaarde te bouwen. 

   1. Geef de gegevens of het veld dat u wilt vergelijken in het linkerdeelvenster.

      Wanneer u in het vak links klikt, wordt de lijst met dynamische inhoud weergegeven, zodat u uitvoer uit de vorige stappen in uw logische app selecteren kunt. 
      Selecteer de samenvatting RSS-feed voor dit voorbeeld.

      ![De voorwaarde bouwen](./media/logic-apps-control-flow-conditional-statement/edit-condition.png)

   2. Selecteer de bewerking uit te voeren in de middelste lijst. 
   Selecteer voor dit voorbeeld '**bevat**'. 

   3. Geef een waarde of een veld als uw criteria in het vak rechts. 
   Deze tekenreeks opgeven voor dit voorbeeld: **Microsoft**

   Dit is de volledige voorwaarde:

   ![Voltooide voorwaarde](./media/logic-apps-control-flow-conditional-statement/edit-condition-2.png)

5. Onder **als de waarde true** en **indien false**, voeg de stappen uit te voeren op basis van de vraag of de voorwaarde wordt voldaan. Bijvoorbeeld:

   ![Met de voorwaarde 'als de waarde true' en 'Indien false' paden](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > Kunt u bestaande acties in de **als de waarde true** en **indien false** paden.

6. Sla uw logische app op.

Deze logische app alleen verzendt nu e-mail wanneer de nieuwe items in de RSS-feed voldoen aan de voorwaarde.

## <a name="json-definition"></a>JSON-definitie

Nu dat u een logische app met behulp van een voorwaarde hebt gemaakt, bekijken we de codedefinitie op hoog niveau achter het voorwaardelijke-instructie.

``` json
"actions": {
  "Condition": {
    "type": "If",
    "actions": {
      "Send_an_email": {
        "inputs": {},
        "runAfter": {}
    },
    "expression": {
      "and": [ 
        { 
          "contains": [ 
            "@triggerBody()?['summary']", "Microsoft"
          ]
        } 
      ]
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u wilt verzenden of stemmen over de functies en suggesties, gaat u naar de [Azure Logic Apps gebruiker feedback site](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Voer stappen uit op basis van verschillende waarden (switch instructies)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uitvoeren en herhaalt u stap (lussen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Uitvoeren of samenvoegen van parallelle stappen (vertakkingen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Voer stappen uit op basis van Actiestatus van de gegroepeerde (scopes genoemd)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
