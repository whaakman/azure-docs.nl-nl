---
title: Meerdere IP-adressen voor virtuele machines van Azure - PowerShell | Microsoft Docs
description: Leer hoe u meerdere IP-adressen toewijzen aan een virtuele machine met behulp van PowerShell. | Resource Manager
services: virtual-network
documentationcenter: na
author: jimdial
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: c44ea62f-7e54-4e3b-81ef-0b132111f1f8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/24/2017
ms.author: jdial;annahar
ms.openlocfilehash: bded6c76aa209a718929c891bd2443f8517ec44a
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58649630"
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-powershell"></a>Meerdere IP-adressen toewijzen aan virtuele machines met behulp van PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

In dit artikel wordt uitgelegd hoe u een virtuele machine (VM) maken via het Azure Resource Manager-implementatiemodel met behulp van PowerShell. Meerdere IP-adressen kunnen niet worden toegewezen aan resources die zijn gemaakt via het klassieke implementatiemodel. Lees voor meer informatie over Azure-implementatiemodellen, de [-implementatiemodellen begrijpen](../resource-manager-deployment-model.md) artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Een virtuele machine maken met meerdere IP-adressen

De volgende stappen wordt uitgelegd hoe u een voorbeeld van de virtuele machine maken met meerdere IP-adressen, zoals beschreven in het scenario. Wijzigen van de waarden zoals vereist voor uw implementatie.

1. Open een PowerShell-opdrachtprompt en voer de overige stappen in deze sectie binnen één PowerShell-sessie. Als u niet al PowerShell is geïnstalleerd en geconfigureerd hebt, voltooit u de stappen in de [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview) artikel.
2. Meld u aan bij uw account met de `Connect-AzAccount` opdracht.
3. Vervang *myResourceGroup* en *westus* met een naam en locatie van uw keuze. Maak een resourcegroep. Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

   ```powershell
   $RgName   = "MyResourceGroup"
   $Location = "westus"

   New-AzResourceGroup `
   -Name $RgName `
   -Location $Location
   ```

4. Maak een virtueel netwerk (VNet) en een subnet in dezelfde locatie als de resourcegroep:

   ```powershell

   # Create a subnet configuration
   $SubnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name MySubnet `
   -AddressPrefix 10.0.0.0/24

   # Create a virtual network
   $VNet = New-AzVirtualNetwork `
   -ResourceGroupName $RgName `
   -Location $Location `
   -Name MyVNet `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $subnetConfig

   # Get the subnet object
   $Subnet = Get-AzVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
   ```

5. Maak een netwerkbeveiligingsgroep (NSG) en een regel. De Netwerkbeveiligingsgroep beveiligt de virtuele machine met behulp van regels voor binnenkomend en uitgaand. In dit geval is er een binnenkomende regel gemaakt voor poort 3389, waarmee binnenkomende verbindingen met een extern bureaublad worden toegestaan.

    ```powershell
    
    # Create an inbound network security group rule for port 3389

    $NSGRule = New-AzNetworkSecurityRuleConfig `
    -Name MyNsgRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow
    
    # Create a network security group
    $NSG = New-AzNetworkSecurityGroup `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyNetworkSecurityGroup `
    -SecurityRules $NSGRule
    ```

6. Definieert de primaire IP-configuratie voor de NIC. 10.0.0.4 wijzigen naar een geldig adres in het subnet dat u hebt gemaakt, als u de eerder gedefinieerde waarde hebt gebruikt. Voordat u een statisch IP-adres toewijst, wordt het aanbevolen dat u eerst bevestigen dat deze nog niet in gebruik. Voer de opdracht `Test-AzPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`. Als het adres beschikbaar is, wordt de uitvoer geretourneerd *waar*. De uitvoer geretourneerd als deze niet beschikbaar is, *False* en een lijst met adressen die beschikbaar zijn. 

    In de volgende opdrachten, **< vervangen-met-uw unieke-naam > vervangen door de unieke DNS-naam te gebruiken.** De naam moet uniek zijn in alle openbare IP-adressen binnen een Azure-regio. Dit is een optionele parameter. Het kan worden verwijderd als u wilt dat alleen verbinding maken met de virtuele machine met behulp van het openbare IP-adres.

    ```powershell
    
    # Create a public IP address
    $PublicIP1 = New-AzPublicIpAddress `
    -Name "MyPublicIP1" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -DomainNameLabel <replace-with-your-unique-name> `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign the public IP address to it
    $IpConfigName1 = "IPConfig-1"
    $IpConfig1     = New-AzNetworkInterfaceIpConfig `
    -Name $IpConfigName1 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.4 `
    -PublicIpAddress $PublicIP1 `
    -Primary
    ```

    Wanneer u meerdere IP-configuraties aan een NIC toewijst, één configuratie moet worden toegewezen als de *-primaire*.

    > [!NOTE]
    > Openbare IP-adressen hebben nominale kosten in rekening. Lees voor meer informatie over prijzen voor IP-adres, de [prijzen van IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina. Er is een limiet aan het aantal openbare IP-adressen die kunnen worden gebruikt in een abonnement. Lees voor meer informatie over de limieten het artikel [Azure-limieten](../azure-subscription-service-limits.md#networking-limits).

7. Definieer de secundaire IP-configuraties voor de NIC. U kunt toevoegen of verwijderen van configuraties indien nodig. Elke IP-configuratie moet een privé IP-adres toegewezen. Elke configuratie kan eventueel een openbaar IP-adres toegewezen hebben.

    ```powershell
    
    # Create a public IP address
    $PublicIP2 = New-AzPublicIpAddress `
    -Name "MyPublicIP2" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign the public IP address to it
    $IpConfigName2 = "IPConfig-2"
    $IpConfig2     = New-AzNetworkInterfaceIpConfig `
    -Name $IpConfigName2 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.5 `
    -PublicIpAddress $PublicIP2
        
    $IpConfigName3 = "IpConfig-3"
    $IpConfig3 = New-AzNetworkInterfaceIpConfig `
    -Name $IPConfigName3 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.6
    ```

8. Maak de NIC en de drie IP-configuraties te koppelen:

   ```powershell
   $NIC = New-AzNetworkInterface `
   -Name MyNIC `
   -ResourceGroupName $RgName `
   -Location $Location `
   -NetworkSecurityGroupId $NSG.Id `
   -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
   ```

   >[!NOTE]
   >Hoewel alle configuraties worden toegewezen aan een NIC in dit artikel, kunt u meerdere IP-configuraties kunt toewijzen aan elke NIC die is gekoppeld aan de virtuele machine. Lees voor meer informatie over het maken van een virtuele machine met meerdere NIC's, de [een virtuele machine maken met meerdere NIC's](../virtual-machines/windows/multiple-nics.md) artikel.

9. De virtuele machine maken met de volgende opdrachten:

    ```powershell
    
    # Define a credential object. When you run these commands, you're prompted to enter a username and password for the VM you're creating.
    $cred = Get-Credential
    
    # Create a virtual machine configuration
    $VmConfig = New-AzVMConfig `
    -VMName MyVM `
    -VMSize Standard_DS1_v2 | `
    Set-AzVMOperatingSystem -Windows `
    -ComputerName MyVM `
    -Credential $cred | `
    Set-AzVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
    Add-AzVMNetworkInterface `
    -Id $NIC.Id
    
    # Create the VM
    New-AzVM `
    -ResourceGroupName $RgName `
    -Location $Location `
    -VM $VmConfig
    ```

10. De privé IP-adressen toevoegen aan het besturingssysteem van de virtuele machine via de stappen voor het besturingssysteem in de [toevoegen IP-adressen aan een VM-besturingssysteem](#os-config) sectie van dit artikel. Voeg de openbare IP-adressen niet toe aan het besturingssysteem.

## <a name="add"></a>IP-adressen toevoegen aan een virtuele machine

U kunt persoonlijke en openbare IP-adressen toevoegen aan de interface van de Azure-netwerk via de stappen die volgen. De voorbeelden in de volgende secties wordt ervan uitgegaan dat u al een virtuele machine met de drie IP-configuraties die worden beschreven hebt in de [scenario](#scenario) in dit artikel, maar het is niet vereist dat u doen.

1. Open een PowerShell-opdrachtprompt en voer de overige stappen in deze sectie binnen één PowerShell-sessie. Als u niet al PowerShell is geïnstalleerd en geconfigureerd hebt, voltooit u de stappen in de [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview) artikel.
2. De 'values' van de volgende $Variables wijzigen op de naam van de NIC die u wilt toevoegen van IP-adres en de resourcegroep en locatie de NIC die is opgeslagen in:

   ```powershell
   $NicName  = "MyNIC"
   $RgName   = "MyResourceGroup"
   $Location = "westus"
   ```

   Als u niet bekend met de naam van de NIC die u wilt wijzigen bent, voer de volgende opdrachten, klikt u vervolgens de waarden van de vorige variabelen te wijzigen:

   ```powershell
   Get-AzNetworkInterface | Format-Table Name, ResourceGroupName, Location
   ```

3. Maak een variabele en stel deze in op de bestaande NIC door de volgende opdracht te typen:

   ```powershell
   $MyNIC = Get-AzNetworkInterface -Name $NicName -ResourceGroupName $RgName
   ```

4. Wijzig in de volgende opdrachten, *MyVNet* en *MySubnet* aan de namen van de VNet en de NIC is verbonden met subnet. Voer de opdrachten voor het ophalen van de VNet en subnet objecten die de NIC is verbonden met:

   ```powershell
   $MyVNet = Get-AzVirtualnetwork -Name MyVNet -ResourceGroupName $RgName
   $Subnet = $MyVnet.Subnets | Where-Object { $_.Name -eq "MySubnet" }
   ```

   Als u de naam van het VNet of subnet die de NIC is verbonden met niet weet, voert u de volgende opdracht uit:

   ```powershell
   $MyNIC.IpConfigurations
   ```

   Zoek in de uitvoer van de tekst die vergelijkbaar is met de volgende voorbeelduitvoer:

   ```
   "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet/subnets/MySubnet"
   ```

    In deze uitvoer *MyVnet* is het VNet en *MySubnet* is het de NIC is verbonden met subnet.

5. Volg de stappen in een van de volgende secties, op basis van uw vereisten:

   **Een privé IP-adres toevoegen**

   Als u wilt een privé IP-adres toevoegen aan een NIC, moet u een IP-configuratie maken. De volgende opdracht maakt een configuratie met een statisch IP-adres 10.0.0.7 toegewezen. Wanneer u een statisch IP-adres opgeeft, moet deze een niet-gebruikte adres voor het subnet. Het wordt aanbevolen dat u eerst het adres om te controleren of deze beschikbaar is door in te voeren test de `Test-AzPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` opdracht. Als het IP-adres beschikbaar is, wordt de uitvoer geretourneerd *waar*. De uitvoer geretourneerd als deze niet beschikbaar is, *False*, en een lijst met adressen die beschikbaar zijn.

   ```powershell
   Add-AzNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
   $MyNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
   ```

   Maak zo veel configuraties die u nodig hebt, met behulp van unieke namen en privé-IP-adressen (voor configuraties met statische IP-adressen).

   Het privé IP-adres toevoegen aan het besturingssysteem van de virtuele machine via de stappen voor het besturingssysteem in de [toevoegen IP-adressen aan een VM-besturingssysteem](#os-config) sectie van dit artikel.

   **Een openbaar IP-adres toevoegen**

   Een openbaar IP-adres is toegevoegd door het koppelen van een openbare IP-adresresource aan een nieuwe IP-configuratie of een bestaande IP-configuratie. Voer de stappen in een van de secties die volgen, die u nodig hebt.

   > [!NOTE]
   > Openbare IP-adressen hebben nominale kosten in rekening. Lees voor meer informatie over prijzen voor IP-adres, de [prijzen van IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina. Er is een limiet aan het aantal openbare IP-adressen die kunnen worden gebruikt in een abonnement. Lees voor meer informatie over de limieten het artikel [Azure-limieten](../azure-subscription-service-limits.md#networking-limits).
   >

   **Koppel het openbare IP-adresresource aan een nieuwe IP-configuratie**

   Wanneer u een openbaar IP-adres aan een nieuwe IP-configuratie toevoegt, moet u ook een privé IP-adres, toevoegen, omdat alle IP-configuraties moeten een privé IP-adres hebben. U kunt een bestaande openbare IP-adresresource toevoegen of u een nieuwe maken. Voor het maken van een nieuwe, voer de volgende opdracht:

   ```powershell
   $myPublicIp3 = New-AzPublicIpAddress `
   -Name "myPublicIp3" `
   -ResourceGroupName $RgName `
   -Location $Location `
   -AllocationMethod Static
   ```

   Een nieuwe IP-configuratie maken met een statisch privé IP-adres en de bijbehorende *myPublicIp3* openbaar IP-adres bron-adres, voer de volgende opdracht:

   ```powershell
   Add-AzNetworkInterfaceIpConfig `
   -Name IPConfig-4 `
   -NetworkInterface $myNIC `
   -Subnet $Subnet `
   -PrivateIpAddress 10.0.0.7 `
   -PublicIpAddress $myPublicIp3
   ```

   **Koppel het openbare IP-adresresource aan een bestaande IP-configuratie**

   Een openbare IP-adresresource kan alleen worden gekoppeld aan een IP-configuratie die nog niet bevat die zijn gekoppeld. U kunt bepalen of een IP-configuratie een gekoppeld openbaar IP-adres heeft met de volgende opdracht:

   ```powershell
   $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
   ```

   U ziet de uitvoer is vergelijkbaar met het volgende:

   ```
   Name       PrivateIpAddress PublicIpAddress                                           Primary

   IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
   IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
   IpConfig-3 10.0.0.6                                                                     False
   ```

   Omdat de **PublicIpAddress** kolom voor *IpConfig 3* is leeg, geen openbare IP-adresresource is momenteel gekoppeld. U kunt een bestaande openbare IP-adresresource toevoegen aan IP-configuratie 3 of Voer de volgende opdracht een te maken:

   ```powershell
   $MyPublicIp3 = New-AzPublicIpAddress `
   -Name "MyPublicIp3" `
   -ResourceGroupName $RgName `
   -Location $Location -AllocationMethod Static
   ```

   Voer de volgende opdracht om te koppelen van de openbare IP-adresresource aan de bestaande IP-configuratie met de naam *IpConfig 3*:

   ```powershell
   Set-AzNetworkInterfaceIpConfig `
   -Name IpConfig-3 `
   -NetworkInterface $mynic `
   -Subnet $Subnet `
   -PublicIpAddress $myPublicIp3
   ```

6. Stel de NIC met de nieuwe IP-configuratie met de volgende opdracht:

   ```powershell
   Set-AzNetworkInterface -NetworkInterface $MyNIC
   ```

7. De privé IP-adressen en de openbare IP-adres-resources toegewezen aan de NIC met de volgende opdracht weergeven:

   ```powershell
   $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
   ```

8. Het privé IP-adres toevoegen aan het besturingssysteem van de virtuele machine via de stappen voor het besturingssysteem in de [toevoegen IP-adressen aan een VM-besturingssysteem](#os-config) sectie van dit artikel. Voeg geen openbaar IP-adres toe aan het besturingssysteem.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]