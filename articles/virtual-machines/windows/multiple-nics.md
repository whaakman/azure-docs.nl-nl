---
title: Windows-machines in Azure die gebruikmaken van meerdere NIC's maken en beheren | Microsoft Docs
description: Informatie over het maken en beheren van een Windows-VM met meerdere NIC's die zijn gekoppeld met behulp van Azure PowerShell of Resource Manager-sjablonen.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/26/2017
ms.author: cynthn
ms.openlocfilehash: d29676b107885350785ceb1c17eb3010cc0907d2
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928342"
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a>Een Windows virtuele machine met meerdere NIC's maken en beheren
Virtuele machines (VM's) in Azure kunnen meerdere virtuele netwerkinterfacekaarten (NIC's) die is gekoppeld aan deze hebben. Een veelvoorkomend scenario is dat verschillende subnetten voor front-end en back-end-connectiviteit of een netwerk dat is toegewezen aan een oplossing voor controle of back-up. Dit artikel wordt uitgelegd hoe u een virtuele machine met meerdere NIC's die zijn gekoppeld aan het maakt. U leert ook hoe u kunt toevoegen of verwijderen van NIC's van een bestaande virtuele machine. Verschillende [VM-grootten](sizes.md) een verschillend aantal NIC's ondersteunen, dus het formaat van uw virtuele machine dienovereenkomstig.

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u hebt de [meest recente versie van Azure PowerShell is geïnstalleerd en geconfigureerd](/powershell/azure/overview).

In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld-parameternamen bevatten *myResourceGroup*, *myVnet*, en *myVM*.


## <a name="create-a-vm-with-multiple-nics"></a>Een virtuele machine met meerdere NIC's maken
Maak eerst een resourcegroep. Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *EastUs* locatie:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a>Virtueel netwerk en subnetten maken
Een veelvoorkomend scenario is voor een virtueel netwerk dat twee of meer subnetten. Een subnet kan zijn voor front-verkeer, de andere voor back-end-verkeer. Voor verbinding met beide subnetten, gebruikt u vervolgens meerdere NIC's op de virtuele machine.

1. Definieert u twee virtuele subnetten met [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Het volgende voorbeeld definieert de subnetten voor *mySubnetFrontEnd* en *mySubnetBackEnd*:

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. Maken van uw virtuele netwerk en subnetten met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVnet*:

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a>Meerdere NIC's maken
Maak twee NIC's met [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Één NIC aan de front-end-subnet en één NIC koppelen aan de back-end-subnet. Het volgende voorbeeld wordt de NIC's met de naam *myNic1* en *myNic2*:

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

Doorgaans ook maakt u een [netwerkbeveiligingsgroep](../../virtual-network/security-overview.md) om netwerkverkeer te filteren op de virtuele machine en een [netwerktaakverdeler](../../load-balancer/load-balancer-overview.md) om verkeer te verdelen over meerdere virtuele machines.

### <a name="create-the-virtual-machine"></a>De virtuele machine maken
Nu beginnen met bouwen van uw VM-configuratie. Elke VM-grootte heeft een limiet voor het totale aantal NIC's die u aan een virtuele machine toevoegen kunt. Zie voor meer informatie, [Windows VM-grootten](sizes.md).

1. De referenties van uw virtuele machine ingesteld op de `$cred` variabele als volgt te werk:

    ```powershell
    $cred = Get-Credential
    ```

2. Definieer uw virtuele machine met [nieuwe-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). Het volgende voorbeeld wordt een virtuele machine met de naam gedefinieerd *myVM* en maakt gebruik van een VM-grootte die ondersteuning biedt voor meer dan twee NIC's (*Standard_DS3_v2*):

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. Maken van de rest van uw VM-configuratie met [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) en [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage). Het volgende voorbeeld wordt een virtuele machine met Windows Server 2016:

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

4. De twee NIC's die u eerder hebt gemaakt met koppelen [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. Maak uw virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

6. Routes voor secundaire NIC's toevoegen aan het besturingssysteem via de stappen in [configureren van het besturingssysteem voor meerdere NIC's](#configure-guest-os-for-multiple-nics).

## <a name="add-a-nic-to-an-existing-vm"></a>Een NIC toevoegen aan een bestaande virtuele machine
Een virtuele NIC toevoegen aan een bestaande virtuele machine, die u Wijs de virtuele machine, de virtuele NIC toevoegen en start u de virtuele machine. Verschillende [VM-grootten](sizes.md) een verschillend aantal NIC's ondersteunen, dus het formaat van uw virtuele machine dienovereenkomstig. Indien nodig, kunt u [vergroten of verkleinen van een virtuele machine](resize-vm.md).

1. Wijs de virtuele machine met [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). Het volgende voorbeeld wordt de virtuele machine met de naam de toewijzing ingetrokken *myVM* in *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. De bestaande configuratie van de virtuele machine met [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). Het volgende voorbeeld wordt een van de virtuele machine met de naam *myVM* in *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Het volgende voorbeeld wordt een virtuele NIC met [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) met de naam *myNic3* die is gekoppeld aan *mySubnetBackEnd*. De virtuele NIC is vervolgens gekoppeld aan de virtuele machine met de naam *myVM* in *myResourceGroup* met [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

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
    Een van de NIC's op een VM meerdere NIC moet primaire. Als een van de bestaande virtuele NIC's op de virtuele machine is al ingesteld als primair, kunt u deze stap overslaan. Het volgende voorbeeld wordt ervan uitgegaan dat twee virtuele NIC's nu aanwezig is op een virtuele machine zijn en u wilt toevoegen van de eerste NIC (`[0]`) als de primaire:
        
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

5. Routes voor secundaire NIC's toevoegen aan het besturingssysteem via de stappen in [configureren van het besturingssysteem voor meerdere NIC's](#configure-guest-os-for-multiple-nics).

## <a name="remove-a-nic-from-an-existing-vm"></a>Een NIC van een bestaande virtuele machine verwijderen
Als u wilt verwijderen van een virtuele NIC van een bestaande virtuele machine, wijs de virtuele machine, verwijder de virtuele NIC en start de virtuele machine.

1. Wijs de virtuele machine met [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). Het volgende voorbeeld wordt de virtuele machine met de naam de toewijzing ingetrokken *myVM* in *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. De bestaande configuratie van de virtuele machine met [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). Het volgende voorbeeld wordt een van de virtuele machine met de naam *myVM* in *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Informatie over het verwijderen van de NIC met [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface). Het volgende voorbeeld wordt informatie over *myNic3*:

    ```powershell
    # List existing NICs on the VM if you need to determine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. Verwijderen van de NIC met [Remove-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) en werk vervolgens de virtuele machine met [Update-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm). Het volgende voorbeeld verwijdert u *myNic3* die zijn verkregen met `$nicId` in de vorige stap:

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. Start de virtuele machine met [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a>Meerdere NIC's maken met sjablonen
Azure Resource Manager-sjablonen bieden een manier om meerdere exemplaren van een resource maken tijdens de implementatie, zoals het maken van meerdere NIC's. Resource Manager-sjablonen gebruiken declaratieve JSON-bestanden voor het definiëren van uw omgeving. Zie voor meer informatie, [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). U kunt *kopie* om op te geven van het aantal exemplaren te maken:

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

Zie voor meer informatie, [het maken van meerdere exemplaren met *kopie*](../../resource-group-create-multiple.md). 

U kunt ook `copyIndex()` toe te voegen een nummer aan de naam van een resource. Vervolgens kunt u maken *myNic1*, *MyNic2* enzovoort. De volgende code toont een voorbeeld van de indexwaarde toe te voegen:

```json
"name": "[concat('myNic', copyIndex())]", 
```

U kunt een compleet voorbeeld van lezen [meerdere NIC's maken met behulp van Resource Manager-sjablonen](../../virtual-network/template-samples.md).

Routes voor secundaire NIC's toevoegen aan het besturingssysteem via de stappen in [configureren van het besturingssysteem voor meerdere NIC's](#configure-guest-os-for-multiple-nics).

## <a name="configure-guest-os-for-multiple-nics"></a>Gast-OS configureren voor meerdere NIC 's

Azure wijst geen standaardgateway toe aan de eerste (primaire) netwerkinterface die is gekoppeld aan de virtuele machine toe. Azure wijst geen standaardgateway toe aan extra (secundaire) netwerkinterfaces die zijn gekoppeld aan een virtuele machine. Daarom kunt u standaard niet communiceren met resources buiten het subnet waarin een secundaire netwerkinterface zich bevindt. Secundaire netwerkinterfaces kunnen echter wel communiceren met resources buiten hun subnet, hoewel de stappen voor het inschakelen van communicatie voor verschillende besturingssystemen verschillen.

1. Uitvoeren vanaf een Windows-opdrachtprompt, de `route print` opdracht waarvan uitvoer die lijkt op de volgende uitvoer gegenereerd voor een virtuele machine met twee gekoppelde netwerkinterfaces:

    ```
    ===========================================================================
    Interface List
    3...00 0d 3a 10 92 ce ......Microsoft Hyper-V Network Adapter #3
    7...00 0d 3a 10 9b 2a ......Microsoft Hyper-V Network Adapter #4
    ===========================================================================
    ```
 
    In dit voorbeeld **Microsoft Hyper-V Network Adapter #4** (interface 7) is de secundaire netwerkinterface die beschikt niet over een standaard-gateway toegewezen.

2. Uitvoeren vanaf een opdrachtprompt, de `ipconfig` opdracht om te zien welk IP-adres is toegewezen aan de secundaire netwerkinterface. In dit voorbeeld is 192.168.2.4 toegewezen aan interface 7. Er is geen standaard gateway-adres wordt geretourneerd voor de secundaire netwerkinterface.

3. Als u wilt routeren van al het verkeer dat is bestemd voor adressen buiten het subnet van de secundaire netwerkinterface met de gateway voor het subnet, voer de volgende opdracht:

    ```
    route add -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5015 IF 7
    ```

    Het adres van de gateway voor het subnet is het eerste IP-adres (hebben die eindigt op.1) in het adresbereik dat is gedefinieerd voor het subnet. Als u niet wilt voor het routeren van al het verkeer buiten het subnet, kunt u afzonderlijke routes in plaats daarvan naar bepaalde bestemmingen toevoegen. Bijvoorbeeld, als u alleen voor het routeren van verkeer van de secundaire netwerkinterface met de 192.168.3.0 wilde netwerk, u de opdracht hebt ingevoerd:

      ```
      route add -p 192.168.3.0 MASK 255.255.255.0 192.168.2.1 METRIC 5015 IF 7
      ```
  
4. Om te bevestigen dat communicatie met een resource op de 192.168.3.0 netwerk, bijvoorbeeld, voert u de volgende opdracht uit om te pingen 192.168.3.4 werd gestart met behulp van interface 7 (192.168.2.4):

    ```
    ping 192.168.3.4 -S 192.168.2.4
    ```

    Mogelijk moet u ICMP openen via de Windows firewall van het apparaat dat u met de volgende opdracht pingen bent:
  
      ```
      netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
      ```
  
5. Voer ter bevestiging de route toegevoegd in de routetabel is de `route print` opdracht waarvan uitvoer die lijkt op de volgende tekst:

    ```
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4     15
              0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.4   5015
    ```

    De route worden weergegeven met *192.168.1.1* onder **Gateway**, is de route die wordt er standaard voor de primaire netwerkinterface. De route met *192.168.2.1* onder **Gateway**, is de route die u hebt toegevoegd.

## <a name="next-steps"></a>Volgende stappen
Beoordeling [Windows VM-grootten](sizes.md) wanneer u probeert te maken van een virtuele machine die meerdere NIC's heeft. Let op het maximum aantal NIC's die ondersteuning biedt voor elke VM-grootte. 


