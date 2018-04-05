---
title: Uitbreiden (kopiëren) waarschuwingen uit de OMS-portal in Azure | Microsoft Docs
description: Hulpprogramma's en API waarmee waarschuwingen van OMS uit te breiden naar de Azure-waarschuwingen kan worden gedaan door klanten vrijwillig.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: vinagara
ms.openlocfilehash: c2e11d89f35915ef0a0c1e1f544b0be8df0473de
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="how-to-extend-copy-alerts-from-oms-into-azure"></a>Het uitbreiden van waarschuwingen (kopiëren) van OMS in Azure
Vanaf **23 April 2018**, alle klanten die gebruikmaken van waarschuwingen die zijn geconfigureerd in [Microsoft Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), zal worden uitgebreid in Azure. Waarschuwingen die zijn uitgebreid naar Azure gedragen zich hetzelfde als in OMS. Mogelijkheden voor bewaking, blijven behouden. Waarschuwingen die zijn gemaakt in OMS naar Azure uitbreiden biedt veel voordelen. Zie voor meer informatie over de voordelen en het proces voor het verlengen van waarschuwingen van OMS naar Azure [waarschuwingen van OMS uitbreiden naar Azure](monitoring-alerts-extend.md).

Klanten willen verplaatsen hun waarschuwingen direct van OMS naar Azure kunt doen met behulp van een van de opties die zijn vermeld.

## <a name="option-1---using-oms-portal"></a>Optie 1 - met OMS-portal
Volg de onderstaande stappen voor het initiëren van vrijwillig de uit te breiden van waarschuwingen van OMS-portal naar Azure.

1. Ga naar instellingen en klik vervolgens in de sectie waarschuwingen in de overzichtspagina met OMS portal. Klik op de knop met het label 'Uitbreiden naar Azure', gemarkeerd in de onderstaande afbeelding.

    ![OMS waarschuwingsinstellingen portalpagina met de optie uitbreiden](./media/monitor-alerts-extend/ExtendInto.png)

2. Zodra de knop wordt geklikt wordt een 3-stap-wizard weergegeven, met de eerste stap biedt details van het proces. Klik op volgende om door te gaan.

    ![Waarschuwingen van OMS-portal in de Azure - stap 1 uitbreiden](./media/monitor-alerts-extend/ExtendStep1.png)

3. In de tweede stap wordt het systeem een samenvatting van de voorgestelde wijziging weergeven door de juiste aanbieding [actiegroepen](monitoring-action-groups.md), voor de waarschuwingen in de OMS-portal. Als soortgelijke acties zijn waargenomen in meer dan één waarschuwing - stelt systeem om te koppelen aan deze een groep met één actie.  Actiegroep voorgestelde volgt u de naamconventie: *WorkspaceName_AG_ #Number*. Klik op volgende om door te gaan worden.
Een voorbeeldscherm hieronder.

    ![Waarschuwingen van OMS-portal in de Azure - stap 2 uitbreiden](./media/monitor-alerts-extend/ExtendStep2.png)


4. In de laatste stap van de wizard vraagt u de OMS-portal om te plannen alle waarschuwingen uit te breiden naar de Azure - door het nieuwe Actiegroepen maken en koppelen met waarschuwingen, zoals wordt weergegeven in het vorige scherm. Om door te gaan kiezen klikt u op Voltooien en Bevestig op de prompt voor het initiëren van het proces. Klanten kunnen eventueel ook bieden voor e-mailadressen waarop ze graag OMS-portal voor het verzenden van een rapport over de verwerking is voltooid.

    ![Waarschuwingen van OMS-portal in de Azure - stap 3 uitbreiden](./media/monitor-alerts-extend/ExtendStep3.png)

5. Zodra de wizard voltooid is, is weer naar de pagina instellingen voor waarschuwingen en de optie 'Uitbreiden naar Azure' wordt verwijderd. Op de achtergrond wordt gepland in OMS-portal waarschuwingen in logboekanalyse worden uitgebreid in Azure. Dit kan enige tijd duren en als de bewerking voor een korte periode waarschuwingen in de OMS-portal begint niet meer worden gewijzigd. Huidige status via banner wordt weergegeven en als e-waar adressen opgegeven tijdens stap 4, zal ze vervolgens worden geïnformeerd wanneer op de achtergrond met succes loopt door alle waarschuwingen in Azure. 

6. Waarschuwingen blijven worden vermeld in de OMS-portal, zelfs nadat ze ophalen is uitgebreid in Azure.

    ![Na het uitbreiden van waarschuwingen in de OMS-portal naar Azure](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2---using-api"></a>Optie 2 - API met behulp van
Voor klanten die willen via een programma bepalen of het proces van waarschuwingen in de OMS-portal uit te breiden naar Azure; automatiseren Microsoft heeft nieuwe AlertsVersion API onder logboekanalyse verstrekt.

De API Log Analytics AlertsVersion RESTful is en toegankelijk zijn via de REST-API van Azure Resource Manager. In dit document vindt u voorbeelden waarbij de API worden geopend vanuit een PowerShell-opdrachtregel met [ARMClient](https://github.com/projectkudu/ARMClient), een open source-opdrachtregelprogramma dat vereenvoudigt de Azure Resource Manager-API wordt aangeroepen. Het gebruik van ARMClient en PowerShell is een van de verschillende opties om toegang te krijgen tot de API. De API uitvoer resulteert in een JSON-indeling, gebruik van de resultaten in veel verschillende manieren programmatisch toestaan.

Via GET op de API kan worden verkregen in resultaat van de samenvatting van de voorgestelde wijziging als lijst met de juiste [actiegroepen](monitoring-action-groups.md) voor de waarschuwingen in de OMS-portal, in JSON indeling. Als soortgelijke acties zijn waargenomen in meer dan één waarschuwing - systeem stelt maken koppelen aan deze een groep met één actie.  Actiegroep voorgestelde volgt u de naamconventie: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

> [!NOTE]
> OPHALEN van de API-aanroep niet leiden tot waarschuwingen in de OMS-portal ophalen uitgebreid naar Azure. Het leveren alleen als reactie het overzicht van voorgestelde wijzigingen. Een POST-aanroep om te bevestigen deze wijzigingen worden gedaan om waarschuwingen in Azure uitbreiden, moet u de API.

Als het aanroepen van GET API geslaagd, samen met 200 OK antwoord is, zou een JSON-lijst met waarschuwingen samen met voorgestelde actiegroepen worden opgegeven. Voorbeeldreactie hieronder:

```json
{
    "version": 1,
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "http://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}

```
Voor het geval er geen waarschuwingen in de opgegeven werkruimte zijn, samen met 200 OK antwoord voor de GET-bewerking zou de JSON zijn:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Als alle waarschuwingen in de werkruimte opgegeven al zijn uitgebreid in Azure - zou de reactie op GET-aanroep zijn:
```json
{
    "version": 2
}
```

Start een POST opgegeven naar de API voor het initiëren van de planning van de waarschuwingen in de OMS-portal uitbreiden naar Azure. Tijdens het doorzoeken van deze opdracht call/bevestigt van de gebruiker bedoeling evenals acceptatie te hebben om hun waarschuwingen in de OMS-portal uitgebreid naar Azure de wijzigingen uitvoeren, zoals aangegeven in het antwoord van GET-aanroep naar de API. Gebruiker kan desgewenst een lijst met e-mailadressen aan welke OMS portal wordt een rapport te verzenden, wanneer de geplande achtergrondproces voor het uitbreiden van de waarschuwingen in de OMS-portal naar Azure met succes wordt voltooid.

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> Resultaat van het uitbreiden van OMS portal waarschuwingen in Azure, kan afwijken van de samenvatting geleverd door GET - op-account van elke wijziging in het systeem uitgevoerd. Als gepland, is waarschuwingen in de OMS-portal tijdelijk niet beschikbaar voor bewerken/wijziging - terwijl er nieuwe waarschuwingen kunnen worden gemaakt. 

Als het bericht geslaagd is, wordt er een 200 OK antwoord samen met geretourneerd:
```json
{
    "version": 2
}
```
Die aangeeft dat de waarschuwingen zijn uitgebreid in Azure, zoals aangegeven door versie 2. Deze versie wordt alleen gebruikt voor controle of waarschuwingen zijn uitgebreid in Azure en geen invloed in gebruik met hebben [Log Analytics-API van zoekservice](../log-analytics/log-analytics-api-alerts.md). Zodra de waarschuwingen worden uitgebreid naar Azure is, alle e-mailadressen die zijn opgegeven tijdens GET een rapport met details van de wijzigingen gedaan worden verzonden.


En ten slotte, als de waarschuwingen in de werkruimte opgegeven al zijn gepland om te worden uitgebreid naar Azure - het antwoord op POST 403-verboden.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de nieuwe [waarschuwingen van Azure-ervaring](monitoring-overview-unified-alerts.md).
* Meer informatie over [waarschuwingen melden in Azure waarschuwingen](monitor-alerts-unified-log.md).
