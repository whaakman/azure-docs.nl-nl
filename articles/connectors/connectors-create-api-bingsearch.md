---
title: Verbinding maken met Bing zoeken - Azure Logic Apps
description: Nieuws met Bing Search REST API's en Azure Logic Apps zoeken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 05/21/2018
tags: connectors
ms.openlocfilehash: 7146e59eabf9e30fa263f957f1c546414ad0fe26
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313546"
---
# <a name="find-news-with-bing-search-and-azure-logic-apps"></a>Nieuwsartikelen met zoeken in Bing en Azure Logic Apps zoeken

Dit artikel wordt beschreven hoe u kunt vinden, nieuws, video's en andere items via Bing zoeken uit in een logische app met de Bing zoeken-connector. Op die manier kunt u logische apps die taken automatiseren en werkstromen voor de verwerking van zoekresultaten en maak deze items beschikbaar voor andere acties. 

U kunt bijvoorbeeld zoeken naar nieuwsitems op basis van zoekcriteria en Twitter plaatsen van deze items zoals tweets in uw Twitter-feed hebt.

Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Quick Start: Maak uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Connector-specifieke technische informatie, Zie de <a href="https://docs.microsoft.com/connectors/bingsearch/" target="blank">documentatie voor zoeken in Bing-connector</a>.

## <a name="prerequisites"></a>Vereisten

* Een [Cognitive Services-account](../cognitive-services/cognitive-services-apis-create-account.md)

* Een [Bing zoeken-API-sleutel](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api), waarmee u uw logische app toegang heeft tot de Bing zoeken-API's

* De logische app waar u toegang tot uw Event Hub. Als uw logische app met een trigger zoeken in Bing wilt, moet u een [lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-trigger"></a>

## <a name="add-a-bing-search-trigger"></a>Een Bing zoeken-trigger toevoegen

In Azure Logic Apps, elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde wordt voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine een exemplaar van de logische app maakt en werkstroom van uw app wordt gestart.

1. In de Azure portal of Visual Studio, maak een lege logische app, die Logic App Designer wordt geopend. Dit voorbeeld wordt de Azure-portal.

2. Typ 'Zoeken in Bing' als filter in het zoekvak. Selecteer de gewenste trigger in de lijst met triggers.

   In dit voorbeeld maakt gebruik van deze trigger: **Bing zoeken - op nieuwe nieuwsartikel**

   ![Bing zoeken-trigger zoeken](./media/connectors-create-api-bing-search/add-trigger.png)

3. Als u wordt gevraagd voor de verbindingsgegevens, [maken van de Bing zoeken-verbinding nu](#create-connection).
Of, als de verbinding al bestaat, geeft u de benodigde gegevens voor de trigger.

   In dit voorbeeld moet u criteria opgeven voor het retourneren van overeenkomende nieuwsartikelen van zoeken in Bing.

   | Eigenschap | Vereist | Value | Description |
   |----------|----------|-------|-------------|
   | Zoekquery | Ja | <*woorden zoeken*> | Voer in de trefwoorden die u wilt gebruiken. |
   | Market | Ja | <*Landinstelling*> | De landinstelling zoeken. De standaardwaarde is "en-US", maar kunt u een andere waarde. |
   | Veilig zoeken | Ja | <*zoeken op serverniveau*> | Het filterniveau voor het uitsluiten van inhoud voor volwassenen. De standaardwaarde is 'Gemiddeld', maar u kunt een ander niveau selecteren. |
   | Count | Nee | <*aantal resultaten*> | Het opgegeven aantal resultaten retourneren. De standaardwaarde is 20, maar u kunt een andere waarde opgeven. Het werkelijke aantal geretourneerde resultaten kan zijn dat kleiner is dan het opgegeven getal. |
   | Offset | Nee | <*skip-value*> | Het aantal resultaten dat moet worden overgeslagen voordat het retourneren van resultaten |
   |||||

   Bijvoorbeeld:

   ![Trigger instellen](./media/connectors-create-api-bing-search/bing-search-trigger.png)

4. Selecteer het interval en frequentie voor hoe vaak u wilt dat de trigger om te controleren op resultaten.

5. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**.

6. Nu doorgaan met een of meer acties toe te voegen aan uw logische app voor de taken die u wilt uitvoeren met de resultaten van de trigger.

<a name="add-action"></a>

## <a name="add-a-bing-search-action"></a>De Bing zoeken-actie toevoegen

In Azure Logic Apps, een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een stap in uw werkstroom die volgt op een trigger of een andere actie. In dit voorbeeld wordt de logische app begint met een Bing zoeken-trigger die wordt geretourneerd die overeenkomt met de opgegeven criteria nieuwsartikelen.

1. Open uw logische app in Logic App Designer in de Azure portal of Visual Studio. Dit voorbeeld wordt de Azure-portal.

2. Kies onder de trigger of actie **nieuwe stap** > **een actie toevoegen**.

   In dit voorbeeld maakt gebruik van deze trigger:

   **Bing zoeken - op nieuwe nieuwsartikel**

   ![Actie toevoegen](./media/connectors-create-api-bing-search/add-action.png)

   Als u wilt toevoegen een actie tussen bestaande stappen, Beweeg de muis boven de verbindende pijl. 
   Kies het plusteken (**+**) die wordt weergegeven, en kies vervolgens **een actie toevoegen**.

3. Typ 'Zoeken in Bing' als filter in het zoekvak.
Selecteer de actie die u wilt in de lijst met acties.

   In dit voorbeeld wordt met deze actie:

   **Bing zoeken - lijst nieuws door query**

   ![De Bing zoeken-actie vinden](./media/connectors-create-api-bing-search/bing-search-select-action.png)

4. Als u wordt gevraagd voor de verbindingsgegevens, [maken van de Bing zoeken-verbinding nu](#create-connection). Of, als de verbinding al bestaat, geeft u de benodigde gegevens voor de actie.

   Voor dit voorbeeld geeft u de criteria voor het retourneren van een subset van de resultaten van de trigger.

   | Eigenschap | Vereist | Value | Description |
   |----------|----------|-------|-------------|
   | Zoekquery | Ja | <*search-expression*> | Voer een expressie voor het uitvoeren van query's de resultaten van de trigger. U kunt selecteren in de velden in de lijst met dynamische inhoud of maakt u een expressie met de opbouwfunctie voor expressies. |
   | Market | Ja | <*Landinstelling*> | De landinstelling zoeken. De standaardwaarde is "en-US", maar kunt u een andere waarde. |
   | Veilig zoeken | Ja | <*zoeken op serverniveau*> | Het filterniveau voor het uitsluiten van inhoud voor volwassenen. De standaardwaarde is 'Gemiddeld', maar u kunt een ander niveau selecteren. |
   | Count | Nee | <*aantal resultaten*> | Het opgegeven aantal resultaten retourneren. De standaardwaarde is 20, maar u kunt een andere waarde opgeven. Het werkelijke aantal geretourneerde resultaten kan zijn dat kleiner is dan het opgegeven getal. |
   | Offset | Nee | <*skip-value*> | Het aantal resultaten dat moet worden overgeslagen voordat het retourneren van resultaten |
   |||||

   Stel bijvoorbeeld dat u wilt dat deze resultaten waarvan Categorienaam het woord "tech bevat".

   1. Klik in de **zoekquery** vak, zodat de lijst met dynamische inhoud wordt weergegeven. 
   Kies uit de lijst, **expressie** , zodat de opbouwfunctie voor expressies wordt weergegeven. 

      ![Bing zoeken-trigger](./media/connectors-create-api-bing-search/bing-search-action.png)

      U kunt nu beginnen met het maken van de expressie.

   2. Selecteer in de functielijst met, de **contains()** functie, die vervolgens wordt weergegeven in het expressievak. Klik op **dynamische inhoud** zodat de lijst met velden opnieuw wordt weergegeven, maar zorg ervoor dat de cursor tussen de haakjes blijft.

      ![Selecteer een functie](./media/connectors-create-api-bing-search/expression-select-function.png)

   3. Selecteer in de lijst met velden, **categorie**, die wordt geconverteerd naar een parameter. 
   Voeg een komma toe na de eerste parameter en na de komma, dit woord toevoegen: `'tech'` 

      ![Selecteer een veld](./media/connectors-create-api-bing-search/expression-select-field.png)

   4. Als u klaar bent, kiest u **Done**.

      De expressie wordt nu weergegeven in de **zoekquery** vak in deze indeling:

      ![Voltooide expressie](./media/connectors-create-api-bing-search/resolved-expression.png)

      In de weergave van code, is deze expressie wordt weergegeven in deze indeling:

      `"@{contains(triggerBody()?['category'],'tech')}"`

5. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**.

<a name="create-connection"></a>

## <a name="connect-to-bing-search"></a>Verbinding maken met Bing zoeken

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Wanneer u wordt gevraagd om de verbindingsgegevens, geef de volgende gegevens:

   | Eigenschap | Vereist | Value | Description |
   |----------|----------|-------|-------------|
   | Verbindingsnaam | Ja | <*naam van de verbinding*> | De naam voor uw verbinding maken |
   | API-versie | Ja | <*API-version*> | De Bing zoeken-API-versie is standaard ingesteld op de huidige versie. Indien nodig kunt u een eerdere versie. |
   | API-sleutel | Ja | <*API-key*> | De Bing zoeken-API-sleutel die u eerder hebt ontvangen. Als u een sleutel niet hebt, krijgt uw [nu API-sleutel](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api). |  
   |||||  

   Bijvoorbeeld:

   ![Verbinding maken](./media/connectors-create-api-bing-search/bing-search-create-connection.png)

2. Wanneer u klaar bent, kiest u **Maken**.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie, zoals triggers en acties limieten, zoals is beschreven in van de connector OpenAPI (voorheen Swagger)-bestand, raadpleegt u de [van de connector-verwijzingspagina](/connectors/bingsearch/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
