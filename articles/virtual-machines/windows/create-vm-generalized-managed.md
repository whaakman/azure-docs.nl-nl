---
title: Virtuele machine maken vanaf een beheerde installatiekopie in Azure | Microsoft Docs
description: Een Windows-machine maken vanaf een gegeneraliseerde beheerde installatiekopie met behulp van Azure PowerShell of de Azure-portal, in het Resource Manager-implementatiemodel.
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
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: 8acbb33b396aa617936eb0333bd68fea60532425
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47404653"
---
# <a name="create-a-vm-from-a-managed-image"></a>Een virtuele machine van een beheerde installatiekopie maken

U kunt meerdere virtuele machines (VM's) maken van een virtuele machine van Azure beheerde installatiekopieën, met behulp van de Azure portal of PowerShell. Een beheerde VM-installatiekopie bevat de informatie die nodig zijn voor het maken van een virtuele machine, met inbegrip van het besturingssysteem en gegevensschijven. De virtuele harde schijven (VHD's) die gezamenlijk de afbeelding, met inbegrip van zowel de besturingssysteemschijven en eventuele gegevensschijven worden opgeslagen als beheerde schijven. 

Voordat u een nieuwe virtuele machine maakt, moet u [maken van een beheerde VM-installatiekopie](capture-image-resource.md) om te gebruiken als de bronafbeelding. 

## <a name="use-the-portal"></a>Gebruik de portal

1. Open de [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu links **alle resources**. U kunt de resources door sorteren **Type** eenvoudig uw installatiekopieën vinden.
3. Selecteer de installatiekopie die u wilt gebruiken in de lijst. De installatiekopie van het **overzicht** pagina wordt geopend.
4. Selecteer **maken VM** in het menu.
5. Geef de informatie van de virtuele machine op. De gebruikersnaam en wachtwoord die u hier opgeeft wordt gebruikt voor aanmelding bij de virtuele machine. Als u klaar bent, selecteert u **OK**. U kunt de nieuwe virtuele machine maken in een bestaande resourcegroep of kies **nieuw** te maken van een nieuwe resourcegroep voor het opslaan van de virtuele machine.
6. Selecteer een grootte voor de VM. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. 
7. Onder **instellingen**, wijzig deze indien nodig, en selecteer **OK**. 
8. Op de pagina overzicht ziet u de installatiekopienaam van uw vermeld als een **persoonlijke installatiekopie**. Selecteer **Ok** implementatie van virtuele machine te starten.


## <a name="use-powershell"></a>PowerShell gebruiken

U kunt PowerShell gebruiken op een virtuele machine maken van een installatiekopie met behulp van de vereenvoudigde parameter is ingesteld voor de [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm) cmdlet. De installatiekopie moet zich in dezelfde resourcegroep bevinden waar u de virtuele machine kunt maken.

Dit voorbeeld vereist dat de AzureRM-moduleversie 5.6.0 of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

De vereenvoudigde parameterset voor [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm) alleen is vereist dat u een naam, resourcegroep en de naam van installatiekopie naar een virtuele machine maken van een installatiekopie opgeven. New-AzureRmVm gebruikt de waarde van de **-naam** parameter als de naam van alle resources die deze automatisch worden gemaakt. In dit voorbeeld wordt er meer gedetailleerde namen voor elk van de resources, maar laat de cmdlet deze automatisch worden gemaakt. U kunt ook resources vooraf, zoals het virtuele netwerk maken en de naam van de resource doorgeven aan de cmdlet. New-AzureRmVm gebruikt de bestaande resources als het vindt deze door hun naam.

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
[Maken en beheren van Windows-VM's met de Azure PowerShell-module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

