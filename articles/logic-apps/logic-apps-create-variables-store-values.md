---
title: 'Variabelen voor het opslaan van waarden zijn: Azure Logic Apps maken | Microsoft Docs'
description: Het opslaan en beheren van de waarden door te maken van variabelen in Azure Logic Apps
services: logic-apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.topic: article
ms.date: 05/30/2018
ms.service: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: e525e5584e4835b0f2b73203c818c3f799b77cf5
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58075711"
---
# <a name="create-variables-for-saving-and-managing-values-in-azure-logic-apps"></a>Variabelen voor het opslaan en beheren van waarden in Azure Logic Apps maken

Dit artikel wordt beschreven hoe u kunt opslaan en werken met waarden in uw logische app met het maken van variabelen. Bijvoorbeeld, kunt variabelen u het aantal keren dat een lus wordt uitgevoerd. Wanneer iteratie van een matrix of een matrix voor een specifiek item controleren, kunt u een variabele gebruiken om te verwijzen naar het indexnummer voor elk matrixitem. 

U kunt variabelen voor de gegevenstypen, zoals geheel getal, drijvende komma, Booleaanse waarde, string, matrix en object maken. Nadat u een variabele maakt, kunt u bijvoorbeeld andere taken uitvoeren:

* Bekijk of verwijzen naar de waarde van de variabele.
* Het vergroten of verkleinen van de variabele met een constante waarde, ook wel bekend als *verhoging* en *verlagen*.
* Wijs een andere waarde toe aan de variabele.
* Invoegen of *append* van de variabele waarde als de laatste keer in een tekenreeks of een matrix.

Variabelen bestaan en globale alleen in de logische app-instantie waarin ze zijn gemaakt. Ze bewaren tevens, voor elke lus herhalingen binnen een exemplaar van de logische app. Wanneer u verwijst naar een variabele, gebruikt u de naam van de variabele als het token, niet van de actie naam, dit is de gebruikelijke manier om te verwijzen naar de uitvoer van een actie. 

> [!IMPORTANT]
> Standaard cycli in een lus 'Foreach' parallel worden uitgevoerd. Wanneer u variabelen in lussen gebruiken, voert u de lus [sequentieel](../logic-apps/logic-apps-control-flow-loops.md#sequential-foreach-loop) zodat variabelen voorspelbare resultaten worden geretourneerd. 

Als u een Azure-abonnement nog geen <a href="https://azure.microsoft.com/free/" target="_blank">zich aanmelden voor een gratis Azure-account</a>. 

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen in dit artikel, zijn dit de items die u nodig hebt:

* De logische app waar u een variabele maken 

  Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Quick Start: Maak uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) als de eerste stap in uw logische app 

  Voordat u acties voor het maken en werken met variabelen toevoegen kunt, wordt uw logische app moet beginnen met een trigger.

<a name="create-variable"></a>

## <a name="initialize-variable"></a>Variabele initialiseren

U kunt een variabele maken en de gegevens van het type en de initiële waarde - alle binnen één actie in uw logische app declareren. U kunt alleen variabelen op globaal niveau, niet binnen de bereiken, voorwaarden en lussen aangeven. 

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> of Visual Studio, open uw logische app in Logic App Designer. 

   In dit voorbeeld maakt gebruik van de Azure-portal en een logische app met een bestaande trigger.

2. In uw logische app, onder de stap waarin u wilt toevoegen van een variabele, een van deze stappen te volgen: 

   * Als u wilt een actie toevoegen onder de laatste stap, kies **nieuwe stap** > **een actie toevoegen**.

     ![Actie toevoegen](./media/logic-apps-create-variables-store-values/add-action.png)

   * Als u wilt toevoegen een actie tussen fasen, u de muisaanwijzer over het maken van verbinding pijl, zodat het plusteken (+) wordt weergegeven. 
   Kies het plusteken en kies vervolgens **een actie toevoegen**.

3. Typ 'variabelen' als filter in het zoekvak. Selecteer in de lijst met acties **variabelen - variabele initialiseren**.

   ![Actie selecteren](./media/logic-apps-create-variables-store-values/select-initialize-variable-action.png)

4. Geef deze informatie voor de variabele:

   | Eigenschap | Vereist | Value |  Beschrijving |
   |----------|----------|-------|--------------|
   | Name | Ja | <*variable-name*> | De naam van de variabele moet worden verhoogd | 
   | Type | Ja | <*variable-type*> | Het gegevenstype voor de variabele | 
   | Value | Nee | <*start-value*> | De beginwaarde van uw variabele <p><p>**Tip**: Hoewel dit optioneel is, moet u deze waarde instellen als een best practice, zodat u altijd weet de beginwaarde voor de variabele wat. | 
   ||||| 

   ![Variabele initialiseren](./media/logic-apps-create-variables-store-values/initialize-variable.png)

5. Nu doorgaan met de acties die u wilt toevoegen. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**.

Als u van de ontwerpfunctie voor de weergave-editor overschakelt, volgt u de manier waarop de **variabele initialiseren** actie wordt weergegeven in het definitie logische app, die zich in JavaScript Object Notation (JSON)-indeling:

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "Count",
               "type": "Integer",
               "value": 0
          } ]
      },
      "runAfter": {}
   }
},
```

Hier volgen enkele voorbeelden voor sommige andere typen variabelen:

*String-variabele*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myStringVariable",
               "type": "String",
               "value": "lorem ipsum"
          } ]
      },
      "runAfter": {}
   }
},
```

*Booleaanse variabele*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myBooleanVariable",
               "type": "Boolean",
               "value": false
          } ]
      },
      "runAfter": {}
   }
},
```

*Matrix met gehele getallen*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myArrayVariable",
               "type": "Array",
               "value": [1, 2, 3]
          } ]
      },
      "runAfter": {}
   }
},
```

*Matrix met tekenreeksen*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myArrayVariable",
               "type": "Array",
               "value": ["red", "orange", "yellow"]
          } ]
      },
      "runAfter": {}
   }
},
```

<a name="get-value"></a>

## <a name="get-the-variables-value"></a>De waarde van de variabele ophalen

Als u wilt ophalen of verwijzen naar de inhoud van een variabele, u kunt ook de [variables() functie](../logic-apps/workflow-definition-language-functions-reference.md#variables) in Logic App Designer en de code-editor weergeven.
Wanneer u verwijst naar een variabele, gebruikt u de naam van de variabele als het token, niet van de actie naam, dit is de gebruikelijke manier om te verwijzen naar de uitvoer van een actie. 

Bijvoorbeeld de items in deze expressie wordt opgehaald van de matrixvariabele [eerder hebt gemaakt in dit artikel](#append-value) met behulp van de **variables()** functie. De **string()** functie retourneert de inhoud van de variabele in de indeling van tekenreeks: `"1, 2, 3, red"`

```json
@{string(variables('myArrayVariable'))}
```

<a name="increment-value"></a>

## <a name="increment-variable"></a>Variabele verhogen 

Om te verhogen of *verhoging* toevoegen van een variabele met een constante waarde de **variabelen - variabele verhogen** actie aan uw logische app. Deze actie werkt alleen met gehele getallen en float-variabelen.

1. In Logic App Designer, onder de stap waar u naar het verhogen van een bestaande variabele wilt kiezen **nieuwe stap** > **een actie toevoegen**. 

   Deze logische app is bijvoorbeeld al een trigger en een actie die een variabele hebt gemaakt. Dus een nieuwe actie toevoegen onder deze stappen:

   ![Actie toevoegen](./media/logic-apps-create-variables-store-values/add-increment-variable-action.png)

   Om toe te voegen een actie tussen bestaande stappen, verplaatst u de muis boven de verbindende pijl zodat het plusteken (+) wordt weergegeven. Kies het plusteken en kies vervolgens **een actie toevoegen**.

2. Typ 'variabele verhogen' als filter in het zoekvak. Selecteer in de lijst met acties **variabelen - variabele verhogen**.

   ![Selecteer de actie "Variabele verhogen"](./media/logic-apps-create-variables-store-values/select-increment-variable-action.png)

3. Geef deze informatie voor het verhogen van de variabele:

   | Eigenschap | Vereist | Value |  Beschrijving |
   |----------|----------|-------|--------------|
   | Name | Ja | <*variable-name*> | De naam van de variabele moet worden verhoogd | 
   | Value | Nee | <*increment-value*> | De waarde die wordt gebruikt voor het verhogen van de variabele. De standaardwaarde is een. <p><p>**Tip**: Hoewel dit optioneel is, moet u deze waarde instellen als een best practice, zodat u altijd de waarde weet voor uw variabele verhoogd. | 
   |||| 

   Bijvoorbeeld: 
   
   ![Voorbeeld van een waarde interval](./media/logic-apps-create-variables-store-values/increment-variable-action-information.png)

4. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**. 

Als u van de ontwerpfunctie voor de weergave-editor overschakelt, volgt u de manier waarop de **variabele verhogen** actie wordt weergegeven in het definitie logische app, dat zich in de JSON-indeling:

```json
"actions": {
   "Increment_variable": {
      "type": "IncrementVariable",
      "inputs": {
         "name": "Count",
         "value": 1
      },
      "runAfter": {}
   }
},
```

## <a name="example-create-loop-counter"></a>Voorbeeld: Lus item maken

Variabelen worden vaak gebruikt voor het tellen van het aantal keren dat een lus wordt uitgevoerd. In dit voorbeeld laat zien hoe u variabelen maken en gebruiken voor deze taak door het maken van een lus waarmee de bijlagen in een e-mailbericht wordt geteld.

1. Maak een lege, logische app in de Azure-portal. Een trigger die controleert op nieuwe e-mailbericht en eventuele bijlagen toevoegen. 

   In dit voorbeeld wordt de Office 365 Outlook-trigger voor **wanneer een nieuwe e-mail binnenkomt**. 
   U kunt deze trigger instellen om te starten wanneer de e-mail bijlagen heeft.
   Echter, kunt u een connector waarmee wordt gecontroleerd voor nieuwe e-mailberichten met bijlagen, zoals de Outlook.com-connector.

2. Kies in de trigger **geavanceerde opties weergeven**. Als u de trigger wordt gecontroleerd op bijlagen en doorgeven die bijlagen in uw logic app-werkstroom wilt weergeven, selecteert u **Ja** voor deze eigenschappen:
   
   * **Heeft bijlage** 
   * **Bijlagen opnemen** 

   ![Controleren en bijlagen opnemen](./media/logic-apps-create-variables-store-values/check-include-attachments.png)

3. Voeg de [ **variabele initialiseren** actie](#create-variable). Maak een geheel getal variabele met de naam **aantal** door een nul beginwaarde.

   ![Actie voor "Variabele initialiseren" toevoegen](./media/logic-apps-create-variables-store-values/initialize-variable.png)

4. Om te bladeren naar elke bijlage, Voeg een *voor elk* lus door te kiezen **nieuwe stap** > **meer** > **toevoegen een voor elke**.

   ![Een lus 'voor elke' toevoegen](./media/logic-apps-create-variables-store-values/add-loop.png)

5. Klik in de lus in de **een uitvoer selecteren uit de vorige stappen** vak. Wanneer de lijst met dynamische inhoud wordt weergegeven, selecteert u **bijlagen**. 

   ![Selecteer 'Bijlagen'](./media/logic-apps-create-variables-store-values/select-attachments.png)

   De **bijlagen** veld geeft een matrix met de e-mailbijlagen uit de uitvoer van de trigger in uw lus.

6. Selecteer in de lus 'voor elke' **een actie toevoegen**. 

   ![Selecteer 'Een actie toevoegen'](./media/logic-apps-create-variables-store-values/add-action-2.png)

7. Typ 'variabele verhogen' als filter in het zoekvak. Selecteer in de lijst met acties **variabelen - variabele verhogen**.

   > [!NOTE]
   > Zorg ervoor dat de **variabele verhogen** actie binnen de lus wordt weergegeven. Als de actie wordt weergegeven buiten de lus, sleept u de actie in de lus.

8. In de **variabele verhogen** actie van de **naam** in de lijst met de **aantal** variabele. 

   !["Aantal" variabele selecteren](./media/logic-apps-create-variables-store-values/add-increment-variable-example.png)

9. Onder de lus toevoegen een actie op die u het aantal bijlagen verzendt. Opnemen in uw actie, de waarde van de **aantal** variabele, bijvoorbeeld: 

   ![Een actie toevoegen die de resultaten verzenden](./media/logic-apps-create-variables-store-values/send-email-results.png)

10. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper. 

### <a name="test-your-logic-app"></a>Uw logische app testen

1. Als uw logische app is niet ingeschakeld op het menu van uw logische app, kiest u **overzicht**. Kies op de werkbalk **inschakelen**. 

2. Kies op de werkbalk Logic App Designer **uitvoeren**. Deze stap kunt u handmatig uw logische app start.

3. Verzendt een e-mailbericht met een of meer bijlagen door naar het e-mailaccount dat u hebt gebruikt in dit voorbeeld.

   Deze stap wordt geactiveerd voor de logische app trigger, die wordt gemaakt en een exemplaar van de werkstroom van uw logische app wordt uitgevoerd.
   Als gevolg hiervan, stuurt de logische app u een bericht of een e-mailbericht dat wordt het aantal bijlagen in de e-mail die u hebt verzonden.

Als u van de ontwerpfunctie voor de weergave-editor overschakelt, dit is de manier die de lus 'voor elke' wordt weergegeven met de **variabele verhogen** actie binnen uw logische app-definitie, dat zich in de JSON-indeling.

```json
"actions": {
   "For_each": {
      "type": "Foreach",
      "actions": {
         "Increment_variable": {
           "type": "IncrementVariable",
            "inputs": {
               "name": "Count",
               "value": 1
            },
            "runAfter": {}
         }
      },
      "foreach": "@triggerBody()?['Attachments']",
      "runAfter": {
         "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

<a name="decrement-value"></a>

## <a name="decrement-variable"></a>Variabele verlagen

Om te verkleinen of *verlagen* een variabele met een constante waarde, volgt u de stappen voor [verhogen van een variabele](#increment-value) , behalve dat u Zoek en selecteer de **variabelen - variabele verlagen**actie in plaats daarvan. Deze actie werkt alleen met gehele getallen en float-variabelen.

Hier worden de eigenschappen voor de **variabele verlagen** actie:

| Eigenschap | Vereist | Value |  Beschrijving |
|----------|----------|-------|--------------|
| Name | Ja | <*variable-name*> | De naam van de variabele verlagen | 
| Value | Nee | <*increment-value*> | De waarde voor de variabele afneemt. De standaardwaarde is een. <p><p>**Tip**: Hoewel dit optioneel is, moet u deze waarde instellen als een best practice, zodat u altijd de specifieke waarde voor afneemt uw variabele weet. | 
||||| 

Als u van de ontwerpfunctie voor de weergave-editor overschakelt, volgt u de manier waarop de **variabele verlagen** actie wordt weergegeven in het definitie logische app, dat zich in de JSON-indeling.

```json
"actions": {
   "Decrement_variable": {
      "type": "DecrementVariable",
      "inputs": {
         "name": "Count",
         "value": 1
      },
      "runAfter": {}
   }
},
```


<a name="assign-value"></a>

## <a name="set-variable"></a>Variabele instellen

Als u wilt een andere waarde aan een bestaande variabele toewijst, volgt u de stappen voor het [verhogen van een variabele](#increment-value) met uitzondering van die u hebt: 

1. Zoek en selecteer de **variabelen - variabele instellen** actie in plaats daarvan. 

2. Geef de naam van variabele en waarde die u wilt toewijzen. Zowel de nieuwe waarde en de variabele moeten hetzelfde gegevenstype hebben.
De waarde is vereist omdat met deze actie niet een standaardwaarde hebben. 

Hier worden de eigenschappen voor de **variabele instellen** actie:

| Eigenschap | Vereist | Value |  Beschrijving | 
|----------|----------|-------|--------------| 
| Name | Ja | <*variable-name*> | De naam van de variabele te wijzigen | 
| Value | Ja | <*new-value*> | De waarde die u wilt toewijzen van de variabele. Beide moeten hetzelfde gegevenstype hebben. | 
||||| 

> [!NOTE]
> Wijzigen van variabelen in lussen, tenzij u verhogen of afneemt variabelen bent *mogelijk* onverwachte resultaten niet maken omdat lussen parallel of gelijktijdig, standaard uitgevoerd. Voor deze gevallen proberen in te stellen van uw lus om te worden opeenvolgend uitgevoerd. Bijvoorbeeld, als u wilt verwijzen naar de variabele waarde binnen de lus en dezelfde waarde aan het begin en einde van het exemplaar van de lus verwacht, volg deze stappen als u wilt wijzigen hoe de lus wordt uitgevoerd op: 
>
> 1. In de rechterbovenhoek van de lus, kiest u de knop met het weglatingsteken (...) en kies vervolgens **instellingen**.
> 
> 2. Onder **gelijktijdigheidsbeheer**, wijzigt de **overschrijven standaard** instelt op **op**.
>
> 3. Sleep de **graad van parallelle uitvoering** schuifregelaar **1**.

Als u van de ontwerpfunctie voor de weergave-editor overschakelt, volgt u de manier waarop de **variabele instellen** actie wordt weergegeven in het definitie logische app, dat zich in de JSON-indeling. De huidige waarde van de variabele "Aantal" verandert in dit voorbeeld in een andere waarde. 

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "Count",
               "type": "Integer",
               "value": 0
          } ]
      },
      "runAfter": {}
   },
   "Set_variable": {
      "type": "SetVariable",
      "inputs": {
         "name": "Count",
         "value": 100
      },
      "runAfter": {
         "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

<a name="append-value"></a>

## <a name="append-to-variable"></a>Toevoegen aan een variabele

Voor variabelen waarin tekenreeksen of matrices, u kunt invoegen of *append* van een variabele waarde als het laatste item in deze tekenreeksen of matrices. U kunt de stappen voor [verhogen van een variabele](#increment-value) , behalve dat u als te werk in plaats daarvan volgt: 

1. Zoek en selecteer een van deze acties die zijn gebaseerd op of de variabele een tekenreeks of een matrix is: 

   * **Variabelen - toevoegen aan een tekenreeksvariabele**
   * **Variabelen - toevoegen aan een matrixvariabele** 

2. Geef de waarde toe te voegen als het laatste item in de matrix of tekenreeks. 
   Deze waarde is verplicht. 

Hier worden de eigenschappen voor de **toevoegen aan...**  acties:

| Eigenschap | Vereist | Value |  Beschrijving | 
|----------|----------|-------|--------------| 
| Name | Ja | <*variable-name*> | De naam van de variabele te wijzigen | 
| Value | Ja | <*append-value*> | De waarde die u toevoegen wilt, en die elk type kan hebben | 
|||||  

Als u van de ontwerpfunctie voor de weergave-editor overschakelt, volgt u de manier waarop de **toevoegen aan een matrixvariabele** actie wordt weergegeven in het definitie logische app, dat zich in de JSON-indeling.
In dit voorbeeld maakt u een matrixvariabele en voegt een andere waarde als het laatste item in de matrix. Het resultaat is een bijgewerkte variabele die deze matrix bevat: `[1,2,3,"red"]` 

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
            "name": "myArrayVariable",
            "type": "Array",
            "value": [1, 2, 3]
         } ]
      },
      "runAfter": {}
   },
   "Append_to_array_variable": {
      "type": "AppendToArrayVariable",
      "inputs": {
         "name": "myArrayVariable",
         "value": "red"
      },
      "runAfter": {
        "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Logic Apps-connectors](../connectors/apis-list.md)
