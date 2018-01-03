---
title: 'Configureer geforceerde tunneling voor Azure Site-naar-Site-verbindingen: klassieke | Microsoft Docs'
description: Het omleiden of 'force' alle internetverkeer terug naar uw on-premises locatie.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 5c0177f1-540c-4474-9b80-f541fa44240b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 79bf6892c823da282c3e763921e830f986419854
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Geforceerde tunneling met het klassieke implementatiemodel configureren

Geforceerde tunneling kunt u omleiding of 'force' alle internetverkeer terug naar uw on-premises locatie via een Site-naar-Site VPN-tunnel voor inspectie en controle. Dit is een kritieke beveiligingsvereiste voor de meeste bedrijven beleidsregels. Zonder geforceerde tunneling wordt internetverkeer van uw virtuele machines in Azure altijd passeren van Azure netwerkinfrastructuur rechtstreeks uit met het Internet, zonder de optie kunt u controleren of het verkeer controleren. Niet-geautoriseerde toegang tot het Internet kan mogelijk leiden tot vrijgeven van informatie of andere typen schendingen van de beveiliging.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

In dit artikel begeleidt u bij het configureren geforceerde tunneling voor virtuele netwerken die zijn gemaakt met behulp van het klassieke implementatiemodel. Geforceerde tunneling kan worden geconfigureerd met behulp van PowerShell, niet via de portal. Als u configureren voor het implementatiemodel van Resource Manager een geforceerde tunneling wilt, selecteert u de klassieke artikel uit de volgende vervolgkeuzelijst:

> [!div class="op_single_selector"]
> * [PowerShell - Klassiek](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a>Vereisten en overwegingen
Geforceerde tunneling in Azure wordt geconfigureerd via het virtuele netwerk zelfgedefinieerde routes (UDR). Intranetwebproxyserver verkeer omleidt naar een on-premises site wordt uitgedrukt als een standaardroute voor de Azure VPN-gateway. Het volgende gedeelte bevat de huidige beperking van de routering tabel en routes voor een Azure-netwerk:

* Elk virtueel netwerksubnet heeft een ingebouwd systeem-routeringstabel. De routeringstabel van het systeem heeft de volgende drie groepen van routes:

  * **Lokale VNet routes:** rechtstreeks aan de doel-virtuele machines in hetzelfde virtuele netwerk.
  * **Lokale routes:** naar de Azure VPN-gateway.
  * **Standaardroute:** rechtstreeks met het Internet. Pakketten die bestemd zijn voor de persoonlijke IP-adressen niet wordt gedekt door de vorige twee routes worden verwijderd.
* U kunt met het uitbrengen van de gebruiker gedefinieerde routes, een routeringstabel om toe te voegen een standaardroute maken en vervolgens de routeringstabel koppelen aan uw VNet subnetten met inschakelen van geforceerde tunneling op deze subnetten.
* U moet een "standaardsite instellen ' tussen de cross-premises lokale sites verbonden met het virtuele netwerk.
* Geforceerde tunneling moet worden gekoppeld aan een VNet met een VPN-gateway voor dynamische routering (geen statische gateway genoemd).
* ExpressRoute geforceerde tunneling via dit mechanisme niet is geconfigureerd, maar in plaats daarvan wordt ingeschakeld door kondigt een standaardroute via de ExpressRoute-BGP-peeringsessies. Zie de [ExpressRoute-documentatie](https://azure.microsoft.com/documentation/services/expressroute/) voor meer informatie.

## <a name="configuration-overview"></a>Configuratie-overzicht
In het volgende voorbeeld wordt het subnet wordt niet geforceerd Frontend tunnel. De werkbelastingen binnen het subnet Frontend kunnen blijven om te accepteren en rechtstreeks reageren op aanvragen van klanten via Internet. De middelste laag en back-end subnetten worden gedwongen via een tunnel. Uitgaande verbindingen van deze twee subnetten met het Internet worden gedwongen of omgeleid naar een on-premises site via een van de S2S VPN-tunnels.

Hiermee kunt u beperken en controleren van toegang tot Internet vanaf uw virtuele machines of cloudservices in Azure, terwijl u het inschakelen van uw service met meerdere lagen architectuur die is vereist. Ook kunt u toepassen geforceerde tunneling naar de volledige virtuele netwerken als er geen internetverbinding werkbelastingen in uw virtuele netwerken.

![Geforceerde tunneling](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a>Voordat u begint
Controleer of u beschikt over de volgende items voordat u begint met de configuratie.

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).
* Een geconfigureerde virtueel netwerk. 
* De meest recente versie van de Azure PowerShell-cmdlets. Zie [How to install and configure Azure PowerShell](/powershell/azure/overview) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van de PowerShell-cmdlets.

## <a name="configure-forced-tunneling"></a>Geforceerde tunneling configureren
De volgende procedure kunt u opgeven geforceerde tunneling voor een virtueel netwerk. De configuratiestappen die overeenkomen met het configuratiebestand van de VNet-netwerk.

```
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

In dit voorbeeld is het virtuele netwerk MultiTier-VNet drie subnetten: 'Frontend', 'Midtier' en 'Back-end' subnetten, met vier cross-premises verbindingen: 'DefaultSiteHQ' en drie vertakkingen. 

De stappen 'DefaultSiteHQ' ingesteld als de standaard site-verbinding voor geforceerde tunneling, en het Midtier configureren en back-end-subnetten gebruiken geforceerde tunneling.

1. Maak een routeringstabel. Gebruik de volgende cmdlet om de routetabel te maken.

  ```powershell
  New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
  ```
2. Een standaardroute toevoegen aan de routeringstabel. 

  Het volgende voorbeeld wordt een standaardroute toegevoegd aan de routeringstabel gemaakt in stap 1. De enige route ondersteund is een voorvoegsel voor de bestemming van '0.0.0.0/0' aan de 'VPNGateway' volgende hop.

  ```powershell
  Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
  ```
3. De routeringstabel naar de subnetten koppelen. 

  Nadat een routeringstabel wordt gemaakt en een route toegevoegd, gebruikt u het volgende voorbeeld toe te voegen of te koppelen van de routetabel met een VNet-subnet. Het voorbeeld wordt de routetabel 'MyRouteTable' toegevoegd aan de subnetten van VNet MultiTier-VNet met het Midtier- en back-end.

  ```powershell
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
  ```
4. Wijs een standaardlocatie voor geforceerde tunneling. 

  In de vorige stap de cmdlet voorbeeldscripts de routeringstabel gemaakt en de routetabel voor twee van de VNet-subnetten die zijn gekoppeld. De laatste stap is het selecteren van een lokale site tussen de verbindingen van meerdere sites van het virtuele netwerk als de standaardlocatie of tunnel.

  ```powershell
  $DefaultSite = @("DefaultSiteHQ")
  Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
  ```

## <a name="additional-powershell-cmdlets"></a>Aanvullende PowerShell-cmdlets
### <a name="to-delete-a-route-table"></a>Een routetabel verwijderen

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="to-list-a-route-table"></a>Voor een lijst met een routetabel

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="to-delete-a-route-from-a-route-table"></a>Een route verwijderen uit een routetabel

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="to-remove-a-route-from-a-subnet"></a>Een route verwijderen van een subnet

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Voor een lijst met de routetabel die zijn gekoppeld aan een subnet

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Een standaard-site verwijderen uit een VNet VPN-gateway

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```