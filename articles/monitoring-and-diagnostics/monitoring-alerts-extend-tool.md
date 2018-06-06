---
title: Waarschuwingen van logboek Analytcs uitbreiden naar Azure | Microsoft Docs
description: Dit artikel worden de hulpprogramma's en API waarmee u waarschuwingen van logboekanalyse op Azure-waarschuwingen kunt uitbreiden.
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
ms.date: 06/04/2018
ms.author: vinagara
ms.openlocfilehash: 0dce6e6772b4efea90df2e095ac0041641d99061
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763524"
---
# <a name="how-to-extend-alerts-from-log-analytics-into-azure-alerts"></a>Het uitbreiden van waarschuwingen van logboekanalyse in Azure-waarschuwingen
Waarschuwingen in Log Analytics met Azure-waarschuwingen en als onderdeel van deze overgang wordt vervangen, waarschuwingen die u hebt geconfigureerd in logboekanalyse zal worden uitgebreid in Azure.  Als u niet wilt wachten tot ze automatisch worden verplaatst naar Azure, kunt u het volgende op een van de opties proces starten:

1. Handmatig van de OMS-portal 
2. Programmatisch met behulp van de API AlertsVersion  

> [!NOTE]
> Waarschuwingen die zijn gemaakt in logboekanalyse te starten op waarschuwingen van Azure wordt automatisch uitbreiden door Microsoft **14 mei 2018** in een gefaseerde benadering totdat voltooid. Vanaf deze dag Microsoft begint de waarschuwingen te migreren naar Azure plannen en tijdens deze overgang waarschuwingen kunnen worden beheerd vanaf de OMS-portal en de Azure-portal. Dit proces is een niet-destructieve en niet interruptive.  

## <a name="option-1---initiate-from-the-oms-portal"></a>Optie 1 - initiëren vanuit de OMS-Portal
De volgende stappen beschrijven het uitbreiden van waarschuwingen voor de werkruimte in de OMS-portal.  

1. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
2. Selecteer een werkruimte in het deelvenster Log Analytics-abonnementen en selecteer vervolgens de **OMS-Portal** tegel.<br><br> ![De knop Zoeken in logboeken](./media/monitor-alerts-extend/azure-portal-01.png)<br><br> 
3. Nadat u wordt omgeleid naar de OMS-portal, klikt u op de tegel instellingen in de rechterbovenhoek van de pagina.<br><br> ![OMS-portal instellingenoptie](./media/monitor-alerts-extend/oms-portal-settings-option.png)<br><br> 
4. Van de **instellingen** pagina **waarschuwingen**.  
5. Klik op de knop **uitbreiden naar Azure**.<br><br> ![OMS waarschuwingsinstellingen portalpagina met de optie uitbreiden](./media/monitor-alerts-extend/ExtendInto.png)
6. Een wizard is opgenomen in het deelvenster met de eerste drie stappen geven een overzicht van het proces.  Klik op **volgende** om door te gaan.<br><br> ![Waarschuwingen van logboekanalyse uitbreiden naar Azure - stap 1](./media/monitor-alerts-extend/ExtendStep1.png)  
7. In de tweede stap van een overzicht van voorgestelde wijzigingen wordt weergegeven met het juiste aanbieding [actiegroepen](monitoring-action-groups.md) voor de waarschuwingen. Als soortgelijke acties zijn waargenomen in meer dan één waarschuwing, stelt de service om te koppelen aan deze een groep met één actie.  Actiegroep voorgestelde volgt u de naamconventie: *WorkspaceName_AG_ #Number*. Als u wilt doorgaan, klikt u op **volgende**.<br><br> ![waarschuwingen van logboekanalyse uitbreiden naar Azure - stap 2](./media/monitor-alerts-extend/ExtendStep2.png)  
8. Klik in de laatste stap van de wizard, op **voltooien** en bevestigt u wanneer u wordt gevraagd het proces te starten.  U kunt eventueel een e-mailadressen opgeven zodat u wordt gewaarschuwd wanneer het proces is voltooid en alle waarschuwingen zijn verplaatst naar de Azure-waarschuwingen.<br><br> ![Waarschuwingen van logboekanalyse uitbreiden naar Azure - stap 3](./media/monitor-alerts-extend/ExtendStep3.png)

Zodra de wizard voltooid is, ziet u op de **waarschuwingsinstellingen** pagina waarop de optie uit te breiden naar Azure waarschuwingen is verwijderd.  Op de achtergrond uw waarschuwingen worden verplaatst naar Azure en dit kan enige tijd duren.  Tijdens de bewerking zich u niet aanbrengen op waarschuwingen van de OMS-portal.  De huidige status van de banner aan de bovenkant van de portal worden weergegeven, en als u eerder een e-mailadres opgegeven ontvangt u een e-mailbericht wanneer het proces is voltooid.  


Waarschuwingen blijven worden vermeld in de OMS-portal, zelfs nadat ze in Azure zijn verplaatst.<br><br> ![Nadat u waarschuwingen in logboekanalyse naar Azure](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2---using-the-alertsversion-api"></a>Optie 2 - met behulp van de AlertsVersion API
De Log Analytics AlertsVersion API kunt u waarschuwingen van logboekanalyse in Azure waarschuwingen uitbreiden van een client die een REST-API kan aanroepen. U kunt openen vanaf het met behulp van PowerShell [ARMClient](https://github.com/projectkudu/ARMClient), een open source-opdrachtregelprogramma dat vereenvoudigt de Azure Resource Manager-API wordt aangeroepen. Het gebruik van ARMClient en PowerShell is een van de verschillende opties om toegang te krijgen tot de API.  Met de API wordt de resultaten in JSON.  

Als u wilt de API gebruiken, moet u eerst een GET-aanvraag die wordt geëvalueerd en retourneren een samenvatting van de voorgestelde wijzigingen voordat u daadwerkelijk uitbreiden naar Azure met behulp van een POST-aanvraag maken. De lijst met resultaten uw waarschuwingen en de voorgestelde lijst met [actiegroepen](monitoring-action-groups.md) in JSON-indeling.  Soortgelijke acties zijn waargenomen in meer dan één waarschuwing, worden de service voorgesteld als deze koppelen aan een groep met één actie.  Voorgestelde actiegroepen volgt u de naamconventie: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Als de GET-aanvraag gelukt is, wordt er een HTTP-statuscode 200 samen met een lijst met waarschuwingen en voorgestelde actiegroepen in de JSON-gegevens geretourneerd. Hier volgt een voorbeeld van antwoord:

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
Als de opgegeven werkruimte geen geen waarschuwingsregels gedefinieerd, inclusief de status HTTP 200 OK code voor de GET-bewerking de JSON-gegevens als resultaat:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Als u alle regels voor waarschuwingen in de opgegeven werkruimte al zijn uitgebreid naar Azure - wordt het antwoord op de GET-aanvraag is:

```json
{
    "version": 2
}
```

Start een POST-antwoord voor het initiëren van de waarschuwingen te migreren naar Azure. De POST-antwoord wordt bevestigd dat uw bedoeling evenals acceptatie om waarschuwingen van logboekanalyse uitgebreid naar Auzre waarschuwingen.  De activiteit is gepland en de waarschuwingen worden verwerkt, zoals wordt aangegeven op basis van de resultaten wanneer u de GET-antwoord eerder hebt uitgevoerd.  U kunt eventueel opgeven dat een lijst met e-mailadressen waarop Log Analytics wordt een rapport te verzenden wanneer de geplande achtergrondproces van de migratie van de waarschuwingen voltooid is.  Dit wordt uitgevoerd met behulp van het volgende voorbeeld van de aanvraag:

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> Resultaat van migreren waarschuwingen in de Azure-waarschuwingen kunnen variëren, afhankelijk van de samenvatting geleverd door GET-antwoord.  Als gepland, is waarschuwingen in logboekanalyse worden tijdelijk niet beschikbaar voor bewerken/wijziging in de OMS-portal.  Nieuwe waarschuwingen kunnen echter worden gemaakt. 

Als de POST-aanvraag gelukt is, wordt de status van een HTTP 200 OK samen met het volgende antwoord:

```json
{
    "version": 2
}
```

Dit antwoord geeft aan de waarschuwingen hebt uitgebreid naar Azure-waarschuwingen. De versie-eigenschap is alleen voor de controle of waarschuwingen zijn uitgebreid naar Azure en zijn niet gerelateerd aan de [Log Analytics-API van zoekservice](../log-analytics/log-analytics-api-alerts.md). Zodra de waarschuwingen worden uitgebreid naar Azure, wordt een e-mailbericht adressen opgegeven met het bericht aanvraag een rapport met details van de wijzigingen die zijn uitgevoerd worden verzonden.  Als alle van de waarschuwingen in de opgegeven werkruimte al zijn gepland worden uitgebreid, is het antwoord op uw POST-aanvraag een 403 status code betekenis die de poging is verboden. Eventuele foutberichten weergeven of als het proces is vastgelopen, kunt u een GET-aanvraag en een foutbericht, indien aanwezig, indienen begrijpen worden samen met de overzichtsgegevens geretourneerd.

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


## <a name="option-3---using-custom-powershell-script"></a>Optie 3 - met behulp van aangepaste PowerShell-script
 Na mei 14, 2018 - als Microsoft niet is uitgebreid uw waarschuwingen van OMS-portal naar Azure. vervolgens tot **5 juli 2018** -gebruiker kan handmatig doen via hetzelfde [optie 1 - GUI Via](#option-1---initiate-from-the-oms-portal) of [optie 2 - Via API](#option-2---using-the-alertsversion-api).

Na **5 juli 2018** -alle waarschuwingen van OMS-portal zal worden uitgebreid in Azure. Gebruikers die niet worden de [nodig herstelstappen voorgestelde](#troubleshooting), hun waarschuwingen uitgevoerd zonder het activeren van acties of meldingen vanwege het gebrek aan zal worden gekoppeld [actiegroepen](monitoring-action-groups.md). 

Handmatig maken [actiegroepen](monitoring-action-groups.md) voor waarschuwingen in logboekanalyse gebruikers het volgende voorbeeldscript kunnen gebruiken.
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

    
        # ARM call to create action group
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


**Met behulp van het aangepaste PowerShell-script** 
- Vereiste wordt de installatie van [ARMclient](https://github.com/projectkudu/ARMClient), een open source-opdrachtregelprogramma dat vereenvoudigt de Azure Resource Manager-API aanroepen
- Gebruiker uitvoeren van het genoemde script zijn rol van inzender of de eigenaar van het Azure-abonnement
- Hier volgen de parameters worden opgegeven voor het script:
    - $subscriptionId: de Azure-abonnements-ID die is gekoppeld aan de werkruimte OMS/LA
    - $resourceGroup: de Azure-resourcegroep waar zich de OMS/LA-werkruimte
    - $workspaceName: de naam van de werkruimte OMS/LA

**Uitvoer van het aangepaste PowerShell-script** het script is uitgebreid en de stappen wordt de uitvoer zoals deze wordt uitgevoerd. 
- De samenvatting, bevat de informatie over de bestaande OMS/LA waarschuwingen in de werkruimte en de actiegroepen Azure moet worden gemaakt voor de acties die zijn gekoppeld aan deze wordt weergegeven. 
- Gebruiker wordt gevraagd gaat u verder gaan met de extensie of sluiten nadat de samenvatting weer te geven.
- Als de gebruiker wordt gevraagd om te gaan verder met de extensie, nieuwe Azure actiegroepen wordt gemaakt en de bestaande waarschuwingen worden gekoppeld aan deze. 
- Uiteindelijk het script wordt afgesloten door het bericht weer te geven ' uitbreiding voltooid!. " In geval van een tussenliggende fouten, wordt de volgende fouten weergegeven.

## <a name="troubleshooting"></a>Problemen oplossen 
Tijdens het proces van waarschuwingen van OMS uit te breiden naar Azure, kunnen er incidentele probleem dat verhindert dat het systeem maken nodig [actiegroepen](monitoring-action-groups.md). In dergelijke gevallen wordt een foutbericht wordt weergegeven in OMS-portal via banner in waarschuwing sectie en GET-aanroep gedaan API.

> [!WARNING]
> Als gebruiker de precribed herstelstappen hieronder vindt u, voordat niet uitvoeren **5 juli 2018** - en waarschuwingen wordt uitgevoerd in Azure, maar zonder een actie of een melding geactiveerd. Als u meldingen voor waarschuwingen, gebruikers moeten handmatig bewerken en voeg [actiegroepen](monitoring-action-groups.md) of gebruik de [aangepaste PowerShell-script](#option-3---using-custom-powershell-script) hierboven beschreven.

Hieronder vindt u de herstelstappen uit voor elke fout:
1. **Fout: Bereik vergrendeling is aanwezig op abonnement/resourcegroep voor schrijfbewerkingen**: ![OMS waarschuwingsinstellingen portalpagina met ScopeLock foutbericht](./media/monitor-alerts-extend/ErrorScopeLock.png)

    a. Wanneer bereik vergrendelen is ingeschakeld, beperken van de nieuwe wijzigingen van abonnement of resourcegroep met de werkruimte voor logboekanalyse (OMS); het systeem is niet waarschuwingen (kopiëren) in Azure uitbreiden en benodigde Actiegroepen maken.
    
    b. Om op te lossen, verwijdert u de *ReadOnly* vergrendeling op uw abonnement of de resource-groep die de werkruimte bevat; met Azure-portal, Powershell, Azure CLI of API. Bekijk voor meer informatie het artikel op [vergrendeling Resourcegebruik](../azure-resource-manager/resource-group-lock-resources.md). 
    
    c. Als opgelost volgens de stappen die worden weergegeven in het artikel, wordt OMS uw waarschuwingen in Azure uitbreiden in de volgende dag geplande run; zonder de noodzaak van een actie of een inleiding.

2. **Fout: Beleid is aanwezig op het niveau van de abonnement/resourcegroep**: ![OMS waarschuwingsinstellingen portalpagina met beleid foutbericht](./media/monitor-alerts-extend/ErrorPolicy.png)

    a. Wanneer [Azure beleid](../azure-policy/azure-policy-introduction.md) wordt toegepast, beperken van alle nieuwe bronnen in het abonnement of resourcegroep met de werkruimte voor logboekanalyse (OMS); wordt door het systeem geen waarschuwingen (kopiëren) in Azure uitbreiden en benodigde Actiegroepen maken.
    
    b. Bewerken om op te lossen, het beleid waardoor *[RequestDisallowedByPolicy](../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)* fout, die voorkomt het maken van nieuwe resources voor uw abonnement of de resource-groep met de werkruimte. Met behulp van Azure-portal, Powershell, Azure CLI of API; u kunt mislukte acties uit om te zoeken naar het juiste beleid ontstaan. Bekijk voor meer informatie het artikel op [activiteitenlogboeken om te controleren van acties weer te geven](../azure-resource-manager/resource-group-audit.md). 
    
    c. Als opgelost volgens de stappen die worden weergegeven in het artikel, wordt OMS uw waarschuwingen in Azure uitbreiden in de volgende dag geplande run; zonder de noodzaak van een actie of een inleiding.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de nieuwe [waarschuwingen van Azure-ervaring](monitoring-overview-unified-alerts.md).
* Meer informatie over [waarschuwingen melden in Azure waarschuwingen](monitor-alerts-unified-log.md).
