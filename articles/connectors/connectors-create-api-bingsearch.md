---
title: Verbinding maken met Bing zoeken - Azure Logic Apps | Microsoft Docs
description: Nieuws met Bing Search REST-API's en Azure Logic Apps zoeken
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/21/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 8ac67f9df0e5baccc668c2aeb70f65d96e574df5
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37021282"
---
# <a name="find-news-with-bing-search-and-azure-logic-apps"></a>Nieuws met Bing-zoekopdracht en Azure Logic Apps zoeken 

Dit artikel laat zien hoe u nieuws, video's en andere items via Bing zoekopdracht uit op een logische app met de Bing zoeken-connector kunt vinden. Op die manier kunt u logic apps die taken automatiseren en werkstromen voor verwerking naar zoekresultaten en maak deze items beschikbaar voor andere acties. 

Bijvoorbeeld, kunt u zoeken op basis van zoekcriteria nieuwsitems en Twitter boekt die items als tweets in uw Twitter-feed.

Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Voor connector-specifieke technische informatie, Zie de <a href="https://docs.microsoft.com/connectors/bingsearch/" target="blank">Bing zoeken-connector verwijzing</a>.

## <a name="prerequisites"></a>Vereisten

* Een [cognitieve Services-account](../cognitive-services/cognitive-services-apis-create-account.md)

* Een [Bing zoeken-API-sleutel](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api), waarmee u uw logische app toegang heeft tot de Bing zoeken-API's 

* De logische app waar u toegang tot uw Event Hub. U kunt uw logische app starten met een trigger Bing zoeken, moet u een [lege logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

<a name="add-trigger"></a>

## <a name="add-a-bing-search-trigger"></a>Bing zoeken-trigger toevoegen

In Azure Logic Apps elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde is voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine maakt een logische app-exemplaar en de werkstroom van uw app wordt gestart.

1. In de Azure-portal of Visual Studio maakt een lege logische app, dat wordt geopend Logic App-ontwerper. In dit voorbeeld wordt de Azure-portal.

2. Voer in het zoekvak 'Bing-zoekopdracht' als filter. Selecteer de gewenste trigger in de lijst triggers. 

   In dit voorbeeld wordt deze trigger: **Bing zoeken - op nieuwe nieuwsbericht**

   ![Bing zoeken-trigger vinden](./media/connectors-create-api-bing-search/add-trigger.png)

3. Als u wordt gevraagd om de details voor verbinding [nu uw Bing zoeken-verbinding maken](#create-connection). Of als uw verbinding al aanwezig is, geeft u de benodigde gegevens voor de trigger.

   In dit voorbeeld criteria kunnen geven voor het retourneren van overeenkomende nieuwsartikelen van Bing zoeken.

   | Eigenschap | Vereist | Waarde | Beschrijving | 
   |----------|----------|-------|-------------| 
   | Zoekquery | Ja | <*woorden zoeken*> | Voer de zoekwoorden die u wilt gebruiken. |
   | Market | Ja | <*Landinstellingen*> | De landinstelling zoeken. De standaardwaarde is 'en-US', maar kunt u een andere waarde. | 
   | Veilig zoeken | Ja | <*zoeken op gebruikersniveau*> | Het filterniveau voor het uitsluiten van inhoud voor volwassenen. De standaardwaarde is 'Gemiddeld', maar u een ander niveau selecteren. | 
   | Count | Nee | <*aantal resultaten*> | Het opgegeven aantal resultaten retourneren. De standaardwaarde is 20, maar u kunt een andere waarde opgeven. Het werkelijke aantal geretourneerde resultaten mogelijk minder dan het opgegeven aantal. | 
   | Offset | Nee | <*waarde voor overslaan*> | Het aantal resultaten om over te slaan voordat het retourneren van resultaten | 
   ||||| 

   Bijvoorbeeld:

   ![Trigger instellen](./media/connectors-create-api-bing-search/bing-search-trigger.png)

4. Selecteer de interval en frequentie voor hoe vaak de trigger voor de resultaten controleren.

5. Wanneer u, klik op de werkbalk ontwerpen bent klaar kiezen **opslaan**. 

6. Nu doorgaan met een of meer acties toe te voegen aan uw logische app voor de taken die u wilt uitvoeren met de trigger-resultaten.

<a name="add-action"></a>

## <a name="add-a-bing-search-action"></a>Bing zoeken-actie toevoegen

In Azure Logic Apps een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een stap in de werkstroom die na een trigger of een andere actie. In dit voorbeeld wordt de logische app begint met een trigger Bing zoeken die voldoen aan de opgegeven criteria nieuwsartikelen retourneert. 

1. Open uw logische app in App-ontwerper voor logica in de Azure-portal of Visual Studio. In dit voorbeeld wordt de Azure-portal.

2. Kies onder de trigger of actie **nieuwe stap** > **een actie toevoegen**.

   In dit voorbeeld wordt deze trigger: **Bing zoeken - op nieuwe nieuwsbericht**

   ![Actie toevoegen](./media/connectors-create-api-bing-search/add-action.png)

   Als u een actie tussen bestaande stappen toevoegen, wilt u de muisaanwijzer over de verbindende pijl. 
   Kies het plusteken (**+**) die wordt weergegeven, en kies vervolgens **een actie toevoegen**.

3. Voer in het zoekvak 'Bing-zoekopdracht' als filter.
Selecteer de actie die u wilt in de lijst van acties.

   In dit voorbeeld wordt met deze actie: **Bing zoeken - lijst nieuws door query**

   ![Bing zoeken-actie vinden](./media/connectors-create-api-bing-search/bing-search-select-action.png)

4. Als u wordt gevraagd om de details voor verbinding [nu uw Bing zoeken-verbinding maken](#create-connection). Of als uw verbinding al aanwezig is, geeft u de benodigde gegevens voor de actie. 

   Geef de criteria voor het retourneren van een subset van de resultaten van de trigger voor dit voorbeeld.

   | Eigenschap | Vereist | Waarde | Beschrijving | 
   |----------|----------|-------|-------------| 
   | Zoekquery | Ja | <*zoeken-expressie*> | Voer een expressie voor de trigger queryresultaten. U kunt selecteren van de velden in de lijst met dynamische inhoud of een expressie maken met de opbouwfunctie voor expressies. |
   | Market | Ja | <*Landinstellingen*> | De landinstelling zoeken. De standaardwaarde is 'en-US', maar kunt u een andere waarde. | 
   | Veilig zoeken | Ja | <*zoeken op gebruikersniveau*> | Het filterniveau voor het uitsluiten van inhoud voor volwassenen. De standaardwaarde is 'Gemiddeld', maar u een ander niveau selecteren. | 
   | Count | Nee | <*aantal resultaten*> | Het opgegeven aantal resultaten retourneren. De standaardwaarde is 20, maar u kunt een andere waarde opgeven. Het werkelijke aantal geretourneerde resultaten mogelijk minder dan het opgegeven aantal. | 
   | Offset | Nee | <*waarde voor overslaan*> | Het aantal resultaten om over te slaan voordat het retourneren van resultaten | 
   ||||| 

   Stel bijvoorbeeld dat u wilt dat deze resultaten waarvan Categorienaam het woord "tech" bevat. 
   
   1. Klik in de **zoekquery** vak zodat de lijst met dynamische inhoud wordt weergegeven. 
   Kies uit de lijst, **expressie** zodat de opbouwfunctie wordt weergegeven. 

      ![Bing zoeken-trigger](./media/connectors-create-api-bing-search/bing-search-action.png)

      U kunt nu gaan maken van de expressie.

   2. Selecteer in de lijst met functies de **contains()** functie, die vervolgens wordt weergegeven in het expressievak. Klik op **dynamische inhoud** zodat de veldenlijst opnieuw wordt weergegeven, maar zorg ervoor dat de cursor tussen de haakjes blijft.

      ![Selecteer een functie](./media/connectors-create-api-bing-search/expression-select-function.png)

   3. Selecteer in de lijst met velden **categorie**, die wordt geconverteerd naar een parameter. 
   Toevoegen van een door komma's na de eerste parameter, en de komma, het toevoegen van dit woord: `'tech'` 

      ![Selecteer een veld](./media/connectors-create-api-bing-search/expression-select-field.png)
   
   4. Als u bent klaar, kiest u **OK**.

      De expressie nu wordt weergegeven de **zoekquery** vak in deze indeling:

      ![Voltooide expressie](./media/connectors-create-api-bing-search/resolved-expression.png)

      Deze expressie wordt in de codeweergave weergegeven in deze indeling:

      `"@{contains(triggerBody()?['category'],'tech')}"`

5. Wanneer u, klik op de werkbalk ontwerpen bent klaar kiezen **opslaan**.

<a name="create-connection"></a>

## <a name="connect-to-bing-search"></a>Verbinding maken met Bing zoeken

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. Wanneer u wordt gevraagd om de verbindingsgegevens, bieden deze details:

   | Eigenschap | Vereist | Waarde | Beschrijving | 
   |----------|----------|-------|-------------| 
   | Verbindingsnaam | Ja | <*naam van de verbinding*> | De naam voor uw verbinding maken |
   | API-versie | Ja | <*API-versie*> | Bing zoeken-API-versie is standaard ingesteld op de huidige versie. Desgewenst kunt u een eerdere versie. | 
   | API-sleutel | Ja | <*API-sleutel*> | De Bing zoeken-API-sleutel die u eerder hebt verkregen. Als u niet een sleutel hebt, kunt u uw [API-sleutel nu](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api). |  
   |||||  

   Bijvoorbeeld:

   ![Verbinding maken](./media/connectors-create-api-bing-search/bing-search-create-connection.png)

2. Wanneer u klaar bent, kiest u **Maken**.

## <a name="connector-reference"></a>Connector-verwijzing

Zie voor technische details, zoals triggers, acties en limieten, zoals is beschreven door de connector Swagger-bestand, de [van connector-verwijzingspagina](/connectors/bingsearch/). 

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
