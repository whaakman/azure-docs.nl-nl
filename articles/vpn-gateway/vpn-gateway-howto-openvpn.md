---
title: 'Het configureren van OpenVPN op Azure VPN-Gateway: PowerShell | Microsoft Docs'
description: Stappen voor het configureren van OpenVPN voor Azure VPN-Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: cherylmc
ms.openlocfilehash: 609c2ef91fafe0ae955252a594292d861e772f87
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002966"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway"></a>OpenVPN voor Azure point-to-site VPN-Gateway configureren

Dit artikel helpt u bij het instellen van **OpenVPN® Protocol** op Azure VPN-Gateway. Het artikel wordt ervan uitgegaan dat u al een werkomgeving voor punt-naar-site. Als u dit niet doet, gebruikt u de instructies in stap 1 te maken van een punt-naar-site-VPN.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="vnet"></a>1. Een punt-naar-site-VPN maken

Als u nog een werkende punt-naar-site-omgeving hebt, volgt u de instructies voor het maken van een. Zie [maken van een punt-naar-site-VPN](vpn-gateway-howto-point-to-site-resource-manager-portal.md) maken en configureren van een punt-naar-site VPN-gateway met systeemeigen Azure certificaatverificatie. 

> [!IMPORTANT]
> De basis-SKU wordt niet ondersteund voor OpenVPN.

## <a name="enable"></a>2. OpenVPN inschakelen op de gateway

Schakel OpenVPN op uw gateway. Zorg ervoor dat de gateway is al geconfigureerd voor punt-naar-site (IKEv2 of SSTP) voordat u de volgende opdrachten uitvoert:

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>Volgende stappen

Zie configureren van clients voor OpenVPN [OpenVPN configureren clients](vpn-gateway-howto-openvpn-clients.md).

**"OpenVPN" is een handelsmerk van OpenVPN Inc.**
