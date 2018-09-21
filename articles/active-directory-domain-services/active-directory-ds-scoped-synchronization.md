---
title: 'Azure Active Directory Domain Services: Binnen het bereik van synchronisatie | Microsoft Docs'
description: Scoped synchronisatie van Azure AD met uw beheerde domeinen configureren
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2018
ms.author: maheshu
ms.openlocfilehash: 1f7e07880c22ec6975f93d8349343986716b9445
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/20/2018
ms.locfileid: "46499228"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-your-managed-domain"></a>Scoped synchronisatie van Azure AD met uw beheerde domein configureren
In dit artikel leest u hoe het configureren van alleen specifieke gebruikersaccounts uit uw Azure AD-directory worden gesynchroniseerd met uw Azure AD Domain Services beheerde domein.


## <a name="group-based-scoped-synchronization"></a>Scoped synchronisatie op basis van een groep
Standaard worden alle gebruikers en groepen in uw Azure AD-directory gesynchroniseerd naar uw beheerde domein. Als het beheerde domein wordt gebruikt door een paar gebruikers, is het wellicht beter om alleen de gebruikersaccounts aan het beheerde domein te synchroniseren. Scoped synchronisatie op basis van een groep kunt u om dit te doen. Wanneer geconfigureerd, worden alleen gebruikersaccounts die tot de groepen die u hebt opgegeven bij het beheerde domein gesynchroniseerd.


## <a name="get-started-install-the-required-powershell-modules"></a>Aan de slag: de vereiste PowerShell-modules installeren

### <a name="install-and-configure-azure-ad-powershell"></a>Azure AD PowerShell installeren en configureren
Volg de instructies in het artikel [installeren van de Azure AD PowerShell-module en maak verbinding met Azure AD](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="install-and-configure-azure-powershell"></a>Azure PowerShell installeren en configureren
Volg de instructies in het artikel [installeren van de Azure PowerShell-module en maak verbinding met uw Azure-abonnement](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).



## <a name="enable-group-based-scoped-synchronization"></a>Op basis van een groep binnen het bereik synchronisatie inschakelen
Voer de volgende stappen uit als u wilt configureren op basis van een groep binnen het bereik synchronisatie met uw beheerde domein:

1. Selecteer de groepen die u wilt synchroniseren en geef de weergavenaam van de groepen die u wilt synchroniseren met uw beheerde domein.

2. Sla het script in de volgende sectie voor een bestand met de naam ```Select-GroupsToSync.ps1```. Voer het script, zoals hieronder:

  ```powershell
  .\Select-GroupsToSync.ps1 -groupsToAdd @(“GroupName1”, “GroupName2”)
  ```

3. Schakel nu binnen het bereik synchronisatie op basis van een groep voor het beheerde domein.

  ```powershell
  // Login to your Azure AD tenant
  Login-AzureRmAccount

  // Retrieve the Azure AD Domain Services resource.
  $DomainServicesResource = Get-AzureRmResource -ResourceType "Microsoft.AAD/DomainServices"

  // Enable group-based scoped synchronization.
  $enableScopedSync = @{"filteredSync" = "Enabled"}

  Set-AzureRmResource -Id $DomainServicesResource.ResourceId -Properties $enableScopedSync
  ```

## <a name="disable-group-based-scoped-synchronization"></a>Op basis van een groep binnen het bereik synchronisatie uitschakelen
Gebruik de volgende PowerShell-script om uit te schakelen op basis van een groep binnen het bereik van de synchronisatie voor uw beheerde domein:

```powershell
// Login to your Azure AD tenant
Login-AzureRmAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzureRmResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

Set-AzureRmResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

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
        Write-Error "Exception occured assigning Object-ID: $id. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
```

## <a name="next-steps"></a>Volgende stappen
* [Inzicht in synchronisatie in Azure AD Domain Services](active-directory-ds-synchronization.md)
