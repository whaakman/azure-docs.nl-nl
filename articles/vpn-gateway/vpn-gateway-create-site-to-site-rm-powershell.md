---
title: 'Uw on-premises netwerk verbinden met een virtueel Azure-netwerk: site-naar-site-VPN: PowerShell | Microsoft Docs'
description: Stappen voor het maken van een IPSec-verbinding van uw on-premises netwerk met een virtueel Azure-netwerk via het openbare internet. Deze stappen helpen u een cross-premises site-naar-site-VPN-gatewayverbinding te maken met PowerShell.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fcc2fda5-4493-4c15-9436-84d35adbda8e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/24/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: 4ec11ffeae94b4a8e5a65566f0f0c067f45a0134
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a>Een VNet met een site-naar-site-VPN-verbinding maken met PowerShell

In dit artikel leest u hoe u PowerShell gebruikt om een site-naar-site-VPN-gatewayverbinding te maken vanaf uw lokale netwerk naar het VNet. De stappen in dit artikel zijn van toepassing op het Resource Manager-implementatiemodel. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager - CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Klassiek - Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klassiek - klassieke portal](vpn-gateway-site-to-site-create.md)
> 
>


![Diagram: cross-premises site-naar-site-VPN-gatewayverbinding](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-connection-diagram.png)

Een site-naar-site-VPN-gatewayverbinding wordt gebruikt om een on-premises netwerk via een IPsec-/IKE-VPN-tunnel (IKEv1 of IKEv2) te verbinden met een virtueel Azure-netwerk. Voor dit type verbinding moet er on-premises een VPN-apparaat aanwezig zijn waaraan een extern openbaar IP-adres is toegewezen. Zie [Overzicht van VPN Gateway](vpn-gateway-about-vpngateways.md) voor meer informatie over VPN-gateways.

## <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint of u aan de volgende criteria hebt voldaan:

* U hebt vastgesteld dat u wilt werken met het Resource Manager-implementatiemodel. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Een compatibel VPN-apparaat en iemand die dit kan configureren. Zie [Over VPN-apparaten](vpn-gateway-about-vpn-devices.md) voor meer informatie over compatibele VPN-apparaten en -apparaatconfiguratie.
* Een extern gericht openbaar IPv4-adres voor het VPN-apparaat. Dit IP-adres kan zich niet achter een NAT bevinden.
* Als u de IP-adresbereiken in uw on-premises netwerkconfiguratie niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken. Wanneer u deze configuratie maakt, moet u de IP-adresbereikvoorvoegsels opgeven die Azure naar uw on-premises locatie doorstuurt. Geen van de subnetten van uw on-premises netwerk kan overlappen met de virtuele subnetten waarmee u verbinding wilt maken.
* De meest recente versie van de PowerShell-cmdlets van Azure Resource Manager. Zie [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="example-values"></a>Voorbeeldwaarden

In de voorbeelden in dit artikel worden de volgende waarden gebruikt. U kunt deze waarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel.

```
#Example values

VnetName                = testvnet 
ResourceGroup           = testrg 
Location                = West US 
AddressSpace            = 10.0.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.0.1.0/28 
GatewaySubnet           = 10.0.0.0/27
LocalNetworkGatewayName = LocalSite
LNG Public IP           = <VPN device IP address> 
Local Address Prefixes  = 10.0.0.0/24','20.0.0.0/24
Gateway Name            = vnetgw1
PublicIP                = gwpip
Gateway IP Config       = gwipconfig1 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = myGWConnection
```

## <a name="Login"></a>1. Verbinding maken met uw abonnement
Zorg ervoor dat u overschakelt naar de PowerShell-modus als u de Resource Manager-cmdlets wilt gebruiken. Zie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

1. Open de PowerShell-console en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

  ```powershell
  Login-AzureRmAccount
  ```
2. Controleer de abonnementen voor het account.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Geef het abonnement op dat u wilt gebruiken.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```

## <a name="VNet"></a>2. Een virtueel netwerk en een gatewaysubnet maken

Als u nog geen virtueel netwerk hebt, maakt u er een. Controleer bij het maken van een virtueel netwerk of de adresruimten die u opgeeft, niet overlappen met adresruimten in uw on-premises netwerk. Voor deze configuratie hebt u ook een gatewaysubnet nodig. De virtuele netwerkgateway gebruikt een gatewaysubnet dat de IP-adressen bevat die door de VPN-gatewayservices worden gebruikt. Wanneer u een gatewaysubnet maakt, moet dit de naam GatewaySubnet krijgen. Als u een andere naam kiest, maakt u wel een subnet, maar wordt dit door Azure niet als een gatewaysubnet behandeld.

De grootte van het gatewaysubnet dat u opgeeft, is afhankelijk van de VPN-gatewayconfiguratie die u wilt maken. Het is mogelijk om een klein gatewaysubnet van /29 te maken, maar we raden u aan een groter subnet met meer adressen te maken door /27 of /28 te selecteren. Met het grotere gatewaysubnet beschikt u over voldoende IP-adressen voor mogelijke toekomstige configuraties.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Een virtueel netwerk en een gatewaysubnet maken

In dit voorbeeld worden een virtueel netwerk en een gatewaysubnet gemaakt. Zie [Een gatewaysubnet toevoegen aan een virtueel netwerk dat u al hebt gemaakt](#gatewaysubnet) als u al een virtueel netwerk hebt waaraan u een gatewaysubnet wilt toevoegen.

Een resourcegroep maken:

```powershell
New-AzureRmResourceGroup -Name testrg -Location 'West US'
```

Maak uw virtuele netwerk. 

1. Stel de variabelen in.

  ```powershell
  $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/27
  $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'
  ```
2. Maak het VNet.

  ```powershell
  New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
  -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2
  ```

### <a name="gatewaysubnet"></a>Een gatewaysubnet toevoegen aan een virtueel netwerk dat u al hebt gemaakt

1. Stel de variabelen in.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet
  ```
2. Maak het gatewaysubnet.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/27 -VirtualNetwork $vnet
  ```
3. Stel de configuratie in.

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

## 3. <a name="localnet"></a>De lokale netwerkgateway maken

De lokale netwerkgateway verwijst doorgaans naar uw on-premises locatie. U geeft de site een naam waarmee Azure hiernaar kan verwijzen en geeft vervolgens het IP-adres op van het on-premises VPN-apparaat waarmee u verbinding maakt. U geeft ook de IP-adresvoorvoegsels op die via de VPN-gateway worden doorgestuurd naar het VPN-apparaat. De adresvoorvoegsels die u opgeeft, zijn de voorvoegsels die zich in uw on-premises netwerk bevinden. Als uw on-premises netwerk verandert, kunt u de voorvoegsels eenvoudig bijwerken.

Gebruik de volgende waarden:

* *GatewayIPAddress* is het IP-adres van uw on-premises VPN-apparaat. Het VPN-apparaat kan zich niet achter een NAT bevinden.
* *AddressPrefix* is uw on-premises adresruimte.

- Ga als volgt te werk als u een lokale netwerkgateway met één adresvoorvoegsel wilt toevoegen:

  ```powershell
  New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
  -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix   '10.0.0.0/24'
  ```

- Ga als volgt te werk als u een lokale netwerkgateway met meerdere adresvoorvoegsels wilt toevoegen:

  ```powershell
  New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
  -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')
  ```

- IP-adresvoorvoegsels wijzigen voor uw lokale netwerkgateway:<br>
Soms veranderen de voorvoegsels voor uw lokale netwerkgateway. De stappen waarmee u de IP-adresvoorvoegsels moet wijzigen, zijn afhankelijk van het feit of u een VPN-gatewayverbinding hebt gemaakt. Raadpleeg de sectie [Adresvoorvoegsels voor een lokale netwerkgateway wijzigen](#modify) van dit artikel.

## <a name="PublicIP"></a>4. Een openbaar IP-adres aanvragen

Vraag een openbaar IP-adres aan dat moet worden toegewezen aan uw virtuele VPN-gateway. Dit is het IP-adres dat u configureert voor het maken van verbinding met uw VPN-apparaat.

De virtuele netwerkgateway voor het Resource Manager-implementatiemodel ondersteunt momenteel alleen openbare IP-adressen met behulp van de dynamische toewijzingsmethode. Dit betekent echter niet dat het IP-adres verandert. Het IP-adres van de VPN-gateway verandert alleen wanneer de gateway wordt verwijderd en opnieuw wordt gemaakt. Het openbare IP-adres van de virtuele netwerkgateway verandert niet wanneer de grootte van uw VPN-gateway verandert, wanneer deze gateway opnieuw wordt ingesteld of wanneer er andere interne onderhoudswerkzaamheden of upgrades worden uitgevoerd.

Gebruik het volgende PowerShell-voorbeeld:

```powershell
$gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>5. De IP-adresseringsconfiguratie voor de gateway maken
De gatewayconfiguratie bepaalt welk subnet en openbaar IP-adres moeten worden gebruikt. Gebruik het volgende voorbeeld om de gatewayconfiguratie te maken:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```

## <a name="CreateGateway"></a>6. De VPN-gateway maken

Maak de VPN-gateway van het virtuele netwerk. Het maken van een VPN-gateway kan 45 minuten of langer duren.

Gebruik de volgende waarden:

* Het *-GatewayType* voor een site-naar-site-configuratie is *VPN*. Het gatewaytype is altijd specifiek voor de configuratie die u implementeert. Andere gatewayconfiguraties vereisen misschien -GatewayType ExpressRoute.
* Het *-VpnType* kan *RouteBased* (in sommige documentatie een dynamische gateway genoemd) of *PolicyBased* (in sommige documentatie een statische gateway genoemd) zijn. Voor meer informatie over VPN-gatewaytypen raadpleegt u [Over VPN Gateway](vpn-gateway-about-vpngateways.md).
* De *-GatewaySku* kan Basic, Standard of HighPerformance zijn. Voor bepaalde SKU's gelden configuratiebeperkingen. Zie [Gateway-SKU's](vpn-gateway-about-vpngateways.md#gateway-skus) voor meer informatie.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku Standard
```

## <a name="ConfigureVPNDevice"></a>7. Uw VPN-apparaat configureren

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

Gebruik het volgende voorbeeld om het openbare IP-adres van uw virtuele netwerkgateway te vinden met behulp van PowerShell:

```powershell
Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG
```

## <a name="CreateConnection"></a>8. De VPN-verbinding maken
Maak vervolgens de site-naar-site-VPN-verbinding tussen de gateway van uw virtuele netwerk en het VPN-apparaat. Zorg dat u de waarden vervangt door die van uzelf. De gedeelde sleutel moet overeenkomen met de waarde die u hebt gebruikt voor de configuratie van uw VPN-apparaat. Het '-ConnectionType' voor site-naar-site is *IPsec*.

1. Stel de variabelen in.
  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
  $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
  ```

2. Maak de verbinding.
  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName testrg `
  -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

Na een korte tijd wordt de verbinding tot stand gebracht.

## <a name="toverify"></a>9. De VPN-verbinding controleren
Er zijn een aantal verschillende manieren om uw VPN-verbinding te controleren.

[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="modify"></a>IP-adresvoorvoegsels wijzigen voor de gateway van een lokaal netwerk
Volg de onderstaande instructies als u de voorvoegsels voor de gateway van een lokaal netwerk wilt wijzigen. Er zijn twee sets met instructies. Welke instructies u kiest, is afhankelijk van de vraag of u uw gatewayverbinding al hebt gemaakt.

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Het IP-adres van de gateway wijzigen in dat van een lokale netwerkgateway
[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Volgende stappen
*  Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie.
* Voor meer informatie over BGP raadpleegt u [BGP Overview](vpn-gateway-bgp-overview.md) (BGP-overzicht) en [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (BGP configureren).

