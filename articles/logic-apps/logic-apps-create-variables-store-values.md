---
title: Variabelen voor het opslaan van waarden - Azure Logic Apps maken | Microsoft Docs
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
ms.openlocfilehash: 0efce9fbbbd241f335f08bb258b6ba343982fdb9
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299185"
---
# <a name="create-variables-for-saving-and-managing-values-in-azure-logic-apps"></a>Variabelen voor het opslaan en beheren van waarden in Azure Logic Apps maken

Dit artikel laat zien hoe u kunt opslaan en werken met waarden in uw logische app door het maken van variabelen. Bijvoorbeeld, kunt variabelen u het aantal keren dat een lus wordt uitgevoerd. Wanneer iteratie van een matrix of een matrix voor een specifiek item controleren, kunt u een variabele om te verwijzen naar het indexnummer voor elk matrixitem. 

U kunt variabelen voor de gegevenstypen zoals geheel getal, float, Booleaanse waarde, string, matrix en object maken. Nadat u een variabele maakt, kunt u bijvoorbeeld andere taken uitvoeren:

* Ophalen van of naar de variabele waarde verwijzen.
* Vergroten of verkleinen van de variabele in met een constante waarde, ook wel bekend als *verhoging* en *verlagen*.
* Een andere waarde toewijzen aan de variabele.
* Invoegen of *toevoegen* van de variabele waarde als de laatste keer in een tekenreeks of matrix.

Variabelen bestaan en globale alleen binnen het logic app-exemplaar waarin ze zijn gemaakt. Ze bewaren tevens, via elke lus iteraties binnen een logic app-exemplaar. Wanneer u verwijst naar een variabele, gebruikt u de naam van de variabele als het token, niet van de actie op de naam, dit is de gebruikelijke manier om te verwijzen naar de uitvoer van de actie.

Als u een Azure-abonnement nog geen <a href="https://azure.microsoft.com/free/" target="_blank">aanmelden voor een gratis Azure-account</a>. 

## <a name="prerequisites"></a>Vereisten

Wilt u dit artikel, vindt hier u de items die u nodig:

* De logische app waar u een variabele maken 

  Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) als de eerste stap in uw logische app 

  Voordat u de acties voor het maken van en werken met variabelen toevoegen kunt, moet uw logische app beginnen met een trigger.

<a name="create-variable"></a>

## <a name="initialize-variable"></a>Variabele initialiseren

U kunt maken van een variabele en het gegevenstype en de initiële waarde — allemaal in één bewerking in uw logische app declareren. U kunt alleen variabelen op globaal niveau niet binnen de bereiken, voorwaarden en lussen declareren. 

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> of Visual Studio, opent u uw logische app in Logic App Designer. 

   Dit voorbeeld wordt de Azure portal en een logische app met een bestaande trigger.

2. In uw logische app, onder de stap waar u een variabele toevoegen Volg een van deze stappen: 

   * Als u wilt een actie onder de laatste stap hebt toegevoegd, kiest u **nieuwe stap** > **een actie toevoegen**.

     ![actie toevoegen](./media/logic-apps-create-variables-store-values/add-action.png)

   * Als u een actie tussen stappen toevoegen, wilt u de muisaanwijzer over de verbindende pijl zodat het plusteken (+) wordt weergegeven. 
   Kies het plusteken en kies vervolgens **een actie toevoegen**.

3. Voer in het zoekvak 'variabelen' als filter. Selecteer in de lijst met acties **variabelen - initialisatie variabele**.

   ![Selecteer actie](./media/logic-apps-create-variables-store-values/select-initialize-variable-action.png)

4. Geef deze informatie voor de variabele:

   | Eigenschap | Vereist | Waarde |  Beschrijving |
   |----------|----------|-------|--------------|
   | Naam | Ja | <*naam variabele*> | De naam voor de variabele moet worden verhoogd | 
   | Type | Ja | <*variabele-type*> | Het gegevenstype voor de variabele | 
   | Waarde | Nee | <*Start-waarde*> | De beginwaarde van uw variabele <p><p>**Tip**: hoewel optioneel, deze waarde instellen als een best practice zodat u weet de beginwaarde voor de variabele wat. | 
   ||||| 

   ![Variabele initialiseren](./media/logic-apps-create-variables-store-values/initialize-variable.png)

5. Nu doorgaan met de acties die u wilt toevoegen. Wanneer u, klik op de werkbalk ontwerpen bent klaar kiezen **opslaan**.

Als u de ontwerpfunctie naar de code-editor weergeven overschakelt, dit is de manier de **initialiseren variabele** actie binnen uw logische app definitie, in JSON JavaScript Object Notation ()-indeling wordt weergegeven:

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

Hier volgen voorbeelden voor andere variabele typen:

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

*Boole-variabele*

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

## <a name="get-the-variables-value"></a>Waarde van de variabele ophalen

Als u wilt ophalen of verwijst naar de inhoud van een variabele, u kunt ook de [variables() functie](../logic-apps/workflow-definition-language-functions-reference.md#variables) in de ontwerpfunctie voor Logic App en code-editor weergeven.
Wanneer u verwijst naar een variabele, gebruikt u de naam van de variabele als het token, niet van de actie op de naam, dit is de gebruikelijke manier om te verwijzen naar de uitvoer van de actie. 

Bijvoorbeeld, opgehaald van de items van de matrixvariabele in deze expressie [eerder hebt gemaakt in dit artikel](#append-value) met behulp van de **variables()** functie. De **string()** functie retourneert de inhoud van de variabele in de indeling van tekenreeks: `"1, 2, 3, red"`

```json
@{string(variables('myArrayVariable'))}
```

<a name="increment-value"></a>

## <a name="increment-variable"></a>Variabele verhogen 

Om te verhogen of *verhoging* toevoegen van een variabele met een constante waarde de **variabelen - Increment-variabele** aan uw logische app in te grijpen. Deze actie werkt alleen met gehele getallen en float-variabelen.

1. In Logic App Designer onder de stap waar u een bestaande variabele verhogen kiezen **nieuwe stap** > **een actie toevoegen**. 

   Deze logische app heeft bijvoorbeeld al een trigger en een actie die een variabele gemaakt. Dus toevoegen van een nieuwe actie onder de volgende stappen uit:

   ![actie toevoegen](./media/logic-apps-create-variables-store-values/add-increment-variable-action.png)

   Wilt een actie tussen bestaande stappen toevoegen, plaatst u de muis boven de verbindende pijl zodat het plusteken (+) wordt weergegeven. Kies het plusteken en kies vervolgens **een actie toevoegen**.

2. Voer in het zoekvak 'increment variabele' als filter. Selecteer in de lijst met acties **variabelen - Increment-variabele**.

   ![Selecteer de actie 'Increment variabele'](./media/logic-apps-create-variables-store-values/select-increment-variable-action.png)

3. Geef deze informatie voor de variabele en oplopend in stappen:

   | Eigenschap | Vereist | Waarde |  Beschrijving |
   |----------|----------|-------|--------------|
   | Naam | Ja | <*naam variabele*> | De naam voor de variabele moet worden verhoogd | 
   | Waarde | Nee | <*incrementele waarde*> | De waarde die wordt gebruikt voor de variabele en oplopend in stappen. De standaardwaarde is een. <p><p>**Tip**: hoewel optioneel, deze waarde instellen als een best practice zodat u altijd de specifieke waarde weet voor de variabele en oplopend in stappen. | 
   |||| 

   Bijvoorbeeld: 
   
   ![Voorbeeld van een incrementele waarde](./media/logic-apps-create-variables-store-values/increment-variable-action-information.png)

4. Wanneer u, klik op de werkbalk ontwerpen bent klaar kiezen **opslaan**. 

Als u de ontwerpfunctie naar de code-editor weergeven overschakelt, dit is de manier de **verhoging variabele** actie binnen uw logische app definitie, in JSON-indeling wordt weergegeven:

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

## <a name="example-create-loop-counter"></a>Voorbeeld: Lus teller maken

Variabelen worden vaak gebruikt voor het tellen van het aantal keren dat een lus wordt uitgevoerd. Dit voorbeeld toont hoe u maken en gebruiken van variabelen voor deze taak door het maken van een lus waarmee de bijlagen in een e-mailbericht wordt geteld.

1. Maak een lege logische app in de Azure portal. Toevoegen van een trigger die op nieuwe e-mailbericht en eventuele bijlagen controleert. 

   In dit voorbeeld wordt de Outlook van Office 365-trigger voor **wanneer een nieuw e-mailadres binnenkomt**. 
   U kunt deze trigger activeert alleen wanneer het e-mailbericht bijlagen heeft instellen.
   U kunt echter elke connector waarmee wordt gecontroleerd of gebruiken voor nieuwe e-mails met bijlagen, zoals de Outlook.com-connector.

2. Kies in de trigger **geavanceerde opties weergeven**. Als de trigger voor bijlagen controleren en deze bijlagen doorgeven in uw logische app werkstroom, schakelt u **Ja** voor deze eigenschappen:
   
   * **Heeft bijlage** 
   * **Bijlagen opnemen** 

   ![Zoek en bijlagen opnemen](./media/logic-apps-create-variables-store-values/check-include-attachments.png)

3. Voeg de [ **initialiseren variabele** actie](#create-variable). Maak een geheel getal variabele met de naam **aantal** starten met een nul waarde.

   ![Actie voor "Initialize variabele" toevoegen](./media/logic-apps-create-variables-store-values/initialize-variable.png)

4. Om te bladeren naar elke bijlage, Voeg een *voor elk* lus door te kiezen **nieuwe stap** > **meer** > **toevoegen een voor elke**.

   ![Toevoegen van een ' elke' for-lus](./media/logic-apps-create-variables-store-values/add-loop.png)

5. Klik in de lus in de **uitvoer selecteren uit de vorige stappen** vak. Als de lijst met dynamische inhoud wordt weergegeven, selecteert u **bijlagen**. 

   ![Selecteer 'Bijlagen'](./media/logic-apps-create-variables-store-values/select-attachments.png)

   De **bijlagen** veld een matrix met de e-mailbijlagen uit de uitvoer van de trigger van de lus wordt doorgegeven.

6. Selecteer in de lus 'voor elk' **een actie toevoegen**. 

   ![Selecteer 'Een actie toevoegen'](./media/logic-apps-create-variables-store-values/add-action-2.png)

7. Voer in het zoekvak 'increment variabele' als filter. Selecteer in de lijst met acties **variabelen - Increment-variabele**.

   > [!NOTE]
   > Zorg ervoor dat de **verhoging variabele** actie binnen de lus wordt weergegeven. Als de actie wordt weergegeven buiten de lus, sleept u de actie in de lus.

8. In de **verhoging variabele** actie van de **naam** lijst, selecteert de **aantal** variabele. 

   ![Selecteer de variabele 'Aantal'](./media/logic-apps-create-variables-store-values/add-increment-variable-example.png)

9. Toevoegen onder de lus, een actie op die u het aantal bijlagen verzendt. Opnemen in uw actie, de waarde van de **aantal** variabele, bijvoorbeeld: 

   ![Een actie die u resultaten verzendt toevoegen](./media/logic-apps-create-variables-store-values/send-email-results.png)

10. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper. 

### <a name="test-your-logic-app"></a>Uw logische app testen

1. Als uw logische app is niet ingeschakeld op uw logische app-menu kiezen **overzicht**. Kies op de werkbalk **inschakelen**. 

2. Kies op de werkbalk Logic App-ontwerper **uitvoeren**. Deze stap start handmatig uw logische app.

3. Sturen een e-mail met een of meer bijlagen aan het e-mailaccount dat u hebt gebruikt in dit voorbeeld.

   Deze stap wordt geactiveerd voor de logische app trigger maakt en een exemplaar van uw logische app werkstroom wordt uitgevoerd.
   Als gevolg hiervan stuurt de logische app u een bericht of een e-mailbericht dat toont het aantal bijlagen in de e-mail die u hebt verzonden.

Als u de ontwerpfunctie naar de code-editor weergeven overschakelt, dit is de manier die de lus 'voor elk' wordt weergegeven met de **verhoging variabele** actie binnen uw logische app definitie, in JSON-indeling.

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

Om te verlagen of *verlagen* een variabele met een constante waarde, volg de stappen voor [verhogen van een variabele](#increment-value) , behalve dat u Zoek en selecteer de **variabelen - verlagen variabele**actie in plaats daarvan. Deze actie werkt alleen met gehele getallen en float-variabelen.

Hier worden de eigenschappen voor de **verlagen variabele** actie:

| Eigenschap | Vereist | Waarde |  Beschrijving |
|----------|----------|-------|--------------|
| Naam | Ja | <*naam variabele*> | De naam voor de variabele om te verlagen | 
| Waarde | Nee | <*incrementele waarde*> | De waarde voor de variabele afneemt. De standaardwaarde is een. <p><p>**Tip**: hoewel optioneel, deze waarde instellen als een best practice zodat u altijd de specifieke waarde voor afneemt uw variabele weet. | 
||||| 

Als u de ontwerpfunctie naar de code-editor weergeven overschakelt, dit is de manier de **verlagen variabele** actie wordt weergegeven in uw logische app definitie, in JSON-indeling.

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

Volg de stappen voor u een andere waarde toewijzen aan een bestaande variabele door [verhogen van een variabele](#increment-value) behalve die u hebt: 

1. Zoek en selecteer de **variabelen - variabele instellen** actie in plaats daarvan. 

2. Geef de naam van variabele en waarde die u wilt toewijzen. Zowel de nieuwe waarde en de variabele moeten hetzelfde gegevenstype hebben.
De waarde is vereist omdat deze actie heeft geen standaardwaarde. 

Hier worden de eigenschappen voor de **variabele instellen** actie:

| Eigenschap | Vereist | Waarde |  Beschrijving | 
|----------|----------|-------|--------------| 
| Naam | Ja | <*naam variabele*> | De naam voor de variabele te wijzigen | 
| Waarde | Ja | <*nieuwe waarde*> | De waarde die u wilt toewijzen van de variabele. Beide moeten hetzelfde gegevenstype hebben. | 
||||| 

> [!NOTE]
> Wijzigen van variabelen in lussen, tenzij u en oplopend in stappen of afneemt variabelen bent *mogelijk* onverwachte resultaten niet maken omdat lussen parallel of gelijktijdig, standaard uitgevoerd. Probeer de instelling van de lus om te worden opeenvolgend uitgevoerd voor deze aanvragen. Bijvoorbeeld, wanneer u wilt verwijzen naar de variabele waarde binnen de lus en dezelfde waarde aan het begin en einde van dit exemplaar van de lus verwacht, volg deze stappen om te wijzigen hoe de lus wordt uitgevoerd op: 
>
> 1. In de rechterbovenhoek van de lus, kiest u de knop met het weglatingsteken (...) en kies vervolgens **instellingen**.
> 
> 2. Onder **Gelijktijdigheidbeheer**, wijzig de **overschrijven standaard** instelt op **op**.
>
> 3. Sleep de **mate van parallelle uitvoering** schuifregelaar naar **1**.

Als u de ontwerpfunctie naar de code-editor weergeven overschakelt, dit is de manier de **variabele instellen** actie wordt weergegeven in uw logische app definitie, in JSON-indeling. Dit voorbeeld wordt de huidige waarde van de variabele 'Aantal' met een andere waarde. 

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

## <a name="append-to-variable"></a>Toevoegen aan de variabele

Voor variabelen die tekenreeksen of matrices opslaan, kunt u invoegen of *toevoegen* een variabele waarde als het laatste item in deze tekenreeksen of matrices. U kunt de stappen voor [verhogen van een variabele](#increment-value) , behalve dat u als te werk in plaats daarvan volgt: 

1. Zoek en selecteer een van deze acties op basis van of de variabele een tekenreeks of een matrix is: 

  * **Variabelen - toevoegen aan de string-variabele**
  * **Variabelen - toevoegen aan de matrixvariabele** 

2. Geef de waarde om toe te voegen als het laatste item in de tekenreeks of matrix. Deze waarde is verplicht. 

Hier worden de eigenschappen voor de **toevoegen aan...**  acties:

| Eigenschap | Vereist | Waarde |  Beschrijving | 
|----------|----------|-------|--------------| 
| Naam | Ja | <*naam variabele*> | De naam voor de variabele te wijzigen | 
| Waarde | Ja | <*toevoeg-waarde*> | De waarde u toevoegen wilt, die elk type kan hebben | 
|||||  

Als u de ontwerpfunctie naar de code-editor weergeven overschakelt, dit is de manier de **toevoegen aan de matrixvariabele** actie wordt weergegeven in uw logische app definitie, in JSON-indeling.
In dit voorbeeld maakt een matrixvariabele en voegt een andere waarde als het laatste item in de matrix. Het resultaat is een bijgewerkte variabele die deze matrix bevat: `[1,2,3,"red"]` 

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
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Logic Apps-connectors](../connectors/apis-list.md)
