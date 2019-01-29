---
title: Waarschuwingen van Log Analytics uitbreiden naar Azure Government-Cloud
description: Dit artikel beschrijft de hulpprogramma's en API waarmee u waarschuwingen van Log Analytics naar Azure-waarschuwingen kunt uitbreiden.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 9d734f74c4e12b369e46c15dcb9d01a8185dddd6
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103374"
---
# <a name="extend-alerts-from-log-analytics-into-azure-alerts"></a>Waarschuwingen van Log Analytics uitbreiden naar Azure-waarschuwingen
De waarschuwingsfunctie in OMS-portal wordt vervangen door de Azure-waarschuwingen in Azure Government-cloud. Als onderdeel van deze overgang, wordt de waarschuwingen die u oorspronkelijk hebt geconfigureerd in Log Analytics worden uitgebreid naar Azure. Als u niet wachten tot deze worden automatisch verplaatst naar Azure wilt, kunt u het proces starten:

- Handmatig vanuit de Operations Management Suite-portal. 
- Programmatisch met behulp van de API AlertsVersion.  

> [!NOTE]
> Microsoft zal automatisch waarschuwingen die zijn gemaakt in Azure Government OMS portal exemplaren van Log Analytics voor Azure-waarschuwingen, uitgebreid vanaf 1 maart 2019 een systematische manier. Als er geen problemen voor het maken van [actiegroepen](../../azure-monitor/platform/action-groups.md), gebruikt u [deze herstelstappen](alerts-extend-tool.md#troubleshooting) om op te halen actiegroepen automatisch gemaakt. U kunt deze stappen tot 15 maart 2019 gebruiken in Azure Government OMS-portal.

## <a name="option-1-initiate-from-the-operations-management-suite-portal"></a>Optie 1: Starten vanuit de Operations Management Suite-portal
De volgende stappen wordt beschreven hoe u waarschuwingen voor de werkruimte van de Operations Management Suite-portal voor Azure Government-cloud uitbreiden.  

1. Selecteer in de Azure-portal de optie **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
2. In het deelvenster voor abonnementen van Log Analytics een werkruimte selecteren en selecteer vervolgens de **OMS-Portal** tegel.
![Schermafbeelding van Log Analytics-abonnement in het deelvenster met OMS-Portal tegel gemarkeerd](media/alerts-extend-tool/azure-portal-01.png) 
3. Nadat u wordt omgeleid naar de Operations Management Suite-portal, selecteert u de **instellingen** pictogram.
![Schermafbeelding van de Operations Management Suite-portal, met Instellingenpictogram gemarkeerd](media/alerts-extend-tool/oms-portal-settings-option.png) 
4. Uit de **instellingen** weergeeft, schakelt **waarschuwingen**.  
5. Selecteer **uitbreiden naar Azure**.
![Schermafbeelding van de Operations Management Suite waarschuwingsinstellingen portalpagina met uitbreiden naar Azure is gemarkeerd](media/alerts-extend-tool/ExtendInto.png)
6. Een wizard drie stappen wordt weergegeven de **waarschuwingen** deelvenster. Lees het overzicht en selecteer **volgende**.
![Schermafbeelding van stap 1 van de wizard](media/alerts-extend-tool/ExtendStep1.png)  
7. In de tweede stap ziet u een overzicht van voorgestelde wijzigingen aanbieding juiste [actiegroepen](../../azure-monitor/platform/action-groups.md) voor de waarschuwingen. Als dezelfde acties zijn waargenomen in meer dan één waarschuwing, wordt de wizard een groep met één actie koppelen aan al deze voorstelt.  De naamconventie is als volgt: *WorkspaceName_AG_#Number*. Om door te gaan, selecteert u **volgende**.
![Schermafbeelding van stap 2 van de wizard](media/alerts-extend-tool/ExtendStep2.png)  
8. Selecteer in de laatste stap van de wizard, **voltooien**, en controleer of wanneer u wordt gevraagd het proces te starten. Geef eventueel een e-mailadres, zodat u wordt gewaarschuwd wanneer het proces is voltooid en alle waarschuwingen zijn verplaatst naar Azure-waarschuwingen.
![Schermafbeelding van stap 3 van de wizard](media/alerts-extend-tool/ExtendStep3.png)

Wanneer de wizard is voltooid, op de **waarschuwingsinstellingen** pagina, de optie voor het uitbreiden van waarschuwingen naar Azure is verwijderd. Op de achtergrond uw waarschuwingen worden verplaatst naar Azure en dit kan enige tijd duren. Tijdens de bewerking kan niet u wijzigingen aanbrengen op waarschuwingen van de Operations Management Suite-portal. Hier ziet u de huidige status van de banner aan de bovenkant van de portal. Als u een e-mailadres eerder hebt opgegeven, wordt een e-mail ontvangen wanneer het proces is voltooid.  


Waarschuwingen worden weergegeven in de Operations Management Suite-portal, zelfs nadat ze zijn verplaatst naar Azure.
![Schermafbeelding van de Operations Management Suite portalpagina waarschuwingsinstellingen](media/alerts-extend-tool/PostExtendList.png)


## <a name="option-2-use-the-alertsversion-api"></a>Optie 2: De AlertsVersion API gebruiken
De Log Analytics AlertsVersion API kunt u waarschuwingen van Log Analytics in Azure-waarschuwingen uitbreiden van een willekeurige client die een REST-API kunt aanroepen. U kunt toegang tot de API vanuit PowerShell met behulp van [ARMClient](https://github.com/projectkudu/ARMClient), een open-source-opdrachtregelprogramma. U kunt de resultaten in JSON.  

Voor het gebruik van de API, moet u eerst een GET-aanvraag maken. Dit wordt geëvalueerd en retourneert een samenvatting van de voorgestelde wijzigingen voordat u daadwerkelijk uitbreiden naar Azure met behulp van een POST-aanvraag. De lijst met resultaten uw waarschuwingen en een voorgestelde lijst [actiegroepen](../../azure-monitor/platform/action-groups.md), in JSON-indeling. Als dezelfde acties zijn waargenomen in meer dan één waarschuwing, wordt de service al deze koppelen aan een groep met één actie. De naamconventie is als volgt: *WorkspaceName_AG_#Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Als de GET-aanvraag geslaagd is, HTTP-statuscode 200 wordt geretourneerd, samen met een lijst met waarschuwingen en voorgestelde actiegroepen in de JSON-gegevens. Hier volgt een voorbeeld van de reactie:

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
                            "serviceUri": "https://test.com"
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
Als de opgegeven werkruimte heeft geen geen waarschuwingsregels die is gedefinieerd, is de JSON-gegevens retourneert het volgende:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Als alle regels voor waarschuwingen in de opgegeven werkruimte hebben al is uitgebreid naar Azure, wordt het antwoord op de GET-aanvraag is:

```json
{
    "version": 2
}
```

Voor het starten van de waarschuwingen naar Azure te migreren, start u een reactie POST. De POST-antwoord wordt bevestigd dat uw doel, evenals acceptatie, als u wilt dat waarschuwingen van Log Analytics uitgebreid naar Azure-waarschuwingen. De activiteit is gepland en de waarschuwingen worden verwerkt, zoals wordt aangegeven, op basis van de resultaten wanneer u eerder hebt uitgevoerd van de GET-antwoord. U kunt eventueel opgeven dat een lijst met e-mailadressen in die Log Analytics een rapport verzonden wanneer de geplande achtergrondproces van de migratie van de waarschuwingen voltooid is. U kunt het volgende voorbeeld van de aanvraag:

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> Het resultaat van het migreren van de waarschuwingen in Azure-waarschuwingen kunnen variëren op basis van de samenvatting die wordt geleverd door GET-antwoord. Bij het plannen, zijn waarschuwingen in Log Analytics tijdelijk niet beschikbaar voor de wijziging in de Operations Management Suite-portal. U kunt echter nieuwe waarschuwingen maken. 

Als de POST-aanvraag geslaagd is, wordt de status van een HTTP 200 OK, samen met het volgende antwoord:

```json
{
    "version": 2
}
```

Dit antwoord geeft aan dat de waarschuwingen hebt uitgebreid naar Azure-waarschuwingen. De versie-eigenschap is alleen om te controleren of er waarschuwingen zijn uitgebreid naar Azure, en zijn niet gerelateerd aan de [Log Analytics Search API](../../azure-monitor/platform/api-alerts.md). Wanneer de waarschuwingen worden uitgebreid naar Azure, een e-mailadressen opgegeven met het bericht aanvraag een rapport worden verzonden. Als de waarschuwingen in de opgegeven werkruimte zijn al gepland worden uitgebreid, wordt het antwoord op uw POST-aanvraag is dat de poging (een 403-statuscode) is niet is toegestaan. Als u wilt weergeven van een foutmelding krijgt of begrijpen als het proces is vastgelopen, kunt u een GET-aanvraag indienen. Als er een foutbericht weergegeven, wordt deze geretourneerd, samen met de samenvattende informatie.

```json
{
    "version": 1,
    "message": "OMS was unable to extend your alerts into Azure, Error: The subscription is not registered to use the namespace 'microsoft.insights'. OMS will schedule extending your alerts, once remediation steps illustrated in the troubleshooting guide are done.",
    "recipients": [
       "john.doe@email.com",
       "jane.doe@email.com"
     ],
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
                            "serviceUri": "https://test.com"
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

## <a name="troubleshooting"></a>Problemen oplossen 
Tijdens het proces voor het uitbreiden van waarschuwingen, problemen kunnen voorkomen dat het systeem het maken van de benodigde [actiegroepen](../../azure-monitor/platform/action-groups.md). In dergelijke gevallen wordt er een foutbericht weergegeven in een banner in de **waarschuwing** gedeelte van de Operations Management Suite-portal of in de GET gereed is voor de API aanroepen.

> [!IMPORTANT]
> Als Azure Government-cloud op basis van gebruikers van de OMS-portal niet de volgende herstelstappen duren vóór 15 maart 2019, waarschuwingen in Azure wordt uitgevoerd, maar niet elke actie of een melding geactiveerd. Als u meldingen voor waarschuwingen, moet u handmatig de regels voor waarschuwingen in Azure te bewerken en toevoegen [actiegroepen](../../azure-monitor/platform/action-groups.md)

Hier volgen de herstelstappen uit voor elke fout:
- **Fout: Bereik vergrendeling aanwezig is op het niveau van bronabonnement/resourcegroep voor schrijfbewerkingen**:   ![Schermafbeelding van de Operations Management Suite waarschuwingsinstellingen-portalpagina met bereik vergrendelen foutbericht gemarkeerd](media/alerts-extend-tool/ErrorScopeLock.png)

    Als bereik vergrendeling is ingeschakeld, beperkt de functie nieuwe wijzigingen in de groep abonnement of resourcegroep met de werkruimte van Log Analytics (Operations Management Suite). Het systeem is niet waarschuwingen uitbreiden naar Azure en de benodigde Actiegroepen maken.
    
    Om op te lossen, verwijdert u de *ReadOnly* vergrendeling op uw abonnement of resourcegroep groep met de werkruimte. U kunt dit doen met behulp van de Azure-portal, PowerShell, Azure CLI of de API. Zie voor meer informatie, [vergrendeling Resourcegebruik](../../azure-resource-manager/resource-group-lock-resources.md). 
    
    Wanneer u de fout oplossen met behulp van de stappen in het artikel geïllustreerd, breidt Operations Management Suite uw waarschuwingen naar Azure binnen de geplande uitvoering van de volgende dag. U wilt geen verdere actie ondernemen of iets initiëren.

- **Fout: Beleid aanwezig is op het niveau van bronabonnement/resourcegroep**:   ![Schermafbeelding van de Operations Management Suite waarschuwingsinstellingen-portalpagina met beleid voor het foutbericht is gemarkeerd](media/alerts-extend-tool/ErrorPolicy.png)

    Wanneer [Azure Policy](../../governance/policy/overview.md) is toegepast, wordt een nieuwe resource in een abonnement of resourcegroep groep met de werkruimte van Log Analytics (Operations Management Suite) wordt beperkt. Het systeem is niet waarschuwingen uitbreiden naar Azure en de benodigde Actiegroepen maken.
    
    Als u wilt oplossen, bewerkt u het beleid dat wordt veroorzaakt door de *[RequestDisallowedByPolicy](../../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)* fout, waardoor het maken van nieuwe resources voor uw abonnement of resourcegroep groep met de werkruimte. U kunt dit doen met behulp van de Azure-portal, PowerShell, Azure CLI of de API. U kunt acties om het juiste beleid dat wordt veroorzaakt door fouten controleren. Zie voor meer informatie, [activiteit bekijken om te controleren van acties](../../azure-resource-manager/resource-group-audit.md). 
    
    Wanneer u de fout oplossen met behulp van de stappen in het artikel geïllustreerd, breidt Operations Management Suite uw waarschuwingen naar Azure binnen de geplande uitvoering van de volgende dag. U wilt geen verdere actie ondernemen of iets initiëren.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de nieuwe [ervaren Azure-waarschuwingen](../../azure-monitor/platform/alerts-overview.md).
* Meer informatie over [waarschuwingen voor activiteitenlogboeken in Azure-waarschuwingen](alerts-unified-log.md).

