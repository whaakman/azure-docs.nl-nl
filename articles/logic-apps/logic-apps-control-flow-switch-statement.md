---
title: Voeg switch-instructies toe aan de werkstromen - Azure Logic Apps | Microsoft Docs
description: Het maken van de switch-instructies die werkstroomacties op basis van specifieke waarden in Azure Logic Apps beheren
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: cfowler
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 2ffac49d14e05ff252d6cd0e90fc23d77ac0caff
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726071"
---
# <a name="create-switch-statements-that-run-workflow-actions-based-on-specific-values-in-azure-logic-apps"></a>Switch-instructies die worden uitgevoerd op basis van specifieke waarden in Azure Logic Apps werkstroomacties maken

Voor het uitvoeren van specifieke acties op basis van de waarden van objecten, expressies of tokens, Voeg een *overschakelen* instructie. Deze structuur evalueert het object, de expressie of het token, kiest de aanvraag die overeenkomt met het resultaat en specifieke acties alleen voor deze aanvraag wordt uitgevoerd. Wanneer de switch-instructie wordt uitgevoerd, hebben slechts één case moet overeenkomen met het resultaat.

Stel bijvoorbeeld dat u wilt dat een logische app waarmee de verschillende stappen op basis van een optie is geselecteerd in e-mailbericht. In dit voorbeeld controleert de logische app een website RSS-feed voor nieuwe inhoud. Wanneer een nieuw item wordt weergegeven in de RSS-feed, verzendt de logische app e-mail naar een goedkeurder. Op basis van of de goedkeurder 'Goedkeuren' of 'Negeren' worden geselecteerd, volgt andere stappen om de logische app.

> [!TIP]
> Net als alle programmeertalen ondersteuning switch instructies alleen gelijkheid operators. Als u andere relationele operators, zoals 'groter dan', gebruiken een [voorwaardelijke instructie](#conditions).
> Zodat deterministische uitvoeringsgedrag bevatten gevallen een uniek en statische waarde in plaats van de dynamische-tokens of expressies.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Te volgen in het voorbeeld in dit artikel [in dit voorbeeld logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md) met een Outlook.com of Outlook van Office 365-account.

  1. Wanneer u de actie voor het verzenden van e-mailadres toevoegen, selecteert u **goedkeuring e-mailbericht verzenden** in plaats daarvan.

     ![Selecteer 'Goedkeuring e-mailbericht verzenden'](./media/logic-apps-control-flow-switch-statement/send-approval-email-action.png)

  2. Geef de vereiste velden, zoals het e-mailadres voor de persoon die de e-mail voor de goedkeuring. 
  Onder **gebruikersopties**, voer 'Goedkeuren, negeren'.

     ![Voer details voor e-mail](./media/logic-apps-control-flow-switch-statement/send-approval-email-details.png)

## <a name="add-a-switch-statement"></a>Toevoegen van een switchinstructie

1. Aan het einde van de voorbeeldwerkstroom, kies **+ een nieuwe stap** > **... Meer** > **toevoegen van een case**. 

   ![Toevoegen van een switchinstructie](./media/logic-apps-control-flow-switch-statement/add-switch-statement.png)

   Er verschijnt een switch-instructie met één case en een standaard-case. 
   Switch-instructie vereist ten minste één case plus de standaard-case. 

   Als u toevoegen van een switchinstructie tussen stappen wilt, de aanwijzer boven de pijl waar u wilt toevoegen van de switch-instructie. 
   Kies de **plusteken** (**+**) dat wordt weergegeven, en kies vervolgens **toevoegen van een case**.

4. In de **op** de optie de **SelectedOption** veld waarvan de uitvoer bepaalt de bewerking uit te voeren. 
   
   Kunt u het veld van de **dynamische inhoud toevoegen** lijst die wordt weergegeven.

5. Voor het afhandelen van de gevallen waarbij de goedkeurder selecteert `Approve` of `Reject`, een andere aanvraag tussen toevoegen **geval** en **standaard**. 
   
6. Deze acties toevoegen aan de bijbehorende gevallen:

   | Case # | **SelectedOption** | Bewerking |
   |:------ |:-------------------|:------ |
   | Voorbeeld 1 | **Goedkeuren** | Toevoegen van de Outlook **e-mailbericht verzenden** actie voor het verzenden van informatie over het RSS-item alleen wanneer de goedkeurder geselecteerd **goedkeuren**. |
   | Voorbeeld 2 | **Afwijzen** | Toevoegen van de Outlook **e-mailbericht verzenden** actie voor het verwittigen van andere goedkeurders dat het RSS-item is afgewezen. |
   | Standaard | \<Geen\> | Er is geen actie nodig. In dit voorbeeld wordt de **standaard** aanvraag is leeg omdat **SelectedOption** heeft slechts twee opties. |
   |         |          |

   ![Switch-instructie](./media/logic-apps-control-flow-switch-statement/switch.png)

7. Sla uw logische app op. 

   Als u handmatig dit voorbeeld kiest **uitvoeren** totdat de logische app vindt een nieuw RSS-item en een goedkeuring e-mailbericht verzendt. 
   Selecteer **goedkeuren** om te zien van de resultaten.

## <a name="json-definition"></a>JSON-definitie

Nu dat u een logische app met een switchinstructie hebt gemaakt, bekijken we de codedefinitie op hoog niveau achter de switch-instructie.

``` json
"Switch": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case" : {
         "actions" : {
           "Send_an_email": { }
         },
         "case" : "Approve"
      },
      "Case_2" : {
         "actions" : {
           "Send_an_email_2": { }
         },
         "case" : "Reject"
      }
   },
   "default": {
      "actions": {}
   },
   "runAfter": {
      "Send_approval_email": [
         "Succeeded"
      ]
   }
}
```

| Label              | Beschrijving |
| :----------------- | :---------- |
| `"Switch"`         | De naam van de switch-instructie, waarmee u kunt de naam voor de leesbaarheid |
| `"type": "Switch"` | Geeft aan dat de actie een switch-instructie |
| `"expression"`     | Hiermee wordt de goedkeurder geselecteerde optie, die wordt geëvalueerd op basis van elk afzonderlijk geval als u later in de definitie is gedeclareerd in dit voorbeeld |
| `"cases"` | Hiermee definieert u een willekeurig aantal cases. Voor elk geval `"Case_*"` is de standaardnaam voor deze aanvraag die u kunt de naam voor de leesbaarheid |
| `"case"` | Hiermee geeft u de aanvraag-waarde moet een constante en unieke waarde die gebruikmaakt van de switch-instructie voor vergelijking. Als geen cases overeenkomen met het resultaat van de expressie switch, de acties in de `"default"` sectie worden uitgevoerd.
|           |         |

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u wilt verzenden of stemmen over functies of suggesties, gaat u naar de [Azure Logic Apps gebruiker feedback site](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Voer stappen uit op basis van een voorwaarde (voorwaardelijke instructies)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Uitvoeren en herhaalt u stap (lussen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Uitvoeren of samenvoegen van parallelle stappen (vertakkingen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Voer stappen uit op basis van Actiestatus van de gegroepeerde (scopes genoemd)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
