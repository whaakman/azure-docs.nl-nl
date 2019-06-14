---
title: Het lokale netwerk gateway IP-adresvoorvoegsels en het VPN-Gateway-IP-adres wijzigen | Azure | CLI | Microsoft Docs
description: Dit artikel begeleidt u bij het wijzigen van IP-adresvoorvoegsels voor uw lokale netwerkgateway met de Azure CLI.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 8c7db48f-d09a-44e7-836f-1fb6930389df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/29/2017
ms.author: cherylmc
ms.openlocfilehash: 20e8269f0ee7463dcff9a5f66e46ebd81cb3df84
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60652843"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a>Lokale instellingen netwerkgateway wijzigen met behulp van de Azure CLI

Soms worden de instellingen voor uw lokale netwerkgateway het adresvoorvoegsel of de Gateway-IP-adres gewijzigd. Dit artikel ziet u hoe u uw instellingen van uw lokale netwerkgateway wijzigen. U kunt ook wijzigen van deze instellingen met een andere methode door een andere optie te selecteren in de volgende lijst:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure-CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before"></a>Voordat u begint

Installeer de nieuwste versie van de CLI-opdrachten (2.0 of hoger). Zie [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) voor meer informatie over het installeren van de CLI-opdrachten.

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="ipaddprefix"></a>IP-adresvoorvoegsels wijzigen

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <a name="gwip"></a>IP-adres van de gateway wijzigen

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a>Volgende stappen

U kunt de gatewayverbinding controleren. Zie [een gatewayverbinding verifiëren](vpn-gateway-verify-connection-resource-manager.md).

