---
title: 'Een virtueel Azure-netwerk verbinden met een ander VNet: portal | Microsoft Docs'
description: Een VPN-gatewayverbinding maken tussen VNets met behulp van Resource Manager en Azure Portal.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a7015cfc-764b-46a1-bfac-043d30a275df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/29/2017
ms.author: cherylmc
ms.openlocfilehash: 406cb4faf53bde5f615593e2e904d91a1d90a729
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-the-azure-portal"></a>Een VPN-gatewayverbinding tussen VNets configureren met behulp van Azure Portal

In dit artikel leest u hoe u virtuele netwerken verbindt met behulp van het verbindingstype VNet-naar-VNet. De virtuele netwerken kunnen zich in dezelfde of verschillende regio's bevinden en tot dezelfde of verschillende abonnementen behoren. Wanneer u VNets uit verschillende abonnementen koppelt, hoeven de abonnementen niet aan dezelfde Active Directory-tenant gekoppeld te zijn. 

De stappen in dit artikel zijn van toepassing op het Resource Manager-implementatiemodel en maken gebruik van Azure Portal. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure-CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure Portal (klassiek)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Verbinding maken tussen verschillende implementatiemodellen - Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Verbinding maken tussen verschillende implementatiemodellen - PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![v2v-diagram](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

## <a name="about"></a>Over het verbinden van VNet's

Er zijn meerdere manieren om VNet's te koppelen. In de onderstaande secties worden verschillende manieren beschreven voor het koppelen van virtuele netwerken.

### <a name="vnet-to-vnet"></a>VNet-naar-VNet

Het configureren van een VNet-naar-VNet-verbinding is een goede manier om VNet's te koppelen. Het verbinden van een virtueel netwerk met een ander virtueel netwerk met behulp van het VNet-naar-VNet-verbindingstype (VNet2VNet) is vergelijkbaar met het maken van een site-naar-site-IPsec-verbinding naar een on-premises locatie. Voor beide verbindingstypen wordt een VPN-gateway gebruikt om een beveiligde tunnel met IPsec/IKE te bieden en beide typen werken tijdens het communiceren op dezelfde manier. Het verschil tussen de verbindingstypen is de manier waarop de lokale netwerkgateway is geconfigureerd. Als u een VNet-naar-VNet-verbinding maakt, ziet u de adresruimte voor het lokale netwerkgateway niet. Deze wordt automatisch gemaakt en ingevuld. Wanneer u de adresruimte voor een VNet bijwerkt, is de route naar de bijgewerkte adresruimte automatisch bekend voor het andere VNet. Als u een VNet-naar-VNet-verbinding maakt, is dit gewoonlijk sneller en makkelijker dan een site-naar-site-verbinding tussen VNet's.

### <a name="site-to-site-ipsec"></a>Site-naar-site (IPsec)

Als u met een gecompliceerde netwerkconfiguratie werkt, kunt u de VNet's wellicht beter koppelen met behulp van de stappen voor [site-naar-site](vpn-gateway-howto-site-to-site-resource-manager-portal.md). Als u de stappen voor site-naar-site-IPsec gebruikt, maakt en configureert u de lokale netwerkgateways handmatig. De lokale netwerkgateway voor elke VNet behandelt de andere VNet als een lokale site. Hiermee kunt u extra adresruimte voor de lokale netwerkgateway opgeven om het verkeer te routeren. Als de adresruimte voor een VNet wordt gewijzigd, moet u de bijbehorende lokale netwerkgateway bijwerken om de wijziging door te voeren. Er vindt niet automatisch een update plaats.

### <a name="vnet-peering"></a>VNet-peering

U kunt overwegen uw VNet's te koppelen door middel van VNET-peering. Bij VNET-peering wordt geen VPN-gateway gebruikt en er gelden diverse beperkingen voor. Bovendien worden de [prijzen voor VNET-peering](https://azure.microsoft.com/pricing/details/virtual-network) anders berekend dan voor [VNet-naar-VNet-VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway). Zie het artikel [VNet-peering](../virtual-network/virtual-network-peering-overview.md) voor meer informatie.

## <a name="why"></a>Waarom een VNet-met-VNet-verbinding maken?

U kunt omwille van de volgende redenen virtuele netwerken koppelen met een VNet-naar-VNet-verbinding:

* **Geografische redundantie en aanwezigheid tussen regio's**

  * U kunt uw eigen geo-replicatie of synchronisatie met beveiligde connectiviteit instellen zonder gebruik te maken van internetgerichte eindpunten.
  * Met Azure Traffic Manager en Load Balancer kunt u workloads met maximale beschikbaarheid instellen met behulp van geografische redundantie over meerdere Azure-regio's. Een belangrijk voorbeeld hiervan is het instellen van SQL Always On met beschikbaarheidsgroepen verspreid over meerdere Azure-regio's.
* **Regionale toepassingen met meerdere lagen met isolatie- of beheergrenzen**

  * Binnen dezelfde regio kunt u vanwege isolatie- of beheervereisten toepassingen met meerdere lagen instellen met meerdere virtuele netwerken die met elkaar zijn verbonden.

VNet-naar-VNet-communicatie kan worden gecombineerd met configuraties voor meerdere locaties. Zoals u in het volgende diagram kunt zien, kunt u netwerktopologieën maken waarin cross-premises connectiviteit wordt gecombineerd met connectiviteit tussen virtuele netwerken:

![Over verbindingen](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Over verbindingen")

Dit artikel helpt u bij het verbinden van VNet's met behulp van het verbindingstype VNet-naar-VNet. Wanneer u deze stappen uitvoert als oefening, kunt u de voorbeeldinstellingen gebruiken. In het voorbeeld vallen de virtuele netwerken onder hetzelfde abonnement, maar behoren ze tot verschillende resourcegroepen. Als uw VNet's onder verschillende abonnementen vallen, kunt u de verbinding niet via de portal maken. Gebruik in plaats daarvan [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) of de [CLI](vpn-gateway-howto-vnet-vnet-cli.md). Zie voor meer informatie over verbindingen tussen VNets de [Veelgestelde vragen over VNet-naar-VNet](#faq) aan het einde van dit artikel.

### <a name="values"></a>Voorbeeldinstellingen

**Waarden voor TestVNet1:**

* VNet-naam: TestVNet1
* Adresruimte: 10.11.0.0/16
* Abonnement: selecteer het gewenste abonnement
* Resourcegroep: TestRG1
* Locatie: VS - oost
* Subnetnaam: FrontEnd
* Subnetadresbereik: 10.11.0.0/24
* Naam gatewaysubnet: GatewaySubnet (dit wordt in de portal automatisch ingevuld)
* Adresbereik gatewaysubnet: 10.11.255.0/27
* DNS-server: gebruik het IP-adres van de DNS-server
* Gatewaynaam van het virtuele netwerk: TestVNet1GW
* Gatewaytype: VPN
* VPN-type: op route gebaseerd
* KSU: selecteer de gateway-SKU die u wilt gebruiken
* Naam van openbare IP-adres: TestVNet1GWIP
* Verbindingsnaam: TestVNet1toTestVNet4
* Gedeelde sleutel: u kunt de gedeelde sleutel zelf maken. In dit voorbeeld gebruiken we abc123. Het belangrijkste is dat wanneer u de verbinding tussen de VNets maakt, de waarde moet overeenkomen.

**Waarden voor TestVNet4:**

* VNet-naam: TestVNet4
* Adresruimte: 10.41.0.0/16
* Abonnement: selecteer het gewenste abonnement
* Resourcegroep: TestRG4
* Locatie: VS - west
* Subnetnaam: FrontEnd
* Subnetadresbereik: 10.41.0.0/24
* Naam GatewaySubnet: GatewaySubnet (dit wordt in de portal automatisch ingevuld)
* Adresbereik GatewaySubnet: 10.41.255.0/27
* DNS-server: gebruik het IP-adres van de DNS-server
* Gatewaynaam van het virtuele netwerk: TestVNet4GW
* Gatewaytype: VPN
* VPN-type: op route gebaseerd
* KSU: selecteer de gateway-SKU die u wilt gebruiken
* Naam van openbare IP-adres: TestVNet4GWIP
* Verbindingsnaam: TestVNet4toTestVNet1
* Gedeelde sleutel: u kunt de gedeelde sleutel zelf maken. In dit voorbeeld gebruiken we abc123. Het belangrijkste is dat wanneer u de verbinding tussen de VNets maakt, de waarde moet overeenkomen.

## <a name="CreatVNet"></a>1. TestVNet1 maken en configureren
Als u al beschikt over een VNet, controleert u of de instellingen compatibel zijn met het ontwerp van de VPN-gateway. Let vooral op eventuele subnetten die met andere netwerken overlappen. Als u overlappende subnetten hebt, werkt de verbinding mogelijk niet goed. Als het VNet met de juiste instellingen is geconfigureerd, kunt u beginnen met de stappen in de sectie [Een DNS-server opgeven](#dns).

### <a name="to-create-a-virtual-network"></a>Een virtueel netwerk maken
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="subnets"></a>2. Extra adresruimte toevoegen en subnetten maken
Wanneer het VNet is gemaakt, kunt u er extra adresruimte en subnetten aan toevoegen.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. Een gatewaysubnet maken
Voordat u het virtuele netwerk verbindt met een gateway, moet u eerst het gatewaysubnet maken voor het virtuele netwerk waarmee u verbinding wilt maken. Indien mogelijk is het beste een gatewaysubnet met een CIDR-blok van /28 of /27 te gebruiken zodat er voldoende IP-adressen zijn om aan toekomstige aanvullende configuratievereisten te voldoen.

Als u deze configuratie bij wijze van oefening maakt, gebruikt u deze [voorbeeldinstellingen](#values) wanneer u het gatewaysubnet maakt.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Een gatewaysubnet maken
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4. Een DNS-server opgeven (optioneel)
DNS is niet vereist voor VNet-VNet-verbindingen. Als u echter naamomzetting wilt instellen voor resources die in uw virtuele netwerk worden geïmplementeerd, moet u een DNS-server opgeven. Met deze instelling kunt u de DNS-server opgeven die u wilt gebruiken voor de naamomzetting voor dit virtuele netwerk. Hierdoor wordt geen DNS-server aangemaakt.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. De gateway van een virtueel netwerk maken
In deze stap maakt u de virtuele netwerkgateway VNet. Het maken van een gateway duurt vaak 45 minuten of langer, afhankelijk van de geselecteerde gateway-SKU. Als u deze configuratie bij wijze van oefening maakt, kunt u de [voorbeeldinstellingen](#values) gebruiken.

### <a name="to-create-a-virtual-network-gateway"></a>De gateway van een virtueel netwerk maken
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="CreateTestVNet4"></a>6. TestVNet4 maken en configureren
Wanneer u TestVNet1 hebt geconfigureerd, maakt u TestVNet4 door de vorige stappen te herhalen en de waarden te vervangen door die van TestVNet4. U hoeft niet te wachten tot de gateway van het virtuele netwerk voor TestVNet1 is gemaakt voordat u TestVNet4 configureert. Als u uw eigen waarden gebruikt, zorgt u ervoor dat de adresruimten niet overlappen met een van de VNets waarmee u verbinding wilt maken.

## <a name="TestVNet1Connection"></a>7. De TestVNet1-gatewayverbinding configureren
Wanneer de virtuele netwerkgateways voor TestVNet1 en TestVNet4 zijn voltooid, kunt u de verbindingen voor uw virtuele netwerkgateway maken. In deze sectie maakt u een verbinding tussen VNet1 en VNet4. Deze stappen werken alleen voor VNets in hetzelfde abonnement. Als uw VNets onder verschillende abonnementen vallen, moet u PowerShell gebruiken om de verbinding te maken. Zie het artikel voor [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md). Als uw VNet's echter behoren tot verschillende resourcegroepen die onder hetzelfde abonnement vallen, kunt u deze verbinden met behulp van de portal.

1. In **Alle resources** gaat u naar de virtuele netwerkgateway voor uw VNet. Bijvoorbeeld **TestVNet1GW**. Klik op **TestVNet1GW** om de pagina voor de virtuele netwerkgateway te openen.

  ![Pagina Verbindingen](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/1to4connect2.png "Pagina Verbindingen")
2. Klik op **+Toevoegen** om de pagina **Verbinding toevoegen** te openen.

  ![Verbinding toevoegen](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add.png "Een verbinding toevoegen")
3. Typ in het naamveld op de pagina **Verbinding toevoegen** een naam voor de verbinding. Bijvoorbeeld **TestVNet1toTestVNet4**.
4. Bij **Verbindingstype** selecteert u **VNet-naar-VNet** in de vervolgkeuzelijst.
5. De veldwaarde **Eerste virtuele netwerkgateway** wordt automatisch ingevuld omdat u deze verbinding maakt vanuit de opgegeven virtuele netwerkgateway.
6. Het veld **Tweede virtuele netwerkgateway** is de virtuele netwerkgateway van de VNet waarvoor u een verbinding wilt maken. Klik op **Een andere virtuele netwerkgateway kiezen** om de pagina **Virtuele netwerkgateway kiezen** te openen.
7. Bekijk de virtuele netwerkgateways die op deze pagina worden vermeld. U ziet dat er alleen virtuele netwerkgateways worden vermeld die binnen uw abonnement vallen. Als u verbinding wilt maken met een virtuele netwerkgateway die geen deel uitmaakt van uw abonnement, gebruikt u het [PowerShell-artikel](vpn-gateway-vnet-vnet-rm-ps.md).
8. Klik op de virtuele netwerkgateway waarmee u verbinding wilt maken.
9. In het veld **Gedeelde sleutel** typt u een gedeelde sleutel voor de verbinding. U kunt deze sleutel ook zelf maken of genereren. In een verbinding tussen sites is de sleutel exact dezelfde voor het on-premises apparaat en uw virtuele netwerkgatewayverbinding. Het concept is hier vergelijkbaar, maar in plaats van een verbinding te maken met een VPN-apparaat, maakt u verbinding met een andere virtuele netwerkgateway.
10. Klik onderaan de pagina op **OK** om de wijzigingen op te slaan.

## <a name="TestVNet4Connection"></a>8. De TestVNet4-gatewayverbinding configureren
Maak vervolgens een verbinding tussen TestVNet4 en TestVNet1. Zoek in de portal de virtuele netwerkgateway die is gekoppeld aan TestVNet4. Volg de stappen uit de vorige sectie, maar maak nu een verbinding van TestVNet4 naar TestVNet1. Zorg ervoor dat u dezelfde gedeelde sleutel gebruikt.

## <a name="VerifyConnection"></a>9. Controleer uw verbindingen

Zoek de virtuele networkgateway in de portal. Klik op de pagina van de virtuele netwerkgateway op **Verbindingen** om de pagina Verbindingen weer te geven voor de virtuele netwerkgateway. Nadat de verbinding tot stand is gebracht, worden de statuswaarden weergegeven als **Geslaagd** en **Verbonden**. U kunt dubbelklikken op een verbinding om de pagina **Essentials** te openen en meer informatie te bekijken.

![Geslaagd](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Geslaagd")

Wanneer de gegevensstromen op gang komen, ziet u waarden voor ingaande en uitgaande gegevens.

![Essentials](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Essentials")

## <a name="to-add-additional-connections"></a>Extra verbindingen toevoegen

Als u extra verbindingen wilt toevoegen, gaat u naar de virtuele netwerkgateway waarmee u de verbinding wilt maken en klikt u vervolgens op **Verbindingen**. U kunt een andere VNet-naar-VNet-verbinding maken, of een site-naar-site-verbinding (IPsec) met een on-premises locatie. Stel **Verbindingstype** in op het gewenste type voor de verbinding die u wilt maken. Controleer voordat u extra verbindingen maakt of de adresruimte voor het virtuele netwerk niet overlapt met een van de adresruimten waarmee u verbinding wilt maken. Zie [Een site-naar-site-verbinding maken](vpn-gateway-howto-site-to-site-resource-manager-portal.md) voor stappen voor het maken van een site-naar-site-verbinding.

## <a name="faq"></a>Veelgestelde vragen over VNet-naar-VNet
Bekijk de Veelgestelde vragen voor meer informatie over VNet-naar-VNet-verbindingen.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

## <a name="next-steps"></a>Volgende stappen

Zie [Netwerkbeveiliging](../virtual-network/security-overview.md) voor informatie over het beperken van netwerkverkeer tot resources in een virtueel netwerk.

Zie [Routering van verkeer in virtuele netwerken](../virtual-network/virtual-networks-udr-overview.md) voor informatie over hoe Azure verkeer routeert tussen Azure-resources, on-premises resources en resources op internet.