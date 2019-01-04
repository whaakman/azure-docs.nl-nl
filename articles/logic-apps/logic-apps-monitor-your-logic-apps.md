---
title: Controleer de status en ontvang waarschuwingen - Azure Logic Apps-logboekregistratie instellen | Microsoft Docs
description: Status controleren, meld u diagnostische gegevens en waarschuwingen instellen voor Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 5c1b1e15-3b6c-49dc-98a6-bdbe7cb75339
ms.date: 07/21/2017
ms.openlocfilehash: e06bf20a04c6a57ae5988d4cc334ec7a3cdd4bf1
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53543853"
---
# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a>Status controleren, instellen van logboekregistratie van diagnostische gegevens en waarschuwingen inschakelen voor Azure Logic Apps

Nadat u [maken en uitvoeren van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md), kunt u de geschiedenis van uitvoeringen, triggergeschiedenis, status en prestaties controleren. Instellen voor gebeurtenissen in realtime bewaking en uitgebreidere foutopsporing, [Diagnostische logboekregistratie](#azure-diagnostics) voor uw logische app. Op die manier kunt u [vinden en weergeven van gebeurtenissen](#find-events), zoals de triggergebeurtenissen, voer gebeurtenissen en actie. U kunt dit ook gebruiken [diagnostische gegevens met andere services](#extend-diagnostic-data), zoals Azure Storage en Azure Event Hubs. 

Instellen als u meldingen over fouten of andere mogelijke problemen, [waarschuwingen](#add-azure-alerts). Bijvoorbeeld, kunt u een waarschuwing waarmee wordt gedetecteerd "wanneer meer dan vijf uitvoering mislukt binnen een uur." U kunt ook instellen van controle, tracering en logboekregistratie via een programma met behulp van [Azure Diagnostics gebeurtenisinstellingen en eigenschappen](#diagnostic-event-properties).

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a>Uitvoeringen weergeven en de triggergeschiedenis voor uw logische app

1. Om te vinden van uw logische app in de [Azure-portal](https://portal.azure.com), kies in het hoofdmenu van Azure **alle services**. In het zoekvak, typ 'logic apps' en kies **Logic apps**.

   ![Uw logische app zoeken](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   De Azure-portal ziet u alle logic-apps die gekoppeld aan uw Azure-abonnement zijn. 

2. Selecteer uw logische app, en kies vervolgens **overzicht**.

   De Azure-portal ziet u de geschiedenis van uitvoeringen en de triggergeschiedenis voor uw logische app. Bijvoorbeeld:

   ![Geschiedenis-en triggergeschiedenis uitvoeringen van logische app](media/logic-apps-monitor-your-logic-apps/overview.png)

   * **Geschiedenis van uitvoeringen** toont alle uitvoeringen voor uw logische app. 
   * **Triggergeschiedenis** ziet u de trigger-activiteit voor uw logische app.

   Zie voor statusbeschrijvingen, [uw logische app oplossen](../logic-apps/logic-apps-diagnosing-failures.md).

   > [!TIP]
   > Als u niet de gegevens die u, op de werkbalk verwacht, kies **vernieuwen**.

3. Om weer te geven van de stappen in een specifieke uitvoering onder **geschiedenis van uitvoeringen**, selecteert u die worden uitgevoerd. 

   De monitor-weergave bevat elke stap in die worden uitgevoerd. Bijvoorbeeld:

   ![Acties voor het uitvoeren van specifieke](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. Voor meer informatie over de uitvoering, kies **Details uitvoering van**. Deze informatie bevat een overzicht van de stappen, status, invoer en uitvoer voor de uitvoering. 

   ![Kies "Details uitvoering van"](media/logic-apps-monitor-your-logic-apps/run-details.png)

   Bijvoorbeeld, krijgt u de uitvoering van **correlatie-ID**, die u mogelijk nodig hebt wanneer u de [REST-API voor Logic Apps](https://docs.microsoft.com/rest/api/logic).

5. Als u meer informatie over een specifieke stap, kiest u deze stap. U kunt nu gedetailleerde gegevens als invoer, uitvoer en eventuele fouten die hebben plaatsgevonden voor deze stap. Bijvoorbeeld:

   ![Details van querystap](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > Alle details van de runtime en gebeurtenissen worden in de Logic Apps-service versleuteld. Ze zijn ontsleuteld alleen wanneer een gebruiker vraagt om die gegevens weer te geven. U kunt ook toegang tot deze gebeurtenissen met [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md).

6. Als u meer informatie over een specifieke triggergebeurtenis, gaat u terug naar de **overzicht** deelvenster. Onder **Triggergeschiedenis**, selecteert u de triggergebeurtenis. U kunt nu gegevens, zoals de invoer en uitvoer, bijvoorbeeld bekijken:

   ![Gebeurtenisdetails van trigger voor uitvoer](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a>Diagnostische gegevens voor uw logische app logboekregistratie inschakelen

Voor uitgebreidere foutopsporing met details van de runtime en gebeurtenissen, kunt u Diagnostische logboekregistratie met instellen [Azure Log Analytics](../log-analytics/log-analytics-overview.md). Log Analytics is een service in Azure waarmee u uw cloud en on-premises omgevingen om te helpen u hun beschikbaarheid en prestaties te onderhouden. 

Voordat u begint, moet u een Log Analytics-werkruimte. Informatie over [over het maken van een Log Analytics-werkruimte](../azure-monitor/learn/quick-create-workspace.md).

1. In de [Azure-portal](https://portal.azure.com), zoek en selecteer uw logische app. 

2. In het menu van de blade in de logische app, onder **bewaking**, kiest u **Diagnostics** > **diagnostische instellingen**.

   ![Ga naar bewaking, diagnose, diagnostische instellingen](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. Onder **diagnostische instellingen**, kiest u **op**.

   ![Logboeken met diagnostische gegevens inschakelen](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. Selecteer nu de Log Analytics-werkruimte en gebeurtenis categorie voor logboekregistratie, zoals wordt weergegeven:

   1. Selecteer **verzenden naar Log Analytics**. 
   2. Onder **Log Analytics**, kiest u **configureren**. 
   3. Onder **OMS-werkruimten**, selecteert u de werkruimte moet worden gebruikt voor logboekregistratie.
   > [!NOTE]
   > OMS-werkruimten worden nu aangeduid als Log Analytics-werkruimten.
   4. Onder **Log**, selecteer de **WorkflowRuntime** categorie.
   5. Kies het interval voor metrische gegevens.
   6. Als u bent klaar, kiest u **Opslaan**.

   ![Selecteer Log Analytics-werkruimte en de gegevens voor logboekregistratie](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

U kunt nu, gebeurtenissen en andere gegevens vinden voor de trigger-gebeurtenissen, gebeurtenissen en actie uitvoeren.

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a>Gegevens over gebeurtenissen en vinden voor uw logische app

Als u wilt zoeken en weergeven van gebeurtenissen in uw logische app, zoals gebeurtenissen, gebeurtenissen, uitvoeren en actie activeren, volgt u deze stappen.

1. In de [Azure-portal](https://portal.azure.com), kiest u **alle Services**. Zoeken naar "log analytics" en kies vervolgens **Log Analytics** zoals hier wordt weergegeven:

   ![Kies "Log Analytics"](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. Onder **Log Analytics**, zoek en selecteer uw Log Analytics-werkruimte. 

   ![Selecteer uw Log Analytics-werkruimte](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. Onder **Management**, kiest u **zoeken in logboeken**.

   ![Kies "Zoeken in Logboeken"](media/logic-apps-monitor-your-logic-apps/log-search.png)

4. In het zoekvak typt, geeft u een veld dat u wilt zoeken, en druk op **Enter**. Wanneer u te typen begint, ziet u mogelijke overeenkomsten en bewerkingen die u kunt gebruiken. 

   Bijvoorbeeld, als u zoekt de top 10-gebeurtenissen die hebben plaatsgevonden, invoeren en deze zoekopdracht selecteren: **zoeken naar categorie == "WorkflowRuntime" | 10 beperken**

   ![Geef zoektekenreeks](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   Meer informatie over [gegevens zoeken in Log Analytics](../log-analytics/log-analytics-log-searches.md).

5. Kies op de pagina met resultaten in de linkerbalk de periode die u wilt weergeven.
Als u wilt uw query te verfijnen door een filter toe te voegen, kies **+ toevoegen**.

   ![Kies tijdsbestek voor de resultaten van query](media/logic-apps-monitor-your-logic-apps/query-results.png)

6. Onder **Filters toevoegen**, voer de naam van het filter zodat u kunt het filter dat u wilt zoeken. Selecteer het filter en kies **+ toevoegen**.

   In dit voorbeeld wordt het woord 'status' mislukt-gebeurtenissen onder vinden **AzureDiagnostics**.
   Hier het filter voor **status_s** is al geselecteerd.

   ![Filter selecteren](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

7. Selecteer de filterwaarde die u wilt gebruiken, en kies in de linkerbalk **toepassen**.

   ![De filterwaarde selecteert, kiest u 'Toepassen'](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

8. Ga nu terug naar de query die u bouwt. Uw query wordt bijgewerkt met het geselecteerde filter en de waarde. De resultaten van uw vorige worden nu te gefilterd.

   ![Ga terug naar uw query's uitvoeren met de gefilterde resultaten](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

9. Sla de query voor toekomstig gebruik, kies **opslaan**. Informatie over [het opslaan van uw query](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Hoe en waar u de diagnostische gegevens gebruiken met andere services uitbreiden

Samen met Azure Log Analytics, kunt u uitbreiden hoe u diagnostische gegevens van uw logische app gebruiken met andere Azure-services, bijvoorbeeld: 

* [Azure Diagnostics-in Azure-opslag logboeken archiveren](../azure-monitor/platform/archive-diagnostic-logs.md)
* [Stream diagnostische logboeken van Azure naar Azure Eventhubs](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md) 

U kunt vervolgens get realtime bewaking met behulp van Telemetrie en analyses van andere services, zoals [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) en [Power BI](../azure-monitor/platform/powerbi.md). Bijvoorbeeld:

* [Stream-gegevens van Gebeurtenishubs met Stream Analytics](../stream-analytics/stream-analytics-define-inputs.md)
* [Met Stream Analytics streaming-gegevens analyseren en een realtime analytics-dashboard maken in Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Op basis van de opties die u wilt instellen, zorg ervoor dat u eerst [maken van een Azure storage-account](../storage/common/storage-create-storage-account.md) of [maken van een Azure event hub](../event-hubs/event-hubs-create.md). Selecteer vervolgens de opties voor waar u voor het verzenden van diagnostische gegevens:

![Gegevens verzenden naar Azure storage-account of event hub](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> Bewaarperioden langer dan gelden alleen wanneer u wilt gebruiken van een storage-account.

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a>Waarschuwingen instellen voor uw logische app

Instellen voor het bewaken van specifieke metrische gegevens of overschreden drempelwaarden voor uw logische app, [waarschuwingen in Azure](../azure-monitor/platform/alerts-overview.md). Meer informatie over [metrische gegevens in Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md). 

Voor het instellen van waarschuwingen zonder [Azure Log Analytics](../log-analytics/log-analytics-overview.md), als volgt te werk. Voor meer geavanceerde criteria voor waarschuwingen en acties, [Log Analytics instellen](#azure-diagnostics) te.

1. In het menu van de blade in de logische app, onder **bewaking**, kiest u **Diagnostics** > **waarschuwingsregels** > **waarschuwing toevoegen**zoals hier wordt weergegeven:

   ![Een waarschuwing voor uw logische app toevoegen](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. Op de **een waarschuwingsregel toevoegen** blade maken van de waarschuwing, zoals wordt weergegeven:

   1. Onder **Resource**, selecteert u uw logische app, als dit nog is geselecteerd. 
   2. Geef een naam en beschrijving voor de waarschuwing.
   3. Selecteer een **Metric** of gebeurtenis die u wilt bijhouden.
   4. Selecteer een **voorwaarde**, Geef een **drempelwaarde** voor de metrische gegevens en selecteer de **periode** voor het bewaken van deze metrische gegevens.
   5. Selecteer of voor het verzenden van e-mail voor de waarschuwing. 
   6. Geef een andere e-mailadressen voor het verzenden van de waarschuwing. 
   U kunt ook opgeven waar u voor het verzenden van de waarschuwing een webhook-URL.

   Bijvoorbeeld: met deze regel verzendt een waarschuwing wanneer vijf of meer uitvoering mislukt binnen een uur:

   ![Maken van waarschuwingsregel voor metrische gegevens](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> Voor een logische app uitvoeren vanaf een waarschuwing, u kunt opnemen de [aanvraagtrigger](../connectors/connectors-native-reqres.md) in uw werkstroom waarmee u taken uitvoeren, zoals deze voorbeelden:
> 
> * [Berichten posten op Slack](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [Een SMS verzenden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [Een bericht toevoegen aan een wachtrij](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a>Azure event-instellingen voor diagnostische gegevens en gegevens

Elke diagnostische gebeurtenis heeft meer informatie over uw logische app en die gebeurtenis, bijvoorbeeld de status, begintijd, eindtijd, en enzovoort. Als u bewaking, tracering en logboekregistratie programmatisch instelt, kunt u deze gegevens met de [REST-API voor Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) en de [REST-API voor Azure Diagnostics](../azure-monitor/platform/metrics-supported.md#microsoftlogicworkflows).

Bijvoorbeeld, de `ActionCompleted` gebeurtenis heeft de `clientTrackingId` en `trackedProperties` eigenschappen die u gebruiken kunt voor het bijhouden en controleren:

``` json
{
    "time": "2016-07-09T17:09:54.4773148Z",
    "workflowId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
    "resourceId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
    "category": "WorkflowRuntime",
    "level": "Information",
    "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
    "properties": {
        "$schema": "2016-06-01",
        "startTime": "2016-07-09T17:09:53.4336305Z",
        "endTime": "2016-07-09T17:09:53.5430281Z",
        "status": "Succeeded",
        "code": "OK",
        "resource": {
            "subscriptionId": "<subscription-ID>",
            "resourceGroupName": "MyResourceGroup",
            "workflowId": "cff00d5458f944d5a766f2f9ad142553",
            "workflowName": "MyLogicApp",
            "runId": "08587361146922712057",
            "location": "westus",
            "actionName": "Http"
        },
        "correlation": {
            "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
            "clientTrackingId": "<my-custom-tracking-ID>"
        },
        "trackedProperties": {
            "myTrackedProperty": "<value>"
        }
    }
}
```

* `clientTrackingId`: Als dat niet opgegeven, wordt Azure automatisch deze ID gegenereerd en gebeurtenissen in een logische app verbindt met inbegrip van eventuele geneste werkstromen die worden aangeroepen vanuit de logische app. U kunt deze ID vanuit een trigger handmatig opgeven door door te geven een `x-ms-client-tracking-id` -header met de aangepaste id-waarde in de aanvraag van de trigger. U kunt een aanvraag als trigger, HTTP-trigger of webhook-trigger.

* `trackedProperties`: Als u wilt bijhouden in- of uitvoer in diagnostische gegevens, kunt u de bijgehouden eigenschappen toevoegen aan acties in de JSON-definitie van uw logische app. Bijgehouden eigenschappen kunnen volgen slechts één actie-invoer en uitvoer, maar u kunt de `correlation` eigenschappen van gebeurtenissen te correleren acties in een uitvoering.

  Voor het volgen van een of meer eigenschappen toevoegen de `trackedProperties` sectie en de eigenschappen die u de definitie van de actie wilt. Stel bijvoorbeeld dat u wilt bijhouden van gegevens, zoals een 'order-ID' in uw telemetrie:

  ``` json
  "myAction": {
    "type": "http",
    "inputs": {
        "uri": "http://uri",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": "@triggerBody()"
    },
    "trackedProperties": {
        "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
        "myActionHTTPValue": "@action()['outputs']['body']['<content>']",
        "transactionId": "@action()['inputs']['body']['<content>']"
    }
  }
  ```

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen maken voor logische app-implementatie en beheer van vrij](../logic-apps/logic-apps-create-deploy-template.md)
* [B2B-scenario's met Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [B2B-berichten bewaken](../logic-apps/logic-apps-monitor-b2b-message.md)
