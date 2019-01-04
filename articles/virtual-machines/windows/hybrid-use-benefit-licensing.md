---
title: Azure Hybrid Benefit voor WindowsServer | Microsoft Docs
description: Meer informatie over het optimaliseren van uw Windows Software Assurance benefits om on-premises licenties naar Azure
services: virtual-machines-windows
documentationcenter: ''
author: xujing
manager: jeconnoc
editor: ''
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 4/22/2018
ms.author: xujing-ms
ms.openlocfilehash: 757891a6968313c1a31ce67b399d6aa052abfa21
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53999948"
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>Azure Hybrid Benefit voor Windows Server
Voor klanten met Software Assurance kunt Azure Hybrid Benefit voor Windows Server u uw on-premises Windows Server-licenties en het uitvoeren van Windows virtuele machines op Azure tegen lagere kosten. U kunt Azure Hybrid Benefit voor Windows Server gebruiken voor het implementeren van nieuwe virtuele machines met Windows-besturingssysteem. In dit artikel gaat over de stappen voor het implementeren van nieuwe virtuele machines met Azure Hybrid Benefit voor Windows Server en hoe u kunt bijwerken bestaande virtuele machines uitvoeren. Zie voor meer informatie over Azure Hybrid Benefit voor Windows Server licentie- en kosten besparen, de [Azure Hybrid Benefit voor Windows Server-licenties pagina](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

> [!Important]
> Elke licentie voor twee processors of elke set licenties voor 16 kerngeheugens geeft recht op twee exemplaren met maximaal 8 kerngeheugens of één exemplaar met maximaal 16 kerngeheugens. Het licentierecht voor de Standard Edition van Azure Hybrid Benefit kan slechts één keer worden gebruikt (on-premises of in Azure). Dankzij de voordelen van de Datacenter Edition kan het systeem gelijktijdig on-premises en in Azure worden gebruikt.
>

> [!Important]
> Met behulp van Azure Hybrid Benefit voor Windows Server met virtuele machines waarop Windows Server-besturingssysteem wordt uitgevoerd, worden nu ondersteund in alle regio's, met inbegrip van virtuele machines met aanvullende software zoals SQL Server- of software van derden marketplace. 
>

> [!NOTE]
> Voor klassieke virtuele machines, wordt alleen implementeren van nieuwe VM op basis van aangepaste installatiekopieën van on-premises ondersteund. Als u wilt profiteren van de mogelijkheden die in dit artikel wordt ondersteund, moet u eerst een klassieke virtuele machines migreren naar Resource Manager-model.
>


## <a name="ways-to-use-azure-hybrid-benefit-for-windows-server"></a>Manieren waarop u met Azure Hybrid Benefit voor Windows Server
Er zijn verschillende manieren Windows virtual machines met Azure Hybrid Benefit gebruiken:

1. U kunt VM's implementeren vanaf een van de opgegeven [Windows Server-installatiekopieën op Azure Marketplace](# https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview)
2. U kunt een aangepaste VM uploaden en implementeren met behulp van een Resource Manager-sjabloon of Azure PowerShell
3. U kunt in-/ uitschakelen en converteren van bestaande virtuele machine tussen actieve met Azure Hybrid Benefit of op aanvraag kosten te betalen voor Windows Server
4. U kunt ook Azure Hybrid Benefit voor Windows Server op virtuele-machineschaalset ook toepassen


## <a name="create-a-vm-with-azure-hybrid-benefit-for-windows-server"></a>Een virtuele machine maken met Azure Hybrid Benefit voor WindowsServer
Alle Windows Server-besturingssysteem op basis van installatiekopieën worden ondersteund voor Azure Hybrid Benefit voor Windows Server. U kunt ondersteuning voor installatiekopieën van Azure-platform gebruiken of uw eigen aangepaste installatiekopieën van Windows Server uploaden. 

### <a name="portal"></a>Portal
Voor het maken van een virtuele machine met Azure Hybrid Benefit voor Windows Server, gebruikt u de wisselknop onder de sectie "Geld besparen".

### <a name="powershell"></a>PowerShell
```powershell
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -ImageName "Win2016Datacenter" `
    -LicenseType "Windows_Server"
```

### <a name="cli"></a>CLI
```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --license-type Windows_Server
```

### <a name="template"></a>Template
In de Resource Manager-sjablonen, een extra parameter `licenseType` moet worden opgegeven. U kunt meer lezen over [Azure Resource Manager-sjablonen](../../resource-group-authoring-templates.md)
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server"></a>Converteren van een bestaande virtuele machine met behulp van Azure Hybrid Benefit voor Windows Server
Als u een bestaande virtuele machine die u wilt converteren om te profiteren van Azure Hybrid Benefit voor Windows Server hebt, kunt u het licentietype van de virtuele machine bijwerken door de onderstaande instructies te volgen.

> [!NOTE]
> Wijzigen van het licentietype op de virtuele machine wordt niet het systeem opnieuw opstarten of een interuption service veroorzaken.  Het is gewoon een update aan voor een markering voor metagegevens.
> 

### <a name="portal"></a>Portal
Vanuit de portal VM-blade kunt u bijwerken van de virtuele machine voor het gebruik van Azure Hybrid Benefit door "Configuratie" optie te selecteren en stel de optie "Azure hybrid use benefit"

### <a name="powershell"></a>PowerShell
- Converteren van bestaande Windows Server-VM's naar Azure Hybrid Benefit voor Windows Server

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
    $vm.LicenseType = "Windows_Server"
    Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
    ```
    
- Windows Server virtuele machines converteren met voordeel terug naar betalen per gebruik

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
    $vm.LicenseType = "None"
    Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
    ```
    
### <a name="cli"></a>CLI
- Converteren van bestaande Windows Server-VM's naar Azure Hybrid Benefit voor Windows Server

    ```azurecli
    az vm update --resource-group myResourceGroup --name myVM --set licenseType=Windows_Server
    ```

### <a name="how-to-verify-your-vm-is-utilizing-the-licensing-benefit"></a>Het controleren van uw virtuele machine met behulp van de licentievoordeel
Nadat u uw virtuele machine via een van beide PowerShell Resource Manager-sjabloon of -portal hebt geïmplementeerd, kunt u de instelling in de volgende methoden kunt controleren.

### <a name="portal"></a>Portal
U kunt vanuit de portal VM-blade de wisselknop weergeven voor Azure Hybrid Benefit voor Windows Server door "Configuratie" tabblad te selecteren.

### <a name="powershell"></a>PowerShell
Het volgende voorbeeld ziet het licentietype voor een enkele virtuele machine
```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Uitvoer:
```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Dit anders uitvoer met de volgende virtuele machine geïmplementeerd zonder Azure Hybrid Benefit voor Windows Server-licentieverlening:
```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

### <a name="cli"></a>CLI
```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVM --query "[?licenseType=='Windows_Server']" -o table
```

> [!NOTE]
> Wijzigen van het licentietype op de virtuele machine wordt niet het systeem opnieuw opstarten of een interuption service veroorzaken. Het is een metagegevens markering alleen-licentieverlening.
>

## <a name="list-all-vms-with-azure-hybrid-benefit-for-windows-server-in-a-subscription"></a>Lijst van alle virtuele machines met Azure Hybrid Benefit voor Windows Server in een abonnement
Als u wilt zien en aantal van alle virtuele machines geïmplementeerd met Azure Hybrid Benefit voor Windows Server, kunt u de volgende opdracht uit uw abonnement uitvoeren:

### <a name="portal"></a>Portal
De virtuele Machine of virtuele machine scale sets resourceblade, kunt u een lijst met al uw VM (s) en type licentieverlening bekijken door het configureren van de tabelkolom om op te nemen 'Azure Hybrid Benefit'. De instelling van de virtuele machine kan worden toegevoegd 'Enabled', staat "Niet is ingeschakeld" of 'Wordt niet ondersteund'.

### <a name="powershell"></a>PowerShell
```powershell
$vms = Get-AzureRMVM 
$vms | ?{$_.LicenseType -like "Windows_Server"} | select ResourceGroupName, Name, LicenseType
```

### <a name="cli"></a>CLI
```azurecli
az vm list --query "[?licenseType=='Windows_Server']" -o table
```

## <a name="deploy-a-virtual-machine-scale-set-with-azure-hybrid-benefit-for-windows-server"></a>Een virtuele-Machineschaalset met Azure Hybrid Benefit voor WindowsServer implementeren
Binnen uw virtuele machine schaalset Resource Manager-sjablonen, een extra parameter `licenseType` binnen uw VirtualMachineProfile-eigenschap moet worden opgegeven. U kunt dit doen tijdens het maken of bijwerken voor uw schaalset via ARM-sjabloon, Powershell, Azure CLI of REST.

Het volgende voorbeeld wordt een ARM-sjabloon met een Windows Server 2016 Datacenter-installatiekopie:
```json
"virtualMachineProfile": {
    "storageProfile": {
        "osDisk": {
            "createOption": "FromImage"
        },
        "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2016-Datacenter",
            "version": "latest"
        }
    },
    "licenseType": "Windows_Server",
    "osProfile": {
            "computerNamePrefix": "[parameters('vmssName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
    }
```
U kunt ook meer informatie over het [wijzigen van een virtuele-machineschaalset](../../virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set.md) voor meer manieren om bij te werken uw schaal ingesteld.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [hoe u geld besparen met Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/)
- Meer informatie over [Veelgestelde vragen over Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/faq/)
- Meer informatie over [Azure Hybrid Benefit voor Windows Server-licentieverlening gedetailleerde richtlijnen](https://docs.microsoft.com/windows-server/get-started/azure-hybrid-benefit)
- Meer informatie over [Azure Hybrid Benefit voor Windows Server en Azure Site Recovery kunt u toepassingen te migreren naar Azure nog voordeliger](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/)
- Meer informatie over [Windows 10 op Azure met Multitenant Hosting rechts](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment)
- Meer informatie over [met behulp van Resource Manager-sjablonen](../../azure-resource-manager/resource-group-overview.md)
