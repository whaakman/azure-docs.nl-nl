---
title: Query's voor B2B-berichten maken in Log Analytics - Azure Logic Apps | Microsoft Docs
description: Query's die AS2, X 12 en EDIFACT-berichten met Log Analytics voor Azure Logic Apps bijhouden maken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/19/2018
ms.openlocfilehash: baccd255fc2812eae0de3a98dfcef3dcbc7e1b46
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124267"
---
# <a name="create-queries-for-tracking-as2-x12-and-edifact-messages-in-log-analytics-for-azure-logic-apps"></a>Query's voor het bijhouden van AS2, X 12 en EDIFACT-berichten in Log Analytics voor Azure Logic Apps maken

Als u wilt zoeken AS2, X12 of EDIFACT-berichten dat u met bijhoudt [Azure Log Analytics](../log-analytics/log-analytics-overview.md), kunt u query's die filteracties op basis van specifieke criteria. U kunt bijvoorbeeld berichten op basis van een specifieke uitwisseling controlenummer vinden.

## <a name="requirements"></a>Vereisten

* Een logische app die ingesteld met de logboekregistratie van diagnostische gegevens. Informatie over [over het maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) en [over het instellen van logboekregistratie voor deze logische app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Integratie-account ingesteld met bewaking en logboekregistratie. Informatie over [over het maken van een integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) en [over het instellen van controle en logboekregistratie voor dat account](../logic-apps/logic-apps-monitor-b2b-message.md).

* Als u dat nog niet gedaan hebt, [diagnostische gegevens publiceren naar Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) en [bericht bijhouden in Log Analytics instellen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Nadat u de bovenstaande vereisten hebt voldaan, moet u een werkruimte van Log Analytics hebben. U moet dezelfde werkruimte gebruiken voor het bijhouden van uw B2B-communicatie in Log Analytics. 
>  
> Als u een Log Analytics-werkruimte hebt, krijgt u informatie [over het maken van een Log Analytics-werkruimte](../log-analytics/log-analytics-quick-create-workspace.md).

## <a name="create-message-queries-with-filters-in-log-analytics"></a>Bericht-query's met filters in Log Analytics maken

In dit voorbeeld laat zien hoe u berichten op basis van hun controlenummer uitwisseling kunt vinden.

> [!TIP] 
> Als u bekend bent met de naam van uw Log Analytics-werkruimte, gaat u naar de startpagina van uw werkruimte (`https://{your-workspace-name}.portal.mms.microsoft.com`), en beginnen bij stap 4. Anders kunt u beginnen bij stap 1.

1. In de [Azure-portal](https://portal.azure.com), kiest u **alle Services**. Zoek naar "log analytics" en kies vervolgens **Log Analytics** zoals hier wordt weergegeven:

   ![Log Analytics zoeken](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. Onder **Log Analytics**, zoek en selecteer uw Log Analytics-werkruimte.

   ![Selecteer uw Log Analytics-werkruimte](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. Onder **Management**, kiest u **zoeken in logboeken**.

   ![Kies Lo zoeken](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/azure-portal-page.png)

4. Voer in het zoekvak, een veld dat u wilt zoeken, en druk op **Enter**. Wanneer u te typen begint, Log Analytics laat zien u mogelijke overeenkomsten en bewerkingen die u kunt gebruiken. Meer informatie over [gegevens zoeken in Log Analytics](../log-analytics/log-analytics-log-searches.md).

   In dit voorbeeld wordt gezocht naar gebeurtenissen met **Type = AzureDiagnostics**.

   ![Begin met het typen van queryreeks](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

5. Kies in de linkerbalk de periode die u wilt weergeven. Als u wilt een filter toevoegen aan uw query, kies **+ toevoegen**.

   ![Filter toevoegen aan query](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

6. Onder **Filters toevoegen**, voer de naam van het filter zodat u kunt het filter dat u wilt zoeken. Selecteer het filter en kies **+ toevoegen**.

   Als u het controlenummer uitwisseling zoekt, in dit voorbeeld wordt gezocht naar het woord 'knooppunt' en selecteert **event_record_messageProperties_interchangeControlNumber_s** als het filter.

   ![Filter selecteren](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

7. Selecteer de filterwaarde die u wilt gebruiken, en kies in de linkerbalk **toepassen**.

   In het volgende voorbeeld wordt de uitwisseling controlenummer voor de berichten die we willen.

   ![De filterwaarde selecteren](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

8. Ga nu terug naar de query die u bouwt. De query is bijgewerkt met het geselecteerde filter gebeurtenis en de waarde. De resultaten van uw vorige worden nu te gefilterd.

    ![Ga terug naar uw query's uitvoeren met de gefilterde resultaten](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a>De query voor toekomstig gebruik opslaan

1. Uit uw query op de **zoeken in logboeken** pagina, kies **opslaan**. Geef een naam op voor uw query, selecteert u een categorie en kies **opslaan**.

   ![Geef uw query een naam en categorie](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. Als u uw query, kies **Favorieten**.

   !['Favorieten' kiezen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. Onder **opgeslagen zoekopdrachten**, selecteert u uw query zodat u kunt de resultaten bekijken. Voor het bijwerken van de query, zodat u verschillende resultaten kunt vinden, kunt u de query bewerken.

   ![Selecteer uw query](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-log-analytics"></a>Zoeken en uitvoeren van opgeslagen query's in Log Analytics

1. Open de startpagina van uw Log Analytics-werkruimte (`https://{your-workspace-name}.portal.mms.microsoft.com`), en kies **zoeken in logboeken**.

   ![Kies op de startpagina van uw Log Analytics, "Zoeken in Logboeken"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   -of-

   ![Kies in het menu "Zoeken in Logboeken"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. Op de **zoeken in logboeken** startpagina, kiest u **Favorieten**.

   !['Favorieten' kiezen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. Onder **opgeslagen zoekopdrachten**, selecteert u uw query zodat u kunt de resultaten bekijken. Voor het bijwerken van de query, zodat u verschillende resultaten kunt vinden, kunt u de query bewerken.

   ![Selecteer uw query](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a>Volgende stappen

* [Volgschema’s voor AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Volgschema’s voor X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Aangepaste volgschema 's](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)