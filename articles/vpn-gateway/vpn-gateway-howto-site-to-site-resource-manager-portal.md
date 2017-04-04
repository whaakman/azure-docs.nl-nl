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
ms.date: 03/23/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 80939bb48c29ba39e2d347cb80d6169d79329cfc
ms.lasthandoff: 03/25/2017


---
# <a name="create-a-site-to-site-connection-in-the-azure-portal"></a>Een site-naar-site-verbinding maken in Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Klassiek - Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klassiek - Klassieke portal](vpn-gateway-site-to-site-create.md)
>
>


Een site-naar-site-VPN-gatewayverbinding (S2S) is een verbinding via een VPN-tunnel met IPsec/IKE (IKEv1 of IKEv2). Voor dit type verbinding moet er een VPN-apparaat on-premises aanwezig zijn waaraan een openbaar IP-adres is toegewezen en dat zich niet achter een NAT bevindt. Site-naar-site-verbindingen kunnen worden gebruikt voor cross-premises en hybride configuraties.

In dit artikel wordt stapsgewijs beschreven hoe u een virtueel netwerk en een site-naar-site-VPN-gatewayverbinding met uw on-premises netwerk maakt met behulp van het Azure Resource Manager-implementatiemodel en Azure Portal. 

![Diagram: cross-premises site-naar-site-VPN-gatewayverbinding](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Implementatiemodellen en -methoden voor site-naar-site-verbindingen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

In de volgende tabel staan de momenteel beschikbare implementatiemodellen en -methoden voor site-naar-site-configuraties. Als er een artikel met configuratiestappen beschikbaar is, kunt u dit via een rechtstreekse koppeling in deze tabel raadplegen.

[!INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Aanvullende configuraties
Als u VNets met elkaar wilt verbinden, maar geen verbinding maakt met een on-premises locatie, raadpleeg dan [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) (Een VNet-naar-VNet-verbinding configureren). Zie [Een S2S-verbinding toevoegen aan een VNet met een bestaande VPN-gatewayverbinding](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) als u een site-naar-site-verbinding wilt toevoegen aan een VNet die al een verbinding heeft.

## <a name="before-you-begin"></a>Voordat u begint
Controleer voordat u met de configuratie begint of u de volgende items hebt.

* Een compatibel VPN-apparaat en iemand die dit kan configureren. Zie [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Over VPN-apparaten).
* Als u de IP-adresbereiken in uw on-premises netwerk niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken. Site-naar-site-verbindingen mogen geen overlappende adresruimten hebben.
* Een extern gericht openbaar IP-adres voor het VPN-apparaat. Dit IP-adres kan zich niet achter een NAT bevinden.
* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](http://azure.microsoft.com/pricing/free-trial).

### <a name="values"></a>Voorbeeldwaarden
Wanneer u deze stappen uitvoert als oefening, kunt u de volgende voorbeeldwaarden gebruiken:

* **VNet-naam:** TestVNet1
* **Adresruimte:** 
    * 10.11.0.0/16
    * 10.12.0.0/16 (optioneel voor deze oefening)
* **Subnetten:**
  * FrontEnd: 10.11.0.0/24
  * Back-end: 10.12.0.0/24 (optioneel voor deze oefening)
  * GatewaySubnet: 10.11.255.0/27
* **Resourcegroep:**TestRG1
* **Locatie:** VS - oost
* **DNS-server:** het IP-adres van de DNS-server
* **Gatewaynaam van het virtuele netwerk:** VNet1GW
* **Openbare IP:** VNet1GWIP
* **VPN-type:** Op route gebaseerd
* **Verbindingstype:** site-naar-site (IPsec)
* **Gatewaytype:** VPN
* **Naam van lokale netwerkgateway:** Site2
* **Verbindingsnaam:** VNet1toSite2

## <a name="CreatVNet"></a>1. Een virtueel netwerk maken

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-s2s-rm-portal-include.md)]

## <a name="dns"></a>2. Een DNS-server opgeven
DNS is niet vereist voor site-naar-site-verbindingen. Als u echter naamomzetting wilt instellen voor resources die in uw virtuele netwerk worden geïmplementeerd, moet u een DNS-server opgeven. Met deze instelling kunt u de DNS-server opgeven die u wilt gebruiken voor de naamomzetting voor dit virtuele netwerk. Hierdoor wordt geen DNS-server aangemaakt.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>3. Een gatewaysubnet maken
U moet een gatewaysubnet maken voor de VPN-gateway. Het gatewaysubnet bevat de IP-adressen waarvan de VPN-gatewayservices gebruik gaan maken. Maak indien mogelijk een gatewaysubnet met een CIDR-blok van /28 of /27. Zo weet u zeker dat u voldoende IP-adressen hebt om aan toekomstige gatewayconfiguratievereisten te voldoen.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-s2s-rm-portal-include.md)]

## <a name="VNetGateway"></a>4. De gateway van een virtueel netwerk maken

[!INCLUDE [vpn-gateway-add-gw-s2s-rm-portal](../../includes/vpn-gateway-add-gw-s2s-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>5. Een lokale netwerkgateway maken
De lokale netwerkgateway verwijst naar uw on-premises locatie. De instellingen die u voor de gateway van het lokale netwerk opgeeft, bepalen de adresruimten die worden doorgestuurd naar het on-premises VPN-apparaat.

[!INCLUDE [vpn-gateway-add-lng-s2s-rm-portal](../../includes/vpn-gateway-add-lng-s2s-rm-portal-include.md)]

## <a name="VPNDevice"></a>6. Uw VPN-apparaat configureren
[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>7. Een site-naar-site-VPN-verbinding maken

In deze stap maakt u de site-naar-site-VPN-verbinding tussen de gateway van uw virtuele netwerk en het on-premises VPN-apparaat. Controleer voordat u met deze sectie begint of de gateway van het virtuele netwerk en de lokale netwerkgateways zijn voltooid.

[!INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include.md)]

## <a name="VerifyConnection"></a>8. De VPN-verbinding controleren

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="next-steps"></a>Volgende stappen
*  Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie.
*  Voor meer informatie over BGP raadpleegt u [BGP Overview](vpn-gateway-bgp-overview.md) (BGP-overzicht) en [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (BGP configureren).


