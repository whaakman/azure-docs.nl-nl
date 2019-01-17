---
title: 'Het configureren van OpenVPN op Azure VPN-Gateway: PowerShell | Microsoft Docs'
description: Stappen voor het configureren van OpenVPN voor Azure VPN-Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: cherylmc
ms.openlocfilehash: 1dad960b0877cddf3be9afc01e3e687ebe4702c0
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54357824"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway-preview"></a>OpenVPN configureren voor Azure point-to-site VPN-Gateway (Preview)

Dit artikel helpt u bij het instellen van OpenVPN op Azure VPN-Gateway. Het artikel wordt ervan uitgegaan dat u al een werkomgeving voor punt-naar-site. Als u dit niet doet, gebruikt u de instructies in stap 1 te maken van een punt-naar-site-VPN.

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder serviceovereenkomst en moet niet worden gebruikt voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register"></a>Deze functie registreren

Klik op de **TryIt** in deze stappen voor het registreren van deze functie eenvoudig met Azure Cloud Shell.

>[!NOTE]
>Als u deze functie niet registreert, wordt het niet mogelijk om deze te gebruiken.
>

Wanneer u op **Proberen** hebt geklikt om Azure Cloud Shell te openen, kopieert en plakt u de volgende opdrachten:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```
 
```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

Zodra wordt weergegeven dat de functie is geregistreerd, registreert u het abonnement opnieuw bij de naamruimte Microsoft.Network.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="vnet"></a>1. Een punt-naar-site-VPN maken

Als u nog een werkende punt-naar-site-omgeving hebt, volgt u de instructies voor het maken van een. Zie [maken van een punt-naar-site-VPN](vpn-gateway-howto-point-to-site-resource-manager-portal.md) maken en configureren van een punt-naar-site VPN-gateway met systeemeigen Azure certificaatverificatie. Houd er rekening mee dat de basis-SKU wordt niet ondersteund voor IKEv2 point-to-site.

## <a name="cmdlets"></a>2. PowerShell-cmdlets installeren

Installeer de meest recente versie van de PowerShell-cmdlets van Resource Manager. Zie [How to install and configure Azure PowerShell](/powershell/azure/overview) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van de PowerShell-cmdlets. Dit is belangrijk omdat eerdere versies van de cmdlets niet de huidige waarden bevatten die u nodig hebt voor deze oefening.

## <a name="enable"></a>3. OpenVPN inschakelen op de gateway

Schakel OpenVPN op uw gateway. Zorg ervoor dat de gateway is al geconfigureerd voor punt-naar-site (IKEv2 of SSTP) voordat u de volgende opdrachten uitvoert:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>Volgende stappen

Zie configureren van clients voor OpenVPN [OpenVPN configureren clients](vpn-gateway-howto-openvpn-clients.md).
