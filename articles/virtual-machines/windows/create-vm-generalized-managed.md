---
title: Virtuele machine maken vanuit een beheerde-installatiekopie in Azure | Microsoft Docs
description: Een virtuele Windows-machine maken van een algemene beheerde installatiekopie met behulp van Azure PowerShell of de Azure-portal in het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: cynthn
ms.openlocfilehash: 1d543bd9590664e74cff70cf55e8f7bd42f2c6f0
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-vm-from-a-managed-image"></a>Een virtuele machine maken van een begeleide afbeelding

U kunt meerdere virtuele machines maken in een beheerde VM-installatiekopie met PowerShell of de Azure-portal. Een beheerde VM-installatiekopie bevat de benodigde gegevens voor het maken van een virtuele machine, met inbegrip van het besturingssysteem en gegevensschijven. De VHD's die gezamenlijk de afbeelding, met inbegrip van zowel de OS-schijven en eventuele gegevensschijven, als beheerde schijven zijn opgeslagen. 

U moet al hebben [gemaakt van een beheerde VM-installatiekopie](capture-image-resource.md) moet worden gebruikt voor het maken van de nieuwe virtuele machine. 

## <a name="use-the-portal"></a>Gebruik de portal

1. Open de [Azure Portal](https://portal.azure.com).
2. Selecteer **Alle resources** in het menu aan de linkerkant. U kunt de resources door sorteren **Type** afbeeldingen gemakkelijk te vinden.
3. Selecteer de installatiekopie die u wilt gebruiken in de lijst. De installatiekopie van het **overzicht** pagina wordt geopend.
4. Klik op **+ maken VM** in het menu.
5. Geef de informatie van de virtuele machine op. De gebruikersnaam en het wachtwoord die u hier opgeeft, worden gebruikt voor aanmelding bij de virtuele machine. Na het voltooien klikt u op **OK**. U kunt de nieuwe virtuele machine maken in een bestaande groep voor Resrouce of kies **nieuw** om een nieuwe resourcegroep voor het opslaan van de virtuele machine te maken.
6. Selecteer een grootte voor de VM. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. 
7. Onder **instellingen**, zo nodig en klikt u op **OK**. 
8. Op de pagina overzicht ziet u de installatiekopienaam van uw die wordt vermeld voor **persoonlijke installatiekopie**. Klik op **Ok** implementatie van virtuele machine te starten.


## <a name="use-powershell"></a>PowerShell gebruiken

U kunt PowerShell gebruiken voor een virtuele machine maken van een installatiekopie met de vereenvoudigde parameter ingesteld voor de [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm) cmdlet. De installatiekopie moet zich in dezelfde resourcegroep waar u de virtuele machine maken.

Dit voorbeeld vereist dat de AzureRM moduleversie 5.6.0 of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

De vereenvoudigde parameterset voor nieuw-AzureRmVm alleen vereist dat u een naam, de groep en afbeelding bronnaam voor het maken van een virtuele machine van een installatiekopie opgeven, maar wordt gebruikt de waarde van de **-naam** parameter als de naam van alle resources die er het wordt automatisch gemaakt. In dit voorbeeld kunt wij bieden meer gedetailleerde namen voor elk van de resource, maar kunt de cmdlet automatisch worden gemaakt. Ook kunt u bronnen, zoals het virtuele netwerk, tevoren maken en de naam van de doorgegeven aan de cmdlet. De bestaande resources wordt gebruikt als het vindt deze door hun naam.

Het volgende voorbeeld wordt een virtuele machine met de naam *myVMFromImage*, in de *myResourceGroup* resourcegroep van de installatiekopie met de naam *myImage*. 


```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```



## <a name="next-steps"></a>Volgende stappen
Zie voor het beheren van uw nieuwe virtuele machine met Azure PowerShell [maken en beheren van Windows-VM's met de Azure PowerShell-module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

