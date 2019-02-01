---
title: Beheren Azure-oplossingen met PowerShell | Microsoft Docs
description: Gebruik Azure PowerShell en Resource Manager om uw resources te beheren.
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
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: e19f9acef256cdd0c690195bd56f971e3ec88c01
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55494728"
---
# <a name="manage-resources-with-azure-powershell"></a>Resources beheren met Azure PowerShell

[!INCLUDE [Resource Manager governance introduction](../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="understand-scope"></a>Bereik

[!INCLUDE [Resource Manager governance scope](../../includes/resource-manager-governance-scope.md)]

In dit artikel hebt toepassen u alle management instellingen op een resourcegroep, zodat u zich eenvoudig deze instellingen wanneer u klaar bent.

Laten we de resourcegroep maken.

```azurepowershell-interactive
Set-AzContext -Subscription <subscription-name>
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

De resourcegroep is momenteel leeg.

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-rbac.md)]

### <a name="assign-a-role"></a>Een rol toewijzen

In dit artikel implementeert u een virtuele machine en de bijbehorende virtuele netwerk. Voor het beheren van virtuele machine-oplossingen zijn er drie resourcespecifieke rollen die toegang bieden die gewoonlijk nodig is:

* [Inzender voor virtuele machines](../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Inzender voor netwerken](../role-based-access-control/built-in-roles.md#network-contributor)
* [Inzender voor opslagaccounts](../role-based-access-control/built-in-roles.md#storage-account-contributor)

In plaats van de rollen toe te wijzen aan individuele gebruikers, is het vaak eenvoudiger [een Azure Active Directory-groep te maken](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) voor gebruikers die vergelijkbare acties moeten ondernemen. U wijst dan de juiste rol aan die groep toe. Ter vereenvoudiging van dit artikel maakt u een Azure Active Directory-groep zonder leden. U kunt aan deze groep wel een rol voor een bereik toewijzen.

Het volgende voorbeeld wordt een groep gemaakt en wijst deze toe aan de rol Inzender voor virtuele machines voor de resourcegroep. Om uit te voeren de `New-AzureAdGroup` opdracht, moet u een van beide gebruikt de [Azure Cloud Shell](/azure/cloud-shell/overview) of [downloaden van de Azure AD PowerShell-module](https://www.powershellgallery.com/packages/AzureAD/).

```azurepowershell-interactive
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
New-AzRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Normaal gesproken herhaalt u het proces voor **Inzender voor netwerken** en **Inzender voor opslagaccounts** om ervoor te zorgen dat gebruikers worden toegewezen om de geïmplementeerde resources te beheren. In dit artikel kunt u deze stappen overslaan.

## <a name="azure-policy"></a>Azure Policy

[Azure-beleid](../azure-policy/azure-policy-introduction.md) helpt u ervoor te zorgen dat alle resources in het abonnement voldoen aan de bedrijfsnormen. Uw abonnement heeft al meerdere beleidsdefinities. Als u wilt zien van de beschikbare beleidsdefinities, gebruikt u:

```azurepowershell-interactive
(Get-AzPolicyDefinition).Properties | Format-Table displayName, policyType
```

U ziet de bestaande beleidsdefinities. Het type beleid is **Ingebouwd** of **Aangepast**. Bekijk de definities voor beleid waarin een voorwaarde wordt beschreven die u wilt toewijzen. In dit artikel wijst u beleid toe waarmee:

* de locaties voor alle resources beperken
* de SKU's voor virtuele machines beperken
* controle uitvoeren op virtuele machines die geen beheerde schijven gebruiken

```azurepowershell-interactive
$locations ="eastus", "eastus2"
$skus = "Standard_DS1_v2", "Standard_E2s_v2"

$rg = Get-AzResourceGroup -Name myResourceGroup

$locationDefinition = Get-AzPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed locations"}
$skuDefinition = Get-AzPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed virtual machine SKUs"}
$auditDefinition = Get-AzPolicyDefinition | where-object {$_.properties.displayname -eq "Audit VMs that do not use managed disks"}

New-AzPolicyAssignment -Name "Set permitted locations" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $locationDefinition `
  -listOfAllowedLocations $locations
New-AzPolicyAssignment -Name "Set permitted VM SKUs" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $skuDefinition `
  -listOfAllowedSKUs $skus
New-AzPolicyAssignment -Name "Audit unmanaged disks" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $auditDefinition
```

## <a name="deploy-the-virtual-machine"></a>De virtuele machine implementeren

U hebt rollen en beleid hebt toegewezen. U bent nu dus klaar om uw oplossing te implementeren. De standaardgrootte is Standard_DS1_v2. Dit is een van de toegestane SKU's. Als deze stap wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert, worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine.

```azurepowershell-interactive
New-AzVm -ResourceGroupName "myResourceGroup" `
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

### <a name="lock-a-resource"></a>Een resource vergrendelen

Als u wilt vergrendelen op de virtuele machine en de netwerkbeveiligingsgroep, gebruikt u:

```azurepowershell-interactive
New-AzResourceLock -LockLevel CanNotDelete `
  -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
New-AzResourceLock -LockLevel CanNotDelete `
  -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

De virtuele machine kan alleen worden verwijderd als u een specifiek Verwijder de vergrendeling. Deze stap wordt weergegeven in [Resources opschonen](#clean-up-resources).

## <a name="tag-resources"></a>Resources taggen

[!INCLUDE [Resource Manager governance tags](../../includes/resource-manager-governance-tags.md)]

### <a name="tag-resources"></a>Resources taggen

[!INCLUDE [Resource Manager governance tags Powershell](../../includes/resource-manager-governance-tags-powershell.md)]

U kunt tags toepassen op een virtuele machine, gebruikt:

```azurepowershell-interactive
$r = Get-AzResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines
Set-AzResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Resources zoeken op tag

Als u resources met een naam van de tag en waarde zoekt, gebruikt:

```azurepowershell-interactive
(Find-AzResource -TagName Environment -TagValue Test).Name
```

U kunt de geretourneerde waarden gebruiken voor beheertaken zoals het stoppen van alle virtuele machines met een tagwaarde.

```azurepowershell-interactive
Find-AzResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzVM
```

### <a name="view-costs-by-tag-values"></a>Kosten weergeven op tagwaarden

Na het toepassen van tags op resources, kunt u kosten voor resources bij deze tags weergeven. Het duurt even voor de kostenanalyse de meest recente informatie toont, dus misschien ziet u de kosten nog niet. Wanneer de kosten beschikbaar zijn, kunt u de kosten voor resources in resourcegroepen in uw abonnement weergeven. Gebruikers moeten [toegang tot factureringsgegevens op abonnementsniveau](../billing/billing-manage-access.md) hebben om de kosten te kunnen zien.

Om kosten per tag in de portal weer te geven, selecteert u uw abonnement en selecteert u **Kostenanalyse**.

![Kostenanalyse](./media/powershell-azure-resource-manager/select-cost-analysis.png)

Vervolgens filtert u op tagwaarde en selecteert u **Toepassen**.

![Kosten per tag weergeven](./media/powershell-azure-resource-manager/view-costs-by-tag.png)

U kunt ook de [Azure Billing-API's](../billing/billing-usage-rate-card-overview.md) gebruiken om kosten programmatisch weer te geven.

## <a name="clean-up-resources"></a>Resources opschonen

De vergrendelde netwerkbeveiligingsgroep kan pas worden verwijderd nadat de vergrendeling is verwijderd. Als u wilt de vergrendeling verwijderen, gebruikt u:

```azurepowershell-interactive
Remove-AzResourceLock -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
Remove-AzResourceLock -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Wanneer u niet meer nodig hebt, kunt u de [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) opdracht voor het verwijderen van de resourcegroep, VM, en alle gerelateerde resources.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het controleren van uw virtuele machines, [controleren en bijwerken van een Windows-Machine met Azure PowerShell](../virtual-machines/windows/tutorial-monitoring.md).
* Voor meer informatie over het gebruik van Azure Security Center voor het implementeren van de aanbevolen beveiligingsprocedures, [beveiliging van virtuele machines bewaken met behulp van Azure Security Center](../virtual-machines/windows/tutorial-azure-security.md).
* U kunt bestaande resources verplaatsen naar een nieuwe resourcegroep. Zie voor voorbeelden van [verplaatsen van Resources aan de nieuwe resourcegroep of abonnement](resource-group-move-resources.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance).
