<properties
   pageTitle="VNet-peering maken met Powershell-cmdlets | Microsoft Azure"
   description="Leer hoe u een virtueel netwerk maakt met de Azure-portal in Resource Manager."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/02/2016"
   ms.author="narayanannamalai"/>

# VNet-peering maken met Powershell-cmdlets

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Volg de onderstaande stappen om met behulp van PowerShell een VNet-peering te maken:

1. Als u Azure PowerShell nog niet eerder hebt gebruikt, raadpleegt u [Azure PowerShell installeren en configureren](../powershell-install-configure.md) en volgt u de instructies helemaal tot aan het einde om u aan te melden bij Azure en uw abonnement te selecteren.

        Note: PowerShell cmdlet for managing VNet peering is shipped with [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Virtueel-netwerkobjecten lezen:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2
    
3. Voor het tot stand brengen van een VNET-peeringkoppeling moet u twee koppelingen maken, een voor elke richting. Met de volgende stap maakt u eerst een VNET-peeringkoppeling voor VNet1 naar VNet2:

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.id 

        Output shows:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Met deze stap maakt u een VNET-peeringkoppeling voor VNet2 naar VNet1:

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.id 

        Output shows:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Zodra de VNET-peeringkoppeling is gemaakt, ziet u hieronder de status van de koppeling:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

        Output shows:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Er zijn enkele configureerbare eigenschappen voor VNet-peering:

  	|Optie|Beschrijving|Standaard|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Of de adresruimte van gelijkwaardige VNet moet worden opgenomen als onderdeel van de tag Virtual_network|Ja|
  	|AllowForwardedTraffic|Zorgt ervoor dat verkeer dat niet afkomstig is van gelijkwaardig VNet wordt geaccepteerd of verwijderd|Nee|
  	|AllowGatewayTransit|Hiermee kan de gelijkwaardige VNet uw VNet-gateway gebruiken|Nee|
  	|UseRemoteGateways|Gebruik de VNet-gateway van uw peer. De gelijkwaardige VNet moeten een geconfigureerde gateway hebben en AllowGatewayTransit moet zijn geselecteerd. U kunt deze optie niet gebruiken als u een gateway hebt geconfigureerd|Nee|

    Elke koppeling in VNet-peering heeft de set eigenschappen die hierboven staan vermeld. U kunt bijvoorbeeld AllowVirtualNetworkAccess instellen op Waar voor de VNET-peeringkoppeling VNet1-naar-VNet2, en instellen op Onwaar voor de VNET-peeringkoppeling in de omgekeerde richting.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

        You can run Get-AzureRmVirtualNetworkPeering to double check the property value after the change.  From the output, you can see AllowForwardedTraffic changes set to True after running the above cmdlets. 

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Nadat peering in dit scenario tot stand is gebracht, moet u de verbindingen van elke virtuele machine naar elke andere virtuele machine kunnen initiëren in beide VNetten. Standaard is AllowVirtualNetworkAccess ingesteld op Waar en zal VNet de juiste ACL's inrichten zodat de communicatie tussen VNetten is toegestaan. U kunt nog steeds regels voor netwerkbeveiligingsgroepen toepassen om verbindingen tussen specifieke subnetten te blokkeren of te voorkomen dat virtuele machines gedetailleerde controle krijgen over de toegang tussen twee virtuele netwerken.  Raadpleeg dit [artikel](virtual-networks-create-nsg-arm-ps.md) voor meer informatie over het maken van regels voor netwerkbeveiligingsgroepen.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Volg de onderstaande stappen als u VNet-peering met behulp van PowerShell voor meerdere abonnementen wilt maken:

1. Meld u aan bij Azure met bevoegdheden van het account van gebruiker A voor abonnement A en voer de volgende cmdlet uit:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

        This is not a requirement, peering can be established even if users individually raise peering requests for thier respective Vnets as long as the requests match. Adding a privileged user of the other VNet as a user in the local VNet makes it easier to do the setup. 

2. Meld u aan bij Azure met bevoegdheden van het account van gebruiker B voor abonnement B en voer de volgende cmdlet uit:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. Voer tijdens de sessie met aanmelding voor gebruiker A de volgende cmdlet uit:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. Voer tijdens de sessie met aanmelding voor gebruiker B de volgende cmdlet uit:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Nadat peering tot stand is gebracht, kan elke virtuele machine in VNet3 communiceren met elke virtuele machine in VNet5.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In dit scenario kunt u de onderstaande PowerShell-cmdlets uitvoeren om VNet-peering tot stand te brengen.  U moet de eigenschap AllowForwardedTraffic instellen op Waar en VNET1 koppelen aan HubVnet, waardoor inkomend verkeer van buiten de VNet-peering-adresruimte mogelijk is. 

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.id

2. Nadat peering tot stand is gebracht, kunt u dit [artikel](virtual-network-create-udr-arm-ps.md) raadplegen en een door de gebruiker gedefinieerde route (UDR-route) definiëren om VNet1-verkeer via een virtueel apparaat om te leiden om de mogelijkheden ervan te gebruiken. Wanneer u het 'volgende hop'-adres in de route opgeeft, kunt u dit instellen op het IP-adres van het virtuele apparaat in het peer-VNet HubVNet. Hieronder staat een voorbeeld:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

## VNet-peering verwijderen

1.  Als u VNet-peering wilt verwijderen, moet u de volgende cmdlet uitvoeren: 

        Remove-AzureRmVirtualNetworkPeering  
    
        remove both links, as shown below:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2 
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2 

2. Nadat u een koppeling in VNET-peering hebt verwijderd, verandert de verbindingsstatus van de peer in Niet-verbonden. In deze status kunt u de koppeling niet opnieuw maken tot de verbindingsstatus van de peer wordt gewijzigd in Geïnitialiseerd. We raden aan dat u beide koppelingen verwijdert voordat u VNET-peering opnieuw tot stand brengt. 



<!--HONumber=ago16_HO4-->


