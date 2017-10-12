---
title: 'Een virtueel Azure-netwerk verbinden met een ander VNet: Powershell | Microsoft Docs'
description: Dit artikel helpt u bij het met elkaar verbinden van virtuele netwerken met behulp van Azure Resource Manager en PowerShell.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0683c664-9c03-40a4-b198-a6529bf1ce8b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 537e80937289d6b46283843c2ee0725e7e08fefc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-powershell"></a>Een VPN-gatewayverbinding tussen VNets configureren met behulp van PowerShell

In dit artikel wordt beschreven hoe u een VPN-gatewayverbinding tussen virtuele netwerken maakt. De virtuele netwerken kunnen zich in dezelfde of verschillende regio's bevinden en tot dezelfde of verschillende abonnementen behoren. Wanneer u VNets uit verschillende abonnementen koppelt, hoeven de abonnementen niet aan dezelfde Active Directory-tenant gekoppeld te zijn. 

De stappen in dit artikel zijn van toepassing op het Resource Manager-implementatiemodel. Er wordt gebruikgemaakt van PowerShell. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure-CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure Portal (klassiek)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Verbinding maken tussen verschillende implementatiemodellen - Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Verbinding maken tussen verschillende implementatiemodellen - PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

Het verbinden van een virtueel netwerk met een ander virtueel netwerk (VNet-naar-VNet) lijkt op het verbinden van een VNet met een on-premises locatie. Voor beide connectiviteitstypen wordt een VPN-gateway gebruikt om een beveiligde tunnel met IPsec/IKE te bieden. Als uw VNET's zich in dezelfde regio bevinden, kunt u daarmee verbinding maken met behulp van VNet-peering. Bij VNet-peering wordt geen VPN-gateway gebruikt. Zie het artikel [VNet-peering](../virtual-network/virtual-network-peering-overview.md) voor meer informatie.

VNet-naar-VNet-communicatie kan worden gecombineerd met configuraties voor meerdere locaties. Zoals u in het volgende diagram kunt zien, kunt u netwerktopologieën maken waarin cross-premises connectiviteit wordt gecombineerd met connectiviteit tussen virtuele netwerken:

![Over verbindingen](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)

### <a name="why-connect-virtual-networks"></a>Waarom virtuele netwerken koppelen?

U wilt virtuele netwerken wellicht koppelen om de volgende redenen:

* **Geografische redundantie en aanwezigheid tussen regio's**

  * U kunt uw eigen geo-replicatie of synchronisatie met beveiligde connectiviteit instellen zonder gebruik te maken van internetgerichte eindpunten.
  * Met Azure Traffic Manager en Load Balancer kunt u workloads met maximale beschikbaarheid instellen met behulp van geografische redundantie over meerdere Azure-regio's. Een belangrijk voorbeeld hiervan is het instellen van SQL Always On met beschikbaarheidsgroepen verspreid over meerdere Azure-regio's.
* **Regionale toepassingen met meerdere lagen met isolatie- of beheergrenzen**

  * Binnen dezelfde regio kunt u vanwege isolatie- of beheervereisten toepassingen met meerdere lagen instellen met meerdere virtuele netwerken die met elkaar zijn verbonden.

Zie voor meer informatie over verbindingen tussen VNets de [Veelgestelde vragen over VNet-naar-VNet](#faq) aan het einde van dit artikel.

## <a name="which-set-of-steps-should-i-use"></a>Welke stappen moet ik gebruiken?

In dit artikel ziet u twee verschillende reeksen stappen. Een reeks stappen voor [VNets die zich in hetzelfde abonnement bevinden](#samesub) en een andere voor [VNets die zich in verschillende abonnementen bevinden](#difsub). Het belangrijkste verschil tussen de reeksen is of u alle resources van het virtuele netwerk en de gateway kunt maken en configureren binnen dezelfde PowerShell-sessie.

In de stappen in dit artikel worden variabelen gebruikt die aan het begin van elke sectie zijn gedeclareerd. Als u al met bestaande VNets werkt, wijzigt u de variabelen zo dat ze overeenkomen met de instellingen in uw eigen omgeving. Zie [Naamomzetting](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) als u naamomzetting voor uw virtuele netwerken wilt.

## <a name="samesub"></a>VNets verbinden die tot hetzelfde abonnement behoren

![v2v-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Voordat u begint

Installeer eerst de meest recente versie van de Azure Resource Manager PowerShell-cmdlets (versie 4.0 of hoger). Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="Step1"></a>Stap 1: De IP-adresbereiken plannen

In de volgende stappen maakt u twee virtuele netwerken en hun bijbehorende gatewaysubnetten en configuraties. Vervolgens maakt u een VPN-verbinding tussen de twee VNets. Het is belangrijk dat u de IP-adresbereiken voor uw netwerkconfiguratie plant. De VNet-bereiken of de bereiken van het lokale netwerk mogen elkaar niet overlappen. In deze voorbeelden behandelen we geen DNS-server. Zie [Naamomzetting](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) als u naamomzetting voor uw virtuele netwerken wilt.

In de voorbeelden worden de volgende waarden gebruikt:

**Waarden voor TestVNet1:**

* VNet-naam: TestVNet1
* Resourcegroep: TestRG1
* Locatie: VS - oost
* TestVNet1: 10.11.0.0/16 en 10.12.0.0/16
* FrontEnd: 10.11.0.0/24
* BackEnd: 10.12.0.0/24
* GatewaySubnet: 10.12.255.0/27
* GatewayName: VNet1GW
* Openbare IP: VNet1GWIP
* VPNType: op route gebaseerd
* Connection(1to4): VNet1toVNet4
* Connection(1to5): VNet1toVNet5
* ConnectionType: VNet2VNet

**Waarden voor TestVNet4:**

* VNet-naam: TestVNet4
* TestVNet2: 10.41.0.0/16 & 10.42.0.0/16
* FrontEnd: 10.41.0.0/24
* BackEnd: 10.42.0.0/24
* GatewaySubnet: 10.42.255.0/27
* Resourcegroep: TestRG4
* Locatie: VS - west
* GatewayName: VNet4GW
* Openbare IP: VNet4GWIP
* VPNType: op route gebaseerd
* Verbinding: VNet4toVNet1
* ConnectionType: VNet2VNet


### <a name="Step2"></a>Stap 2: TestVNet1 maken en configureren

1. Declareer uw variabelen. In dit voorbeeld worden de variabelen gedeclareerd met de waarden voor deze oefening. In de meeste gevallen moet u de waarden vervangen door uw eigen waarden. U kunt deze variabelen echter gebruiken als u de stappen alleen wilt doorlopen om vertrouwd te raken met dit type configuratie. Wijzig de variabelen als dat nodig is en kopieer en plak ze vervolgens in uw PowerShell-console.

  ```powershell
  $Sub1 = "Replace_With_Your_Subcription_Name"
  $RG1 = "TestRG1"
  $Location1 = "East US"
  $VNetName1 = "TestVNet1"
  $FESubName1 = "FrontEnd"
  $BESubName1 = "Backend"
  $GWSubName1 = "GatewaySubnet"
  $VNetPrefix11 = "10.11.0.0/16"
  $VNetPrefix12 = "10.12.0.0/16"
  $FESubPrefix1 = "10.11.0.0/24"
  $BESubPrefix1 = "10.12.0.0/24"
  $GWSubPrefix1 = "10.12.255.0/27"
  $GWName1 = "VNet1GW"
  $GWIPName1 = "VNet1GWIP"
  $GWIPconfName1 = "gwipconf1"
  $Connection14 = "VNet1toVNet4"
  $Connection15 = "VNet1toVNet5"
  ```

2. Maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

  ```powershell
  Login-AzureRmAccount
  ```

  Controleer de abonnementen voor het account.

  ```powershell
  Get-AzureRmSubscription
  ```

  Geef het abonnement op dat u wilt gebruiken.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub1
  ```
3. Maak een nieuwe resourcegroep.

  ```powershell
  New-AzureRmResourceGroup -Name $RG1 -Location $Location1
  ```
4. Maak de subnetconfiguraties voor TestVNet1. In dit voorbeeld wordt een virtueel netwerk gemaakt met de naam TestVNet1. Er worden ook drie subnetten gemaakt, GatewaySubnet, FrontEnd en BackEnd. Wanneer u de waarden vervangt, is het belangrijk dat u de juiste namen voor de gatewaysubnets gebruikt, in het bijzonder GatewaySubnet. Als u een andere naam kiest, mislukt het maken van de gateway.

  In het volgende voorbeeld worden de variabelen gebruikt die u eerder hebt ingesteld. In dit voorbeeld maakt het gatewaysubnet gebruik van een /27. Het is mogelijk om een klein gatewaysubnet van /29 te maken, maar we raden u aan een groter subnet met meer adressen te maken door ten minste /28 of /27 te selecteren. Hierdoor hebt u genoeg adressen voor mogelijke aanvullende toekomstige configuraties.

  ```powershell
  $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
  $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
  $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1
  ```
5. Maak TestVNet1.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
  ```
6. Vraag om een openbaar IP-adres toe te wijzen aan de gateway die u voor uw VNet gaat maken. De toewijzingsmethode is Dynamisch. U kunt het IP-adres dat u wilt gebruiken niet zelf opgeven. Het wordt dynamisch toegewezen aan uw gateway. 

  ```powershell
  $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
  ```
7. Maak de gatewayconfiguratie. De gatewayconfiguratie bepaalt welk subnet en openbaar IP-adres moeten worden gebruikt. Gebruik het voorbeeld om de gatewayconfiguratie te maken.

  ```powershell
  $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
  $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```
8. Maak de gateway voor TestVNet1. In deze stap maakt u de virtuele netwerkgateway TestVNet1. VNet-naar-VNet-configuraties vereisen een op route gebaseerd VpnType. Het maken van een gateway duurt vaak 45 minuten of langer, afhankelijk van de geselecteerde gateway-SKU.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-3---create-and-configure-testvnet4"></a>Stap 3: TestVNet4 maken en configureren

Wanneer u TestVNet1 hebt geconfigureerd, maakt u TestVNet4. Volg de stappen hieronder en vervang de waarden door uw eigen waarden wanneer dat nodig is. Deze stap kan worden uitgevoerd binnen dezelfde PowerShell-sessie omdat deze tot hetzelfde abonnement behoort.

1. Declareer uw variabelen. Zorg ervoor dat u de waarden vervangt door de waarden die u voor uw configuratie wilt gebruiken.

  ```powershell
  $RG4 = "TestRG4"
  $Location4 = "West US"
  $VnetName4 = "TestVNet4"
  $FESubName4 = "FrontEnd"
  $BESubName4 = "Backend"
  $GWSubName4 = "GatewaySubnet"
  $VnetPrefix41 = "10.41.0.0/16"
  $VnetPrefix42 = "10.42.0.0/16"
  $FESubPrefix4 = "10.41.0.0/24"
  $BESubPrefix4 = "10.42.0.0/24"
  $GWSubPrefix4 = "10.42.255.0/27"
  $GWName4 = "VNet4GW"
  $GWIPName4 = "VNet4GWIP"
  $GWIPconfName4 = "gwipconf4"
  $Connection41 = "VNet4toVNet1"
  ```
2. Maak een nieuwe resourcegroep.

  ```powershell
  New-AzureRmResourceGroup -Name $RG4 -Location $Location4
  ```
3. Maak de subnetconfiguraties voor TestVNet4.

  ```powershell
  $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
  $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
  $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4
  ```
4. Maak TestVNet4.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4
  ```
5. Vraag een openbaar IP-adres aan.

  ```powershell
  $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AllocationMethod Dynamic
  ```
6. Maak de gatewayconfiguratie.

  ```powershell
  $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
  $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
  $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4
  ```
7. Maak de TestVNet4-gateway. Het maken van een gateway duurt vaak 45 minuten of langer, afhankelijk van de geselecteerde gateway-SKU.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
  -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-4---create-the-connections"></a>Stap 4: de verbindingen maken

1. Verkrijg beide gateways van het virtuele netwerk. Als beide gateways tot hetzelfde abonnement behoren, zoals in het voorbeeld, kunt u deze stap in dezelfde PowerShell-sessie voltooien.

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4
  ```
2. Maak de verbinding tussen TestVNet1 en TestVNet4. In deze stap maakt u de verbinding van TestVNet1 naar TestVNet4. Hier ziet u een gedeelde sleutel waarnaar wordt verwezen in de voorbeelden. U kunt uw eigen waarden voor de gedeelde sleutel gebruiken. Het belangrijkste is dat de gedeelde sleutel voor beide verbindingen moet overeenkomen. Het kan even duren voordat de verbinding is gemaakt.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
3. Maak de verbinding tussen TestVNet4 en TestVNet1. Deze stap is vergelijkbaar met die hierboven, alleen maakt u de verbinding nu vanuit TestVNet4 naar TestVNet1. Zorg dat de gedeelde sleutels overeenkomen. De verbinding wordt na enkele minuten tot stand gebracht.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
  -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. Controleer de verbinding. Raadpleeg de sectie [De verbinding controleren](#verify).

## <a name="difsub"></a>VNets verbinden die tot verschillende abonnement behoren

![v2v-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

In dit scenario worden TestVNet1 en TestVNet5 met elkaar verbonden. TestVNet1 en TestVNet5 bevinden zich in een verschillend abonnement. De abonnementen hoeven niet aan dezelfde Active Directory-tenant gekoppeld te zijn. Het verschil tussen deze stappen en de eerste set is dat een deel van de configuratiestappen moet worden uitgevoerd in een aparte PowerShell-sessie in de context van het tweede abonnement. Dit geldt met name wanneer de twee abonnementen tot verschillende organisaties behoren.

### <a name="step-5---create-and-configure-testvnet1"></a>Stap 5: TestVNet1 maken en configureren

U moet [Stap 1](#Step1) en [Stap 2](#Step2) van de vorige sessie uitvoeren om TestVNet1 en de VPN-gateway voor TestVNet1 te maken en te configureren. Voor deze configuratie is het niet vereist TestVNet4 uit de vorige sectie te maken. Als u deze wel maakt, levert dit geen problemen op met de volgende stappen. Nadat u stap 1 en stap 2 hebt voltooid, gaat u verder met stap 6 om TestVNet5 te maken. 

### <a name="step-6---verify-the-ip-address-ranges"></a>Stap 6: de IP-adresbereiken controleren

Het is belangrijk dat u controleert of de IP-adresruimte van het nieuwe virtuele netwerk, TestVNet5, niet overlapt met een van de VNet-bereiken of de gatewaybereiken van het lokale netwerk. In dit voorbeeld kunnen de virtuele netwerken tot verschillende organisaties behoren. Voor deze oefening kunt u de volgende waarden voor TestVNet5 gebruiken:

**Waarden voor TestVNet5:**

* VNet-naam: TestVNet5
* Resourcegroep: TestRG5
* Locatie: Japan - oost
* TestVNet5: 10.51.0.0/16 & 10.52.0.0/16
* FrontEnd: 10.51.0.0/24
* BackEnd: 10.52.0.0/24
* GatewaySubnet: 10.52.255.0.0/27
* GatewayName: VNet5GW
* Openbare IP: VNet5GWIP
* VPNType: op route gebaseerd
* Verbinding: VNet5toVNet1
* ConnectionType: VNet2VNet

### <a name="step-7---create-and-configure-testvnet5"></a>Stap 7: TestVNet5 maken en configureren

Deze stap moet worden uitgevoerd in de context van het nieuwe abonnement. Dit deel kan worden uitgevoerd door de beheerder in een andere organisatie die eigenaar is van het abonnement.

1. Declareer uw variabelen. Zorg ervoor dat u de waarden vervangt door de waarden die u voor uw configuratie wilt gebruiken.

  ```powershell
  $Sub5 = "Replace_With_the_New_Subcription_Name"
  $RG5 = "TestRG5"
  $Location5 = "Japan East"
  $VnetName5 = "TestVNet5"
  $FESubName5 = "FrontEnd"
  $BESubName5 = "Backend"
  $GWSubName5 = "GatewaySubnet"
  $VnetPrefix51 = "10.51.0.0/16"
  $VnetPrefix52 = "10.52.0.0/16"
  $FESubPrefix5 = "10.51.0.0/24"
  $BESubPrefix5 = "10.52.0.0/24"
  $GWSubPrefix5 = "10.52.255.0/27"
  $GWName5 = "VNet5GW"
  $GWIPName5 = "VNet5GWIP"
  $GWIPconfName5 = "gwipconf5"
  $Connection51 = "VNet5toVNet1"
  ```
2. Maak verbinding met abonnement 5. Open de PowerShell-console en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

  ```powershell
  Login-AzureRmAccount
  ```

  Controleer de abonnementen voor het account.

  ```powershell
  Get-AzureRmSubscription
  ```

  Geef het abonnement op dat u wilt gebruiken.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub5
  ```
3. Maak een nieuwe resourcegroep.

  ```powershell
  New-AzureRmResourceGroup -Name $RG5 -Location $Location5
  ```
4. Maak de subnetconfiguraties voor TestVNet5.

  ```powershell
  $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
  $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
  $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5
  ```
5. Maak TestVNet5.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
  -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5
  ```
6. Vraag een openbaar IP-adres aan.

  ```powershell
  $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
  -Location $Location5 -AllocationMethod Dynamic
  ```
7. Maak de gatewayconfiguratie.

  ```powershell
  $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
  $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
  $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5
  ```
8. Maak de TestVNet5-gateway.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
  -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-8---create-the-connections"></a>Stap 8: de verbindingen maken

Omdat de gateways in dit voorbeeld tot verschillende abonnementen behoren, is deze stap opgesplitst in twee PowerShell-sessies, aangeduid als [Abonnement 1] en [Abonnement 5].

1. **[Abonnement 1]** Verkrijg de gateway van het virtuele netwerk voor abonnement 1. Meld u aan bij en maak verbinding met abonnement 1 voordat u het volgende voorbeeld uitvoert:

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  ```

  Kopieer de uitvoer van de volgende elementen en stuur deze via e-mail of een andere manier naar de beheerder van abonnement 5.

  ```powershell
  $vnet1gw.Name
  $vnet1gw.Id
  ```

  De waarden van deze twee elementen lijken op die in het volgende voorbeeld:

  ```
  PS D:\> $vnet1gw.Name
  VNet1GW
  PS D:\> $vnet1gw.Id
  /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```
2. **[Abonnement 5]** Verkrijg de gateway van het virtuele netwerk voor abonnement 5. Meld u aan bij en maak verbinding met abonnement 5 voordat u het volgende voorbeeld uitvoert:

  ```powershell
  $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5
  ```

  Kopieer de uitvoer van de volgende elementen en stuur deze via e-mail of een andere manier naar de beheerder van Abonnement 1.

  ```powershell
  $vnet5gw.Name
  $vnet5gw.Id
  ```

  De waarden van deze twee elementen lijken op die in het volgende voorbeeld:

  ```
  PS C:\> $vnet5gw.Name
  VNet5GW
  PS C:\> $vnet5gw.Id
  /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```
3. **[Abonnement 1]** Maak de verbinding tussen TestVNet1 en TestVNet5. In deze stap maakt u de verbinding van TestVNet1 naar TestVNet5. Het verschil is hier dat $vnet5gw niet rechtstreeks kan worden verkregen omdat het zich in een ander abonnement bevindt. U moet een nieuw PowerShell-object maken met de waarden die in de bovenstaande stappen zijn gecommuniceerd vanuit Abonnement 1. Gebruik onderstaand voorbeeld. Vervang de naam, de id en de gedeelde sleutel door uw eigen waarden. Het belangrijkste is dat de gedeelde sleutel voor beide verbindingen moet overeenkomen. Het kan even duren voordat de verbinding is gemaakt.

  Maak verbinding met abonnement 1 voordat u het volgende voorbeeld uitvoert:

  ```powershell
  $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet5gw.Name = "VNet5GW"
  $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
  $Connection15 = "VNet1toVNet5"
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. **[Abonnement 5]** Maak de verbinding tussen TestVNet5 en TestVNet1. Deze stap is vergelijkbaar met die hierboven, alleen maakt u de verbinding nu vanuit TestVNet5 naar TestVNet1. Hier moet op dezelfde manier een PowerShell-object worden gemaakt op basis van de waarden die zijn verkregen van Abonnement 1. Zorg in deze stap dat de gedeelde sleutels overeenkomen.

  Maak verbinding met abonnement 5 voordat u het volgende voorbeeld uitvoert:

  ```powershell
  $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet1gw.Name = "VNet1GW"
  $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```

## <a name="verify"></a>Een verbinding controleren

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="faq"></a>Veelgestelde vragen over VNet-naar-VNet

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

## <a name="next-steps"></a>Volgende stappen

* Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Raadpleeg de [documentatie over Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie.
* Voor meer informatie over BGP raadpleegt u [BGP Overview](vpn-gateway-bgp-overview.md) (BGP-overzicht) en [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (BGP configureren).