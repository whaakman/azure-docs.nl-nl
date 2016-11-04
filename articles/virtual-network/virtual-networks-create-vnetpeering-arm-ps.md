---
title: VNet-peering maken met Powershell-cmdlets | Microsoft Docs
description: Leer hoe u een virtueel netwerk maakt met de Azure Portal in Resource Manager.
services: virtual-network
documentationcenter: ''
author: NarayanAnnamalai
manager: jefco
editor: ''
tags: azure-resource-manager

ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: narayanannamalai; annahar

---
# VNet-peering maken met Powershell-cmdlets
[!INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[!INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Volg de onderstaande stappen om met behulp van PowerShell een VNet-peering te maken:

1. Als u Azure PowerShell nog niet eerder hebt gebruikt, raadpleegt u [Azure PowerShell installeren en configureren](../powershell-install-configure.md) en volgt u de instructies helemaal tot aan het einde om u aan te melden bij Azure en uw abonnement te selecteren.

> [!NOTE]
> De PowerShell-cmdlet voor het beheer van VNet-peering wordt geleverd met [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)
> 
> 

1. Virtueel-netwerkobjecten lezen:
   
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2
2. Voor het tot stand brengen van een VNET-peeringkoppeling moet u twee koppelingen maken, een voor elke richting. Met de volgende stap maakt u eerst een VNET-peeringkoppeling voor VNet1 naar VNet2:
   
        Add-AzureRmVirtualNetworkPeering -name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.id
   
    Weergave uitvoer:
   
        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null
3. Met deze stap maakt u een VNET-peeringkoppeling voor VNet2 naar VNet1:
   
        Add-AzureRmVirtualNetworkPeering -name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.id
   
    Weergave uitvoer:
   
        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null
4. Zodra de VNET-peeringkoppeling is gemaakt, ziet u hieronder de status van de koppeling:
   
        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2
   
    Weergave uitvoer:
   
        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
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
   
   | Optie | Beschrijving | Standaard |
   |:--- |:--- |:--- |
   | AllowVirtualNetworkAccess |Of de adresruimte van gelijkwaardige VNet moet worden opgenomen als onderdeel van de tag Virtual_network |Ja |
   | AllowForwardedTraffic |Zorgt ervoor dat verkeer dat niet afkomstig is van gelijkwaardig VNet wordt geaccepteerd of verwijderd |Nee |
   | AllowGatewayTransit |Hiermee kan de gelijkwaardige VNet uw VNet-gateway gebruiken |Nee |
   | UseRemoteGateways |Gebruik de VNet-gateway van uw peer. De gelijkwaardige VNet moeten een geconfigureerde gateway hebben en AllowGatewayTransit moet zijn geselecteerd. U kunt deze optie niet gebruiken als u een gateway hebt geconfigureerd |Nee |
   
    Elke koppeling in VNet-peering heeft de set eigenschappen die hierboven staan vermeld. U kunt bijvoorbeeld AllowVirtualNetworkAccess instellen op Waar voor de VNET-peeringkoppeling VNet1-naar-VNet2, en instellen op Onwaar voor de VNET-peeringkoppeling in de omgekeerde richting.
   
        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2
   
    U kunt Get-AzureRmVirtualNetworkPeering uitvoeren om de waarde van de eigenschap te controleren na de wijziging.  In de uitvoer ziet u dat AllowForwardedTraffic is gewijzigd in 'True' na het uitvoeren van de bovenstaande cmdlets.
   
        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
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

[!INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Volg de onderstaande stappen als u VNet-peering met behulp van PowerShell voor meerdere abonnementen wilt maken:

1. Meld u aan bij Azure met bevoegdheden van het account van gebruiker A voor abonnement A en voer de volgende cmdlet uit:
   
        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5
   
    Dit is geen vereiste. Peering kan ook tot stand worden gebracht als gebruikers afzonderlijk een peeringaanvraag voor hun respectieve Vnets genereren, mits de aanvragen met elkaar overeenkomen. Door de bevoegde gebruiker van het andere VNet toe te voegen als gebruiker in het lokale VNet, kunt u de peering gemakkelijker instellen.
2. Meld u aan bij Azure met bevoegdheden van het account van gebruiker B voor abonnement B en voer de volgende cmdlet uit:
   
        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3
3. Voer tijdens de sessie met aanmelding voor gebruiker A de volgende cmdlet uit:
   
        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3
   
        Add-AzureRmVirtualNetworkPeering -name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess
4. Voer tijdens de sessie met aanmelding voor gebruiker B de volgende cmdlet uit:
   
        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5
   
        Add-AzureRmVirtualNetworkPeering -name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess
5. Nadat peering tot stand is gebracht, kan elke virtuele machine in VNet3 communiceren met elke virtuele machine in VNet5.

[!INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

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

[!INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Voer onderstaande stappen uit om een VNet-peering te maken tussen een klassiek virtueel netwerk en een virtueel netwerk van Azure Resource Manager in PowerShell:

1. Lees het virtuele-netwerkobject voor **VNET1**, het virtuele netwerk van Azure Resource Manager als volgt: $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
2. U hebt slechts één koppeling nodig om VNet-peering in dit scenario in te schakelen, namelijk een koppeling van **VNET1** naar **VNET2**. Voor deze stap hebt u uw klassieke VNet-resource-ID nodig. De indeling van de resourcegroep-ID ziet er als volgt uit: /subscriptions/SubscriptionID/resourceGroups/ResourceGroupName/providers/Microsoft.ClassicNetwork/virtualNetworks/VirtualNetworkName
   
    Vervang SubscriptionID, ResourceGroupName en VirtualNetworkName door de juiste namen.
   
    Dit doet u als volgt:
   
        Add-AzureRmVirtualNetworkPeering -name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2
3. Zodra de VNet-peeringkoppeling is gemaakt, ziet u de status van de koppeling in de onderstaande uitvoer:
   
        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## VNet-peering verwijderen
1. Als u VNet-peering wilt verwijderen, moet u de volgende cmdlet uitvoeren:
   
       Remove-AzureRmVirtualNetworkPeering  
   
       remove both links, as shown below:
   
       Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
       Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
2. Nadat u een koppeling in VNET-peering hebt verwijderd, verandert de verbindingsstatus van de peer in Niet-verbonden. In deze status kunt u de koppeling niet opnieuw maken tot de verbindingsstatus van de peer wordt gewijzigd in Geïnitialiseerd. We raden aan dat u beide koppelingen verwijdert voordat u VNET-peering opnieuw tot stand brengt.

<!--HONumber=Oct16_HO1-->


