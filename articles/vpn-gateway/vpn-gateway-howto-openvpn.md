---
title: 'Het configureren van OpenVPN op Azure VPN-Gateway: PowerShell | Microsoft Docs'
description: Stappen voor het configureren van OpenVPN voor Azure VPN-Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: cherylmc
ms.openlocfilehash: 8cc2a6d4ad06bf4a55d4ef078970823df1e0d910
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59281961"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway-preview"></a>OpenVPN configureren voor Azure point-to-site VPN-Gateway (Preview)

Dit artikel helpt u bij het instellen van **OpenVPN® Protocol** op Azure VPN-Gateway. Het artikel wordt ervan uitgegaan dat u al een werkomgeving voor punt-naar-site. Als u dit niet doet, gebruikt u de instructies in stap 1 te maken van een punt-naar-site-VPN.

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder serviceovereenkomst en moet niet worden gebruikt voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="register"></a>Deze functie registreren

Klik op de **TryIt** in deze stappen voor het registreren van deze functie eenvoudig met Azure Cloud Shell.

>[!NOTE]
>Als u deze functie niet registreert, wordt het niet mogelijk om deze te gebruiken.
>

Wanneer u op **Proberen** hebt geklikt om Azure Cloud Shell te openen, kopieert en plakt u de volgende opdrachten:

```azurepowershell-interactive
Register-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```
 
```azurepowershell-interactive
Get-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

Zodra wordt weergegeven dat de functie is geregistreerd, registreert u het abonnement opnieuw bij de naamruimte Microsoft.Network.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

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