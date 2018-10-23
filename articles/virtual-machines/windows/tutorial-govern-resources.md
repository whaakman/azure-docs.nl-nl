---
title: 'Zelfstudie: Virtuele Azure-machines beheren met Azure PowerShell | Microsoft Docs'
description: In deze zelfstudie leert u hoe u Azure PowerShell gebruikt voor het beheren van virtuele Azure-machines door RBAC, beleid, vergrendelingen en tags toe te passen
services: virtual-machines-windows
documentationcenter: virtual-machines
author: tfitzmac
manager: jeconnoc
editor: tysonn
ms.service: virtual-machines-windows
ms.workload: infrastructure
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: tutorial
ms.date: 10/12/2018
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: 6377a54cc862bb5f62726c3ce91a41cc6eb0763d
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2018
ms.locfileid: "49311385"
---
# <a name="tutorial-learn-about-windows-virtual-machine-governance-with-azure-powershell"></a>Zelfstudie: Meer informatie over het beheren van virtuele Windows-machines met Azure PowerShell

[!INCLUDE [Resource Manager governance introduction](../../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Voor de voorbeelden in dit artikel hebt u versie 6.0 of hoger van Azure PowerShell nodig. Als u PowerShell lokaal uitvoert en niet beschikt over versie 6.0 of hoger, moet u [uw versie bijwerken](/powershell/azure/install-azurerm-ps). U moet ook `Connect-AzureRmAccount` uitvoeren om een verbinding met Azure tot stand te brengen. Voor lokale installaties moet u ook [de Azure AD PowerShell-module downloaden](https://www.powershellgallery.com/packages/AzureAD/) om een nieuwe Azure Active Directory-groep te maken.

## <a name="understand-scope"></a>Bereik

[!INCLUDE [Resource Manager governance scope](../../../includes/resource-manager-governance-scope.md)]

In deze zelfstudie past u alle beheerinstellingen toe op een resourcegroep zodat u deze instellingen eenvoudig kunt verwijderen wanneer u klaar bent.

We gaan nu de resourcegroep maken.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

De resourcegroep is momenteel leeg.

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

U wilt er zeker van zijn dat gebruikers in uw organisatie het juiste toegangsniveau tot deze resources hebben. U wilt gebruikers geen onbeperkte toegang verlenen, maar u moet er ook voor zorgen dat ze hun werk kunnen doen. Met [toegangsbeheer op basis van rollen](../../role-based-access-control/overview.md) kunt u beheren welke gebruikers gemachtigd zijn specifieke acties binnen een bepaald bereik uit te voeren.

Om roltoewijzingen te maken en te verwijderen, moeten gebruikers `Microsoft.Authorization/roleAssignments/*`-toegang hebben. Deze toegang wordt verleend via de rol Eigenaar of Administrator voor gebruikerstoegang.

Voor het beheren van virtuele machine-oplossingen zijn er drie resourcespecifieke rollen die toegang bieden die gewoonlijk nodig is:

* [Inzender voor virtuele machines](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Inzender voor netwerken](../../role-based-access-control/built-in-roles.md#network-contributor)
* [Inzender voor opslagaccounts](../../role-based-access-control/built-in-roles.md#storage-account-contributor)

In plaats van rollen toe te wijzen aan individuele gebruikers, is het vaak eenvoudiger om een Azure Active Directory-groep te gebruiken die gebruikers bevat die vergelijkbare acties moeten ondernemen. U wijst dan de juiste rol aan die groep toe. Gebruik voor dit artikel een bestaande groep om de virtuele machine te beheren of gebruik de portal om [een Azure Active Directory-groep te maken](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Gebruik, nadat u een nieuwe groep hebt gemaakt of een bestaande groep hebt gevonden, de opdracht [New AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) om de Azure Active Directory-groep toe te wijzen aan de rol Inzender voor virtuele machines voor de resourcegroep.  

```azurepowershell-interactive
$adgroup = Get-AzureRmADGroup -DisplayName <your-group-name>

New-AzureRmRoleAssignment -ObjectId $adgroup.id `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Als een fout optreedt waarin staat dat **Principal<guid> niet bestaat in de directory**, is de nieuwe groep niet in heel Azure Active Directory doorgegeven. Probeer de opdracht opnieuw uit te voeren.

Normaal gesproken herhaalt u het proces voor *Inzender voor netwerken* en *Inzender voor opslagaccounts* om ervoor te zorgen dat gebruikers worden toegewezen om de geïmplementeerde resources te beheren. In dit artikel kunt u deze stappen overslaan.

## <a name="azure-policy"></a>Azure Policy

[Azure-beleid](../../azure-policy/azure-policy-introduction.md) helpt u ervoor te zorgen dat alle resources in het abonnement voldoen aan de bedrijfsnormen. Uw abonnement heeft al meerdere beleidsdefinities. Als u de beschikbare beleidsdefinities wilt bekijken, gebruikt u de opdracht [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition):

```azurepowershell-interactive
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

U ziet de bestaande beleidsdefinities. Het type beleid is **Ingebouwd** of **Aangepast**. Bekijk de definities voor beleid waarin een voorwaarde wordt beschreven die u wilt toewijzen. In dit artikel wijst u beleid toe waarmee:

* De locaties voor alle resources worden beperkt.
* De SKU's voor virtuele machines worden beperkt.
* Controleer virtuele machines die niet gebruikmaken van beheerde schijven.

In het volgende voorbeeld haalt u drie beleidsdefinities op basis van de weergavenaam op. U gebruikt de opdracht [New-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) om deze definities toe te wijzen aan de resourcegroep. Voor sommige beleidsregels kunt u parameterwaarden opgeven om de toegestane waarden te specificeren.

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

## <a name="deploy-the-virtual-machine"></a>De virtuele machine implementeren

U hebt rollen en beleid hebt toegewezen. U bent nu dus klaar om uw oplossing te implementeren. De standaardgrootte is Standard_DS1_v2. Dit is een van de toegestane SKU's. Als u deze stap uitvoert, wordt u gevraagd referenties op te geven. De waarden die u invoert, worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine.

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

Nadat de implementatie is voltooid, kunt u meer beheerinstellingen toepassen op de oplossing.

## <a name="lock-resources"></a>Resources vergrendelen

Met [resourcevergrendelingen](../../azure-resource-manager/resource-group-lock-resources.md) voorkomt u dat gebruikers in uw organisatie per ongeluk kritieke bronnen wijzigen of verwijderen. In tegenstelling tot toegangsbeheer op basis van rollen wordt met resourcevergrendelingen een beperking toegepast op alle gebruikers en rollen. U kunt de vergrendeling instellen op *CanNotDelete* of *ReadOnly*.

Als u de virtuele machine en de netwerkbeveiligingsgroep wilt vergrendelen, gebruikt u de opdracht [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock):

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

Voer de volgende opdracht uit om de vergrendelingen te testen:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup
```

U ziet een fout met de melding dat de verwijderbewerking niet kan worden voltooid vanwege een vergrendeling. De resourcegroep kan alleen worden verwijderd als u de vergrendelingen specifiek verwijdert. Deze stap wordt weergegeven in [Resources opschonen](#clean-up-resources).

## <a name="tag-resources"></a>Resources taggen

U past [tags](../../azure-resource-manager/resource-group-using-tags.md) toe op uw Azure-resources om deze logisch te ordenen in categorieën. Elke tag bestaat uit een naam en een waarde. U kunt de naam Omgeving en de waarde Productie bijvoorbeeld toepassen op alle resources in de productie.

[!INCLUDE [Resource Manager governance tags Powershell](../../../includes/resource-manager-governance-tags-powershell.md)]

Voor het toepassen van tags op een virtuele machine gebruikt u de opdracht [Set AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource):

```azurepowershell-interactive
# Get the virtual machine
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines

# Apply tags to the virtual machine
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Resources zoeken op tag

Als u naar resources met een tagnaam en -waarde wilt zoeken, gebruikt u de opdracht [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource):

```azurepowershell-interactive
(Get-AzureRmResource -Tag @{ Environment="Test"}).Name
```

U kunt de geretourneerde waarden gebruiken voor beheertaken zoals het stoppen van alle virtuele machines met een tagwaarde.

```azurepowershell-interactive
Get-AzureRmResource -Tag @{ Environment="Test"} | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>Kosten weergeven op tagwaarden

[!INCLUDE [Resource Manager governance tags billing](../../../includes/resource-manager-governance-tags-billing.md)]

## <a name="clean-up-resources"></a>Resources opschonen

De vergrendelde netwerkbeveiligingsgroep kan pas worden verwijderd nadat de vergrendeling is verwijderd. Gebruik de opdracht [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) om de vergrendeling te verwijderen:

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
> * Gebruikers toewijst aan een rol
> * Beleidsregels toepast die standaarden afdwingen
> * Kritieke resources beveiligt met vergrendelingen
> * Resources tagt voor facturering en beheer

Ga door met de volgende zelfstudie voor meer informatie over virtuele machines met hoge beschikbaarheid.

> [!div class="nextstepaction"]
> [Virtuele machines bewaken](tutorial-monitoring.md)

