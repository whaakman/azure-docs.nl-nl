---
title: Waarschuwingen van logboek Analytcs uitbreiden naar Azure
description: Dit artikel worden de hulpprogramma's en API waarmee u waarschuwingen van logboekanalyse op Azure-waarschuwingen kunt uitbreiden.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 21ba95a7b3efff177afe63d22da3f6ba9848ded2
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301028"
---
# <a name="extend-alerts-from-log-analytics-into-azure-alerts"></a>Waarschuwingen van logboekanalyse uitbreiden naar Azure-waarschuwingen
De functie alerts in Azure Log Analytics wordt vervangen door de Azure-waarschuwingen. Waarschuwingen die u oorspronkelijk hebt geconfigureerd in Log Analytics wordt als onderdeel van deze overgang wordt uitgebreid in Azure. Als u niet wilt wachten tot ze automatisch worden verplaatst naar Azure, kunt u het proces starten:

- Handmatig van de Operations Management Suite-portal. 
- Programmatisch met behulp van de API AlertsVersion.  

> [!NOTE]
> Microsoft wordt automatisch gemaakt in logboekanalyse tot Azure-waarschuwingen, waarschuwingen uitbreiden van 14 mei 2018, gestart in een reeks terugkerende totdat voltooid. Microsoft-planningen de waarschuwingen te migreren naar Azure en tijdens deze overgang, kunnen waarschuwingen worden beheerd vanaf de Operations Management Suite-portal en de Azure-portal. Dit proces is geen destructieve of interruptive.  

## <a name="option-1-initiate-from-the-operations-management-suite-portal"></a>Optie 1: Initiëren vanuit de Operations Management Suite-portal
De volgende stappen beschrijven het uitbreiden van waarschuwingen voor de werkruimte vanuit de Operations Management Suite-portal.  

1. Selecteer in de Azure-portal **alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
2. Selecteer een werkruimte in het deelvenster Log Analytics-abonnementen en selecteer vervolgens de **OMS-Portal** tegel.
![Schermopname van logboekanalyse abonnement deelvenster met OMS-Portal tegel gemarkeerd](./media/monitor-alerts-extend/azure-portal-01.png) 
3. Nadat u wordt omgeleid naar de Operations Management Suite-portal, selecteert u de **instellingen** pictogram.
![Schermopname van Operations Management Suite-portal met Instellingenpictogram gemarkeerd](./media/monitor-alerts-extend/oms-portal-settings-option.png) 
4. Van de **instellingen** pagina **waarschuwingen**.  
5. Selecteer **uitbreiden naar Azure**.
![Schermopname van Operations Management Suite waarschuwingsinstellingen portalpagina met uitbreiden naar Azure gemarkeerd](./media/monitor-alerts-extend/ExtendInto.png)
6. Een wizard drie stappen wordt weergegeven de **waarschuwingen** deelvenster. Lees het overzicht en selecteer **volgende**.
![Schermopname van stap 1 van de wizard](./media/monitor-alerts-extend/ExtendStep1.png)  
7. In de tweede stap u ziet een overzicht van voorgestelde wijzigingen aanbieding juiste [actiegroepen](monitoring-action-groups.md) voor de waarschuwingen. Als soortgelijke acties zijn waargenomen in meer dan één waarschuwing, wordt de wizard een groep met één actie koppelen aan deze voorstelt.  De naamconventie is als volgt: *WorkspaceName_AG_ #Number*. Selecteer het volgende om door te gaan **volgende**.
![Schermopname van stap 2 van de wizard](./media/monitor-alerts-extend/ExtendStep2.png)  
8. Selecteer in de laatste stap van de wizard **voltooien**, en bevestigt u wanneer u wordt gevraagd het proces te starten. U kunt eventueel een e-mailadres opgeven zodat u wordt gewaarschuwd wanneer het proces is voltooid en alle waarschuwingen zijn verplaatst naar de Azure-waarschuwingen.
![Schermopname van stap 3 van de wizard](./media/monitor-alerts-extend/ExtendStep3.png)

Wanneer de wizard is voltooid, op de **waarschuwingsinstellingen** pagina, de optie voor het uitbreiden van waarschuwingen naar Azure is verwijderd. Op de achtergrond uw waarschuwingen worden verplaatst naar Azure en dit kan enige tijd duren. Tijdens de bewerking kan niet u wijzigingen aanbrengen op waarschuwingen van de Operations Management Suite-portal. Hier ziet u de huidige status van de banner aan de bovenkant van de portal. Als u een e-mailadres eerder hebt opgegeven, ontvangt u een e-mailbericht wanneer het proces is voltooid.  


Waarschuwingen blijven worden vermeld in de Operations Management Suite-portal, zelfs nadat ze in Azure zijn verplaatst.
![Schermopname van Operations Management Suite portalpagina instellingen voor waarschuwingen](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2-use-the-alertsversion-api"></a>Optie 2: De AlertsVersion API gebruiken
De Log Analytics AlertsVersion API kunt u waarschuwingen van logboekanalyse in Azure waarschuwingen uitbreiden van een client die een REST-API kan aanroepen. U kunt de API openen vanuit PowerShell met behulp van [ARMClient](https://github.com/projectkudu/ARMClient), een opdrachtregelprogramma voor open-source. U kunt de resultaten in JSON.  

Als u wilt de API gebruiken, moet u eerst een GET-aanvraag maken. Dit wordt geëvalueerd en retourneert een samenvatting van de voorgestelde wijzigingen voordat u daadwerkelijk uitbreiden naar Azure met behulp van een POST-aanvraag. De lijst met resultaten uw waarschuwingen en een voorgestelde lijst van [actiegroepen](monitoring-action-groups.md), in JSON-indeling. Als soortgelijke acties zijn waargenomen in meer dan één waarschuwing, wordt de service wordt voorgesteld al deze koppelen aan een groep met één actie. De naamconventie is als volgt: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Als de GET-aanvraag geslaagd is, een HTTP-statuscode 200 wordt geretourneerd, samen met een lijst met waarschuwingen en voorgestelde actiegroepen in de JSON-gegevens. Hier volgt een voorbeeld van antwoord:

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
Als de opgegeven werkruimte geen geen waarschuwingsregels gedefinieerd, kan de JSON-gegevens retourneert het volgende:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Als u alle regels voor waarschuwingen in de opgegeven werkruimte al zijn uitgebreid naar Azure, wordt het antwoord op de GET-aanvraag is:

```json
{
    "version": 2
}
```

Start een POST-antwoord voor het initiëren van de waarschuwingen te migreren naar Azure. De POST-antwoord wordt bevestigd dat uw doel, evenals acceptatie, als u wilt dat waarschuwingen van logboekanalyse uitgebreid naar Azure-waarschuwingen. De activiteit is gepland en de waarschuwingen worden verwerkt, zoals wordt aangegeven, op basis van de resultaten wanneer u de GET-antwoord eerder hebt uitgevoerd. U kunt eventueel opgeven dat een lijst met e-mailadressen waarop Log Analytics een rapport verzonden wanneer de geplande achtergrondproces van de migratie van de waarschuwingen voltooid is. U kunt het volgende voorbeeld van de aanvraag:

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> Het resultaat van de migratie waarschuwingen in de Azure-waarschuwingen kunnen variëren, afhankelijk van de samenvatting geleverd door GET-antwoord. Bij het plannen, zijn waarschuwingen in logboekanalyse tijdelijk niet beschikbaar voor bewerken in de Operations Management Suite-portal. U kunt echter nieuwe waarschuwingen maken. 

Als de POST-aanvraag gelukt is, wordt de status van een HTTP 200 OK, samen met het volgende antwoord:

```json
{
    "version": 2
}
```

Dit antwoord geeft aan de waarschuwingen hebt uitgebreid naar Azure-waarschuwingen. De versie-eigenschap is alleen voor de controle of waarschuwingen zijn uitgebreid naar Azure en zijn niet gerelateerd aan de [Log Analytics-API van zoekservice](../log-analytics/log-analytics-api-alerts.md). Wanneer de waarschuwingen worden uitgebreid naar Azure, een e-mailadressen opgegeven met het bericht aanvraag een rapport worden verzonden. Als de waarschuwingen in de opgegeven werkruimte zijn al gepland worden uitgebreid, wordt het antwoord op uw POST-aanvraag is dat de poging (een 403-statuscode) is verboden. Als u een foutbericht te bekijken of begrijpen als het proces is vastgelopen, kunt u een GET-aanvraag indienen. Als er een foutbericht weergegeven, wordt deze geretourneerd, samen met de samenvattende informatie.

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


## <a name="option-3-use-a-custom-powershell-script"></a>Optie 3: Een aangepaste PowerShell-script gebruiken
 Als Microsoft niet is uitgebreid uw waarschuwingen van de Operations Management Suite-portal naar Azure, kunt u dit handmatig doen tot en met 5 juli 2018. De twee opties voor handmatige extensie worden in de vorige twee secties besproken.

Na 5 juli 2018, worden alle waarschuwingen van de Operations Management Suite-portal uitgebreid naar Azure. Gebruikers die niet worden de [nodig herstelstappen voorgestelde](#troubleshooting) hun waarschuwingen uitgevoerd zonder het activeren van acties of meldingen, vanwege het gebrek aan zal worden gekoppeld [actiegroepen](monitoring-action-groups.md). 

Maken van [actiegroepen](monitoring-action-groups.md) voor waarschuwingen handmatig in Log Analytics, het volgende voorbeeldscript gebruiken:
```PowerShell
########## Input Parameters Begin ###########


$subscriptionId = ""
$resourceGroup = ""
$workspaceName = "" 


########## Input Parameters End ###########

armclient login

try
{
    $workspace = armclient get /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/"$workspaceName"?api-version=2015-03-20 | ConvertFrom-Json
    $workspaceId = $workspace.properties.customerId
    $resourceLocation = $workspace.location
}
catch
{
    "Please enter valid input parameters i.e. Subscription Id, Resource Group and Workspace Name !!"
    exit
}

# Get Extend Summary of the Alerts
"`nGetting Extend Summary of Alerts for the workspace...`n"
try
{

    $value = armclient get /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/alertsversion?api-version=2017-04-26-preview

    "Extend preview summary"
    "=========================`n"

    $value

    $result = $value | ConvertFrom-Json
}
catch
{

    $ErrorMessage = $_.Exception.Message
    "Error occured while fetching/parsing Extend summary: $ErrorMessage"
    exit 
}

if ($result.version -eq 2)
{
    "`nThe alerts in this workspace have already been extended to Azure."
    exit
}

$in = Read-Host -Prompt "`nDo you want to continue extending the alerts to Azure? (Y/N)"

if ($in.ToLower() -ne "y")
{
    exit
} 


# Check for resource provider registration
try
{
    $val = armclient get subscriptions/$subscriptionId/providers/microsoft.insights/?api-version=2017-05-10 | ConvertFrom-Json
    if ($val.registrationState -eq "NotRegistered")
    {
        $val = armclient post subscriptions/$subscriptionId/providers/microsoft.insights/register/?api-version=2017-05-10
    }
}
catch
{
    "`nThe user does not have required access to register the resource provider. Please try with user having Contributor/Owner role in the subscription"
    exit
}

$actionGroupsMap = @{}
try
{
    "`nCreating new action groups for alerts extension...`n"
    foreach ($actionGroup in $result.migrationSummary.actionGroups)
    {
        $actionGroupName = $actionGroup.actionGroupName
        $actions = $actionGroup.actions
        if ($actionGroupsMap.ContainsKey($actionGroupName))
        {
            continue
        } 
        
        # Create action group payload
        $shortName = $actionGroupName.Substring($actionGroupName.LastIndexOf("AG_"))
        $properties = @{"groupShortName"= $shortName; "enabled" = $true}
        $emailReceivers = New-Object Object[] $actions.emailIds.Count
        $webhookReceivers = New-Object Object[] $actions.webhookActions.Count
        
        $count = 0
        foreach ($email in $actions.emailIds)
        {
            $emailReceivers[$count] = @{"name" = "Email$($count+1)"; "emailAddress" = "$email"}
            $count++
        }

        $count = 0
        foreach ($webhook in $actions.webhookActions)
        {
            $webhookReceivers[$count] = @{"name" = "$($webhook.name)"; "serviceUri" = "$($webhook.serviceUri)"}
            $count++
        }

        $itsmAction = $actions.itsmAction
        if ($itsmAction.connectionId -ne $null)
        {
            $val = @{
            "name" = "ITSM"
            "workspaceId" = "$subscriptionId|$workspaceId"
            "connectionId" = "$($itsmAction.connectionId)"
            "ticketConfiguration" = $itsmAction.templateInfo
            "region" = "$resourceLocation"
            }
            $properties["itsmReceivers"] = @($val)  
        }

        $properties["emailReceivers"] = @($emailReceivers)
        $properties["webhookReceivers"] = @($webhookReceivers)
        $armPayload = @{"properties" = $properties; "location" = "Global"} | ConvertTo-Json -Compress -Depth 4

    
        # Azure Resource Manager call to create action group
        $response = $armPayload | armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroupName/?api-version=2017-04-01

        "Created Action Group with name $actionGroupName" 
        $actionGroupsMap[$actionGroupName] = $actionGroup.actionGroupResourceId.ToLower()
        $index++
    }

    "`nSuccessfully created all action groups!!"
}
catch
{
    $ErrorMessage = $_.Exception.Message

    #Delete all action groups in case of failure
    "`nDeleting newly created action groups if any as some error happened..."
    
    foreach ($actionGroup in $actionGroupsMap.Keys)
    {
        $response = armclient delete /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroup/?api-version=2017-04-01      
    }

    "`nError: $ErrorMessage"
    "`nExiting..."
    exit
}

# Update all alerts configuration to the new version
"`nExtending OMS alerts to Azure...`n"

try
{
    $index = 1
    foreach ($alert in $result.migrationSummary.alerts)
    {
        $uri = $alert.alertId + "?api-version=2015-03-20"
        $config = armclient get $uri | ConvertFrom-Json
        $aznsNotification = @{
            "GroupIds" = @($actionGroupsMap[$alert.actionGroupName])
        }
        if ($alert.customWebhookPayload)
        {
            $aznsNotification.Add("CustomWebhookPayload", $alert.customWebhookPayload)
        }
        if ($alert.customEmailSubject)
        {
            $aznsNotification.Add("CustomEmailSubject", $alert.customEmailSubject)
        }      

        # Update alert version
        $config.properties.Version = 2

        $config.properties | Add-Member -MemberType NoteProperty -Name "AzNsNotification" -Value $aznsNotification
        $payload = $config | ConvertTo-Json -Depth 4
        $response = $payload | armclient put $uri
    
        "Extended alert with name $($alert.alertName)"
        $index++
    }
}
catch
{
    $ErrorMessage = $_.Exception.Message   
    if ($index -eq 1)
    {
        "`nDeleting all newly created action groups as no alerts got extended..."
        foreach ($actionGroup in $actionGroupsMap.Keys)
        {
            $response = armclient delete /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroup/?api-version=2017-04-01      
        }
        "`nDeleted all action groups."  
    }
    
    "`nError: $ErrorMessage"
    "`nPlease resolve the issue and try extending again!!"
    "`nExiting..."
    exit
}

"`nSuccessfully extended all OMS alerts to Azure!!" 

# Update version of workspace to indicate extension
"`nUpdating alert version information in OMS workspace..." 

$response = armclient post "/subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/alertsversion?api-version=2017-04-26-preview&iversion=2"

"`nExtension complete!!"
```


### <a name="about-the-custom-powershell-script"></a>Over het aangepaste PowerShell-script 
De volgende is belangrijke informatie over het gebruik van het script:
- Een vereiste is de installatie van [ARMclient](https://github.com/projectkudu/ARMClient), een open source-opdrachtregelprogramma dat vereenvoudigt de Azure Resource Manager-API wordt aangeroepen.
- Het script wordt uitgevoerd, moet u de rol Inzender of eigenaar in de Azure-abonnement hebt.
- U moet de volgende parameters opgeven:
    - $subscriptionId: de Azure-abonnements-ID die is gekoppeld aan de werkruimte voor logboekanalyse voor Operations Management Suite.
    - $resourceGroup: de Azure-resourcegroep voor de werkruimte voor logboekanalyse voor Operations Management Suite.
    - $workspaceName: de naam van de werkruimte voor logboekanalyse voor Operations Management Suite.

### <a name="output-of-the-custom-powershell-script"></a>Uitvoer van het aangepaste PowerShell-script
Het script is uitgebreid en levert de stappen uit als dit wordt uitgevoerd: 
- De samenvatting, bevat de informatie over de bestaande Operations Management Suite Log Analytics-waarschuwingen in de werkruimte wordt weergegeven. De samenvatting bevat ook informatie over de Azure actiegroepen worden gemaakt voor de acties die zijn gekoppeld. 
- U wordt gevraagd te gaat u verder gaan met de extensie of sluiten nadat de samenvatting weer te geven.
- Als u verder met de extensie gaat, nieuwe Azure actiegroepen zijn gemaakt en de bestaande waarschuwingen zijn gekoppeld aan deze. 
- Het script wordt afgesloten door het bericht "Extensie is voltooid!" weer te geven Het script worden in het geval van alle tussenliggende mislukte verdere fouten weergegeven.

## <a name="troubleshooting"></a>Problemen oplossen 
Tijdens het proces voor het uitbreiden van waarschuwingen, problemen kunnen voorkomen dat het systeem maakt de benodigde [actiegroepen](monitoring-action-groups.md). In dergelijke gevallen wordt er een foutbericht weergegeven in een banner in de **waarschuwing** sectie van de Operations Management Suite-portal of in de GET gereed is voor de API aanroepen.

> [!IMPORTANT]
> Als u niet de volgende herstelstappen uit voordat u 5 juli 2018 uitvoeren, waarschuwingen in Azure wordt uitgevoerd, maar wordt niet gestart een actie of een melding. Als u meldingen voor waarschuwingen, moet u handmatig bewerken en toevoegen [actiegroepen](monitoring-action-groups.md), of gebruik de voorgaande [aangepaste PowerShell-script](#option-3---using-custom-powershell-script).

Hier volgen de herstelstappen voor elke fout:
- **Fout: Bereik vergrendeling is aanwezig op abonnement/resourcegroep voor schrijfbewerkingen**: ![schermafbeelding van de Operations Management Suite waarschuwingsinstellingen portalpagina met bereik vergrendelen foutbericht gemarkeerd](./media/monitor-alerts-extend/ErrorScopeLock.png)

    Als bereik vergrendeling is ingeschakeld, wordt de functie beperkt nieuwe wijzigingen in de groep abonnement of resourcegroep met de werkruimte voor logboekanalyse (Operations Management Suite). Het systeem is niet waarschuwingen in Azure uitbreiden en benodigde Actiegroepen maken.
    
    Om op te lossen, verwijdert u de *ReadOnly* vergrendeling op uw abonnement of de resource-groep die de werkruimte bevat. U kunt dit doen met behulp van de Azure-portal, PowerShell, Azure CLI of de API. Zie voor meer informatie, [vergrendeling Resourcegebruik](../azure-resource-manager/resource-group-lock-resources.md). 
    
    Wanneer u de fout oplossen met behulp van de stappen die worden weergegeven in het artikel, breidt Operations Management Suite uw waarschuwingen in Azure binnen de geplande run van de volgende dag. U hoeft niet te verdere actie ondernemen of het initiëren van alles.

- **Fout: Beleid is aanwezig op het niveau van de abonnement/resourcegroep**: ![schermafbeelding van de Operations Management Suite waarschuwingsinstellingen portalpagina met beleid foutbericht gemarkeerd](./media/monitor-alerts-extend/ErrorPolicy.png)

    Wanneer [Azure beleid](../azure-policy/azure-policy-introduction.md) is toegepast, beperkt alle nieuwe bronnen in een abonnement of resourcegroep groep met de werkruimte voor logboekanalyse (Operations Management Suite). Het systeem is niet waarschuwingen in Azure uitbreiden en benodigde Actiegroepen maken.
    
    Om op te lossen, bewerkt u het beleid dat wordt veroorzaakt door de *[RequestDisallowedByPolicy](../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)* fout, die voorkomt het maken van nieuwe resources van uw abonnement of de resource-groep die de werkruimte bevat. U kunt dit doen met behulp van de Azure-portal, PowerShell, Azure CLI of de API. U kunt controleren acties uit om te zoeken naar het juiste beleid fout veroorzaakt. Zie voor meer informatie, [activiteitenlogboeken om te controleren van acties weer te geven](../azure-resource-manager/resource-group-audit.md). 
    
    Wanneer u de fout oplossen met behulp van de stappen die worden weergegeven in het artikel, breidt Operations Management Suite uw waarschuwingen in Azure binnen de geplande run van de volgende dag. U hoeft niet te verdere actie ondernemen of het initiëren van alles.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de nieuwe [Azure waarschuwingen optreden](monitoring-overview-unified-alerts.md).
* Meer informatie over [waarschuwingen melden in Azure waarschuwingen](monitor-alerts-unified-log.md).
