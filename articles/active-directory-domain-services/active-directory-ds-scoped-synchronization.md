---
title: 'Azure Active Directory Domain Services: Binnen het bereik van synchronisatie | Microsoft Docs'
description: Scoped synchronisatie van Azure AD met uw beheerde domeinen configureren
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: ergreenl
ms.openlocfilehash: ac11244b87c87285722b4922da69530fab98c299
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58117605"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-your-managed-domain"></a>Scoped synchronisatie van Azure AD met uw beheerde domein configureren
In dit artikel leest u hoe het configureren van alleen specifieke gebruikersaccounts uit uw Azure AD-directory worden gesynchroniseerd met uw Azure AD Domain Services beheerde domein.


## <a name="group-based-scoped-synchronization"></a>Scoped synchronisatie op basis van een groep
Standaard worden alle gebruikers en groepen in uw Azure AD-directory gesynchroniseerd naar uw beheerde domein. Als er slechts een paar gebruikers het beheerde domein gebruikt, mag u alleen deze gebruikersaccounts gesynchroniseerd. Scoped synchronisatie op basis van een groep kunt u om dit te doen. Wanneer geconfigureerd, worden alleen gebruikersaccounts die tot de groepen die u hebt opgegeven bij het beheerde domein gesynchroniseerd.

De volgende tabel kunt u bepalen hoe u binnen het bereik synchronisatie:

| **Huidige status** | **Gewenste status** | **Vereiste configuratie** |
| --- | --- | --- |
| Uw bestaande beheerde domein is geconfigureerd voor synchronisatie van alle gebruikersaccounts en groepen. | U wilt synchroniseren uitsluitend gebruikersaccounts die tot specifieke groepen met uw beheerde domein behoren. | [Verwijder het bestaande beheerde domein](active-directory-ds-disable-aadds.md). Volg daarna de instructies in dit artikel voor het opnieuw maken binnen het bereik-synchronisatie is geconfigureerd. |
| U hebt geen een bestaande beheerde domein. | U wilt maken van een nieuwe beheerde domein en alleen de gebruikersaccounts die zijn die behoren tot specifieke groepen synchroniseren. | Volg de instructies in dit artikel voor het maken van een nieuwe beheerde domein scoped-synchronisatie is geconfigureerd. |
| Uw bestaande beheerde domein is geconfigureerd voor synchronisatie van alleen de accounts die bij bepaalde groepen horen. | U wilt wijzigen van de lijst met groepen waarvan de gebruikers moeten worden gesynchroniseerd met het domein beheren. | Volg de instructies in dit artikel om synchronisatie van binnen het bereik. |

> [!WARNING]
> **Wijzigen van het bereik van de synchronisatie zorgt ervoor dat uw beheerde domein te doorlopen opnieuw synchroniseren.**
> 
>  * Wanneer u het synchronisatie-bereik voor een beheerd domein wijzigt, wordt een volledige hersynchronisatie uitgevoerd.
>  * Objecten die niet langer in het beheerde domein vereist zijn worden verwijderd. Nieuwe objecten worden gemaakt in het beheerde domein.
>  * Hersynchronisatie kan lang duren om uit te voeren, afhankelijk van het aantal objecten (gebruikers, groepen en groepslidmaatschappen) in uw beheerde domein en uw Azure AD-directory. Opnieuw synchroniseren kan een paar dagen duren voor grote mappen met veel honderden of duizenden objecten.


## <a name="create-a-new-managed-domain-and-enable-group-based-scoped-synchronization-using-azure-portal"></a>Een nieuwe beheerde domein maken en op basis van een groep binnen het bereik synchronisatie met Azure portal inschakelen

1. Ga als volgt de [introductiehandleiding](active-directory-ds-getting-started.md) te maken van een beheerd domein.
2. Kies **binnen het bereik van** tijdens de synchronisatie-stijl-selectie in de wizard Azure AD Domain Services maken.

## <a name="create-a-new-managed-domain-and-enable-group-based-scoped-synchronization-using-powershell"></a>Een nieuwe beheerde domein maken en inschakelen van binnen het bereik synchronisatie op basis van een groep met behulp van PowerShell
PowerShell gebruiken voor het voltooien van deze reeks stappen. Raadpleeg de instructies voor het [inschakelen van Azure Active Directory Domain Services met behulp van PowerShell](active-directory-ds-enable-using-powershell.md). Een aantal stappen in dit artikel zijn enigszins gewijzigd voor het configureren van synchronisatie van binnen het bereik.

Voer de volgende stappen uit als u wilt configureren op basis van een groep binnen het bereik synchronisatie met uw beheerde domein:

1. De volgende taken uitvoeren:
   * [Taak 1: Installeer de vereiste PowerShell-modules](active-directory-ds-enable-using-powershell.md#task-1-install-the-required-powershell-modules).
   * [Taak 2: De vereiste service-principal maken in uw Azure AD-directory](active-directory-ds-enable-using-powershell.md#task-2-create-the-required-service-principal-in-your-azure-ad-directory).
   * [Taak 3: Maken en configureren van de groep 'AAD DC Administrators'](active-directory-ds-enable-using-powershell.md#task-3-create-and-configure-the-aad-dc-administrators-group).
   * [Taak 4: Registreer de resourceprovider van Azure AD Domain Services](active-directory-ds-enable-using-powershell.md#task-4-register-the-azure-ad-domain-services-resource-provider).
   * [Taak 5: Maak een resourcegroep](active-directory-ds-enable-using-powershell.md#task-5-create-a-resource-group).
   * [Taak 6: Maken en configureren van het virtuele netwerk](active-directory-ds-enable-using-powershell.md#task-6-create-and-configure-the-virtual-network).

2. Selecteer de groepen die u wilt synchroniseren en geef de weergavenaam van de groepen die u wilt synchroniseren met uw beheerde domein.

3. Sla de [script in de volgende sectie](active-directory-ds-scoped-synchronization.md#script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1) naar een bestand met de naam ```Select-GroupsToSync.ps1```. Voer het script, zoals hieronder:

   ```powershell
   .\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName2")
   ```

   > [!WARNING]
   > **Vergeet niet om op te nemen van de groep 'AAD DC Administrators'.**
   >
   > U moet de groep 'AAD DC Administrators' opnemen in de lijst van groepen die zijn geconfigureerd voor synchronisatie van binnen het bereik. Als u deze groep niet opgeeft, wordt het beheerde domein onbruikbaar worden.
   >

4. Nu het beheerde domein maken en inschakelen van binnen het bereik synchronisatie op basis van een groep voor het beheerde domein. De eigenschap ```"filteredSync" = "Enabled"``` in de ```Properties``` parameter. Bijvoorbeeld, Zie het volgende fragment van het script opgehaald uit [taak 7: Inrichten van het Azure AD Domain Services beheerde domein](active-directory-ds-enable-using-powershell.md#task-7-provision-the-azure-ad-domain-services-managed-domain).

   ```powershell
   $AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
   $ManagedDomainName = "contoso100.com"
   $ResourceGroupName = "ContosoAaddsRg"
   $VnetName = "DomainServicesVNet_WUS"
   $AzureLocation = "westus"

   # Enable Azure AD Domain Services for the directory.
   New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
   -Location $AzureLocation `
   -Properties @{"DomainName"=$ManagedDomainName; "filteredSync" = "Enabled"; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
   -ApiVersion 2017-06-01 -Force -Verbose
   ```

   > [!TIP]
   > Vergeet niet om op te nemen ```"filteredSync" = "Enabled"``` in de ```-Properties``` parameter, dus binnen het bereik synchronisatie is ingeschakeld voor het beheerde domein.


## <a name="script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1"></a>Script groepen moeten worden gesynchroniseerd met het beheerde domein (Select-GroupsToSync.ps1) selecteren
Sla het volgende script naar een bestand (```Select-GroupsToSync.ps1```). Dit script wordt geconfigureerd voor Azure AD Domain Services om te synchroniseren van de geselecteerde groepen aan het beheerde domein. Alle gebruikersaccounts die tot de opgegeven groepen worden aan het beheerde domein gesynchroniseerd.

```powershell
param (
    [Parameter(Position = 0)]
    [String[]]$groupsToAdd
)

Connect-AzureAD
$sp = Get-AzureADServicePrincipal -Filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
$role = $sp.AppRoles | where-object -FilterScript {$_.DisplayName -eq "User"}

Write-Output "`n****************************************************************************"

Write-Output "Total group-assignments need to be added: $($groupsToAdd.Count)"
$newGroupIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($groupName in $groupsToAdd)
{
    try
    {
        $group = Get-AzureADGroup -Filter "DisplayName eq '$groupName'"
        $newGroupIds.Add($group.ObjectId)

        Write-Output "Group-Name: $groupName, Id: $($group.ObjectId)"
    }
    catch
    {
        Write-Error "Failed to find group: $groupName. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

$currentAssignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId
Write-Output "Total current group-assignments: $($currentAssignments.Count), SP-ObjectId: $($sp.ObjectId)"

$currAssignedObjectIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($assignment in $currentAssignments)
{
    Write-Output "Assignment-ObjectId: $($assignment.PrincipalId)"

    if ($newGroupIds.Contains($assignment.PrincipalId) -eq $false)
    {
        Write-Output "This assignment is not needed anymore. Removing it! Assignment-ObjectId: $($assignment.PrincipalId)"
        Remove-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -AppRoleAssignmentId $assignment.ObjectId
    }
    else
    {
        $currAssignedObjectIds.Add($assignment.PrincipalId)
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

foreach ($id in $newGroupIds)
{
    try
    {
        if ($currAssignedObjectIds.Contains($id) -eq $false)
        {
            Write-Output "Adding new group-assignment. Role-Id: $($role.Id), Group-Object-Id: $id, ResourceId: $($sp.ObjectId)"
            New-AzureADGroupAppRoleAssignment -Id $role.Id -ObjectId $id -PrincipalId $id -ResourceId $sp.ObjectId
        }
        else
        {
            Write-Output "Group-ObjectId: $id is already assigned."
        }
    }
    catch
    {
        Write-Error "Exception occurred assigning Object-ID: $id. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
```


## <a name="modify-group-based-scoped-synchronization"></a>Scoped synchronisatie op basis van een groep wijzigen
Voer voor het wijzigen van de lijst met groepen waarvan de gebruikers moeten worden gesynchroniseerd met uw beheerde domein, de [PowerShell-script](active-directory-ds-scoped-synchronization.md#script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1) en geeft u de nieuwe lijst van groepen. Vergeet niet om altijd opgeven van de groep 'AAD DC Administrators' in deze lijst.

> [!WARNING]
> **Vergeet niet om op te nemen van de groep 'AAD DC Administrators'.**
>
> U moet de groep 'AAD DC Administrators' opnemen in de lijst van groepen die zijn geconfigureerd voor synchronisatie van binnen het bereik. Als u deze groep niet opgeeft, wordt het beheerde domein onbruikbaar worden.
>


## <a name="disable-group-based-scoped-synchronization"></a>Op basis van een groep binnen het bereik synchronisatie uitschakelen
Gebruik de volgende PowerShell-script om uit te schakelen op basis van een groep binnen het bereik van de synchronisatie voor uw beheerde domein:

```powershell
// Login to your Azure AD tenant
Login-AzAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

## <a name="next-steps"></a>Volgende stappen
* [Inzicht in synchronisatie in Azure AD Domain Services](active-directory-ds-synchronization.md)
* [Azure Active Directory Domain Services met behulp van PowerShell inschakelen](active-directory-ds-enable-using-powershell.md)
