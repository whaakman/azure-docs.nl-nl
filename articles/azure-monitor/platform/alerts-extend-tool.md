---
title: Waarschuwingen van Log Analytics uitbreiden naar Azure
description: Dit artikel beschrijft de hulpprogramma's en API waarmee u waarschuwingen van Log Analytics naar Azure-waarschuwingen kunt uitbreiden.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 0524e38f155049c630ace7f33805ef230c8dccca
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53344241"
---
# <a name="extend-alerts-from-log-analytics-into-azure-alerts"></a>Waarschuwingen van Log Analytics uitbreiden naar Azure-waarschuwingen
De functie alerts in Azure Log Analytics wordt vervangen door de Azure-waarschuwingen. Als onderdeel van deze overgang, wordt de waarschuwingen die u oorspronkelijk hebt geconfigureerd in Log Analytics worden uitgebreid naar Azure. Als u niet wachten tot deze worden automatisch verplaatst naar Azure wilt, kunt u het proces starten:

- Handmatig vanuit de Operations Management Suite-portal. 
- Programmatisch met behulp van de API AlertsVersion.  

> [!NOTE]
> Microsoft zal automatisch waarschuwingen die zijn gemaakt in de openbare cloud exemplaren van Log Analytics voor Azure-waarschuwingen, uitgebreid vanaf 14 mei 2018 wordt in een reeks terugkerende tot voltooid. Als er geen problemen voor het maken van [actiegroepen](../../azure-monitor/platform/action-groups.md), gebruikt u [deze herstelstappen](alerts-extend-tool.md#troubleshooting) om op te halen actiegroepen automatisch gemaakt. U kunt deze stappen gebruiken tot 5 juli 2018. *Niet van toepassing voor Azure Government en onafhankelijke cloudgebruikers van Log Analytics*. 

## <a name="option-1-initiate-from-the-operations-management-suite-portal"></a>Optie 1: Starten vanuit de Operations Management Suite-portal
De volgende stappen wordt beschreven hoe u waarschuwingen voor de werkruimte van de Operations Management Suite-portal uitbreiden.  

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
7. In de tweede stap ziet u een overzicht van voorgestelde wijzigingen aanbieding juiste [actiegroepen](../../azure-monitor/platform/action-groups.md) voor de waarschuwingen. Als dezelfde acties zijn waargenomen in meer dan één waarschuwing, wordt de wizard een groep met één actie koppelen aan al deze voorstelt.  De naamconventie is als volgt: *WorkspaceName_AG_ #Number*. Om door te gaan, selecteert u **volgende**.
![Schermafbeelding van stap 2 van de wizard](media/alerts-extend-tool/ExtendStep2.png)  
8. Selecteer in de laatste stap van de wizard, **voltooien**, en controleer of wanneer u wordt gevraagd het proces te starten. Geef eventueel een e-mailadres, zodat u wordt gewaarschuwd wanneer het proces is voltooid en alle waarschuwingen zijn verplaatst naar Azure-waarschuwingen.
![Schermafbeelding van stap 3 van de wizard](media/alerts-extend-tool/ExtendStep3.png)

Wanneer de wizard is voltooid, op de **waarschuwingsinstellingen** pagina, de optie voor het uitbreiden van waarschuwingen naar Azure is verwijderd. Op de achtergrond uw waarschuwingen worden verplaatst naar Azure en dit kan enige tijd duren. Tijdens de bewerking kan niet u wijzigingen aanbrengen op waarschuwingen van de Operations Management Suite-portal. Hier ziet u de huidige status van de banner aan de bovenkant van de portal. Als u een e-mailadres eerder hebt opgegeven, wordt een e-mail ontvangen wanneer het proces is voltooid.  


Waarschuwingen worden weergegeven in de Operations Management Suite-portal, zelfs nadat ze zijn verplaatst naar Azure.
![Schermafbeelding van de Operations Management Suite portalpagina waarschuwingsinstellingen](media/alerts-extend-tool/PostExtendList.png)


## <a name="option-2-use-the-alertsversion-api"></a>Optie 2: De AlertsVersion API gebruiken
De Log Analytics AlertsVersion API kunt u waarschuwingen van Log Analytics in Azure-waarschuwingen uitbreiden van een willekeurige client die een REST-API kunt aanroepen. U kunt toegang tot de API vanuit PowerShell met behulp van [ARMClient](https://github.com/projectkudu/ARMClient), een open-source-opdrachtregelprogramma. U kunt de resultaten in JSON.  

Voor het gebruik van de API, moet u eerst een GET-aanvraag maken. Dit wordt geëvalueerd en retourneert een samenvatting van de voorgestelde wijzigingen voordat u daadwerkelijk uitbreiden naar Azure met behulp van een POST-aanvraag. De lijst met resultaten uw waarschuwingen en een voorgestelde lijst [actiegroepen](../../azure-monitor/platform/action-groups.md), in JSON-indeling. Als dezelfde acties zijn waargenomen in meer dan één waarschuwing, wordt de service al deze koppelen aan een groep met één actie. De naamconventie is als volgt: *WorkspaceName_AG_ #Number*.

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


## <a name="option-3-use-a-custom-powershell-script"></a>Optie 3: Gebruik een aangepaste PowerShell-script
 Als Microsoft niet is uitgebreid uw waarschuwingen vanuit de Operations Management Suite-portal naar Azure, kunt u dit handmatig doen tot 5 juli 2018. De twee opties voor handmatige extensie worden behandeld in de vorige twee secties.

Alle waarschuwingen van de Operations Management Suite-portal worden na 5 juli 2018 uitgebreid naar Azure. Gebruikers die niet de [nodig herstelstappen](#troubleshooting) hun waarschuwingen die worden uitgevoerd zonder het uitvoeren van acties of meldingen, vanwege het gebrek aan zal worden gekoppeld [actiegroepen](../../azure-monitor/platform/action-groups.md). 

Maken [actiegroepen](../../azure-monitor/platform/action-groups.md) voor waarschuwingen handmatig in Log Analytics, het volgende voorbeeldscript gebruiken:
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


### <a name="about-the-custom-powershell-script"></a>Over de aangepaste PowerShell-script 
De volgende is belangrijke informatie over het gebruik van het script:
- Een vereiste is de installatie van [ARMclient](https://github.com/projectkudu/ARMClient), een open-source-opdrachtregelprogramma dat vereenvoudigt het aanroepen van de Azure Resource Manager-API.
- Het script wordt uitgevoerd, moet u een rol van inzender of eigenaar in het Azure-abonnement hebben.
- U moet de volgende parameters opgeven:
    - $subscriptionId: De Azure abonnements-ID die is gekoppeld aan de Operations Management Suite Log Analytics-werkruimte.
    - $resourceGroup: De Azure-resourcegroep voor Operations Management Suite Log Analytics-werkruimte.
    - $workspaceName: De naam van de Operations Management Suite Log Analytics-werkruimte.

### <a name="output-of-the-custom-powershell-script"></a>Uitvoer van de aangepaste PowerShell-script
Het script is uitgebreid en voert de stappen uit als dit wordt uitgevoerd: 
- De samenvatting, waarin u de informatie over de bestaande Operations Management Suite Log Analytics-waarschuwingen in de werkruimte wordt weergegeven. De samenvatting bevat ook informatie over de Azure-actiegroepen worden gemaakt voor de acties die zijn gekoppeld aan deze. 
- U wordt gevraagd om te gaan met de extensie of sluiten nadat de samenvatting bekijken.
- Wanneer u verder met de extensie gaat, nieuwe Azure-actiegroepen worden gemaakt en alle bestaande waarschuwingen zijn gekoppeld aan deze. 
- Het script wordt afgesloten door het bericht "Extensie is voltooid!" weer te geven Het script geeft de volgende fouten weer in het geval van eventuele tussenliggende fouten.

## <a name="troubleshooting"></a>Problemen oplossen 
Tijdens het proces voor het uitbreiden van waarschuwingen, problemen kunnen voorkomen dat het systeem het maken van de benodigde [actiegroepen](../../azure-monitor/platform/action-groups.md). In dergelijke gevallen wordt er een foutbericht weergegeven in een banner in de **waarschuwing** gedeelte van de Operations Management Suite-portal of in de GET gereed is voor de API aanroepen.

> [!IMPORTANT]
> Als openbare Azure-cloud op basis van Log Analytics-gebruikers niet de volgende herstelstappen duren vóór 5 juli 2018, waarschuwingen in Azure wordt uitgevoerd, maar niet elke actie of een melding geactiveerd. Als u meldingen voor waarschuwingen, moet u handmatig bewerken en toevoegen [actiegroepen](../../azure-monitor/platform/action-groups.md), of gebruik de voorgaande [aangepaste PowerShell-script](#option-3---using-custom-powershell-script).

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
