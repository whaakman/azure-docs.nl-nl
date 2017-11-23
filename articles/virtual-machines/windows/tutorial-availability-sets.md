---
title: Beschikbaarheidssets zelfstudie voor Windows-machines in Azure | Microsoft Docs
description: Meer informatie over de Beschikbaarheidssets voor Windows-machines in Azure.
documentationcenter: 
services: virtual-machines-windows
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/05/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 2345b434a51b768793c2dea4587dc0a49ab35b70
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="how-to-use-availability-sets"></a>Het gebruik van beschikbaarheidssets

In deze zelfstudie leert u hoe verhoogt de beschikbaarheid en betrouwbaarheid van uw virtuele Machine-oplossingen in Azure met een mogelijkheid Beschikbaarheidssets aangeroepen. Beschikbaarheidssets Zorg ervoor dat de virtuele machines die u implementeert in Azure worden gedistribueerd over meerdere geïsoleerde hardwareknooppunten in een cluster. Dit zorgt ervoor dat een submap reeks uw virtuele machines worden beïnvloed als er een storing hardware of software in Azure gebeurt, doen en dat de algehele oplossing beschikbaar is en operationele blijft. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een beschikbaarheidsset maken
> * Een virtuele machine in een beschikbaarheidsset maken
> * Controleer de beschikbare grootten voor virtuele machine
> * Azure Advisor controleren

Voor deze zelfstudie is moduleversie 3,6 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="availability-set-overview"></a>Overzicht van de beschikbaarheidsset

Een Beschikbaarheidsset is een logische groepering-functie die u in Azure gebruiken kunt om ervoor te zorgen dat de VM-resources die u in het plaatsen van elkaar geïsoleerd zijn wanneer ze zijn geïmplementeerd in een Azure-datacenter. Azure zorgt ervoor dat de virtuele machines die u binnen een Beschikbaarheidsset uitvoeren op meerdere fysieke servers plaatst, compute rekken eenheden voor opslag en netwerkswitches. Als er een hardware- of Azure software-fout optreedt, alleen een subset van uw virtuele machines worden beïnvloed en uw algehele toepassing blijft en blijft beschikbaar voor uw klanten. Beschikbaarheidssets zijn een essentieel mogelijkheid bij om betrouwbare cloudoplossingen te bouwen.

Laten we eens een typische VM-oplossing op basis van waar u mogelijk 4 front-end-webservers en 2 back-end virtuele machines die een database te hosten. Met Azure twee beschikbaarheidssets definiëren voordat u uw virtuele machines implementeert u zou willen: één beschikbaarheidsset voor de weblaag en één beschikbaarheidsset voor de databaselaag. Bij het maken van een nieuwe virtuele machine vervolgens u kunt de beschikbaarheidsset als een parameter voor de vm az opdracht maken en Azure automatisch zorgt ervoor dat de virtuele machines die u maakt in zijn over meerdere fysieke hardwareresources de beschikbare set geïsoleerd. Als de fysieke hardware die een van uw webserver of VM's Database-Server wordt uitgevoerd op een probleem is, weet u dat de andere exemplaren van uw webserver en de Database virtuele machines actief blijven, omdat ze op andere hardware.

Gebruik Beschikbaarheidssets wanneer u wilt implementeren, betrouwbare oplossingen op basis van virtuele machine in Azure.

## <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken

Kunt u een beschikbaarheidsset met [nieuw AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). In dit voorbeeld wordt zowel het aantal update en fouttolerantie domeinen op ingesteld *2* voor de beschikbaarheid van de set met de naam *myAvailabilitySet* in de *myResourceGroupAvailability* resourcegroep.

Maak een resourcegroep.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```

Maken van een beheerde beschikbaarheid instellen met behulp van [nieuw AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) met de **- sku uitgelijnd** parameter.

```azurepowershell-interactive
New-AzureRmAvailabilitySet `
   -Location EastUS `
   -Name myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability `
   -sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Virtuele machines in een beschikbaarheidsset maken

Virtuele machines moeten worden gemaakt binnen de beschikbaarheidsset om ervoor te zorgen dat ze correct zijn verdeeld over de hardware. U kunt een bestaande virtuele machine toevoegen aan een beschikbaarheidsset nadat deze is gemaakt. 

De hardware op een locatie is onderverdeeld meerdere domeinen van de update en domeinen met fouten. Een **updatedomein** is een groep VM's en de onderliggende fysieke hardware die op hetzelfde moment kan worden opgestart. Virtuele machines in dezelfde **foutdomein** algemene storage, evenals een gemeenschappelijk power-bron- en switch delen. 

Wanneer u een VM-configuratie met maakt [nieuw AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) u gebruikt de `-AvailabilitySetId` parameter om de ID van de beschikbaarheidsset.

Maak twee virtuele machines met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) in de beschikbaarheid instellen.

```azurepowershell-interactive
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAvailability `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig
    
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

$nsg = New-AzureRmNetworkSecurityGroup `
    -Location eastus `
    -Name myNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAvailability `
    -SecurityRules $nsgRuleRDP
    
# Apply the network security group to a subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name mySubnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24

# Update the virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

for ($i=1; $i -le 2; $i++)
{
   $pip = New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name "mypublicdns$(Get-Random)" `
        -AllocationMethod Static `
        -IdleTimeoutInMinutes 4

   $nic = New-AzureRmNetworkInterface `
        -Name myNic$i `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -SubnetId $vnet.Subnets[0].Id `
        -PublicIpAddressId $pip.Id `
        -NetworkSecurityGroupId $nsg.Id

   # Here is where we specify the availability set
   $vm = New-AzureRmVMConfig `
        -VMName myVM$i `
        -VMSize Standard_D1 `
        -AvailabilitySetId $availabilitySet.Id

   $vm = Set-AzureRmVMOperatingSystem `
        -ComputerName myVM$i `
        -Credential $cred `
        -VM $vm `
        -Windows `
        -EnableAutoUpdate `
        -ProvisionVMAgent
   $vm = Set-AzureRmVMSourceImage `
        -VM $vm `
        -PublisherName MicrosoftWindowsServer `
        -Offer WindowsServer `
        -Skus 2016-Datacenter `
        -Version latest
   $vm = Set-AzureRmVMOSDisk `
        -VM $vm `
        -Name myOsDisk$i `
        -DiskSizeInGB 128 `
        -CreateOption FromImage `
        -Caching ReadWrite
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -VM $vm
}

```

Het duurt enkele minuten maken en configureren van beide virtuele machines. Wanneer u klaar bent, hebt u twee virtuele machines die zijn verdeeld over de onderliggende hardware. 

Als u de beschikbaarheidsset in de portal door te gaan aan resourcegroepen bekijkt > myResourceGroupAvailability > myAvailabilitySet, moet u zien hoe de virtuele machines zijn verdeeld over de fout met de 2 en domeinen bijwerken.

![Beschikbaarheidsset voor de portal](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Controleren op beschikbare VM-grootten 

U kunt meer virtuele machines toevoegen aan de beschikbaarheid later instellen, maar u moet weten welke VM-grootten zijn beschikbaar op de hardware. Gebruik [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) voor een lijst met alle van de beschikbare grootten van de hardware-cluster gebruikt voor de beschikbaarheidsset.

```azurepowershell-interactive
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="check-azure-advisor"></a>Azure Advisor controleren 

U kunt ook Azure Advisor voor meer informatie over het verbeteren van de beschikbaarheid van uw virtuele machines. Azure Advisor helpt u Volg de aanbevolen procedures om uw Azure-implementaties te optimaliseren. Het analyseert uw resourceconfiguratie en de telemetrie van de informatie over het gebruik en vervolgens raadt aan om de oplossingen waarmee u de kosteneffectiviteit, prestaties, beschikbaarheid en beveiliging van uw Azure-resources te verbeteren.

Aanmelden bij de [Azure-portal](https://portal.azure.com), selecteer **meer services**, en het type **Advisor**. De Advisor-dashboard toont de persoonlijke aanbevelingen voor het geselecteerde abonnement. Zie voor meer informatie [aan de slag met Azure Advisor](../../advisor/advisor-get-started.md).


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een beschikbaarheidsset maken
> * Een virtuele machine in een beschikbaarheidsset maken
> * Controleer de beschikbare grootten voor virtuele machine
> * Azure Advisor controleren

Ga naar de volgende zelfstudie voor meer informatie over virtuele-machineschaalsets.

> [!div class="nextstepaction"]
> [Een VM-schaalset maken](tutorial-create-vmss.md)


