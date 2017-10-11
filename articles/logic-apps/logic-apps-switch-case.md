---
title: Switch-instructie voor verschillende acties in Azure Logic Apps | Microsoft Docs
description: Kies verschillende acties uitvoeren in logic apps op basis van Expressiewaarden met behulp van een switch-instructie
services: logic-apps
keywords: Switch-instructie
author: derek1ee
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: 338b6a5b549d7bf81186550295608438ac4aee32
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a>Verschillende acties worden uitgevoerd in logic apps met een switch-instructie

Bij het ontwerpen van een werkstroom, moet u vaak andere acties onderneemt op basis van de waarde van een object of een expressie. Bijvoorbeeld, kunt u uw logische app zich gedragen anders op basis van de statuscode van een HTTP-aanvraag, of een optie in een e-mailbericht hebt geselecteerd.

U kunt een switch-instructie voor het implementeren van deze scenario's. Uw logische app kunt evalueren van een token of een expressie en kies het geval is bij dezelfde waarde binnen de opgegeven acties uitvoeren. Slechts één case moet overeenkomen met de switch-instructie.

> [!TIP]
> Net als alle programmeertalen ondersteuning switch instructies alleen gelijkheid operators. Als u andere relationele operators, zoals 'groter dan', gebruikt u een voorwaardeninstructie.
> Om ervoor te zorgen deterministische uitvoeringsgedrag, moeten gevallen een uniek en statische waarde in plaats van de dynamische-tokens of expressie bevatten.

## <a name="prerequisites"></a>Vereisten

- Een actief Azure-abonnement. Als u een actief Azure-abonnement geen [een gratis account maken](https://azure.microsoft.com/free/), of probeer [Logic Apps voor gratis](https://tryappservice.azure.com/).
- [Elementaire kennis over logic apps](logic-apps-what-are-logic-apps.md)

## <a name="add-a-switch-statement-to-your-workflow"></a>Een switchinstructie toevoegen aan uw werkstroom

Als u wilt weergeven op de werking van een switch-instructie, wordt in dit voorbeeld een logische app dat bestanden geüpload naar Dropbox bewaakt. Wanneer de nieuwe bestanden zijn geüpload, verzendt de logische app e-mail naar een goedkeurder die u kiest of moet worden overgedragen van die bestanden naar SharePoint. De app gebruikmaakt van een switch-instructie waarmee u verschillende acties die zijn gebaseerd op de waarde die de goedkeurder selecteert.

1. Een logische app maken en selecteer deze trigger: **Dropbox - wanneer een bestand wordt gemaakt**.

   ![Dropbox - gebruiken wanneer een bestand trigger wordt gemaakt](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. Deze actie toevoegen onder de trigger: **Outlook.com - e-mailbericht verzenden-goedkeuring**

   > [!TIP]
   > Logische apps ondersteunen ook verzenden goedkeuring e-scenario's uit een Outlook van Office 365-account.

   - Als u een bestaande verbinding geen hebt, wordt u gevraagd een maken.
   - Vul de vereiste velden in. Bijvoorbeeld onder **naar**, geef het e-mailadres voor het verzenden van het e-mailbericht goedkeurder.
   - Onder **gebruikersopties**, voer `Approve, Reject`.

   ![Configureer de verbinding](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. Toevoegen van een switchinstructie.

   - Selecteer **+ een nieuwe stap** > **... Meer** > **toevoegen van een case**. 
   - Nu we willen selecteert u de actie om uit te voeren op basis van de `SelectedOptions` de uitvoer van de *goedkeuring per E-mail* in te grijpen. 
   U vindt dit veld in de **dynamische inhoud toevoegen** selector.
   - Gebruik *geval 1* om af te handelen wanneer de goedkeurder selecteert `Approve`.
     - Als goedgekeurd, kopieert u het oorspronkelijke bestand tot SharePoint Online met de [ **SharePoint Online - bestand maken** actie](../connectors/connectors-create-api-sharepointonline.md).
     - Voeg een andere actie binnen de aanvraag om gebruikers te waarschuwen dat een nieuw bestand beschikbaar in SharePoint is.
   - Een andere aanvraag om af te handelen wanneer gebruiker selecteert toevoegen `Reject`.
     - Als afgewezen, verzendt u een e-mailmelding andere goedkeurders wordt geïnformeerd dat het bestand wordt geweigerd en geen verdere actie vereist is.
   - `SelectedOptions`biedt slechts twee opties, zodat we kunt laten de **standaard** geval leeg.

   ![Switch-instructie](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > Switch-instructie moet ten minste één case naast de standaard-case.

4. Het oorspronkelijke bestand geüpload naar Dropbox door deze actie toe te voegen na de instructie switch worden verwijderd: **Dropbox - bestand verwijderen**

5. Sla uw logische app. Uw app testen door een bestand uploadt naar Dropbox. U ontvangt binnenkort een e-mailbericht goedkeuring. Selecteer een optie en het gedrag observeren.

   > [!TIP]
   > Bekijk hoe [uw logische apps bewaken](logic-apps-monitor-your-logic-apps.md).

## <a name="understand-the-code-behind-switch-statements"></a>Inzicht in de code achter de switch-instructies

Nu dat u een logische app met een switchinstructie is gemaakt, bekijken we de definitie van de code achter de switch-instructie.

```json
"Switch": {
    "type": "Switch",
    "expression": "@body('Send_approval_email')?['SelectedOption']",
    "cases": {
        "Case 1" : {
            "case" : "Approved",
            "actions" : {}
        },
        "Case 2" : {
            "case" : "Rejected",
            "actions" : {}
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

* `"Switch"`is de naam van de switch-instructie, waarmee u kunt de naam voor de leesbaarheid. 
* `"type": "Switch"`Hiermee wordt aangegeven dat de actie een switch-instructie is. 
* `"expression"`de goedkeurder geselecteerde optie in dit voorbeeld is en wordt geëvalueerd op basis van elk geval later in de definitie is gedeclareerd. 
* `"cases"`kan een onbeperkt aantal gevallen bevatten. Voor elk geval `"Case *"` is de standaardnaam van de aanvraag die u kunt de naam voor de leesbaarheid. 
`"case"`Hiermee geeft u de case-label, waarbij de switch-expressie wordt gebruikt voor vergelijking, en moet een constante en unieke waarde. Als geen van de gevallen overeenkomt met de schakeloptie-expressie, acties onder `"default"` worden uitgevoerd.

## <a name="get-help"></a>Help opvragen

Ga naar het [forum voor Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) om vragen te stellen, vragen te beantwoorden en te zien wat andere gebruikers van Azure Logic Apps aan het doen zijn.

Ter verbetering van Azure Logic Apps en connectors kunt u stemmen op ideeën of ideeën indien op de [site voor gebruikersfeedback van Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [voorwaarden toevoegen](logic-apps-use-logic-app-features.md)
- Meer informatie over [fout en uitzonderingsverwerking](logic-apps-exception-handling.md)
- Ontdek meer [werkstroom taal mogelijkheden](logic-apps-author-definitions.md)