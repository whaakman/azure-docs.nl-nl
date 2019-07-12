---
title: Virtuele Windows-bureaublad-tenant en host-groep maken - Azure
description: Hoe u problemen op te lossen van de tenant en host pool problemen tijdens de installatie van een omgeving met virtuele Windows-bureaublad tenants.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.author: v-chjenk
ms.openlocfilehash: 7ec4e0ffd87c0ef73a551416d8a8cc672f095483
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786736"
---
# <a name="tenant-and-host-pool-creation"></a>Tenants en hostpools maken

In dit artikel bevat informatie over problemen tijdens de eerste installatie van de virtuele Windows-bureaublad-tenant en de bijbehorende sessie host pool-infrastructuur.

## <a name="provide-feedback"></a>Feedback geven

We zijn niet op dit moment kwesties duurt zolang virtuele Windows-bureaublad in preview. Ga naar de [Windows virtuele bureaublad Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) bespreken van de virtuele Windows-bureaublad-service met het productteam en actieve communityleden.

## <a name="acquiring-the-windows-10-enterprise-multi-session-image"></a>De installatiekopie van het Windows 10 Enterprise-meerdere sessie ophalen

Voor het gebruik van de installatiekopie van het Windows 10 Enterprise-meerdere sessies, gaat u naar de Azure Marketplace, selecteer **aan de slag** > **Microsoft Windows 10** > en [Windows 10 Enterprise voor Virtuele bureaubladen Preview-versie, versie 1809](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsdesktop.windows-10?tab=PlansAndPrice).

![Een schermafbeelding van het selecteren van Windows 10 Enterprise voor de virtuele bureaubladen Preview-versie 1809.](media/AzureMarketPlace.png)

## <a name="creating-windows-virtual-desktop-tenant"></a>Het maken van virtuele Windows-bureaublad-tenant

In deze sectie bevat informatie over potentiële problemen bij het maken van de tenant virtuele Windows-bureaublad.

### <a name="error-the-user-isnt-authorized-to-query-the-management-service"></a>Fout: De gebruiker is niet geautoriseerd om op te vragen van de management-service

![Schermafbeelding van de PowerShell-venster waarin een gebruiker is niet geautoriseerd om op te vragen van de management-service.](media/UserNotAuthorizedNewTenant.png)

Voorbeeld van onbewerkte fout:

```Error
   New-RdsTenant : User isn't authorized to query the management service.
   ActivityId: ad604c3a-85c6-4b41-9b81-5138162e5559
   Powershell commands to diagnose the failure:
   Get-RdsDiagnosticActivities -ActivityId ad604c3a-85c6-4b41-9b81-5138162e5559
   At line:1 char:1
   + New-RdsTenant -Name "testDesktopTenant" -AadTenantId "01234567-89ab-c ...
   + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
       + CategoryInfo          : FromStdErr: (Microsoft.RDInf...nt.NewRdsTenant:NewRdsTenant) [New-RdsTenant], RdsPowerSh
      ellException
       + FullyQualifiedErrorId : UnauthorizedAccess,Microsoft.RDInfra.RDPowershell.Tenant.NewRdsTenant
```

**Oorzaak:** De TenantCreator-rol in Azure Active Directory nog niet is toegewezen aan de gebruiker die aangemeld.

**Fix:** Volg de instructies in [de toepassingsrol TenantCreator toewijzen aan een gebruiker in uw Azure Active Directory-tenant](https://docs.microsoft.com/azure/virtual-desktop/tenant-setup-azure-active-directory#assign-the-tenantcreator-application-role-to-a-user-in-your-azure-active-directory-tenant). Nadat u de instructies te volgen, hebt u een gebruiker die is toegewezen aan de rol TenantCreator.

![Schermafbeelding van TenantCreator-rol is toegewezen.](media/TenantCreatorRoleAssigned.png)

## <a name="creating-windows-virtual-desktop-session-host-vms"></a>Het maken van virtuele Windows-bureaublad-sessie hosten van virtuele machines

Sessiehost virtuele machines kan op verschillende manieren worden gemaakt, maar alleen ondersteuning voor Remote Desktop Services/Windows virtueel bureaublad teams problemen met betrekking tot de Azure Resource Manager-sjabloon Inrichtingsstatus van VM's. De Azure Resource Manager-sjabloon is beschikbaar in [Azure Marketplace](https://azuremarketplace.microsoft.com/) en [GitHub](https://github.com/).

## <a name="issues-using-windows-virtual-desktop--provision-a-host-pool-azure-marketplace-offering"></a>Problemen met behulp van Windows virtueel bureaublad: inrichten van een host groep Azure Marketplace-aanbiedingen

Het virtuele bureaublad van Windows: inrichten van een sjabloon van de groep host is beschikbaar via Azure Marketplace.

### <a name="error-when-using-the-link-from-github-the-message-create-a-free-account-appears"></a>Fout: Wanneer u de koppeling vanuit GitHub, het bericht 'Maak een gratis account' wordt weergegeven

![Schermafbeelding van een gratis account maken.](media/be615904ace9832754f0669de28abd94.png)

**1 oorzaak:** Er actieve abonnementen in het account dat wordt gebruikt om aan te melden bij Azure of het gebruikte account geen machtigingen om de abonnementen weer te geven.

**Fix 1:** Meld u aan met een account dat Inzender-toegang (minimaal) tot het abonnement heeft waarin sessiehost VM's gaat worden geïmplementeerd.

**2 oorzaak:** Het gebruikte abonnement maakt deel uit van een Microsoft Cloud Service Provider (CSP)-tenant.

**Fix 2:** Ga naar de GitHub-locatie voor **maken en inrichten nieuwe virtuele Windows-bureaublad host pool** en volg deze instructies:

1. Met de rechtermuisknop op **implementeren in Azure** en selecteer **adres van de koppeling kopiëren**.
2. Open **Kladblok** en plak de koppeling.
3. Plaats de naam van de CSP end klant tenant voordat u het teken #.
4. Open de nieuwe koppeling in een browser en de Azure portal wordt de sjabloon laden.

    ```Example
    Example: https://portal.azure.com/<CSP end customer tenant name>
    #create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%
    2FRDS-Templates%2Fmaster%2Fwvd-templates%2FCreate%20and%20provision%20WVD%20host%20pool%2FmainTemplate.json
    ```

## <a name="azure-resource-manager-template-and-powershell-desired-state-configuration-dsc-errors"></a>Azure Resource Manager-sjabloon en fouten van PowerShell Desired State Configuration (DSC)

Volg deze instructies om problemen met mislukte implementaties van Azure Resource Manager-sjablonen en PowerShell DSC oplossen.

1. Bekijk fouten in de implementatie met [implementatiebewerkingen met Azure Resource Manager weergeven](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).
2. Als er geen fouten in de implementatie, Bekijk fouten in de activiteit logboek met [activiteitenlogboeken om te controleren van acties op resources bekijken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
3. Nadat de fout wordt geïdentificeerd, gebruikt u het foutbericht en de resources in [veelvoorkomende problemen oplossen Azure-implementatie met Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-common-deployment-errors) om het probleem te verhelpen.
4. Verwijder alle resources die zijn gemaakt tijdens de vorige implementatie en probeer het opnieuw implementeren van de sjabloon opnieuw.

### <a name="error-your-deployment-failedhostnamejoindomain"></a>Fout: Uw implementatie is mislukt... <hostname> /JoinDomain

![De schermafbeelding implementatie is mislukt.](media/e72df4d5c05d390620e07f0d7328d50f.png)

Voorbeeld van onbewerkte fout:

```Error
 {"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. 
 Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\": \"Failed\",\r\n \"error\":
 {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.
 \",\r\n \"details\": [\r\n {\r\n \"code\": \"VMExtensionProvisioningError\",\r\n \"message\": \"VM has reported a failure when processing
 extension 'joindomain'. Error message: \\\"Exception(s) occurred while joining Domain 'diamondsg.onmicrosoft.com'\\\".\"\r\n }\r\n ]\r\n }\r\n}"}]}
```

**1 oorzaak:** Referenties die zijn opgegeven voor virtuele machines toevoegen aan het domein zijn onjuist.

**Fix 1:** Zie de fout 'Ongeldige referenties' voor VM's niet zijn gekoppeld aan het domein in [Session host-VM-configuratie](troubleshoot-vm-configuration.md).

**2 oorzaak:** De domeinnaam van het niet wordt opgelost.

**Fix 2:** Zie de fout 'de domeinnaam niet oplossen' voor VM's niet zijn gekoppeld aan het domein in [Session host-VM-configuratie](troubleshoot-vm-configuration.md).


### <a name="error-your-deployment-failedunauthorized"></a>Fout: Uw implementatie failed...\Unauthorized

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Unauthorized","message":"{\r\n \"Code\": \"Unauthorized\",\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\",\r\n \"Target\": null,\r\n \"Details\": [\r\n {\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\"\r\n },\r\n {\r\n \"Code\": \"Unauthorized\"\r\n },\r\n {\r\n \"ErrorEntity\": {\r\n \"ExtendedCode\": \"52020\",\r\n \"MessageTemplate\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\",\r\n \"Parameters\": [\r\n \"default\"\r\n ],\r\n \"Code\": \"Unauthorized\",\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\"\r\n }\r\n }\r\n ],\r\n \"Innererror\": null\r\n}"}]}
```

**Oorzaak:** Het abonnement dat u gebruikt, is een type dat geen toegang de vereiste onderdelen in de regio waar de klant probeert tot te implementeren. Abonnementen voor MSDN, gratis of onderwijs kunnen bijvoorbeeld deze fout weergegeven.

**Fix:** Wijzig uw abonnementstype of de regio die toegang hebben tot de vereiste onderdelen.

### <a name="error-vmextensionprovisioningerror"></a>Fout: VMExtensionProvisioningError

![Schermafbeelding van de implementatie is mislukt met afgesloten Inrichtingsstatus is mislukt.](media/7aaf15615309c18a984673be73ac969a.png)

**1 oorzaak:** Tijdelijke fout met de virtuele Windows-bureaublad-omgeving.

**2 oorzaak:** Tijdelijke fout bij verbinding.

**Fix:** Controleer of virtuele Windows-bureaublad-omgeving is in orde door aanmelding bij met behulp van PowerShell. Voltooien van de registratie van de virtuele machine handmatig in [een host-pool maken met PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).

### <a name="error-the-admin-username-specified-isnt-allowed"></a>Fout: De opgegeven Beheerdersgebruikersnaam is niet toegestaan

![Schermafbeelding van de implementatie is mislukt in die een beheerder die is opgegeven is niet toegestaan.](media/f2b3d3700e9517463ef88fa41875bac9.png)

Voorbeeld van onbewerkte fout:

```Error
 { "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostDesktop/providers/Microsoft.
  Resources/deployments/vmCreation-linkedTemplate/operations/EXAMPLE", "operationId": "EXAMPLE", "properties": { "provisioningOperation":
 "Create", "provisioningState": "Failed", "timestamp": "2019-01-29T20:53:18.904917Z", "duration": "PT3.0574505S", "trackingId":
 "1f460af8-34dd-4c03-9359-9ab249a1a005", "statusCode": "BadRequest", "statusMessage": { "error": { "code": "InvalidParameter", "message":
 "The Admin Username specified is not allowed.", "target": "adminUsername" } }, "targetResource": { "id": "/subscriptions/EXAMPLE
 /resourceGroups/demoHostDesktop/providers/Microsoft.Compute/virtualMachines/demo", "resourceType": "Microsoft.Compute/virtualMachines", "resourceName": "demo" } }}
```

**Oorzaak:** Wachtwoord dat is opgegeven, bevat niet-toegestane subtekenreeksen (admin, administrator, root).

**Fix:** Update van de gebruikersnaam of gebruik van verschillende gebruikers.

### <a name="error-vm-has-reported-a-failure-when-processing-extension"></a>Fout: Virtuele machine heeft een fout gerapporteerd bij het verwerken van extensie

![Schermopname van de resourcebewerking is voltooid met afgesloten Inrichtingsstatus in uw implementatie is mislukt.](media/49c4a1836a55d91cd65125cf227f411f.png)

Voorbeeld van onbewerkte fout:

```Error
{ "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostD/providers/Microsoft.Resources/deployments/
 rds.wvd-hostpool4-preview-20190129132410/operations/5A0757AC9E7205D2", "operationId": "5A0757AC9E7205D2", "properties":
 { "provisioningOperation": "Create", "provisioningState": "Failed", "timestamp": "2019-01-29T21:43:05.1416423Z",
 "duration": "PT7M56.8150879S", "trackingId": "43c4f71f-557c-4abd-80c3-01f545375455", "statusCode": "Conflict",
 "statusMessage": { "status": "Failed", "error": { "code": "ResourceDeploymentFailure", "message":
 "The resource operation completed with terminal provisioning state 'Failed'.", "details": [ { "code":
 "VMExtensionProvisioningError", "message": "VM has reported a failure when processing extension 'dscextension'. 
 Error message: \"DSC Configuration 'SessionHost' completed with error(s). Following are the first few: 
 PowerShell DSC resource MSFT_ScriptResource failed to execute Set-TargetResource functionality with error message: 
 One or more errors occurred. The SendConfigurationApply function did not succeed.\"." } ] } }, "targetResource": 
 { "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostD/providers/Microsoft. 
 Compute/virtualMachines/desktop-1/extensions/dscextension",
 "resourceType": "Microsoft.Compute/virtualMachines/extensions", "resourceName": "desktop-1/dscextension" } }}
```

**Oorzaak:** PowerShell DSC-extensie kan geen om beheerderstoegang te krijgen op de virtuele machine.

**Fix:** Controleer of gebruikersnaam en wachtwoord beheerderstoegang hebben op de virtuele machine en de Azure Resource Manager-sjabloon opnieuw uitvoeren.

### <a name="error-deploymentfailed--powershell-dsc-configuration-firstsessionhost-completed-with-errors"></a>Fout: Implementatie mislukt: PowerShell DSC-configuratie is voltooid met fout(en) FirstSessionHost

![Schermafbeelding van de implementatie mislukken met PowerShell DSC-configuratie is voltooid met fout(en) FirstSessionHost.](media/64870370bcbe1286906f34cf0a8646ab.png)

Voorbeeld van onbewerkte fout:

```Error
{
    "code": "DeploymentFailed",
   "message": "At least one resource deployment operation failed. Please list 
 deployment operations for details. 4 Please see https://aka.ms/arm-debug for usage details.",
 "details": [
         { "code": "Conflict",  
         "message": "{\r\n \"status\": \"Failed\",\r\n \"error\": {\r\n \"code\":
         \"ResourceDeploymentFailure\",\r\n \"message\": \"The resource
         operation completed with terminal provisioning state 'Failed'.\",\r\n
         \"details\": [\r\n {\r\n \"code\":
        \"VMExtensionProvisioningError\",\r\n \"message\": \"VM has
              reported a failure when processing extension 'dscextension'.
              Error message: \\\"DSC Configuration 'FirstSessionHost'
              completed with error(s). Following are the first few:
              PowerShell DSC resource MSFT ScriptResource failed to
              execute Set-TargetResource functionality with error message:
              One or more errors occurred. The SendConfigurationApply
              function did not succeed.\\\".\"\r\n }\r\n ]\r\n }\r\n}"  }

```

**Oorzaak:** PowerShell DSC-extensie kan geen om beheerderstoegang te krijgen op de virtuele machine.

**Fix:** Controleer of de gebruikersnaam en wachtwoord opgegeven beheerderstoegang hebben op de virtuele machine en voer de Azure Resource Manager-sjabloon opnieuw uit.

### <a name="error-deploymentfailed--invalidresourcereference"></a>Fout: Implementatie mislukt – InvalidResourceReference

Voorbeeld van onbewerkte fout:

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation
failed. Please list deployment operations for details. Please see https://aka.ms/arm-
debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\":
\"Failed\",\r\n \"error\": {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n
\"message\": \"The resource operation completed with terminal provisioning state
'Failed'.\",\r\n \"details\": [\r\n {\r\n \"code\": \"DeploymentFailed\",\r\n
\"message\": \"At least one resource deployment operation failed. Please list
deployment operations for details. Please see https://aka.ms/arm-debug for usage
details.\",\r\n \"details\": [\r\n {\r\n \"code\": \"BadRequest\",\r\n \"message\":
\"{\\r\\n \\\"error\\\": {\\r\\n \\\"code\\\": \\\"InvalidResourceReference\\\",\\r\\n
\\\"message\\\": \\\"Resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/virtualNetworks/wvd-vnet/subnets/default
referenced by resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/networkInterfaces/erd. Please make sure that
the referenced resource exists, and that both resources are in the same
region.\\\",\\r\\n\\\"details\\\": []\\r\\n }\\r\\n}\"\r\n }\r\n ]\r\n }\r\n ]\r\n }\r\n}"}]}
```

**Oorzaak:** Onderdeel van de naam van de resourcegroep wordt gebruikt voor bepaalde resources die worden gemaakt door de sjabloon. Vanwege de naam die overeenkomt met bestaande resources, kan de sjabloon een bestaande resource selecteren uit een andere groep.

**Fix:** Bij het uitvoeren van de Azure Resource Manager-sjabloon voor het implementeren van virtuele machines-sessiehost, zodat u de eerste twee tekens uniek zijn voor uw abonnement op de Resourcegroepnaam.

### <a name="error-deploymentfailed--invalidresourcereference"></a>Fout: Implementatie mislukt – InvalidResourceReference

Voorbeeld van onbewerkte fout:

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation
failed. Please list deployment operations for details. Please see https://aka.ms/arm-
debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\":
\"Failed\",\r\n \"error\": {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n
\"message\": \"The resource operation completed with terminal provisioning state
'Failed'.\",\r\n \"details\": [\r\n {\r\n \"code\": \"DeploymentFailed\",\r\n
\"message\": \"At least one resource deployment operation failed. Please list
deployment operations for details. Please see https://aka.ms/arm-debug for usage
details.\",\r\n \"details\": [\r\n {\r\n \"code\": \"BadRequest\",\r\n \"message\":
\"{\\r\\n \\\"error\\\": {\\r\\n \\\"code\\\": \\\"InvalidResourceReference\\\",\\r\\n
\\\"message\\\": \\\"Resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/virtualNetworks/wvd-vnet/subnets/default
referenced by resource /subscriptions/EXAMPLE/resourceGroups/DEMO/providers/Microsoft.Network/networkInterfaces
/EXAMPLE was not found. Please make sure that the referenced resource exists, and that both
resources are in the same region.\\\",\\r\\n \\\"details\\\": []\\r\\n }\\r\\n}\"\r\n
}\r\n ]\r\n }\r\n ]\r\n }\r\n\
```

**Oorzaak:** Deze fout is omdat de NIC die zijn gemaakt met de Azure Resource Manager-sjabloon dezelfde naam als een andere NIC al in het VNET heeft.

**Fix:** Gebruik een andere host-voorvoegsel.

### <a name="error-deploymentfailed--error-downloading"></a>Fout: Implementatie mislukt: fout bij het downloaden

Voorbeeld van onbewerkte fout:

```Error
\\\"The DSC Extension failed to execute: Error downloading
https://catalogartifact.azureedge.net/publicartifacts/rds.wvd-hostpool-3-preview-
2dec7a4d-006c-4cc0-965a-02bbe438d6ff-private-preview-
1/Artifacts/DSC/Configuration.zip after 29 attempts: The remote name could not be
resolved: 'catalogartifact.azureedge.net'.\\nMore information about the failure can
be found in the logs located under
'C:\\\\WindowsAzure\\\\Logs\\\\Plugins\\\\Microsoft.Powershell.DSC\\\\2.77.0.0' on
the VM.\\\"
```

**Oorzaak:** Deze fout wordt veroorzaakt door een statische route, firewallregel of NSG blokkeert het downloaden van het zip-bestand dat is gekoppeld aan de Azure Resource Manager-sjabloon.

**Fix:** Verwijder statische route, een firewallregel of NSG blokkeren. (Optioneel) het json-bestand van de Azure Resource Manager-sjabloon in een teksteditor te openen, nemen de koppeling naar het zip-bestand en downloaden van de resource met een toegestane locatie.

### <a name="error-the-user-isnt-authorized-to-query-the-management-service"></a>Fout: De gebruiker is niet geautoriseerd om op te vragen van de management-service

Voorbeeld van onbewerkte fout:

```Error
"response": { "content": { "startTime": "2019-04-01T17:45:33.3454563+00:00", "endTime": "2019-04-01T17:48:52.4392099+00:00", 
"status": "Failed", "error": { "code": "VMExtensionProvisioningError", "message": "VM has reported a failure when processing 
extension 'dscextension'. Error message: \"DSC Configuration 'FirstSessionHost' completed with error(s). 
Following are the first few: PowerShell DSC resource MSFT_ScriptResource failed to execute Set-TargetResource
 functionality with error message: User is not authorized to query the management service.
\nActivityId: 1b4f2b37-59e9-411e-9d95-4f7ccd481233\nPowershell commands to diagnose the failure:
\nGet-RdsDiagnosticActivities -ActivityId 1b4f2b37-59e9-411e-9d95-4f7ccd481233\n 
The SendConfigurationApply function did not succeed.\"." }, "name": "2c3272ec-d25b-47e5-8d70-a7493e9dc473" } } }}
```

**Oorzaak:** De opgegeven virtuele Windows-bureaublad-tenantbeheerder beschikt niet over een geldige roltoewijzing.

**Fix:** De gebruiker die heeft gemaakt van de virtuele Windows-bureaublad-tenant moet zich aanmelden bij Windows virtuele bureaublad PowerShell en een roltoewijzing voor de poging tot gebruiker toewijzen. Als u de GitHub Azure Resource Manager-Sjabloonparameters uitvoert, volgt u deze instructies met behulp van PowerShell-opdrachten:

```PowerShell
Add-RdsAccount -DeploymentUrl “https://rdbroker.wvd.microsoft.com”
New-RdsRoleAssignment -TenantName <Windows Virtual Desktop tenant name> -RoleDefinitionName “RDS Contributor” -SignInName <UPN>
```

### <a name="error-user-requires-azure-multi-factor-authentication-mfa"></a>Fout: Azure multi-factor Authentication (MFA) is vereist

![Schermafbeelding van de implementatie is mislukt vanwege een gebrek van multi-factor Authentication (MFA)](media/MFARequiredError.png)

Voorbeeld van onbewerkte fout:

```Error
"message": "{\r\n  \"status\": \"Failed\",\r\n  \"error\": {\r\n    \"code\": \"ResourceDeploymentFailure\",\r\n    \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.\",\r\n    \"details\": [\r\n      {\r\n        \"code\": \"VMExtensionProvisioningError\",\r\n        \"message\": \"VM has reported a failure when processing extension 'dscextension'. Error message: \\\"DSC Configuration 'FirstSessionHost' completed with error(s). Following are the first few: PowerShell DSC resource MSFT_ScriptResource  failed to execute Set-TargetResource functionality with error message: One or more errors occurred.  The SendConfigurationApply function did not succeed.\\\".\"\r\n      }\r\n    ]\r\n  }\r\n}"
```

**Oorzaak:** De opgegeven virtuele Windows-bureaublad-tenantbeheer vereist dat Azure multi-factor Authentication (MFA) aan te melden.

**Fix:** Een service-principal maken en deze een rol toewijzen voor uw tenant virtuele Windows-bureaublad met de volgende stappen in [zelfstudie: Service-principals en roltoewijzingen maken met PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-service-principal-role-powershell). Nadat u hebt gecontroleerd of u met virtuele Windows-bureaublad met de service-principal aanmelden kunt, opnieuw de Azure Marketplace-aanbieding of de GitHub Azure Resource Manager-sjabloon, afhankelijk van welke methode u gebruikt. Volg de onderstaande instructies om in te voeren van de juiste parameters voor de methode.

Als u de Azure Marketplace-aanbieding uitvoert, waarden opgeven voor de volgende parameters voor de virtuele Windows-bureaublad correct verificatie:

- Virtuele Windows-bureaublad-tenant de eigenaar van de extern bureaublad-services: Service-principal
- Toepassings-ID: De toepassings-id van de nieuwe service-principal die u hebt gemaakt.
- Wachtwoord/Bevestig het wachtwoord: Het wachtwoord geheim dat u hebt gegenereerd voor de service-principal
- Azure AD-Tenant-ID: De Azure AD-Tenant-ID van de service-principal die u hebt gemaakt.

Als u de GitHub Azure Resource Manager-sjabloon, waarden opgeven voor de volgende parameters voor de virtuele Windows-bureaublad correct verificatie:

- Tenant-beheerder UPN (user Principal name) of toepassings-ID: De toepassings-id van de nieuwe service-principal die u hebt gemaakt.
- Tenant Administrator-wachtwoord: Het wachtwoord geheim dat u hebt gegenereerd voor de service-principal
- IsServicePrincipal: **true**
- AadTenantId: De Azure AD-Tenant-ID van de service-principal die u hebt gemaakt.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht over het oplossen van virtuele Windows-bureaublad en de sporen escalatie [overzicht, feedback en ondersteuning voor probleemoplossing](troubleshoot-set-up-overview.md).
- Zie voor het oplossen van problemen tijdens het configureren van een virtuele machine (VM) in virtuele Windows-bureaublad, [Session host Virtuele-machineconfiguratie](troubleshoot-vm-configuration.md).
- Zie voor het oplossen van problemen met virtuele Windows-bureaublad-clientverbindingen, [verbindingen met extern bureaublad-client](troubleshoot-client-connection.md).
- Zie voor het oplossen van problemen bij het gebruik van PowerShell met virtuele Windows-bureaublad, [Windows virtuele bureaublad PowerShell](troubleshoot-powershell.md).
- Zie voor meer informatie over de Preview-service, [Windows Desktop Preview-omgeving](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Als u wilt u een zelfstudie voor problemen oplossen, Zie [zelfstudie: Problemen met sjabloonimplementaties van Resource Manager-oplossen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Zie voor meer informatie over het controleren van acties, [bewerkingen controleren met Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Zie voor meer informatie over acties voor het bepalen van de fouten tijdens de implementatie, [implementatiebewerkingen bekijken](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).