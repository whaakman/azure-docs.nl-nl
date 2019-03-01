---
title: Bewaken met Azure Monitor-logs - Azure Logic Apps B2B-berichten | Microsoft Docs
description: AS2, X 12 en EDIFACT-berichten voor integratieaccounts en Azure Logic Apps bewaken en het instellen van logboekregistratie van diagnostische gegevens met Azure Monitor-Logboeken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 10/23/2018
ms.openlocfilehash: 12799a308157c3c0e19de1f82c0fe3df44fad37e
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57195158"
---
# <a name="monitor-b2b-messages-with-azure-monitor-logs-in-azure-logic-apps"></a>B2B-berichten met Azure Monitor-Logboeken in Azure Logic Apps bewaken

Na het instellen van B2B-communicatie tussen handelspartners in uw integratie-account, kan deze partners berichten met elkaar kunnen uitwisselen. Om te controleren dat deze communicatie werkt zoals verwacht, u AS2, X12, bewaken kunt en EDIFACT-berichten en het instellen van diagnostische gegevens van logboekregistratie voor uw integratie-account met [logboeken van Azure Monitor](../log-analytics/log-analytics-overview.md). Deze service controleert uw cloud en on-premises omgevingen, helpen u hun beschikbaarheid en prestaties te onderhouden en verzamelt runtime-gegevens en gebeurtenissen voor uitgebreidere foutopsporing. U kunt deze gegevens ook gebruiken met andere services zoals Azure Storage en Azure Event Hubs.

> [!NOTE]
> Deze pagina mogelijk nog steeds verwijzingen naar Microsoft Operations Management Suite (OMS), die is [buiten gebruik stellen in januari 2019](../azure-monitor/platform/oms-portal-transition.md), maar deze stappen vervangt met Azure Log Analytics waar mogelijk. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Vereisten

* Een logische app die ingesteld met de logboekregistratie van diagnostische gegevens. Informatie over [over het maken van een logische app](quickstart-create-first-logic-app-workflow.md) en [over het instellen van logboekregistratie voor deze logische app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Nadat u voldoet aan de bovenstaande vereisten, moet u ook een Log Analytics-werkruimte die u gebruiken voor het controleren en bijhouden van B2B-communicatie via Azure Monitor-Logboeken. Als u een Log Analytics-werkruimte hebt, krijgt u informatie [over het maken van een Log Analytics-werkruimte](../azure-monitor/learn/quick-create-workspace.md).

* Een integratieaccount dat gekoppeld aan uw logische app. Informatie over [hoe u een integratieaccount maakt met een koppeling naar uw logische app](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

## <a name="turn-on-diagnostics-logging"></a>Diagnostische logboekregistratie inschakelen

U kunt inschakelen logboekregistratie rechtstreeks vanaf uw integratie-account of [via de service Azure Monitor](#azure-monitor-service). Azure Monitor biedt basiscontrole met gegevens op infrastructuurniveau. Meer informatie over [Azure Monitor](../azure-monitor/overview.md).

### <a name="turn-on-logging-from-integration-account"></a>Logboekregistratie van integratie-account inschakelen

1. In de [Azure-portal](https://portal.azure.com), zoek en selecteer uw integratie-account. Onder **bewaking**, selecteer **diagnostische instellingen**.

   ![Zoek en selecteer uw integratieaccount, selecteert u 'Diagnostische instellingen'](media/logic-apps-monitor-b2b-message/find-integration-account.png)

1. Nu zoeken en selecteer uw integratieaccount. In een lijst met een filter, selecteert u de waarden die betrekking hebben op uw integratie-account.
Wanneer u klaar bent, kiest u **diagnostische instelling toevoegen**.

   | Eigenschap | Waarde | Description | 
   |----------|-------|-------------|
   | **Abonnement** | <*Azure-subscription-name*> | De Azure-abonnement dat is gekoppeld aan uw integratie-account | 
   | **Resourcegroep** | <*Azure-resource-group-name*> | De Azure-resourcegroep voor uw integratie-account | 
   | **Resourcetype** | **Integratieaccounts** | Het type voor de Azure-resource waar u wilt logboekregistratie inschakelen | 
   | **Resource** | <*integration-account-name*> | De naam voor uw Azure-resource waar u wilt logboekregistratie inschakelen | 
   ||||  

   Bijvoorbeeld:

   ![Diagnostische gegevens voor uw integratie-account instellen](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

1. Geef een naam op voor uw nieuwe diagnostische instelling en selecteer uw Log Analytics-werkruimte en de gegevens die u wilt registreren.

   1. Selecteer **verzenden naar Log Analytics**. 

   1. Onder **Log Analytics**, selecteer **configureren**. 

   1. Onder **OMS-werkruimten**, selecteert u de Log Analytics-werkruimte die u wilt gebruiken voor aanmelding. 

      > [!NOTE]
      > OMS-werkruimten worden vervangen door Log Analytics-werkruimten. 

   1. Onder **Log**, selecteer de **IntegrationAccountTrackingEvents** categorie, en kies **opslaan**.

   Bijvoorbeeld: 

   ![Logboeken van Azure Monitor instellen, zodat u diagnostische gegevens naar een logboek verzenden kunt](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

1. Nu [bijhouden voor uw B2B-berichten in de logboeken van Azure Monitor instellen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

<a name="azure-monitor-service"></a>

### <a name="turn-on-logging-through-azure-monitor"></a>Schakel logboekregistratie in via Azure Monitor

1. In de [Azure-portal](https://portal.azure.com), selecteer in het hoofdmenu van Azure **Monitor**. Onder **instellingen**, selecteer **diagnostische instellingen**. 

   ![Selecteer 'Monitor' > "Diagnostische instellingen" > uw integratie-account](media/logic-apps-monitor-b2b-message/monitor-diagnostics-settings.png)

1. Nu zoeken en selecteer uw integratieaccount. In een lijst met een filter, selecteert u de waarden die betrekking hebben op uw integratie-account.
Wanneer u klaar bent, kiest u **diagnostische instelling toevoegen**.

   | Eigenschap | Waarde | Description | 
   |----------|-------|-------------|
   | **Abonnement** | <*Azure-subscription-name*> | De Azure-abonnement dat is gekoppeld aan uw integratie-account | 
   | **Resourcegroep** | <*Azure-resource-group-name*> | De Azure-resourcegroep voor uw integratie-account | 
   | **Resourcetype** | **Integratieaccounts** | Het type voor de Azure-resource waar u wilt logboekregistratie inschakelen | 
   | **Resource** | <*integration-account-name*> | De naam voor uw Azure-resource waar u wilt logboekregistratie inschakelen | 
   ||||  

   Bijvoorbeeld:

   ![Diagnostische gegevens voor uw integratie-account instellen](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

1. Geef een naam op voor uw nieuwe diagnostische instelling en selecteer uw Log Analytics-werkruimte en de gegevens die u wilt registreren.

   1. Selecteer **verzenden naar Log Analytics**. 

   1. Onder **Log Analytics**, selecteer **configureren**. 

   1. Onder **OMS-werkruimten**, selecteert u de Log Analytics-werkruimte die u wilt gebruiken voor aanmelding. 

      > [!NOTE]
      > OMS-werkruimten worden vervangen door Log Analytics-werkruimten. 

   1. Onder **Log**, selecteer de **IntegrationAccountTrackingEvents** categorie, en kies **opslaan**.

   Bijvoorbeeld: 

   ![Logboeken van Azure Monitor instellen, zodat u diagnostische gegevens naar een logboek verzenden kunt](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

1. Nu [bijhouden voor uw B2B-berichten in de logboeken van Azure Monitor instellen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

## <a name="use-diagnostic-data-with-other-services"></a>Diagnostische gegevens met andere services gebruiken

Samen met Azure Monitor-Logboeken, kunt u uitbreiden hoe u diagnostische gegevens van uw logische app gebruiken met andere Azure-services, bijvoorbeeld: 

* [Azure Diagnostics-in Azure-opslag logboeken archiveren](../azure-monitor/platform/archive-diagnostic-logs.md)
* [Stream diagnostische logboeken van Azure naar Azure Eventhubs](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md) 

U kunt vervolgens get realtime bewaking met behulp van Telemetrie en analyses van andere services, zoals [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) en [Power BI](../azure-monitor/platform/powerbi.md). Bijvoorbeeld:

* [Stream-gegevens van Gebeurtenishubs met Stream Analytics](../stream-analytics/stream-analytics-define-inputs.md)
* [Met Stream Analytics streaming-gegevens analyseren en een realtime analytics-dashboard maken in Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Op basis van de opties die u instellen wilt, zorg ervoor dat u eerst [maken van een Azure storage-account](../storage/common/storage-create-storage-account.md) of [maken van een Azure event hub](../event-hubs/event-hubs-create.md). Vervolgens kunt u de doelen waar u voor het verzenden van diagnostische gegevens.
Bewaarperioden langer dan gelden alleen wanneer u wilt gebruiken van een storage-account.

![Gegevens verzenden naar Azure storage-account of event hub](./media/logic-apps-monitor-b2b-message/diagnostics-storage-event-hub-log-analytics.png)

## <a name="supported-tracking-schemas"></a>Ondersteunde volgschema 's

Azure biedt ondersteuning voor deze tracering schematypen waarin alle schema's met uitzondering van het type aangepast hebt opgelost.

* [Volgschema voor AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Volgschema voor X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Aangepaste volgschema's](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Volgende stappen

* [B2B-berichten bijhouden in Logboeken van Azure Monitor](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "bijhouden B2B-berichten in de logboeken van Azure Monitor")
* [Meer informatie over het Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")

