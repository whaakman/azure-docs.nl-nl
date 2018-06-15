---
title: De beleidsmodule Azure Stack gebruiken | Microsoft Docs
description: Meer informatie over het beperken van een Azure-abonnement kan worden gebruikt als een Azure-Stack-abonnement
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 937ef34f-14d4-4ea9-960b-362ba986f000
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: mabrigg
ms.openlocfilehash: 538cf0eb0f9f2351f7a71a1dd24aab05938963c5
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34259080"
---
# <a name="manage-azure-policy-using-the-azure-stack-policy-module"></a>Azure met behulp van de beleidsmodule van de Azure-Stack-beleid beheren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

De beleidsmodule van Azure Stack kunt u een Azure-abonnement configureren met de dezelfde versies en de beschikbaarheid van de service als Azure-Stack.  De module wordt gebruikt de **nieuw AzureRMPolicyAssignment** cmdlet voor het maken van een Azure-beleid, waardoor de resourcetypen en services die beschikbaar zijn in een abonnement.  Nadat het beleid zijn geconfigureerd, kunt u uw Azure-abonnement voor het ontwikkelen van apps die zijn gericht voor Azure-Stack.

## <a name="install-the-module"></a>De module installeren

1. Installeer de vereiste versie van de AzureRM PowerShell-module, zoals beschreven in stap 1 van [Installeer PowerShell voor Azure-Stack](azure-stack-powershell-install.md).
2. [Download de Azure-Stack-tools van GitHub](azure-stack-powershell-download.md)
3. [PowerShell configureren voor gebruik met Azure Stack](azure-stack-powershell-configure-user.md)

4. Importeer de module AzureStack.Policy.psm1:

   ```PowerShell
   Import-Module .\Policy\AzureStack.Policy.psm1
   ```

## <a name="apply-policy-to-azure-subscription"></a>Beleid toepassen op Azure-abonnement

De volgende opdracht kunt u een Azure-Stack standaardbeleid van toepassing op basis van uw Azure-abonnement. Voordat u deze opdracht uitvoert, vervangt u *de naam van de Azure-abonnement* met uw Azure-abonnement.

```PowerShell
Add-AzureRmAccount
$s = Select-AzureRmSubscription -SubscriptionName "<Azure Subscription Name>"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID

```

## <a name="apply-policy-to-a-resource-group"></a>Beleid toepassen op een resourcegroep

U kunt beleidsregels voor gedetailleerdere toepassen. Een voorbeeld: als u wellicht andere resources voor uitvoering in hetzelfde abonnement. U kunt het bereik van de toepassing van beleid aan een specifieke resourcegroep, waarmee u uw apps testen voor Azure-resources met Azure-Stack. Voordat u de volgende opdracht uitvoert, vervangt *de naam van de Azure-abonnement* met de naam van uw Azure-abonnement.

```PowerShell
Add-AzureRmAccount
$rgName = 'myRG01'
$s = Select-AzureRmSubscription -SubscriptionName "<Azure Subscription Name>"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID/resourceGroups/$rgName

```

## <a name="policy-in-action"></a>Beleid in actie

Zodra u het Azure-beleid hebt geïmplementeerd, u een foutbericht ontvangt bij het implementeren van een resource die niet toegestaan door het beleid.

![Resultaat van de resource-implementatie is mislukt, omdat de beperking van beleid](./media/azure-stack-policy-module/image1.png)

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)
* [Sjablonen met Azure CLI implementeren](azure-stack-deploy-template-command-line.md)
* [Sjablonen met Visual Studio implementeren](azure-stack-deploy-template-visual-studio.md)
