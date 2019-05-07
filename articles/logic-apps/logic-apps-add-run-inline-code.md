---
title: Toevoegen en uitvoeren van codefragmenten - Azure Logic Apps
description: Toevoegen en uitvoeren van codefragmenten inline code in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: derek1ee, LADocs
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: 9ef11eb2099ff617fb4da4b9a924dc3f0550f226
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160543"
---
# <a name="add-and-run-code-snippets-by-using-inline-code-in-azure-logic-apps"></a>Toevoegen en codefragmenten uitvoeren met behulp van inline-code in Azure Logic Apps

Als u een stukje code binnen uw logische app uitvoeren wilt, kunt u de ingebouwde toevoegen **Inline Code** actie als wanneer u een stap in uw logic app-werkstroom. Deze actie werkt het beste als u wilt uitvoeren van code die geschikt is voor dit scenario:

* Wordt uitgevoerd in JavaScript. Meer talen is binnenkort beschikbaar.
* Is voltooid in de vijf seconden of minder uitgevoerd.
* Gegevens verwerkt in de grootte van maximaal 50 MB.
* Maakt gebruik van Node.js versie 8.11.1. Zie voor meer informatie, [standaard ingebouwde objecten](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

Met deze actie wordt het volgende codefragment wordt uitgevoerd en wordt de uitvoer van dit fragment geretourneerd als een token met de naam **resultaat**, die u kunt gebruiken in de volgende acties in uw logische app. Voor andere scenario's waarin u wilt maken van een functie voor uw code, kunt u proberen [maken en een Azure-functie aan te roepen](../logic-apps/logic-apps-azure-functions.md) in uw logische app.

In dit artikel wordt het voorbeeld logische app wordt geactiveerd wanneer een nieuwe e-mail binnenkomt in een Office 365 Outlook-account. Het codefragment worden uitgepakt en retourneert geen e-mailadressen die worden weergegeven in de hoofdtekst van de e-mail.

![Voorbeeld-overzicht](./media/logic-apps-add-run-inline-code/inline-code-example-overview.png)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* De logische app waar u het codefragment, met inbegrip van een trigger toevoegen. Als u een logische app niet hebt, raadpleegt u [Quick Start: Maak uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

   Het voorbeeld van logische app in dit onderwerp maakt gebruik van deze Office 365 Outlook-trigger: **Wanneer een nieuwe e-mail binnenkomt**

* Een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) dat gekoppeld aan uw logische app

## <a name="add-inline-code"></a>Inlinecode toevoegen

1. Als u dat nog niet gedaan hebt, in de [Azure-portal](https://portal.azure.com), opent u uw logische app in Logic App Designer.

1. Voeg in de ontwerpfunctie voor de **Inline Code** actie op de locatie die u in uw logic app-werkstroom wilt.

   * Als u wilt toevoegen de actie aan het einde van de werkstroom, kies **nieuwe stap**.

   * Als u wilt toevoegen de actie tussen bestaande stappen, de muisaanwijzer boven de pijl die verbinding deze stappen maakt. Kies het plusteken (**+**), en selecteer **een actie toevoegen**.

   In dit voorbeeld wordt de **Inline Code** actie bij de Office 365 Outlook-trigger.

   ![Nieuwe stap toevoegen](./media/logic-apps-add-run-inline-code/add-new-step.png)

1. Onder **een actie kiezen**, typ in het zoekvak 'Inlinecode' als filter. Selecteer deze actie uit de lijst met acties: **JavaScript-Code uitvoeren**

   ![Selecteer "JavaScript-Code uitvoeren"](./media/logic-apps-add-run-inline-code/select-inline-code-action.png)

   De actie wordt weergegeven in de ontwerpfunctie en sommige standaard-voorbeeldcode, met inbegrip van een instructie return bevat.

   ![Inline Code actie met de standaard-voorbeeldcode](./media/logic-apps-add-run-inline-code/inline-code-action-default.png)

1. In de **Code** vak, verwijderen van de voorbeeldcode en voer de code in die u wilt uitvoeren. Schrijf code die u plaatst binnen een methode, maar zonder de methodehandtekening definiëren. 

   Wanneer u een herkende trefwoord typt, wordt de lijst met automatisch aanvullen weergegeven, zodat u in beschikbare trefwoorden, bijvoorbeeld selecteren kunt:

   ![Lijst met trefwoorden automatisch aanvullen](./media/logic-apps-add-run-inline-code/auto-complete.png)

   Dit codefragment voorbeeld maakt u eerst een variabele waarin een *reguliere expressie*, dat aangeeft dat een patroon voor de overeenkomst in de invoertekst. De code maakt vervolgens een variabele waarin de gegevens van de hoofdtekst van de e-mailadres van de trigger.

   ![Variabelen maken](./media/logic-apps-add-run-inline-code/save-email-body-variable.png)

   De resultaten van de trigger en vorige acties om eenvoudiger te maken om te verwijzen naar, wordt de lijst met dynamische inhoud weergegeven terwijl de cursor zich binnen de **Code** vak. In dit voorbeeld ziet u de lijst met beschikbare resultaten van de trigger met inbegrip van de **hoofdtekst** token, dat u kunt nu selecteren.

   Nadat u hebt geselecteerd de **hoofdtekst** token, de actie van de code inline wordt omgezet het token voor een `workflowContext` -object dat verwijst naar het e-mailbericht `Body` waarde van eigenschap:

   ![Selecteer resultaat](./media/logic-apps-add-run-inline-code/inline-code-example-select-outputs.png)

   In de **Code** vak het codefragment kunt gebruiken voor de alleen-lezen `workflowContext` object als invoer. Dit object heeft subeigenschappen waarmee uw code-toegang tot de resultaten van de trigger en vorige acties in uw werkstroom.
   Zie voor meer informatie in deze sectie verderop in dit onderwerp: [Trigger en actie resultaten in uw code verwijzen naar](#workflowcontext).

   > [!NOTE]
   >
   > Als uw codefragment verwijst naar de actienamen van de die gebruikmaken van de punt (.), moet u deze actienamen toevoegen de [ **acties** parameter](#add-parameters). Deze verwijzingen moeten ook worden omsloten de actienamen van de met vierkante haken ([]) en tussen aanhalingstekens, bijvoorbeeld:
   >
   > `// Correct`</br> 
   > `workflowContext.actions["my.action.name"].body`</br>
   >
   > `// Incorrect`</br>
   > `workflowContext.actions.my.action.name.body`

   De actie van de code inline geen vereist een `return` -instructie, maar de resultaten van een `return` overzicht zijn beschikbaar voor verwijzing bij latere acties via de **resultaat** token. 
   Het codefragment retourneert het resultaat bijvoorbeeld door het aanroepen van de `match()` , welke vindt u in de hoofdtekst van de e-mail met de reguliere expressie komt overeen met de functie. De **opstellen** actie wordt de **resultaat** token om te verwijzen naar de resultaten van de inline code actie en maakt u een enkelvoudig resultaat wordt verkregen.

   ![Voltooide logische app](./media/logic-apps-add-run-inline-code/inline-code-complete-example.png)

1. Wanneer u klaar bent, kunt u uw logische app opslaan.

<a name="workflowcontext"></a>

### <a name="reference-trigger-and-action-results-in-your-code"></a>Referentie-trigger en actie resultaten in uw code

De `workflowContext` object heeft deze structuur, waaronder de `actions`, `trigger`, en `workflow` subeigenschappen:

```json
{
   "workflowContext": {
      "actions": {
         "<action-name-1>": @actions('<action-name-1>'),
         "<action-name-2>": @actions('<action-name-2>')
      },
      "trigger": {
         @trigger()
      },
      "workflow": {
         @workflow()
      }
   }
}
```

Deze tabel bevat meer informatie over deze subeigenschappen:

| Eigenschap | Type | Description |
|----------|------|-------|
| `actions` | Objectverzameling | Hierdoor worden objecten van de acties die worden uitgevoerd voordat het codefragment wordt uitgevoerd. Elk object heeft een *sleutel / waarde-* waar de sleutel is de naam van een actie en de waarde is gelijk aan aanroepen de [actions() functie](../logic-apps/workflow-definition-language-functions-reference.md#actions) met `@actions('<action-name>')`. De naam van de actie maakt gebruik van dezelfde als de actienaam die wordt gebruikt in de onderliggende werkstroomdefinitie die wordt vervangen door spaties ("") in de actienaam van de met onderstrepingstekens (_). Dit object biedt toegang tot actie eigenschapswaarden van de huidige werkstroomexemplaar uitvoeren. |
| `trigger` | Object | Resultaatobject aan het aanroepen van de trigger en equivalent de [trigger() functie](../logic-apps/workflow-definition-language-functions-reference.md#trigger). Dit object biedt toegang tot de waarden van de trigger-eigenschappen van het huidige werkstroomexemplaar uitvoeren. |
| `workflow` | Object | De werkstroomobject en komt overeen met het aanroepen de [workflow ()-functie](../logic-apps/workflow-definition-language-functions-reference.md#workflow). Dit object biedt toegang tot eigenschapswaarden van de werkstroom, zoals de naam van de werkstroom, run-ID, enzovoort, van het huidige werkstroomexemplaar uitvoeren. |
|||

In voorbeeld van dit onderwerp wordt de `workflowContext` object heeft deze eigenschappen die toegang hebben tot uw code:

```json
{
   "workflowContext": {
      "trigger": {
         "name": "When_a_new_email_arrives",
         "inputs": {
            "host": {
               "connection": {
                  "name": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/connections/office365"
               }
            },
            "method": "get",
            "path": "/Mail/OnNewEmail",
            "queries": {
               "includeAttachments": "False"
            }
         },
         "outputs": {
            "headers": {
               "Pragma": "no-cache",
               "Content-Type": "application/json; charset=utf-8",
               "Expires": "-1",
               "Content-Length": "962095"
            },
            "body": {
               "Id": "AAMkADY0NGZhNjdhLTRmZTQtNGFhOC1iYjFlLTk0MjZlZjczMWRhNgBGAAAAAABmZwxUQtCGTqSPpjjMQeD",
               "DateTimeReceived": "2019-03-28T19:42:16+00:00",
               "HasAttachment": false,
               "Subject": "Hello World",
               "BodyPreview": "Hello World",
               "Importance": 1,
               "ConversationId": "AAQkADY0NGZhNjdhLTRmZTQtNGFhOC1iYjFlLTk0MjZlZjczMWRhNgAQ",
               "IsRead": false,
               "IsHtml": true,
               "Body": "Hello World",
               "From": "<sender>@<domain>.com",
               "To": "<recipient-2>@<domain>.com;<recipient-2>@<domain>.com",
               "Cc": null,
               "Bcc": null,
               "Attachments": []
            }
         },
         "startTime": "2019-05-03T14:30:45.971564Z",
         "endTime": "2019-05-03T14:30:50.1746874Z",
         "scheduledTime": "2019-05-03T14:30:45.8778117Z",
         "trackingId": "1cd5ffbd-f989-4df5-a96a-6e9ce31d03c5",
         "clientTrackingId": "08586447130394969981639729333CU06",
         "originHistoryName": "08586447130394969981639729333CU06",
         "code": "OK",
         "status": "Succeeded"
      },
      "workflow": {
         "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<logic-app-workflow-name>",
         "name": "<logic-app-workflow-name>",
         "type": "Microsoft.Logic/workflows",
         "location": "<Azure-region>",
         "run": {
            "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<logic-app-workflow-name>/runs/08586453954668694173655267965CU00",
            "name": "08586453954668694173655267965CU00",
            "type": "Microsoft.Logic/workflows/runs"
         }
      }
   }
}
```

<a name="add-parameters"></a>

## <a name="add-parameters"></a>Parameters toevoegen

In sommige gevallen moet u mogelijk expliciet vereisen dat de **Inline Code** actie neemt de resultaten van de trigger of specifieke acties die uw code verwijst naar als afhankelijkheden door toe te voegen de **Trigger** of **Acties** parameters. Deze optie is handig voor scenario's waarin de resultaten worden waarnaar wordt verwezen, zijn niet tijdens de uitvoering gevonden.

> [!TIP]
> Als u van plan bent om uw code opnieuw te gebruiken, verwijzingen naar eigenschappen toevoegen met behulp van de **Code** zodat uw code de opgelost token verwijzingen bevat, in plaats van de trigger of acties toe te voegen als expliciete afhankelijkheden.

Stel bijvoorbeeld dat u hebt code die verwijst naar de **SelectedOption** als gevolg van de **e-mail voor goedkeuring verzenden** actie voor de Office 365 Outlook-connector. Bij het maken, de Logic Apps-engine analyseert uw code om te bepalen of u hebt waarnaar wordt verwezen een trigger of actie resulteert en bevat de resultaten automatisch. Tijdens runtime, moet u een foutmelding krijgt dat het waarnaar wordt verwezen, trigger of actie resultaat is niet beschikbaar in de opgegeven `workflowContext` -object, kunt u deze trigger of actie toevoegen als een expliciete afhankelijkheid. In dit voorbeeld voegt u toe de **acties** parameter en opgeven dat de **Inline Code** actie expliciet zijn het resultaat van de **e-mail voor goedkeuring verzenden** actie.

Als u wilt deze parameters toevoegt, opent u de **toevoegen van nieuwe parameter** lijst en selecteer de gewenste parameters:

   ![Parameters toevoegen](./media/logic-apps-add-run-inline-code/inline-code-action-add-parameters.png)

   | Parameter | Description |
   |-----------|-------------|
   | **Acties** | Resultaten van vorige acties opnemen. Zie [actie resultaten opnemen](#action-results). |
   | **Trigger** | Neem de resultaten van de trigger. Zie [opnemen trigger resultaten](#trigger-results). |
   |||

<a name="trigger-results"></a>

### <a name="include-trigger-results"></a>Trigger resultaten opnemen

Als u selecteert **Triggers**, wordt u gevraagd of trigger opnemen.

* Uit de **Trigger** in de lijst met **Ja**.

<a name="action-results"></a>

### <a name="include-action-results"></a>Actie resultaten opnemen

Als u selecteert **acties**, wordt u gevraagd voor de acties die u wilt toevoegen. Voordat u begint met het toevoegen van acties, moet u de versie van de naam van de actie die wordt weergegeven in de onderliggende werkstroom-definitie van de logische app.

* Deze mogelijkheid biedt geen ondersteuning voor variabelen, lussen en herhaling van indexen.

* Namen in de definitie van de werkstroom van uw logische app gebruiken een onderstrepingsteken (_), niet een spatie.

* Actie-namen die gebruikmaken van de punt (.), zijn onder andere deze operators, bijvoorbeeld:

  `My.Action.Name`

1. Kies op de werkbalk van de ontwerper **codeweergave**, en zoek in de `actions` kenmerk voor de actienaam van de.

   Bijvoorbeeld, `Send_approval_email_` is de naam van de JSON voor de **e-mail voor goedkeuring verzenden** actie.

   ![Naam van de actie niet vinden in JSON](./media/logic-apps-add-run-inline-code/find-action-name-json.png)

1. Als u wilt terugkeren naar de ontwerpfunctie weergave op de werkbalk van de weergave code Kies **Designer**.

1. Toevoegen van de eerste actie in de **acties Item - 1** naam in van de actie JSON.

   ![Voer de eerste actie](./media/logic-apps-add-run-inline-code/add-action-parameter.png)

1. Als u wilt nog een actie toevoegen, kiest u **nieuw item toevoegen**.

## <a name="reference"></a>Referentie

Voor meer informatie over de **JavaScript-Code uitvoeren** de structuur van de actie en de syntaxis in uw logische app onderliggende definitie van de werkstroom met behulp van de taal van de definitie van werkstroom zien van de actie [verwijzen naar de sectie ](../logic-apps/logic-apps-workflow-actions-triggers.md#run-javascript-code).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Connectors voor Azure Logic Apps](../connectors/apis-list.md)
