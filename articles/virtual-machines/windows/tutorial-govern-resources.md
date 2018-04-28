---
title: Azure virtuele machines met Azure PowerShell reguleren | Microsoft Docs
description: Zelfstudie - virtuele machines in Azure beheren door toe te passen RBAC, beleidsregels, vergrendelingen en labels met Azure PowerShell
services: virtual-machines-windows
documentationcenter: virtual-machines
author: tfitzmac
manager: jeconnoc
editor: tysonn
ms.service: virtual-machines-windows
ms.workload: infrastructure
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: tomfitz
ms.openlocfilehash: d4e09eb11ea04c31b7e302b7f66f8e67c13e8252
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="virtual-machine-governance-with-azure-powershell"></a>Beheer van de virtuele machine met Azure PowerShell

[!INCLUDE [Resource Manager governance introduction](../../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van de PowerShell lokaal, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. Voor lokale installaties, moet u ook [downloaden van de Azure AD PowerShell-module](https://www.powershellgallery.com/packages/AzureAD/) voor het maken van een nieuwe Azure Active Directory-groep.

## <a name="understand-scope"></a>Bereik begrijpen

[!INCLUDE [Resource Manager governance scope](../../../includes/resource-manager-governance-scope.md)]

In deze zelfstudie maakt u alle management instellingen toepassen op een resourcegroep zodat u deze instellingen wanneer u klaar bent eenvoudig kunt verwijderen.

We maken die resourcegroep.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

De resourcegroep is momenteel leeg.

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

U wilt controleren of gebruikers in uw organisatie hebben het juiste niveau van toegang tot deze bronnen. U niet wilt onbeperkte toegang verlenen aan gebruikers, maar moet u ook om ervoor te zorgen dat ze hun werk te kunnen doen. [Toegangsbeheer op basis van rollen](../../role-based-access-control/overview.md) kunt u beheren welke gebruikers hebben een machtiging voor het voltooien van specifieke acties op een scope.

Als u wilt maken en verwijderen van roltoewijzingen, moeten gebruikers hebben `Microsoft.Authorization/roleAssignments/*` toegang. Deze toegang wordt verleend door middel van de eigenaar of beheerder voor gebruikerstoegang rollen.

Voor het beheren van virtuele machine oplossingen, zijn er drie resourcespecifieke rollen die vaak nodig toegang bieden:

* [Virtual Machine Contributor](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Inzender voor netwerken](../../role-based-access-control/built-in-roles.md#network-contributor)
* [Storage-Account Inzender](../../role-based-access-control/built-in-roles.md#storage-account-contributor)

In plaats van de rollen toewijzen aan individuele gebruikers, is het vaak eenvoudiger [een Azure Active Directory-groep maken](../../active-directory/active-directory-groups-create-azure-portal.md) voor gebruikers hoeven vergelijkbare acties te ondernemen. Vervolgens die groep toewijzen aan de juiste rol. Om te vereenvoudigen in dit artikel, moet u een Azure Active Directory-groep zonder leden maken. U kunt deze groep nog steeds toewijzen aan een rol voor een scope. 

Het volgende voorbeeld wordt een Azure Active Directory-groep met de naam *VMDemoContributors* met een e-mail bijnaam van *vmDemoGroup*. De mail-bijnaam fungeert als een alias voor de groep.

```azurepowershell-interactive
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
```

Het duurt even na de opdrachtprompt voor de groep worden doorgegeven in Azure Active Directory als resultaat geven. Gebruik na een wachttijd van 20 of 30 seconden, de [New AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) opdracht voor het toewijzen van de nieuwe Azure Active Directory-groep aan de rol Inzender van de virtuele Machine voor de resourcegroep.  Als u de volgende opdracht uitvoeren voordat deze is doorgegeven, ontvangt u een foutbericht waarin wordt gemeld **Principal <guid> bestaat niet in de map**. Probeer de opdracht opnieuw uit te voeren.

```azurepowershell-interactive
New-AzureRmRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Normaal gesproken u het proces voor herhalen *Network Contributor* en *Storage Account Inzender* om ervoor te zorgen dat gebruikers worden toegewezen aan het geïmplementeerde resources beheren. In dit artikel kunt u deze stappen overslaan.

## <a name="azure-policies"></a>Azure-beleid

[!INCLUDE [Resource Manager governance policy](../../../includes/resource-manager-governance-policy.md)]

### <a name="apply-policies"></a>Beleid toepassen

Uw abonnement al heeft meerdere beleidsdefinities. Als de beleidsdefinities beschikbaar weergeven, gebruikt de [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition) opdracht:

```azurepowershell-interactive
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

Ziet u de bestaande beleidsdefinities. Het type beleid dat is **BuiltIn** of **aangepaste**. Bekijk de definities voor apparaten die worden beschreven van een voorwaarde die u wilt toewijzen. In dit artikel leert toewijzen u beleid die:

* De locaties voor alle resources worden beperkt.
* De SKU's voor virtuele machines beperkt.
* Virtuele machines die geen van beheerde schijven gebruikmaken controleren.

In het volgende voorbeeld moet u drie beleidsdefinities op basis van de weergavenaam ophalen. U gebruikt de [nieuw AzureRMPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) opdracht deze definities toewijzen aan de resourcegroep. Voor sommige beleidsregels, kunt u parameterwaarden om op te geven van de toegestane waarden opgeven.

```azurepowershell-interactive
# Values to use for parameters
$locations ="eastus", "eastus2"
$skus = "Standard_DS1_v2", "Standard_E2s_v2"

# Get the resource group
$rg = Get-AzureRmResourceGroup -Name myResourceGroup

# Get policy definitions for allowed locations, allowed SKUs, and auditing VMs that don't use managed disks
$locationDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed locations"}
$skuDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed virtual machine SKUs"}
$auditDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Audit VMs that do not use managed disks"}

# Assign policy for allowed locations
New-AzureRMPolicyAssignment -Name "Set permitted locations" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $locationDefinition `
  -listOfAllowedLocations $locations

# Assign policy for allowed SKUs
New-AzureRMPolicyAssignment -Name "Set permitted VM SKUs" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $skuDefinition `
  -listOfAllowedSKUs $skus

# Assign policy for auditing unmanaged disks
New-AzureRMPolicyAssignment -Name "Audit unmanaged disks" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $auditDefinition
```

## <a name="deploy-the-virtual-machine"></a>Implementeer de virtuele machine

U kunt functies en het beleid hebt toegewezen, zodat u klaar bent om uw oplossing implementeren. De standaardgrootte is Standard_DS1_v2, namelijk een van de toegestane SKU's. Als deze stap wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert, worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine.

```azurepowershell-interactive
New-AzureRmVm -ResourceGroupName "myResourceGroup" `
     -Name "myVM" `
     -Location "East US" `
     -VirtualNetworkName "myVnet" `
     -SubnetName "mySubnet" `
     -SecurityGroupName "myNetworkSecurityGroup" `
     -PublicIpAddressName "myPublicIpAddress" `
     -OpenPorts 80,3389
```

Nadat de implementatie is voltooid, kunt u meer instellingen toepassen op de oplossing.

## <a name="lock-resources"></a>Resources vergrendelen

[Resource vergrendelingen](../../azure-resource-manager/resource-group-lock-resources.md) te voorkomen dat gebruikers in uw organisatie per ongeluk worden kritieke bronnen wijzigen of verwijderen. In tegenstelling tot toegangsbeheer op basis van functie toepassen resource vergrendelingen een beperking voor alle gebruikers en rollen. U kunt de vergrendeling op instellen *CanNotDelete* of *ReadOnly*.

Als u wilt vergrendelen op de virtuele machine en de netwerkbeveiligingsgroep, gebruiken de [nieuw AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) opdracht:

```azurepowershell-interactive
# Add CanNotDelete lock to the VM
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup

# Add CanNotDelete lock to the network security group
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Als u wilt testen vergrendelingen zijn opgegeven, probeert de volgende opdracht uit te voeren:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup
```

Er is een fout die aangeeft dat de delete-bewerking kan niet worden uitgevoerd vanwege een vergrendeling. De resourcegroep kan alleen worden verwijderd als u de vergrendelingen specifiek verwijdert. Deze stap wordt weergegeven in [resources opschonen](#clean-up-resources).

## <a name="tag-resources"></a>Tag resources

U hebt toegepast [labels](../../azure-resource-manager/resource-group-using-tags.md) voor uw Azure-resources aan een logische manier te organiseren in categorieën. Elke tag bestaat uit een naam en een waarde. U kunt de naam Omgeving en de waarde Productie bijvoorbeeld toepassen op alle resources in de productie.

[!INCLUDE [Resource Manager governance tags Powershell](../../../includes/resource-manager-governance-tags-powershell.md)]

Gebruiken om tags toepassen op een virtuele machine, de [Set AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) opdracht:

```azurepowershell-interactive
# Get the virtual machine
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines

# Apply tags to the virtual machine
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Resources zoeken op label

Gebruiken om resources te zoeken met een naam en waarde, de [zoeken AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource) opdracht:

```azurepowershell-interactive
(Find-AzureRmResource -TagName Environment -TagValue Test).Name
```

U kunt de geretourneerde waarden gebruiken voor beheertaken zoals alle virtuele machines met een tagwaarde wordt gestopt.

```azurepowershell-interactive
Find-AzureRmResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>Kosten weergeven door labelwaarden

[!INCLUDE [Resource Manager governance tags billing](../../../includes/resource-manager-governance-tags-billing.md)]

## <a name="clean-up-resources"></a>Resources opschonen

De vergrendelde netwerkbeveiligingsgroep kan niet worden verwijderd nadat de vergrendeling wordt verwijderd. U kunt de vergrendeling te verwijderen met de [verwijderen AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) opdracht:

```azurepowershell-interactive
Remove-AzureRmResourceLock -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
Remove-AzureRmResourceLock -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een aangepaste installatiekopie voor een virtuele machine gemaakt. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Gebruikers toewijzen aan een rol
> * Beleidsregels die standaarden afdwingen
> * Kritieke bronnen met vergrendelingen beveiligen
> * Tag-resources voor facturering en -beheer

Ga naar de volgende zelfstudie voor meer informatie over hoe maximaal beschikbare virtuele machines.

> [!div class="nextstepaction"]
> [Virtuele machines bewaken](tutorial-monitoring.md)

