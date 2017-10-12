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
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: cbbae06d4e5774d5fdf7bcce65dc7efddaa5f280
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-the-azure-portal"></a>Een VPN-gatewayverbinding tussen VNets configureren met behulp van Azure Portal

In dit artikel wordt beschreven hoe u een VPN-gatewayverbinding tussen virtuele netwerken maakt. De virtuele netwerken kunnen zich in dezelfde of verschillende regio's bevinden en tot dezelfde of verschillende abonnementen behoren. Wanneer u VNets uit verschillende abonnementen koppelt, hoeven de abonnementen niet aan dezelfde Active Directory-tenant gekoppeld te zijn. 

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

Het verbinden van een virtueel netwerk met een ander virtueel netwerk (VNet-naar-VNet) lijkt op het verbinden van een VNet met een on-premises locatie. Voor beide connectiviteitstypen wordt een VPN-gateway gebruikt om een beveiligde tunnel met IPsec/IKE te bieden. Als uw VNET's zich in dezelfde regio bevinden, kunt u daarmee verbinding maken met behulp van VNet-peering. Bij VNet-peering wordt geen VPN-gateway gebruikt. Zie het artikel [VNet-peering](../virtual-network/virtual-network-peering-overview.md) voor meer informatie.

VNet-naar-VNet-communicatie kan worden gecombineerd met configuraties voor meerdere locaties. Zoals u in het volgende diagram kunt zien, kunt u netwerktopologieën maken waarin cross-premises connectiviteit wordt gecombineerd met connectiviteit tussen virtuele netwerken:

![Over verbindingen](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Over verbindingen")

### <a name="why-connect-virtual-networks"></a>Waarom virtuele netwerken koppelen?

U wilt virtuele netwerken wellicht koppelen om de volgende redenen:

* **Geografische redundantie en aanwezigheid tussen regio's**
  
  * U kunt uw eigen geo-replicatie of synchronisatie met beveiligde connectiviteit instellen zonder gebruik te maken van internetgerichte eindpunten.
  * Met Azure Traffic Manager en Load Balancer kunt u workloads met maximale beschikbaarheid instellen met behulp van geografische redundantie over meerdere Azure-regio's. Een belangrijk voorbeeld hiervan is het instellen van SQL Always On met beschikbaarheidsgroepen verspreid over meerdere Azure-regio's.
* **Regionale toepassingen met meerdere lagen met isolatie- of beheergrenzen**
  
  * Binnen dezelfde regio kunt u vanwege isolatie- of beheervereisten toepassingen met meerdere lagen instellen met meerdere virtuele netwerken die met elkaar zijn verbonden.

Zie voor meer informatie over verbindingen tussen VNets de [Veelgestelde vragen over VNet-naar-VNet](#faq) aan het einde van dit artikel. Als uw VNet's echter onder verschillende abonnementen vallen, kunt u de verbinding niet via de portal maken. Gebruik in plaats daarvan [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) of de [CLI](vpn-gateway-howto-vnet-vnet-cli.md).

### <a name="values"></a>Voorbeeldinstellingen

Wanneer u deze stappen uitvoert als oefening, kunt u de voorbeeldinstellingen gebruiken. Als voorbeeld gebruiken we meerdere adresruimten voor elke VNet. Voor VNet-naar-VNet-configuraties zijn meerdere adresruimten echter niet vereist.

**Waarden voor TestVNet1:**

* VNet-naam: TestVNet1
* Adresruimte: 10.11.0.0/16
  * Subnetnaam: FrontEnd
  * Subnetadresbereik: 10.11.0.0/24
* Resourcegroep: TestRG1
* Locatie: VS - oost
* Adresruimte: 10.12.0.0/16
  * Subnetnaam: BackEnd
  * Subnetadresbereik: 10.12.0.0/24
* Naam gatewaysubnet: GatewaySubnet (dit wordt in de portal automatisch ingevuld)
  * Adresbereik gatewaysubnet: 10.11.255.0/27
* DNS-server: gebruik het IP-adres van de DNS-server
* Gatewaynaam van het virtuele netwerk: TestVNet1GW
* Gatewaytype: VPN
* VPN-type: op route gebaseerd
* KSU: selecteer de gateway-SKU die u wilt gebruiken
* Naam van openbare IP-adres: TestVNet1GWIP
* Verbindingswaarden:
  * Naam: TestVNet1toTestVNet4
  * Gedeelde sleutel: u kunt de gedeelde sleutel zelf maken. In dit voorbeeld gebruiken we abc123. Het belangrijkste is dat wanneer u de verbinding tussen de VNets maakt, de waarde moet overeenkomen.

**Waarden voor TestVNet4:**

* VNet-naam: TestVNet4
* Adresruimte: 10.41.0.0/16
  * Subnetnaam: FrontEnd
  * Subnetadresbereik: 10.41.0.0/24
* Resourcegroep: TestRG1
* Locatie: VS - west
* Adresruimte: 10.42.0.0/16
  * Subnetnaam: BackEnd
  * Subnetadresbereik: 10.42.0.0/24
* Naam GatewaySubnet: GatewaySubnet (dit wordt in de portal automatisch ingevuld)
  * Adresbereik GatewaySubnet: 10.41.255.0/27
* DNS-server: gebruik het IP-adres van de DNS-server
* Gatewaynaam van het virtuele netwerk: TestVNet4GW
* Gatewaytype: VPN
* VPN-type: op route gebaseerd
* KSU: selecteer de gateway-SKU die u wilt gebruiken
* Naam van openbare IP-adres: TestVNet4GWIP
* Verbindingswaarden:
  * Naam: TestVNet4toTestVNet1
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

## <a name="TestVNet1Connection"></a>7. De TestVNet1-verbinding configureren
Wanneer de virtuele netwerkgateways voor TestVNet1 en TestVNet4 zijn voltooid, kunt u de verbindingen voor uw virtuele netwerkgateway maken. In deze sectie maakt u een verbinding tussen VNet1 en VNet4. Deze stappen werken alleen voor VNets in hetzelfde abonnement. Als uw VNets onder verschillende abonnementen vallen, moet u PowerShell gebruiken om de verbinding te maken. Zie het artikel voor [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md).

1. In **Alle resources** gaat u naar de virtuele netwerkgateway voor uw VNet. Bijvoorbeeld **TestVNet1GW**. Klik op **TestVNet1GW** om de blade voor de virtuele netwerkgateway te openen.
   
    ![Blade Verbindingen](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Blade Verbindingen")
2. Klik op **+Toevoegen** om de blade **Verbinding toevoegen** te openen.
3. Typ in het naamveld op de blade **Verbinding toevoegen** een naam voor de verbinding. Bijvoorbeeld **TestVNet1toTestVNet4**.
   
    ![Verbindingsnaam](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Verbindingsnaam")
4. Voor **Verbindingstype** selecteert u **VNet-naar-VNet** in de vervolgkeuzelijst.
5. De veldwaarde **Eerste virtuele netwerkgateway** wordt automatisch ingevuld omdat u deze verbinding maakt vanuit de opgegeven virtuele netwerkgateway.
6. Het veld **Tweede virtuele netwerkgateway** is de virtuele netwerkgateway van de VNet waarvoor u een verbinding wilt maken. Klik op **Een andere virtuele netwerkgateway kiezen** om de blade **Virtuele netwerkgateway kiezen** te openen.
   
    ![Verbinding toevoegen](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Een verbinding toevoegen")
7. Bekijk de virtuele netwerkgateways die worden vermeld op deze blade. U ziet dat er alleen virtuele netwerkgateways worden vermeld die binnen uw abonnement vallen. Als u verbinding wilt maken met een virtuele netwerkgateway die geen deel uitmaakt van uw abonnement, gebruikt u het [PowerShell-artikel](vpn-gateway-vnet-vnet-rm-ps.md). 
8. Klik op de virtuele netwerkgateway waarmee u verbinding wilt maken.
9. In het veld **Gedeelde sleutel** typt u een gedeelde sleutel voor de verbinding. U kunt deze sleutel ook zelf maken of genereren. In een verbinding tussen sites is de sleutel exact dezelfde voor het on-premises apparaat en uw virtuele netwerkgatewayverbinding. Het concept is hier vergelijkbaar, maar in plaats van een verbinding te maken met een VPN-apparaat, maakt u verbinding met een andere virtuele netwerkgateway.
   
    ![Gedeelde sleutel](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Gedeelde sleutel")
10. Klik op **OK** onder aan de blade om de wijzigingen op te slaan.

## <a name="TestVNet4Connection"></a>8. De TestVNet4-verbinding configureren
Maak vervolgens een verbinding tussen TestVNet4 en TestVNet1. Gebruik dezelfde methode die u hebt gebruikt voor het maken van de verbinding tussen TestVNet1 en TestVNet4. Zorg ervoor dat u dezelfde gedeelde sleutel gebruikt.

## <a name="VerifyConnection"></a>9. De verbinding controleren
Controleer de VPN-verbinding. Doe voor elke virtuele netwerkgateway het volgende:

1. Zoek de blade voor de gateway van het virtuele netwerk. Bijvoorbeeld **TestVNet4GW**. 
2. Klik op de blade van het virtuele netwerkgateway op **Verbindingen** om de blade Verbindingen weer te geven voor de virtuele netwerkgateway.

Geef de verbindingen weer en controleer de status. Nadat de verbinding is gemaakt, worden de statuswaarden weergegeven als **Geslaagd** en **Verbonden**.

![Geslaagd](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Geslaagd")

U kunt afzonderlijk op elke verbinding dubbelklikken voor meer informatie over de verbinding.

![Essentials](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Essentials")

## <a name="faq"></a>Veelgestelde vragen over VNet-naar-VNet
Bekijk de Veelgestelde vragen voor meer informatie over VNet-naar-VNet-verbindingen.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Raadpleeg de [documentatie over Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie.
