---
title: Lussen die acties herhalen of verwerken van matrices - Azure Logic Apps toevoegen | Microsoft Docs
description: Lussen die werkstroomacties herhalen of verwerken van matrices in Azure Logic Apps maken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
manager: jeconnoc
ms.date: 01/05/2019
ms.topic: article
ms.openlocfilehash: b2e8d629f4007729ad0538aee9bdb8e67747b026
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58015125"
---
# <a name="create-loops-that-repeat-workflow-actions-or-process-arrays-in-azure-logic-apps"></a>Lussen die werkstroomacties herhalen of verwerken van matrices in Azure Logic Apps maken

Voor het verwerken van een matrix in uw logische app, kunt u een ['Foreach'-lus](#foreach-loop). Deze lus een of meer acties voor elk item in de matrix. Zie voor beperkingen met betrekking tot het aantal matrixitems die 'Foreach' kunnen worden verwerkt, [limieten en configuratie](../logic-apps/logic-apps-limits-and-config.md). 

Als u wilt herhalen acties totdat een voorwaarde wordt voldaan, of een status verandert, kunt u een ['Tot'-lus](#until-loop). Uw logische app de acties binnen de lus wordt uitgevoerd, en controleert vervolgens of de voorwaarde of status. Als de voorwaarde wordt voldaan, wordt de lus gestopt. Anders wordt de lus wordt herhaald. Zie voor beperkingen met betrekking tot het aantal 'Tot' Lussen in een logische app uitvoeren, [limieten en configuratie](../logic-apps/logic-apps-limits-and-config.md). 

> [!TIP]
> Als u een trigger die een matrix ontvangt en een werkstroom voor elk matrixitem wilt uitvoeren, kunt u *debatch* matrix met de [ **SplitOn** activeren van de eigenschap](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch). 

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/). 

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="foreach-loop"></a>

## <a name="foreach-loop"></a>'Foreach'-lus

Een "Foreach-lus' wordt herhaald voor een of meer acties voor elk matrixitem en werkt alleen op matrices. Iteraties in een lus 'Foreach' parallel worden uitgevoerd. Echter, u iteraties één tegelijk kunt uitvoeren door het instellen van een [opeenvolgende 'Foreach'-lus](#sequential-foreach-loop). 

Hier volgen enkele overwegingen wanneer u "Foreach" lussen gebruiken:

* In de geneste lussen iteraties altijd worden opeenvolgend uitgevoerd, niet parallel. Als u wilt uitvoeren van bewerkingen voor items in een geneste lus parallel, maken en [een onderliggende logische app aanroepen](../logic-apps/logic-apps-http-endpoint.md).

* Als u voorspelbare resultaten van bewerkingen op de variabelen tijdens elke herhaling van de lus, moet u deze lussen worden opeenvolgend uitgevoerd. Bijvoorbeeld, wanneer een gelijktijdig worden uitgevoerd in een lus eindigt, verhogen, verlagen en toevoegen aan de variabele operations voorspelbare resultaten geretourneerd. Deze bewerkingen kunnen echter tijdens elke herhaling in de lus gelijktijdig actieve retourneren onvoorspelbare resultaten. 

* Acties in een 'Foreach'-lus gebruiken de [`@item()`](../logic-apps/workflow-definition-language-functions-reference.md#item) 
de expressie om te verwijzen naar en verwerken van elk item in de matrix. Als u gegevens die zich niet in een matrix opgeeft, wordt de werkstroom voor logische Apps mislukt. 

In dit voorbeeld van logische app verzendt een dagelijks overzicht voor de RSS-feed van een website. De app gebruikt een lus "Foreach" waarmee een e-mailbericht voor elk nieuw item wordt verzonden.

1. [In dit voorbeeld logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md) met een Outlook.com- of Office 365 Outlook-account.

2. Tussen de RSS-trigger en verzenden van e-mailactie, Voeg een lus 'Foreach'. 

   1. Als u wilt toevoegen een lus tussen fasen, de aanwijzer over de pijl tussen deze stappen. 
   Kies de **plusteken** (**+**) die wordt weergegeven, schakelt u **een actie toevoegen**.

      ![Selecteer 'Een actie toevoegen'](media/logic-apps-control-flow-loops/add-for-each-loop.png)

   1. Kies **Alle** onder het zoekvak. Typ 'voor elke' als filter in het zoekvak in. Selecteer deze actie uit de lijst met acties: **Voor elk - besturingselement**

      ![Lus 'voor elke' toevoegen](media/logic-apps-control-flow-loops/select-for-each.png)

3. Bouw nu de lus. Onder **een uitvoer selecteren uit de vorige stappen** nadat de **dynamische inhoud toevoegen** lijst wordt weergegeven, selecteert u de **Feedkoppelingen** matrix, maar dit de uitvoer van de RSS-trigger is. 

   ![Selecteer in de lijst met dynamische inhoud](media/logic-apps-control-flow-loops/for-each-loop-dynamic-content-list.png)

   > [!NOTE] 
   > U kunt selecteren *alleen* matrix uitvoer van de vorige stap.

   De geselecteerde matrix is nu hier weergegeven:

   ![Selecteer matrix](media/logic-apps-control-flow-loops/for-each-loop-select-array.png)

4. Als u wilt een actie uitgevoerd voor elk matrixitem, sleept u de **een e-mailbericht verzenden** actie in de lus. 

   Uw logische app ziet er ongeveer als volgt uit:

   ![Voeg stappen toe aan 'Foreach'-lus](media/logic-apps-control-flow-loops/for-each-loop-with-step.png)

5. Sla uw logische app op. Als u wilt testen handmatig uw logische app, op de werkbalk van de ontwerper, kies **uitvoeren**.

<a name="for-each-json"></a>

## <a name="foreach-loop-definition-json"></a>Definitie van de lus 'Foreach' (JSON)

Als u in de codeweergave voor uw logische app werkt, kunt u definiëren de `Foreach` lus in JSON-definitie van uw logische app in plaats daarvan, bijvoorbeeld:

``` json
"actions": {
   "myForEachLoopName": {
      "type": "Foreach",
      "actions": {
         "Send_an_email": {
            "type": "ApiConnection",
            "inputs": {
               "body": {
                  "Body": "@{item()}",
                  "Subject": "New CNN post @{triggerBody()?['publishDate']}",
                  "To": "me@contoso.com"
               },
               "host": {
                  "api": {
                     "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/office365"
                  },
                  "connection": {
                     "name": "@parameters('$connections')['office365']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            },
            "runAfter": {}
         }
      },
      "foreach": "@triggerBody()?['links']",
      "runAfter": {}
   }
}
```

<a name="sequential-foreach-loop"></a>

## <a name="foreach-loop-sequential"></a>'Foreach'-lus: Sequentieel

Standaard cycli in een lus 'Foreach' parallel worden uitgevoerd. Als u wilt uitvoeren elke cyclus sequentieel worden verwerkt, instellen van de lus **sequentieel** optie. Lussen "Foreach" moeten worden opeenvolgend uitgevoerd wanneer u geneste lussen of variabelen in lussen waar u voorspelbare resultaten verwacht. 

1. Kies in de rechterbovenhoek van de lus **weglatingstekens** (**...** ) > **Instellingen**.

   ![Kies '...' op 'Foreach'-lus, > 'Instellingen'](media/logic-apps-control-flow-loops/for-each-loop-settings.png)

1. Onder **gelijktijdigheidsbeheer**, schakelt u de **gelijktijdigheidsbeheer** instelt op **op**. Verplaats de **graad van parallelle uitvoering** schuifregelaar **1**, en kies **gedaan**.

   ![Schakel gelijktijdigheidsbeheer](media/logic-apps-control-flow-loops/for-each-loop-sequential-setting.png)

Als u met JSON-definitie van uw logische app werkt, kunt u de `Sequential` optie door toe te voegen de `operationOptions` parameter, bijvoorbeeld:

``` json
"actions": {
   "myForEachLoopName": {
      "type": "Foreach",
      "actions": {
         "Send_an_email": { }
      },
      "foreach": "@triggerBody()?['links']",
      "runAfter": {},
      "operationOptions": "Sequential"
   }
}
```

<a name="until-loop"></a>

## <a name="until-loop"></a>"Tot" lus
  
Om te worden herhaald acties totdat een voorwaarde wordt voldaan, of een status wordt gewijzigd, plaatst u deze acties in een lus 'Tot'. Hier volgen enkele algemene scenario's waarin u een lus 'Tot kunt':

* Een eindpunt aanroepen totdat u het gewenste antwoord.

* Een record in een database maken. Wachten tot een bepaald veld in die record is goedgekeurd. Gaan met de verwerking. 

Beginnen om 8:00 uur per dag, verhoogd in dit voorbeeld van logische app een variabele totdat de waarde van de variabele is gelijk aan 10. De logische app verzendt vervolgens een e-mailbericht wordt bevestigd de huidige waarde dat. 

> [!NOTE]
> Deze stappen gebruiken Office 365 Outlook, maar u kunt een e-mailprovider die ondersteuning biedt voor logische Apps gebruiken. 
> [Controleer de lijst met connectors hier](https://docs.microsoft.com/connectors/). Als u een ander e-mailaccount gebruikt, blijven de algemene stappen gelijk, maar uw gebruikersinterface kan er iets anders uitzien. 

1. Een lege, logische app maken. In Logic App Designer, onder het zoekvak typt, kiest u **alle**. Zoeken naar "terugkeerpatroon". 
   Selecteer deze trigger uit de lijst met triggers: **Herhaling - schema**

   !['--Terugkeerschema' trigger toevoegen](./media/logic-apps-control-flow-loops/do-until-loop-add-trigger.png)

1. Geef op wanneer de trigger wordt geactiveerd door in te stellen de interval, de frequentie en het uur van de dag. Om het uur, kies **geavanceerde opties weergeven**.

   ![Terugkerend schema instellen](./media/logic-apps-control-flow-loops/do-until-loop-set-trigger-properties.png)

   | Eigenschap | Value |
   | -------- | ----- |
   | **Interval** | 1 | 
   | **Frequentie** | Dag |
   | **Deze uren** | 8 |
   ||| 

1. Kies onder de trigger **nieuwe stap**. 
   Zoek naar 'variabelen' en selecteer deze actie: **Variabele - variabelen initialiseren**

   ![De actie 'Initialize variabele - variabelen' toevoegen](./media/logic-apps-control-flow-loops/do-until-loop-add-variable.png)

1. Instellen van uw variabele met de volgende waarden:

   ![Eigenschappen van de variabele instellen](./media/logic-apps-control-flow-loops/do-until-loop-set-variable-properties.png)

   | Eigenschap | Waarde | Beschrijving |
   | -------- | ----- | ----------- |
   | **Naam** | Limiet | De naam van uw variabele | 
   | **Type** | Geheel getal | Het gegevenstype van uw variabele | 
   | **Waarde** | 0 | De variabele de beginwaarde | 
   |||| 

1. Onder de **variabele initialiseren** actie, kiest u **nieuwe stap**. 

1. Kies **Alle** onder het zoekvak. Zoek naar 'tot' en selecteer deze actie: **Totdat - besturingselement**

   !["Tot" lus toevoegen](./media/logic-apps-control-flow-loops/do-until-loop-add-until-loop.png)

1. Voorwaarde voor het afsluiten van de lus kunt maken met het selecteren van de **limiet** variabele en de **is gelijk** operator. 
   Voer **10** als de vergelijkingswaarde.

   ![Voorwaarde voor het afsluiten voor het stoppen van de lus bouwen](./media/logic-apps-control-flow-loops/do-until-loop-settings.png)

1. Binnen de lus, kiest u **een actie toevoegen**. 

1. Kies **Alle** onder het zoekvak. Zoek naar 'variabelen' en selecteer deze actie: **Variabele verhogen - variabelen**

   ![Actie voor het verhogen van de variabele toevoegen](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable.png)

1. Voor **naam**, selecteer de **limiet** variabele. Voor **waarde**, voer '1'. 

    ![Verhogen 'Limiet' met 1](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable-settings.png)

1. Buiten en onder de lus, kiest u **nieuwe stap**. 

1. Kies **Alle** onder het zoekvak. 
    Zoeken en een actie toevoegen waarmee e-mailadres, bijvoorbeeld verzendt: 

    ![Actie toevoegen waarmee e-mail wordt verzonden](media/logic-apps-control-flow-loops/do-until-loop-send-email.png)

1. Meld u aan bij uw e-mailaccount als dat wordt gevraagd.

1. Het e-mailadres van de actie-eigenschappen instellen. Voeg de **limiet** variabele aan het onderwerp. Op die manier kunt u bevestigen de huidige waarde van de variabele voldoet aan de opgegeven criteria, bijvoorbeeld:

     ![E-eigenschappen instellen](./media/logic-apps-control-flow-loops/do-until-loop-send-email-settings.png)

     | Eigenschap | Waarde | Description |
     | -------- | ----- | ----------- | 
     | **Aan** | *<email-address\@domain>* | e-mailadres van de ontvanger. Voor het testen, moet u uw eigen e-mailadres gebruiken. | 
     | **Onderwerp** | Huidige waarde voor de 'Limiet' is **limiet** | Geef het e-mailonderwerp. Zorg ervoor dat u voor dit voorbeeld wordt de **limiet** variabele. | 
     | **Hoofdtekst** | <*email-content*> | Geef de inhoud van het e-bericht die u wilt verzenden. Voer de tekst die u in dit voorbeeld. | 
     |||| 

1. Sla uw logische app op. Als u wilt testen handmatig uw logische app, op de werkbalk van de ontwerper, kies **uitvoeren**.

     Nadat uw logica uitgevoerd wordt, krijgt u een e-mailbericht met de inhoud die u hebt opgegeven:

     ![Ontvangen e-mail](./media/logic-apps-control-flow-loops/do-until-loop-sent-email.png)

## <a name="prevent-endless-loops"></a>Oneindige lus te voorkomen dat

Een lus 'Tot' heeft standaardlimieten die niet meer kan worden uitgevoerd als een van deze voorwaarden is gebeurd:

| Eigenschap | Standaardwaarde | Description | 
| -------- | ------------- | ----------- | 
| **Aantal** | 60 | Het hoogste aantal lussen die worden uitgevoerd voordat de lus wordt afgesloten. De standaardwaarde is 60 cycli. | 
| **Timeout** | PT1H | De meeste hoeveelheid tijd om uit te voeren een lus voordat u de lus wordt afgesloten. De standaardinstelling is één uur en is opgegeven in de ISO 8601-notatie. <p>De time-outwaarde wordt geëvalueerd voor elke lus-cyclus. Als geen actie op de hoogte langer duurt dan de time-outlimiet, wordt de huidige cyclus niet stoppen. De volgende cyclus niet echter worden gestart omdat de voorwaarde van de limiet is niet voldaan. | 
|||| 

Als u wilt deze standaardlimieten wijzigen, kiest u **geavanceerde opties weergeven** in de vorm van de actie lus.

<a name="until-json"></a>

## <a name="until-definition-json"></a>"Tot"-definitie (JSON)

Als u in de codeweergave voor uw logische app werkt, kunt u een `Until` lus in JSON-definitie van uw logische app in plaats daarvan, bijvoorbeeld:

``` json
"actions": {
   "Initialize_variable": {
      // Definition for initialize variable action
   },
   "Send_an_email": {
      // Definition for send email action
   },
   "Until": {
      "type": "Until",
      "actions": {
         "Increment_variable": {
            "type": "IncrementVariable",
            "inputs": {
               "name": "Limit",
               "value": 1
            },
            "runAfter": {}
         }
      },
      "expression": "@equals(variables('Limit'), 10)",
      // To prevent endless loops, an "Until" loop 
      // includes these default limits that stop the loop. 
      "limit": { 
         "count": 60,
         "timeout": "PT1H"
      },
      "runAfter": {
         "Initialize_variable": [
            "Succeeded"
         ]
      }
   }
}
```

In dit voorbeeld "Tot" lus roept een HTTP-eindpunt, waarmee een resource wordt gemaakt. De lus stopt wanneer de hoofdtekst van het HTTP-antwoord geretourneerd met `Completed` status. Om te voorkomen dat een oneindige lus, stopt de lus ook als een van deze voorwaarden is gebeurd:

* De lus uitgevoerd 10 keer zoals is opgegeven door de `count` kenmerk. De standaardwaarde is 60 keer. 

* De lus uitgevoerd gedurende twee uur zoals opgegeven door de `timeout` kenmerk in ISO 8601-notatie. De standaardwaarde is één uur.
  
``` json
"actions": {
   "myUntilLoopName": {
      "type": "Until",
      "actions": {
         "Create_new_resource": {
            "type": "Http",
            "inputs": {
               "body": {
                  "resourceId": "@triggerBody()"
               },
               "url": "https://domain.com/provisionResource/create-resource",
               "body": {
                  "resourceId": "@triggerBody()"
               }
            },
            "runAfter": {},
            "type": "ApiConnection"
         }
      },
      "expression": "@equals(triggerBody(), 'Completed')",
      "limit": {
         "count": 10,
         "timeout": "PT2H"
      },
      "runAfter": {}
   }
}
```

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Op indienen of hierop stemmen op de functies en suggesties voor [site voor gebruikersfeedback van Azure Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Voer stappen uit op basis van een voorwaarde (voorwaardelijke instructies)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Voer stappen uit op basis van verschillende waarden (switch-instructies)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uitvoeren of samenvoegen van parallelle stappen (vertakkingen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Voer stappen uit op basis van gegroepeerde Actiestatus (bereiken)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
