---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/23/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 3cffd2de0763ea6984b64b965ce1214951d3d569
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056476"
---
### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Hoeveel VPN-clienteindpunten kan mijn punt-naar-site-configuratie hebben?

Dat hangt ervan af op de gateway-SKU. Zie voor meer informatie over het aantal verbindingen ondersteund [Gateway-SKU's](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku).

### <a name="supportedclientos"></a>Welke clientbesturingssystemen kan ik met point-to-site gebruiken?

De volgende clientbesturingssystemen worden ondersteund:

* Windows 7 (32-bits en 64-bits)
* Windows Server 2008 R2 (alleen 64-bits)
* Windows 8.1 (32-bits en 64-bits)
* Windows Server 2012 (alleen 64-bits)
* Windows Server 2012 R2 (alleen 64-bits)
* Windows Server 2016 (alleen 64-bits)
* Windows 10
* Mac OS X-versie 10.11 of hoger
* Linux (StrongSwan)
* iOS

[!INCLUDE [TLS](vpn-gateway-tls-updates.md)]

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Kan ik met punt-naar-site-functionaliteit proxy's en firewalls passeren?

Azure ondersteunt drie typen van punt-naar-site VPN-opties:

* Secure Socket Tunneling Protocol (SSTP). SSTP is een Microsoft bedrijfseigen SSL gebaseerde oplossing die firewalls passeren kan, omdat de meeste firewalls openen de uitgaande TCP-poort 443 SSL wordt gebruikt.

* OpenVPN. OpenVPN is een op SSL gebaseerde oplossing die firewalls passeren kan, omdat de meeste firewalls openen de uitgaande TCP-poort 443 SSL wordt gebruikt.

* IKEv2 VPN. IKEv2 VPN is een op standaarden gebaseerde IPsec VPN-oplossing die gebruikmaakt van uitgaande UDP-poort 500 en 4500 en IP-protocol niet. 50. Firewalls openen deze poorten niet altijd, zodat de kans bestaat dat een IKEv2 VPN proxy's en firewalls niet kan passeren.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Als ik een clientcomputer die is geconfigureerd voor punt-naar-site opnieuw start, wordt de VPN-verbinding dan automatisch opnieuw tot stand gebracht?

Standaard wordt de VPN-verbinding niet automatisch op de clientcomputer hersteld.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Biedt punt-naar-site ondersteuning voor automatisch opnieuw verbinding maken en DDNS op de VPN-clients?

Automatisch opnieuw verbinding maken en DDNS worden momenteel niet ondersteund in VPN’s met punt-naar-site-verbinding.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kunnen site-naar-site- en punt-naar-site-configuraties naast elkaar worden gebruikt in hetzelfde virtuele netwerk?

Ja. Voor het Resource Manager-implementatiemodel moet u een RouteBased VPN-type hebben voor uw gateway. Voor het klassieke implementatiemodel hebt u een dynamische gateway nodig. Point-to-site wordt niet ondersteund voor VPN-gateways met statische routering of PolicyBased VPN-gateways.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kan ik een punt-naar-site-client configureren om verbinding te maken met meerdere virtuele netwerken tegelijk?

Nee. Een point-to-site-client kan alleen verbinding maken met resources in het VNet waarin de virtuele netwerkgateway zich bevindt.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Hoeveel doorvoer kan ik verwachten via site-naar-site- of punt-naar-site-verbindingen?

Het is moeilijk om de exacte doorvoer van de VPN-tunnels te onderhouden. IPSec en SSTP zijn cryptografisch zware VPN-protocollen. Doorvoer wordt ook beperkt door de latentie en bandbreedte tussen uw locatie en het internet. Voor een VPN-gateway met alleen IKEv2 point-to-site-VPN-verbindingen is de totale doorvoer die u kunt verwachten, afhankelijk van de Gateway-SKU. Zie [Gateway-SKU's](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku) voor meer informatie over doorvoer.

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp-andor-ikev2"></a>Kan ik voor point-to-site elke VPN-softwareclient gebruiken die SSTP en/of IKEv2 ondersteunt?

Nee. U kunt alleen de systeemeigen VPN-client van Windows voor SSTP en de systeemeigen VPN-client van Mac voor IKEv2 gebruiken. U kunt echter de OpenVPN-client op alle platforms verbinding maken via OpenVPN-protocol gebruiken. Raadpleeg de lijst met ondersteunde clientbesturingssystemen.

### <a name="does-azure-support-ikev2-vpn-with-windows"></a>Biedt Azure ondersteuning voor IKEv2-VPN met Windows?

IKEv2 wordt ondersteund op Windows 10 en Server 2016. Als u IKEv2 wilt gebruiken, moet u lokaal updates installeren en een registersleutelwaarde instellen. OS-versies voorafgaand aan Windows 10 worden niet ondersteund en SSTP kunnen alleen worden gebruikt of **OpenVPN® Protocol**.

Windows 10 of Server 2016 voorbereiden voor IKEv2:

1. Installeer de update.

   | Versie van het besturingssysteem | Date | Aantal/koppeling |
   |---|---|---|
   | Windows Server 2016<br>Windows 10 versie 1607 | 17 januari 2018 | [KB4057142](https://support.microsoft.com/help/4057142/windows-10-update-kb4057142) |
   | Windows 10 versie 1703 | 17 januari 2018 | [KB4057144](https://support.microsoft.com/help/4057144/windows-10-update-kb4057144) |
   | Windows 10 Version 1709 | 22 maart 2018 | [KB4089848](https://www.catalog.update.microsoft.com/search.aspx?q=kb4089848) |
   |  |  |  |

2. De registersleutelwaarde instellen. Maak of stel de REG_DWORD-sleutel 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\ IKEv2\DisableCertReqPayload' in het register in op 1.

### <a name="what-happens-when-i-configure-both-sstp-and-ikev2-for-p2s-vpn-connections"></a>Wat gebeurt er als ik zowel SSTP als IKEv2 voor P2S-VPN-verbindingen configureer?

Als u zowel SSTP en IKEv2 configureert in een gemengde omgeving (bestaande uit Windows- en Mac-apparaten), probeert de VPN-client van Windows altijd als eerst de IKEv2-tunnel, maar valt de client terug op SSTP als de IKEv2-verbinding niet is geslaagd. MacOSX maakt alleen verbinding via IKEv2.

### <a name="other-than-windows-and-mac-which-other-platforms-does-azure-support-for-p2s-vpn"></a>Welke platformen, naast Windows en Mac, worden door Azure ondersteund voor P25-VPN?

Azure biedt voor P2S VPN ondersteuning voor Windows, Mac en Linux.

### <a name="i-already-have-an-azure-vpn-gateway-deployed-can-i-enable-radius-andor-ikev2-vpn-on-it"></a>Ik heb al een Azure VPN-gateway geïmplementeerd. Kan ik er RADIUS en/of IKEv2 VPN voor inschakelen?

Ja, u kunt deze nieuwe functies op reeds geïmplementeerde gateways inschakelen met behulp van PowerShell of Azure Portal, mits de gateway-SKU die u gebruikt RADIUS en/of IKEv2 ondersteunt. Bijvoorbeeld de VPN-gateway Basic SKU ondersteunt RADIUS of IKEv2 niet.
