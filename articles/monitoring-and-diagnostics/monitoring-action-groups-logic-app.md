---
title: Het activeren van complexe bewerkingen met Azure-waarschuwingen en actiegroepen
description: Informatie over het maken van een logische app-actie voor het verwerken van de Azure-waarschuwingen.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 14e562234152d2f1f2f2d2b57b34cd5724d3dd14
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753090"
---
# <a name="create-a-logic-app-action"></a>Maken van een logische app actie

In dit artikel leest u hoe instellen en het activeren van een logische app voor het maken van een conversatie in Microsoft-Teams wanneer een waarschuwing wordt geactiveerd.

## <a name="overview"></a>Overzicht
Wanneer een waarschuwing Azure veroorzaakt, ontvangt een [actiegroep](monitoring-action-groups.md). Actiegroepen kunnen u voor het activeren van een of meer acties om anderen te informeren over een waarschuwing en wordt ook opgelost.

De algemene procedure is:

-   De logische app voor het betreffende Waarschuwingstype maken.

-   Het schema voor het betreffende Waarschuwingstype in de logische app importeren.

-   Definieer het gedrag van de app logica.

-   Kopieer het HTTP-eindpunt van de logische app in een groep voor Azure in te grijpen.

Het proces is vergelijkbaar zijn als u wilt dat de logische app in een andere actie uit te voeren.

## <a name="create-an-activity-log-alert-administrative"></a>Maken van een activiteit logboek-waarschuwing: beheerdersrechten

1.  Selecteer in de Azure-portal **maken van een resource** in de linkerbovenhoek.

2.  Zoek en selecteer **logische App**, selecteer daarna **maken**.

3.  Uw logische app geven een **naam**, kies een **resourcegroep**, enzovoort.

    ![Een logische app maken](media/monitoring-action-groups/create-logic-app-dialog.png "een logische app maken")

4.  Selecteer **maken** voor het maken van de logische app. Een pop-upbericht geeft aan dat de logische app is gemaakt. Selecteer **Resource starten** openen de **Logic Apps Designer**.

5.  Selecteer de trigger: **wanneer een HTTP-aanvraag wordt ontvangen**.

    ![App-triggers voor Logic](media/monitoring-action-groups/logic-app-triggers.png "Logic app-triggers")

6.  Selecteer **bewerken** wijzigen van de aanvraag HTTP-trigger.

    ![HTTP-aanvraag triggers](media/monitoring-action-groups/http-request-trigger-shape.png "HTTP-aanvraag triggers")

7.  Selecteer **Voorbeeldnettolading om een schema te genereren**.

    ![Gebruik een voorbeeld-nettolading](media/monitoring-action-groups/use-sample-payload-button.png "gebruikt u een voorbeeld-nettolading")

8.  Kopieer en plak het volgende voorbeeldschema in het dialoogvenster:

    ```json
        {
            "schemaId": "Microsoft.Insights/activityLogs",
            "data": {
                "status": "Activated",
                "context": {
                "activityLog": {
                    "authorization": {
                    "action": "microsoft.insights/activityLogAlerts/write",
                    "scope": "/subscriptions/…"
                    },
                    "channels": "Operation",
                    "claims": "…",
                    "caller": "logicappdemo@contoso.com",
                    "correlationId": "91ad2bac-1afa-4932-a2ce-2f8efd6765a3",
                    "description": "",
                    "eventSource": "Administrative",
                    "eventTimestamp": "2018-04-03T22:33:11.762469+00:00",
                    "eventDataId": "ec74c4a2-d7ae-48c3-a4d0-2684a1611ca0",
                    "level": "Informational",
                    "operationName": "microsoft.insights/activityLogAlerts/write",
                    "operationId": "61f59fc8-1442-4c74-9f5f-937392a9723c",
                    "resourceId": "/subscriptions/…",
                    "resourceGroupName": "LOGICAPP-DEMO",
                    "resourceProviderName": "microsoft.insights",
                    "status": "Succeeded",
                    "subStatus": "",
                    "subscriptionId": "…",
                    "submissionTimestamp": "2018-04-03T22:33:36.1068742+00:00",
                    "resourceType": "microsoft.insights/activityLogAlerts"
                }
                },
                "properties": {}
            }
        }
    ```

9. De **Logic App-ontwerper** geeft een pop-upvenster om eraan te herinneren dat de aanvraag verzonden naar de logische app ingesteld moet de **Content-Type** koptekst tot **application/json**. Sluit het pop-upvenster. De Azure-monitorwaarschuwing stelt de header.

    ![Instellen van de header Content-Type](media/monitoring-action-groups/content-type-header.png "de header Content-Type instellen")

10. Selecteer **+** **nieuwe stap** en kies vervolgens **een actie toevoegen**.

    ![Een actie toevoegen](media/monitoring-action-groups/add-action.png "een actie toevoegen")

11. Zoek en selecteer de connector voor Microsoft-Teams. Kies de **Microsoft-Teams Post bericht** in te grijpen.

    ![Microsoft-Teams acties](media/monitoring-action-groups/microsoft-teams-actions.png "acties van de Microsoft-Teams")

12. Configureer de actie van de Microsoft-Teams. De **Logic Apps Designer** vraagt u om uw Office 365-account te verifiëren. Kies de **Team-Id** en **kanaal-Id** aan het bericht wordt verzonden.

13. Het bericht configureren met behulp van een combinatie van statische tekst en verwijzingen naar de \<velden\> in de dynamische inhoud. Kopieer en plak de volgende tekst in de **bericht** veld:

    ```text
      Activity Log Alert: <eventSource>
      operationName: <operationName>
      status: <status>
      resourceId: <resourceId>
    ```

    Zoek vervolgens naar en vervang de \<velden\> met dynamische inhoud labels met dezelfde naam.

    > [!NOTE]
    > Er zijn twee dynamische velden die worden genoemd **status**. Toevoegen van deze velden aan het bericht. Gebruik het veld dat in de **activityLog** eigenschappenverzameling en verwijder de ander veld. Houd de cursor boven de **status** veld voor een overzicht van de volledig gekwalificeerde veldverwijzing, zoals wordt weergegeven in de volgende schermafbeelding:

    ![Microsoft-Teams actie: een bericht](media/monitoring-action-groups/teams-action-post-message.png "Microsoft-Teams actie: een bericht")

14. Aan de bovenkant van de **Logic Apps Designer**, selecteer **opslaan** opslaan van uw logische app.

15. Open uw bestaande actiegroep en een actie om te verwijzen naar de logische app toevoegen. Als u een bestaande actiegroep niet hebt, raadpleegt u [maken en beheren van actiegroepen in de Azure portal](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-action-groups) een maken. Vergeet niet uw wijzigingen op te slaan.

    ![De bewerking groep bijwerken](media/monitoring-action-groups/update-action-group.png "de bewerking groep bijwerken")

Uw logische app in de volgende keer dat een waarschuwing wordt aangeroepen voor uw actiegroep wordt genoemd.

## <a name="create-a-service-health-alert"></a>Een melding van de health service maken

Azure servicestatus vermeldingen maken deel uit van het activiteitenlogboek. Het proces voor het maken van de waarschuwing is vergelijkbaar met [maken van een activiteit logboek waarschuwing](#create-an-activity-log-alert-administrative), maar met enkele wijzigingen:

- Stap 1 tot en met 7 zijn hetzelfde.
- Gebruik het volgende voorbeeldschema voor de aanvraag HTTP-trigger voor stap 8:

    ```json
    {
        "schemaId": "Microsoft.Insights/activityLogs",
        "data": {
            "status": "Activated",
            "context": {
                "activityLog": {
                    "channels": "Admin",
                    "correlationId": "e416ed3c-8874-4ec8-bc6b-54e3c92a24d4",
                    "description": "…",
                    "eventSource": "ServiceHealth",
                    "eventTimestamp": "2018-04-03T22:44:43.7467716+00:00",
                    "eventDataId": "9ce152f5-d435-ee31-2dce-104228486a6d",
                    "level": "Informational",
                    "operationName": "Microsoft.ServiceHealth/incident/action",
                    "operationId": "e416ed3c-8874-4ec8-bc6b-54e3c92a24d4",
                    "properties": {
                        "title": "…",
                        "service": "…",
                        "region": "Global",
                        "communication": "…",
                        "incidentType": "Incident",
                        "trackingId": "…",
                        "impactStartTime": "2018-03-22T21:40:00.0000000Z",
                        "impactMitigationTime": "2018-03-22T21:41:00.0000000Z",
                        "impactedServices": "[{"ImpactedRegions"}]",
                        "defaultLanguageTitle": "…",
                        "defaultLanguageContent": "…",
                        "stage": "Active",
                        "communicationId": "11000001466525",
                        "version": "0.1.1"
                    },
                    "status": "Active",
                    "subscriptionId": "…",
                    "submissionTimestamp": "2018-04-03T22:44:50.8013523+00:00"
                }
            },
            "properties": {}
        }
    }
    ```

-  Stap 9 en 10 zijn hetzelfde.
-  Gebruik het volgende proces voor de stappen 11 tot en met 14:

   1. Selecteer **+** **nieuwe stap** en kies vervolgens **een voorwaarde toevoegen**. De volgende voorwaarden om ervoor te zorgen dat de logische app wordt uitgevoerd alleen wanneer de invoergegevens overeenkomt met deze waarden instellen:
       - `schemaId == Microsoft.Insights/activityLogs`
       - `eventSource == ServiceHealth`
       - `version == 0.1.1`

      !['Servicestatus nettolading voorwaarde'](media/monitoring-action-groups/service-health-payload-condition.png "servicestatus nettolading voorwaarde")

   1. In de **als de waarde true** voorwaarde en volg de aanwijzingen in stappen 11 tot en met 13 in [maken van een activiteit logboek waarschuwing](#create-an-activity-log-alert-administrative) om toe te voegen van de Microsoft-Teams in te grijpen.

   1. Het bericht definiëren met behulp van een combinatie van HTML- en dynamische inhoud. Kopieer en plak de volgende inhoud in de **bericht** veld. Vervang de `[incidentType]`, `[trackingID]`, `[title]`, en `[communication]` velden met dynamische inhoud labels met dezelfde naam:

       ```html
       <p>
       <b>Alert Type:</b>&nbsp;<strong>[incidentType]</strong>&nbsp;
       <strong>Tracking ID:</strong>&nbsp;[trackingId]&nbsp;
       <strong>Title:</strong>&nbsp;[title]</p>
       <p>
       <a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details, log in to the Azure Service Health dashboard.</a>
       </p>
       <p>[communication]</p>
       ```

       !['Actie servicestatus voorwaarde na'](media/monitoring-action-groups/service-health-true-condition-post-action.png "servicestatus voorwaarde post actie")

   1. Voor de **indien false** voorwaarde, geeft u een nuttig bericht:

       ```html
       <p><strong>Service Health Alert</strong></p>
       <p><b>Unrecognized alert schema</b></p>
       <p><a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details, log in to the Azure Service Health dashboard.\</a></p>
       ```

       !['Actie servicestatus false voorwaarde na'](media/monitoring-action-groups/service-health-false-condition-post-action.png "servicestatus false voorwaarde post actie")

- Stap 15 is hetzelfde. Volg de instructies voor het opslaan van uw logische app en uw actiegroep bijwerken.

## <a name="create-a-metric-alert"></a>Waarschuwing voor een metriek maken

Het proces voor het maken van een waarschuwing voor metrische is vergelijkbaar met [maken van een activiteit logboek waarschuwing](#create-an-activity-log-alert-administrative), maar met enkele wijzigingen:

- Stap 1 tot en met 7 zijn hetzelfde.
- Gebruik het volgende voorbeeldschema voor de aanvraag HTTP-trigger voor stap 8:

    ```json
    {
    "schemaId": "AzureMonitorMetricAlert",
    "data": {
        "version": "2.0",
        "status": "Activated",
        "context": {
        "timestamp": "2018-04-09T19:00:07.7461615Z",
        "id": "…",
        "name": "TEST-VM CPU Utilization",
        "description": "",
        "conditionType": "SingleResourceMultipleMetricCriteria",
        "condition": {
            "windowSize": "PT15M",
            "allOf": [
                {
                    "metricName": "Percentage CPU",
                    "dimensions": [
                    {
                        "name": "ResourceId",
                        "value": "d92fc5cb-06cf-4309-8c9a-538eea6a17a6"
                    }
                ],
                "operator": "GreaterThan",
                "threshold": "5",
                "timeAggregation": "PT15M",
                "metricValue": 1.0
            }
            ]
        },
        "subscriptionId": "…",
        "resourceGroupName": "TEST",
        "resourceName": "test-vm",
        "resourceType": "Microsoft.Compute/virtualMachines",
        "resourceId": "…",
        "portalLink": "…"
        },
        "properties": {}
    }
    }
    ```

- Stap 9 en 10 zijn hetzelfde.
- Gebruik het volgende proces voor de stappen 11 tot en met 14:

   1. Selecteer **+** **nieuwe stap** en kies vervolgens **een voorwaarde toevoegen**. De volgende voorwaarden om ervoor te zorgen dat de logische app wordt uitgevoerd alleen wanneer de invoergegevens overeenkomt met deze waarden instellen:
       - `schemaId == AzureMonitorMetricAlert`
       - `version == 2.0`

       !['Metrische waarschuwing nettolading voorwaarde'](media/monitoring-action-groups/metric-alert-payload-condition.png "metrische waarschuwing nettolading voorwaarde")

   1. In de **als de waarde true** voorwaarde, het toevoegen van een **voor elk** lus en de actie van de Microsoft-Teams. Het bericht definiëren met behulp van een combinatie van HTML- en dynamische inhoud.

       !["Action metrische waarschuwing voorwaarde post"](media/monitoring-action-groups/metric-alert-true-condition-post-action.png "metrische waarschuwing voorwaarde post actie")

   1. In de **indien false** voorwaarde, het definiëren van een Microsoft-Teams actie om te communiceren dat de waarschuwing metrische komt niet overeen met de verwachtingen van de logische app. De JSON-nettolading bevatten. U ziet hoe om te verwijzen naar de `triggerBody` dynamische inhoud in de `json()` expressie.

       !["Action metrische false meldingsvoorwaarde post"](media/monitoring-action-groups/metric-alert-false-condition-post-action.png "metrische false meldingsvoorwaarde post actie")

- Stap 15 is hetzelfde. Volg de instructies voor het opslaan van uw logische app en uw actiegroep bijwerken.

## <a name="next-steps"></a>Volgende stappen
* Ophalen van een [overzicht van Azure activiteit logboek waarschuwingen](monitoring-overview-alerts.md) en leer hoe u waarschuwingen ontvangt.  
* Meer informatie over hoe [waarschuwingen configureren wanneer een melding van de status van Azure-Service wordt teruggepost](monitoring-activity-log-alerts-on-service-notifications.md).
* Meer informatie over [actiegroepen](monitoring-action-groups.md).