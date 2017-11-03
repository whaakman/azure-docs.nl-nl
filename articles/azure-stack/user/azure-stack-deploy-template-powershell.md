---
title: Sjablonen met PowerShell in Azure Stack implementeren | Microsoft Docs
description: Informatie over het implementeren van een virtuele machine met een Resource Manager-sjabloon en PowerShell.
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: e4837be016b569dbd0b4bf8e071e6381b8daa85f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Sjablonen in Azure-Stack met behulp van PowerShell implementeren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

PowerShell gebruiken voor het implementeren van Azure Resource Manager-sjablonen op de Azure-Stack Development Kit.  Resource Manager-sjablonen implementeren en inrichten van alle resources voor uw toepassing in een enkele, gecoördineerde bewerking.

## <a name="run-azurerm-powershell-cmdlets"></a>Voer AzureRM PowerShell-cmdlets
In dit voorbeeld wordt u een script uitvoeren om een virtuele machine implementeren in Azure Stack Development Kit met een Resource Manager-sjabloon.  Voordat u doorgaat, moet u hebben [PowerShell geconfigureerd](azure-stack-powershell-configure-user.md)  

De VHD die is gebruikt in deze voorbeeldsjabloon is Windows Server 2012-R2 Datacenter.

1. Ga naar <http://aka.ms/AzureStackGitHub>, zoekt de **101-eenvoudige-windows-vm** -sjabloon en sla het op de volgende locatie: c:\\sjablonen\\ azuredeploy-101-eenvoudige-windows-vm.json.
2. Voer het volgende script voor implementatie in PowerShell. Vervang *gebruikersnaam* en *wachtwoord* met uw gebruikersnaam en wachtwoord. Op de volgende gebruikt Verhoog de waarde voor de *$myNum* parameter om te voorkomen dat uw implementatie wordt overschreven.
   
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
3. Open de Stack van Azure portal op **Bladeren**, klikt u op **virtuele machines**, en zoek naar de nieuwe virtuele machine (*myDeployment001*).


## <a name="next-steps"></a>Volgende stappen
[Sjablonen implementeren met Visual Studio](azure-stack-deploy-template-visual-studio.md)

