---
title: Toewijzing van Administrator rollen toewijzen en verwijderen met Azure PowerShell-Azure Active Directory | Microsoft Docs
description: Voor degenen die regel matig roltoewijzingen beheren, kunt u nu leden van een Azure AD-beheerdersrol beheren met Azure PowerShell.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: aa4bddf84720265afe361dff665f10ff8184f6f6
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706489"
---
# <a name="assign-azure-active-directory-admin-roles-using-powershell"></a>Azure Active Directory beheerdersrol toewijzen met behulp van Power shell

U kunt automatiseren hoe u rollen toewijst aan gebruikers accounts met behulp van Azure PowerShell. In dit artikel wordt gebruikgemaakt van de [Azure Active Directory module Power shell versie 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles) .

## <a name="prepare-powershell"></a>Power shell voorbereiden

Eerst moet u [de Azure AD Power shell-module downloaden](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="install-the-azure-ad-powershell-module"></a>De Azure AD PowerShell-module installeren

Als u de Azure AD Power shell-module wilt installeren, gebruikt u de volgende opdrachten:

```powershell
install-module azuread
import-module azuread
```

Als u wilt controleren of de module gereed is voor gebruik, gebruikt u de volgende opdracht:

```powershell
get-module azuread
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
```

U kunt nu beginnen met het gebruik van de cmdlets in de module. Raadpleeg de online-referentie documentatie voor [Azure Active Directory Power shell-versie 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles)voor een volledige beschrijving van de cmdlets in de Azure ad-module.

## <a name="permissions-required"></a>Vereiste machtigingen

Maak verbinding met uw Azure AD-Tenant met behulp van een algemeen beheerders account om rollen toe te wijzen of te verwijderen.

## <a name="assign-a-single-role"></a>Een afzonderlijke rol toewijzen

Als u een rol wilt toewijzen, moet u eerst de weergave naam en de naam van de rol die u toewijst verkrijgen. Wanneer u de weergave naam van het account en de naam van de rol hebt, gebruikt u de volgende cmdlets om de rol aan de gebruiker toe te wijzen.

``` PowerShell
# Fetch user to assign to role
$roleMember = Get-AzureADUser -ObjectId "username@contoso.com"

# Fetch User Account Administrator role instance
$role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}

# If role instance does not exist, instantiate it based on the role template
if ($role -eq $null) {
    # Instantiate an instance of the role template
    $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq 'User Account Administrator'}
    Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId

    # Fetch User Account Administrator role instance again
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}
}

# Add user to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser
```

## <a name="assign-a-role-to-a-service-principal"></a>Een rol toewijzen aan een Service-Principal

Voor beeld van het toewijzen van een service-principal aan een rol.

```powershell
# Fetch a service principal to assign to role
$roleMember = Get-AzureADServicePrincipal -ObjectId "00221b6f-4387-4f3f-aa85-34316ad7f956"
 
#Fetch list of all directory roles with object ID
Get-AzureADDirectoryRole
 
# Fetch a directory role by ID
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"
 
# Add user to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId
 
# Fetch the assignment for the role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADServicePrincipal
```

## <a name="multiple-role-assignments"></a>Meervoudige roltoewijzingen

Voor beelden van het toewijzen en verwijderen van meerdere rollen tegelijk.

```powershell
#File name
$fileName="<file path>" 
 
$input_Excel = New-Object -ComObject Excel.Application
$input_Workbook = $input_Excel.Workbooks.Open($fileName)
$input_Worksheet = $input_Workbook.Sheets.Item(1)
 
        #Count number of users who have to be assigned to role
$count = $input_Worksheet.UsedRange.Rows.Count
 
#Loop through each line of the csv file starting from line 2 (assuming first line is title)
for ($i=2; $i -le $count; $i++)
{
       #Fetch user display name
       $displayName = $input_Worksheet.Cells.Item($i,1).Text
       
       #Fetch role name
       $roleName = $input_Worksheet.Cells.Item($i,2).Text
 
       #Assign role
       Add-AzureADDirectoryRoleMember -ObjectId (Get-AzureADDirectoryRole | Where-Object DisplayName -eq $roleName).ObjectId -RefObjectId (Get-AzureADUser | Where-Object DisplayName -eq $displayName).ObjectId
}
 
#Remove multiple role assignments
for ($i=2; $i -le $count; $i++)
{
       $displayName = $input_Worksheet.Cells.Item($i,1).Text
       $roleName = $input_Worksheet.Cells.Item($i,2).Text
 
       Remove-AzureADDirectoryRoleMember -ObjectId (Get-AzureADDirectoryRole | Where-Object DisplayName -eq $roleName).ObjectId -MemberId (Get-AzureADUser | Where-Object DisplayName -eq $displayName).ObjectId
}
```

## <a name="remove-a-role-assignment"></a>Roltoewijzing verwijderen

In dit voor beeld wordt een roltoewijzing voor de opgegeven gebruiker verwijderd.

```powershell
# Fetch user to assign to role
$roleMember = Get-AzureADUser -ObjectId "username@contoso.com"

#Fetch list of all directory roles with object id
Get-AzureADDirectoryRole

# Fetch a directory role by id
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"

# Remove user from role
Remove-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -MemberId $roleMember.ObjectId 

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser

```

## <a name="next-steps"></a>Volgende stappen

* U kunt graag delen met ons op het [forum van Azure AD-beheerders](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Zie [beheerders rollen toewijzen](directory-assign-admin-roles.md)voor meer informatie over functies en de toewijzing van beheerdersrol.
* Zie voor standaard gebruikers machtigingen een [vergelijking van de standaard machtigingen voor gast-en gebruikers rechten](../fundamentals/users-default-permissions.md).
