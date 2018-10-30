---
title: Voeg voorwaardelijke instructies toe aan de werkstromen - Azure Logic Apps | Microsoft Docs
description: Voorwaarden waarmee acties in werkstromen in Azure Logic Apps maken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 10/09/2018
ms.openlocfilehash: 9ee484971e217b0ca4dd7ad855e9e6dc3313e5d4
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230332"
---
# <a name="create-conditional-statements-that-control-workflow-actions-in-azure-logic-apps"></a>Voorwaardelijke instructies waarmee werkstroomacties in Azure Logic Apps maken

Voor het uitvoeren van specifieke acties in uw logische app alleen na een opgegeven voorwaarde wordt doorgegeven, Voeg een *voorwaardelijke statement*. Deze structuur besturingselement vergelijkt de gegevens in uw werkstroom op basis van specifieke waarden of de velden. Vervolgens kunt u verschillende acties die worden uitgevoerd op basis van op of de gegevens aan de voorwaarde voldoet. U kunt voorwaarden in de andere nesten.

Stel bijvoorbeeld dat u hebt een logische app die te veel e-mailberichten verzendt wanneer nieuwe items worden weergegeven op RSS-feed van een website. U kunt een voorwaardelijke statement om e-mail te verzenden wanneer een nieuw item een specifieke tekenreeks bevat toevoegen. 

> [!TIP]
> Als u wilt uitvoeren op basis van verschillende waarden voor de specifieke stappen, gebruikt u een [ *instructie switch* ](../logic-apps/logic-apps-control-flow-switch-statement.md) in plaats daarvan.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Het voorbeeld in dit artikel volgen [in dit voorbeeld logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md) met een Outlook.com- of Office 365 Outlook-account.

## <a name="add-condition"></a>Voorwaarde toevoegen

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, opent u uw logische app in Logic App Designer.

1. Een voorwaarde op de locatie die u wilt toevoegen. 

   Om toe te voegen een voorwaarde tussen fasen, gaat u de aanwijzer over de pijl waar u wilt toevoegen van de voorwaarde. Kies de **plusteken** (**+**) die wordt weergegeven, en kies vervolgens **een actie toevoegen**. Bijvoorbeeld:

   ![Actie tussen fasen toevoegen](./media/logic-apps-control-flow-conditional-statement/add-action.png)

   Als u wilt een voorwaarde toevoegen aan het einde van uw werkstroom, aan de onderkant van uw logische app, kies **nieuwe stap** > **een actie toevoegen**.

1. Typ 'voorwaarde' als filter in het zoekvak. Selecteer deze actie: **voorwaarde - controle**

   ![Voorwaarde toevoegen](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

1. In de **voorwaarde** vak, het bouwen van uw voorwaarde. 

   1. Geef de gegevens of een veld dat u wilt vergelijken in het linkerdeelvenster.

      Wanneer u in het vak links klikt, wordt de lijst met dynamische inhoud weergegeven zodat u uitvoer uit de vorige stappen in uw logische app selecteren kunt. 
      Selecteer voor dit voorbeeld de samenvatting van de RSS-feed.

      ![Maak uw voorwaarde](./media/logic-apps-control-flow-conditional-statement/edit-condition.png)

   1. Selecteer de bewerking uit te voeren in het middelste vak. 
   Selecteer voor dit voorbeeld '**bevat**'. 

   1. Geef een waarde of een veld als uw criteria in het vak rechts. 
   Geef deze tekenreeks op voor dit voorbeeld: **Microsoft**

   Dit is de volledige voorwaarde:

   ![Voltooide voorwaarde](./media/logic-apps-control-flow-conditional-statement/edit-condition-2.png)

   Als u wilt een nieuwe rij toevoegt aan de voorwaarde, kiest u **toevoegen** > **rij toevoegen**. 
   Als u wilt toevoegen van een groep met subconditions, kies **toevoegen** > **groep toevoegen**. 
   Als u wilt groeperen bestaande rijen, schakel de selectievakjes in voor de rijen, kiest u de knop met weglatingstekens (...) voor elke rij en kies vervolgens **maken groep**.

1. Onder **als de waarde true** en **indien onwaar**, voeg de stappen om uit te voeren op basis van op of de voorwaarde wordt voldaan. Bijvoorbeeld:

   ![Voorwaarde met 'indien waar' en 'indien onwaar' paden](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > Kunt u bestaande acties in de **als de waarde true** en **indien onwaar** paden.

1. Sla uw logische app op.

Deze logische app wordt nu een e-mail verzendt alleen wanneer de nieuwe items in de RSS-feed voldoen aan de voorwaarde.

## <a name="json-definition"></a>JSON-definitie

Hier volgt de codedefinitie op hoog niveau achter een voorwaardelijke statement:

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
            "@triggerBody()?['summary']", 
            "Microsoft"
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
* Als u wilt indienen of hierop stemmen op de functies en suggesties, gaat u naar de [site voor gebruikersfeedback van Azure Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Voer stappen uit op basis van verschillende waarden (switch-instructies)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uitvoeren en herhaalt u stap (lussen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Uitvoeren of samenvoegen van parallelle stappen (vertakkingen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Voer stappen uit op basis van gegroepeerde Actiestatus (bereiken)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
