---
title: Een virtueel netwerk in Azure niet verwijderen | Microsoft Docs
description: Informatie over het oplossen van het probleem waarin u een virtueel netwerk in Azure niet verwijderen.
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2017
ms.author: genli
ms.openlocfilehash: 60e4a0dbbc15b0414bf1b81e1fd7885e47aa962d
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-failed-to-delete-a-virtual-network-in-azure"></a>Voor probleemoplossing: Kan niet verwijderen van een virtueel netwerk in Azure

Fouten kan worden weergegeven wanneer u probeert te verwijderen van een virtueel netwerk in Microsoft Azure. Dit artikel bevat de stappen voor probleemoplossing waarmee u kunt dit probleem oplossen. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a>Hulp bij het oplossen van problemen 

1. [Controleer of een virtuele netwerkgateway actief is in het virtuele netwerk](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).
2. [Controleer of een application gateway actief is in het virtuele netwerk](#check-whether-an-application-gateway-is-running-in-the-virtual-network).
3. [Controleer of de Azure Active Directory Domain Services is ingeschakeld in het virtuele netwerk](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).
4. [Controleer of het virtuele netwerk is verbonden met andere resources](#check-whether-the-virtual-network-is-connected-to-other-resource).
5. [Controleer of een virtuele machine nog steeds actief is in het virtuele netwerk](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).
6. [Controleer of het virtuele netwerk is vastgelopen bij migratie](#check-whether-the-virtual-network-is-stuck-in-migration).

## <a name="troubleshooting-steps"></a>Stappen voor het oplossen van problemen

### <a name="check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network"></a>Controleer of een virtuele netwerkgateway actief is in het virtuele netwerk

Als u wilt verwijderen van het virtuele netwerk, moet u eerst de virtuele netwerkgateway verwijderen.

Voor klassieke virtuele netwerken, gaat u naar de **overzicht** pagina van het klassieke virtuele netwerk in de Azure-portal. In de **VPN-verbindingen** sectie, als de gateway wordt uitgevoerd in het virtuele netwerk, ziet u het IP-adres van de gateway. 

![Controleer of de gateway is actief](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

Voor virtuele netwerken, gaat u naar de **overzicht** pagina van het virtuele netwerk. Controleer **verbonden apparaten** voor de virtuele netwerkgateway.

![Controleer het aangesloten apparaat](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

Voordat u de gateway verwijderen kunt, eerste Verwijder een **verbinding** objecten in de gateway. 

### <a name="check-whether-an-application-gateway-is-running-in-the-virtual-network"></a>Controleer of een application gateway actief is in het virtuele netwerk

Ga naar de **overzicht** pagina van het virtuele netwerk. Controleer de **verbonden apparaten** voor de toepassingsgateway.

![Controleer het aangesloten apparaat](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

Als er een application gateway, moet u deze verwijderen voordat u het virtuele netwerk kunt verwijderen.

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network"></a>Controleer of de Azure Active Directory Domain Services is ingeschakeld in het virtuele netwerk

Als de Active Directory Domain Services is ingeschakeld en met het virtuele netwerk verbonden, kunt u dit virtuele netwerk niet verwijderen. 

![Controleer het aangesloten apparaat](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

Zie de service uitschakelen [uitschakelen Azure Active Directory Domain Services met Azure portal](../active-directory-domain-services/active-directory-ds-disable-aadds.md).

### <a name="check-whether-the-virtual-network-is-connected-to-other-resource"></a>Controleer of het virtuele netwerk is verbonden met andere resources

Controleren op Circuitkoppelingen, verbindingen en virtuele netwerk peerings. Deze kan leiden tot een virtueel netwerk verwijderen is mislukt. 

De volgorde van de aanbevolen verwijdering is als volgt:

1. Gatewayverbindingen
2. Gateways
3. IP-adressen
4. Virtueel netwerk peerings
5. App Service-omgeving (as-omgeving)

### <a name="check-whether-a-virtual-machine-is-still-running-in-the-virtual-network"></a>Controleer of een virtuele machine nog steeds actief is in het virtuele netwerk

Zorg ervoor dat er geen virtuele machine zich in het virtuele netwerk.

### <a name="check-whether-the-virtual-network-is-stuck-in-migration"></a>Controleer of het virtuele netwerk is vastgelopen bij migratie

Als het virtuele netwerk is vastgelopen in de migratiestatus van een, kan niet worden verwijderd. Voer de volgende opdracht om af te breken van de migratie en verwijder vervolgens het virtuele netwerk.

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a>Volgende stappen

- [Azure Virtual Network](virtual-networks-overview.md)
- [Veelgestelde vragen over virtuele Azure-netwerken (FAQ)](virtual-networks-faq.md)
