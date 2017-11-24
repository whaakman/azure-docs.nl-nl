---
title: Hybride Azure-voordeel voor WindowsServer | Microsoft Docs
description: Meer informatie over het maximaliseren van uw Windows Software Assurance voordelen voor het maken van lokale licenties naar Azure
services: virtual-machines-windows
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/22/2017
ms.author: kmouss
ms.openlocfilehash: c2b406530aec60299ea2db38ad9e34895fe36dcd
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>Azure Hybrid Benefit voor Windows Server
Voor klanten met Software Assurance kunt Azure hybride voordeel voor Windows Server u uw lokale Windows Server-licenties en voer Windows virtuele machines in Azure tegen lagere kosten. U kunt Azure hybride voordeel voor Windows Server gebruiken voor het implementeren van nieuwe virtuele machines vanaf elke Azure ondersteund platforminstallatiekopie voor Windows Server of Windows aangepaste installatiekopieën. In dit artikel gaat over de stappen voor het implementeren van nieuwe virtuele machines met Azure hybride voordeel voor Windows Server en hoe u kunt bijwerken bestaande VM's worden uitgevoerd. Zie voor meer informatie over Azure hybride voordeel voor Windows Server licentieverlening en kosten besparingen, de [Azure hybride voordeel voor Windows Server-licentieverlening pagina](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

> [!IMPORTANT]
> De oude [HUB] Windows Server-installatiekopieën die zijn gepubliceerd voor klanten met Enterprise Agreement op Azure Marketplace is buiten gebruik gesteld vanaf 11-9/2017, gebruikt u de standaard Windows-Server met de optie 'Geld opslaan' op de portal voor Azure hybride Benefit voor WindowsServer. Voor meer informatie raadpleegt u dit [artikel.](https://support.microsoft.com/en-us/help/4036360/retirement-azure-hybrid-use-benefit-images-for-ea-subscriptions)
>

> [!NOTE]
> Met behulp van Azure hybride voordeel voor Windows Server met virtuele machines die worden in rekening gebracht voor aanvullende software zoals SQL Server of een van de derde partij marketplace-installatiekopieën is uit teruggedraaid. Als u, zoals een 409 fout krijgt: het wijzigen van de eigenschap 'LicenseType' is niet toegestaan; u probeert vervolgens te converteren of implementeren van een nieuwe versie van Windows Server VM met aanvullende software kosten, die mogelijk niet ondersteund in deze regio. Hetzelfde als u probeert te zoeken voor de configuratie van de portal optie om de conversie en u niet zien voor die VM.
>


> [!NOTE]
> Voor klassieke VM's wordt alleen implementeren van nieuwe virtuele machine van de aangepaste installatiekopieën on-premises ondersteund. Om te profiteren van de mogelijkheden die in dit artikel wordt ondersteund, moet u eerst klassieke virtuele machines migreren naar Resource Manager-model.
>


## <a name="ways-to-use-azure-hybrid-benefit-for-windows-server"></a>Manieren waarop u met Azure hybride voordeel voor Windows Server
Er zijn enkele manieren voor gebruik van virtuele machines van Windows met het voordeel van de hybride Azure:

1. U kunt virtuele machines van een van de opgegeven implementeren [installatiekopieën van Windows Server op Azure Marketplace](#https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview)
2. U kunt [uploaden van een aangepaste VM](#upload-a-windows-vhd) en [implementeren met behulp van een Resource Manager-sjabloon](#deploy-a-vm-via-resource-manager) of [Azure PowerShell](#detailed-powershell-deployment-walkthrough)
3. U kunt in-of uitschakelen en een bestaande VM tussen uitgevoerd met Azure hybride voordeel converteren of op aanvraag kosten betalen voor Windows Server
4. U kunt ook een nieuwe virtuele-machineschaalset ingesteld met Azure hybride voordeel voor Windows Server implementeren

> [!NOTE]
> Converteren van een bestaande virtuele-machineschaalset ingesteld voor gebruik van Azure hybride voordeel voor Windows Server wordt niet ondersteund
>

## <a name="deploy-a-vm-from-a-windows-server-marketplace-image"></a>Een virtuele machine vanuit een Marketplace-installatiekopie van Windows Server implementeren
Alle installatiekopieën van Windows Server die beschikbaar vanuit Azure Marketplace zijn zijn ingeschakeld met Azure hybride voordeel voor Windows Server. Bijvoorbeeld, Windows Server 2016, Windows Server 2012R2, Windows Server 2012 en Windows Server 2008SP1 en meer. U kunt deze installatiekopieën gebruiken voor het implementeren van virtuele machines rechtstreeks vanuit de Azure-portal, Resource Manager-sjablonen, Azure PowerShell of andere SDK.

U kunt deze installatiekopieën rechtstreeks vanuit de Azure portal kunt implementeren. Voor gebruik in het Resource Manager-sjablonen en met Azure PowerShell, moet u de lijst met afbeeldingen als volgt bekijken:

### <a name="powershell"></a>PowerShell
```powershell
Get-AzureRmVMImagesku -Location westus -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```
U kunt de stappen te volgen [virtuele Windows-machine maken met PowerShell](#https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-powershell?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) en LicenseType doorgeven = 'Windows_Server'. Deze optie kunt u uw bestaande Windows Server-licentie gebruiken in Azure.

### <a name="portal"></a>Portal
U kunt de stappen te volgen [virtuele Windows-machine maken met de Azure portal](#https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal) en selecteer de optie voor het gebruik van uw bestaande Windows Server-licentie.

## <a name="convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server"></a>Converteren van een bestaande virtuele machine met behulp van Azure hybride voordeel voor Windows Server
Als u een bestaande virtuele machine die u converteren wilt om te profiteren van Azure hybride voordeel voor Windows Server hebt, kunt u uw VM licentietype als volgt bijwerken:

### <a name="convert-to-using-azure-hybrid-benefit-for-windows-server"></a>Converteren naar het gebruik van Azure hybride voordeel voor Windows Server
```powershell
$vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
$vm.LicenseType = "Windows_Server"
Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
```

### <a name="convert-back-to-pay-as-you-go"></a>Converteren naar vorige omslagstelsel
```powershell
$vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
$vm.LicenseType = "None"
Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
```

### <a name="portal"></a>Portal
Vanuit de portal VM-blade kunt u bijwerken van de virtuele machine voor het gebruik van Azure hybride profiteren door "Configuratie" optie te selecteren en de optie 'hybride Azure profiteren' in-of uitschakelen

> [!NOTE]
> Als u de optie voor het uitschakelen van 'Azure hybride voordeel' onder 'Configuratie' niet ziet, is het omdat de conversie wordt nog niet ondersteund voor het geselecteerde VM-type (bijvoorbeeld een virtuele machine van de aangepaste installatiekopie of van een afbeelding met extra betaald software, zoals SQL Server ingebouwd of Azure Marketplace software van derden).
>

## <a name="upload-a-windows-server-vhd"></a>Een WindowsServer-VHD uploaden
Voor het implementeren van een Windows Server-VM in Azure, moet u eerst een VHD met uw Windows-build base maken. Deze VHD moet op de juiste wijze worden voorbereid met Sysprep voordat u de gegevens naar Azure uploaden. U kunt [voor meer informatie over de vereisten van de VHD en de Sysprep-proces](upload-generalized-managed.md) en [Sysprep-ondersteuning voor serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Back-up van de virtuele machine voordat Sysprep wordt uitgevoerd. 

Eenmaal u uw VHD hebt voorbereid, de harde schijf geüpload naar uw Azure-opslagaccount als volgt:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> Microsoft SQL Server, SharePoint Server en Dynamics kan ook gebruikmaken van uw Software Assurance-licentieverlening. U moet wordt de installatiekopie van het Windows Server voorbereiden door het installeren van onderdelen van uw toepassing en licentiesleutels dienovereenkomstig bieden, en vervolgens de schijfimage uploaden naar Azure. Bekijk de relevante documentatie voor het uitvoeren van Sysprep met uw toepassing, zoals [overwegingen voor SQL Server installeren met behulp van Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) of [bouwen van een SharePoint-Server 2016 referentie-installatiekopie (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).
>
>

U kunt ook meer informatie over [uploaden van de VHD naar Azure proces](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)

## <a name="deploy-a-vm-via-resource-manager-template"></a>Een virtuele machine via Resource Manager-sjabloon implementeren
In de Resource Manager-sjablonen, een extra parameter `licenseType` moet worden opgegeven. U kunt meer lezen over [Azure Resource Manager-sjablonen ontwerpen](../../resource-group-authoring-templates.md). Zodra u uw VHD die is geüpload naar Azure hebt, bewerkt u Resource Manager-sjabloon voor het licentietype opnemen als onderdeel van de compute-provider en de sjabloon als normale implementeren:

```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a>Een virtuele machine via PowerShell Quick Start implementeren
Wanneer u uw Windows Server-VM via PowerShell implementeert, hebt u een extra parameter `-LicenseType`. Zodra u uw VHD die is geüpload naar Azure hebt, maakt u een VM met `New-AzureRmVM` en geef het type van de licentieverlening als volgt:

Voor WindowsServer:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

U kunt een meer beschrijvende gids op de verschillende stappen voor het lezen [maken van een Windows-VM met Resource Manager en PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Controleer of uw virtuele machine wordt gebruikt door de licentiegegevens benefit
Zodra u hebt geïmplementeerd uw virtuele machine via de PowerShell, Resource Manager-sjabloon of -portal, kunt u controleren of het licentietype met `Get-AzureRmVM` als volgt:

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

De uitvoer is vergelijkbaar met het volgende voorbeeld voor Windows Server:

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

## <a name="list-all-azure-hybrid-benefit-for-windows-server-vms-in-a-subscription"></a>Lijst van alle Azure hybride voordeel voor Windows Server-VM's in een abonnement

Als u wilt zien en tellen van alle virtuele machines die zijn geïmplementeerd met Azure hybride voordeel voor Windows Server, kunt u de volgende opdracht uitvoeren uit uw abonnement:

```powershell
$vms = Get-AzureRMVM 
foreach ($vm in $vms) {"VM Name: " + $vm.Name, "   Azure Hybrid Benefit for Windows Server: "+ $vm.LicenseType}
```

## <a name="deploy-a-virtual-machine-scale-set-with-azure-hybrid-benefit-for-windows-server"></a>Een virtuele-machineschaalset ingesteld met Azure hybride voordeel voor Windows Server implementeren
Binnen uw virtuele machine schaalset Resource Manager-sjablonen, een extra parameter `licenseType` moet worden opgegeven. U kunt meer lezen over [Azure Resource Manager-sjablonen ontwerpen](../../resource-group-authoring-templates.md). De Resource Manager-sjabloon voor de eigenschap licenseType opnemen als onderdeel van de schaalaanpassingsset virtualMachineProfile en implementeren van uw sjabloon als normale - Zie het volgende voorbeeld met behulp van de installatiekopie van Windows Server 2016 bewerken:


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
U kunt ook [maken en implementeren van een virtuele-machineschaalset](#https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-create) en stel de eigenschap LicenseType

## <a name="next-steps"></a>Volgende stappen
Lees meer over [hoe u geld besparen en de schaalvoordelen van Azure hybride](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

Meer informatie over [Azure hybride voordeel voor Windows Server-licentieverlening gedetailleerde richtlijnen](https://docs.microsoft.com/en-us/windows-server/get-started/azure-hybrid-benefit)

Meer informatie over [met behulp van Resource Manager-sjablonen](../../azure-resource-manager/resource-group-overview.md)

Meer informatie over [Azure hybride voordeel voor Windows Server en Azure Site Recovery toepassingen maken voor migratie naar Azure nog meer rendabele](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/)

Meer informatie over [Windows 10 in Azure met Multitenant Hosting-rechts](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment)

Lees meer over [Veelgestelde vragen](#https://azure.microsoft.com/en-us/pricing/hybrid-use-benefit/faq/)
