---
title: Problemen oplossen en vaststellen - Azure Logic Apps | Microsoft Docs
description: Begrijpen hoe en waarom logic apps is mislukt
services: logic-apps
documentationcenter: ''
author: jeffhollan
manager: jeconnoc
editor: ''
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 10/15/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: b0bf6cd747860d938f80787d9bef6634a6a22d09
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441529"
---
# <a name="troubleshoot-and-diagnose-logic-app-failures"></a>Fouten in logische Apps vaststellen en oplossen

Uw logische app genereert informatie waarmee u vaststellen en oplossen van problemen in uw app. Aan de hand van elke stap in de werkstroom via de Azure-portal kunt u een logische app onderzoeken. Of u kunt sommige stappen toevoegen aan een werkstroom voor het opsporen van fouten in runtime.

## <a name="review-trigger-history"></a>Triggergeschiedenis controleren

Elke logische app begint met de trigger. Als de trigger wordt niet gestart, controleert u eerst de triggergeschiedenis. Dit overzicht ziet u de trigger-pogingen die uw logische app gemaakt en meer informatie over de invoer en uitvoer voor elke poging trigger.

1. Als u wilt controleren of de trigger geactiveerd op het menu van uw logische app, kies **overzicht**. Onder **Triggergeschiedenis**, controleert u de status van de trigger.

   > [!TIP]
   > Als u het menu van de logische app niet ziet, probeer dan terug te gaan naar het Azure-dashboard. Open uw logische app dan opnieuw.

   ![Triggergeschiedenis controleren](./media/logic-apps-diagnosing-failures/logic-app-trigger-history-overview.png)

   > [!TIP]
   > * Als u de gegevens die u verwacht niet vinden, probeert u **vernieuwen** op de werkbalk.
   > * Als de lijst bevat veel pogingen activeren, en u de vermelding die u wilt niet kunt vinden, probeert u het filteren van de lijst.

   Hier volgen de mogelijke statussen van een trigger poging:

   | Status | Beschrijving | 
   | ------ | ----------- | 
   | **Is voltooid** | De trigger gecontroleerd van het eindpunt en beschikbare gegevens gevonden. Normaal gesproken een 'Fired'-status wordt ook weergegeven naast deze status. Als niet zo is, de definitie van de trigger een voorwaarde wellicht of `SplitOn` opdracht die niet is voldaan. <p>Deze status kunt toepassen op een handmatige trigger, een trigger met terugkeerpatroon of een polling-trigger. Een trigger met succes kunt uitvoeren, maar de uitvoering zelf nog steeds kan mislukken als de acties die niet-opgeloste fouten gegenereerd. | 
   | **Overgeslagen** | De trigger is ingeschakeld voor het eindpunt, maar er zijn geen gegevens gevonden. | 
   | **Is mislukt** | Er is een fout opgetreden. Als u wilt controleren eventuele gegenereerde foutberichten voor een mislukte trigger, selecteert u die poging trigger en kies **uitvoer**. Bijvoorbeeld, wellicht u invoer die niet geldig. | 
   ||| 

   Mogelijk hebt u meerdere trigger vermeldingen met dezelfde datum en tijd, die wordt uitgevoerd wanneer de logische app vindt meerdere items. 
   Telkens wanneer de trigger wordt geactiveerd, in de Logic Apps-engine maakt een exemplaar van de logische app als uw werkstroom wilt uitvoeren. Standaard elk exemplaar wordt parallel uitgevoerd zodat geen werkstroom moet worden gewacht voordat u begint met een uitvoering is.

   > [!TIP]
   > U kunt de trigger opnieuw zonder te wachten op het terugkeerpatroon van de volgende. Kies op de werkbalk overzicht **trigger uitvoeren**, en selecteer de trigger, waardoor een selectievakje. Of selecteer **uitvoeren** op de werkbalk van de ontwerper van logische Apps.

3. Het onderzoeken van de details voor de poging van een trigger, onder **Triggergeschiedenis**, selecteert u die poging trigger. 

   ![Een poging tot trigger selecteren](./media/logic-apps-diagnosing-failures/logic-app-trigger-history.png)

4. Controleer de invoer en uitvoer die de trigger wordt gegenereerd. Triggeruitvoer weergeven de gegevens die afkomstig uit de trigger zijn. Deze uitvoer kunt u bepalen of alle eigenschappen geretourneerd zoals verwacht.

   > [!NOTE]
   > Als u alle inhoud die u niet machtig bent, hoe Azure Logic Apps meer [verschillende typen inhoud verwerkt](../logic-apps/logic-apps-content-type.md).

   ![Triggeruitvoer](./media/logic-apps-diagnosing-failures/trigger-outputs.png)

## <a name="review-run-history"></a>Uitvoeringsgeschiedenis controleren

Elke geactiveerde trigger start een werkstroom uitvoert. U kunt bekijken wat is er gebeurd tijdens die worden uitgevoerd, met inbegrip van de status van elke stap in de werkstroom, plus de invoer en uitvoer voor elke stap.

1. Kies **Overzicht** in het menu van de logische app. Onder **geschiedenis van uitvoeringen**, het uitvoeren van de geactiveerde trigger bekijken.

   > [!TIP]
   > Als u het menu van de logische app niet ziet, probeer dan terug te gaan naar het Azure-dashboard. Open uw logische app dan opnieuw.

   ![Geschiedenis van uitvoeringen controleren](./media/logic-apps-diagnosing-failures/logic-app-runs-history-overview.png)

   > [!TIP]
   > * Als u de gegevens die u verwacht niet vinden, probeert u **vernieuwen** op de werkbalk.
   > * Als de lijst ziet u veel wordt uitgevoerd en u de vermelding die u niet kunt vinden, probeert u het filteren van de lijst.

   Hier volgen de mogelijke statussen voor het uitvoeren van:

   | Status | Beschrijving | 
   | ------ | ----------- | 
   | **Is voltooid** | Alle acties is voltooid. <p>Als er fouten opgetreden in een bepaalde actie, verwerkt een volgende actie in de werkstroom die fout. | 
   | **Is mislukt** | Ten minste één actie is mislukt en er is geen latere acties in de werkstroom zijn ingesteld voor het afhandelen van de fout. | 
   | **Geannuleerd** | De werkstroom werd uitgevoerd, maar heeft een aanvraag annuleren ontvangen. | 
   | **Wordt uitgevoerd** | De werkstroom wordt momenteel uitgevoerd. <p>Deze status kan optreden voor beperkte werkstromen, hetzij vanwege het huidige prijsschema. Zie voor meer informatie de [actie limieten op de pagina met prijzen](https://azure.microsoft.com/pricing/details/logic-apps/). Als u [Diagnostische logboekregistratie](../logic-apps/logic-apps-monitor-your-logic-apps.md), krijgt u ook informatie over eventuele vertraging gebeurtenissen die plaatsvinden. | 
   ||| 

2. Bekijk de details voor elke stap in een specifieke uitvoering. Onder **geschiedenis van uitvoeringen**, selecteert u de uitvoering die u wilt onderzoeken.

   ![Geschiedenis van uitvoeringen controleren](./media/logic-apps-diagnosing-failures/logic-app-run-history.png)

   Of de uitvoering zelf is geslaagd of mislukt, de Details uitvoering van weergave bevat elke stap en of ze geslaagd of mislukt.

   ![Details bekijken van een run van een logische app](./media/logic-apps-diagnosing-failures/logic-app-run-details.png)

3. Voor het onderzoeken van de invoer, uitvoer en eventuele foutberichten voor een specifieke stap, kiest u die stap zodat de vorm wordt uitgebreid en worden de details weergegeven. Bijvoorbeeld:

   ![Details van de stappen bekijken](./media/logic-apps-diagnosing-failures/logic-app-run-details-expanded.png)

## <a name="perform-runtime-debugging"></a>Runtimefoutopsporing uitvoeren

Om u te helpen bij het opsporen van fouten, kunt u diagnostische stappen om een werkstroom, samen met het controleren van de trigger en geschiedenis van uitvoeringen toevoegen. Bijvoorbeeld, kunt u stappen die gebruikmaken van toevoegen de [Webhook Tester](https://webhook.site/) service zodat u kunt HTTP-aanvragen te inspecteren en de exacte grootte, vorm en indeling bepalen.

1. Ga naar [Webhook Tester](https://webhook.site/) en kopieer de unieke URL die is gemaakt

2. In uw logische app, moet u een HTTP POST-actie met de inhoud van de hoofdtekst die u testen wilt, bijvoorbeeld, een expressie of een andere stap uitvoer toevoegen.

3. Plak de URL voor de Webhook-Tester in de HTTP POST-actie.

4. De logische app uitvoeren om te controleren hoe een aanvraag wordt gevormd wanneer gegenereerd op basis van de Logic Apps-engine, en Zie Webhook Testprogramma voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

[De logische app controleren](../logic-apps/logic-apps-monitor-your-logic-apps.md)
