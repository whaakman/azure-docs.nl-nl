---
title: Gebruik de Module voor Azure Stack | Microsoft Docs
description: Meer informatie over het beperken van een Azure-abonnement zich gedraagt als een Azure Stack-abonnement
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 937ef34f-14d4-4ea9-960b-362ba986f000
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/26/2019
ms.author: sethm
ms.lastreviewed: 03/26/2019
ms.openlocfilehash: 9fade97bbe783cf156f5b73523bc0834a34df926
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487444"
---
# <a name="manage-azure-policy-using-the-azure-stack-policy-module"></a>Azure policy met behulp van de Azure Stack-beleidsmodule beheren

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

De beleidsmodule van Azure Stack kunt u een Azure-abonnement configureren met de dezelfde versiebeheer en de beschikbaarheid van services als Azure Stack. Maakt gebruik van de module de [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition) PowerShell-cmdlet voor het maken van een Azure-beleid, zodat ze worden beperkt de resourcetypen en services die beschikbaar zijn in een abonnement. U maakt een beleidstoewijzing binnen het juiste bereik met behulp van de [New-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) cmdlet. Na het configureren van het beleid, kunt u uw Azure-abonnement voor het ontwikkelen van apps is gericht op Azure Stack.

## <a name="install-the-module"></a>Installeer de module

1. Installeer de vereiste versie van de AzureRM PowerShell-module, zoals beschreven in stap 1 van [PowerShell installeren voor Azure Stack](azure-stack-powershell-install.md).
2. [De hulpprogramma's voor Azure Stack downloaden vanuit GitHub](azure-stack-powershell-download.md).
3. [PowerShell configureren voor gebruik met Azure Stack](azure-stack-powershell-configure-user.md).
4. Importeer de module AzureStack.Policy.psm1:


   ```powershell
   Import-Module .\Policy\AzureStack.Policy.psm1
   ```

## <a name="apply-policy-to-azure-subscription"></a>Beleid toepassen op Azure-abonnement

U kunt de volgende opdracht gebruiken om toe te passen een standaardbeleid voor Azure Stack op basis van uw Azure-abonnement. Voordat u deze opdracht uitvoert, vervangt `Azure subscription name` met de naam van uw Azure-abonnement:

```powershell
Add-AzureRmAccount
$s = Select-AzureRmSubscription -SubscriptionName "Azure subscription name"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID
```

## <a name="apply-policy-to-a-resource-group"></a>Beleid toepassen op een resourcegroep

Het is raadzaam om toe te passen van beleidsregels die gedetailleerdere zijn. Een voorbeeld: als u mogelijk andere resources die worden uitgevoerd in hetzelfde abonnement. U kunt het bereik van de toepassing van het beleid aan een specifieke resourcegroep, waarmee u uw apps testen voor Azure Stack met behulp van Azure-resources. Voordat u de volgende opdracht uitvoert, Vervang `Azure subscription name` met de naam van uw Azure-abonnement:

```powershell
Add-AzureRmAccount
$rgName = 'myRG01'
$s = Select-AzureRmSubscription -SubscriptionName "Azure subscription name"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID/resourceGroups/$rgName
```

## <a name="policy-in-action"></a>Beleid in actie

Nadat u de Azure-beleid hebt geïmplementeerd, ontvangt u een foutbericht wanneer u probeert een resource die niet is toegestaan door het beleid implementeren:

![Resultaat van de resource-implementatie is mislukt, omdat beleidsbeperking](./media/azure-stack-policy-module/image1.png)

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)
* [Sjablonen implementeren met Azure CLI](azure-stack-deploy-template-command-line.md)
* [Sjablonen implementeren met Visual Studio](azure-stack-deploy-template-visual-studio.md)
