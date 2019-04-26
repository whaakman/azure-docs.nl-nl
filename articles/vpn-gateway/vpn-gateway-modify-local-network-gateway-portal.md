---
title: Het lokale netwerk gateway IP-adresvoorvoegsels en het VPN-Gateway-IP-adres wijzigen | Azure | Portal | Microsoft Docs
description: Dit artikel begeleidt u bij het wijzigen van IP-adresvoorvoegsels voor uw lokale netwerkgateway met behulp van de Azure portal.
services: vpn-gateway
documentationcenter: na
author: WenJason
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 06/19/2017
ms.date: 12/10/2018
ms.author: v-jay
ms.openlocfilehash: 12f1f8bbcb103d0882059cadc12bc1a8b9d40bdb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60419583"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Lokale instellingen netwerkgateway wijzigen met behulp van de Azure portal

Soms worden de instellingen voor uw lokale netwerkgateway AddressPrefix of GatewayIPAddress gewijzigd. Dit artikel ziet u hoe u uw instellingen van uw lokale netwerkgateway wijzigen. U kunt ook wijzigen van deze instellingen met een andere methode door een andere optie te selecteren in de volgende lijst:

Voordat u de verbinding verwijdert, kunt u de configuratie voor uw verbindende apparaten om op te halen van de gedefinieerde PSK downloaden. Op die manier hoeft u te definiëren het aan de andere kant.

> [!div class="op_single_selector"]
> * [Azure-portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure-CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <a name="ipaddprefix"></a>IP-adresvoorvoegsels wijzigen

Wanneer u IP-adresvoorvoegsels wijzigt, afhankelijk van de te volgen stappen of de gateway van uw lokale netwerk een verbinding heeft.

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <a name="gwip"></a>IP-adres van de gateway wijzigen

Als van het VPN-apparaat waarmee u verbinding wilt maken het openbare IP-adres is gewijzigd, moet u de gateway van het lokale netwerk aanpassen met deze wijziging. Wanneer u het openbare IP-adres wijzigt, afhankelijk van de te volgen stappen of de gateway van uw lokale netwerk een verbinding heeft.

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a>Volgende stappen

U kunt de gatewayverbinding controleren. Zie [een gatewayverbinding verifiëren](vpn-gateway-verify-connection-resource-manager.md).