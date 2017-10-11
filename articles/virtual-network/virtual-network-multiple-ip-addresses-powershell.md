---
title: Meerdere IP-adressen voor virtuele machines in Azure - PowerShell | Microsoft Docs
description: Meer informatie over meerdere IP-adressen toewijzen aan een virtuele machine met behulp van PowerShell | Resource Manager.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c44ea62f-7e54-4e3b-81ef-0b132111f1f8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/24/2017
ms.author: jdial;annahar
ms.openlocfilehash: 29f64aeefc2a7deb1f84d759c2323347536b9c27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-powershell"></a>Meerdere IP-adressen toewijzen aan virtuele machines met behulp van PowerShell

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Dit artikel wordt uitgelegd hoe u een virtuele machine (VM) maken via het Azure Resource Manager-implementatiemodel met behulp van PowerShell. Meerdere IP-adressen kunnen niet worden toegewezen aan resources die zijn gemaakt met behulp van het klassieke implementatiemodel. Lees voor meer informatie over Azure-implementatiemodellen de [begrijpen implementatiemodellen](../resource-manager-deployment-model.md) artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Een virtuele machine maken met meerdere IP-adressen

Welke stappen volgen wordt uitgelegd hoe een voorbeeld van de virtuele machine maken met meerdere IP-adressen, zoals beschreven in het scenario. Variabele waarden zoals vereist voor uw implementatie te wijzigen.

1. Open een PowerShell-opdrachtprompt en voer de overige stappen in deze sectie binnen één PowerShell-sessie. Als u nog niet PowerShell geïnstalleerd en geconfigureerd, voer de stappen in de [installeren en configureren van Azure PowerShell](/powershell/azure/overview) artikel.
2. Meld u aan bij uw account met de `login-azurermaccount` opdracht.
3. Vervang *myResourceGroup* en *westus* met een naam en locatie van uw keuze. Maak een resourcegroep. Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

    ```powershell
    $RgName   = "MyResourceGroup"
    $Location = "westus"

    New-AzureRmResourceGroup `
    -Name $RgName `
    -Location $Location
    ```

4. Maak een virtueel netwerk (VNet) en het subnet in dezelfde locatie als de resourcegroep:

    ```powershell
    
    # Create a subnet configuration
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name MySubnet `
    -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $VNet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyVNet `
    -AddressPrefix 10.0.0.0/16 `
    -Subnet $subnetConfig

    # Get the subnet object
    $Subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
    ```

5. Een netwerkbeveiligingsgroep (NSG) en een regel maken. Het NSG beveiligt de virtuele machine met regels voor binnenkomend en uitgaand. In dit geval is er een binnenkomende regel gemaakt voor poort 3389, waarmee binnenkomende verbindingen met een extern bureaublad worden toegestaan.

    ```powershell
    
    # Create an inbound network security group rule for port 3389

    $NSGRule = New-AzureRmNetworkSecurityRuleConfig `
    -Name MyNsgRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow
    
    # Create a network security group
    $NSG = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyNetworkSecurityGroup `
    -SecurityRules $NSGRule
    ```

6. Definieer de primaire IP-configuratie voor de NIC. 10.0.0.4 wijzigen naar een geldig adres in het subnet dat u hebt gemaakt, als u de eerder gedefinieerde waarde hebt gebruikt. Voordat u een statisch IP-adres toewijst, wordt het aanbevolen dat u eerst bevestigen dat dit nog niet in gebruik. Voer de opdracht `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`. De uitvoer geretourneerd als het adres beschikbaar is, *True*. De uitvoer geretourneerd als deze niet beschikbaar is, *False* en een lijst met adressen die beschikbaar zijn. 

    In de volgende opdrachten **< vervangen-met-uw unieke-naam > vervangt door de unieke DNS-naam te gebruiken.** De naam moet uniek zijn in alle openbare IP-adressen binnen een Azure-regio. Dit is een optionele parameter. Het kan worden verwijderd als u alleen verbinding wilt maken met de virtuele machine met behulp van het openbare IP-adres.

    ```powershell
    
    # Create a public IP address
    $PublicIP1 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP1" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -DomainNameLabel <replace-with-your-unique-name> `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign the public IP ddress to it
    $IpConfigName1 = "IPConfig-1"
    $IpConfig1     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName1 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.4 `
    -PublicIpAddress $PublicIP1 `
    -Primary
    ```

    Wanneer u meerdere IP-configuraties aan een NIC toewijst, één configuratie moet worden toegewezen als de *-primaire*.

    > [!NOTE]
    > Openbare IP-adressen hebben een nominaal kosten. Lees meer informatie over prijzen voor IP-adres, de [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina. Er is een limiet aan het aantal openbare IP-adressen die kunnen worden gebruikt in een abonnement. Lees voor meer informatie over de limieten het artikel [Azure-limieten](../azure-subscription-service-limits.md#networking-limits).

7. Definieer de secundaire IP-configuraties voor de NIC. U kunt toevoegen of verwijderen van configuraties indien nodig. Elk IP-adresconfiguratie moet een particulier IP-adres toegewezen. Elke configuratie kan desgewenst een openbaar IP-adres toegewezen hebben.

    ```powershell
    
    # Create a public IP address
    $PublicIP2 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP2" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign the public IP ddress to it
    $IpConfigName2 = "IPConfig-2"
    $IpConfig2     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName2 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.5 `
    -PublicIpAddress $PublicIP2
        
    $IpConfigName3 = "IpConfig-3"
    $IpConfig3 = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IPConfigName3 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.6
    ```

8. De NIC maken en koppelen van de drie IP-configuraties:

    ```powershell
    
    $NIC = New-AzureRmNetworkInterface `
    -Name MyNIC `
    -ResourceGroupName $RgName `
    -Location $Location `
    -NetworkSecurityGroupId $NSG.Id `
    -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
    ```

    >[!NOTE]
    >Hoewel alle configuraties zijn toegewezen aan één NIC in dit artikel, kunt u meerdere IP-configuraties kunt toewijzen aan elke NIC die is gekoppeld aan de virtuele machine. Lees voor meer informatie over het maken van een virtuele machine met meerdere NIC's, de [een virtuele machine maken met meerdere NIC's](virtual-network-deploy-multinic-arm-ps.md) artikel.

9. De virtuele machine maken door te voeren van de volgende opdrachten:

    ```powershell
    
    # Define a credential object. When you run these commands, you're prompted to enter a sername and password for the VM you're reating.
    $cred = Get-Credential
    
    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
    -VMName MyVM `
    -VMSize Standard_DS1_v2 | `
    Set-AzureRmVMOperatingSystem -Windows `
    -ComputerName MyVM `
    -Credential $cred | `
    Set-AzureRmVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
    Add-AzureRmVMNetworkInterface `
    -Id $NIC.Id
    
    # Create the VM
    New-AzureRmVM `
    -ResourceGroupName $RgName `
    -Location $Location `
    -VM $VmConfig
    ```

10. De particuliere IP-adressen toevoegen aan het besturingssysteem van de virtuele machine via de stappen voor het besturingssysteem in de [toevoegen IP-adressen naar een VM-besturingssysteem](#os-config) sectie van dit artikel. Het openbare IP-adressen niet aan het besturingssysteem toevoegen.

## <a name="add"></a>IP-adressen toevoegen aan een virtuele machine

U kunt persoonlijke en openbare IP-adressen toevoegen aan een NIC via de stappen volgen. De voorbeelden in de volgende secties wordt ervan uitgegaan dat er al een virtuele machine met de drie IP-configuraties beschreven in de [scenario](#Scenario) in dit artikel, maar het is niet vereist dat u doen.

1. Open een PowerShell-opdrachtprompt en voer de overige stappen in deze sectie binnen één PowerShell-sessie. Als u nog niet PowerShell geïnstalleerd en geconfigureerd, voer de stappen in de [installeren en configureren van Azure PowerShell](/powershell/azure/overview) artikel.
2. "Waarden" van de volgende $Variables wijzigen in de naam van de NIC die u wilt toevoegen, IP-adres en de resourcegroep en locatie van die de NIC bestaat in:

    ```powershell
    $NicName  = "MyNIC"
    $RgName   = "MyResourceGroup"
    $Location = "westus"
    ```

    Als u de naam van de NIC die u wilt wijzigen, voert de volgende opdrachten niet weet, klikt u vervolgens de waarden van de vorige variabelen te wijzigen:

    ```powershell
    Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location
    ```
3. Maken van een variabele en stel deze in op de bestaande NIC door de volgende opdracht te typen:

    ```powershell
    $MyNIC = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName
    ```
4. Wijzig in de volgende opdrachten *MyVNet* en *MySubnet* tot de namen van de VNet en de NIC is verbonden met subnet. Voer de opdrachten voor het ophalen van de VNet en subnet objecten die de NIC is verbonden met:

    ```powershell
    $MyVNet = Get-AzureRMVirtualnetwork -Name MyVNet -ResourceGroupName $RgName
    $Subnet = $MyVnet.Subnets | Where-Object { $_.Name -eq "MySubnet" }
    ```
    Als u de naam van het VNet of subnet die de NIC is verbonden met niet weet, voert u de volgende opdracht:
    ```powershell
    $MyNIC.IpConfigurations
    ```
    In de uitvoer is vergelijkbaar met de volgende voorbeelduitvoer tekst zoeken:
    
    ```
    "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet/subnets/MySubnet"
    ```
    In deze uitvoer *MyVnet* is het VNet en *MySubnet* is het de NIC is verbonden met subnet.

5. Volg de stappen in een van de volgende secties, op basis van uw vereisten:

    **Een persoonlijke IP-adres toevoegen**

    Als u wilt een particulier IP-adres toevoegen aan een NIC, moet u een IP-configuratie. De volgende opdracht maakt een configuratie met een statisch IP-adres 10.0.0.7. Wanneer u een statisch IP-adres opgeeft, moet dit een ongebruikt adres voor het subnet. Het raadzaam dat u eerst het adres om te controleren of deze beschikbaar is door te voeren test de `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` opdracht. De uitvoer geretourneerd als het IP-adres beschikbaar is, *True*. De uitvoer geretourneerd als deze niet beschikbaar is, *False*, en een lijst met adressen die beschikbaar zijn.

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
    $MyNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
    ```
    Configuraties die u nodig hebt, gebruik van unieke namen en privé IP-adressen (voor configuraties met statische IP-adressen) maken.

    De privé IP-adres toevoegen aan het besturingssysteem van de virtuele machine via de stappen voor het besturingssysteem in de [toevoegen IP-adressen naar een VM-besturingssysteem](#os-config) sectie van dit artikel.

    **Een openbaar IP-adres toevoegen**

    Een openbaar IP-adres wordt toegevoegd door het koppelen van een resource met openbare IP-adres aan een nieuwe IP-configuratie of een bestaande IP-configuratie. Voer de stappen in een van de secties die volgen, die u nodig hebt.

    > [!NOTE]
    > Openbare IP-adressen hebben een nominaal kosten. Lees meer informatie over prijzen voor IP-adres, de [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina. Er is een limiet aan het aantal openbare IP-adressen die kunnen worden gebruikt in een abonnement. Lees voor meer informatie over de limieten het artikel [Azure-limieten](../azure-subscription-service-limits.md#networking-limits).
    >

    - **Koppel het openbare IP-adres resource naar een nieuwe IP-configuratie**
    
        Als u een openbaar IP-adres in een nieuw IP-configuratie toevoegt, moet u ook een privé IP-adres toevoegen, omdat alle IP-configuraties moeten een particulier IP-adres hebben. U kunt een bestaande resource voor openbare IP-adres toevoegen of u een nieuwe maken. Voer de volgende opdracht om een nieuwe maken:
    
        ```powershell
        $myPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "myPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location `
        -AllocationMethod Static
        ```

        Een nieuwe IP-configuratie maken met een statisch privé IP-adres en de bijbehorende *myPublicIp3* openbaar IP adres resource, voer de volgende opdracht:

        ```powershell
        Add-AzureRmNetworkInterfaceIpConfig `
        -Name IPConfig-4 `
        -NetworkInterface $myNIC `
        -Subnet $Subnet `
        -PrivateIpAddress 10.0.0.7 `
        -PublicIpAddress $myPublicIp3
        ```

    - **Koppel het openbare IP-adres resource aan een bestaande IP-configuratie**

        Een openbare IP-adres resource kan alleen worden gekoppeld aan een IP-configuratie die nog geen die zijn gekoppeld. U kunt bepalen of een IP-configuratie een gekoppeld openbare IP-adres heeft met de volgende opdracht:

        ```powershell
        $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
        ```

        U ziet de uitvoer ziet er als volgt:

        ```     
        Name       PrivateIpAddress PublicIpAddress                                           Primary
        
        IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
        IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
        IpConfig-3 10.0.0.6                                                                     False
        ```

        Aangezien de **PublicIpAddress** kolom voor *IpConfig 3* is leeg, geen openbare IP-adres-resource is momenteel gekoppeld. U kunt een bestaande resource voor openbare IP-adres toevoegen aan IpConfig 3 of Voer de volgende opdracht een te maken:

        ```powershell
        $MyPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "MyPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location -AllocationMethod Static
        ```

        Voer de volgende opdracht om te koppelen van het openbare IP-adres resource aan de bestaande IP-configuratie met de naam *IpConfig 3*:
    
        ```powershell
        Set-AzureRmNetworkInterfaceIpConfig `
        -Name IpConfig-3 `
        -NetworkInterface $mynic `
        -Subnet $Subnet `
        -PublicIpAddress $myPublicIp3
        ```

6. Stel de NIC met de nieuwe IP-configuratie met de volgende opdracht:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $MyNIC
    ```

7. Bekijk de privé IP-adressen en openbare IP-adres resources toegewezen aan de NIC met de volgende opdracht:

    ```powershell   
    $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```
8. De privé IP-adres toevoegen aan het besturingssysteem van de virtuele machine via de stappen voor het besturingssysteem in de [toevoegen IP-adressen naar een VM-besturingssysteem](#os-config) sectie van dit artikel. Het openbare IP-adres niet aan het besturingssysteem toevoegen.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
