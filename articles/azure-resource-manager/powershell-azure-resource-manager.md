---
title: Beheren van Azure-oplossingen met PowerShell | Microsoft Docs
description: Azure PowerShell en Resource Manager gebruiken voor het beheren van uw resources.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: tomfitz
ms.openlocfilehash: 02616ef566dd576c3f406d4b9f3059dab27bf3e0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34603410"
---
# <a name="manage-resources-with-azure-powershell"></a>Resources beheren met Azure PowerShell

[!INCLUDE [Resource Manager governance introduction](../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Zie [Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps) als u PowerShell lokaal wilt installeren en gebruiken. Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="understand-scope"></a>Reikwijdte

[!INCLUDE [Resource Manager governance scope](../../includes/resource-manager-governance-scope.md)]

In dit artikel leert u alle management instellingen toepassen op een resourcegroep zodat u deze instellingen wanneer u klaar bent eenvoudig kunt verwijderen.

Laten we de resourcegroep maken.

```azurepowershell-interactive
Set-AzureRmContext -Subscription <subscription-name>
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

De resourcegroep is momenteel leeg.

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-rbac.md)]

### <a name="assign-a-role"></a>Een rol toewijzen

In dit artikel door een virtuele machine en de bijbehorende virtuele netwerk op te implementeren. Voor het beheren van virtuele machine-oplossingen zijn er drie resourcespecifieke rollen die toegang bieden die gewoonlijk nodig is:

* [Inzender voor virtuele machines](../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Inzender voor netwerken](../role-based-access-control/built-in-roles.md#network-contributor)
* [Inzender voor opslagaccounts](../role-based-access-control/built-in-roles.md#storage-account-contributor)

In plaats van de rollen toe te wijzen aan individuele gebruikers, is het vaak eenvoudiger [een Azure Active Directory-groep te maken](../active-directory/active-directory-groups-create-azure-portal.md) voor gebruikers die vergelijkbare acties moeten ondernemen. U wijst dan de juiste rol aan die groep toe. Ter vereenvoudiging van dit artikel maakt u een Azure Active Directory-groep zonder leden. U kunt aan deze groep wel een rol voor een bereik toewijzen. 

Het volgende voorbeeld wordt een groep gemaakt en toegewezen aan de rol Inzender van de virtuele Machine voor de resourcegroep. Om uit te voeren de `New-AzureAdGroup` opdracht gebruiken, moet u de [Azure Cloud Shell](/azure/cloud-shell/overview) of [downloaden van de Azure AD PowerShell-module](https://www.powershellgallery.com/packages/AzureAD/).

```azurepowershell-interactive
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
New-AzureRmRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Normaal gesproken herhaalt u het proces voor **Inzender voor netwerken** en **Inzender voor opslagaccounts** om ervoor te zorgen dat gebruikers worden toegewezen om de geïmplementeerde resources te beheren. In dit artikel kunt u deze stappen overslaan.

## <a name="azure-policies"></a>Azure-beleid

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-policy.md)]

### <a name="apply-policies"></a>Beleid toepassen

Uw abonnement heeft al meerdere beleidsdefinities. Als de beleidsdefinities beschikbaar weergeven, gebruikt u het:

```azurepowershell-interactive
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

U ziet de bestaande beleidsdefinities. Het type beleid is **Ingebouwd** of **Aangepast**. Bekijk de definities voor beleid waarin een voorwaarde wordt beschreven die u wilt toewijzen. In dit artikel wijst u beleid toe waarmee:

* beperken van de locaties voor alle resources
* de SKU's voor virtuele machines te beperken
* virtuele machines die geen van beheerde schijven gebruikmaken controleren

```azurepowershell-interactive
$locations ="eastus", "eastus2"
$skus = "Standard_DS1_v2", "Standard_E2s_v2"

$rg = Get-AzureRmResourceGroup -Name myResourceGroup

$locationDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed locations"}
$skuDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed virtual machine SKUs"}
$auditDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Audit VMs that do not use managed disks"}

New-AzureRMPolicyAssignment -Name "Set permitted locations" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $locationDefinition `
  -listOfAllowedLocations $locations
New-AzureRMPolicyAssignment -Name "Set permitted VM SKUs" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $skuDefinition `
  -listOfAllowedSKUs $skus
New-AzureRMPolicyAssignment -Name "Audit unmanaged disks" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $auditDefinition
```

## <a name="deploy-the-virtual-machine"></a>De virtuele machine implementeren

U hebt rollen en beleid hebt toegewezen. U bent nu dus klaar om uw oplossing te implementeren. De standaardgrootte is Standard_DS1_v2. Dit is een van de toegestane SKU's. Als deze stap wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert, worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine.

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

[!INCLUDE [Resource Manager governance locks](../../includes/resource-manager-governance-locks.md)]

### <a name="lock-a-resource"></a>Vergrendelen van een resource

Als u wilt vergrendelen op de virtuele machine en de netwerkbeveiligingsgroep, gebruiken:

```azurepowershell-interactive
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

De virtuele machine kan alleen worden verwijderd als u de vergrendeling specifiek verwijdert. Deze stap wordt weergegeven in [Resources opschonen](#clean-up-resources).

## <a name="tag-resources"></a>Resources taggen

[!INCLUDE [Resource Manager governance tags](../../includes/resource-manager-governance-tags.md)]

### <a name="tag-resources"></a>Resources taggen

[!INCLUDE [Resource Manager governance tags Powershell](../../includes/resource-manager-governance-tags-powershell.md)]

Als u wilt tags toepassen op een virtuele machine, gebruiken:

```azurepowershell-interactive
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Resources zoeken op tag

Om resources te zoeken met een naam en waarde, gebruiken:

```azurepowershell-interactive
(Find-AzureRmResource -TagName Environment -TagValue Test).Name
```

U kunt de geretourneerde waarden gebruiken voor beheertaken zoals het stoppen van alle virtuele machines met een tagwaarde.

```azurepowershell-interactive
Find-AzureRmResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>Kosten weergeven op tagwaarden

Na het toepassen van tags aan resources, kunt u de kosten voor bronnen met deze labels weergeven. Het duurt even voor kostenanalyse om weer te geven van de meest recente informatie over het gebruik, zodat u wel de kosten niet nog eens. Wanneer de kosten beschikbaar zijn, kunt u de kosten voor resources weergeven over resourcegroepen in uw abonnement. Gebruikers moeten beschikken over [abonnement niveau toegang tot factureringsgegevens](../billing/billing-manage-access.md) om te zien van de kosten.

Als u wilt weergeven kosten per tag in de portal, selecteer uw abonnement en selecteer **kosten Analysis**.

![Kostenanalyse](./media/powershell-azure-resource-manager/select-cost-analysis.png)

Vervolgens filteren op de waarde van het label en selecteer **toepassen**.

![Weergave kosten per tag](./media/powershell-azure-resource-manager/view-costs-by-tag.png)

U kunt ook de [Azure Billing-API's](../billing/billing-usage-rate-card-overview.md) programmatisch kosten weergeven.

## <a name="clean-up-resources"></a>Resources opschonen

De vergrendelde netwerkbeveiligingsgroep kan pas worden verwijderd nadat de vergrendeling is verwijderd. Gebruik voor het verwijderen van de vergrendeling:

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

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het bewaken van uw virtuele machines, [controleren en bijwerken van een virtuele Windows-Machine met Azure PowerShell](../virtual-machines/windows/tutorial-monitoring.md).
* Voor meer informatie over het gebruik van Azure Security Center voor het implementeren van de aanbevolen beveiligingsprocedures [bewaken van de beveiliging van de virtuele machine met behulp van Azure Security Center](../virtual-machines/windows/tutorial-azure-security.md).
* U kunt bestaande resources verplaatsen naar een nieuwe resourcegroep. Zie voor voorbeelden [Resources verplaatsen naar de nieuwe resourcegroep of abonnement](resource-group-move-resources.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance).
