---
title: Hoe kunt u complexe acties met Azure Monitor-waarschuwingen activeren
description: Informatie over het maken van een actie voor logische app voor het verwerken van Azure Monitor-waarschuwingen.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: e69158a6ee4d8415f52cf458c028cab56f481d8b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58121125"
---
# <a name="how-to-trigger-complex-actions-with-azure-monitor-alerts"></a>Hoe kunt u complexe acties met Azure Monitor-waarschuwingen activeren

Dit artikel leest u hoe u instelt en activeren van een logische app voor een discussie in Microsoft Teams maken wanneer een waarschuwing wordt geactiveerd.

## <a name="overview"></a>Overzicht
Wanneer een Azure Monitor-waarschuwing wordt geactiveerd, wordt een [actiegroep](../../azure-monitor/platform/action-groups.md). Actiegroepen kunnen u voor het activeren van een of meer acties om anderen te informeren over een waarschuwing en ook opgelost.

Het algemene proces is:

-   Maak de logische app voor het betreffende Waarschuwingstype.

-   Importeer het schema voor het desbetreffende Waarschuwingstype in de logische app.

-   Definieer het gedrag van logische app.

-   Kopieer de HTTP-eindpunt van de logische app in een Azure actiegroep.

Het proces is vergelijkbaar als u wilt dat de logische app een andere actie uit te voeren.

## <a name="create-an-activity-log-alert-administrative"></a>Een waarschuwing voor activiteitenlogboek maken: Administratief

1.  Selecteer in de Azure portal, **een resource maken** in de linkerbovenhoek.

2.  Zoek en selecteer **logische App**en selecteer vervolgens **maken**.

3.  Geef uw logische app een **naam**, kiest u een **resourcegroep**, enzovoort.

    ![Maak een logische app](media/action-groups-logic-app/create-logic-app-dialog.png "een logische app maken")

4.  Selecteer **maken** te maken van de logische app. Een pop-upbericht geeft aan dat de logische app is gemaakt. Selecteer **start Resource** openen de **ontwerper van logische Apps**.

5.  Selecteer de trigger: **Wanneer een HTTP-aanvraag wordt ontvangen**.

    ![Triggers voor logische app](media/action-groups-logic-app/logic-app-triggers.png "triggers voor logische app")

6.  Selecteer **bewerken** te wijzigen van de HTTP-aanvraag als trigger.

    ![HTTP-aanvraag triggers](media/action-groups-logic-app/http-request-trigger-shape.png "triggers voor HTTP-aanvragen")

7.  Selecteer **Voorbeeldnettolading om een schema te genereren**.

    ![Een voorbeeld van payload gebruiken](media/action-groups-logic-app/use-sample-payload-button.png "een voorbeeldnettolading gebruiken")

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

9. De **Logic App Designer** wordt een pop-upvenster u eraan te herinneren dat de verzonden aanvraag naar de logische app moet instellen de **Content-Type** koptekst **application/json**. Sluit het pop-upvenster. De waarschuwing Azure Monitor stelt de header.

    ![De header Content-Type instellen](media/action-groups-logic-app/content-type-header.png "de header Content-Type instellen")

10. Selecteer **+** **nieuwe stap** en kies vervolgens **een actie toevoegen**.

    ![Een actie toevoegen](media/action-groups-logic-app/add-action.png "een actie toevoegen")

11. Zoek en selecteer de Microsoft Teams-connector. Kies de **Microsoft Teams-bericht posten** actie.

    ![Microsoft Teams-acties](media/action-groups-logic-app/microsoft-teams-actions.png "Microsoft Teams-acties")

12. Configureer de actie Microsoft Teams. De **ontwerper van logische Apps** vraagt u om uw Office 365-account te verifiëren. Kies de **Team-ID** en **kanaal-ID** het bericht moet worden verzonden.

13. Het bericht configureren met behulp van een combinatie van statische tekst en verwijzingen naar de \<velden\> in de dynamische inhoud. Kopieer en plak de volgende tekst in de **bericht** veld:

    ```text
      Activity Log Alert: <eventSource>
      operationName: <operationName>
      status: <status>
      resourceId: <resourceId>
    ```

    Zoek en vervang de \<velden\> met dynamische inhoud tags met dezelfde naam.

    > [!NOTE]
    > Er zijn twee dynamische velden die zijn benoemde **status**. Voeg beide van deze velden toe aan het bericht. Gebruik het veld dat in de **activityLog** eigenschappenverzameling en verwijder de andere velden. Beweeg de muisaanwijzer de cursor over de **status** veld voor een overzicht van de volledig gekwalificeerde veldverwijzing, zoals wordt weergegeven in de volgende schermafbeelding:

    ![Microsoft Teams-actie: Een bericht](media/action-groups-logic-app/teams-action-post-message.png "Microsoft Teams-actie: Een bericht plaatsen")

14. Aan de bovenkant van de **ontwerper van logische Apps**, selecteer **opslaan** om op te slaan, uw logische app.

15. Open uw bestaande actiegroep en een actie toevoegen om te verwijzen naar de logische app. Als u een bestaande actiegroep hebt, raadpleegt u [maken en beheren van actiegroepen in Azure portal](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) een te maken. Vergeet niet uw wijzigingen op te slaan.

    ![Bijwerken van de actiegroep](media/action-groups-logic-app/update-action-group.png "bijwerken van de actiegroep")

De volgende keer dat een waarschuwing de actiegroep roept wordt uw logische app genoemd.

## <a name="create-a-service-health-alert"></a>Een statuswaarschuwing voor de service maken

Azure Service Health-vermeldingen maken deel uit van het activiteitenlogboek. Het proces voor het maken van de waarschuwing is vergelijkbaar met [het maken van een waarschuwing voor activiteitenlogboek](#create-an-activity-log-alert-administrative), maar met een aantal wijzigingen:

- Stap 1 tot en met 7 zijn hetzelfde.
- Gebruik het volgende voorbeeldschema voor de HTTP-aanvraag als trigger voor stap 8:

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
-  Voor de stappen 11 tot en met 14, gebruikt u het volgende proces:

   1. Selecteer **+** **nieuwe stap** en kies vervolgens **een voorwaarde toevoegen**. De volgende voorwaarden instellen, zodat de logische app wordt uitgevoerd alleen wanneer de invoergegevens komt overeen met de onderstaande waarden.  Wanneer u de waarde voor de versie in het tekstvak invoert, plaatst u aanhalingstekens rond het ('0.1.1') om ervoor te zorgen dat deze wordt geëvalueerd als een tekenreeks en niet een numeriek type.  Het systeem wordt niet weergegeven voor de aanhalingstekens als u terug naar de pagina, maar de onderliggende code nog steeds het tekenreekstype.   
       - `schemaId == Microsoft.Insights/activityLogs`
       - `eventSource == ServiceHealth`
       - `version == "0.1.1"`

      !['Service Health nettolading voorwaarde'](media/action-groups-logic-app/service-health-payload-condition.png "servicestatus nettolading voorwaarde")

   1. In de **als de waarde true** voorwaarde, volg de instructies in de stappen 11 tot en met 13 in [maken van een waarschuwing voor activiteitenlogboek](#create-an-activity-log-alert-administrative) om toe te voegen van de Microsoft Teams-actie.

   1. Het bericht definiëren met behulp van een combinatie van HTML- en dynamische inhoud. Kopieer en plak de volgende inhoud in de **bericht** veld. Vervang de `[incidentType]`, `[trackingID]`, `[title]`, en `[communication]` velden met dynamische inhoud tags met dezelfde naam:

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

       !["Service Health voorwaarde is voldaan post action"](media/action-groups-logic-app/service-health-true-condition-post-action.png "actie na de servicestatus voorwaarde is voldaan")

   1. Voor de **indien onwaar** voorwaarde, geeft u een nuttig bericht:

       ```html
       <p><strong>Service Health Alert</strong></p>
       <p><b>Unrecognized alert schema</b></p>
       <p><a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details, log in to the Azure Service Health dashboard.\</a></p>
       ```

       !["Service Health voorwaarde false post action"](media/action-groups-logic-app/service-health-false-condition-post-action.png "actie na de servicestatus voorwaarde false")

- Stap 15 is hetzelfde. Volg de instructies voor het opslaan van uw logische app en bijwerken van de actiegroep.

## <a name="create-a-metric-alert"></a>Een metrische waarschuwing maken

Het proces voor het maken van een waarschuwing voor metrische gegevens is vergelijkbaar met [het maken van een waarschuwing voor activiteitenlogboek](#create-an-activity-log-alert-administrative), maar met een aantal wijzigingen:

- Stap 1 tot en met 7 zijn hetzelfde.
- Gebruik het volgende voorbeeldschema voor de HTTP-aanvraag als trigger voor stap 8:

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
- Voor de stappen 11 tot en met 14, gebruikt u het volgende proces:

  1. Selecteer **+** **nieuwe stap** en kies vervolgens **een voorwaarde toevoegen**. De volgende voorwaarden instellen, zodat de logische app wordt uitgevoerd alleen wanneer de invoergegevens overeenkomt met deze onderstaande waarden. Wanneer de waarde voor de versie in te voeren in het tekstvak, aanhalingstekens rond het ("2.0") plaatsen op ervoor zorgt dat deze wordt geëvalueerd als een tekenreeks en niet een numeriek type.  Het systeem wordt niet weergegeven voor de aanhalingstekens als u terug naar de pagina, maar de onderliggende code nog steeds het tekenreekstype. 
     - `schemaId == AzureMonitorMetricAlert`
     - `version == "2.0"`
       
       !['Metrische alert payload voorwaarde'](media/action-groups-logic-app/metric-alert-payload-condition.png "metrische alert payload voorwaarde")

  1. In de **als de waarde true** voorwaarde, voegt u een **voor elk** lus en de Microsoft Teams-actie. Het bericht definiëren met behulp van een combinatie van HTML- en dynamische inhoud.

      !['Actie metrische waarschuwing voorwaarde na'](media/action-groups-logic-app/metric-alert-true-condition-post-action.png "actie na de metrische waarschuwing voorwaarde is voldaan")

  1. In de **indien onwaar** voorwaarde, het definiëren van een Microsoft Teams-actie om te communiceren dat de waarschuwing voor metrische gegevens, komt niet overeen met de verwachtingen van de logische app. De JSON-nettolading opnemen. U ziet hoe om te verwijzen naar de `triggerBody` dynamische inhoud in de `json()` expressie.

      !['Actie metrische waarschuwing false na'](media/action-groups-logic-app/metric-alert-false-condition-post-action.png "actie na de waarschuwing false metrische gegevens")

- Stap 15 is hetzelfde. Volg de instructies voor het opslaan van uw logische app en bijwerken van de actiegroep.

## <a name="calling-other-applications-besides-microsoft-teams"></a>Aanroepen van andere toepassingen dan Microsoft Teams
Logic Apps heeft een aantal andere connectors waarmee u kunt acties van de trigger in een breed scala van toepassingen en databases. Slack, SQL Server, Oracle, Salesforce, zijn enkele voorbeelden. Zie voor meer informatie over connectors [Logic App-connectors](../../connectors/apis-list.md).  

## <a name="next-steps"></a>Volgende stappen
* Krijgen een [overzicht van waarschuwingen voor activiteitenlogboeken van Azure](../../azure-monitor/platform/alerts-overview.md) en leer hoe u waarschuwingen ontvangt.  
* Meer informatie over het [waarschuwingen configureren wanneer er een melding van Azure Service Health is geplaatst](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).
* Meer informatie over [actiegroepen](../../azure-monitor/platform/action-groups.md).

