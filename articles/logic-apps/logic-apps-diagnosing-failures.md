---
title: Fouten - Azure Logic Apps vaststellen en oplossen | Microsoft Docs
description: Begrijpen hoe en waarom logic apps is mislukt
services: logic-apps
documentationcenter: 
author: jeffhollan
manager: anneta
editor: 
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 10/15/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: de706f711e9c57b2e575d130a2a0cfd0bdc907a1
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2017
---
# <a name="troubleshoot-and-diagnose-logic-app-failures"></a>Logic app problemen vaststellen en oplossen

Uw logische app genereert informatie waarmee u kunt opsporen en opsporen en oplossen in uw app. Aan de hand van elke stap in de werkstroom via de Azure portal kunt u een logische app onderzoeken. Of u kunt een aantal stappen toevoegen aan een werkstroom voor het opsporen van de runtime.

## <a name="review-trigger-history"></a>Trigger revisiegeschiedenis

Elke logische app begint met de trigger. Als de trigger wordt niet gestart, controleert u eerst de geschiedenis van de trigger. Dit overzicht ziet u de trigger pogingen die uw logische app gemaakt en meer informatie over het in- en uitgangen voor elke poging trigger.

1. Als u wilt controleren of de trigger geactiveerd voor uw logische app-menu kiezen **overzicht**. Onder **Trigger geschiedenis**, controleert u de status van de trigger.

   > [!TIP]
   > Als u het menu van de logische app niet ziet, probeer dan terug te gaan naar het Azure-dashboard. Open uw logische app dan opnieuw.

   ![Trigger revisiegeschiedenis](./media/logic-apps-diagnosing-failures/logic-app-trigger-history-overview.png)

   > [!TIP]
   > * Als u niet de verwachte gegevens vindt, probeert u **vernieuwen** op de werkbalk.
   > * Als de lijst bevat veel pogingen activeren en u kan de vermelding die u wilt vinden, probeert u het filteren van de lijst.

   Hier volgen de mogelijke statussen voor een trigger poging:

   | Status | Beschrijving | 
   | ------ | ----------- | 
   | **Is voltooid** | De trigger gecontroleerd van het eindpunt en beschikbare gegevens gevonden. Normaal gesproken 'Fired' status wordt ook weergegeven naast deze status. Als niet zo is, de definitie van de trigger een voorwaarde wellicht of `SplitOn` opdracht die niet is voldaan. <p>Deze status kunt toepassen op een handmatige trigger, terugkeerpatroon trigger of polling-trigger. Een trigger kan probleemloos worden uitgevoerd, maar de sessie zelf nog steeds kan mislukken als de acties onverwerkte fouten worden gegenereerd. | 
   | **Overgeslagen** | De trigger het eindpunt ingeschakeld maar er zijn geen gegevens gevonden. | 
   | **Is mislukt** | Er is een fout opgetreden. Als u wilt controleren gegenereerde foutberichten voor een mislukte trigger, selecteert u die poging trigger en kiest **uitvoer**. Bijvoorbeeld wellicht invoerwaarden die niet geldig. | 
   ||| 

   Mogelijk hebt u meerdere trigger vermeldingen met dezelfde datum en tijd die wordt uitgevoerd als met uw logische app meerdere items gevonden. 
   Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine maakt een logische app-exemplaar om uit te voeren van uw werkstroom. Standaard elke instantie wordt parallel uitgevoerd zodat geen werkstroom moet worden gewacht voordat een uitvoering wordt gestart.

   > [!TIP]
   > U kunt de trigger opnieuw te controleren zonder te wachten op het volgende terugkeerpatroon. Kies op de werkbalk overzicht **uitvoeren trigger**, en selecteer de trigger, waardoor een controle. Of selecteer **uitvoeren** op de werkbalk Logic Apps Designer.

3. Onder de details voor een poging trigger onderzoeken **Trigger geschiedenis**, trigger poging selecteert. 

   ![Selecteer een trigger poging](./media/logic-apps-diagnosing-failures/logic-app-trigger-history.png)

4. Controleer de invoer en uitvoer die de trigger wordt gegenereerd. De gegevens die afkomstig zijn van de trigger weergeven trigger uitvoer Deze uitvoer kunt u bepalen of alle eigenschappen naar verwachting geretourneerd.

   > [!NOTE]
   > Als u alle inhoud die u niet begrijpt vindt, informatie over hoe Azure Logic Apps [verschillende typen inhoud verwerkt](../logic-apps/logic-apps-content-type.md).

   ![Trigger-uitvoer](./media/logic-apps-diagnosing-failures/trigger-outputs.png)

## <a name="review-run-history"></a>Uitvoeringsgeschiedenis controleren

Elke gestarte trigger wordt gestart van een werkstroom uitgevoerd. U kunt bekijken wat is er gebeurd tijdens die worden uitgevoerd, inclusief de status voor elke stap in de werkstroom, plus de invoer en uitvoer voor elke stap.

1. Kies **Overzicht** in het menu van de logische app. Onder **wordt uitgevoerd geschiedenis**, Controleer de uitvoeren voor de gestarte trigger.

   > [!TIP]
   > Als u het menu van de logische app niet ziet, probeer dan terug te gaan naar het Azure-dashboard. Open uw logische app dan opnieuw.

   ![Bekijk geschiedenis wordt uitgevoerd](./media/logic-apps-diagnosing-failures/logic-app-runs-history-overview.png)

   > [!TIP]
   > * Als u niet de verwachte gegevens vindt, probeert u **vernieuwen** op de werkbalk.
   > * Als de lijst veel wordt uitgevoerd bevat en u de gewenste vermelding niet kunt vinden, probeert u het filteren van de lijst.

   Hier volgen de mogelijke statussen voor een run:

   | Status | Beschrijving | 
   | ------ | ----------- | 
   | **Is voltooid** | Alle acties is voltooid. <p>Als er fouten opgetreden in een specifieke actie, verwerkt een volgende actie in de werkstroom die is mislukt. | 
   | **Is mislukt** | Ten minste één actie is mislukt en er is geen latere acties in de werkstroom zijn ingesteld voor het afhandelen van de fout. | 
   | **Geannuleerd** | De werkstroom werd uitgevoerd, maar heeft een annuleringsaanvraag ontvangen. | 
   | **Uitgevoerd** | De werkstroom momenteel wordt uitgevoerd. <p>Deze status kan gebeuren voor beperkte werkstromen, hetzij vanwege de huidige prijsstelling. Zie voor meer informatie de [actie limieten op de pagina met prijzen](https://azure.microsoft.com/pricing/details/logic-apps/). Als u instelt [logboekregistratie van diagnostische gegevens](../logic-apps/logic-apps-monitor-your-logic-apps.md), kunt u ook informatie over eventuele vertraging gebeurtenissen die zich voordoen opvragen. | 
   ||| 

2. Bekijk de details voor elke stap in een specifieke uitvoering. Onder **wordt uitgevoerd geschiedenis**, selecteer de run die u wilt onderzoeken.

   ![Bekijk geschiedenis wordt uitgevoerd](./media/logic-apps-diagnosing-failures/logic-app-run-history.png)

   Hiermee wordt aangegeven of de uitvoering zelf is geslaagd of mislukt, de weergave Details uitvoering toont elke stap en of ze is gelukt of mislukt.

   ![Details bekijken van een run van een logische app](./media/logic-apps-diagnosing-failures/logic-app-run-details.png)

3. Als u wilt onderzoeken in de invoer, kiezen uitvoer en foutberichten voor een specifieke stap die stap zodat de vorm wordt uitgebreid en worden de details weergegeven. Bijvoorbeeld:

   ![Details van de stappen bekijken](./media/logic-apps-diagnosing-failures/logic-app-run-details-expanded.png)

## <a name="perform-runtime-debugging"></a>Runtime-foutopsporing uitvoeren

Om u te helpen bij foutopsporing, kunt u diagnostische stappen om een werkstroom, samen met het controleren van de trigger en wordt uitgevoerd geschiedenis toevoegen. Bijvoorbeeld, kunt u stappen die gebruikmaken van toevoegen de [RequestBin](http://requestb.in) service zodat u kunt inspecteren van HTTP-aanvragen en de exacte grootte, vorm en opmaak bepalen.

1. Maak een RequestBin, kunt u persoonlijke en alleen in de browser kan worden weergegeven.

2. Voeg in uw logische app, een HTTP POST-actie met de inhoud van de hoofdtekst die u testen wilt, bijvoorbeeld, een expressie of een andere stap uitvoer.

3. Plak de URL voor uw RequestBin in de HTTP POST-actie.

4. Voer de logische app om te controleren hoe een aanvraag wordt gevormd wanneer gegenereerd op basis van de engine voor Logic Apps, en uw RequestBin vernieuwen.

## <a name="next-steps"></a>Volgende stappen

[Uw logische app bewaken](../logic-apps/logic-apps-monitor-your-logic-apps.md)