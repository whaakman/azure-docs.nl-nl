---
title: Query voor B2B-berichten in Log Analytics - Azure Logic Apps | Microsoft Docs
description: Maken van query's te traceren AS2, X 12 en EDIFACT berichten in Log Analytics
author: padmavc
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: LADocs; padmavc
ms.openlocfilehash: 48cca9919bd09906bdcc3faaaef186ec109c9169
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294306"
---
# <a name="query-for-as2-x12-and-edifact-messages-in-log-analytics"></a>Query voor AS2, X 12 en EDIFACT berichten in Log Analytics

Ga voor de AS2 X12 of EDIFACT berichten die u met bijhoudt [Azure Log Analytics](../log-analytics/log-analytics-overview.md), kunt u query's die acties op basis van specifieke criteria filteren. U kunt bijvoorbeeld berichten op basis van een specifieke interchange besturingselement getal vinden.

## <a name="requirements"></a>Vereisten

* Een logische app die ingesteld met de logboekregistratie van diagnostische gegevens. Meer informatie over [het maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) en [het instellen van logboekregistratie voor die app logica](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Integratie-account ingesteld met de controle en logboekregistratie. Meer informatie over [het maken van een account integratie](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) en [het instellen van controle en logboekregistratie voor dat account](../logic-apps/logic-apps-monitor-b2b-message.md).

* Als u dat nog niet gedaan hebt, [diagnostische gegevens publiceren met logboekanalyse](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) en [mailbericht bijhouden logboekanalyse instellen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Nadat u de vorige vereisten hebt voldaan, moet u een werkruimte in logboekanalyse hebben. U moet dezelfde werkruimte gebruiken voor het bijhouden van uw B2B-communicatie in logboekanalyse. 
>  
> Als u een werkruimte voor logboekanalyse niet hebt, ontdek [het maken van een werkruimte voor logboekanalyse](../log-analytics/log-analytics-quick-create-workspace.md).

## <a name="create-message-queries-with-filters-in-log-analytics"></a>Bericht-query's maken met filters in Log Analytics

In dit voorbeeld ziet hoe u berichten op basis van hun DIF besturingselement aantal kunt vinden.

> [!TIP] 
> Als u de naam van uw Log Analytics-werkruimte, gaat u naar de startpagina van uw werkruimte (`https://{your-workspace-name}.portal.mms.microsoft.com`), en beginnen bij stap 4. Anders kunt u beginnen bij stap 1.

1. In de [Azure-portal](https://portal.azure.com), kies **alle Services**. Zoek naar 'log analytics' in en kies vervolgens **logboekanalyse** als volgt te werk:

   ![Log Analytics vinden](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. Onder **logboekanalyse**, zoeken en selecteert u de werkruimte voor logboekanalyse.

   ![Selecteer de werkruimte voor logboekanalyse](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. Onder **Management**, kies **logboek zoeken**.

   ![Kies Lo zoeken](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/azure-portal-page.png)

4. Voer in het zoekvak, een veld dat u wilt zoeken en druk op **Enter**. Wanneer u te typen begint, logboekanalyse laat zien u mogelijke overeenkomsten en de bewerkingen die u kunt gebruiken. Meer informatie over [hoe u gegevens wilt zoeken in logboekanalyse](../log-analytics/log-analytics-log-searches.md).

   Dit voorbeeld wordt gezocht voor gebeurtenissen met de **Type = AzureDiagnostics**.

   ![Begin met het typen van queryreeks](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

5. Kies in de linkerbalk de periode die u wilt weergeven. Als een filter aan uw query wilt toevoegen, kiest u **+ toevoegen**.

   ![Filter toevoegen aan de query](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

6. Onder **Filters toevoegen**, voer de naam van het filter zodat u kunt het filter dat u wilt zoeken. Selecteer het filter en kies **+ toevoegen**.

   Ga voor het besturingselement interchange nummer in dit voorbeeld wordt gezocht naar het woord 'DIF' en selecteert **event_record_messageProperties_interchangeControlNumber_s** als het filter.

   ![Selecteer filter](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

7. Selecteer de filterwaarde die u wilt gebruiken en kies in de linkerbalk **toepassen**.

   In het volgende voorbeeld wordt het nummer van het besturingselement interchange voor de berichten die we willen.

   ![Selecteer filterwaarde](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

8. Nu kunt u terugkeren naar de query die u maakt. De query is bijgewerkt met het geselecteerde filter gebeurtenis en de waarde. De resultaten van uw vorige worden nu te gefilterd.

    ![Ga terug naar uw query met gefilterde resultaten](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a>Sla de query voor toekomstig gebruik

1. Uit uw query op de **logboek zoeken** pagina **opslaan**. De query een naam geven, selecteert u een categorie en kies **opslaan**.

   ![De query een naam en categorie geven](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. U kunt uw query bekijken **Favorieten**.

   !['Favorieten' kiezen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. Onder **opgeslagen zoekacties**, selecteert u uw query zodat u kunt de resultaten bekijken. Voor het bijwerken van de query zodat u kunt verschillende resultaten vinden door de query te bewerken.

   ![Uw query selecteren](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-log-analytics"></a>Zoeken naar en opgeslagen query's uitvoeren in Log Analytics

1. Open de startpagina van uw Log Analytics-werkruimte (`https://{your-workspace-name}.portal.mms.microsoft.com`), en kies **logboek zoeken**.

   ![Kies op uw startpagina Log Analytics "Logboek zoeken"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   -of-

   ![Kies in het menu "Logboek zoeken"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. Op de **logboek zoeken** startpagina, kiest u **Favorieten**.

   !['Favorieten' kiezen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. Onder **opgeslagen zoekacties**, selecteert u uw query zodat u kunt de resultaten bekijken. Voor het bijwerken van de query zodat u kunt verschillende resultaten vinden door de query te bewerken.

   ![Uw query selecteren](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a>Volgende stappen

* [Volgschema’s voor AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Volgschema’s voor X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Bijhouden van aangepaste schema 's](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)