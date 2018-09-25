---
title: Azure virtuele WAN locaties-partners | Microsoft Docs
description: In dit artikel bevat een overzicht van Azure virtuele WAN-partners en locaties van de hub
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect find a Virtual WAN partner
ms.openlocfilehash: 76542392522ff14642ed7ccc6de8ed543d66513f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984990"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Virtuele WAN-partners en locaties van de virtuele hub

Dit artikel bevat informatie over virtuele WAN ondersteunde regio's en gewenste partners voor verbindingen in de virtuele Hub.

Azure Virtual WAN is een netwerkservice die via Azure voor een geoptimaliseerde en geautomatiseerde verbinding tussen filialen zorgt. Met Virtual WAN kunt u apparaten in filialen verbinden en configureren, zodat deze met Azure kunnen communiceren. Dit kan worden gedaan ofwel handmatig of met behulp van de gewenste provider apparaten via een virtueel WAN partner van voorkeur. Partner van voorkeur apparaten gebruikt, kunt dat u eenvoudig gebruik, connectiviteit en beheer van de configuratie te vereenvoudigen. De connectiviteit van de on-premises apparaat in een automatische manier voor het virtuele Hub tot stand is gebracht. Een virtuele hub is een Microsoft-beheerde virtueel netwerk. De hub bevat verschillende service-eindpunten die verbindingen vanaf uw on-premises netwerk (vpnsite) mogelijk maken. U kunt slechts één hub per regio hebben.

## <a name="regions"></a>Regio's

De lijst met regio's ondersteund en beschikbaar zijn als volgt:

|Geopolitieke regio | Azure-regio's|
|---|---|
|Noord-Amerika | US - oost, US - west, US - oost 2, US - west 2, US - centraal, US - zuid-centraal, US - noord-centraal, US - west-centraal, Canada Centraal, Canada Oost |
|Zuid-Amerika |Brazilië - zuid |
| Europa | Frankrijk - centraal, Frankrijk - zuid, Europa - noord, Europa - west, VK - west, VK - zuid |
| Azië | Azië - oost, Azië - zuidoost |
| Japan  | Japan - west, Japan - oost |
| Australië | Australië - zuidoost, Australië - oost | 
| Australië Government | Australië Centraal, Australië Centraal 2 |
| India | India - west, India - centraal, India - zuid |
| Zuid-Korea | Korea Centraal, Korea Zuid | 

## <a name="automation-from-connectivity-partners"></a>Automatisering van connectiviteitspartners

Deze sectie beschrijft de details op hoog niveau van automatisering van connectiviteitsproviders.

Apparaten die verbinding met Azure virtuele WAN maken hebben ingebouwde automation om verbinding te maken. Dit wordt meestal ingesteld omhoog in de Apparaatbeheer gebruikersinterface (of equivalent), waarmee de connectiviteit en configuration management tussen het filiaal VPN-apparaat naar een Azure-virtuele Hub-VPN-eindpunt (VPN-gateway) wordt ingesteld.

De volgende automatisering op hoog niveau is ingesteld in het midden van de console/Beheer van apparaten:

* Juiste machtigingen voor het apparaat toegang tot Azure virtuele WAN Resource Group
* Uploaden van Branch-apparaat naar de Azure virtuele WAN
* Automatische downloads van informatie over Azure connectiviteit 
* Configuratie van on-premises vertakking apparaat 

Sommige connectiviteitspartners kunnen uitbreiden om op te nemen met het maken van de Azure virtuele Hub VNet en VPN-Gateway. Als u meer weten over automation wilt, Zie [configureren Automation – WAN-Partners](virtual-wan-configure-automation-providers.md).

## <a name="connectivity-through-preferred-partners"></a>Connectiviteit via aanbevolen partners

Als uw apparaat vertakking partner niet wordt vermeld in de onderstaande sectie, moet u contact op met uw provider vertakking-apparaat en hebben ze contact met ons opnemen door te sturen een e-mail naar azurevirtualwan@microsoft.com.

U kunt de volgende koppelingen vindt u meer informatie over de services die worden aangeboden door de gewenste partners controleren. 

|Aanbevolen Partners|
|---|
|[Barracuda Networks](https://www.barracuda.com/AzurevWAN)|
| [Check Point](https://www.checkpoint.com/solutions/microsoft-azure-virtual-wan/) |
| [Citrix](https://www.citrix.com/global-partners/microsoft/sd-wan-for-azure-virtual-wan.html)|
|[Netfoundry](https://netfoundry.io/solutions/netfoundry-for-microsoft-azure-virtual-wan/)|
|[Palo Alto Networks](https://researchcenter.paloaltonetworks.com/2018/09/azure-vwan-integration/) |
|[Riverbed-technologie](https://www.riverbed.com/go/steelconnect-azurewan.html)|
|[128-technologie](https://www.128technology.com/partners/azure) |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over virtuele WAN, de [virtuele WAN-Veelgestelde vragen over](virtual-wan-faq.md).

Zie voor meer informatie over het automatiseren van de verbinding met Azure virtuele WAN [virtuele WAN-Partners - automatiseren](virtual-wan-configure-automation-providers.md).
