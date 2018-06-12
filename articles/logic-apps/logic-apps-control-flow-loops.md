---
title: Lussen die acties herhalen of verwerken van matrices - Azure Logic Apps toevoegen | Microsoft Docs
description: Het maken van lussen die werkstroomacties herhalen of matrices in Azure Logic Apps verwerken
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 87595eeb0330a2d8210258c097c29b205b628cf4
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298182"
---
# <a name="create-loops-that-repeat-workflow-actions-or-process-arrays-in-azure-logic-apps"></a>Lussen die werkstroomacties herhalen of verwerken van matrices in Azure Logic Apps maken

Als u wilt doorlopen matrices in uw logische app, kunt u een ['Foreach' lus](#foreach-loop) of een [opeenvolgende 'Foreach' lus](#sequential-foreach-loop). Het iteraties voor een standaard 'Foreach' lus parallel uitgevoerd, terwijl het iteraties voor een opeenvolgende 'Foreach' lus één tegelijk worden uitgevoerd. Zie voor het maximum aantal matrixitems waarmee 'Foreach' lussen kunnen worden verwerkt in een enkel logic app uitvoeren, [en -configuratie](../logic-apps/logic-apps-limits-and-config.md). 

> [!TIP] 
> Als u een trigger die een matrix ontvangt en een werkstroom voor elk matrixitem wilt uitvoeren hebt, kunt u *debatch* dat matrix met de [ **SplitOn** activeren eigenschap](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch). 
  
Als u wilt herhalen acties totdat een voorwaarde wordt voldaan of sommige status is gewijzigd, gebruikt u een ['Tot' lus](#until-loop). Uw logische app, voert eerst alle acties binnen de lus en vervolgens de voorwaarde als laatste stap wordt gecontroleerd. Als de voorwaarde wordt voldaan, wordt de lus gestopt. Anders wordt de lus wordt herhaald. Zie voor het maximum aantal 'Tot' Lussen in een enkel logic app uitvoert, [en -configuratie](../logic-apps/logic-apps-limits-and-config.md). 

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/). 

* Elementaire kennis over [logic apps maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="foreach-loop"></a>

## <a name="foreach-loop"></a>'Foreach' lus

Als u wilt herhalen acties voor elk item in een matrix, gebruikt u een lus 'Foreach' in de werkstroom van uw logische app. U kunt meerdere acties opnemen in een lus 'Foreach' en 'Foreach' lussen binnen elkaar kunnen worden genest. Standaard cycli in een standaard 'Foreach' lus parallel worden uitgevoerd. Zie voor het maximale aantal parallelle replicatiecycli die 'Foreach' lussen kunnen uitvoeren, [limieten en config](../logic-apps/logic-apps-limits-and-config.md).

> [!NOTE] 
> Een lus 'Foreach' werkt alleen met een matrix en acties in de lus gebruiken de `@item()` verwijzing naar het verwerken van elk item in de matrix. Als u gegevens die zich niet in een matrix opgeeft, mislukt de logic app-werkstroom. 

Bijvoorbeeld: deze logische app stuurt u een dagelijkse samenvatting van een website RSS-feed. De app gebruikmaakt van een lus 'Foreach' die een e-mailbericht verzendt voor elk nieuw item gevonden.

1. [In dit voorbeeld logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md) met een Outlook.com of Outlook van Office 365-account.

2. Tussen de RSS activeren en verzenden van e-mailadres in te grijpen, een lus "Foreach" toevoegen. 

   Als u wilt een lus tussen stappen toevoegen, de aanwijzer boven de pijl waar u wilt dat de lus toevoegen. 
   Kies de **plusteken** (**+**) dat wordt weergegeven, en kies vervolgens **toevoegen een voor elk**.

   ![Een lus 'Foreach' tussen stappen toevoegen](media/logic-apps-control-flow-loops/add-for-each-loop.png)

3. Nu opbouwen en de lus. Onder **uitvoer selecteren uit de vorige stappen** nadat de **dynamische inhoud toevoegen** lijst wordt weergegeven, selecteert u de **Feed koppelingen** matrix, maar dit is de uitvoer van de RSS-trigger. 

   ![Selecteer in de lijst met dynamische inhoud](media/logic-apps-control-flow-loops/for-each-loop-dynamic-content-list.png)

   > [!NOTE] 
   > U kunt selecteren *alleen* matrix uitvoer van de vorige stap.

   De geselecteerde matrix is nu hier weergegeven:

   ![Matrix selecteren](media/logic-apps-control-flow-loops/for-each-loop-select-array.png)

4. Uit te voeren op elk matrixitem, sleept u de **e-mailbericht verzenden** actie naar de **voor elk** lus. 

   Uw logische app ziet er ongeveer als volgt:

   ![Voeg stappen toe aan de lus "Foreach"](media/logic-apps-control-flow-loops/for-each-loop-with-step.png)

5. Sla uw logische app op. Als u wilt testen handmatig uw logische app, klik op de werkbalk ontwerpen kiezen **uitvoeren**.

<a name="for-each-json"></a>

## <a name="foreach-loop-definition-json"></a>Definitie van de lus 'Foreach' (JSON)

Als u voor uw logische app in de codeweergave werkt, kunt u definiëren de `Foreach` lus in JSON-definitie van uw logische app in plaats daarvan, bijvoorbeeld:

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

## <a name="foreach-loop-sequential"></a>'Foreach' lus: opeenvolgende

Standaard elke cyclus in een lus 'Foreach' wordt voor elk matrixitem parallel worden uitgevoerd. Voor elke cyclus worden opeenvolgend uitgevoerd, stelt de **sequentiële** optie in de lus 'Foreach'.

1. Kies in de rechterbovenhoek van de lus **weglatingstekens** (**...** ) > **Instellingen**.

   ![Op de lus 'Foreach', kiest u '...' > 'Instellingen'](media/logic-apps-control-flow-loops/for-each-loop-settings.png)

2. Schakel de **sequentiële** instelt, en kies vervolgens **gedaan**.

   ![Sequentiële instelling 'Foreach'-lus inschakelen](media/logic-apps-control-flow-loops/for-each-loop-sequential-setting.png)

U kunt ook instellen de **operationOptions** -parameter voor `Sequential` in JSON-definitie van uw logische app. Bijvoorbeeld:

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

## <a name="until-loop"></a>'Tot' lus
  
Als u wilt herhalen acties totdat een voorwaarde wordt voldaan of sommige status is gewijzigd, gebruikt u een lus 'Tot' in de werkstroom van uw logische app. Hier volgen enkele algemene gebruiksvoorbeelden waar u een lus 'Tot' kunt gebruiken:

* Een eindpunt niet aanroepen totdat u het antwoord dat u wilt ophalen.
* Een record maken in een database, wachten tot een bepaald veld in deze record is goedgekeurd en gaan met de verwerking. 

Bijvoorbeeld, om 8:00 uur per dag verhoogd deze logische app een variabele totdat de waarde gelijk is aan 10. De logische app verzendt vervolgens een e-mailbericht wordt bevestigd de huidige waarde dat. Hoewel dit voorbeeld Outlook van Office 365 wordt, kunt u een e-provider wordt ondersteund door logische Apps gebruiken ([controleren de connectors hier lijst](https://docs.microsoft.com/connectors/)). Als u een ander e-mailaccount gebruikt, zijn de algemene stappen hetzelfde, maar ziet de gebruikersinterface er misschien iets anders uit. 

1. Een lege, logische app maken. Zoeken in Logic App Designer "recurrence" en selecteer deze trigger: **schema - terugkeerpatroon** 

   !["Planning--Recurrence" trigger toevoegen](./media/logic-apps-control-flow-loops/do-until-loop-add-trigger.png)

2. Geef op wanneer de trigger wordt geactiveerd door de interval, de frequentie en het uur van de dag. Als u wilt dat het uur, kies **geavanceerde opties weergeven**.

   !["Planning--Recurrence" trigger toevoegen](./media/logic-apps-control-flow-loops/do-until-loop-set-trigger-properties.png)

   | Eigenschap | Waarde |
   | -------- | ----- |
   | **Interval** | 1 | 
   | **Frequentie** | Dag |
   | **Deze uren** | 8 |
   ||| 

3. Kies onder de trigger **nieuwe stap** > **een actie toevoegen**. Zoek naar 'variabelen' en selecteer vervolgens deze actie: **variabelen - variabele initialiseren**

   !['Variabelen - variabele initialiseren' toevoegen actie](./media/logic-apps-control-flow-loops/do-until-loop-add-variable.png)

4. De variabele met deze waarden instellen:

   ![De variabele eigenschappen instellen](./media/logic-apps-control-flow-loops/do-until-loop-set-variable-properties.png)

   | Eigenschap | Waarde | Beschrijving |
   | -------- | ----- | ----------- |
   | **Naam** | Limiet | De naam van de variabele | 
   | **Type** | Geheel getal | Het gegevenstype van de variabele | 
   | **Waarde** | 0 | De variabele de beginwaarde | 
   |||| 

5. Onder de **initialiseren variabele** actie, kies **nieuwe stap** > **meer**. Selecteer deze lus: **een komen pas toevoegen**

   ![Lus voor 'do until' toevoegen](./media/logic-apps-control-flow-loops/do-until-loop-add-until-loop.png)

6. Voorwaarde voor het afsluiten van de lus bouwen door het selecteren van de **limiet** variabele en de **gelijk** operator. Voer **10** als de vergelijkingswaarde.

   ![Voorwaarde voor het afsluiten voor het stoppen van de lus bouwen](./media/logic-apps-control-flow-loops/do-until-loop-settings.png)

7. Kies in de lus **een actie toevoegen**. Zoek naar 'variabelen' en voeg vervolgens deze actie: **variabelen - Increment-variabele**

   ![Actie voor de variabele en oplopend in stappen toevoegen](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable.png)

8. Voor **naam**, selecteer de **limiet** variabele. Voor **waarde**, voer '1'. 

   !['Limiet' verhoogd met 1](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable-settings.png)

9. Onder maar buiten de lus, moet u een actie die e-mailbericht verzendt toevoegen. Meld u aan bij uw e-mailaccount als dat wordt gevraagd.

   ![Actie die e-mailbericht verzendt toevoegen](media/logic-apps-control-flow-loops/do-until-loop-send-email.png)

10. Het e-mailadres eigenschappen instellen. Voeg de **limiet** variabele als het onderwerp. Op die manier kunt u bevestigen de huidige waarde van de variabele voldoet aan de opgegeven voorwaarde, bijvoorbeeld:

    ![E-eigenschappen instellen](./media/logic-apps-control-flow-loops/do-until-loop-send-email-settings.png)

    | Eigenschap | Waarde | Beschrijving |
    | -------- | ----- | ----------- | 
    | **Aan** | *<email-address@domain>* | e-mailadres van de geadresseerde. Voor het testen, moet u uw eigen e-mailadres gebruiken. | 
    | **Onderwerp** | Huidige waarde voor 'Limiet' **limiet** | Geef het onderwerp van e-mail. Zorg ervoor dat u voor dit voorbeeld de **limiet** variabele. | 
    | **Hoofdtekst** | <*email-content*> | Geef de inhoud van het e-bericht te verzenden. Voer de tekst die u voor dit voorbeeld. | 
    |||| 

11. Sla uw logische app op. Als u wilt testen handmatig uw logische app, klik op de werkbalk ontwerpen kiezen **uitvoeren**.

    Nadat uw logica wordt gestart, krijgt u een e-mailbericht met de inhoud die u hebt opgegeven:

    ![Ontvangen e-mail](./media/logic-apps-control-flow-loops/do-until-loop-sent-email.png)

## <a name="prevent-endless-loops"></a>Oneindige lussen voorkomen

Een lus 'Tot' heeft de standaardlimiet die niet meer kan worden uitgevoerd als een van de volgende situaties optreden:

| Eigenschap | Standaardwaarde | Beschrijving | 
| -------- | ------------- | ----------- | 
| **Aantal** | 60 | Het maximum aantal lussen die worden uitgevoerd voordat de lus wordt afgesloten. De standaardwaarde is 60 cycli. | 
| **Timeout** | PT1H | De maximale hoeveelheid tijd in beslag een lus voordat de lus wordt afgesloten. De standaardwaarde is een uur en is opgegeven in de ISO 8601-notatie. <p>De time-outwaarde wordt geëvalueerd voor elke lus-cyclus. Als geen actie in de lus langer dan de time-outlimiet duurt, de huidige cyclus niet stoppen, maar de volgende cyclus niet starten omdat de voorwaarde van de limiet niet is voldaan. | 
|||| 

Als u deze standaardlimieten wijzigen kiezen **geavanceerde opties weergeven** in de vorm van de actie lus.

<a name="until-json"></a>

## <a name="until-definition-json"></a>'Tot' definition (JSON)

Als u voor uw logische app in de codeweergave werkt, kunt u definiëren een `Until` lus in JSON-definitie van uw logische app in plaats daarvan, bijvoorbeeld:

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

Deze lus 'Tot' roept een ander voorbeeld: een HTTP-eindpunt dat u maakt een resource en stopt wanneer de HTTP-antwoordtekst met status 'Voltooid retourneert'. Om te voorkomen dat een oneindige lussen, zorgt de lus er ook als een van de volgende situaties optreden:

* De lus wordt al uitgevoerd 10 keer zoals is opgegeven door de `count` kenmerk. De standaardwaarde is 60 keer. 
* De lus heeft geprobeerd om uit te voeren voor twee uur zoals opgegeven door de `timeout` kenmerk in de ISO 8601-notatie. De standaardwaarde is 1 uur.
  
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
* Om te verzenden of op de functies en suggesties, stemmen [Azure Logic Apps gebruiker feedback site](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Voer stappen uit op basis van een voorwaarde (voorwaardelijke instructies)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Voer stappen uit op basis van verschillende waarden (switch instructies)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uitvoeren of samenvoegen van parallelle stappen (vertakkingen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Voer stappen uit op basis van Actiestatus van de gegroepeerde (scopes genoemd)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
