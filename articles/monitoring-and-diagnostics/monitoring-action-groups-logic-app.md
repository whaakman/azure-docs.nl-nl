---
title: Het activeren van complexe bewerkingen met Azure-waarschuwingen en acties groepen
description: Informatie over het maken van een logische App actie verwerken waarschuwingen van Azure controleren.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: eafb2bcf0175190748c9dd020051cbebfcaee1fd
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263881"
---
# <a name="create-a-logic-app-action"></a>De actie van een logische App maken
## <a name="overview"></a>Overzicht ##
Wanneer een waarschuwing Azure veroorzaakt, ontvangt een [actiegroep](monitoring-action-groups.md). Actiegroepen kunnen u voor het activeren van een of meer acties voor het waarschuwen van de waarschuwing en zelfs opgelost.

In dit artikel laat zien hoe instellen en het activeren van een logische App voor het maken van een conversatie in Microsoft-Teams wanneer een waarschuwing wordt geactiveerd.

De algemene procedure is:

-   De logische App voor het betreffende Waarschuwingstype maken

-   Het schema voor het betreffende Waarschuwingstype in de logische App importeren

-   Definieer het gedrag van de logische App

-   HTTP-eindpunt van de logische App kopiëren naar de groep van een Azure-acties

Het proces is vergelijkbaar zijn als u wilt dat de logische App in een andere actie uit te voeren.

## <a name="create-an-activity-log-alert--administrative"></a>Een waarschuwing van het logboek activiteit – Administrative maken

1.  Klik op de knop **Een resource maken** in de linkerbovenhoek van Azure Portal.

2.  Zoek en selecteer **Logische app**. Klik op de knop **Maken**.

3.  Geef een naam op voor uw logische App, kiest u een resourcegroep, enzovoort.

    ![Maak logic app dialoogvenster](media/monitoring-action-groups/create-logic-app-dialog.png "maken logic app dialoogvenster")

4.  Klik op de knop maken voor het maken van de logische App. Een pop-up wordt weergegeven wanneer de logische App is gemaakt. Klik op de knop starten Resource openen van de ontwerpfunctie Logic Apps.

5.  Selecteer de trigger **wanneer een HTTP-aanvraag wordt ontvangen**.

    ![App-Triggers voor Logic](media/monitoring-action-groups/logic-app-triggers.png "Logic App-Triggers")

6.  Selecteer **bewerken** wijzigen van de trigger van HTTP-aanvraag

    ![HTTP-aanvraag trigger vorm](media/monitoring-action-groups/http-request-trigger-shape.png "HTTP-aanvraag trigger vorm")

7.  Selecteer **Voorbeeldnettolading om een schema te genereren**.

    ![Gebruik voorbeeld nettolading knop](media/monitoring-action-groups/use-sample-payload-button.png "gebruik voorbeeld nettolading knop")

8.  Kopieer en plak het volgende voorbeeldschema in het dialoogvenster.

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

9. De Logic App-ontwerper kan een notitie herinneren dat verzoek verzonden naar de logische App moet de header Content-Type is ingesteld op application/json wordt weergegeven. Doorgaan en het dialoogvenster te sluiten. De Azure-monitorwaarschuwing wordt dit correct doen.

    ![Content-Type-header](media/monitoring-action-groups/content-type-header.png "header Content-Type")

10. Selecteer **+ een nieuwe stap** en kies vervolgens **een actie toevoegen**.

    ![Actie toevoegen](media/monitoring-action-groups/add-action.png "actie toevoegen")

11. Zoek en selecteer de connector voor Microsoft-Teams. Kies de **Microsoft-Teams Post bericht** in te grijpen.

    ![Microsoft-Teams acties](media/monitoring-action-groups/microsoft-teams-actions.png "acties van de Microsoft-Teams")

12. Configureer de actie van de Microsoft-Teams. De ontwerpfunctie van Logic Apps wordt u gevraagd om uw account Office365 te verifiëren. Kies de **Team-Id** en **kanaal-Id** aan het bericht wordt verzonden.

13. Configureer de **bericht** met een combinatie van statische tekst en verwijzingen naar de \<velden\> in de dynamische inhoud. Knippen en plakken van de volgende tekst naar het veld bericht.

    ```text
      Activity Log Alert: <eventSource>
      operationName: <operationName>
      status: <status>
      resourceId: <resourceId>
    ```

    Zoek vervolgens naar en vervang de \<velden\> met dynamische inhoud labels met dezelfde naam.

    **[OPMERKING]**  Er zijn twee dynamische velden met de naam **status**. Beide statusvelden toevoegen aan het bericht. Gebruik een in de eigenschappenverzameling activityLog en verwijder de andere. Als u de muisaanwijzer boven de **status** veld ziet u de volledig gekwalificeerde veldverwijzing zoals weergegeven in de schermafbeelding

    ![Actie - Post bericht teams](media/monitoring-action-groups/teams-action-post-message.png "Teams actie - bericht posten")

14. Uw logische App opslaan door te klikken op de knop opslaan aan de bovenkant van de ontwerpfunctie

15. Klik op de vorm HTTP-aanvraag, wordt deze uit te breiden. Kopieer de HTTP POST-URL.

    ![HTTP POST-URL](media/monitoring-action-groups/http-post-url.png "HTTP POST-URL")

16. Open uw bestaande groep voor de actie en een actie om te verwijzen naar de logische App toevoegen. Als u een bestaande actiegroep niet hebt, raadpleegt u <https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-action-groups> een maken. Vergeet niet uw wijzigingen op te slaan.

    ![Actie-updategroep](media/monitoring-action-groups/update-action-group.png "updategroep actie")

Uw logische App in de volgende keer dat een waarschuwing wordt aangeroepen voor uw groep actie wordt genoemd.

## <a name="create-a-service-health-alert"></a>Een waarschuwing servicestatus maken

Vermeldingen van de health service uitmaken deel van het activiteitenlogboek, zodat het proces vergelijkbaar met de volgende wijzigingen is

1.  Stap 1 tot en met 7 zijn hetzelfde.
2.  Gebruik het volgende voorbeeldschema voor de HTTP-Trigger in stap 8.

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

3.  Stap 9 10 zijn hetzelfde.
4.  Gebruik de onderstaande procedure vanaf stap 11 op.
5.  Klik op de **+ een nieuwe stap** knop en kies **een voorwaarde toevoegen**. De volgende voorwaarden om ervoor te zorgen dat de logische App alleen wordt uitgevoerd wanneer de invoergegevens overeenkomt met deze waarden instellen
    - schemaId == Microsoft.Insights/activityLogs
    - eventSource == ServiceHealth
    - versie == 0.1.1

    !['Service health nettolading voorwaarde'](media/monitoring-action-groups/service-health-payload-condition.png "Service health nettolading voorwaarde")

6. In de **als de waarde true** voorwaarde, het toevoegen van de Microsoft-Teams actie via stappen 11 – 13 uit het vorige voorbeeld.

7. Definieer het bericht met een combinatie van HTML- en [dynamische inhoud]. Kopieer en plak de inhoud onder in het berichtenveld. Vervang [incidentType], [trackingID], [titel] en [communicatie] velden met dynamische inhoud labels met dezelfde naam

    ```html
    <p>
    <b>Alert Type:</b>&nbsp;<strong>[incidentType]</strong>&nbsp;
    <strong>Tracking ID:</strong>&nbsp;[trackingId]&nbsp;
    <strong>Title:</strong>&nbsp;[title]</p>
    <p>
    <a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details log into the Azure Service Health dashboard</a>
    </p>
    <p>[communication]</p>
    ```

    !["Service health voorwaarde post actie"](media/monitoring-action-groups/service-health-true-condition-post-action.png "Service health voorwaarde post actie")

8. Voor de **indien false** voorwaarde een nuttig bericht opgeven

    ```html
    <p><strong>Service Health Alert</strong></p>
    <p><b>Unrecognized alert schema</b></p>
    <p><a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details log into the Azure Service Health dashboard\</a></p>
    ```

    !['Actie servicestatus false voorwaarde na'](media/monitoring-action-groups/service-health-false-condition-post-action.png "servicestatus false voorwaarde post actie")

9.  Voer de stappen 15 – 16 van het vorige voorbeeld opslaan van uw logische App en uw groep actie bijwerken

## <a name="metric-alert"></a>Metrische waarschuwing

1.  Stap 1 tot en met 7 zijn hetzelfde als het eerste voorbeeld
2.  Gebruik het volgende voorbeeldschema voor de HTTP-Trigger in stap 8.

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

3.  Stap 9 10 zijn hetzelfde.
4.  Gebruik de onderstaande procedure vanaf stap 11 op.
5.  Klik op de **+ een nieuwe stap** knop en kies **een voorwaarde toevoegen**. De volgende voorwaarden om ervoor te zorgen dat de logische App alleen wordt uitgevoerd wanneer de invoergegevens overeenkomt met deze waarden instellen

    - schemaId == AzureMonitorMetricAlert
    - versie 2.0 ==

    !['Metrische waarschuwing nettolading voorwaarde'](media/monitoring-action-groups/metric-alert-payload-condition.png "metrische waarschuwing nettolading voorwaarde")

6.  In de **als de waarde true** voorwaarde voegen we toe een **voor elk** vorm en de actie van Microsoft-Teams en definiëren van het bericht met een combinatie van HTML- en [dynamische inhoud]

    !["Action metrische waarschuwing voorwaarde post"](media/monitoring-action-groups/metric-alert-true-condition-post-action.png "metrische waarschuwing voorwaarde post actie")

7.  In de **indien false** vorm, het definiëren van een actie van Microsoft-Teams dat communiceert dat metriek waarschuwing komt niet overeen met de logische App verwachtingen en de JSON-nettolading omvatten. Houd er rekening mee hoe we verwijzen naar de triggerBody dynamische inhoud in de expressie json().

    !["Action metrische false meldingsvoorwaarde post"](media/monitoring-action-groups/metric-alert-false-condition-post-action.png "metrische false meldingsvoorwaarde post actie")

8.  Voer de stappen 15 – 16 van het eerste voorbeeld opslaan van uw logische App en uw groep actie bijwerken

## <a name="next-steps"></a>Volgende stappen ##
* Ophalen van een [overzicht van de activiteit logboek waarschuwingen](monitoring-overview-alerts.md), en meer informatie over het om waarschuwingen te ontvangen.  
* Meer informatie over hoe [waarschuwingen configureren wanneer een melding van de health service wordt geboekt](monitoring-activity-log-alerts-on-service-notifications.md).
* Meer informatie over [actiegroepen](monitoring-action-groups.md)