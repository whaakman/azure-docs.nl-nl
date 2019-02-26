---
title: Aangepaste rollen voor Azure-resources met behulp van Azure PowerShell maken | Microsoft Docs
description: Informatie over het maken van aangepaste rollen met op rollen gebaseerd toegangsbeheer (RBAC) voor Azure-resources met behulp van Azure PowerShell. Dit omvat het weergeven, maken, bijwerken en verwijderen van aangepaste rollen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: ad1185cab2b2bd2d0fea10f21b7859fd9ab1339f
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56807606"
---
# <a name="create-custom-roles-for-azure-resources-using-azure-powershell"></a>Aangepaste rollen maken voor Azure-resources met behulp van Azure PowerShell

Als de [ingebouwde rollen voor Azure-resources](built-in-roles.md) niet voldoen aan de specifieke behoeften van uw organisatie, kunt u uw eigen aangepaste rollen maken. In dit artikel wordt beschreven hoe u maken en beheren van aangepaste rollen met behulp van Azure PowerShell.

Zie voor een stapsgewijze zelfstudie voor het maken van een aangepaste rol [zelfstudie: Maken van een aangepaste rol voor Azure-resources met behulp van Azure PowerShell](tutorial-custom-role-powershell.md).

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Voor het maken van aangepaste rollen, hebt u het volgende nodig:

- Machtigingen voor het maken van aangepaste rollen, zoals [Eigenaar](built-in-roles.md#owner) of [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator)
- [Azure Cloudshell](../cloud-shell/overview.md) of [Azure PowerShell](/powershell/azure/install-az-ps)

## <a name="list-custom-roles"></a>Aangepaste rollen opvragen

Als u de functies die beschikbaar voor toewijzing aan een bereik zijn, gebruikt u de [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) opdracht. Het volgende voorbeeld worden alle functies die beschikbaar voor toewijzing in het geselecteerde abonnement zijn.

```azurepowershell
Get-AzRoleDefinition | FT Name, IsCustom
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

Het volgende voorbeeld worden alleen de aangepaste functies die beschikbaar voor toewijzing in het geselecteerde abonnement zijn.

```azurepowershell
Get-AzRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
```

```Example
Name                     IsCustom
----                     --------
Virtual Machine Operator     True
```

Als het geselecteerde abonnement zich niet in de `AssignableScopes` van de rol, de aangepaste rol niet weergegeven.

## <a name="list-a-custom-role-definition"></a>Lijst van een aangepaste roldefinitie

U kunt een aangepaste roldefinitie gebruiken [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition). Dit is dezelfde opdracht die u voor een ingebouwde rol gebruikt.

```azurepowershell
Get-AzRoleDefinition <role name> | ConvertTo-Json
```

```Example
PS C:\> Get-AzRoleDefinition "Virtual Machine Operator" | ConvertTo-Json

{
  "Name": "Virtual Machine Operator",
  "Id": "00000000-0000-0000-0000-000000000000",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.ResourceHealth/availabilityStatuses/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Het volgende voorbeeld worden alleen de acties van de rol:

```azurepowershell
(Get-AzRoleDefinition <role name>).Actions
```

```Example
PS C:\> (Get-AzRoleDefinition "Virtual Machine Operator").Actions

"Microsoft.Storage/*/read",
"Microsoft.Network/*/read",
"Microsoft.Compute/*/read",
"Microsoft.Compute/virtualMachines/start/action",
"Microsoft.Compute/virtualMachines/restart/action",
"Microsoft.Authorization/*/read",
"Microsoft.ResourceHealth/availabilityStatuses/read",
"Microsoft.Resources/subscriptions/resourceGroups/read",
"Microsoft.Insights/alertRules/*",
"Microsoft.Insights/diagnosticSettings/*",
"Microsoft.Support/*"
```

## <a name="create-a-custom-role"></a>Een aangepaste rol maken

U kunt een aangepaste rol maken met de [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) opdracht. Er zijn twee methoden voor het structureren van de rol, met behulp van `PSRoleDefinition` object of een JSON-sjabloon. 

### <a name="get-operations-for-a-resource-provider"></a>Bewerkingen ophalen voor een resourceprovider

Wanneer u aangepaste rollen maken, is het belangrijk te weten van alle mogelijke bewerkingen van de resourceproviders.
U kunt de lijst weergeven met [resourceproviderbewerkingen](resource-provider-operations.md) of kunt u de [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) opdracht voor het ophalen van deze informatie.
Als u controleren van de beschikbare bewerkingen voor virtuele machines wilt, bijvoorbeeld deze opdracht gebruiken:

```azurepowershell
Get-AzProviderOperation <operation> | FT OperationName, Operation, Description -AutoSize
```

```Example
PS C:\> Get-AzProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation, Description -AutoSize

OperationName                                  Operation                                                      Description
-------------                                  ---------                                                      -----------
Get Virtual Machine                            Microsoft.Compute/virtualMachines/read                         Get the propertie...
Create or Update Virtual Machine               Microsoft.Compute/virtualMachines/write                        Creates a new vir...
Delete Virtual Machine                         Microsoft.Compute/virtualMachines/delete                       Deletes the virtu...
Start Virtual Machine                          Microsoft.Compute/virtualMachines/start/action                 Starts the virtua...
...
```

### <a name="create-a-custom-role-with-the-psroledefinition-object"></a>Een aangepaste rol maken met het object PSRoleDefinition

Als u PowerShell gebruiken om een aangepaste rol maken, kunt u een van de [ingebouwde rollen](built-in-roles.md) als uitgangspunt of als u opnieuw kunt beginnen. Het eerste voorbeeld in deze sectie begint met een ingebouwde rol en past deze met meer machtigingen. Bewerk de kenmerken toe te voegen de `Actions`, `NotActions`, of `AssignableScopes` die u wilt en klikt u vervolgens de wijzigingen opslaan als een nieuwe rol.

Het volgende voorbeeld wordt gestart met de [Inzender voor virtuele machines](built-in-roles.md#virtual-machine-contributor) ingebouwde rol te maken van een aangepaste rol met de naam *virtuele Machine-Operator*. De nieuwe rol verleent toegang tot alle leesbewerkingen van *Microsoft.Compute*, *Microsoft.Storage*, en *Microsoft.Network* resource providers en geeft toegang tot het starten , opnieuw opstarten en controleren van virtuele machines. De aangepaste rol kan worden gebruikt in twee abonnementen.

```azurepowershell
$role = Get-AzRoleDefinition "Virtual Machine Contributor"
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
$role.Actions.Add("Microsoft.ResourceHealth/availabilityStatuses/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/00000000-0000-0000-0000-000000000000")
$role.AssignableScopes.Add("/subscriptions/11111111-1111-1111-1111-111111111111")
New-AzRoleDefinition -Role $role
```

Het volgende voorbeeld ziet u een andere manier om te maken de *virtuele Machine-Operator* aangepaste rol. Deze methode begint met het maken van een nieuw `PSRoleDefinition` object. De actie-bewerkingen zijn opgegeven in de `perms` -variabele en ingesteld op de `Actions` eigenschap. De `NotActions` eigenschap wordt ingesteld met het lezen van de `NotActions` uit de [Inzender voor virtuele machines](built-in-roles.md#virtual-machine-contributor) ingebouwde rol. Aangezien [Inzender voor virtuele machines](built-in-roles.md#virtual-machine-contributor) geen `NotActions`, deze regel is niet vereist, maar u ziet hoe gegevens kan worden opgehaald uit een andere rol.

```azurepowershell
$role = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
$role.Name = 'Virtual Machine Operator 2'
$role.Description = 'Can monitor and restart virtual machines.'
$role.IsCustom = $true
$perms = 'Microsoft.Storage/*/read','Microsoft.Network/*/read','Microsoft.Compute/*/read'
$perms += 'Microsoft.Compute/virtualMachines/start/action','Microsoft.Compute/virtualMachines/restart/action'
$perms += 'Microsoft.Authorization/*/read'
$perms += 'Microsoft.ResourceHealth/availabilityStatuses/read'
$perms += 'Microsoft.Resources/subscriptions/resourceGroups/read'
$perms += 'Microsoft.Insights/alertRules/*','Microsoft.Support/*'
$role.Actions = $perms
$role.NotActions = (Get-AzRoleDefinition -Name 'Virtual Machine Contributor').NotActions
$subs = '/subscriptions/00000000-0000-0000-0000-000000000000','/subscriptions/11111111-1111-1111-1111-111111111111'
$role.AssignableScopes = $subs
New-AzRoleDefinition -Role $role
```

### <a name="create-a-custom-role-with-json-template"></a>Een aangepaste rol maken met JSON-sjabloon

Een JSON-sjabloon kan worden gebruikt als de definitie van de gegevensbron voor de aangepaste rol. Het volgende voorbeeld wordt een aangepaste rol lezen-toegang tot opslagruimte en rekencapaciteit resources, toegang tot ondersteuning, en die rol worden toegevoegd aan twee abonnementen. Maak een nieuw bestand `C:\CustomRoles\customrole1.json` met het volgende voorbeeld. De Id moet worden ingesteld op `null` bij het maken van de rol van de eerste als een nieuwe ID wordt automatisch gegenereerd. 

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
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

De rol toevoegen aan de abonnementen, voer de volgende PowerShell-opdracht:

```azurepowershell
New-AzRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="update-a-custom-role"></a>Een aangepaste rol bijwerken

Net als bij het maken van een aangepaste rol, kunt u een bestaande aangepaste rol met behulp van de `PSRoleDefinition` object of een JSON-sjabloon.

### <a name="update-a-custom-role-with-the-psroledefinition-object"></a>Een aangepaste rol bijwerken met het object PSRoleDefinition

Voor het wijzigen van een aangepaste rol, gebruikt u eerst de [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) opdracht voor het ophalen van de roldefinitie. Controleer vervolgens de gewenste wijzigingen aan de roldefinitie van de. Gebruik tot slot de [Set AzRoleDefinition](/powershell/module/az.resources/set-azroledefinition) opdracht voor het opslaan van de aangepaste roldefinitie.

Het volgende voorbeeld wordt de `Microsoft.Insights/diagnosticSettings/*` bewerking naar de *virtuele Machine-Operator* aangepaste rol.

```azurepowershell
$role = Get-AzRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzRoleDefinition -Role $role
```

```Example
PS C:\> $role = Get-AzRoleDefinition "Virtual Machine Operator"
PS C:\> $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
PS C:\> Set-AzRoleDefinition -Role $role

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

Het volgende voorbeeld wordt een Azure-abonnement aan de toewijsbare bereiken van de *virtuele Machine-Operator* aangepaste rol.

```azurepowershell
Get-AzSubscription -SubscriptionName Production3

$role = Get-AzRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
Set-AzRoleDefinition -Role $role
```

```Example
PS C:\> Get-AzSubscription -SubscriptionName Production3

Name     : Production3
Id       : 22222222-2222-2222-2222-222222222222
TenantId : 99999999-9999-9999-9999-999999999999
State    : Enabled

PS C:\> $role = Get-AzRoleDefinition "Virtual Machine Operator"
PS C:\> $role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
PS C:\> Set-AzRoleDefinition -Role $role

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

### <a name="update-a-custom-role-with-a-json-template"></a>Een aangepaste rol bijwerken met een JSON-sjabloon

U kunt eenvoudig een een bestaande aangepaste rol toevoegen of verwijderen van acties wijzigen met behulp van de vorige JSON-sjabloon. De JSON-sjabloon bijwerken en toevoegen van de gelezen actie voor netwerken, zoals wordt weergegeven in het volgende voorbeeld. De definities die worden vermeld in de sjabloon zijn niet cumulatief toegepast op een bestaande definitie, wat betekent dat de rol wordt weergegeven zoals u in de sjabloon. U moet ook het veld-Id bijwerken met de ID van de rol. Als u niet zeker weet wat deze waarde is, kunt u de [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) cmdlet om op te halen van deze informatie.

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
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Voer de volgende PowerShell-opdracht voor het bijwerken van de bestaande rol:

```azurepowershell
Set-AzRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen

Als u wilt een aangepaste rol verwijderen, gebruikt u de [Remove-AzRoleDefinition](/powershell/module/az.resources/remove-azroledefinition) opdracht.

Het volgende voorbeeld verwijdert u de *virtuele Machine-Operator* aangepaste rol.

```azurepowershell
Get-AzRoleDefinition "Virtual Machine Operator"
Get-AzRoleDefinition "Virtual Machine Operator" | Remove-AzRoleDefinition
```

```Example
PS C:\> Get-AzRoleDefinition "Virtual Machine Operator"

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}

PS C:\> Get-AzRoleDefinition "Virtual Machine Operator" | Remove-AzRoleDefinition

Confirm
Are you sure you want to remove role definition with name 'Virtual Machine Operator'.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Een aangepaste rol voor Azure-resources met behulp van Azure PowerShell maken](tutorial-custom-role-powershell.md)
- [Aangepaste rollen voor Azure-resources](custom-roles.md)
- [Azure Resource Manager-resourceproviderbewerkingen](resource-provider-operations.md)
