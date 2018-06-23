---
title: Maken van aangepaste rollen met Azure PowerShell | Microsoft Docs
description: Informatie over het maken van aangepaste rollen voor op rollen gebaseerde toegangsbeheer (RBAC) met Azure PowerShell. Dit omvat de lijst, maken, bijwerken en verwijderen van aangepaste rollen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 6205198d3f9c4ec5bfa2311014cf9b90dcade6dc
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320518"
---
# <a name="create-custom-roles-using-azure-powershell"></a>Maken van aangepaste rollen met Azure PowerShell

Als de [ingebouwde rollen](built-in-roles.md) niet voldoen aan de specifieke behoeften van uw organisatie, kunt u uw eigen aangepaste rollen maken. In dit artikel wordt beschreven hoe maken en beheren van aangepaste rollen met Azure PowerShell.

## <a name="prerequisites"></a>Vereisten

Voor het aangepaste rollen maken, hebt u het volgende nodig:

- Machtigingen voor het maken van aangepaste rollen, zoals [eigenaar](built-in-roles.md#owner) of [beheerder voor gebruikerstoegang](built-in-roles.md#user-access-administrator)
- [Azure PowerShell](/powershell/azure/install-azurerm-ps) lokaal geïnstalleerd

## <a name="list-custom-roles"></a>Lijst met aangepaste rollen

Als de functies die beschikbaar voor toewijzing op een scope zijn wilt weergeven, gebruikt de [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) opdracht. Het volgende voorbeeld worden alle functies die beschikbaar voor toewijzing in het geselecteerde abonnement zijn.

```azurepowershell
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

```Example
Name                                              IsCustom
----                                              --------
Virtual Machine Operator                              True
AcrImageSigner                                       False
AcrQuarantineReader                                  False
AcrQuarantineWriter                                  False
API Management Service Contributor                   False
...
```

Het volgende voorbeeld worden alleen de aangepaste rollen die beschikbaar voor toewijzing in het geselecteerde abonnement zijn.

```azurepowershell
Get-AzureRmRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
```

```Example
Name                     IsCustom
----                     --------
Virtual Machine Operator     True
```

Als het geselecteerde abonnement niet in de `AssignableScopes` van de functie, de aangepaste rol die niet weergegeven.

## <a name="create-a-custom-role"></a>Een aangepaste beveiligingsrol maken

Gebruik voor het maken van een aangepaste rol de [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) opdracht. Er zijn twee methoden van de rol structureren met `PSRoleDefinition` object of een JSON-sjabloon. 

### <a name="get-operations-for-a-resource-provider"></a>Bewerkingen ophalen voor een resourceprovider

Wanneer u aangepaste rollen maken, is het belangrijk te weten van alle mogelijke bewerkingen van de resourceproviders.
U kunt de lijst weergeven met [resource provider operations](resource-provider-operations.md) of kunt u de [Get-AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) opdracht voor deze informatie.
Als u controleren van de beschikbare bewerkingen voor virtuele machines wilt, gebruikt u deze opdracht in:

```azurepowershell
Get-AzureRMProviderOperation <operation> | FT OperationName, Operation, Description -AutoSize
```

```Example
PS C:\> Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation, Description -AutoSize

OperationName                                  Operation                                                      Description
-------------                                  ---------                                                      -----------
Get Virtual Machine                            Microsoft.Compute/virtualMachines/read                         Get the propertie...
Create or Update Virtual Machine               Microsoft.Compute/virtualMachines/write                        Creates a new vir...
Delete Virtual Machine                         Microsoft.Compute/virtualMachines/delete                       Deletes the virtu...
Start Virtual Machine                          Microsoft.Compute/virtualMachines/start/action                 Starts the virtua...
...
```

### <a name="create-a-custom-role-with-the-psroledefinition-object"></a>Een aangepaste beveiligingsrol maken met het object PSRoleDefinition

Wanneer u een aangepaste beveiligingsrol maken met PowerShell, kunt u een van de [ingebouwde rollen](built-in-roles.md) als uitgangspunt nemen of u vanaf het begin starten kunt. Het eerste voorbeeld in deze sectie begint met een ingebouwde rol en past deze met meer machtigingen. Bewerk de kenmerken toevoegen de `Actions`, `NotActions`, of `AssignableScopes` die u wilt en klikt u vervolgens de wijzigingen opslaan als een nieuwe rol.

Het volgende voorbeeld begint met de [Virtual Machine Contributor](built-in-roles.md#virtual-machine-contributor) ingebouwde rol voor het maken van een aangepaste beveiligingsrol met de naam *virtuele Machine Operator*. De nieuwe rol verleent toegang tot alle leesbewerkingen van *Microsoft.Compute*, *Microsoft.Storage*, en *Microsoft.Network* resource providers en geeft toegang tot het starten , start opnieuw op en virtuele machines bewaken. De aangepaste rol kan worden gebruikt in twee abonnementen.

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/00000000-0000-0000-0000-000000000000")
$role.AssignableScopes.Add("/subscriptions/11111111-1111-1111-1111-111111111111")
New-AzureRmRoleDefinition -Role $role
```

Het volgende voorbeeld ziet u een andere manier om te maken de *virtuele Machine Operator* aangepaste rol. Deze methode begint met het maken van een nieuw `PSRoleDefinition` object. De actie-bewerkingen zijn opgegeven in de `perms` -variabele en is ingesteld op de `Actions` eigenschap. De `NotActions` eigenschap is ingesteld door het lezen van de `NotActions` van de [Virtual Machine Contributor](built-in-roles.md#virtual-machine-contributor) ingebouwde rol. Aangezien [Virtual Machine Contributor](built-in-roles.md#virtual-machine-contributor) geen `NotActions`, deze regel is niet vereist, maar er wordt weergegeven hoe gegevens kan worden opgehaald uit een andere rol.

```azurepowershell
$role = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
$role.Name = 'Virtual Machine Operator 2'
$role.Description = 'Can monitor and restart virtual machines.'
$role.IsCustom = $true
$perms = 'Microsoft.Storage/*/read','Microsoft.Network/*/read','Microsoft.Compute/*/read'
$perms += 'Microsoft.Compute/virtualMachines/start/action','Microsoft.Compute/virtualMachines/restart/action'
$perms += 'Microsoft.Authorization/*/read','Microsoft.Resources/subscriptions/resourceGroups/read'
$perms += 'Microsoft.Insights/alertRules/*','Microsoft.Support/*'
$role.Actions = $perms
$role.NotActions = (Get-AzureRmRoleDefinition -Name 'Virtual Machine Contributor').NotActions
$subs = '/subscriptions/00000000-0000-0000-0000-000000000000','/subscriptions/11111111-1111-1111-1111-111111111111'
$role.AssignableScopes = $subs
New-AzureRmRoleDefinition -Role $role
```

### <a name="create-a-custom-role-with-json-template"></a>Een aangepaste beveiligingsrol maken met JSON-sjabloon

Een JSON-sjabloon kan worden gebruikt als de definitie van de gegevensbron voor de aangepaste rol. Het volgende voorbeeld wordt een aangepaste rol waarmee de leestoegang voor opslagruimte en rekencapaciteit resources, toegang tot ondersteuning, en die rol worden toegevoegd aan twee abonnementen. Maak een nieuw bestand `C:\CustomRoles\customrole1.json` met het volgende voorbeeld. De Id moet worden ingesteld op `null` op het eerste rol maken als een nieuwe ID automatisch wordt gegenereerd. 

```json
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Als u wilt de rol toevoegen aan de abonnementen, voer de volgende PowerShell-opdracht:

```azurepowershell
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="update-a-custom-role"></a>Bijwerken van een aangepaste rol

Net als bij het maken van een aangepaste beveiligingsrol, kunt u een bestaande aangepaste rol met behulp van de `PSRoleDefinition` object of een JSON-sjabloon.

### <a name="update-a-custom-role-with-the-psroledefinition-object"></a>Een aangepaste beveiligingsrol aan het object PSRoleDefinition bijwerken

Voor het wijzigen van een aangepaste beveiligingsrol, gebruikt u eerst de [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) opdracht voor het ophalen van de functiedefinitie. Controleer vervolgens de gewenste wijzigingen aan de functiedefinitie. Gebruik tot slot de [Set-AzureRmRoleDefinition](/powershell/module/azurerm.resources/set-azurermroledefinition) opdracht voor het opslaan van de gewijzigde roldefinitie.

Het volgende voorbeeld wordt de `Microsoft.Insights/diagnosticSettings/*` bewerking is de *virtuele Machine Operator* aangepaste rol.

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}
```

Het volgende voorbeeld wordt een Azure-abonnement aan de toewijsbare bereiken van de *virtuele Machine Operator* aangepaste rol.

```azurepowershell
Get-AzureRmSubscription -SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> Get-AzureRmSubscription -SubscriptionName Production3

Name     : Production3
Id       : 22222222-2222-2222-2222-222222222222
TenantId : 99999999-9999-9999-9999-999999999999
State    : Enabled

PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111,
                   /subscriptions/22222222-2222-2222-2222-222222222222}
```

### <a name="update-a-custom-role-with-a-json-template"></a>Bijwerken van een aangepaste beveiligingsrol met een JSON-sjabloon

De vorige JSON-sjabloon gebruikt, kunt u eenvoudig wijzigen een bestaande aangepaste rol wilt toevoegen of verwijderen van acties. Het JSON-sjabloon bijwerken en toevoegen van de gelezen actie voor netwerken, zoals wordt weergegeven in het volgende voorbeeld. De definities die worden vermeld in de sjabloon zijn niet cumulatief toegepast op de definitie van een bestaande, wat betekent dat de rol wordt weergegeven zoals u in de sjabloon opgeven. Ook moet u het veld Id bijwerken met de ID van de rol. Als u niet zeker weet wat deze waarde is, kunt u de [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) cmdlet deze gegevens worden opgehaald.

```json
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Voer de volgende PowerShell-opdracht voor het bijwerken van de bestaande rol:

```azurepowershell
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen

U een aangepaste rol verwijderen met de [Remove-AzureRmRoleDefinition](/powershell/module/azurerm.resources/remove-azurermroledefinition) opdracht.

Het volgende voorbeeld verwijdert u de *virtuele Machine Operator* aangepaste rol.

```azurepowershell
Get-AzureRmRoleDefinition "Virtual Machine Operator"
Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator"

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}

PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition

Confirm
Are you sure you want to remove role definition with name 'Virtual Machine Operator'.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Een aangepaste beveiligingsrol met Azure PowerShell maken](tutorial-custom-role-powershell.md)
- [Aangepaste rollen in Azure](custom-roles.md)
- [Azure Resource Manager resource provider-bewerkingen](resource-provider-operations.md)
