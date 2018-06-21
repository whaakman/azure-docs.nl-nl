---
title: Beheren van toegang met RBAC en Azure PowerShell | Microsoft Docs
description: Informatie over het beheren van toegang voor gebruikers, groepen en toepassingen met behulp van op rollen gebaseerde toegangsbeheer (RBAC) en Azure PowerShell. Dit omvat het weergeven van access, toegang te verlenen en toegang verwijderen.
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
ms.date: 04/17/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 231f7b915c324a5af91564c80d17bbad335d658d
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294766"
---
# <a name="manage-access-using-rbac-and-azure-powershell"></a>Toegang met RBAC en Azure PowerShell beheren

[Op rollen gebaseerde toegangsbeheer (RBAC)](overview.md) is de manier waarop dat u toegang tot bronnen in Azure beheren. Dit artikel wordt beschreven hoe u de toegang voor gebruikers, groepen en toepassingen die gebruikmaken van RBAC en Azure PowerShell beheren.

## <a name="prerequisites"></a>Vereisten

Voordat u PowerShell gebruiken kunt voor het beheren van RBAC, moet u een van de volgende:

* [PowerShell in de Azure-Cloud-Shell](/azure/cloud-shell/overview)
* [Azure PowerShell](/powershell/azure/install-azurerm-ps)

## <a name="list-roles"></a>Lijst met rollen

### <a name="list-all-available-roles"></a>Lijst van alle beschikbare rollen

Lijst met RBAC-rollen die beschikbaar zijn voor toewijzing en het controleren van de bewerkingen die zij toegang verlenen gebruiken [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition | FT Name, Description
```

```Example
AcrImageSigner                                    acr image signer
AcrQuarantineReader                               acr quarantine data reader
AcrQuarantineWriter                               acr quarantine data writer
API Management Service Contributor                Can manage service and the APIs
API Management Service Operator Role              Can manage service but not the APIs
API Management Service Reader Role                Read-only access to service and APIs
Application Insights Component Contributor        Can manage Application Insights components
Application Insights Snapshot Debugger            Gives user permission to use Application Insights Snapshot Debugge...
Automation Job Operator                           Create and Manage Jobs using Automation Runbooks.
Automation Operator                               Automation Operators are able to start, stop, suspend, and resume ...
...
```

### <a name="list-a-specific-role"></a>Lijst van een specifieke rol

U kunt een specifieke rol gebruiken [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition <role name>
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Contributor"

Name             : Contributor
Id               : b24988ac-6180-42a0-ab88-20f7382dd24c
IsCustom         : False
Description      : Lets you manage everything except access to resources.
Actions          : {*}
NotActions       : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
                   Microsoft.Authorization/elevateAccess/Action}
AssignableScopes : {/}
```

### <a name="list-a-specific-role-in-json-format"></a>Lijst van een specifieke functie in JSON-indeling

U kunt een specifieke functie in JSON-indeling gebruiken [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition <role name> | ConvertTo-Json
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Contributor" | ConvertTo-Json

{
    "Name":  "Contributor",
    "Id":  "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "IsCustom":  false,
    "Description":  "Lets you manage everything except access to resources.",
    "Actions":  [
                    "*"
                ],
    "NotActions":  [
                       "Microsoft.Authorization/*/Delete",
                       "Microsoft.Authorization/*/Write",
                       "Microsoft.Authorization/elevateAccess/Action"
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

### <a name="list-actions-of-a-role"></a>Van Lijstacties van een rol

U kunt de acties voor een specifieke rol gebruiken [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition <role name> | FL Actions, NotActions
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Contributor" | FL Actions, NotActions

Actions    : {*}
NotActions : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
             Microsoft.Authorization/elevateAccess/Action}
```

```azurepowershell
(Get-AzureRmRoleDefinition <role name>).Actions
```

```Example
PS C:\> (Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions

Microsoft.Authorization/*/read
Microsoft.Compute/availabilitySets/*
Microsoft.Compute/locations/*
Microsoft.Compute/virtualMachines/*
Microsoft.Compute/virtualMachineScaleSets/*
Microsoft.DevTestLab/schedules/*
Microsoft.Insights/alertRules/*
Microsoft.Network/applicationGateways/backendAddressPools/join/action
Microsoft.Network/loadBalancers/backendAddressPools/join/action
...
```

## <a name="list-access"></a>Lijst met toegang

In RBAC, voor toegang tot lijst, u de naam de roltoewijzingen.

### <a name="list-role-assignments-at-a-specific-scope"></a>Roltoewijzingen lijst op een specifiek bereik

Hier ziet u alle roltoewijzingen voor een opgegeven abonnement, resourcegroep of resource. Bijvoorbeeld, als alle actieve toewijzingen voor een resourcegroep wilt weergeven, gebruikt [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>
```

```Example
PS C:\> Get-AzureRmRoleAssignment -ResourceGroupName pharma-sales-projectforecast | FL DisplayName, RoleDefinitionName, Scope

DisplayName        : Alain Charon
RoleDefinitionName : Backup Operator
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast

DisplayName        : Isabella Simonsen
RoleDefinitionName : BizTalk Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast

DisplayName        : Alain Charon
RoleDefinitionName : Virtual Machine Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
```

### <a name="list-role-assignments-for-a-user"></a>Lijst roltoewijzingen voor een gebruiker

U kunt de rollen die zijn toegewezen aan een opgegeven gebruiker gebruiken [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -SignInName <user email>
```

```Example
PS C:\> Get-AzureRmRoleAssignment -SignInName isabella@example.com | FL DisplayName, RoleDefinitionName, Scope

DisplayName        : Isabella Simonsen
RoleDefinitionName : BizTalk Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
```

U kunt de rollen die zijn toegewezen aan een opgegeven gebruiker en de functies die zijn toegewezen aan de groepen waartoe de gebruiker behoort gebruiken [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -SignInName <user email> -ExpandPrincipalGroups
```

```Example
Get-AzureRmRoleAssignment -SignInName isabella@example.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

### <a name="list-role-assignments-for-classic-service-administrator-and-co-administrators"></a>De roltoewijzingen lijst voor klassieke servicebeheerder en medebeheerders

Roltoewijzingen voor klassieke abonnementsbeheerder en medebeheerders weergeven door gebruiken [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment):

```azurepowershell
Get-AzureRmRoleAssignment -IncludeClassicAdministrators
```

## <a name="grant-access"></a>Toegang verlenen

In RBAC, voor het verlenen van toegang, maakt u een roltoewijzing.

### <a name="search-for-object-ids"></a>Zoeken naar object-id 's

Als u een rol toewijzen, moet u zowel het object (gebruiker, groep of toepassing) en het bereik te identificeren.

Als u de abonnements-ID niet weet, kunt u vinden in de **abonnementen** blade in de Azure portal of u kunt gebruiken [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

Als u de object-ID voor een Azure AD-groep, gebruikt [Get-AzureRmADGroup](/powershell/module/azurerm.resources/get-azurermadgroup):

```azurepowershell
Get-AzureRmADGroup -SearchString <group name in quotes>
```

Als u de object-ID voor een Azure AD-service-principal of toepassing, gebruikt [Get-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal):

```azurepowershell
Get-AzureRmADServicePrincipal -SearchString <service name in quotes>
```

### <a name="create-a-role-assignment-for-an-application-at-a-subscription-scope"></a>Een roltoewijzing voor een toepassing op een scope abonnement maken

Gebruiken voor het verlenen van toegang tot een toepassing op het abonnementsbereik [New AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```azurepowershell
New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>
```

```Example
PS C:\> New-AzureRmRoleAssignment -ObjectId 77777777-7777-7777-7777-777777777777 -RoleDefinitionName "Reader" -Scope /subscriptions/00000000-0000-0000-0000-000000000000

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/66666666-6666-6666-6666-666666666666
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
DisplayName        : MyApp1
SignInName         :
RoleDefinitionName : Reader
RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId           : 77777777-7777-7777-7777-777777777777
ObjectType         : ServicePrincipal
CanDelegate        : False
```

### <a name="create-a-role-assignment-for-a-user-at-a-resource-group-scope"></a>Een roltoewijzing voor een gebruiker bij een groepsbereik resource maken

Gebruiken om toegang te verlenen aan een gebruiker op het groepsbereik resource, [New AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```azurepowershell
New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>
```

```Example
PS C:\> New-AzureRmRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales-projectforecast


RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast/pr
                     oviders/Microsoft.Authorization/roleAssignments/55555555-5555-5555-5555-555555555555
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
DisplayName        : Alain Charon
SignInName         : alain@example.com
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

### <a name="create-a-role-assignment-for-a-group-at-a-resource-scope"></a>Een roltoewijzing voor een groep maken op een scope resource

Gebruiken om toegang te verlenen aan een groep voor de resource-scope, [New AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```azurepowershell
New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>
```

```Example
PS C:\> Get-AzureRmADGroup -SearchString "Pharma"

SecurityEnabled DisplayName         Id                                   Type
--------------- -----------         --                                   ----
           True Pharma Sales Admins aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa Group

PS C:\> New-AzureRmRoleAssignment -ObjectId aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa -RoleDefinitionName "Virtual Machine Contributor" -ResourceName RobertVirtualNetwork -ResourceType Microsoft.Network/virtualNetworks -ResourceGroupName RobertVirtualNetworkResourceGroup

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/RobertVirtualNetwork/providers/Microsoft.Authorizat
                     ion/roleAssignments/bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/RobertVirtualNetwork
DisplayName        : Pharma Sales Admins
SignInName         :
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa
ObjectType         : Group
CanDelegate        : False
```

## <a name="remove-access"></a>Toegang verwijderen

In RBAC, als u wilt verwijderen, access, u verwijdert een roltoewijzing met behulp van [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment):

```azurepowershell
Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>
```

```Example
PS C:\> Remove-AzureRmRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales-projectforecast
```

## <a name="list-custom-roles"></a>Lijst met aangepaste rollen

Als de functies die beschikbaar voor toewijzing op een scope zijn wilt weergeven, gebruikt de [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) opdracht.

Het volgende voorbeeld worden alle functies die beschikbaar voor toewijzing in het geselecteerde abonnement zijn.

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

### <a name="create-a-role-with-psroledefinition-object"></a>Een gebruikersrol maakt met PSRoleDefinition object

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

Het volgende voorbeeld ziet u een andere manier om te maken de *virtuele Machine Operator* aangepaste rol. Deze begint met het maken van een nieuw PSRoleDefinition-object. De actie-bewerkingen zijn opgegeven in de `perms` -variabele en is ingesteld op de `Actions` eigenschap. De `NotActions` eigenschap is ingesteld door het lezen van de `NotActions` van de [Virtual Machine Contributor](built-in-roles.md#virtual-machine-contributor) ingebouwde rol. Aangezien [Virtual Machine Contributor](built-in-roles.md#virtual-machine-contributor) geen `NotActions`, deze regel is niet vereist, maar er wordt weergegeven hoe gegevens kan worden opgehaald uit een andere rol.

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

### <a name="create-role-with-json-template"></a>Rol met JSON-sjabloon maken

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

## <a name="modify-a-custom-role"></a>Een aangepaste rol wijzigen

Net als bij het maken van een aangepaste beveiligingsrol, kunt u een bestaande aangepaste rol met behulp van de `PSRoleDefinition` object of een JSON-sjabloon.

### <a name="modify-role-with-psroledefinition-object"></a>Rol met PSRoleDefinition object wijzigen

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

### <a name="modify-role-with-json-template"></a>Rol met JSON-sjabloon wijzigen

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

* [Azure PowerShell gebruiken met Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
