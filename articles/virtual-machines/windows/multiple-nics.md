---
title: Maken en beheren van Windows-virtuele machines in Azure met meerdere NIC's | Microsoft Docs
description: Informatie over het maken en beheren van een Windows-VM met meerdere NIC's gekoppeld met behulp van Azure PowerShell of de Resource Manager-sjablonen.
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/26/2017
ms.author: iainfou
ms.openlocfilehash: 941791ba398a3abbaa5137c36391fd23789cd3b1
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a>Maken en beheren van een virtuele Windows-machine met meerdere NIC 's
Virtuele machines (VM's) in Azure, kunnen meerdere virtuele netwerkinterfacekaarten (NIC's) gekoppeld aan deze hebben. Een veelvoorkomend scenario is om verschillende subnetten voor front-end en back-end-verbinding of een netwerk dat is toegewezen aan een oplossing met bewaking of back-up. Dit artikel wordt uitgelegd hoe u een virtuele machine met meerdere NIC's die zijn gekoppeld aan het maken. U leert ook hoe toevoegen of verwijderen van NIC's van een bestaande virtuele machine. Andere [VM-grootten](sizes.md) ondersteunen een verschillend aantal NIC's, dus het formaat van uw virtuele machine dienovereenkomstig.

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u hebt de [meest recente versie van Azure PowerShell geïnstalleerd en geconfigureerd](/powershell/azure/overview).

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. De namen van de voorbeeld-parameter *myResourceGroup*, *myVnet*, en *myVM*.


## <a name="create-a-vm-with-multiple-nics"></a>Een virtuele machine met meerdere NIC's maken
Maak eerst een resourcegroep. Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *EastUs* locatie:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a>Virtueel netwerk en subnetten maken
Er is een veelvoorkomend scenario voor een virtueel netwerk aan twee of meer subnetten hebben. Eén subnet mogelijk voor front-verkeer, de andere voor back-end-verkeer. Beide subnetten verbinding wordt vervolgens gebruikt u meerdere NIC's op de virtuele machine.

1. Twee subnetten voor virtueel netwerk met definiëren [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Het volgende voorbeeld definieert de subnetten voor *mySubnetFrontEnd* en *mySubnetBackEnd*:

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. Maken van uw virtuele netwerk en de subnetten met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVnet*:

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a>Meerdere NIC's maken
Maak twee NIC's met [nieuw AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Één NIC met het front-end-subnet en één NIC koppelen aan het subnet voor back-end. Het volgende voorbeeld wordt de NIC's met de naam *myNic1* en *myNic2*:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

Maakt u gewoonlijk ook een [netwerkbeveiligingsgroep](../../virtual-network/virtual-networks-nsg.md) om netwerkverkeer te filteren met de virtuele machine en een [netwerktaakverdeler](../../load-balancer/load-balancer-overview.md) voor de distributie van verkeer over meerdere virtuele machines.

### <a name="create-the-virtual-machine"></a>De virtuele machine maken
Nu beginnen met het bouwen van uw VM-configuratie. Elk VM-grootte heeft een limiet voor het totale aantal NIC's die u aan een virtuele machine toevoegen kunt. Zie voor meer informatie [Windows VM-grootten](sizes.md).

1. De referenties van uw virtuele machine ingesteld op de `$cred` variabele als volgt:

    ```powershell
    $cred = Get-Credential
    ```

2. Definieer uw virtuele machine met [nieuwe AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). Het volgende voorbeeld definieert een virtuele machine met de naam *myVM* en maakt gebruik van een VM-grootte die ondersteuning biedt voor meer dan twee NIC's (*Standard_DS3_v2*):

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. Maken van de rest van de VM-configuratie met [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) en [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage). Het volgende voorbeeld wordt een Windows Server 2016 VM:

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. Koppelen van de twee NIC's die u eerder hebt gemaakt met [toevoegen AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. Maak ten slotte uw virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

## <a name="add-a-nic-to-an-existing-vm"></a>Een NIC naar een bestaande virtuele machine toevoegen
Als een virtuele NIC toevoegen aan een bestaande virtuele machine, u de VM ongedaan virtuele NIC toevoegen en start u de virtuele machine. Andere [VM-grootten](sizes.md) ondersteunen een verschillend aantal NIC's, dus het formaat van uw virtuele machine dienovereenkomstig. Indien nodig, kunt u [vergroten of verkleinen van een virtuele machine](resize-vm.md).

1. Toewijzing van de virtuele machine met [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). Het volgende voorbeeld de virtuele machine met de naam deallocates *myVM* in *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Ophalen van de bestaande configuratie van de virtuele machine met [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). Het volgende voorbeeld wordt de informatie voor de virtuele machine met de naam *myVM* in *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Het volgende voorbeeld wordt een virtuele NIC met [nieuw AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) met de naam *myNic3* die is gekoppeld aan *mySubnetBackEnd*. De virtuele NIC is vervolgens gekoppeld aan de virtuele machine met de naam *myVM* in *myResourceGroup* met [toevoegen AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    # Get info for the back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get the ID of the new virtual NIC and add to VM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a>Primaire virtuele NIC 's
    Een van de netwerkadapters op een VM meerdere NIC moet primaire. Als een van de bestaande virtuele NIC's op de virtuele machine al is ingesteld als primaire, kunt u deze stap overslaan. Het volgende voorbeeld wordt ervan uitgegaan dat twee virtuele NIC's nu aanwezig is op een virtuele machine zijn en u wilt toevoegen, de eerste NIC (`[0]`) als de primaire:
        
    ```powershell
    # List existing NICs on the VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 to be primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update the VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. Start de virtuele machine met [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

## <a name="remove-a-nic-from-an-existing-vm"></a>Een NIC van een bestaande virtuele machine verwijderen
Als u wilt verwijderen van een virtuele NIC van een bestaande virtuele machine, toewijzing van de virtuele machine, verwijdert u de virtuele NIC en start de virtuele machine.

1. Toewijzing van de virtuele machine met [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). Het volgende voorbeeld de virtuele machine met de naam deallocates *myVM* in *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Ophalen van de bestaande configuratie van de virtuele machine met [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). Het volgende voorbeeld wordt de informatie voor de virtuele machine met de naam *myVM* in *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Informatie over het verwijderen van de NIC met [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface). Het volgende voorbeeld wordt informatie over *myNic3*:

    ```powershell
    # List existing NICs on the VM if you need to determine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. Verwijderen van de NIC met [verwijderen AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) en werk vervolgens de virtuele machine met [Update-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm). Het volgende voorbeeld verwijdert u *myNic3* verkregen met `$nicId` in de vorige stap:

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. Start de virtuele machine met [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a>Meerdere NIC's met behulp van sjablonen maken
Azure Resource Manager-sjablonen kunnen u meerdere exemplaren van een bron tijdens implementatie, zoals het maken van meerdere NIC's maken. Resource Manager-sjablonen gebruiken declaratieve JSON-bestanden voor het definiëren van uw omgeving. Zie voor meer informatie [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). U kunt *kopie* om op te geven van het aantal exemplaren te maken:

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

Zie voor meer informatie [maken van meerdere exemplaren met *kopie*](../../resource-group-create-multiple.md). 

U kunt ook `copyIndex()` naar een nummer toevoegen aan de naam van een resource. Vervolgens kunt u maken *myNic1*, *MyNic2* enzovoort. De volgende code toont een voorbeeld van de waarde voor de index:

```json
"name": "[concat('myNic', copyIndex())]", 
```

U kunt een compleet voorbeeld van lezen [meerdere NIC's maken met Resource Manager-sjablonen](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Volgende stappen
Bekijk [Windows VM-grootten](sizes.md) wanneer u probeert te maken van een virtuele machine die meerdere NIC's heeft. Let op het maximum aantal NIC's die ondersteuning biedt voor elke VM-grootte. 


