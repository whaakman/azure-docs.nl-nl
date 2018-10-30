---
title: Switch-instructies in werkstromen - Azure Logic Apps toevoegen | Microsoft Docs
description: Switch-instructies waarmee acties van de werkstroom op basis van specifieke waarden in Azure Logic Apps maken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 10/08/2018
ms.openlocfilehash: 2a3f8ee5cba3110d392555fad78c1cb2513b5d4e
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232440"
---
# <a name="create-switch-statements-that-run-workflow-actions-based-on-specific-values-in-azure-logic-apps"></a>Switch-instructies die worden uitgevoerd op basis van specifieke waarden in Azure Logic Apps werkstroomacties maken

Voor het uitvoeren van specifieke acties op basis van de waarden van objecten, de expressies of tokens, Voeg een *overschakelen* instructie. Deze structuur wordt geëvalueerd als het object, een expressie of een token, kiest de aanvraag die overeenkomt met het resultaat en specifieke acties die alleen voor deze aanvraag wordt uitgevoerd. Wanneer de switch-instructie wordt uitgevoerd, moet het resultaat overeenkomen met slechts één aanvraag.

Stel bijvoorbeeld dat u wilt dat een logische app waarmee verschillende stappen die zijn gebaseerd op een optie hebt geselecteerd in e-mailbericht. In dit voorbeeld controleert de logische app de RSS-feed voor nieuwe inhoud van een website. Wanneer een nieuw item wordt weergegeven in de RSS-feed, wordt in de logische app e-mailbericht verzendt naar een goedkeurder. Afhankelijk van of de fiatteur 'Goedkeuren' of 'Afwijzen' selecteert, volgt de logische app verschillende stappen.

> [!TIP]
> Alle programmeertalen, zoals ondersteuning switch-instructies voor alleen gelijkheid operators. Als u andere relationele operators, zoals 'groter dan', moet gebruiken een [voorwaardelijke statement](../logic-apps/logic-apps-control-flow-conditional-statement.md).
> Om ervoor te zorgen deterministische uitvoeringsgedrag, moeten de gevallen een unieke en statische waarde in plaats van de dynamische-tokens of expressies bevatten.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Het voorbeeld in dit artikel volgen [in dit voorbeeld logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md) met een Outlook.com- of Office 365 Outlook-account.

  1. Wanneer u de actie voor het verzenden van e-mailadres toevoegt, zoeken en in plaats daarvan selecteert u deze actie: **een e-mail voor goedkeuring verzenden**

     ![Selecteer 'Een e-mail met goedkeuring verzenden'](./media/logic-apps-control-flow-switch-statement/send-approval-email-action.png)

  1. Geef de vereiste velden, zoals het e-mailadres voor de persoon die het goedkeurings-e-mailbericht ontvangt. 
  Onder **gebruikersopties**, voer 'Goedkeuren, afwijzen'.

     ![Details van de e-mailadres invoeren](./media/logic-apps-control-flow-switch-statement/send-approval-email-details.png)

## <a name="add-switch-statement"></a>Switch-instructie toevoegen

1. Voegt een switch-instructie aan het einde de voorbeeldwerkstroom in dit voorbeeld. Na de laatste stap kiezen **nieuwe stap**.

   Als u toevoegen van een switch-instructie tussen fasen wilt, gaat u de aanwijzer over de pijl waar u wilt toevoegen van de switch-instructie. Kies de **plusteken** (**+**) die wordt weergegeven, en kies vervolgens **een actie toevoegen**.

1. Typ 'Ga' als filter in het zoekvak. Selecteer deze actie: **overschakelen - controle**

   ![Switch toevoegen](./media/logic-apps-control-flow-switch-statement/add-switch-statement.png)

   Een switch-instructie wordt weergegeven met een aanvraag en een standaard-aanvraag. 
   Een switch-instructie moet standaard ten minste één aanvraag plus de standaard-aanvraag. 

   ![Lege standaard switch-instructie](./media/logic-apps-control-flow-switch-statement/empty-switch.png)

1. Klik in de **op** vak zodat de lijst met dynamische inhoud wordt weergegeven. Uit de lijst, selecteer de **SelectedOption** veld waarvan uitvoer die bepaalt welke actie om uit te voeren. 

   ![Selecteer 'SelectedOption'](./media/logic-apps-control-flow-switch-statement/select-selected-option.png)

1. Voor het afhandelen van de gevallen waarbij de fiatteur selecteert `Approve` of `Reject`, een andere aanvraag tussen toevoegen **geval** en **standaard**. 

   ![Een andere aanvraag toevoegen](./media/logic-apps-control-flow-switch-statement/switch-plus.png)

1. Deze acties toevoegen aan de bijbehorende gevallen:

   | De aanvraag # | **SelectedOption** | Bewerking |
   |--------|--------------------|--------|
   | Voorbeeld 1 | **Goedkeuren** | Toevoegen van de Outlook **een e-mailbericht verzenden** actie voor het verzenden van informatie over het RSS-item alleen wanneer de fiatteur geselecteerd **goedkeuren**. |
   | Geval 2 | **Afwijzen** | Toevoegen van de Outlook **een e-mailbericht verzenden** actie voor het melden van andere goedkeurders dat het RSS-item is afgewezen. |
   | Standaard | Geen | Er is geen actie nodig. In dit voorbeeld wordt de **standaard** aanvraag is leeg omdat **SelectedOption** heeft slechts twee opties. |
   |||

   ![Voltooide switch-instructie](./media/logic-apps-control-flow-switch-statement/finished-switch.png)

1. Sla uw logische app op. 

   Als u wilt testen handmatig in dit voorbeeld, kies **uitvoeren** totdat de logische app zoekt naar een nieuw RSS-item en een e-mail met goedkeuring verzonden. 
   Selecteer **goedkeuren** om te bekijken van de resultaten.

## <a name="json-definition"></a>JSON-definitie

Nu u een logische app met behulp van een switch-instructie hebt gemaakt, gaan we bekijken nu de codedefinitie op hoog niveau achter de switch-instructie.

``` json
"Switch": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions": {
           "Send_an_email": {}
         },
         "case" : "Approve"
      },
      "Case_2": {
         "actions": {
           "Send_an_email_2": {}
         },
         "case": "Reject"
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

| Label | Beschrijving |
|-------|-------------|
| `"Switch"`         | De naam van de switch-instructie, die u kunt de naam voor de leesbaarheid |
| `"type": "Switch"` | Geeft aan dat de actie een switch-instructie |
| `"expression"`     | In dit voorbeeld geeft de fiatteur geselecteerde optie die wordt geëvalueerd op basis van elk afzonderlijk geval als gedeclareerd verderop in het definitie |
| `"cases"` | Hiermee definieert u een willekeurig aantal gevallen. Voor elk geval `"Case_*"` is de standaardnaam voor deze aanvraag, waarmee u kunt de naam voor de leesbaarheid |
| `"case"` | Hiermee geeft u het geval is de waarde moet een constante en unieke waarde die gebruikmaakt van de switch-instructie voor een vergelijking. Als er geen gevallen overeenkomen met het resultaat van de expressie switch, de acties in de `"default"` sectie worden uitgevoerd. | 
| | | 

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u wilt indienen of hierop stemmen op de functies of suggesties, gaat u naar de [site voor gebruikersfeedback van Azure Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Voer stappen uit op basis van een voorwaarde (voorwaardelijke instructies)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Uitvoeren en herhaalt u stap (lussen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Uitvoeren of samenvoegen van parallelle stappen (vertakkingen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Voer stappen uit op basis van gegroepeerde Actiestatus (bereiken)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
