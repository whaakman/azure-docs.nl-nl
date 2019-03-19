---
title: 'Geforceerde tunneling voor Azure Site-naar-Site-verbindingen configureren: klassieke | Microsoft Docs'
description: Het omleiden of 'afdwingen' alle internetverkeer terug naar uw on-premises locatie.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: 5c0177f1-540c-4474-9b80-f541fa44240b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 0955d95ebfd9e1f72ed1da577bf3520a70b71624
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58008327"
---
# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Geforceerde tunneling met het klassieke implementatiemodel configureren

Geforceerde tunneling kunt u omleiding of 'force' alle internetverkeer terug naar uw on-premises locatie via een Site-naar-Site VPN-tunnel voor inspectie en controle. Dit is een essentiële vereiste voor de meeste bedrijven beleid. Zonder geforceerde tunnels wordt via internetverkeer van uw virtuele machines in Azure altijd van Azure network-infrastructuur rechtstreeks uit met het Internet, zonder de optie waarmee u kunt controleren of het verkeer controleren. Niet-geautoriseerde toegang tot Internet kan mogelijk leiden tot vrijgeven van informatie of andere typen schendingen van de beveiliging.

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

In dit artikel begeleidt u bij het configureren geforceerde tunneling voor virtuele netwerken die zijn gemaakt met het klassieke implementatiemodel. Geforceerde tunneling kan worden geconfigureerd met behulp van PowerShell, niet via de portal. Als u configureren met geforceerde tunnels zijn voor het Resource Manager-implementatiemodel wilt, selecteert u de Resource Manager-artikel in de volgende vervolgkeuzelijst:

> [!div class="op_single_selector"]
> * [PowerShell - Klassiek](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a>Vereisten en overwegingen
Geforceerde tunneling in Azure wordt geconfigureerd via het virtuele netwerk zelfgedefinieerde routes (UDR). Verkeer omleiden naar een on-premises site wordt uitgedrukt als een standaard-Route naar de Azure VPN-gateway. Het volgende gedeelte bevat de huidige beperking van de tabel en routes de routering voor een Azure-netwerk:

* Elk virtueel netwerksubnet heeft een ingebouwde, systeem-routeringstabel. De routeringstabel van het systeem heeft de volgende drie groepen van routes:

  * **Lokale VNet routes:** Rechtstreeks naar de bestemming VM's in hetzelfde virtuele netwerk.
  * **On-premises routes:** Met Azure VPN-gateway.
  * **Standaard-route:** Rechtstreeks met Internet. Pakketten dat is bestemd voor het particuliere IP-adressen niet wordt gedekt door de vorige twee routes verwijderd.
* U kunt met de release van de gebruiker gedefinieerde routes maken een routeringstabel om toe te voegen een standaardroute en koppel vervolgens de routeringstabel naar de subnetten van uw VNet om in te schakelen geforceerde tunneling op deze subnetten.
* U moet een 'standaard-site"tussen de cross-premises lokale sites die zijn verbonden met het virtuele netwerk instellen.
* Geforceerde tunneling moet worden gekoppeld aan een VNet met een VPN-gateway voor dynamische routering (niet een statische gateway genoemd).
* ExpressRoute geforceerde tunneling is niet geconfigureerd via dit mechanisme, maar in plaats daarvan wordt ingeschakeld door kondigt een standaardroute via de ExpressRoute-BGP-peeringsessies. Raadpleeg de [documentatie voor ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) voor meer informatie.

## <a name="configuration-overview"></a>Configuratie-overzicht
In het volgende voorbeeld wordt tunnel de front-end subnet niet gedwongen. De werkbelastingen in het subnet Frontend kunnen blijven om te accepteren en rechtstreeks reageren op aanvragen van klanten via Internet. De middelste laag en back-end-subnetten zijn geforceerde tunnels. Uitgaande verbindingen vanuit deze twee subnetten met het Internet wordt afgedwongen of keert u terug naar een on-premises site via een van de S2S VPN-tunnels.

Hiermee kunt u om te beperken en controleren van toegang tot het Internet van uw virtuele machines of cloudservices in Azure, terwijl u verdergaat met het inschakelen van uw service met meerdere lagen architectuur die is vereist. Ook kunt u toepassen geforceerde tunneling naar de volledige virtuele netwerken als er geen internetverbinding werkbelastingen in uw virtuele netwerken.

![Geforceerde tunneling](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a>Voordat u begint
Controleer of u beschikt over de volgende items voordat u begint met de configuratie.

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).
* Een geconfigureerde virtueel netwerk. 
* De meest recente versie van de Azure PowerShell-cmdlets. Zie [How to install and configure Azure PowerShell](/powershell/azure/overview) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van de PowerShell-cmdlets.

## <a name="configure-forced-tunneling"></a>Geforceerde tunneling configureren
De volgende procedure ziet u een geforceerde tunnels voor een virtueel netwerk opgeven. De configuratiestappen komen overeen met het configuratiebestand van de VNet-netwerk.

```xml
<VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>
```

In dit voorbeeld is het virtuele netwerk MultiTier-VNet drie subnetten: Subnetten 'Frontend', 'Midtier' en 'Back-end', met vier cross-premises verbindingen: 'DefaultSiteHQ' en drie vertakkingen. 

De stappen wordt de 'DefaultSiteHQ' ingesteld als de standaard-site-verbinding voor geforceerde tunneling en het Midtier configureren en back-end-subnetten gebruik van geforceerde tunneling.

1. Maak een routeringstabel. Gebruik de volgende cmdlet om de routetabel te maken.

   ```powershell
   New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
   ```

2. Een standaard-route toevoegen aan de routeringstabel. 

   Het volgende voorbeeld wordt een standaard-route toegevoegd aan de routeringstabel in stap 1 hebt gemaakt. Houd er rekening mee dat de route alleen wordt ondersteund, is het voorvoegsel voor bestemming '0.0.0.0/0' aan de 'VPN-gateway' volgende hop.

   ```powershell
   Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
   ```

3. De routeringstabel naar de subnetten koppelen. 

   Nadat een routeringstabel wordt gemaakt en een route toegevoegd, gebruikt u het volgende voorbeeld toe te voegen of de routetabel aan een VNet-subnet koppelen. Het voorbeeld wordt de routetabel 'MyRouteTable' toegevoegd aan het Midtier- en back-end-subnetten van VNet MultiTier-VNet.

   ```powershell
   Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
   Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
   ```

4. Een standaardsite toewijzen voor geforceerde tunneling. 

   In de vorige stap, de cmdlet voorbeeldscripts gemaakt van de routeringstabel en de routetabel op twee van de VNet-subnetten die zijn gekoppeld. De laatste stap is om te selecteren van een lokale site tussen de verbindingen van meerdere sites van het virtuele netwerk als de standaard-site of -tunnel.

   ```powershell
   $DefaultSite = @("DefaultSiteHQ")
   Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
   ```

## <a name="additional-powershell-cmdlets"></a>Andere PowerShell-cmdlets
### <a name="to-delete-a-route-table"></a>Een routetabel verwijderen

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="to-list-a-route-table"></a>Om een routetabel weer te geven

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="to-delete-a-route-from-a-route-table"></a>Een route van een routetabel verwijderen

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="to-remove-a-route-from-a-subnet"></a>Een route van een subnet verwijderen

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Om de routetabel die zijn gekoppeld aan een subnet weer te geven

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Een standaard-site verwijderen uit een VNet-VPN-gateway

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```
