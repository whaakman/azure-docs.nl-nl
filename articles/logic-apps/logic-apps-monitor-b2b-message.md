---
title: B2B-transacties bewaken en logboekregistratie - Azure Logic Apps instellen | Microsoft Docs
description: Monitor AS2, X 12 en EDIFACT berichten, start logboekregistratie van diagnostische gegevens voor uw account integratie
author: padmavc
manager: anneta
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 6afab12b9e2d6e8686ecbc95be9743afbe70d98c
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="monitor-and-set-up-diagnostics-logging-for-b2b-communication-in-integration-accounts"></a>Bewaken en het instellen van logboekregistratie van diagnostische gegevens voor B2B-communicatie in integratieaccounts

Na het instellen van B2B-communicatie tussen twee bedrijfsprocessen of toepassingen via uw account integratie met deze entiteiten berichten met elkaar kunnen uitwisselen. Om te bevestigen dat deze communicatie werkt zoals verwacht, kunt u instellen voor AS2, X12, bewaking en EDIFACT berichten, samen met de logboekregistratie van diagnostische gegevens voor uw account integratie via de [Azure Log Analytics](../log-analytics/log-analytics-overview.md) service. Deze service controleert uw cloud en on-premises omgevingen, zodat u kunt de beschikbaarheid en prestaties, onderhouden en verzamelt ook details van de runtime en gebeurtenissen voor uitgebreidere foutopsporing. U kunt ook [uw diagnostische gegevens met andere services gebruiken](#extend-diagnostic-data), zoals Azure Storage en Azure Event Hubs.

## <a name="requirements"></a>Vereisten

* Een logische app die ingesteld met de logboekregistratie van diagnostische gegevens. Meer informatie over [het instellen van logboekregistratie voor die app logica](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

  > [!NOTE]
  > Nadat u hebt deze vereiste voldaan, moet u een werkruimte in logboekanalyse hebben. Bij het instellen van logboekregistratie voor uw account integratie, moet u de dezelfde werkruimte voor logboekanalyse gebruiken. Als u een werkruimte voor logboekanalyse niet hebt, ontdek [het maken van een werkruimte voor logboekanalyse](../log-analytics/log-analytics-quick-create-workspace.md).

* Integratie-account gekoppeld aan uw logische app. Meer informatie over [een integratie-account maken met een koppeling naar uw logische app](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

## <a name="turn-on-diagnostics-logging-for-your-integration-account"></a>Diagnostische gegevens voor uw account integratie logboekregistratie inschakelen

U kunt rechtstreeks vanaf uw account integratie logboekregistratie inschakelen of [via de Azure-controleservice](#azure-monitor-service). Azure biedt een eenvoudige bewaking met niveau van de infrastructuur-gegevens. Meer informatie over [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md).

### <a name="turn-on-diagnostics-logging-directly-from-your-integration-account"></a>Diagnostische gegevens rechtstreeks vanuit uw account integratie logboekregistratie inschakelen

1. In de [Azure-portal](https://portal.azure.com), zoeken en selecteert u uw integratie-account. Onder **bewaking**, kies **diagnostische logboeken** als volgt te werk:

   ![Zoeken en selecteert u uw account integratie, kiest u 'Diagnostische logboeken'](media/logic-apps-monitor-b2b-message/integration-account-diagnostics.png)

2. Nadat u uw integratie-account hebt geselecteerd, worden de volgende waarden automatisch geselecteerd. Als deze waarden correct zijn, kiest u **diagnostische gegevens inschakelen**. Anders selecteert u de waarden die u wilt:

   1. Onder **abonnement**, selecteert u de Azure-abonnement dat u met uw integratie-account gebruiken.
   2. Onder **resourcegroep**, selecteert u de resourcegroep die u met uw account integratie gebruikt.
   3. Onder **brontype**, selecteer **integratieaccounts**. 
   4. Onder **Resource**, selecteert u uw integratie-account. 
   5. Kies **diagnostische gegevens inschakelen**.

   ![Diagnostische gegevens voor uw account integratie instellen](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. Onder **diagnostische instellingen**, en vervolgens **Status**, kies **op**.

   ![Azure diagnostische gegevens inschakelen](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Nu selecteren de werkruimte voor logboekanalyse en gegevens die moet worden gebruikt voor logboekregistratie, zoals wordt weergegeven:

   1. Selecteer **verzenden met logboekanalyse**. 
   2. Onder **logboekanalyse**, kies **configureren**. 
   3. Onder **OMS werkruimten**, selecteert u de werkruimte voor logboekanalyse voor logboekregistratie.
   4. Onder **logboek**, selecteer de **IntegrationAccountTrackingEvents** categorie.
   5. Kies **Opslaan**.

   ![Log Analytics instellen, zodat u diagnostische gegevens in een logboek verzenden kunt](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Nu [bijhouden voor uw B2B-berichten in logboekanalyse instellen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

<a name="azure-monitor-service"></a>

### <a name="turn-on-diagnostics-logging-through-azure-monitor"></a>Inschakelen van logboekregistratie van diagnostische gegevens via de Azure-Monitor

1. In de [Azure-portal](https://portal.azure.com), kiest u in het Azure hoofdmenu **Monitor**, **diagnostische logboeken**. Selecteer vervolgens uw account integratie als volgt te werk:

   ![Kies 'Bewaken', 'Diagnostische logboeken', selecteert u uw integratie-account](media/logic-apps-monitor-b2b-message/monitor-service-diagnostics-logs.png)

2. Nadat u uw integratie-account hebt geselecteerd, worden de volgende waarden automatisch geselecteerd. Als deze waarden correct zijn, kiest u **diagnostische gegevens inschakelen**. Anders selecteert u de waarden die u wilt:

   1. Onder **abonnement**, selecteert u de Azure-abonnement dat u met uw integratie-account gebruiken.
   2. Onder **resourcegroep**, selecteert u de resourcegroep die u met uw account integratie gebruikt.
   3. Onder **brontype**, selecteer **integratieaccounts**.
   4. Onder **Resource**, selecteert u uw integratie-account.
   5. Kies **diagnostische gegevens inschakelen**.

   ![Diagnostische gegevens voor uw account integratie instellen](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. Onder **diagnostische instellingen**, kies **op**.

   ![Azure diagnostische gegevens inschakelen](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Selecteer in de categorie voor Log Analytics-werkruimte en de gebeurtenis voor logboekregistratie nu zoals wordt weergegeven:

   1. Selecteer **verzenden met logboekanalyse**. 
   2. Onder **logboekanalyse**, kies **configureren**. 
   3. Onder **OMS werkruimten**, selecteert u de werkruimte voor logboekanalyse voor logboekregistratie.
   4. Onder **logboek**, selecteer de **IntegrationAccountTrackingEvents** categorie.
   5. Als u bent klaar, kiest u **Opslaan**.

   ![Log Analytics instellen, zodat u diagnostische gegevens in een logboek verzenden kunt](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Nu [bijhouden voor uw B2B-berichten in logboekanalyse instellen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Hoe en waar u diagnostische gegevens gebruikt met andere services uitbreiden

U kunt samen met Azure Log Analytics uitbreiden hoe u diagnostische gegevens van uw logische app gebruiken met andere Azure-services, bijvoorbeeld: 

* [Archief Azure Diagnostics wordt geregistreerd in Azure Storage](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Azure Diagnostics logboeken naar Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

U kunt vervolgens get real-time bewaking met Telemetrie en analyses van andere services, zoals [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) en [Power BI](../log-analytics/log-analytics-powerbi.md). Bijvoorbeeld:

* [Stroomgegevens uit Event Hubs met Stream Analytics](../stream-analytics/stream-analytics-define-inputs.md)
* [Streaming gegevens analyseren met Stream Analytics en een realtime analytics-dashboard maken in Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Op basis van de opties die u wilt instellen, zorg ervoor dat u eerste [maken van een Azure storage-account](../storage/common/storage-create-storage-account.md) of [maken van een Azure event hub](../event-hubs/event-hubs-create.md). Selecteer de opties voor waar u diagnostische gegevens verzenden:

![Gegevens verzenden naar Azure storage-account of event hub](./media/logic-apps-monitor-b2b-message/storage-account-event-hubs.png)

> [!NOTE]
> Bewaarperiode gelden alleen wanneer u kiest voor een opslagaccount.

## <a name="supported-tracking-schemas"></a>Bijhouden van de ondersteunde schema 's

Azure ondersteunt deze bijhouden schematypen waarin alle schema's met uitzondering van het type aangepast hebt opgelost.

* [Volgschema voor AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Volgschema voor X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Aangepaste volgschema's](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Volgende stappen

* [B2B-berichten in logboekanalyse bijhouden](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "bijhouden B2B-berichten in OMS")
* [Meer informatie over het Enterprise-integratiepakket](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")

