---
title: Connectiviteit instellen vanaf het virtuele netwerk met SAP HANA op Azure (grote instanties) | Microsoft Docs
description: De verbinding vanaf het virtuele netwerk is ingesteld voor het gebruik van SAP HANA op Azure (grote instanties).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ebe303b24c497fe8ac52ac90a236a23c279ea2e9
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709681"
---
# <a name="connect-a-virtual-network-to-hana-large-instances"></a>Een virtueel netwerk verbinden met HANA grote instanties

Nadat u een Azure-netwerk hebt gemaakt, kunt u dat netwerk verbinden met SAP HANA op Azure grote instanties. Maak een Azure ExpressRoute-gateway op het virtuele netwerk. Deze gateway kunt u het virtueel netwerk koppelen aan het ExpressRoute-circuit dat is verbonden met de tenant van de klant op het stempel HANA grote instantie.

> [!NOTE] 
> Deze stap kan tot 30 minuten duren. De nieuwe gateway is gemaakt in het toegewezen Azure-abonnement en klikt u vervolgens verbinding met de opgegeven Azure-netwerk.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

Als er al een gateway bestaat, controleert u of het een ExpressRoute-gateway of niet is. Als het is niet een ExpressRoute-gateway, de gateway verwijderen en opnieuw maken als een ExpressRoute-gateway. Als een ExpressRoute-gateway al tot stand is gebracht, raadpleegt u de volgende sectie van dit artikel "Koppeling virtuele netwerken." 

- Een gebruiken de [Azure-portal](https://portal.azure.com/) of PowerShell voor het maken van een ExpressRoute-VPN-gateway verbonden met uw virtuele netwerk.
  - Als u de Azure-portal, toevoegen van een nieuwe **virtuele netwerkgateway**, en selecteer vervolgens **ExpressRoute** als het Gatewaytype.
  - Als u PowerShell gebruikt, eerst downloaden en gebruiken van de meest recente [Azure PowerShell SDK](https://azure.microsoft.com/downloads/). 
 
De volgende opdrachten maken een ExpressRoute-gateway. De tekst wordt voorafgegaan door een _$_ zijn van de gebruiker gedefinieerde variabelen die moeten worden bijgewerkt met uw specifieke gegevens.

```powershell
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA large instances are: HighPerformance or UltraPerformance

# These Commands create the Public IP and ExpressRoute Gateway
$vnet = Get-AzVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

In dit voorbeeld is de gateway-HighPerformance SKU gebruikt. Uw opties zijn HighPerformance of UltraPerformance als de enige gateway-SKU's die worden ondersteund voor SAP HANA op Azure (grote instanties).

> [!IMPORTANT]
> Voor HANA grote instanties van de SKU van het Type II-klasse, moet u de Gateway-SKU UltraPerformance.

## <a name="link-virtual-networks"></a>Virtuele netwerken koppelen

Het Azure-netwerk heeft nu een ExpressRoute-gateway. Gebruik de autorisatie-informatie die is geleverd door Microsoft om de ExpressRoute-gateway aan de SAP HANA grote instanties ExpressRoute-circuit. U kunt verbinding maken met behulp van de Azure portal of PowerShell. De PowerShell-instructies zijn als volgt. 

Voer de volgende opdrachten voor elk ExpressRoute-gateway met behulp van een andere AuthGUID voor elke verbinding. De eerste twee items in het volgende script afkomstig zijn van de informatie die wordt geleverd door Microsoft. Er is ook de AuthGUID specifiek voor elke virtueel netwerk en de gateway. Als u toevoegen van een andere Azure-netwerk wilt, moet u een andere AuthID ophalen voor uw ExpressRoute-circuit dat is verbonden HANA grote instanties in Azure van Microsoft. 

```powershell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID -ExpressRouteGatewayBypass
```

> [!NOTE]
> De laatste parameter in de opdracht New-AzVirtualNetworkGatewayConnection **ExpressRouteGatewayBypass** is een nieuwe parameter waarmee ExpressRoute snelle pad. Een functionaliteit die de netwerklatentie tussen uw HANA grote instantie eenheden en de Azure-VM's vermindert. De functionaliteit is toegevoegd in mei 2019. Raadpleeg voor meer informatie het artikel [netwerkarchitectuur van SAP HANA (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture). Zorg ervoor dat u de meest recente versie van PowerShell-cmdlets worden uitgevoerd voordat u de opdrachten uitvoert.

Als u wilt verbinding maken met de gateway aan meer dan één ExpressRoute-circuit dat is gekoppeld aan uw abonnement, moet u mogelijk meer dan één keer uitvoeren van deze stap. Bijvoorbeeld, gaat u waarschijnlijk de dezelfde virtuele netwerkgateway naar het ExpressRoute-circuit dat is het virtuele netwerk met uw on-premises netwerk verbonden verbinding maken.

## <a name="applying-expressroute-fast-path-to-existing-hana-large-instance-expressroute-circuits"></a>ExpressRoute snel pad toe te passen op bestaande HANA grote instantie ExpressRoute-circuits
Tot nu toe in de documentatie wordt uitgelegd hoe u verbinding maken met een nieuwe ExpressRoute-circuit dat is gemaakt met een HANA grote instantie-implementatie naar een Azure ExpressRoute-gateway van een van uw virtuele netwerken van Azure. Maar al hun ExpressRoute-circuits setup al veel klanten en hebben hun virtuele netwerken verbonden met HANA grote instanties al. Als het nieuwe ExpressRoute snel pad is een vermindering van de netwerklatentie, verdient het aanbeveling de wijziging voor het gebruik van deze functionaliteit toe te passen. De opdrachten om een nieuw ExpreesRoute circuit verbinding te maken en te wijzigen van een bestaande ExpressRoute-Circuit zijn hetzelfde. Als gevolg hiervan moet u uitvoeren van deze reeks PowerShell-opdrachten voor het wijzigen van een bestaand circuit te gebruiken 

```powershell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID -ExpressRouteGatewayBypass
```

Het is belangrijk dat u de laatste parameter toevoegen, zoals hierboven weergegeven waarmee de functionaliteit voor ExpressRoute snel pad


## <a name="expressroute-global-reach"></a>ExpressRoute wereldwijd bereik
Als u inschakelen globaal bereik voor een of beide van de twee scenario's wilt:

 - HANA-Systeemreplicatie zonder extra proxy's of firewalls
 - Kopiëren back-ups tussen HANA grote instantie eenheden in twee verschillende regio's om uit te voeren system kopieën of systeem wordt vernieuwd

u moet overwegen die:

- U moet opgeven van een ruimte-adresbereik van een/29-adresruimte. Dat adresbereik mogen elkaar niet overlappen met een van de andere adresruimtebereiken die u gebruikt de tot nu toe HANA grote instanties verbinden met Azure en mogen elkaar niet overlappen met een van de IP-adresbereiken u gebruikt een andere locatie in Azure of on-premises.
- Er is een beperking op de ASN's (Autonomous System Number) die kunnen worden gebruikt om uw on-premises routes naar HANA grote instanties te adverteren. Uw on-premises moeten niet adverteert routes met persoonlijke ASN's in het bereik van 65000 – 65020 of 65515. 
- Voor het scenario van een on-premises directe toegang verbinding met HANA grote instanties, moet u voor het berekenen van een bedrag voor het circuit waarmee u verbinding met Azure maakt. Controleer voor prijzen, de prijzen voor [globale bereiken invoegtoepassing](https://azure.microsoft.com/pricing/details/expressroute/).

Als u een of beide van de scenario's voor uw implementatie worden toegepast, opent u een bericht ondersteuning met Azure zoals wordt beschreven in [een ondersteuningsaanvraag voor HANA grote instanties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#open-a-support-request-for-hana-large-instances)

Gegevens die nodig is en trefwoorden die u wilt gebruiken voor Microsoft om te kunnen uitvoeren op uw aanvraag en routeren ziet eruit zoals:

- Service: SAP HANA grote instantie
- Probleemtype: Configuratie en installatie
- Probleem-subtype: Mijn probleem staat er niet bij
- Onderwerp 'Mijn netwerk - wijzigen globaal bereik toevoegen'
- Details: ' Globaal bereik toevoegen aan HANA grote instantie aan HANA grote instantie tenant of ' globaal bereik toevoegen naar on-premises naar HANA grote instantie-tenant.
- Aanvullende details voor de HANA grote instantie aan tenant-aanvraag HANA grote instantie: U moet definiëren de **twee Azure-regio's** waar de twee tenants verbinding maken zich bevinden **en** u nodig hebt om in te dienen de   **/29 IP-adresbereik**
- Aanvullende details voor de on-premises met HANA grote instantie tenant aanvraag: U moet definiëren de **Azure-regio** waar de tenant HANA grote instantie wordt geïmplementeerd u rechtstreeks verbinding wilt maken. Daarnaast moet u opgeven de **Auth GUID** en **Circuit Peer ID** dat u hebt ontvangen toen u uw ExpressRoute-circuit tussen on-premises en Azure tot stand gebracht. Daarnaast moet u om de naam van uw **ASN**. De laatste product is een **/29 IP-adresbereik** voor ExpressRoute globaal bereik.

> [!NOTE]
> Als u wilt dat beide gevallen verwerkt, moet u opgeven twee verschillende/29 IP-adresbereiken die elkaar niet overlappen met andere IP-adresbereik gebruikt tot nu toe. 




## <a name="next-steps"></a>Volgende stappen

- [Aanvullende vereisten voor HLI](hana-additional-network-requirements.md)
