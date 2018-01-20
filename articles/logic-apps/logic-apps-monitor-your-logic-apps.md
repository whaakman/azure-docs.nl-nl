---
title: Controleer de status, logboekregistratie instellen en ontvang waarschuwingen - Azure Logic Apps | Microsoft Docs
description: Status en prestaties voor logische apps bewaken, meld u diagnostische gegevens en waarschuwingen instellen
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 5c1b1e15-3b6c-49dc-98a6-bdbe7cb75339
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 8ad8281744293a05b50f8664a7e5a3fea7aa7b33
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a>Status controleren, instellen van logboekregistratie van diagnostische gegevens en waarschuwingen inschakelen voor Azure Logic Apps

Nadat u [maken en uitvoeren van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md), u kunt de geschiedenis wordt uitgevoerd, trigger geschiedenis, status en prestaties controleren. Instellen voor het bewaken van realtime-gebeurtenissen en uitgebreidere foutopsporing, [logboekregistratie van diagnostische gegevens](#azure-diagnostics) voor uw logische app. Op die manier kunt u [zoeken en weergeven van gebeurtenissen](#find-events), zoals de trigger-gebeurtenissen, voer gebeurtenissen en in te grijpen. U kunt dit ook gebruiken [diagnostics-gegevens met andere services](#extend-diagnostic-data), zoals Azure Storage en Azure Event Hubs. 

Als u meldingen over fouten of andere mogelijke problemen, instellen van [waarschuwingen](#add-azure-alerts). Bijvoorbeeld, kunt u een waarschuwing die detecteert 'wanneer meer dan vijf wordt uitgevoerd niet in een uur." U kunt ook instellen bewaking, bijhouden en logboekregistratie programmatisch met behulp van [Azure Diagnostics gebeurtenisinstellingen en eigenschappen](#diagnostic-event-properties).

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a>Weergave wordt uitgevoerd en geschiedenis van trigger voor uw logische app

1. Vinden van uw logische app in de [Azure-portal](https://portal.azure.com), kiest u in het Azure hoofdmenu **meer services**. In het zoekvak 'logische apps' vinden en kies **Logic apps**.

   ![Uw logische app zoeken](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   De Azure-portal bevat alle logic-apps die gekoppeld aan uw Azure-abonnement zijn. 

2. Selecteer uw logische app, en kies vervolgens **overzicht**.

   De Azure-portal ziet u de geschiedenis wordt uitgevoerd en de geschiedenis van trigger voor uw logische app. Bijvoorbeeld:

   ![Logische app geschiedenis en trigger geschiedenis wordt uitgevoerd](media/logic-apps-monitor-your-logic-apps/overview.png)

   * **Wordt uitgevoerd geschiedenis** toont de uitvoert voor uw logische app. 
   * **Activeren van de geschiedenis** ziet u de triggeractiviteit voor uw logische app.

   Zie voor een beschrijving van status [problemen met uw logische app](../logic-apps/logic-apps-diagnosing-failures.md).

   > [!TIP]
   > Als u de gegevens die u, op de werkbalk verwacht niet kunt vinden kiezen **vernieuwen**.

3. Om weer te geven van de stappen van een specifieke uitvoeren onder **wordt uitgevoerd geschiedenis**, selecteren die worden uitgevoerd. 

   De weergave van de monitor geeft elke stap in die worden uitgevoerd. Bijvoorbeeld:

   ![Acties voor een specifieke uitvoeren](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. Voor meer informatie over het uitvoeren, kiest u **Details uitvoering van**. Deze informatie bevat een overzicht van de stappen, status, invoer en uitvoer voor de verwerking. 

   ![Kies 'Details uitvoeren'](media/logic-apps-monitor-your-logic-apps/run-details.png)

   Bijvoorbeeld, krijgt u de run **correlatie-ID**, die u mogelijk wanneer u de [REST-API voor Logic Apps](https://docs.microsoft.com/rest/api/logic).

5. Als u meer informatie over een specifieke stap, kiest u die stap. U kunt nu gegevens, zoals de invoer, uitvoer en eventuele fouten die hebben plaatsgevonden voor die stap bekijken. Bijvoorbeeld:

   ![Stap-details](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > Alle details van de runtime en gebeurtenissen worden in de service Logic Apps versleuteld. Ze worden alleen als een gebruiker vraagt om weer te geven dat de gegevens ontsleuteld. U kunt ook toegang tot deze gebeurtenissen met [gebaseerd toegangsbeheer (RBAC)](../active-directory/role-based-access-control-what-is.md).

6. Als u meer informatie over een specifieke triggergebeurtenis, gaat u terug naar de **overzicht** deelvenster. Onder **activeren geschiedenis**, selecteert u de triggergebeurtenis. U kunt nu gegevens, zoals de invoer en uitvoer, bijvoorbeeld bekijken:

   ![Gebeurtenisdetails van trigger voor uitvoer](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a>Diagnostische gegevens voor uw logische app logboekregistratie inschakelen

Voor uitgebreidere foutopsporing met details van de runtime en gebeurtenissen, kunt u zich aanmelden met diagnostische gegevens instellen [Azure Log Analytics](../log-analytics/log-analytics-overview.md). Log Analytics is een service in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) die wordt bewaakt uw cloud en on-premises omgevingen om te helpen uw behouden hun beschikbaarheid en prestaties. 

Voordat u begint, moet u een OMS-werkruimte hebt. Meer informatie over [het maken van een OMS-werkruimte](../log-analytics/log-analytics-get-started.md).

1. In de [Azure-portal](https://portal.azure.com), zoeken en selecteert u uw logische app. 

2. Klik in het menu logic app blade onder **bewaking**, kies **Diagnostics** > **diagnostische instellingen**.

   ![Ga naar de bewakings-, diagnose, diagnostische instellingen](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. Onder **diagnostische instellingen**, kies **op**.

   ![Logboeken met diagnostische gegevens inschakelen](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. Selecteer de OMS-werkruimte en gebeurtenis categorie voor logboekregistratie nu zoals wordt weergegeven:

   1. Selecteer **verzenden met logboekanalyse**. 
   2. Onder **logboekanalyse**, kies **configureren**. 
   3. Onder **OMS werkruimten**, selecteer de OMS-werkruimte moet worden gebruikt voor logboekregistratie.
   4. Onder **logboek**, selecteer de **WorkflowRuntime** categorie.
   5. Kies een metrische interval.
   6. Als u bent klaar, kiest u **opslaan**.

   ![Selecteer de OMS-werkruimte en de gegevens voor logboekregistratie](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

U kunt nu gebeurtenissen en andere gegevens zoeken voor trigger-gebeurtenissen, gebeurtenissen en actie uitgevoerd.

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a>Zoeken naar gebeurtenissen en gegevens voor uw logische app

Om te zoeken en weergeven van gebeurtenissen in uw logische app, zoals gebeurtenissen, gebeurtenissen, uitvoeren en actie activeren, als volgt te werk.

1. In de [Azure-portal](https://portal.azure.com), kies **meer Services**. Zoek naar 'log analytics' en kies vervolgens **logboekanalyse** als volgt te werk:

   ![Kies 'Log Analytics'](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. Onder **logboekanalyse**, zoeken en selecteert u de OMS-werkruimte. 

   ![Selecteer de OMS-werkruimte](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. Onder **Management**, kies **OMS-Portal**.

   ![Kies 'OMS-Portal'](media/logic-apps-monitor-your-logic-apps/omsportalpage.png)

4. Kies op de startpagina OMS **logboek zoeken**.

   ![Kies op uw startpagina OMS "Logboek zoeken"](media/logic-apps-monitor-your-logic-apps/logsearch.png)

   -of-

   ![Kies in het menu OMS "Logboek zoeken"](media/logic-apps-monitor-your-logic-apps/logsearch-2.png)

5. Geef in het zoekvak, een veld dat u wilt zoeken en druk op **Enter**. Wanneer u te typen begint, OMS laat zien u mogelijke overeenkomsten en de bewerkingen die u kunt gebruiken. 

   Bijvoorbeeld als u zoekt de top 10-gebeurtenissen die hebben plaatsgevonden, invoeren en deze zoekopdracht selecteren: **categorie WorkflowRuntime = | top 10**

   ![Geef zoektekenreeks](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   Meer informatie over [hoe u gegevens wilt zoeken in logboekanalyse](../log-analytics/log-analytics-log-searches.md).

6. Kies de periode die u wilt weergeven in de linkerbalk op de resultatenpagina.
Als u wilt uw query te verfijnen door een filter toe te voegen, kies **+ toevoegen**.

   ![Kies tijdskader voor de queryresultaten](media/logic-apps-monitor-your-logic-apps/query-results.png)

7. Onder **Filters toevoegen**, voer de naam van het filter zodat u kunt het filter dat u wilt zoeken. Selecteer het filter en kies **+ toevoegen**.

   In dit voorbeeld wordt het woord 'status' mislukt-gebeurtenissen onder vinden **AzureDiagnostics**.
   Hier het filter voor **status_s** is al geselecteerd.

   ![Selecteer filter](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

8. Selecteer de filterwaarde die u wilt gebruiken en kies in de linkerbalk **toepassen**.

   ![Filterwaarde selecteert, kiest u 'Toepassen'](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

9. Nu kunt u terugkeren naar de query die u maakt. De query wordt bijgewerkt met het geselecteerde filter en de waarde. De resultaten van uw vorige worden nu te gefilterd.

   ![Ga terug naar uw query met gefilterde resultaten](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

10. Sla de query voor toekomstig gebruik, kies **opslaan**. Meer informatie over [het opslaan van uw query](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Hoe en waar u diagnostische gegevens gebruikt met andere services uitbreiden

U kunt samen met Azure Log Analytics uitbreiden hoe u diagnostische gegevens van uw logische app gebruiken met andere Azure-services, bijvoorbeeld: 

* [Archief Azure Diagnostics wordt geregistreerd in Azure Storage](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Azure Diagnostics logboeken naar Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

U kunt vervolgens get real-time bewaking met Telemetrie en analyses van andere services, zoals [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) en [Power BI](../log-analytics/log-analytics-powerbi.md). Bijvoorbeeld:

* [Stroomgegevens uit Event Hubs met Stream Analytics](../stream-analytics/stream-analytics-define-inputs.md)
* [Streaming gegevens analyseren met Stream Analytics en een realtime analytics-dashboard maken in Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Op basis van de opties die u wilt instellen, zorg ervoor dat u eerste [maken van een Azure storage-account](../storage/common/storage-create-storage-account.md) of [maken van een Azure event hub](../event-hubs/event-hubs-create.md). Selecteer de opties voor waar u diagnostische gegevens verzenden:

![Gegevens verzenden naar Azure storage-account of event hub](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> Bewaarperiode gelden alleen wanneer u kiest voor een opslagaccount.

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a>Waarschuwingen voor uw logische app instellen

Instellen voor het bewaken van specifieke metrische gegevens of overschreden drempelwaarden voor uw logische app [waarschuwingen in Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md). Meer informatie over [metrische gegevens in Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md). 

Voor het instellen van waarschuwingen zonder [Azure Log Analytics](../log-analytics/log-analytics-overview.md), als volgt te werk. Voor meer geavanceerde criteria voor waarschuwingen en acties, [logboekanalyse instellen](#azure-diagnostics) te.

1. In het menu logic app blade onder **bewaking**, kies **Diagnostics** > **waarschuwing regels** > **waarschuwing toevoegen** als volgt te werk:

   ![Een waarschuwing voor uw logische app toevoegen](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. Op de **een waarschuwingsregel toevoegen** blade maken van de waarschuwing, zoals wordt weergegeven:

   1. Onder **Resource**, selecteer uw logische app, als dat niet is gebeurd. 
   2. Geef een naam en beschrijving voor de waarschuwing.
   3. Selecteer een **metriek** of gebeurtenis die u wilt traceren.
   4. Selecteer een **voorwaarde**, Geef een **drempelwaarde** voor de metriek, en selecteer de **periode** voor het bewaken van deze metrische gegevens.
   5. Geef aan of e-mail voor de waarschuwing te verzenden. 
   6. Geef een andere e-mailadressen voor het verzenden van de waarschuwing. 
   U kunt ook opgeven van een webhook-URL waarvoor de waarschuwing te verzenden.

   Bijvoorbeeld: met deze regel verzendt een waarschuwing wanneer vijf of meer wordt uitgevoerd in een uur mislukken:

   ![Metrische waarschuwingsregel maken](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> Voor een logische app uitvoeren vanaf een waarschuwing, u kunt opnemen de [aanvraag trigger](../connectors/connectors-native-reqres.md) in uw werkstroom waarmee u taken, zoals deze voorbeelden uitvoeren:
> 
> * [Boeken naar ongebruikt](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [Een tekst verzenden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [Een bericht toevoegen aan een wachtrij](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a>Instellingen van Azure Diagnostics-gebeurtenis en details

Elke diagnostische gebeurtenissen voor meer informatie over uw logische app en dat de gebeurtenis, bijvoorbeeld de status heeft, begintijd, eindtijd en enzovoort. Als u bewaking, bijhouden en logboekregistratie programmatisch instelt, kunt organisatie-eenheid deze details met de [REST-API voor Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) en de [REST-API voor Azure Diagnostics](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).

Bijvoorbeeld, de `ActionCompleted` gebeurtenis heeft de `clientTrackingId` en `trackedProperties` eigenschappen die u gebruiken kunt voor bewaking en bij te houden:

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

* `clientTrackingId`: Als dat niet het opgegeven Azure automatisch deze ID wordt gegenereerd en gebeurtenissen via een logische app uitgevoerd verbindt, inclusief alle geneste werkstromen die worden aangeroepen vanuit de logische app. U kunt deze ID van een trigger handmatig opgeven door een `x-ms-client-tracking-id` header met uw aangepaste id-waarde in de aanvraag van de trigger. U kunt een aanvraag trigger, HTTP-trigger of webhook trigger.

* `trackedProperties`: Als u wilt in- of uitgangen in diagnostische gegevens bijhouden, kunt u bijgehouden eigenschappen toevoegen aan de acties in uw logische app JSON-definitie. Bijgehouden eigenschappen kunnen volgen slechts één actie in- en uitgangen, maar u kunt de `correlation` eigenschappen van gebeurtenissen die correleren over acties in een uitvoering.

  Om bij te houden op een of meer eigenschappen, voeg de `trackedProperties` sectie en de eigenschappen die u de definitie van de actie wilt. Stel bijvoorbeeld dat u wilt bijhouden van gegevens, zoals een 'volgorde-ID' in uw telemetrie:

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

* [Sjablonen maken voor logic app-implementatie en releasebeheer](../logic-apps/logic-apps-create-deploy-template.md)
* [B2B-scenario's met Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [B2B-berichten bewaken](../logic-apps/logic-apps-monitor-b2b-message.md)