---
title: Bewerkingen uitvoeren op gegevens - Azure Logic Apps | Microsoft Docs
description: Converteren, beheren en manipuleren van gegevens-uitvoer en indelingen in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 07/30/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 7e62986569888ebbcd9f17b4eb4cfb2c70411d4a
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39392080"
---
# <a name="perform-data-operations-in-azure-logic-apps"></a>Gegevensbewerkingen uitvoeren in Azure Logic Apps

Dit artikel wordt beschreven hoe u kunt werken met gegevens in uw logische apps door het toevoegen van acties voor deze taken en meer:

* Maak tabellen vanuit matrices.
* Maken van matrices van andere matrices op basis van een voorwaarde.
* Gebruiksvriendelijke tokens van eigenschappen voor JavaScript Object Notation (JSON)-object maken zodat u deze eigenschappen in uw werkstroom gebruiken kunt.

Als u de actie die u hier niet kunt vinden, kunt u bladeren door de vele verschillende [functies voor het bewerken van gegevens](../logic-apps/workflow-definition-language-functions-reference.md) die Logic Apps biedt. 

Deze tabel bevat een overzicht van de gegevens die bewerkingen u kunt gebruiken en zijn ingedeeld op basis van de bron-gegevenstypen die de operations werken op, maar de beschrijving wordt weergegeven op alfabetische volgorde.

**Matrix-acties** 

Deze acties kunnen u werken met gegevens in matrices.

| Bewerking | Beschrijving | 
|--------|-------------| 
| [**CSV-tabel maken**](#create-csv-table-action) | Een door komma's gescheiden waarden (CSV)-tabel maken van een matrix. | 
| [**HTML-tabel maken**](#create-html-table-action) | Een HTML-tabel maken van een matrix. | 
| [**Matrix filteren**](#filter-array-action) | Maak een subset van de matrix van een matrix op basis van het opgegeven filter of een voorwaarde. | 
| [**join**](#join-action) | Maken van een tekenreeks van de items in een matrix en scheiden van elk item met het opgegeven teken. | 
| [**Selecteer**](#select-action) | Een matrix van de opgegeven eigenschappen voor alle items in een andere matrix maken. | 
||| 

**JSON-acties**

Deze acties kunnen u werken met gegevens in JavaScript Object Notation (JSON)-indeling.

| Bewerking | Beschrijving | 
|--------|-------------| 
| [**Opstellen**](#compose-action) | Een bericht of een tekenreeks, maken van meerdere invoergegevens die verschillende soorten gegevens kunnen hebben. U kunt vervolgens deze tekenreeks als een enkel invoer, in plaats van herhaaldelijk de dezelfde invoer gebruiken. U kunt bijvoorbeeld een enkel JSON-bericht maken van verschillende invoer. | 
| [**JSON parseren**](#parse-json-action) | Gebruiksvriendelijke gegevenstokens aanmaken voor eigenschappen in JSON-inhoud zodat u de eigenschappen eenvoudiger in uw logische apps gebruiken kunt. | 
||| 

Zie voor het maken van meer complexe JSON-transformaties [uitvoeren JSON-transformaties met Liquid sjablonen geavanceerde](../logic-apps/logic-apps-enterprise-integration-liquid-transform.md).

## <a name="prerequisites"></a>Vereisten

Als u wilt de voorbeelden in dit artikel volgen, moet u deze items:

* Een Azure-abonnement. Als u een Azure-abonnement nog geen <a href="https://azure.microsoft.com/free/" target="_blank">zich aanmelden voor een gratis Azure-account</a>.

* De logische app waar u de bewerking voor het werken met gegevens nodig hebt 

  Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) als de eerste stap in uw logische app 

  Gegevensbewerkingen zijn alleen beschikbaar als acties, dus voordat u kunt deze acties beginnen uw logische app met een trigger en eventuele andere vereiste acties vereist voor het maken van de uitvoer die u wilt opnemen.

<a name="compose-action"></a>

## <a name="compose-action"></a>Actie opstellen

Kan een enkele uitvoer, zoals een JSON-object van meerdere invoergegevens, kunt u de **gegevensbewerkingen - opstellen** actie. Uw invoer kunnen verschillende typen hebben, zoals gehele getallen, Booleaanse waarden, matrices, JSON-objecten en alle andere native typen die ondersteuning voor Azure Logic Apps, bijvoorbeeld binaire gegevens en XML. U kunt de uitvoer vervolgens gebruiken in acties die volgen na de **opstellen** actie. De **opstellen** actie kunt u ook opslaan van de dezelfde invoer herhaaldelijk voeren terwijl u de werkstroom van uw logische app bouwen. 

U kunt bijvoorbeeld een JSON-bericht van meerdere variabelen, zoals tekenreeksvariabelen waarin de voornaam en achternaam van mensen en een geheel getal variabele waarin de leeftijd van gebruikers maken. Hier de **opstellen** actie accepteert deze invoer:

`{ "age": <ageVar>, "fullName": "<lastNameVar>, <firstNameVar>" }`

en maakt deze uitvoer:

`{"age":35,"fullName":"Owens,Sophie"}`

Als u een voorbeeld, volg deze stappen met behulp van de ontwerper van logische App. Of, als u liever in het code-editor weergave werkt, kunt u het voorbeeld **opstellen** en **variabele initialiseren** definities van de actie van dit artikel in uw eigen logica-app in de onderliggende werkstroom definitie: [gegevens bewerking-codevoorbeelden - opstellen](../logic-apps/logic-apps-data-operations-code-samples.md#compose-action-example) 

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> of Visual Studio, open uw logische app in Logic App Designer. 

   In dit voorbeeld wordt de Azure-portal en een logische app met een **terugkeerpatroon** trigger en diverse **variabele initialiseren** acties. 
   Deze acties worden ingesteld voor het maken van twee tekenreeksvariabelen die en een variabele geheel getal zijn. Wanneer u later de logische app test, kunt u uw app handmatig uitvoeren zonder te wachten op de trigger wordt geactiveerd.

   ![Eerste voorbeeld logische app](./media/logic-apps-perform-data-operations/sample-starting-logic-app-compose-action.png)

2. In uw logische app waar u wilt maken van de uitvoer, een van deze stappen te volgen: 

   * Als u wilt een actie toevoegen onder de laatste stap, kies **nieuwe stap** > **een actie toevoegen**.

     ![Actie toevoegen](./media/logic-apps-perform-data-operations/add-compose-action.png)

   * Als u wilt toevoegen een actie tussen fasen, u de muisaanwijzer over het maken van verbinding pijl, zodat het plusteken (+) wordt weergegeven. 
   Kies het plusteken en selecteer vervolgens **een actie toevoegen**.

3. Typ 'opstellen' als filter in het zoekvak. Selecteer in de lijst met acties met deze actie: **opstellen**

   ![Selecteer de actie 'Opstellen'](./media/logic-apps-perform-data-operations/select-compose-action.png)

4. In de **invoer** geeft u de invoer die u wilt gebruiken voor het maken van de uitvoer. 

   Voor dit voorbeeld, wanneer u klikt u in de **invoer** vak de lijst met dynamische inhoud wordt weergegeven, zodat u de eerder gemaakte variabelen kunt selecteren:

   ![Invoer voor het opstellen van selecteren](./media/logic-apps-perform-data-operations/configure-compose-action.png)

   Hier volgt de voltooide voorbeeld **opstellen** actie: 

   ![Actie voltooid 'Opstellen'](./media/logic-apps-perform-data-operations/finished-compose-action.png)

5. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

Zie voor meer informatie over deze actie in de onderliggende werkstroomdefinitie, de [actie opstellen](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action). 

### <a name="test-your-logic-app"></a>Uw logische app testen

Om te bevestigen of de **opstellen** actie wordt gemaakt van de verwachte resultaten, Stuur uzelf een melding met de uitvoer van de **opstellen** actie.

1. In uw logische app, een actie toevoegen die u de resultaten van verzenden kunt de **opstellen** actie.

2. In deze actie, klikt u op elke locatie die u wilt dat de resultaten worden weergegeven. Wanneer de lijst met dynamische inhoud wordt geopend, onder de **opstellen** actie, selecteer **uitvoer**. 

   In dit voorbeeld wordt de **Office 365 Outlook - een e-mail verzenden** actie en bevat de **uitvoer** velden in de hoofdtekst van het e-mailbericht en het onderwerp:

   ![De velden "Uitvoeren" in de actie 'Een e-mail verzenden'](./media/logic-apps-perform-data-operations/send-email-compose-action.png)

3. Nu uitvoeren uw logische app handmatig. Kies op de werkbalk van de ontwerper **uitvoeren**. 

   Op basis van de e-mailconnector die u hebt gebruikt, zijn de resultaten die u krijgt hier:

   ![Stuur een e-mail met resultaten van de actie 'Opstellen'](./media/logic-apps-perform-data-operations/compose-email-results.png)

<a name="create-csv-table-action"></a>

## <a name="create-csv-table-action"></a>Actie CSV-tabel maken

Gebruik voor het maken van een tabel met door komma's gescheiden waarden (CSV) met de eigenschappen en waarden van JavaScript Object Notation (JSON)-objecten in een matrix de **gegevensbewerkingen - CSV-tabel maken** actie. U kunt vervolgens de resulterende tabel in de acties die volgen op de **CSV-tabel maken** actie. 

Als u liever in het code-editor weergave werkt, kunt u het voorbeeld **CSV-tabel maken** en **variabele initialiseren** definities van de actie van dit artikel in uw eigen logica-app in de onderliggende werkstroom definitie: [gegevens bewerking codevoorbeelden - CSV-tabel maken](../logic-apps/logic-apps-data-operations-code-samples.md#create-csv-table-action-example) 

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> of Visual Studio, open uw logische app in Logic App Designer. 

   In dit voorbeeld wordt de Azure-portal en een logische app met een **terugkeerpatroon** trigger en een **variabele initialiseren** actie. 
   De actie is ingesteld voor het maken van een variabele waarvan de eerste waarde is een matrix met enkele eigenschappen en waarden in de JSON-indeling. 
   Wanneer u later de logische app test, kunt u uw app handmatig uitvoeren zonder te wachten op de trigger wordt geactiveerd.

   ![Eerste voorbeeld logische app](./media/logic-apps-perform-data-operations/sample-starting-logic-app-create-table-action.png)

2. In uw logische app waar u de CSV-tabel maken, een van deze stappen te volgen: 

   * Als u wilt een actie toevoegen onder de laatste stap, kies **nieuwe stap** > **een actie toevoegen**.

     ![Actie toevoegen](./media/logic-apps-perform-data-operations/add-create-table-action.png)

   * Als u wilt toevoegen een actie tussen fasen, u de muisaanwijzer over het maken van verbinding pijl, zodat het plusteken (+) wordt weergegeven. 
   Kies het plusteken en selecteer vervolgens **een actie toevoegen**.

3. Voer 'csv-tabel maken' als filter in het zoekvak in. Selecteer in de lijst met acties met deze actie: **CSV-tabel maken**

   ![Selecteer ' maken CSV tabel "actie](./media/logic-apps-perform-data-operations/select-create-csv-table-action.png)

4. In de **van** Geef de ingevoerde matrix of expressie die u wilt gebruiken voor het maken van de tabel. 

   Voor dit voorbeeld, wanneer u klikt u in de **van** vak de lijst met dynamische inhoud wordt weergegeven, zodat u kunt de eerder gemaakte variabele selecteren:

   ![Matrixuitvoer voor het maken van CSV-tabel selecteren](./media/logic-apps-perform-data-operations/configure-create-csv-table-action.png)

   Hier volgt de voltooide voorbeeld **CSV-tabel maken** actie: 

   ![Voltooid ' maken CSV tabel "actie](./media/logic-apps-perform-data-operations/finished-create-csv-table-action.png)

   Standaard maakt deze actie automatisch de kolommen op basis van de matrixitems. 
   Kies voor het handmatig maken van de kolomkoppen en de waarden, **geavanceerde opties weergeven**. 
   Wijzigen zodat alleen de aangepaste waarden **kolommen** naar **aangepaste**. 
   Als u aangepaste kolomkoppen te bieden, wilt wijzigen **bestaan uit kopteksten** naar **Ja**. 

   > [!TIP]
   > Gebruik voor het gebruiksvriendelijke tokens voor de eigenschappen in JSON-objecten maken zodat u deze eigenschappen als invoer selecteren kunt de [JSON parseren](#parse-json-action) voordat de **CSV-tabel maken** actie.

5. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

Zie voor meer informatie over deze actie in de onderliggende werkstroomdefinitie, de [tabel actie](../logic-apps/logic-apps-workflow-actions-triggers.md#table-action).

### <a name="test-your-logic-app"></a>Uw logische app testen

Om te bevestigen of de **CSV-tabel maken** actie wordt gemaakt van de verwachte resultaten, Stuur uzelf een melding met de uitvoer van de **CSV-tabel maken** actie.

1. In uw logische app, een actie toevoegen die u de resultaten van verzenden kunt de **CSV-tabel maken** actie.

2. In deze actie, klikt u op elke locatie die u wilt dat de resultaten worden weergegeven. Wanneer de lijst met dynamische inhoud wordt geopend, onder de **CSV-tabel maken** actie, selecteer **uitvoer**. 

   In dit voorbeeld wordt de **Office 365 Outlook - een e-mail verzenden** actie en bevat de **uitvoer** veld in de hoofdtekst van de e-mail:

   ![De velden "Uitvoeren" in de actie 'Een e-mail verzenden'](./media/logic-apps-perform-data-operations/send-email-create-csv-table-action.png)

3. Nu uitvoeren uw logische app handmatig. Kies op de werkbalk van de ontwerper **uitvoeren**. 

   Op basis van de e-mailconnector die u hebt gebruikt, zijn de resultaten die u krijgt hier:

   ![Een e-mail met ' maken CSV tabel "actie resultaten](./media/logic-apps-perform-data-operations/create-csv-table-email-results.png)

<a name="create-html-table-action"></a>

## <a name="create-html-table-action"></a>Actie voor HTML-tabel maken

Gebruik voor het maken van een HTML-tabel die de eigenschappen en waarden van JavaScript Object Notation (JSON)-objecten in een matrix heeft de **gegevensbewerkingen - HTML-tabel maken** actie. U kunt vervolgens de resulterende tabel in de acties die volgen op de **HTML-tabel maken** actie.

Als u liever in het code-editor weergave werkt, kunt u het voorbeeld **HTML-tabel maken** en **variabele initialiseren** definities van de actie van dit artikel in uw eigen logica-app in de onderliggende werkstroom definitie: [gegevens bewerking codevoorbeelden - HTML-tabel maken](../logic-apps/logic-apps-data-operations-code-samples.md#create-html-table-action-example) 

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> of Visual Studio, open uw logische app in Logic App Designer. 

   In dit voorbeeld wordt de Azure-portal en een logische app met een **terugkeerpatroon** trigger en een **variabele initialiseren** actie. 
   De actie is ingesteld voor het maken van een variabele waarvan de eerste waarde is een matrix met enkele eigenschappen en waarden in de JSON-indeling. 
   Wanneer u later de logische app test, kunt u uw app handmatig uitvoeren zonder te wachten op de trigger wordt geactiveerd.

   ![Eerste voorbeeld logische app](./media/logic-apps-perform-data-operations/sample-starting-logic-app-create-table-action.png)

2. In uw logische app waar u wilt maken van een HTML-tabel, een van deze stappen te volgen: 

   * Als u wilt een actie toevoegen onder de laatste stap, kies **nieuwe stap** > **een actie toevoegen**.

     ![Actie toevoegen](./media/logic-apps-perform-data-operations/add-create-table-action.png)

   * Als u wilt toevoegen een actie tussen fasen, u de muisaanwijzer over het maken van verbinding pijl, zodat het plusteken (+) wordt weergegeven. 
   Kies het plusteken en selecteer vervolgens **een actie toevoegen**.

3. Voer 'html-tabel maken' als filter in het zoekvak in. Selecteer in de lijst met acties met deze actie: **HTML-tabel maken**

   ![Selecteer de actie 'Maken HTML-tabel'](./media/logic-apps-perform-data-operations/select-create-html-table-action.png)

4. In de **van** Geef de ingevoerde matrix of expressie die u wilt gebruiken voor het maken van de tabel. 

   Voor dit voorbeeld, wanneer u klikt u in de **van** vak de lijst met dynamische inhoud wordt weergegeven, zodat u kunt de eerder gemaakte variabele selecteren:

   ![Matrixuitvoer voor het maken van HTML-tabel selecteren](./media/logic-apps-perform-data-operations/configure-create-html-table-action.png)

   Hier volgt de voltooide voorbeeld **HTML-tabel maken** actie: 

   ![Voltooide "Create HTML-tabel"-actie](./media/logic-apps-perform-data-operations/finished-create-html-table-action.png)

   Standaard maakt deze actie automatisch de kolommen op basis van de matrixitems. 
   Kies voor het handmatig maken van de kolomkoppen en de waarden, **geavanceerde opties weergeven**. 
   Wijzigen zodat alleen de aangepaste waarden **kolommen** naar **aangepaste**. 
   Als u aangepaste kolomkoppen te bieden, wilt wijzigen **bestaan uit kopteksten** naar **Ja**. 

   > [!TIP]
   > Gebruik voor het gebruiksvriendelijke tokens voor de eigenschappen in JSON-objecten maken zodat u deze eigenschappen als invoer selecteren kunt de [JSON parseren](#parse-json-action) voordat de **HTML-tabel maken** actie.

5. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

Zie voor meer informatie over deze actie in de onderliggende werkstroomdefinitie, de [tabel actie](../logic-apps/logic-apps-workflow-actions-triggers.md#table-action).

### <a name="test-your-logic-app"></a>Uw logische app testen

Om te bevestigen of de **HTML-tabel maken** actie wordt gemaakt van de verwachte resultaten, Stuur uzelf een melding met de uitvoer van de **HTML-tabel maken** actie.

1. In uw logische app, een actie toevoegen die u de resultaten van verzenden kunt de **HTML-tabel maken** actie.

2. In deze actie, klikt u op elke locatie die u wilt dat de resultaten worden weergegeven. Wanneer de lijst met dynamische inhoud wordt geopend, onder de **HTML-tabel maken** actie, selecteer **uitvoer**. 

   In dit voorbeeld wordt de **Office 365 Outlook - een e-mail verzenden** actie en bevat de **uitvoer** veld in de hoofdtekst van de e-mail:

   ![De velden "Uitvoeren" in de actie 'Een e-mail verzenden'](./media/logic-apps-perform-data-operations/send-email-create-html-table-action.png)
   
   > [!NOTE]
   > Wanneer de HTML-tabel uitvoer in een e-mailactie, zorg ervoor dat u in te stellen de **Is HTML** eigenschap **Ja** in het e-mailbericht actie de geavanceerde opties. Op die manier kunnen de e-mailactie correct wordt de HTML-tabel opgemaakt.

3. Nu uitvoeren uw logische app handmatig. Kies op de werkbalk van de ontwerper **uitvoeren**. 

   Op basis van de e-mailconnector die u hebt gebruikt, zijn de resultaten die u krijgt hier:

   ![Een e-mail met 'Maken HTML-tabel' actie resultaten](./media/logic-apps-perform-data-operations/create-html-table-email-results.png)

<a name="filter-array-action"></a>

## <a name="filter-array-action"></a>Actie matrix filteren

Gebruik voor het maken van een kleinere matrix met items die voldoen aan bepaalde criteria voldoen, van een bestaande matrix de **gegevensbewerkingen - matrix filteren** actie. Vervolgens kunt u de gefilterde matrix gebruiken in acties die volgen na de **matrix filteren** actie. 

> [!NOTE]
> Filtertekst die u in de voorwaarde is hoofdlettergevoelig. Deze actie kan niet ook de indeling of onderdelen van de items in de matrix wijzigen. 
> 
> Voor bewerkingen voor het gebruik van de matrixuitvoer van de **matrix filteren** actie, deze acties matrices moeten accepteert als invoer, of mogelijk hebt u de uitvoermatrix omzetten in een andere indeling die compatibel. 

Als u liever in het code-editor weergave werkt, kunt u het voorbeeld **matrix filteren** en **variabele initialiseren** definities van de actie van dit artikel in uw eigen logica-app in de onderliggende werkstroom definitie: [gegevens bewerking codevoorbeelden - matrix filteren](../logic-apps/logic-apps-data-operations-code-samples.md#filter-array-action-example) 

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> of Visual Studio, open uw logische app in Logic App Designer. 

   In dit voorbeeld wordt de Azure-portal en een logische app met een **terugkeerpatroon** trigger en een **variabele initialiseren** actie. 
   De actie is ingesteld voor het maken van een variabele waarvan de eerste waarde is een matrix met een voorbeeld van gehele getallen. Wanneer u later de logische app test, kunt u uw app handmatig uitvoeren zonder te wachten op de trigger wordt geactiveerd.

   > [!NOTE]
   > Hoewel dit voorbeeld wordt een eenvoudige integerArray gebruikt, kan deze actie is vooral handig voor JSON-object matrices waar u kunt filteren op basis van de eigenschappen en waarden van de objecten.

   ![Eerste voorbeeld logische app](./media/logic-apps-perform-data-operations/sample-starting-logic-app-filter-array-action.png)

2. In uw logische app waar u de gefilterde matrix maken, een van deze stappen te volgen: 

   * Als u wilt een actie toevoegen onder de laatste stap, kies **nieuwe stap** > **een actie toevoegen**.

     ![Actie toevoegen](./media/logic-apps-perform-data-operations/add-filter-array-action.png)

   * Als u wilt toevoegen een actie tussen fasen, u de muisaanwijzer over het maken van verbinding pijl, zodat het plusteken (+) wordt weergegeven. 
   Kies het plusteken en selecteer vervolgens **een actie toevoegen**.

3. Typ 'matrix filteren' als filter in het zoekvak. Selecteer in de lijst met acties met deze actie: **matrix filteren**

   ![Selecteer de actie 'Matrix filteren'](./media/logic-apps-perform-data-operations/select-filter-array-action.png)

4. In de **van** Geef de ingevoerde matrix of expressie die u wilt filteren. 

   Voor dit voorbeeld, wanneer u klikt u in de **van** vak de lijst met dynamische inhoud wordt weergegeven, zodat u kunt de eerder gemaakte variabele selecteren:

   ![Matrixuitvoer voor het maken van gefilterde matrix selecteren](./media/logic-apps-perform-data-operations/configure-filter-array-action.png)

5. Geef op de matrixitems wilt vergelijken, selecteert u de vergelijkingsoperator voor de voorwaarde, en geef de vergelijkingswaarde.

   In dit voorbeeld wordt de **RS** functie voor elk item in de matrix tijdens het verkrijgen van toegang tot de **matrix filteren** actie zoekopdrachten voor matrix items waarvan de waarde is groter dan 1:
   
   ![Actie 'Matrix filteren' is voltooid](./media/logic-apps-perform-data-operations/finished-filter-array-action.png)

6. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

Zie voor meer informatie over deze actie in het definitie van de onderliggende werkstroom [queryactie](../logic-apps/logic-apps-workflow-actions-triggers.md#query-action).

### <a name="test-your-logic-app"></a>Uw logische app testen

Om te bevestigen of **matrix filteren** actie wordt gemaakt van de verwachte resultaten, Stuur uzelf een melding met de uitvoer van de **matrix filteren** actie.

1. In uw logische app, een actie toevoegen die u de resultaten van verzenden kunt de **matrix filteren** actie.

2. In deze actie, klikt u op elke locatie die u wilt dat de resultaten worden weergegeven. Wanneer de lijst met dynamische inhoud wordt geopend, kiest u **expressie**. Aan de matrixuitvoer van de **matrix filteren** actie, voer deze expressie met de **matrix filteren** de naam van de actie:

   ```
   @actionBody('Filter_array')
   ```

   In dit voorbeeld wordt de **Office 365 Outlook - een e-mail verzenden** actie en bevat de uitvoer van de **actionBody('Filter_array')** expressie in de hoofdtekst van de e-mail:

   ![Actie wordt de uitvoer met de actie 'Een e-mail verzenden'](./media/logic-apps-perform-data-operations/send-email-filter-array-action.png)

3. Nu uitvoeren uw logische app handmatig. Kies op de werkbalk van de ontwerper **uitvoeren**. 

   Op basis van de e-mailconnector die u hebt gebruikt, zijn de resultaten die u krijgt hier:

   ![Stuur een e-mail met resultaten van de actie 'Matrix filteren'](./media/logic-apps-perform-data-operations/filter-array-email-results.png)

<a name="join-action"></a>

## <a name="join-action"></a>Actie toevoegen

Voor het maken van een tekenreeks is die alle items uit een matrix heeft en deze items te scheiden met een specifiek scheidingsteken, gebruikt u de **gegevensbewerkingen - lid worden** actie. U kunt vervolgens de tekenreeks in de acties die volgen na de **Join** actie.

Als u liever in het code-editor weergave werkt, kunt u het voorbeeld **Join** en **variabele initialiseren** definities van de actie van dit artikel in uw eigen logica-app in de onderliggende werkstroom-definitie: [ Voorbeelden van gegevens-bewerking code - Join](../logic-apps/logic-apps-data-operations-code-samples.md#join-action-example) 

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> of Visual Studio, open uw logische app in Logic App Designer. 

   In dit voorbeeld wordt de Azure-portal en een logische app met een **terugkeerpatroon** trigger en een **variabele initialiseren** actie. 
   Deze actie is ingesteld voor het maken van een variabele waarvan de eerste waarde is een matrix met een voorbeeld van gehele getallen. 
   Wanneer u uw logische app hoger test, kunt u uw app handmatig uitvoeren zonder te wachten op de trigger wordt geactiveerd.

   ![Eerste voorbeeld logische app](./media/logic-apps-perform-data-operations/sample-starting-logic-app-join-action.png)

2. In uw logische app waar u wilt maken van de tekenreeks in een matrix, een van deze stappen te volgen: 

   * Als u wilt een actie toevoegen onder de laatste stap, kies **nieuwe stap** > **een actie toevoegen**.

     ![Actie toevoegen](./media/logic-apps-perform-data-operations/add-join-action.png)

   * Als u wilt toevoegen een actie tussen fasen, u de muisaanwijzer over het maken van verbinding pijl, zodat het plusteken (+) wordt weergegeven. 
   Kies het plusteken en selecteer vervolgens **een actie toevoegen**.

3. Typ 'koppelen' als filter in het zoekvak. Selecteer in de lijst met acties met deze actie: **toevoegen**

   !["Gegevens Operations – Join" actie selecteren](./media/logic-apps-perform-data-operations/select-join-action.png)

4. In de **van** geeft u de matrix met de items die u wilt toevoegen als een tekenreeks. 

   Voor dit voorbeeld, wanneer u klikt u in de **van** vak, de lijst met dynamische inhoud die wordt weergegeven, zodat u kunt de eerder gemaakte variabele selecteren:  

   ![Matrixuitvoer voor het maken van de tekenreeks selecteren](./media/logic-apps-perform-data-operations/configure-join-action.png)

5. In de **ontmoet** voert u het teken dat u wilt gebruiken voor het scheiden van elk matrixitem. 

   Dit voorbeeld wordt een dubbele punt (:) als het scheidingsteken.

   ![Geef het scheidingsteken](./media/logic-apps-perform-data-operations/finished-join-action.png)

6. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

Zie voor meer informatie over deze actie in de onderliggende werkstroomdefinitie, de [Join actie](../logic-apps/logic-apps-workflow-actions-triggers.md#join-action).

### <a name="test-your-logic-app"></a>Uw logische app testen

Om te bevestigen of de **Join** actie wordt gemaakt van de verwachte resultaten, Stuur uzelf een melding met de uitvoer van de **Join** actie. 

1. In uw logische app, een actie toevoegen die u de resultaten van verzenden kunt de **Join** actie.

2. In deze actie, klikt u op elke locatie die u wilt dat de resultaten worden weergegeven. Wanneer de lijst met dynamische inhoud wordt geopend, onder de **Join** actie, selecteer **uitvoer**. 

   In dit voorbeeld wordt de **Office 365 Outlook - een e-mail verzenden** actie en bevat de **uitvoer** veld in de hoofdtekst van de e-mail:

   ![De velden "Uitvoeren" in de actie 'Een e-mail verzenden'](./media/logic-apps-perform-data-operations/send-email-join-action.png)

3. Nu uitvoeren uw logische app handmatig. Kies op de werkbalk van de ontwerper **uitvoeren**. 

   Op basis van de e-mailconnector die u hebt gebruikt, zijn de resultaten die u krijgt hier:

   ![Een e-mail met 'Koppelen' actie resultaten](./media/logic-apps-perform-data-operations/join-email-results.png)

<a name="parse-json-action"></a>

## <a name="parse-json-action"></a>De actie JSON parseren

Eigenschappen van verwijzing of toegang in JavaScript Object Notation (JSON) inhoud, u gebruiksvriendelijke velden of -tokens voor deze eigenschappen kunt maken met behulp van de **gegevensbewerkingen - JSON parseren** actie.
Op die manier kunt u deze eigenschappen uit de lijst met dynamische inhoud wanneer u invoer voor uw logische app opgeeft. Voor deze actie kunt u bieden een JSON-schema of een JSON-schema genereren van uw voorbeeld-JSON-inhoud of de nettolading.

Als u liever in het code-editor weergave werkt, kunt u het voorbeeld **JSON parseren** en **variabele initialiseren** definities van de actie van dit artikel in uw eigen logica-app in de onderliggende werkstroom-definitie : [Gegevens bewerking codevoorbeelden - JSON parseren](../logic-apps/logic-apps-data-operations-code-samples.md#parse-json-action-example) 

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> of Visual Studio, open uw logische app in Logic App Designer. 

   In dit voorbeeld wordt de Azure-portal en een logische app met een **terugkeerpatroon** trigger en een **variabele initialiseren** actie. 
   De actie is ingesteld voor het maken van een variabele waarvan de eerste waarde is een JSON-object met eigenschappen en waarden. 
   Wanneer u later de logische app test, kunt u uw app handmatig uitvoeren zonder te wachten op de trigger wordt geactiveerd.

   ![Eerste voorbeeld logische app](./media/logic-apps-perform-data-operations/sample-starting-logic-app-parse-json-action.png)

2. In uw logische app waar u de inhoud van de JSON parseren, een van deze stappen te volgen: 

   * Als u wilt een actie toevoegen onder de laatste stap, kies **nieuwe stap** > **een actie toevoegen**.

     ![Actie toevoegen](./media/logic-apps-perform-data-operations/add-parse-json-action.png)

   * Als u wilt toevoegen een actie tussen fasen, u de muisaanwijzer over het maken van verbinding pijl, zodat het plusteken (+) wordt weergegeven. 
   Kies het plusteken en selecteer vervolgens **een actie toevoegen**.

3. Voer 'json parseren' als filter in het zoekvak. Selecteer in de lijst met acties met deze actie: **JSON parseren**

   ![Selecteer de actie 'JSON parseren'](./media/logic-apps-perform-data-operations/select-parse-json-action.png)

4. In de **inhoud** geeft u de JSON-inhoud die u wilt parseren. 

   Voor dit voorbeeld, wanneer u klikt u in de **inhoud** vak de lijst met dynamische inhoud wordt weergegeven, zodat u kunt de eerder gemaakte variabele selecteren:

   ![Selecteer JSON-object voor de actie JSON parseren](./media/logic-apps-perform-data-operations/configure-parse-json-action.png)

5. Voer in het JSON-schema dat beschrijft de JSON-inhoud dat u bent bij het parseren. 

   In dit voorbeeld wordt hier het JSON-schema:

   ![Geef op JSON-schema voor de JSON-object dat u wilt parseren](./media/logic-apps-perform-data-operations/provide-schema-parse-json-action.png)

   Als u het schema niet hebt, kunt u dat schema genereren van de JSON-inhoud of *nettolading*, u bent bij het parseren. 
   
   1. In de **JSON parseren** actie, selecteer **voorbeeldnettolading gebruiken voor het genereren van schema**.

   2. Onder **typt of plakt u een voorbeeld-JSON-nettolading**, geef de JSON-inhoud en kies vervolgens **gedaan**.

      ![Voer de JSON-inhoud voor het genereren van het schema](./media/logic-apps-perform-data-operations/generate-schema-parse-json-action.png)

6. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

Zie voor meer informatie over deze actie in het definitie van de onderliggende werkstroom [actie JSON parseren](../logic-apps/logic-apps-workflow-actions-triggers.md).

### <a name="test-your-logic-app"></a>Uw logische app testen

Om te bevestigen of de **JSON parseren** actie wordt gemaakt van de verwachte resultaten, Stuur uzelf een melding met de uitvoer van de **JSON parseren** actie.

1. In uw logische app, een actie toevoegen die u de resultaten van verzenden kunt de **JSON parseren** actie.

2. In deze actie, klikt u op elke locatie die u wilt dat de resultaten worden weergegeven. Wanneer de lijst met dynamische inhoud wordt geopend, onder de **JSON parseren** actie, kunt u nu de eigenschappen van de geparseerde JSON-inhoud selecteren.

   In dit voorbeeld wordt de **Office 365 Outlook - een e-mail verzenden** actie en bevat de **FirstName**, **LastName**, en **e** velden in de hoofdtekst van het e-mailadres:

   ![JSON-eigenschappen in de actie 'Een e-mail verzenden'](./media/logic-apps-perform-data-operations/send-email-parse-json-action.png)

   Hier volgt de voltooide e-mailactie:

   ![Voltooide e-mailactie](./media/logic-apps-perform-data-operations/send-email-parse-json-action-2.png)

3. Nu uitvoeren uw logische app handmatig. Kies op de werkbalk van de ontwerper **uitvoeren**. 

   Op basis van de e-mailconnector die u hebt gebruikt, zijn de resultaten die u krijgt hier:

   ![Een e-mail met 'Koppelen' actie resultaten](./media/logic-apps-perform-data-operations/parse-json-email-results.png)

<a name="select-action"></a>

## <a name="select-action"></a>Actie selecteren

Gebruik voor het maken van een matrix met JSON-objecten die zijn gebouwd op basis van waarden in een bestaande matrix de **gegevensbewerkingen - Selecteer** actie. U kunt bijvoorbeeld een JSON-object voor elke waarde in een matrix met gehele getallen maken door de eigenschappen die elk JSON-object moet hebben en de waarden in de bronmatrix toewijzen aan deze eigenschappen op te geven. En hoewel u de onderdelen in de JSON-objecten wijzigt kunt, heeft de uitvoermatrix altijd hetzelfde aantal items als de bronmatrix.

> [!NOTE]
> Voor bewerkingen voor het gebruik van de matrixuitvoer van de **Selecteer** actie, deze acties matrices moeten accepteert als invoer, of mogelijk hebt u de uitvoermatrix omzetten in een andere indeling die compatibel. 

Als u liever in het code-editor weergave werkt, kunt u het voorbeeld **Selecteer** en **variabele initialiseren** definities van de actie van dit artikel in uw eigen logica-app in de onderliggende werkstroom-definitie: [Gegevens bewerking-codevoorbeelden - selecteren](../logic-apps/logic-apps-data-operations-code-samples.md#select-action-example) 

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> of Visual Studio, open uw logische app in Logic App Designer. 

   In dit voorbeeld wordt de Azure-portal en een logische app met een **terugkeerpatroon** trigger en een **variabele initialiseren** actie. 
   De actie is ingesteld voor het maken van een variabele waarvan de eerste waarde is een matrix met een voorbeeld van gehele getallen. 
   Wanneer u later de logische app test, kunt u uw app handmatig uitvoeren zonder te wachten op de trigger wordt geactiveerd.

   ![Eerste voorbeeld logische app](./media/logic-apps-perform-data-operations/sample-starting-logic-app-select-action.png)

2. In uw logische app waar u wilt maken van de matrix, een van deze stappen te volgen: 

   * Als u wilt een actie toevoegen onder de laatste stap, kies **nieuwe stap** > **een actie toevoegen**.

     ![Actie toevoegen](./media/logic-apps-perform-data-operations/add-select-action.png)

   * Als u wilt toevoegen een actie tussen fasen, u de muisaanwijzer over het maken van verbinding pijl, zodat het plusteken (+) wordt weergegeven. 
   Kies het plusteken en selecteer vervolgens **een actie toevoegen**.

3. Typ 'selecteren' als filter in het zoekvak. Selecteer in de lijst met acties met deze actie: **selecteren**

   ![Selecteer de actie 'Selecteren'](./media/logic-apps-perform-data-operations/select-select-action.png)

4. In de **van** , geeft u de bronmatrix die u wilt.

   Voor dit voorbeeld, wanneer u klikt u in de **van** vak de lijst met dynamische inhoud wordt weergegeven, zodat u kunt de eerder gemaakte variabele selecteren:

   ![Matrix van de bron selecteren voor de actie selecteren](./media/logic-apps-perform-data-operations/configure-select-action.png)

5. In de **kaart** linkerkolom van het vak, geef de naam van de eigenschap die u wilt toewijzen van elke waarde in de bronmatrix. Geef een expressie waarmee de waarde die u wilt toewijzen van de eigenschap in de rechterkolom.

   In dit voorbeeld geeft 'Product_ID' als de naam van de eigenschap aan elke waarde in de matrix met gehele getallen toewijzen met behulp van de **RS** functie in een expressie die toegang heeft tot elk matrixitem. 

   ![Geef de JSON-objecteigenschap en waarden voor de matrix die u wilt maken](./media/logic-apps-perform-data-operations/configure-select-action-2.png)

   Dit is de actie is voltooid:

   ![Selecteer actie voltooid](./media/logic-apps-perform-data-operations/finished-select-action.png)

6. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

Zie voor meer informatie over deze actie in het definitie van de onderliggende werkstroom [actie selecteren](../logic-apps/logic-apps-workflow-actions-triggers.md).

### <a name="test-your-logic-app"></a>Uw logische app testen

Om te bevestigen of de **Selecteer** actie wordt gemaakt van de verwachte resultaten, Stuur uzelf een melding met de uitvoer van de **Selecteer** actie.

1. In uw logische app, een actie toevoegen die u de resultaten van verzenden kunt de **Selecteer** actie.

2. In deze actie, klikt u op elke locatie die u wilt dat de resultaten worden weergegeven. Wanneer de lijst met dynamische inhoud wordt geopend, kiest u **expressie**. Om op te halen van de matrixuitvoer van de **Selecteer** actie, voer deze expressie met de **Selecteer** de naam van de actie:

   ```
   @actionBody('Select')
   ```

   In dit voorbeeld wordt de **Office 365 Outlook - een e-mail verzenden** actie en bevat de uitvoer van de **actionBody('Select')** expressie in de hoofdtekst van de e-mail:

   ![Actie wordt de uitvoer met de actie 'Een e-mail verzenden'](./media/logic-apps-perform-data-operations/send-email-select-action.png)

3. Nu uitvoeren uw logische app handmatig. Kies op de werkbalk van de ontwerper **uitvoeren**. 

   Op basis van de e-mailconnector die u hebt gebruikt, zijn de resultaten die u krijgt hier:

   ![Een e-mail met de resultaten van de actie 'Selecteren'](./media/logic-apps-perform-data-operations/select-email-results.png)

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Logic Apps-connectors](../connectors/apis-list.md)
