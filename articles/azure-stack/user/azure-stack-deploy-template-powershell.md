---
title: Sjablonen met behulp van PowerShell in Azure-Stack implementeren | Microsoft Docs
description: Een sjabloon implementeren naar Azure-Stack met behulp van PowerShell.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 4af82deef029120aa2699e7c69c501ae61a1e8bd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34359812"
---
# <a name="deploy-a-template-to-azure-stack-using-powershell"></a>Een sjabloon implementeren naar Azure-Stack met behulp van PowerShell

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt PowerShell gebruiken voor het implementeren van Azure Resource Manager-sjablonen op Azure-Stack. In dit artikel leest u hoe u PowerShell gebruikt om een sjabloon te implementeren.

## <a name="run-azurerm-powershell-cmdlets"></a>Voer AzureRM PowerShell-cmdlets

Dit voorbeeld wordt AzureRM PowerShell-cmdlets en een sjabloon die is opgeslagen op GitHub. De sjabloon maakt u een virtuele machine van Windows Server 2012 R2 Datacenter.

>[!NOTE]
>Voordat u dit voorbeeld, zorg ervoor dat u bent [geconfigureerd PowerShell](azure-stack-powershell-configure-user.md) voor een Azure-Stack-gebruiker.

1. Ga naar <http://aka.ms/AzureStackGitHub> en zoek de **101-eenvoudige-windows-vm** sjabloon. De sjabloon opslaan op deze locatie: C:\\sjablonen\\azuredeploy-101-eenvoudige-windows-vm.json.
2. Open een opdrachtprompt met verhoogde bevoegdheid PowerShell.
3. Vervang *gebruikersnaam* en *wachtwoord* in het volgende script met uw gebruikersnaam en wachtwoord en voer het script.

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
   >Telkens wanneer u dit script uitvoert Verhoog de waarde van de parameter '$myNum' om te voorkomen dat uw implementatie wordt overschreven.

4. Open de Stack van Azure portal, selecteer **Bladeren**, en selecteer vervolgens **virtuele machines** uw nieuwe virtuele machine vinden (*myDeployment001*).

## <a name="next-steps"></a>Volgende stappen

[Sjablonen implementeren met Visual Studio](azure-stack-deploy-template-visual-studio.md)
