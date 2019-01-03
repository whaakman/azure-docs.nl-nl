---
title: 'Uw on-premises netwerk verbinden met een Azure-netwerk: Site-naar-Site-VPN: Portal | Microsoft Docs'
description: Stappen voor het maken van een IPSec-verbinding van uw on-premises netwerk met een virtueel Azure-netwerk via het openbare internet. Deze stappen helpen u een cross-premises site-naar-site-VPN-gatewayverbinding te maken met de portal.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: cherylmc
ms.openlocfilehash: 032b6a4f5147d06a4613a827a0372437dca47f47
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53651636"
---
# <a name="create-a-site-to-site-connection-in-the-azure-portal"></a>Een site-naar-site-verbinding maken in Azure Portal

In dit artikel leest u hoe u Azure Portal gebruikt om een site-naar-site-VPN-gatewayverbinding te maken vanaf uw on-premises netwerk naar het VNet. De stappen in dit artikel zijn van toepassing op het Resource Manager-implementatiemodel. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure Portal (klassiek)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

Een site-naar-site-VPN-gatewayverbinding wordt gebruikt om een on-premises netwerk via een IPsec-/IKE-VPN-tunnel (IKEv1 of IKEv2) te verbinden met een virtueel Azure-netwerk. Voor dit type verbinding moet er on-premises een VPN-apparaat aanwezig zijn waaraan een extern openbaar IP-adres is toegewezen. Zie [Overzicht van VPN Gateway](vpn-gateway-about-vpngateways.md) voor meer informatie over VPN-gateways.

![Diagram: cross-premises site-naar-site-VPN-gatewayverbinding](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint of u aan de volgende criteria hebt voldaan:

* U hebt een compatibel VPN-apparaat nodig en iemand die dit kan configureren. Zie [Over VPN-apparaten](vpn-gateway-about-vpn-devices.md) voor meer informatie over compatibele VPN-apparaten en -apparaatconfiguratie.
* Controleer of u een extern gericht openbaar IPv4-adres voor het VPN-apparaat hebt.
* Als u de IP-adresbereiken in uw on-premises netwerkconfiguratie niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken. Wanneer u deze configuratie maakt, moet u de IP-adresbereikvoorvoegsels opgeven die Azure naar uw on-premises locatie doorstuurt. Geen van de subnetten van uw on-premises netwerk kan overlappen met de virtuele subnetten waarmee u verbinding wilt maken. 

### <a name="values"></a>Voorbeeldwaarden

In de voorbeelden in dit artikel worden de volgende waarden gebruikt. U kunt deze waarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel. Zie [Over instellingen voor VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md) voor meer informatie over instellingen voor VPN-gateways in het algemeen.

* **VNet-naam:** TestVNet1
* **Adresruimte:** 10.1.0.0/16
* **Abonnement:** Het abonnement dat u wilt gebruiken
* **Resourcegroep:** TestRG1
* **Locatie:** US - oost
* **Subnet:** FrontEnd: 10.1.0.0/24, BackEnd: 10.1.1.0/24 (optioneel voor deze oefening)
* **Naam Gatewaysubnet:** GatewaySubnet (dit wordt automatisch ingevuld in de portal)
* **Adresbereik Gatewaysubnet:** 10.1.255.0/27
* **DNS-Server:** 8.8.8.8 - optioneel. Het IP-adres van de DNS-server.
* **Gatewaynaam van het virtuele netwerk:** VNet1GW
* **Openbaar IP-adres:** VNet1GWIP
* **VPN-Type:** Op route gebaseerd
* **Verbindingstype:** Site-naar-site (IPsec)
* **Gatewaytype:** VPN
* **Naam van lokale netwerkgateway:** Site1
* **Verbindingsnaam:** VNet1toSite1
* **Gedeelde sleutel:** In dit voorbeeld gebruiken we abc123. Maar u kunt datgene gebruiken wat compatibel is met uw VPN-hardware. Het belangrijkste is dat de waarden aan beide zijden van de verbinding met elkaar overeenkomen.

## <a name="CreatVNet"></a>1. Een virtueel netwerk maken

[!INCLUDE [Create a virtual network](../../includes/vpn-gateway-create-virtual-network-portal-include.md)]

## <a name="dns"></a>2. Een DNS-server opgeven

DNS is niet vereist om site-naar-site-verbindingen te maken. Als u echter naamomzetting wilt instellen voor resources die in uw virtuele netwerk worden geïmplementeerd, moet u een DNS-server opgeven. Met deze instelling kunt u de DNS-server opgeven die u wilt gebruiken voor de naamomzetting voor dit virtuele netwerk. Hierdoor wordt geen DNS-server aangemaakt. Zie [Naamomzetting voor VM's en rolinstanties](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) voor meer informatie over naamomzetting.

[!INCLUDE [Specify a dns server - optional](../../includes/vpn-gateway-specify-dns-portal-include.md)]

## <a name="gatewaysubnet"></a>3. Her gatewaysubnet maken

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [Add a gateway subnet](../../includes/vpn-gateway-add-gateway-subnet-portal-include.md)]

[!INCLUDE [NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="VNetGateway"></a>4. De VPN-gateway maken

[!INCLUDE [Create a vpn gateway](../../includes/vpn-gateway-add-gateway-portal-include.md)]

## <a name="LocalNetworkGateway"></a>5. De lokale netwerkgateway maken

De lokale netwerkgateway verwijst doorgaans naar uw on-premises locatie. U geeft de site een naam waarmee Azure hiernaar kan verwijzen en geeft vervolgens het IP-adres op van het on-premises VPN-apparaat waarmee u verbinding maakt. U geeft ook de IP-adresvoorvoegsels op die via de VPN-gateway worden doorgestuurd naar het VPN-apparaat. De adresvoorvoegsels die u opgeeft, zijn de voorvoegsels die zich in uw on-premises netwerk bevinden. Als uw on-premises netwerk verandert of als u het openbare IP-adres voor het VPN-apparaat moet wijzigen, kunt u de waarden later eenvoudig bijwerken.

[!INCLUDE [Add a local network gateway](../../includes/vpn-gateway-add-local-network-gateway-portal-include.md)]

## <a name="VPNDevice"></a>6. Uw VPN-apparaat configureren

Voor site-naar-site-verbindingen met een on-premises netwerk is een VPN-apparaat vereist. In deze stap configureert u het VPN-apparaat. Bij de configuratie van uw VPN-apparaat hebt u het volgende nodig:

- Een gedeelde sleutel. Dit is dezelfde gedeelde sleutel die u opgeeft wanneer u uw site-naar-site-VPN-verbinding maakt. In onze voorbeelden gebruiken we een eenvoudige gedeelde sleutel. We raden u aan een complexere sleutel te genereren.
- Het openbare IP-adres van de gateway van uw virtuele netwerk. U kunt het openbare IP-adres weergeven met behulp van Azure Portal, PowerShell of de CLI. Navigeer naar **Virtuele netwerkgateways** en klik op de naam van uw VPN-gateway om het openbare IP-adres dat gebruikmaakt van Azure Portal te achterhalen.

[!INCLUDE [Configure a VPN device](../../includes/vpn-gateway-configure-vpn-device-include.md)]

## <a name="CreateConnection"></a>7. De VPN-verbinding maken

Maak de site-naar-site-VPN-verbinding tussen de gateway van uw virtuele netwerk en het on-premises VPN-apparaat.

[!INCLUDE [Add a site-to-site connection](../../includes/vpn-gateway-add-site-to-site-connection-portal-include.md)]

## <a name="VerifyConnection"></a>8. De VPN-verbinding controleren

[!INCLUDE [Verify the connection](../../includes/vpn-gateway-verify-connection-portal-include.md)]

## <a name="connectVM"></a>Verbinding maken met een virtuele machine

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <a name="reset"></a>Een VPN-gateway opnieuw instellen

Het opnieuw instellen van een Azure VPN-gateway is handig als u cross-premises VPN-connectiviteit verliest in een of meer Site-to-Site VPN-tunnels. In een dergelijke situatie functioneren al uw on-premises VPN-apparaten naar behoren, maar kunnen ze geen IPSec-tunnels tot stand brengen met de Azure VPN-gateways. Zie [Een VPN-gateway opnieuw instellen](vpn-gateway-resetgw-classic.md) voor de stappen.

## <a name="resize"></a>Een gateway-SKU wijzigen (formaat van een gateway wijzigen)

Zie [Gateway-SKU's](vpn-gateway-about-vpn-gateway-settings.md#gwsku) voor de stappen voor het wijzigen van een gateway-SKU.

## <a name="addconnect"></a>Een extra verbinding toevoegen aan een VPN-gateway

U kunt extra verbindingen toevoegen, mits geen van de adresruimten tussen verbindingen met elkaar overlappen.

1. Als u een extra verbinding wilt toevoegen, gaat u naar de VPN-gateway en klikt u vervolgens op **Verbindingen** om de pagina Verbindingen te openen.
2. Klik op **+Toevoegen** om uw verbinding toe te voegen. Pas het verbindingstype zodanig aan dat dit overeenkomt met VNet-naar-VNet (als u verbinding wilt maken met een andere VNet-gateway) of met site-naar-site.
3. Als u verbinding maakt met behulp van site-naar-site maar nog geen lokale netwerkgateway hebt gemaakt, kunt u een nieuwe maken.
4. Geef op welke gedeelde sleutel u wilt gebruiken en klik vervolgens op **OK** om de verbinding te maken.

## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie over BGP raadpleegt u [BGP Overview](vpn-gateway-bgp-overview.md) (BGP-overzicht) en [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (BGP configureren).
* Zie [Informatie over geforceerde tunneling](vpn-gateway-forced-tunneling-rm.md) voor meer informatie over geforceerde tunneling.
* Zie [Maximaal beschikbare cross-premises en VNet-naar-VNet-connectiviteit](vpn-gateway-highlyavailable.md) voor meer informatie over maximaal beschikbare actieve verbindingen.
* Zie [Netwerkbeveiliging](../virtual-network/security-overview.md) voor informatie over het beperken van netwerkverkeer tot resources in een virtueel netwerk.
* Zie [Routering van verkeer in virtuele netwerken](../virtual-network/virtual-networks-udr-overview.md) voor informatie over hoe Azure verkeer routeert tussen Azure-resources, on-premises resources en resources op internet.
* Zie [Een site-naar-site-VPN-verbinding maken](https://azure.microsoft.com/resources/templates/101-site-to-site-vpn-create/) voor meer informatie over het maken van een site-naar-site-VPN-verbinding met behulp van een Azure Resource Manager-sjabloon.
* Zie [HBase-geo-replicatie implementeren](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-geo/) voor meer informatie over het maken van een VNet-naar-VNet-VPN-verbinding met behulp van een Azure Resource Manager-sjabloon.
