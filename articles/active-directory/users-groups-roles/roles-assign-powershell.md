---
title: Toewijzen en beheerder rollen toewijzing met Azure PowerShell - Azure Active Directory verwijderen | Microsoft Docs
description: Voor diegenen die vaak roltoewijzingen beheren, kunt u nu leden van een Azure AD-beheerdersrol met Azure PowerShell beheren.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/15/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f6877c3e547d625cf58129a546dae798b37a24ae
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60469091"
---
# <a name="assign-azure-active-directory-admin-roles-using-powershell"></a>Met behulp van PowerShell van Azure Active Directory-beheerdersrollen toewijzen

U kunt automatiseren hoe u rollen toewijzen aan gebruikersaccounts met behulp van Azure PowerShell. In dit artikel wordt de [Azure Active Directory PowerShell versie 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles) module.

## <a name="prepare-powershell"></a>Voorbereiden van PowerShell

Eerst moet u [downloaden van de Azure AD PowerShell-module](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="install-the-azure-ad-powershell-module"></a>De Azure AD PowerShell-module installeren

Gebruik de volgende opdrachten voor het installeren van de Azure AD PowerShell-module:

```powershell
install-module azuread
import-module azuread
```

Om te controleren dat de module kan worden gebruikt, gebruik de volgende opdracht:

```powershell
get-module azuread
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
```

U kunt nu beginnen met de cmdlets in de module. Raadpleeg voor een volledige beschrijving van de cmdlets in de Azure AD-module, de online documentatie voor [Azure Active Directory PowerShell versie 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles).

## <a name="permissions-required"></a>Machtigingen die vereist zijn

Verbinding maken met uw Azure AD-tenant met behulp van een account voor globale beheerders voor het toewijzen of verwijderen van rollen.

## <a name="assign-a-single-role"></a>Een enkele rol toewijzen

Als u wilt een rol toewijzen, moet u eerst de weergavenaam en de naam van de rol die u toewijst verkrijgen. Wanneer u de weergavenaam van het account en de naam van de rol, moet u de volgende cmdlets gebruiken de rol toewijzen aan de gebruiker.

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

## <a name="assign-a-role-to-a-service-principal"></a>Een rol toewijzen aan een service-principal

Voorbeeld van een service-principal toewijzen aan een rol.

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

Voorbeelden van toe te wijzen en meerdere functies tegelijk verwijderen.

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

In dit voorbeeld wordt een roltoewijzing voor de opgegeven gebruiker verwijderd.

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

* U kunt delen met ons op de [forum van Azure AD-beheerdersrollen](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Zie voor meer informatie over de functies en de toewijzing van de rol beheerder [beheerdersrollen toewijzen](directory-assign-admin-roles.md).
* Zie voor gedetailleerde bevoegdheden voor standaard een [vergelijking van de standaard Gast en lid gebruikersmachtigingen](../fundamentals/users-default-permissions.md).
