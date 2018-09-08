---
title: Verbindingsconfiguratie van virtueel netwerk met SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Verbindingsconfiguratie van virtueel netwerk voor gebruik van SAP HANA op Azure (grote instanties).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5efdda485e4e1f5013948c6636b267f0d388f4d5
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164709"
---
# <a name="connecting-a-vnet-to-hana-large-instance-expressroute"></a>Een VNet verbinden met HANA grote instantie ExpressRoute

Als u alle IP-adresbereiken gedefinieerd en de gegevens nu terug van Microsoft hebt, kunt u beginnen met het verbinden van de VNet die u eerder hebt gemaakt met HANA grote instanties. Zodra het Azure-VNet is gemaakt, moet een ExpressRoute-gateway op de VNet-naar de VNet koppelen aan het ExpressRoute-circuit dat is verbonden met de tenant van de klant op het stempel grote instantie worden gemaakt.

> [!NOTE] 
> Deze stap duurt maximaal 30 minuten om uit te voeren, omdat de nieuwe gateway is gemaakt in het toegewezen Azure-abonnement en klikt u vervolgens verbinding met het opgegeven Azure-VNet.

Als er al een gateway bestaat, controleert u of het een ExpressRoute-gateway of niet is. Zo niet, in de gateway moet worden verwijderd en opnieuw gemaakt als een ExpressRoute-gateway. Als een ExpressRoute-gateway is al ingesteld, omdat het Azure-VNet al met het ExpressRoute-circuit voor on-premises connectiviteit verbonden is, gaat u verder met de onderstaande sectie met VNets koppelen.

- Gebruik een van beide de (nieuw) [Azure-portal](https://portal.azure.com/), of PowerShell voor het maken van een ExpressRoute-VPN-gateway verbonden met uw VNet.
  - Als u de Azure-portal, toevoegen van een nieuwe **virtuele netwerkgateway** en selecteer vervolgens **ExpressRoute** als het Gatewaytype.
  - Als u PowerShell in plaats daarvan, eerst downloaden en gebruiken van de meest recente [Azure PowerShell SDK](https://azure.microsoft.com/downloads/) om te controleren of een optimale ervaring. De volgende opdrachten maken een ExpressRoute-gateway. De tekst wordt voorafgegaan door een _$_ zijn door de gebruiker gedefinieerde variabelen die moeten worden bijgewerkt met uw specifieke gegevens.

```PowerShell
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA Large Instances are: HighPerformance or UltraPerformance

# These Commands create the Public IP and ExpressRoute Gateway
$vnet = Get-AzureRmVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

In dit voorbeeld is de gateway-HighPerformance SKU gebruikt. Uw opties zijn HighPerformance of UltraPerformance als de enige gateway-SKU's die worden ondersteund voor SAP HANA op Azure (grote instanties).

> [!IMPORTANT]
> Voor HANA grote instanties van het Type II classs SKU is het gebruik van de Gateway-SKU UltraPerformance verplicht.

**VNets koppelen**

Nu dat het Azure-VNet een ExpressRoute-gateway heeft, kunt u de autorisatie-informatie geleverd door Microsoft gebruiken om de ExpressRoute-gateway met de SAP HANA op Azure (grote instanties) ExpressRoute-circuit gemaakt voor deze connectiviteit verbinding te maken. Deze stap kan worden uitgevoerd met behulp van de Azure portal of PowerShell. De portal wordt aanbevolen, maar PowerShell-instructies zijn als volgt. 

- U voert u de volgende opdrachten voor elke VNet-gateway met behulp van een andere AuthGUID voor elke verbinding. De eerste twee items in het volgende script afkomstig zijn van de informatie die wordt geleverd door Microsoft. Er is ook de AuthGUID specifiek voor elke VNet en een gateway. Betekent dat, als u wilt toevoegen van een ander Azure-VNet, moet u een andere AuthID ophalen voor uw ExpressRoute-circuit dat is verbonden HANA grote instanties in Azure. 

```PowerShell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway Information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzureRmVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Als u verbinding maken met de gateway aan meerdere ExpressRoute-circuits die gekoppeld aan uw abonnement wilt zijn, moet u mogelijk meer dan één keer uitvoeren van deze stap. Bijvoorbeeld, waarschijnlijk gaat u naar het hetzelfde VNet-Gateway naar het ExpressRoute-circuit dat is het VNet met uw on-premises netwerk verbonden verbinding maken.

**Volgende stappen**

- Raadpleeg [extra netwerkvereisten voor HLI](hana-additional-network-requirements.md).