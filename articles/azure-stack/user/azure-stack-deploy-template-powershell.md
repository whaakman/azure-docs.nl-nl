---
title: Sjablonen implementeren met behulp van PowerShell in Azure Stack | Microsoft Docs
description: Een sjabloon implementeren in Azure Stack met behulp van PowerShell.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 445628679a09a1884f63cdce446adec476af39af
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42058429"
---
# <a name="deploy-a-template-to-azure-stack-using-powershell"></a>Een sjabloon implementeren in Azure Stack met behulp van PowerShell

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt PowerShell gebruiken om te implementeren van Azure Resource Manager-sjablonen voor Azure Stack. Dit artikel leest u hoe u PowerShell gebruikt om een sjabloon te implementeren.

## <a name="run-azurerm-powershell-cmdlets"></a>Voer AzureRM PowerShell-cmdlets

Dit voorbeeld wordt de AzureRM PowerShell-cmdlets en een op GitHub opgeslagen sjabloon. De sjabloon maakt u een virtuele machine van Windows Server 2012 R2 Datacenter.

>[!NOTE]
>Voordat u dit voorbeeld probeert, zorgt u ervoor dat u hebt [geconfigureerd PowerShell](azure-stack-powershell-configure-user.md) voor een Azure Stack-gebruiker.

1. Ga naar <http://aka.ms/AzureStackGitHub> en zoek de **101-eenvoudig-windows-vm** sjabloon. De sjabloon opslaan in deze locatie: C:\\sjablonen\\azuredeploy-101-eenvoudig-windows-vm.json.
2. Open een PowerShell-opdrachtprompt met verhoogde bevoegdheid.
3. Vervang *gebruikersnaam* en *wachtwoord* in het volgende script met uw gebruikersnaam en wachtwoord, en voer het script.

   ```PowerShell
       # Set Deployment Variables
       $myNum = "001" #Modify this per deployment
       $RGName = "myRG$myNum"
       $myLocation = "local"
   
       # Create Resource Group for Template Deployment
       New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
       # Deploy Simple IaaS Template
       New-AzureRmResourceGroupDeployment `
           -Name myDeployment$myNum `
           -ResourceGroupName $RGName `
           -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
           -NewStorageAccountName mystorage$myNum `
           -DnsNameForPublicIP mydns$myNum `
           -AdminUsername <username> `
           -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
           -VmName myVM$myNum `
           -WindowsOSVersion 2012-R2-Datacenter
   ```

   >[!IMPORTANT]
   >Wanneer u dit script uitvoeren Verhoog de waarde van de parameter '$myNum' om te voorkomen dat uw implementatie wordt overschreven.

4. Open de Azure Stack-portal, selecteer **Bladeren**, en selecteer vervolgens **virtuele machines** te vinden van uw nieuwe virtuele machine (*myDeployment001*).

## <a name="next-steps"></a>Volgende stappen

[Sjablonen implementeren met Visual Studio](azure-stack-deploy-template-visual-studio.md)
