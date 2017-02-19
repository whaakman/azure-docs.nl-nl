---
title: 'Uw on-premises netwerk verbinden met een virtueel Azure-netwerk: site-naar-site-VPN: portal | Microsoft Docs'
description: Stappen voor het maken van een IPSec-verbinding van uw on-premises netwerk met een virtueel Azure-netwerk via het openbare internet. Deze stappen helpen u een cross-premises site-naar-site-VPN-gatewayverbinding te maken met de portal.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: eea00841708212789e14fa8717d83dd81d472bac
ms.openlocfilehash: c99788aded849933289555dd133b8146d6b2bdf2


---
# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Een VNet met een site-naar-site-verbinding maken met Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Klassiek - Klassieke portal](vpn-gateway-site-to-site-create.md)
> 
> 

In dit artikel wordt stapsgewijs beschreven hoe u een virtueel netwerk en een site-naar-site-VPN-gatewayverbinding met uw on-premises netwerk maakt met behulp van het Azure Resource Manager-implementatiemodel en Azure Portal. Site-naar-site-verbindingen kunnen worden gebruikt voor cross-premises en hybride configuraties.

![Diagram: cross-premises site-naar-site-VPN-gatewayverbinding](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Implementatiemodellen en -methoden voor site-naar-site-verbindingen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

In de volgende tabel staan de momenteel beschikbare implementatiemodellen en -methoden voor site-naar-site-configuraties. Als er een artikel met configuratiestappen beschikbaar is, kunt u dit via een rechtstreekse koppeling in deze tabel raadplegen.

[!INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Aanvullende configuraties
Als u VNets met elkaar wilt verbinden, maar geen verbinding maakt met een on-premises locatie, raadpleeg dan [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) (Een VNet-naar-VNet-verbinding configureren). Zie [Een S2S-verbinding toevoegen aan een VNet met een bestaande VPN-gatewayverbinding](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) als u een site-naar-site-verbinding wilt toevoegen aan een VNet die al een verbinding heeft.

## <a name="before-you-begin"></a>Voordat u begint
Controleer voordat u met de configuratie begint of u de volgende items hebt.

* Een compatibel VPN-apparaat en iemand die dit kan configureren. Zie [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Over VPN-apparaten). Als u niet weet hoe u uw VPN-apparaat moet configureren of de IP-adresbereiken in uw on-premises netwerkconfiguratie niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken.
* Een extern gericht openbaar IP-adres voor het VPN-apparaat. Dit IP-adres kan zich niet achter een NAT bevinden.
* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](http://azure.microsoft.com/pricing/free-trial).

### <a name="a-namevaluesasample-configuration-values-for-this-exercise"></a><a name="values"></a>Voorbeeld van configuratiewaarden voor deze oefening
Wanneer u deze stappen uitvoert als oefening kunt u de volgende voorbeeldconfiguratiewaarden gebruiken:

* **VNet-naam:** TestVNet1
* **Adresruimte:** 10.11.0.0/16 en 10.12.0.0/16
* **Subnetten:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24
  * GatewaySubnet: 10.12.255.0/27
* **Resourcegroep: **TestRG1
* **Locatie:** VS - oost
* **DNS-server:** 8.8.8.8
* **Gatewaynaam:** VNet1GW
* **Openbare IP:** VNet1GWIP
* **VPN-type:** Op route gebaseerd
* **Verbindingstype:** site-naar-site (IPsec)
* **Gatewaytype:** VPN
* **Naam van lokale netwerkgateway:** Site2
* **Verbindingsnaam:** VNet1toSite2

## <a name="a-namecreatvneta1-create-a-virtual-network"></a><a name="CreatVNet"></a>1. Een virtueel netwerk maken
Als u al beschikt over een VNet, controleert u of de instellingen compatibel zijn met het ontwerp van de VPN-gateway. Let vooral op eventuele subnetten die met andere netwerken overlappen. Als u overlappende subnetten hebt, werkt de verbinding mogelijk niet goed. Als het VNet met de juiste instellingen is geconfigureerd, kunt u beginnen met de stappen in de sectie [Een DNS-server opgeven](#dns).

### <a name="to-create-a-virtual-network"></a>Een virtueel netwerk maken
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="a-namesubnetsa2-add-additional-address-space-and-subnets"></a><a name="subnets"></a>2. Extra adresruimte en subnetten toevoegen
Wanneer het VNet is gemaakt, kunt u er extra adresruimte en subnetten aan toevoegen.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="a-namednsa3-specify-a-dns-server"></a><a name="dns"></a>3. Een DNS-server opgeven
### <a name="to-specify-a-dns-server"></a>Een DNS-server opgeven
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="a-namegatewaysubneta4-create-a-gateway-subnet"></a><a name="gatewaysubnet"></a>4. Een gatewaysubnet maken
Voordat u het virtuele netwerk verbindt met een gateway, moet u eerst het gatewaysubnet maken voor het virtuele netwerk waarmee u verbinding wilt maken. Indien mogelijk is het beste een gatewaysubnet met een CIDR-blok van /28 of /27 te gebruiken zodat er voldoende IP-adressen zijn om aan toekomstige aanvullende configuratievereisten te voldoen.

Als u deze configuratie bij wijze van oefening maakt, gebruikt u deze [waarden](#values) wanneer u het gatewaysubnet maakt.

### <a name="to-create-a-gateway-subnet"></a>Een gatewaysubnet maken
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="a-namevnetgatewaya5-create-a-virtual-network-gateway"></a><a name="VNetGateway"></a>5. De gateway van een virtueel netwerk maken
Als u deze configuratie bij wijze van oefening maakt, kunt u deze [voorbeeldwaarden voor configuraties](#values) raadplegen.

### <a name="to-create-a-virtual-network-gateway"></a>De gateway van een virtueel netwerk maken
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="a-namelocalnetworkgatewaya6-create-a-local-network-gateway"></a><a name="LocalNetworkGateway"></a>6. Een lokale netwerkgateway maken
De 'lokale netwerkgateway' verwijst naar uw on-premises locatie. Geef de lokale netwerkgateway een naam waarmee Azure naar de gateway kan verwijzen. 

Als u deze configuratie bij wijze van oefening maakt, kunt u deze [voorbeeldwaarden voor configuraties](#values) raadplegen.

### <a name="to-create-a-local-network-gateway"></a>Een lokale netwerkgateway maken
[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="a-namevpndevicea7-configure-your-vpn-device"></a><a name="VPNDevice"></a>7. Uw VPN-apparaat configureren
[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="a-namecreateconnectiona8-create-a-site-to-site-vpn-connection"></a><a name="CreateConnection"></a>8. Een site-naar-site-VPN-verbinding maken
Maak de site-naar-site-VPN-verbinding tussen de gateway van uw virtuele netwerk en het VPN-apparaat. Zorg dat u de waarden vervangt door die van uzelf. De gedeelde sleutel moet overeenkomen met de waarde die u hebt gebruikt voor de configuratie van uw VPN-apparaat. 

Controleer voordat u met deze sectie begint of de gateway van het virtuele netwerk en de lokale netwerkgateways zijn voltooid. Als u deze configuratie bij wijze van oefening maakt, gebruikt u deze [waarden](#values) wanneer u de verbinding maakt.

### <a name="to-create-the-vpn-connection"></a>De VPN-verbinding maken
[!INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="a-nameverifyconnectiona9-verify-the-vpn-connection"></a><a name="VerifyConnection"></a>9. De VPN-verbinding controleren
U kunt de VPN-verbinding controleren in de portal of met behulp van PowerShell.

[!INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Volgende stappen
*  Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie.
*  Voor meer informatie over BGP raadpleegt u [BGP Overview](vpn-gateway-bgp-overview.md) (BGP-overzicht) en [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (BGP configureren).




<!--HONumber=Jan17_HO4-->


