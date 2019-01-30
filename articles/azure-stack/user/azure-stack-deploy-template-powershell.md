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
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 7af06347dd1ca5a3e7e27e5db4849b5540b106b9
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55250007"
---
# <a name="deploy-a-template-to-azure-stack-using-powershell"></a>Een sjabloon implementeren in Azure Stack met behulp van PowerShell

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt PowerShell gebruiken om te implementeren van Azure Resource Manager-sjablonen voor Azure Stack. In dit artikel wordt beschreven hoe u PowerShell gebruikt om een sjabloon te implementeren.

## <a name="run-azurerm-powershell-cmdlets"></a>Voer AzureRM PowerShell-cmdlets

In dit voorbeeld wordt **AzureRM** PowerShell-cmdlets en een sjabloon die zijn opgeslagen op GitHub. De sjabloon maakt u een virtuele machine van Windows Server 2012 R2 Datacenter.

>[!NOTE]
>Voordat u dit voorbeeld probeert, zorgt u ervoor dat u hebt [geconfigureerd PowerShell](azure-stack-powershell-configure-user.md) voor een Azure Stack-gebruiker.

1. Ga naar [ https://aka.ms/AzureStackGitHub ](https://aka.ms/AzureStackGitHub) en zoek de **101-eenvoudig-windows-vm** sjabloon. De sjabloon opslaan in deze locatie: `C:\templates\azuredeploy-101-simple-windows-vm.json`.
2. Open een PowerShell-opdrachtprompt met verhoogde bevoegdheid.
3. Vervang `username` en `password` in het volgende script met uw gebruikersnaam en wachtwoord, en voer het script:

    ```PowerShell
    # Set deployment variables
    $myNum = "001" # Modify this per deployment
    $RGName = "myRG$myNum"
    $myLocation = "local"
   
    # Create resource group for template deployment
    New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
    # Deploy simple IaaS template
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
    >Telkens wanneer u dit script uitvoert, verhoog de waarde van de `$myNum` parameter om te voorkomen dat uw implementatie wordt overschreven.

4. Open de Azure Stack-portal, selecteer **Bladeren**, en selecteer vervolgens **virtuele machines** te vinden van uw nieuwe virtuele machine (**myDeployment001**).

## <a name="next-steps"></a>Volgende stappen

- [Sjablonen implementeren met Visual Studio](azure-stack-deploy-template-visual-studio.md)
