---
title: Query voor B2B-berichten in de Operations Management Suite - Azure Logic Apps | Microsoft Docs
description: Maken van query's te traceren AS2, X 12 en EDIFACT berichten in de Operations Management Suite
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 2748d3d3daf7c13dca05f663a4a088598e1b3605
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="query-for-as2-x12-and-edifact-messages-in-the-microsoft-operations-management-suite-oms"></a>Query voor AS2, X 12 en EDIFACT berichten in de Microsoft Operations Management Suite (OMS)

Ga voor de AS2 X12 of EDIFACT berichten die u met bijhoudt [Azure Log Analytics](../log-analytics/log-analytics-overview.md) in de [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), kunt u query's die acties op basis van specifieke criteria filteren. U kunt bijvoorbeeld berichten op basis van een specifieke interchange besturingselement getal vinden.

## <a name="requirements"></a>Vereisten

* Een logische app die ingesteld met de logboekregistratie van diagnostische gegevens. Meer informatie over [het maken van een logische app](../logic-apps/logic-apps-create-a-logic-app.md) en [het instellen van logboekregistratie voor die app logica](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Integratie-account ingesteld met de controle en logboekregistratie. Meer informatie over [het maken van een account integratie](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) en [het instellen van controle en logboekregistratie voor dat account](../logic-apps/logic-apps-monitor-b2b-message.md).

* Als u dat nog niet gedaan hebt, [diagnostische gegevens publiceren met logboekanalyse](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) en [bericht bijhouden in OMS instellen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Nadat u de vorige vereisten hebt voldaan, moet er een werkruimte in de [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). U moet dezelfde OMS-werkruimte gebruiken voor het bijhouden van uw B2B-communicatie in OMS. 
>  
> Als u een OMS-werkruimte niet hebt, ontdek [het maken van een OMS-werkruimte](../log-analytics/log-analytics-get-started.md).

## <a name="create-message-queries-with-filters-in-the-operations-management-suite-portal"></a>Bericht-query's maken met filters in de Operations Management Suite-portal

In dit voorbeeld ziet hoe u berichten op basis van hun DIF besturingselement aantal kunt vinden.

> [!TIP] 
> Als u de naam van uw OMS-werkruimte, gaat u naar de startpagina van uw werkruimte (`https://{your-workspace-name}.portal.mms.microsoft.com`), en beginnen bij stap 4. Anders kunt u beginnen bij stap 1.

1. In de [Azure-portal](https://portal.azure.com), kies **meer Services**. Zoek naar 'log analytics' in en kies vervolgens **logboekanalyse** als volgt te werk:

   ![Log Analytics vinden](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. Onder **logboekanalyse**, zoeken en selecteert u de OMS-werkruimte.

   ![Selecteer de OMS-werkruimte](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. Onder **Management**, kies **OMS-Portal**.

   ![Kies de OMS-portal](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/omsportalpage.png)

4. Kies op de startpagina OMS **logboek zoeken**.

   ![Kies op uw startpagina OMS "Logboek zoeken"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   -of-

   ![Kies in het menu OMS "Logboek zoeken"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

5. Voer in het zoekvak, een veld dat u wilt zoeken en druk op **Enter**. Wanneer u te typen begint, OMS laat zien u mogelijke overeenkomsten en de bewerkingen die u kunt gebruiken. Meer informatie over [hoe u gegevens wilt zoeken in logboekanalyse](../log-analytics/log-analytics-log-searches.md).

   Dit voorbeeld wordt gezocht voor gebeurtenissen met de **Type = AzureDiagnostics**.

   ![Begin met het typen van queryreeks](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

6. Kies in de linkerbalk de periode die u wilt weergeven. Als een filter aan uw query wilt toevoegen, kiest u **+ toevoegen**.

   ![Filter toevoegen aan de query](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

7. Onder **Filters toevoegen**, voer de naam van het filter zodat u kunt het filter dat u wilt zoeken. Selecteer het filter en kies **+ toevoegen**.

   Ga voor het besturingselement interchange nummer in dit voorbeeld wordt gezocht naar het woord 'DIF' en selecteert **event_record_messageProperties_interchangeControlNumber_s** als het filter.

   ![Selecteer filter](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

9. Selecteer de filterwaarde die u wilt gebruiken en kies in de linkerbalk **toepassen**.

   In het volgende voorbeeld wordt het nummer van het besturingselement interchange voor de berichten die we willen.

   ![Selecteer filterwaarde](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

10. Nu kunt u terugkeren naar de query die u maakt. De query is bijgewerkt met het geselecteerde filter gebeurtenis en de waarde. De resultaten van uw vorige worden nu te gefilterd.

    ![Ga terug naar uw query met gefilterde resultaten](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a>Sla de query voor toekomstig gebruik

1. Uit uw query op de **logboek zoeken** pagina **opslaan**. De query een naam geven, selecteert u een categorie en kies **opslaan**.

   ![De query een naam en categorie geven](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. U kunt uw query bekijken **Favorieten**.

   !['Favorieten' kiezen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. Onder **opgeslagen zoekacties**, selecteert u uw query zodat u kunt de resultaten bekijken. Voor het bijwerken van de query zodat u kunt verschillende resultaten vinden door de query te bewerken.

   ![Uw query selecteren](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-the-operations-management-suite-portal"></a>Zoeken en opgeslagen query's uitvoeren in de Operations Management Suite-portal

1. Open de startpagina van de OMS-werkruimte (`https://{your-workspace-name}.portal.mms.microsoft.com`), en kies **logboek zoeken**.

   ![Kies op uw startpagina OMS "Logboek zoeken"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   -of-

   ![Kies in het menu OMS "Logboek zoeken"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. Op de **logboek zoeken** startpagina, kiest u **Favorieten**.

   !['Favorieten' kiezen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. Onder **opgeslagen zoekacties**, selecteert u uw query zodat u kunt de resultaten bekijken. Voor het bijwerken van de query zodat u kunt verschillende resultaten vinden door de query te bewerken.

   ![Uw query selecteren](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a>Volgende stappen

* [Volgschema’s voor AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Volgschema’s voor X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Bijhouden van aangepaste schema 's](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)