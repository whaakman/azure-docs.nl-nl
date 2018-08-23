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
ms.date: 08/15/2018
ms.author: sethm
ms.openlocfilehash: 273b1065d51552dd7b92d4a10fc856294a23a4e7
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42058123"
---
# <a name="manage-azure-policy-using-the-azure-stack-policy-module"></a>Azure policy met behulp van de Azure Stack-beleidsmodule beheren

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

De beleidsmodule van Azure Stack kunt u een Azure-abonnement configureren met de dezelfde versiebeheer en servicebeschikbaarheid als Azure Stack.  Maakt gebruik van de module de **New-AzureRMPolicyAssignment** cmdlet voor het maken van een Azure-beleid, zodat ze worden beperkt de resourcetypen en services die beschikbaar zijn in een abonnement.  Na het configureren van het beleid, kunt u uw Azure-abonnement voor het ontwikkelen van apps is gericht op Azure Stack.

## <a name="install-the-module"></a>De module installeren

1. Installeer de vereiste versie van de AzureRM PowerShell-module, zoals beschreven in stap 1 van [PowerShell installeren voor Azure Stack](azure-stack-powershell-install.md).
2. [De hulpprogramma's voor Azure Stack downloaden vanuit GitHub](azure-stack-powershell-download.md)
3. [PowerShell configureren voor gebruik met Azure Stack](azure-stack-powershell-configure-user.md)

4. Importeer de module AzureStack.Policy.psm1:

   ```PowerShell
   Import-Module .\Policy\AzureStack.Policy.psm1
   ```

## <a name="apply-policy-to-azure-subscription"></a>Beleid toepassen op Azure-abonnement

U kunt de volgende opdracht gebruiken om toe te passen een standaardbeleid voor Azure Stack op basis van uw Azure-abonnement. Voordat u deze opdracht uitvoert, vervangt *de naam van de Azure-abonnement* met uw Azure-abonnement.

```PowerShell
Add-AzureRmAccount
$s = Select-AzureRmSubscription -SubscriptionName "<Azure Subscription Name>"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID

```

## <a name="apply-policy-to-a-resource-group"></a>Beleid toepassen op een resourcegroep

U wilt toepassen van beleid dat meer gedetailleerde is. Een voorbeeld: als u mogelijk andere resources die worden uitgevoerd in hetzelfde abonnement. U kunt het bereik van de toepassing van het beleid aan een specifieke resourcegroep, waarmee u uw apps testen voor Azure Stack met behulp van Azure-resources. Voordat u de volgende opdracht uitvoert, Vervang *de naam van de Azure-abonnement* met de naam van uw Azure-abonnement.

```PowerShell
Add-AzureRmAccount
$rgName = 'myRG01'
$s = Select-AzureRmSubscription -SubscriptionName "<Azure Subscription Name>"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID/resourceGroups/$rgName

```

## <a name="policy-in-action"></a>Beleid in actie

Nadat u de Azure-beleid hebt geïmplementeerd, u een foutbericht ontvangt wanneer u probeert te implementeren van een resource die niet toegestaan door het beleid.

![Resultaat van de resource-implementatie is mislukt, omdat beleidsbeperking](./media/azure-stack-policy-module/image1.png)

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)
* [Sjablonen implementeren met Azure CLI](azure-stack-deploy-template-command-line.md)
* [Sjablonen implementeren met Visual Studio](azure-stack-deploy-template-visual-studio.md)
