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
ms.openlocfilehash: cb14d0784ecb87c85b02952880e9eb5744d205a2
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58850659"
---
# <a name="connect-a-virtual-network-to-hana-large-instances"></a>Een virtueel netwerk verbinden met HANA grote instanties

Nadat u een Azure-netwerk hebt gemaakt, kunt u dat netwerk verbinden met SAP HANA op Azure grote instanties. Maak een Azure ExpressRoute-gateway op het virtuele netwerk. Deze gateway kunt u het virtueel netwerk koppelen aan het ExpressRoute-circuit dat is verbonden met de tenant van de klant op het stempel grote instantie.

> [!NOTE] 
> Deze stap kan tot 30 minuten duren. De nieuwe gateway is gemaakt in het toegewezen Azure-abonnement en klikt u vervolgens verbinding met de opgegeven Azure-netwerk.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

Als er al een gateway bestaat, controleert u of het een ExpressRoute-gateway of niet is. Als dit niet het geval is, de gateway verwijderen en opnieuw maken als een ExpressRoute-gateway. Als een ExpressRoute-gateway al tot stand is gebracht, raadpleegt u de volgende sectie van dit artikel "Koppeling virtuele netwerken." 

- Een gebruiken de [Azure-portal](https://portal.azure.com/) of PowerShell voor het maken van een ExpressRoute-VPN-gateway verbonden met uw virtuele netwerk.
  - Als u de Azure-portal, toevoegen van een nieuwe **virtuele netwerkgateway**, en selecteer vervolgens **ExpressRoute** als het Gatewaytype.
  - Als u PowerShell gebruikt, eerst downloaden en gebruiken van de meest recente [Azure PowerShell SDK](https://azure.microsoft.com/downloads/). De volgende opdrachten maken een ExpressRoute-gateway. De tekst wordt voorafgegaan door een _$_ zijn door de gebruiker gedefinieerde variabelen die moeten worden bijgewerkt met uw specifieke gegevens.

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

Het Azure-netwerk heeft nu een ExpressRoute-gateway. Gebruik de autorisatie-informatie die is geleverd door Microsoft om de ExpressRoute-gateway met de SAP HANA op Azure (grote instanties) ExpressRoute-circuit. U kunt verbinding maken met behulp van de Azure portal of PowerShell. De portal wordt aanbevolen, maar als u PowerShell gebruiken wilt, de instructies zijn als volgt. 

Voer de volgende opdrachten voor elke virtuele netwerkgateway met behulp van een andere AuthGUID voor elke verbinding. De eerste twee items in het volgende script afkomstig zijn van de informatie die wordt geleverd door Microsoft. Er is ook de AuthGUID specifiek voor elke virtueel netwerk en de gateway. Als u toevoegen van een andere Azure-netwerk wilt, moet u een andere AuthID ophalen voor uw ExpressRoute-circuit dat is verbonden HANA grote instanties in Azure. 

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
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Als u wilt verbinding maken met de gateway aan meer dan één ExpressRoute-circuit dat is gekoppeld aan uw abonnement, moet u mogelijk meer dan één keer uitvoeren van deze stap. Bijvoorbeeld, gaat u waarschijnlijk de dezelfde virtuele netwerkgateway naar het ExpressRoute-circuit dat is het virtuele netwerk met uw on-premises netwerk verbonden verbinding maken.

## <a name="next-steps"></a>Volgende stappen

- [Aanvullende vereisten voor HLI](hana-additional-network-requirements.md)
