---
title: Actiegroepen in Azure portal maken en beheren
description: Informatie over het maken en beheren van actiegroepen in Azure portal.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 7/08/2019
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 842965aa49ae4cd546fe9c107107d2a2ceebebbb
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705261"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Actiegroepen in Azure portal maken en beheren
Een actiegroep is een verzameling van voorkeuren voor meldingen gedefinieerd door de eigenaar van een Azure-abonnement. Waarschuwingen van Azure Monitor en de servicestatus actiegroepen gebruiken om gebruikers te waarschuwen dat een waarschuwing is geactiveerd. Verschillende waarschuwingen kunnen gebruiken voor de actiegroep dezelfde of verschillende actiegroepen, afhankelijk van de vereisten van de gebruiker. U kunt maximaal 2.000 actiegroepen configureren in een abonnement.

U configureren een actie om de hoogte van een persoon via e-mail of SMS, ontvangen ze een bevestiging die aangeeft dat ze zijn toegevoegd aan de actiegroep te.

In dit artikel wordt beschreven hoe u maken en beheren van actiegroepen in Azure portal.

Elke actie bestaat uit de volgende eigenschappen:

* **Naam**: Een unieke id binnen de actiegroep.  
* **Actietype**: De actie uitgevoerd. Voorbeelden zijn onder meer het verzenden van een stem oproep, SMS, e-mail; of verschillende soorten acties die automatisch wordt geactiveerd. Zie typen verderop in dit artikel.
* **Details**: De bijbehorende details die per variëren *actietype*.

Zie voor meer informatie over het gebruik van Azure Resource Manager-sjablonen configureren actiegroepen [actie groep Resource Manager-sjablonen](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Een actiegroep maken met behulp van de Azure-portal

1. In de [Azure-portal](https://portal.azure.com), selecteer **Monitor**. De **Monitor** deelvenster consolideert alle controle-instellingen en gegevens in één weergave.

    ![De service 'Controleren'](./media/action-groups/home-monitor.png)
    
1. Selecteer **waarschuwingen** Selecteer **acties beheren**.

    ![Knop acties beheren](./media/action-groups/manage-action-groups.png)
    
1. Selecteer **actiegroep toevoegen**, en vul de velden in.

    ![De opdracht 'Actiegroep toevoegen'](./media/action-groups/add-action-group.png)
    
1. Voer een naam in de **naam van de actiegroep** vak en voer een naam in de **afkorting** vak. De korte naam wordt gebruikt in plaats van een volledige naam van de actiegroep als er meldingen via deze groep worden verzonden.

      ![In het dialoogvenster van de actiegroep toevoegen'](./media/action-groups/action-group-define.png)

1. De **abonnement** vak autofills met uw huidige abonnement. Dit abonnement is de waarin de actiegroep is opgeslagen.

1. Selecteer de **resourcegroep** in de actiegroep is opgeslagen.

1. Een lijst met acties definiëren. Hiermee geeft u de volgende voor elke actie:

    1. **Naam**: Voer een unieke id voor deze actie.

    1. **Actietype**: Selecteer de e-mailadres/SMS/Push/Voice, logische App, Webhook, ITSM of Automation-Runbook.

    1. **Details**: Op basis van het actietype, voer een telefoonnummer, e-mailadres, webhook URI, Azure-app, ITSM-verbinding of Automation-runbook. Voor ITSM-actie, Daarnaast Geef **werkitem** en andere velden uw ITSM-hulpprogramma is vereist.
    
    1. **Algemene waarschuwing schema**: U kunt kiezen om in te schakelen de [algemene waarschuwing schema](https://aka.ms/commonAlertSchemaDocs), waarmee u het voordeel dat een enkele, uitbreidbare en geïntegreerde alert payload voor alle van de waarschuwing services in Azure Monitor.

1. Selecteer **OK** te maken van de actiegroep.

## <a name="manage-your-action-groups"></a>Beheren van uw actiegroepen

Nadat u een actiegroep die u hebt gemaakt, wordt het weergegeven in de **actiegroepen** sectie van de **Monitor** deelvenster. Selecteer de actiegroep die u wilt beheren:

* Toevoegen, bewerken of verwijderen van acties.
* De actiegroep verwijderen.

## <a name="action-specific-information"></a>Actie-specifieke informatie

> [!NOTE]
> Zie [abonnement Servicelimieten voor bewaking](https://docs.microsoft.com/azure/azure-subscription-service-limits#azure-monitor-limits) voor numerieke limieten op elk van de volgende items.  

### <a name="azure-app-push-notifications"></a>Azure-app Pushmeldingen te verzenden
U mogelijk een beperkt aantal Azure-app-acties in een actiegroep.

### <a name="email"></a>Email
E-mailberichten ontvangt van de volgende e-mailadressen. Zorg ervoor dat uw e-mail filteren op de juiste wijze is geconfigureerd
- azure-noreply@microsoft.com
- azureemail-noreply@microsoft.com
- alerts-noreply@mail.windowsazure.com

U mogelijk een beperkt aantal e-mailacties in een actiegroep. Zie de [snelheidsbeperking informatie](./../../azure-monitor/platform/alerts-rate-limiting.md) artikel.

### <a name="itsm"></a>ITSM
ITSM-actie vereist een ITSM-verbinding. Meer informatie over het maken van een [ITSM-verbinding](../../azure-monitor/platform/itsmc-overview.md).

U mogelijk een beperkt aantal ITSM-acties in een actiegroep. 

### <a name="logic-app"></a>Logische apps
U mogelijk een beperkt aantal acties van logische apps in een actiegroep.

### <a name="function"></a>Function
De functietoetsen voor functie-Apps die zijn geconfigureerd als acties worden gelezen via de API-functies, die momenteel v2 functie-apps vereist configureren van de app-instelling 'AzureWebJobsSecretStorageType' naar 'files'. Zie voor meer informatie, [wijzigingen in de Key Management in functies V2]( https://aka.ms/funcsecrets).

U mogelijk een beperkt aantal functie acties in een actiegroep.

### <a name="automation-runbook"></a>Automation-Runbook
Raadpleeg de [Servicelimieten van Azure-abonnement](../../azure-subscription-service-limits.md) voor beperkingen met betrekking tot de Runbook-nettoladingen.

U mogelijk een beperkt aantal Runbook-acties in een actiegroep. 

### <a name="sms"></a>Sms
Zie de [snelheidsbeperking informatie](./../../azure-monitor/platform/alerts-rate-limiting.md) en [SMS waarschuwen gedrag](../../azure-monitor/platform/alerts-sms-behavior.md) voor belangrijke aanvullende informatie.

U mogelijk een beperkt aantal SMS-acties in een actiegroep.  

### <a name="voice"></a>Spraak
Zie de [snelheidsbeperking informatie](./../../azure-monitor/platform/alerts-rate-limiting.md) artikel.

U mogelijk een beperkt aantal Voice-acties in een actiegroep.

### <a name="webhook"></a>Webhook
Webhooks worden opnieuw uitgevoerd met behulp van de volgende regels. De webhook-aanroep opnieuw is geprobeerd een maximum van 2 tijden wanneer de volgende HTTP-statuscodes worden weergegeven: 408, 429, 503, 504 of het HTTP-eindpunt reageert niet. De eerste poging vindt plaats na 10 seconden. De tweede nieuwe poging gebeurt na 100 seconden. Na twee fouten zal geen actiegroep het eindpunt aanroepen gedurende 30 minuten. 

Bron-IP-adresbereiken
 - 13.72.19.232
 - 13.106.57.181
 - 13.106.54.3
 - 13.106.54.19
 - 13.106.38.142
 - 13.106.38.148
 - 13.106.57.196
 - 52.244.68.117
 - 52.244.65.137
 - 52.183.31.0
 - 52.184.145.166
 - 51.4.138.199
 - 51.5.148.86
 - 51.5.149.19

U wordt aangeraden dat u een waarschuwing status van de Service, waarmee wordt gecontroleerd voor informatieve meldingen over de service actiegroepen configureren voor het ontvangen van updates over wijzigingen in deze IP-adressen.

U mogelijk een beperkt aantal webhookacties in een actiegroep.

#### <a name="secure-webhook"></a>Webhook beveiligen
**De Webhook beveiligen-functionaliteit is momenteel in Preview.**

Door de webhookactie actie groepen kunt u profiteren van Azure Active Directory voor het beveiligen van de verbinding tussen uw actiegroep en uw beveiligde web-API (webhook-eindpunt). De algemene werkstroom voor het gebruik van deze functionaliteit wordt hieronder beschreven. Zie voor een overzicht van Azure AD-toepassingen en service-principals, [overzicht van Microsoft identity-platform (v2.0)](https://docs.microsoft.com/azure/active-directory/develop/v2-overview).

1. Maak een Azure AD-toepassing voor uw beveiligde web-API. Zie https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview.
    - Uw beveiligde API te worden opgeroepen door een daemon-app configureren.
    
1. Actiegroepen gebruiken van uw Azure AD-toepassing inschakelen.

    > [!NOTE]
    > U moet een lid van de [beheerdersrol voor Azure AD-toepassing](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#available-roles) dit script wilt uitvoeren.
    
    - Wijzigen van de aanroep van de PowerShell-script-Connect-AzureAD gebruik van uw Azure AD-Tenant-ID.
    - Het PowerShell-script variabele $myAzureADApplicationObjectId voor het gebruik van de Object-ID van uw Azure AD-toepassing worden gewijzigd
    - Voer het gewijzigde script.
    
1. Configureer de actie groep Webhook-actie.
    - Kopieer de waarde $myApp.ObjectId van het script en voer deze in het veld ID van toepassingsobject in de definitie van de Webhook-actie.
    
    ![Beveiligen van webhookactie](./media/action-groups/action-groups-secure-webhook.png)

##### <a name="secure-webhook-powershell-script"></a>Webhook-PowerShell-Script beveiligen

```PowerShell
Connect-AzureAD -TenantId "<provide your Azure AD tenant ID here>"
    
# This is your Azure AD Application's ObjectId. 
$myAzureADApplicationObjectId = "<the Object Id of your Azure AD Application>"
    
# This is the Action Groups Azure AD AppId
$actionGroupsAppId = "461e8683-5575-4561-ac7f-899cc907d62a"
    
# This is the name of the new role we will add to your Azure AD Application
$actionGroupRoleName = "ActionGroupsSecureWebhook"
    
# Create an application role of given name and description
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}
    
# Get my Azure AD Application, it's roles and service principal
$myApp = Get-AzureADApplication -ObjectId $myAzureADApplicationObjectId
$myAppRoles = $myApp.AppRoles
$actionGroupsSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $actionGroupsAppId + "'")

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
    
# Create the role if it doesn't exist
if ($myAppRoles -match "ActionGroupsSecureWebhook")
{
    Write-Host "The Action Groups role is already defined.`n"
}
else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
    # Add our new role to the Azure AD Application
    $newRole = CreateAppRole -Name $actionGroupRoleName -Description "This is a role for Action Groups to join"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}
    
# Create the service principal if it doesn't exist
if ($actionGroupsSP -match "AzNS AAD Webhook")
{
    Write-Host "The Service principal is already defined.`n"
}
else
{
    # Create a service principal for the Action Groups Azure AD Application and add it to the role
    $actionGroupsSP = New-AzureADServicePrincipal -AppId $actionGroupsAppId
}
    
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $actionGroupsSP.ObjectId -PrincipalId $actionGroupsSP.ObjectId
    
Write-Host "My Azure AD Application ($myApp.ObjectId): " + $myApp.ObjectId
Write-Host "My Azure AD Application's Roles"
Write-Host $myApp.AppRoles
```


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [SMS waarschuwen gedrag](../../azure-monitor/platform/alerts-sms-behavior.md).  
* Krijg een [begrip van de activiteit log waarschuwing webhook-schema](../../azure-monitor/platform/activity-log-alerts-webhook.md).  
* Meer informatie over [ITSM-Connector](../../azure-monitor/platform/itsmc-overview.md)
* Meer informatie over [gelden enkele beperkingen](../../azure-monitor/platform/alerts-rate-limiting.md) op waarschuwingen.
* Krijgen een [overzicht van waarschuwingen voor activiteitenlogboeken](../../azure-monitor/platform/alerts-overview.md), en leer hoe u waarschuwingen ontvangt.  
* Meer informatie over het [waarschuwingen configureren wanneer er een melding van de health service wordt geplaatst](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).
