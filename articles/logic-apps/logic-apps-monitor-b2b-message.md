---
title: B2B-berichten bewaken en het instellen van logboekregistratie - Azure Logic Apps | Microsoft Docs
description: Monitor AS2, X 12 en EDIFACT-berichten. Instellen van logboekregistratie van diagnostische gegevens voor uw integratieaccount in Azure Logic Apps.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 07/21/2017
ms.openlocfilehash: fd48dfaf7b863ab026da1758751509d9911ae01e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948006"
---
# <a name="monitor-b2b-messages-and-set-up-logging-for-integration-accounts-in-azure-logic-apps"></a>B2B-berichten bewaken en het instellen van logboekregistratie voor integratieaccounts in Azure Logic Apps

Na het instellen van B2B-communicatie tussen twee met zakelijke processen of toepassingen via uw integratie-account, deze entiteiten berichten met elkaar kunnen uitwisselen. Om te bevestigen van deze communicatie werkt zoals verwacht, kunt u een controle van het AS2, X12, instellen en EDIFACT-berichten, samen met de logboekregistratie van diagnostische gegevens voor uw integratie-account via de [Azure Log Analytics](../log-analytics/log-analytics-overview.md) service. Deze service uw cloud en on-premises omgevingen, zodat u hun beschikbaarheid en prestaties, onderhouden en verzamelt tevens informatie over runtime-gegevens en gebeurtenissen voor uitgebreidere foutopsporing. U kunt ook [uw diagnostische gegevens gebruiken met andere services](#extend-diagnostic-data), zoals Azure Storage en Azure Event Hubs.

## <a name="requirements"></a>Vereisten

* Een logische app die ingesteld met de logboekregistratie van diagnostische gegevens. Informatie over [over het instellen van logboekregistratie voor deze logische app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

  > [!NOTE]
  > Nadat u hebt voldaan aan deze vereiste, hebt u een werkruimte van Log Analytics. Bij het instellen van logboekregistratie voor uw integratie-account, moet u de dezelfde werkruimte van Log Analytics gebruiken. Als u een Log Analytics-werkruimte hebt, krijgt u informatie [over het maken van een Log Analytics-werkruimte](../log-analytics/log-analytics-quick-create-workspace.md).

* Een integratieaccount dat gekoppeld aan uw logische app. Informatie over [hoe u een integratieaccount maakt met een koppeling naar uw logische app](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

## <a name="turn-on-diagnostics-logging-for-your-integration-account"></a>Logboekregistratie voor uw integratieaccount diagnostische gegevens inschakelen

U kunt inschakelen logboekregistratie rechtstreeks vanaf uw integratie-account of [via de service Azure Monitor](#azure-monitor-service). Azure Monitor biedt basiscontrole met gegevens op infrastructuurniveau. Meer informatie over [Azure Monitor](../azure-monitor/overview.md).

### <a name="turn-on-diagnostics-logging-directly-from-your-integration-account"></a>Diagnostische gegevens rechtstreeks vanuit uw integratieaccount logboekregistratie inschakelen

1. In de [Azure-portal](https://portal.azure.com), zoek en selecteer uw integratie-account. Onder **bewaking**, kiest u **diagnoselogboeken** zoals hier wordt weergegeven:

   ![Zoeken en selecteer uw integratieaccount, kies "Diagnostische logboeken"](media/logic-apps-monitor-b2b-message/integration-account-diagnostics.png)

2. Nadat u uw integratie-account hebt geselecteerd, worden de volgende waarden worden automatisch geselecteerd. Als deze waarden correct zijn, kiest u **diagnostische gegevens inschakelen**. Anders selecteert u de waarden die u wilt:

   1. Onder **abonnement**, selecteert u de Azure-abonnement dat u met uw integratie-account gebruiken.
   2. Onder **resourcegroep**, selecteer de resourcegroep die u met uw integratie-account gebruikt.
   3. Onder **resourcetype**, selecteer **integratieaccounts**. 
   4. Onder **Resource**, selecteert u uw integratieaccount. 
   5. Kies **diagnostische gegevens inschakelen**.

   ![Diagnostische gegevens voor uw integratie-account instellen](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. Onder **diagnostische instellingen**, en vervolgens **Status**, kiest u **op**.

   ![Azure diagnostische gegevens inschakelen](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Selecteer nu de Log Analytics-werkruimte en de gegevens die moet worden gebruikt voor logboekregistratie, zoals wordt weergegeven:

   1. Selecteer **verzenden naar Log Analytics**. 
   2. Onder **Log Analytics**, kiest u **configureren**. 
   3. Onder **OMS-werkruimten**, selecteert u de Log Analytics-werkruimte moet worden gebruikt voor logboekregistratie.
   4. Onder **Log**, selecteer de **IntegrationAccountTrackingEvents** categorie.
   5. Kies **Opslaan**.

   ![Log Analytics instellen zodat u diagnostische gegevens naar een logboek verzenden kunt](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Nu [bijhouden voor uw B2B-berichten in Log Analytics instellen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

<a name="azure-monitor-service"></a>

### <a name="turn-on-diagnostics-logging-through-azure-monitor"></a>Logboekregistratie van diagnostische gegevens via Azure Monitor inschakelen

1. In de [Azure-portal](https://portal.azure.com), kies in het hoofdmenu van Azure **Monitor**, **diagnoselogboeken**. Selecteer vervolgens uw integratie-account, zoals hier wordt weergegeven:

   ![Kies 'Controleren', 'Diagnostische logboeken', selecteert u uw integratieaccount](media/logic-apps-monitor-b2b-message/monitor-service-diagnostics-logs.png)

2. Nadat u uw integratie-account hebt geselecteerd, worden de volgende waarden worden automatisch geselecteerd. Als deze waarden correct zijn, kiest u **diagnostische gegevens inschakelen**. Anders selecteert u de waarden die u wilt:

   1. Onder **abonnement**, selecteert u de Azure-abonnement dat u met uw integratie-account gebruiken.
   2. Onder **resourcegroep**, selecteer de resourcegroep die u met uw integratie-account gebruikt.
   3. Onder **resourcetype**, selecteer **integratieaccounts**.
   4. Onder **Resource**, selecteert u uw integratieaccount.
   5. Kies **diagnostische gegevens inschakelen**.

   ![Diagnostische gegevens voor uw integratie-account instellen](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. Onder **diagnostische instellingen**, kiest u **op**.

   ![Azure diagnostische gegevens inschakelen](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Selecteer nu de Log Analytics-werkruimte en gebeurtenis categorie voor logboekregistratie, zoals wordt weergegeven:

   1. Selecteer **verzenden naar Log Analytics**. 
   2. Onder **Log Analytics**, kiest u **configureren**. 
   3. Onder **OMS-werkruimten**, selecteert u de Log Analytics-werkruimte moet worden gebruikt voor logboekregistratie.
   4. Onder **Log**, selecteer de **IntegrationAccountTrackingEvents** categorie.
   5. Als u bent klaar, kiest u **Opslaan**.

   ![Log Analytics instellen zodat u diagnostische gegevens naar een logboek verzenden kunt](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Nu [bijhouden voor uw B2B-berichten in Log Analytics instellen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Hoe en waar u de diagnostische gegevens gebruiken met andere services uitbreiden

Samen met Azure Log Analytics, kunt u uitbreiden hoe u diagnostische gegevens van uw logische app gebruiken met andere Azure-services, bijvoorbeeld: 

* [Azure Diagnostics-in Azure-opslag logboeken archiveren](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Stream diagnostische logboeken van Azure naar Azure Eventhubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

U kunt vervolgens get realtime bewaking met behulp van Telemetrie en analyses van andere services, zoals [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) en [Power BI](../log-analytics/log-analytics-powerbi.md). Bijvoorbeeld:

* [Stream-gegevens van Gebeurtenishubs met Stream Analytics](../stream-analytics/stream-analytics-define-inputs.md)
* [Met Stream Analytics streaming-gegevens analyseren en een realtime analytics-dashboard maken in Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Op basis van de opties die u wilt instellen, zorg ervoor dat u eerst [maken van een Azure storage-account](../storage/common/storage-create-storage-account.md) of [maken van een Azure event hub](../event-hubs/event-hubs-create.md). Selecteer vervolgens de opties voor waar u voor het verzenden van diagnostische gegevens:

![Gegevens verzenden naar Azure storage-account of event hub](./media/logic-apps-monitor-b2b-message/storage-account-event-hubs.png)

> [!NOTE]
> Bewaarperioden langer dan gelden alleen wanneer u wilt gebruiken van een storage-account.

## <a name="supported-tracking-schemas"></a>Ondersteunde volgschema 's

Azure biedt ondersteuning voor deze tracering schematypen waarin alle schema's met uitzondering van het type aangepast hebt opgelost.

* [Volgschema voor AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Volgschema voor X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Aangepaste volgschema's](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Volgende stappen

* [B2B-berichten bijhouden in Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "bijhouden B2B-berichten in OMS")
* [Meer informatie over het Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")

