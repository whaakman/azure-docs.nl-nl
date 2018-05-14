---
title: Hybride Azure-voordeel voor WindowsServer | Microsoft Docs
description: Meer informatie over het maximaliseren van uw Windows Software Assurance voordelen voor het maken van lokale licenties naar Azure
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
ms.openlocfilehash: a4b0baefc8c3c839a06d6540e57b34657138c8ff
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>Azure Hybrid Benefit voor Windows Server
Voor klanten met Software Assurance kunt Azure hybride voordeel voor Windows Server u uw lokale Windows Server-licenties en voer Windows virtuele machines in Azure tegen lagere kosten. U kunt Azure hybride voordeel voor Windows Server gebruiken voor het implementeren van nieuwe virtuele machines met het Windows-besturingssysteem. In dit artikel gaat over de stappen voor het implementeren van nieuwe virtuele machines met Azure hybride voordeel voor Windows Server en hoe u kunt bijwerken bestaande VM's worden uitgevoerd. Zie voor meer informatie over Azure hybride voordeel voor Windows Server licentieverlening en kosten besparingen, de [Azure hybride voordeel voor Windows Server-licentieverlening pagina](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

> [!Important]
> Elke licentie voor twee processors of elke set licenties voor 16 kerngeheugens geeft recht op twee exemplaren met maximaal 8 kerngeheugens of één exemplaar met maximaal 16 kerngeheugens. Het licentierecht voor de Standard Edition van Azure Hybrid Benefit kan slechts één keer worden gebruikt (on-premises of in Azure). Dankzij de voordelen van de Datacenter Edition kan het systeem gelijktijdig on-premises en in Azure worden gebruikt.
>

> [!Important]
> Met behulp van Azure hybride voordeel voor Windows Server met alle virtuele machines met Windows Server-besturingssysteem worden nu ondersteund in alle regio's, met inbegrip van virtuele machines met aanvullende software zoals SQL Server of van derden marketplace-software. 
>

> [!NOTE]
> Voor klassieke VM's wordt alleen implementeren van nieuwe virtuele machine van de aangepaste installatiekopieën on-premises ondersteund. Om te profiteren van de mogelijkheden die in dit artikel wordt ondersteund, moet u eerst klassieke virtuele machines migreren naar Resource Manager-model.
>


## <a name="ways-to-use-azure-hybrid-benefit-for-windows-server"></a>Manieren waarop u met Azure hybride voordeel voor Windows Server
Er zijn enkele manieren voor gebruik van virtuele machines van Windows met het voordeel van de hybride Azure:

1. U kunt virtuele machines van een van de opgegeven implementeren [installatiekopieën van Windows Server op Azure Marketplace](#https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview)
2. U kunt een aangepaste VM uploaden en implementeren met behulp van een Resource Manager-sjabloon of Azure PowerShell
3. U kunt in-of uitschakelen en een bestaande VM tussen uitgevoerd met Azure hybride voordeel converteren of op aanvraag kosten betalen voor Windows Server
4. U kunt ook Azure hybride voordeel voor Windows Server toepassen op virtuele-machineschaalset ook instellen


## <a name="create-a-vm-with-azure-hybrid-benefit-for-windows-server"></a>Een virtuele machine maken met Azure hybride-voordeel voor WindowsServer
Alle Windows Server-besturingssysteem op basis van installatiekopieën worden ondersteund voor Azure hybride voordeel voor Windows Server. U kunt installatiekopieën voor ondersteuning van Azure-platform gebruiken of uw eigen aangepaste installatiekopieën van Windows Server uploaden. 

### <a name="portal"></a>Portal
Als een virtuele machine maken met Azure hybride voordeel voor Windows Server, gebruikt u de wisselknop onder de sectie 'Geld besparen'.

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
In de Resource Manager-sjablonen, een extra parameter `licenseType` moet worden opgegeven. U kunt meer lezen over [Azure Resource Manager-sjablonen ontwerpen](../../resource-group-authoring-templates.md)
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server"></a>Converteren van een bestaande virtuele machine met behulp van Azure hybride voordeel voor Windows Server
Als u een bestaande virtuele machine die u converteren wilt om te profiteren van Azure hybride voordeel voor Windows Server hebt, kunt u uw VM licentietype als volgt bijwerken:

### <a name="portal"></a>Portal
Vanuit de portal VM-blade kunt u bijwerken van de virtuele machine voor het gebruik van Azure hybride profiteren door "Configuratie" optie te selecteren en de optie 'hybride Azure profiteren' in-of uitschakelen

### <a name="powershell"></a>PowerShell
- Bestaande Windows Server-VM's converteren naar Azure hybride voordeel voor Windows Server

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
    $vm.LicenseType = "Windows_Server"
    Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
    ```
    
- Converteren van Windows Server-VM's met voordeel terug naar betalen per gebruik

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
    $vm.LicenseType = "None"
    Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
    ```
    
### <a name="cli"></a>CLI
- Bestaande Windows Server-VM's converteren naar Azure hybride voordeel voor Windows Server

    ```azurecli
    az vm update --resource-group myResourceGroup --name myVM --set licenseType=Windows_Server
    ```
    
### <a name="how-to-verify-your-vm-is-utilizing-the-licensing-benefit"></a>Het controleren van uw virtuele machine wordt gebruikt door de licentiegegevens benefit
Nadat u uw virtuele machine via de PowerShell, Resource Manager-sjabloon of -portal hebt geïmplementeerd, kunt u de instelling in de volgende methoden kunt controleren.

### <a name="portal"></a>Portal
U kunt vanuit de portal VM blade de wisselknop weergeven voor Azure hybride voordeel voor Windows Server door "Configuratie" tabblad te selecteren.

### <a name="powershell"></a>PowerShell
Het volgende voorbeeld ziet het licentietype voor een enkele VM
```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Uitvoer:
```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Dit is anders dan uitvoer met de volgende virtuele machine geïmplementeerd zonder Azure hybride voordeel voor Windows Server-licentieverlening:
```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

### <a name="cli"></a>CLI
```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVM --query '[?licenseType==Windows_Server]' -o table
```

## <a name="list-all-vms-with-azure-hybrid-benefit-for-windows-server-in-a-subscription"></a>Lijst van alle virtuele machines in Azure hybride voordeel voor Windows Server in een abonnement
Als u wilt zien en tellen van alle virtuele machines die zijn geïmplementeerd met Azure hybride voordeel voor Windows Server, kunt u de volgende opdracht uitvoeren uit uw abonnement:

### <a name="portal"></a>Portal
Vanaf de virtuele Machine of virtuele machine scale sets, kunt u een lijst met alle VM('s) en type licentieverlening weergeven door het configureren van de tabelkolom zodanig 'Azure hybride voordeel'. De VM-instelling kan zijn in 'Enabled', 'Niet is ingeschakeld' of 'Wordt niet ondersteund' vermelden.

### <a name="powershell"></a>PowerShell
```powershell
$vms = Get-AzureRMVM 
$vms | ?{$_.LicenseType -like "Windows_Server"} | select ResourceGroupName, Name, LicenseType
```

### <a name="cli"></a>CLI
```azurecli
az vm list --query '[?licenseType==Windows_Server]' -o table
```

## <a name="deploy-a-virtual-machine-scale-set-with-azure-hybrid-benefit-for-windows-server"></a>Een virtuele-Machineschaalset die u met Azure hybride-voordeel voor WindowsServer implementeren
Binnen uw virtuele machine schaalset Resource Manager-sjablonen, een extra parameter `licenseType` binnen uw VirtualMachineProfile-eigenschap moet worden opgegeven. U kunt dit doen tijdens het maken of bijwerken voor uw scale ingesteld via de ARM-sjabloon, Powershell, Azure CLI of REST.

Het volgende voorbeeld wordt de ARM-sjabloon met een installatiekopie van het Windows Server 2016 Datacenter:
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
U kunt ook meer informatie over het [wijzigen van een virtuele-machineschaalset](../../virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set.md) voor meer manieren om bij te werken van uw scale ingesteld.

## <a name="next-steps"></a>Volgende stappen
- Lees meer over [hoe u geld besparen en de schaalvoordelen van Azure hybride](https://azure.microsoft.com/pricing/hybrid-use-benefit/)
- Lees meer over [Veelgestelde vragen over Azure hybride Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/faq/)
- Meer informatie over [Azure hybride voordeel voor Windows Server-licentieverlening gedetailleerde richtlijnen](https://docs.microsoft.com/windows-server/get-started/azure-hybrid-benefit)
- Meer informatie over [Azure hybride voordeel voor Windows Server en Azure Site Recovery toepassingen maken voor migratie naar Azure nog meer rendabele](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/)
- Meer informatie over [Windows 10 in Azure met Multitenant Hosting-rechts](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment)
- Meer informatie over [met behulp van Resource Manager-sjablonen](../../azure-resource-manager/resource-group-overview.md)
