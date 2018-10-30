---
title: Lussen die acties herhalen of verwerken van matrices - Azure Logic Apps toevoegen | Microsoft Docs
description: Lussen die werkstroomacties herhalen of verwerken van matrices in Azure Logic Apps maken
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 5ba5e5abef4ebdc58c44cbe7f5ba584efe8abfc7
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233103"
---
# <a name="create-loops-that-repeat-workflow-actions-or-process-arrays-in-azure-logic-apps"></a>Lussen die werkstroomacties herhalen of verwerken van matrices in Azure Logic Apps maken

Als u wilt doorlopen en matrices in uw logische app, kunt u een ['Foreach'-lus](#foreach-loop) of een [opeenvolgende 'Foreach'-lus](#sequential-foreach-loop). De herhalingen voor een standaard 'Foreach'-lus parallel uit te voeren terwijl de herhalingen voor een opeenvolgende 'Foreach'-lus een op een tijdstip wordt uitgevoerd. Zie voor het maximum aantal matrixitems waarmee "Foreach" lussen kunnen worden verwerkt in een enkele logische app, [limieten en configuratie](../logic-apps/logic-apps-limits-and-config.md). 

> [!TIP] 
> Als u een trigger die een matrix ontvangt en een werkstroom voor elk matrixitem wilt uitvoeren, kunt u *debatch* matrix met de [ **SplitOn** activeren van de eigenschap](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch). 
  
Als u wilt herhalen acties totdat een voorwaarde is voldaan of sommige status is gewijzigd, gebruikt u een ['Tot'-lus](#until-loop). Uw logische app eerst alle acties die binnen de lus wordt uitgevoerd en daarna de voorwaarde als laatste stap. Als de voorwaarde wordt voldaan, wordt de lus gestopt. Anders wordt de lus wordt herhaald. Zie voor het maximum aantal "Tot" lussen in een enkele logische app, [limieten en configuratie](../logic-apps/logic-apps-limits-and-config.md). 

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/). 

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="foreach-loop"></a>

## <a name="foreach-loop"></a>'Foreach'-lus

Als u wilt herhalen acties voor elk item in een matrix, gebruikt u een lus "Foreach" in uw werkstroom voor logische Apps. U kunt meerdere acties opnemen in een lus 'Foreach' en kunt u "Foreach" lussen in elkaar nesten. Standaard cycli in een standaard 'Foreach'-lus parallel worden uitgevoerd. Zie voor het maximum aantal parallelle cycli die kunnen worden uitgevoerd met lussen 'Foreach", [limieten en configuratie](../logic-apps/logic-apps-limits-and-config.md).

> [!NOTE] 
> Een 'Foreach'-lus werkt alleen met een matrix en acties in de lus gebruiken de `@item()` verwijzing voor het verwerken van elk item in de matrix. Als u gegevens die zich niet in een matrix opgeeft, wordt de werkstroom voor logische Apps mislukt. 

Bijvoorbeeld, deze logische app verzendt u een dagelijkse samenvatting van RSS-feed van een website. De app maakt gebruik van een 'Foreach'-lus die een e-mailbericht verzendt voor elk nieuw item is gevonden.

1. [In dit voorbeeld logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md) met een Outlook.com- of Office 365 Outlook-account.

2. Tussen de RSS-trigger en verzenden van e-mailactie, Voeg een lus 'Foreach'. 

   Als u wilt toevoegen een lus tussen fasen, gaat u de aanwijzer over de pijl waaraan u wilt toevoegen van de lus. 
   Kies de **plusteken** (**+**) die wordt weergegeven, en kies vervolgens **toevoegen een voor elke**.

   ![Een lus "Foreach" tussen fasen toevoegen](media/logic-apps-control-flow-loops/add-for-each-loop.png)

3. Bouw nu de lus. Onder **een uitvoer selecteren uit de vorige stappen** nadat de **dynamische inhoud toevoegen** lijst wordt weergegeven, selecteert u de **Feedkoppelingen** matrix, maar dit de uitvoer van de RSS-trigger is. 

   ![Selecteer in de lijst met dynamische inhoud](media/logic-apps-control-flow-loops/for-each-loop-dynamic-content-list.png)

   > [!NOTE] 
   > U kunt selecteren *alleen* matrix uitvoer van de vorige stap.

   De geselecteerde matrix is nu hier weergegeven:

   ![Selecteer matrix](media/logic-apps-control-flow-loops/for-each-loop-select-array.png)

4. Uit te voeren voor elk matrixitem, sleept u de **een e-mailbericht verzenden** actie in de **voor elk** lus. 

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
        "runAfter": {},
    }
},
```

<a name="sequential-foreach-loop"></a>

## <a name="foreach-loop-sequential"></a>'Foreach'-lus: opeenvolgende

Standaard elke cyclus in een lus "Foreach" wordt voor elk matrixitem parallel worden uitgevoerd. Als u wilt uitvoeren elke cyclus sequentieel worden verwerkt, de **sequentieel** optie in de lus 'Foreach'.

1. Kies in de rechterbovenhoek van de lus **weglatingstekens** (**...** ) > **Instellingen**.

   ![Kies '...' op 'Foreach'-lus, > 'Instellingen'](media/logic-apps-control-flow-loops/for-each-loop-settings.png)

2. Schakel de **sequentieel** instellen, en kies vervolgens **gedaan**.

   !['Foreach'-lus opeenvolgende instelling inschakelen](media/logic-apps-control-flow-loops/for-each-loop-sequential-setting.png)

U kunt ook instellen de **operationOptions** parameter `Sequential` in JSON-definitie van uw logische app. Bijvoorbeeld:

``` json
"actions": {
    "myForEachLoopName": {
        "type": "Foreach",
        "actions": {
            "Send_an_email": {               
            }
        },
        "foreach": "@triggerBody()?['links']",
        "runAfter": {},
        "operationOptions": "Sequential"
    }
},
```

<a name="until-loop"></a>

## <a name="until-loop"></a>"Tot" lus
  
Om acties te herhalen totdat een voorwaarde is voldaan of sommige status is gewijzigd, gebruikt u een lus 'Tot' in uw werkstroom voor logische Apps. Hier volgen enkele algemene scenario's waarin u een lus 'Tot kunt':

* Een eindpunt aanroepen totdat u het antwoord dat u wilt.
* Een record in een database maken, wachten tot een bepaald veld in die record is goedgekeurd en gaan met de verwerking. 

Bijvoorbeeld, om 8:00 uur per dag, deze logische app een variabele verhoogd totdat de waarde van de variabele is gelijk aan 10. De logische app verzendt vervolgens een e-mailbericht wordt bevestigd de huidige waarde dat. Hoewel dit voorbeeld wordt Office 365 Outlook gebruikt, kunt u een e-mailprovider die door Logic Apps worden ondersteund ([Controleer de lijst met connectors hier](https://docs.microsoft.com/connectors/)). Als u een ander e-mailaccount gebruikt, zijn de algemene stappen hetzelfde, maar ziet de gebruikersinterface er misschien iets anders uit. 

1. Een lege, logische app maken. Zoek in Logic App Designer "terugkeerpatroon" en selecteer deze trigger: **planning - terugkeerpatroon** 

   ![Trigger "Planning--terugkeerpatroon" toevoegen](./media/logic-apps-control-flow-loops/do-until-loop-add-trigger.png)

2. Geef op wanneer de trigger wordt geactiveerd door in te stellen de interval, de frequentie en het uur van de dag. Om het uur, kies **geavanceerde opties weergeven**.

   ![Trigger "Planning--terugkeerpatroon" toevoegen](./media/logic-apps-control-flow-loops/do-until-loop-set-trigger-properties.png)

   | Eigenschap | Waarde |
   | -------- | ----- |
   | **Interval** | 1 | 
   | **Frequentie** | Dag |
   | **Deze uren** | 8 |
   ||| 

3. Kies onder de trigger **nieuwe stap** > **een actie toevoegen**. Zoek naar 'variabelen' en selecteer vervolgens deze actie: **variabelen - variabele initialiseren**

   ![Toevoegen van "Variabelen - variabele initialiseren" actie](./media/logic-apps-control-flow-loops/do-until-loop-add-variable.png)

4. Instellen van uw variabele met de volgende waarden:

   ![Eigenschappen van de variabele instellen](./media/logic-apps-control-flow-loops/do-until-loop-set-variable-properties.png)

   | Eigenschap | Waarde | Beschrijving |
   | -------- | ----- | ----------- |
   | **Naam** | Limiet | De naam van uw variabele | 
   | **Type** | Geheel getal | Het gegevenstype van uw variabele | 
   | **Waarde** | 0 | De variabele de beginwaarde | 
   |||| 

5. Onder de **variabele initialiseren** actie, kiest u **nieuwe stap** > **meer**. Selecteer deze lus: **een do until toevoegen**

   ![Lus 'do until' toevoegen](./media/logic-apps-control-flow-loops/do-until-loop-add-until-loop.png)

6. Voorwaarde voor het afsluiten van de lus kunt maken met het selecteren van de **limiet** variabele en de **is gelijk** operator. Voer **10** als de vergelijkingswaarde.

   ![Voorwaarde voor het afsluiten voor het stoppen van de lus bouwen](./media/logic-apps-control-flow-loops/do-until-loop-settings.png)

7. Binnen de lus, kiest u **een actie toevoegen**. Zoek naar 'variabelen' en voegt u deze actie: **variabelen - variabele verhogen**

   ![Actie voor het verhogen van de variabele toevoegen](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable.png)

8. Voor **naam**, selecteer de **limiet** variabele. Voor **waarde**, voer '1'. 

   ![Verhogen 'Limiet' met 1](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable-settings.png)

9. Onder maar buiten de lus, een actie toevoegen waarmee e-mail wordt verzonden. Meld u aan bij uw e-mailaccount als dat wordt gevraagd.

   ![Actie toevoegen waarmee e-mail wordt verzonden](media/logic-apps-control-flow-loops/do-until-loop-send-email.png)

10. Eigenschappen van het e-mailbericht instellen. Voeg de **limiet** variabele aan het onderwerp. Op die manier kunt u bevestigen de huidige waarde van de variabele voldoet aan de opgegeven criteria, bijvoorbeeld:

    ![E-eigenschappen instellen](./media/logic-apps-control-flow-loops/do-until-loop-send-email-settings.png)

    | Eigenschap | Waarde | Beschrijving |
    | -------- | ----- | ----------- | 
    | **Aan** | *<email-address@domain>* | e-mailadres van de ontvanger. Voor het testen, moet u uw eigen e-mailadres gebruiken. | 
    | **Onderwerp** | Huidige waarde voor de 'Limiet' is **limiet** | Geef het e-mailonderwerp. Zorg ervoor dat u voor dit voorbeeld wordt de **limiet** variabele. | 
    | **Hoofdtekst** | <*email-content*> | Geef de inhoud van het e-bericht die u wilt verzenden. Voer de tekst die u in dit voorbeeld. | 
    |||| 

11. Sla uw logische app op. Als u wilt testen handmatig uw logische app, op de werkbalk van de ontwerper, kies **uitvoeren**.

    Nadat uw logica uitgevoerd wordt, krijgt u een e-mailbericht met de inhoud die u hebt opgegeven:

    ![Ontvangen e-mail](./media/logic-apps-control-flow-loops/do-until-loop-sent-email.png)

## <a name="prevent-endless-loops"></a>Oneindige lus te voorkomen dat

Een lus 'Tot' heeft standaardlimieten die niet meer kan worden uitgevoerd als een van deze voorwaarden is gebeurd:

| Eigenschap | Standaardwaarde | Beschrijving | 
| -------- | ------------- | ----------- | 
| **Aantal** | 60 | Het maximale aantal lussen die worden uitgevoerd voordat de lus wordt afgesloten. De standaardwaarde is 60 cycli. | 
| **Timeout** | PT1H | De maximale hoeveelheid tijd om uit te voeren een lus voordat u de lus wordt afgesloten. De standaardinstelling is één uur en is opgegeven in de ISO 8601-notatie. <p>De time-outwaarde wordt geëvalueerd voor elke lus-cyclus. Als geen actie op de hoogte langer duurt dan de time-outlimiet, de huidige cyclus niet stoppen, maar de volgende cyclus niet starten omdat de voorwaarde van de limiet is niet voldaan. | 
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
        },
    }
},
```

Deze lus 'Tot' roept een ander voorbeeld: een HTTP-eindpunt dat een resource wordt gemaakt en stopt wanneer de hoofdtekst van het HTTP-antwoord geretourneerd met de status 'Voltooid'. Om te voorkomen dat een oneindige lus, stopt de lus ook als een van deze voorwaarden is gebeurd:

* De lus is uitgevoerd 10 keer zoals is opgegeven door de `count` kenmerk. De standaardwaarde is 60 keer. 
* De lus heeft geprobeerd om uit te voeren voor twee uur zoals opgegeven door de `timeout` kenmerk in ISO 8601-notatie. De standaardwaarde is één uur.
  
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
},
```

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Op indienen of hierop stemmen op de functies en suggesties voor [site voor gebruikersfeedback van Azure Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Voer stappen uit op basis van een voorwaarde (voorwaardelijke instructies)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Voer stappen uit op basis van verschillende waarden (switch-instructies)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uitvoeren of samenvoegen van parallelle stappen (vertakkingen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Voer stappen uit op basis van gegroepeerde Actiestatus (bereiken)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
