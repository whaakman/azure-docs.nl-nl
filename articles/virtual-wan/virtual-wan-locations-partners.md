---
title: Azure virtuele WAN locaties-partners | Microsoft Docs
description: In dit artikel bevat een overzicht van Azure virtuele WAN-partners en locaties van de hub.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect find a Virtual WAN partner
ms.openlocfilehash: f38cd0565b2e90fe0803d8e815c622e22e954a18
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57409700"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Virtuele WAN-partners en locaties van de virtuele hub

Dit artikel bevat informatie over virtuele WAN ondersteunde regio's en partners voor verbindingen in de virtuele Hub.

Azure Virtual WAN is een netwerkservice die via Azure voor een geoptimaliseerde en geautomatiseerde verbinding tussen filialen zorgt. Met Virtual WAN kunt u apparaten in filialen verbinden en configureren, zodat deze met Azure kunnen communiceren. Dit kan worden gedaan ofwel handmatig of met behulp van de provider-apparaten via een virtueel WAN-partner. Partner-apparaten, kunt dat u eenvoudig gebruik, connectiviteit en beheer van de configuratie te vereenvoudigen.

De connectiviteit van de on-premises apparaat in een automatische manier voor het virtuele Hub tot stand is gebracht. Een virtuele hub is een Microsoft-beheerde virtueel netwerk. De hub bevat verschillende service-eindpunten die verbindingen vanaf uw on-premises netwerk (vpnsite) mogelijk maken. U kunt slechts één hub per regio hebben.

## <a name="automation"></a>Automatisering van connectiviteitspartners

Apparaten die verbinding met Azure virtuele WAN maken hebben ingebouwde automation om verbinding te maken. Dit wordt meestal ingesteld omhoog in de Apparaatbeheer gebruikersinterface (of equivalent), waarmee de connectiviteit en configuration management tussen het filiaal VPN-apparaat naar een Azure-virtuele Hub-VPN-eindpunt (VPN-gateway) wordt ingesteld.

De volgende automatisering op hoog niveau is ingesteld in het midden van de console/Beheer van apparaten:

* Juiste machtigingen voor het apparaat toegang tot Azure virtuele WAN Resource Group
* Uploaden van Branch-apparaat naar de Azure virtuele WAN
* Automatische downloads van informatie over Azure connectiviteit
* Configuratie van on-premises vertakking apparaat 

Sommige connectiviteitspartners kunnen uitbreiden om op te nemen met het maken van de Azure virtuele Hub VNet en VPN-Gateway. Als u meer weten over automation wilt, Zie [configureren Automation – WAN-Partners](virtual-wan-configure-automation-providers.md).

## <a name="partners"></a>Connectiviteit via partners

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

## <a name="locations"></a>locaties

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over virtuele WAN, de [virtuele WAN-Veelgestelde vragen over](virtual-wan-faq.md).

* Zie voor meer informatie over het automatiseren van de verbinding met Azure virtuele WAN [virtuele WAN-Partners - automatiseren](virtual-wan-configure-automation-providers.md).
