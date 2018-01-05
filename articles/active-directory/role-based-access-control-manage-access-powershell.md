---
title: Op rollen gebaseerde toegangsbeheer (RBAC) met Azure PowerShell beheren | Microsoft Docs
description: Het beheren van RBAC met Azure PowerShell, met inbegrip van de aanbieding rollen, rollen toewijzen en het verwijderen van roltoewijzingen.
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 631274ec57586a777df8ee07a18b0ad72b905222
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a>Toegangsbeheer op basis van rollen beheren met Azure PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure-CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)

U kunt op rollen gebaseerde toegangsbeheer (RBAC) in de Azure portal en Azure Resource Management-API gebruiken om toegang te beheren voor uw abonnement heel nauwkeurig. Met deze functie kunt u toegang tot Active Directory-gebruikers, groepen of service-principals verlenen door sommige rollen toewijzen aan deze bij een bepaald bereik.

Voordat u PowerShell gebruiken kunt voor het beheren van RBAC, moet u de volgende vereisten:

* Azure PowerShell versie 0.8.8 of hoger. Zie voor het installeren van de meest recente versie en deze koppelen aan uw Azure-abonnement, [installeren en configureren van Azure PowerShell](/powershell/azure/overview).
* Azure Resource Manager-cmdlets. Installeer de [Azure Resource Manager-cmdlets](/powershell/azure/overview) in PowerShell.

## <a name="list-roles"></a>Lijst met rollen
### <a name="list-all-available-roles"></a>Lijst van alle beschikbare rollen
Lijst met RBAC-rollen die beschikbaar zijn voor toewijzing en het controleren van de bewerkingen die zij toegang verlenen gebruiken `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC PowerShell-Get AzureRmRoleDefinition - schermafbeelding](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Van Lijstacties van een rol
U kunt de acties voor een specifieke rol gebruiken `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC PowerShell-Get AzureRmRoleDefinition voor een specifieke rol - schermafbeelding](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Zien wie toegang heeft
RBAC toegangstoewijzingen weergeven door gebruiken `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Roltoewijzingen lijst op een specifiek bereik
Hier ziet u alle toegangstoewijzingen van de voor een opgegeven abonnement, resourcegroep of resource. Bijvoorbeeld, als alle actieve toewijzingen voor een resourcegroep wilt weergeven, gebruikt `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell - Get-AzureRmRoleAssignment voor een resourcegroep - schermafbeelding](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>Lijst met functies die zijn toegewezen aan een gebruiker
U kunt de rollen die zijn toegewezen aan een opgegeven gebruiker en de functies die zijn toegewezen aan de groepen waartoe de gebruiker behoort gebruiken `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell - Get-AzureRmRoleAssignment voor een gebruiker - schermafbeelding](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Lijst met klassieke servicebeheerder en coadmin roltoewijzingen
Om toegangstoewijzingen voor de klassieke abonnementsbeheerder en coadministrators, gebruiken:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Toegang verlenen
### <a name="search-for-object-ids"></a>Zoeken naar object-id 's
Als u een rol toewijzen, moet u zowel het object (gebruiker, groep of toepassing) en het bereik te identificeren.

Als u de abonnements-ID niet weet, kunt u vinden in de **abonnementen** blade in de Azure-portal. Zie voor informatie over het zoeken naar de abonnements-ID, [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) op MSDN.

Als u de object-ID voor een Azure AD-groep, gebruikt u:

    Get-AzureRmADGroup -SearchString <group name in quotes>

Als u de object-ID voor een Azure AD-service-principal of toepassing, gebruikt u:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Een rol toewijzen aan een toepassing op het abonnementsbereik
Voor het verlenen van toegang tot een toepassing bij het abonnementsbereik gebruiken:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC PowerShell - nieuwe AzureRmRoleAssignment - schermafbeelding](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Een rol toewijzen aan een gebruiker op het groepsbereik resource
Om toegang te verlenen aan een gebruiker op het groepsbereik resource, gebruiken:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC PowerShell - nieuwe AzureRmRoleAssignment - schermafbeelding](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Een rol toewijzen aan een groep in het bereik van de resource
Om toegang te verlenen aan een groep voor de resource-scope, gebruiken:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC PowerShell - nieuwe AzureRmRoleAssignment - schermafbeelding](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Toegang verwijderen
Toegang voor gebruikers, groepen en toepassingen, gebruik verwijderen:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC PowerShell-Remove AzureRmRoleAssignment - schermafbeelding](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Een aangepaste beveiligingsrol maken
Gebruik voor het maken van een aangepaste rol de ```New-AzureRmRoleDefinition``` opdracht. Er zijn twee methoden voor het structureren van de rol, met behulp van PSRoleDefinitionObject of een JSON-sjabloon. 

## <a name="get-actions-for-a-resource-provider"></a>Acties voor een Resourceprovider ophalen
Tijdens het maken van aangepaste rollen maken, is het belangrijk te weten van alle mogelijke bewerkingen van de resourceproviders.
Gebruik de ```Get-AzureRMProviderOperation``` opdracht voor deze informatie.
Als u wilt controleren gebruiken de beschikbare bewerkingen voor de virtuele Machine met deze opdracht:

```
Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation , Description -AutoSize
```

### <a name="create-role-with-psroledefinitionobject"></a>Rol maken met PSRoleDefinitionObject
Wanneer u een aangepaste beveiligingsrol maken met PowerShell, kunt u helemaal opnieuw begint of gebruik een van de [ingebouwde rollen](role-based-access-built-in-roles.md) als uitgangspunt. In het voorbeeld in deze sectie begint met een ingebouwde rol en past deze met meer bevoegdheden. Bewerk de kenmerken toevoegen de *acties*, *notActions*, of *scopes* die u wilt en klikt u vervolgens de wijzigingen opslaan als een nieuwe rol.

Het volgende voorbeeld begint met de *Virtual Machine Contributor* rol en gebruikt die voor het maken van een aangepaste beveiligingsrol aangeroepen *virtuele Machine Operator*. De nieuwe rol verleent toegang tot alle leesbewerkingen van *Microsoft.Compute*, *Microsoft.Storage*, en *Microsoft.Network* resource providers en geeft toegang tot het starten , start opnieuw op en virtuele machines bewaken. De aangepaste rol kan worden gebruikt in twee abonnementen.

```
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
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Get AzureRmRoleDefinition - schermafbeelding](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

### <a name="create-role-with-json-template"></a>Rol met JSON-sjabloon maken
Een JSON-sjabloon kan worden gebruikt als de definitie van de gegevensbron voor de aangepaste rol. Het volgende voorbeeld wordt een aangepaste rol waarmee de leestoegang voor opslagruimte en rekencapaciteit resources, toegang tot ondersteuning, en die rol worden toegevoegd aan twee abonnementen. Maak een nieuw bestand `C:\CustomRoles\customrole1.json` met het volgende voorbeeld. De Id moet worden ingesteld op `null` op het eerste rol maken als een nieuwe ID automatisch wordt gegenereerd. 

```
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
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```
Als u wilt de rol toevoegen aan de abonnementen, voer de volgende PowerShell-opdracht:
```
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a>Een aangepaste rol wijzigen
Net als bij het maken van een aangepaste beveiligingsrol, kunt u een bestaande aangepaste rol met behulp van de PSRoleDefinitionObject of een JSON-sjabloon.

### <a name="modify-role-with-psroledefinitionobject"></a>Rol met PSRoleDefinitionObject wijzigen
Voor het wijzigen van een aangepaste beveiligingsrol, gebruikt u eerst de `Get-AzureRmRoleDefinition` opdracht voor het ophalen van de functiedefinitie. Controleer vervolgens de gewenste wijzigingen aan de functiedefinitie. Gebruik tot slot de `Set-AzureRmRoleDefinition` opdracht voor het opslaan van de gewijzigde roldefinitie.

Het volgende voorbeeld wordt de `Microsoft.Insights/diagnosticSettings/*` bewerking is de *virtuele Machine Operator* aangepaste rol.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell - Set-AzureRmRoleDefinition - schermafbeelding](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Het volgende voorbeeld wordt een Azure-abonnement aan de toewijsbare bereiken van de *virtuele Machine Operator* aangepaste rol.

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell - Set-AzureRmRoleDefinition - schermafbeelding](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

### <a name="modify-role-with-json-template"></a>Rol met JSON-sjabloon wijzigen
De vorige JSON-sjabloon gebruikt, kunt u eenvoudig wijzigen een bestaande aangepaste rol wilt toevoegen of verwijderen van acties. Het JSON-sjabloon bijwerken en toevoegen van de gelezen actie voor netwerken, zoals wordt weergegeven in het volgende voorbeeld. De definities die worden vermeld in de sjabloon zijn niet cumulatief toegepast op de definitie van een bestaande, wat betekent dat de rol wordt weergegeven zoals u in de sjabloon opgeven. Ook moet u het veld Id bijwerken met de ID van de rol. Als u niet zeker weet wat deze waarde is, kunt u de `Get-AzureRmRoleDefinition` cmdlet deze gegevens worden opgehaald.

```
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
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

Voer de volgende PowerShell-opdracht voor het bijwerken van de bestaande rol:
```
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Een aangepaste rol verwijderen
U een aangepaste rol verwijderen met de `Remove-AzureRmRoleDefinition` opdracht.

Het volgende voorbeeld verwijdert u de *virtuele Machine Operator* aangepaste rol.

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC PowerShell - Remove-AzureRmRoleDefinition - schermafbeelding](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Lijst met aangepaste rollen
Als de functies die beschikbaar voor toewijzing op een scope zijn wilt weergeven, gebruikt de `Get-AzureRmRoleDefinition` opdracht.

Het volgende voorbeeld worden alle functies die beschikbaar voor toewijzing in het geselecteerde abonnement zijn.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC PowerShell-Get AzureRmRoleDefinition - schermafbeelding](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

In het volgende voorbeeld wordt de *virtuele Machine Operator* aangepaste rol is niet beschikbaar in de *Production4* abonnement omdat dat abonnement bevindt zich niet in de **AssignableScopes** van de rol.

![RBAC PowerShell-Get AzureRmRoleDefinition - schermafbeelding](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Zie ook
* [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md)
  [!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

