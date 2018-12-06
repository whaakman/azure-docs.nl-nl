---
title: Oplossen van Azure Site-naar-Site VPN wordt herhaaldelijk verbroken | Microsoft Docs
description: Informatie over het oplossen van het probleem waarin de Site-naar-Site VPN-verbinding regelmatig verbroken.
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: ''
tags: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 2fdd82c2f0c96b3bd20231911bb88cf54c172931
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52958011"
---
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a>Problemen oplossen: Azure Site-naar-Site-VPN wordt herhaaldelijk verbroken

U kunt het probleem dat in een nieuwe of bestaande Microsoft Azure Site-naar-Site VPN-verbinding niet stabiel is of de verbinding regelmatig verbreekt ondervinden. Dit artikel bevat stappen om u te identificeren en oplossen van de oorzaak van het probleem oplossen. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

### <a name="prerequisite-step"></a>Vereiste stap

Controleer het type virtuele netwerkgateway van Azure:

1. Ga naar [Azure-portal](https://portal.azure.com).
2. Controleer de **overzicht** pagina van de virtuele netwerkgateway voor informatie over het.
    
    ![Het overzicht van de gateway](media/vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently/gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a>Stap 1 Controleer of de on-premises VPN-apparaat wordt gevalideerd

1. Controleer of u gebruikt een [gevalideerde VPN-apparaat en de versie van besturingssysteem](vpn-gateway-about-vpn-devices.md#devicetable). Als het VPN-apparaat is niet gevalideerd, is het wellicht contact opnemen met de fabrikant van het apparaat om te zien of er een compatibiliteitsprobleem op.
2. Zorg ervoor dat het VPN-apparaat correct is geconfigureerd. Zie voor meer informatie, [bewerken van apparaatconfiguraties](vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-check-the-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a>Stap 2 Controleer de instellingen van de beveiligingskoppeling (voor virtuele Azure-netwerk op beleid gebaseerde gateways)

1. Zorg ervoor dat het virtuele netwerk, subnetten en bereiken in de **lokale netwerkgateway** definitie in Microsoft Azure zijn hetzelfde als de configuratie op de on-premises VPN-apparaat.
2. Controleer of de beveiligingskoppeling-instellingen die overeenkomen met.

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a>Stap 3-controle voor de gebruiker gedefinieerde Routes of de Netwerkbeveiligingsgroepen op het Gatewaysubnet

Een door de gebruiker gedefinieerde route op het gatewaysubnet kan worden bepaald verkeer beperken en andere verkeer wordt toegestaan. Dit maakt het worden weergegeven dat de VPN-verbindingen onbetrouwbaar voor een deel van netwerkverkeer en geschikt is voor anderen. 

### <a name="step-4-check-the-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a>Stap 4 de 'één VPN-Tunnel per Subnet paar' controleren (voor gateways voor virtueel netwerk op basis van beleid)

Zorg ervoor dat de on-premises VPN-apparaat is ingesteld dat **één VPN-tunnel per subnet paar** voor virtuele netwerkgateways op basis van beleid.

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a>Stap 5-controle voor beperking van de Security Association (voor gateways voor virtueel netwerk op basis van beleid)

De op beleid gebaseerde virtuele netwerkgateway heeft de limiet van 200 subnet beveiligingskoppeling paren. Als het aantal Azure-netwerksubnetten vermenigvuldigd tijden door het aantal lokale subnetten groter is dan 200, ziet u sporadisch subnetten verbreken.

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a>Stap 6 selectievakje on-premises externe interface-adres van VPN-apparaat

- Als het internetgerichte IP-adres van het VPN-apparaat is opgenomen in de **lokale netwerkgateway** definitie in Azure, treden er kunnen sporadisch worden verbroken.
- De externe interface van het apparaat moet rechtstreeks op het Internet. Er zijn geen NAT (Network Address Translation) of de firewall tussen het Internet en het apparaat.
-  Als u de Firewall Clustering als u een virtueel IP-adres wilt configureren, moet u het einde van het cluster en rechtstreeks naar een openbare interface die de gateway met samenwerken kan het VPN-apparaat beschikbaar.

### <a name="step-7-check-whether-the-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a>Stap 7 Controleer of de on-premises VPN-apparaat Perfect Forward Secrecy ingeschakeld is

De **Perfect Forward Secrecy** functie kan leiden tot problemen met de verbinding verbreken. Als het VPN-apparaat heeft **Perfect forward Secrecy** ingeschakeld, schakelt u de functie. Vervolgens [bijwerken van de virtuele netwerkgateway IPsec-beleid](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).

## <a name="next-steps"></a>Volgende stappen

- [Een Site-naar-Site-verbinding met een virtueel netwerk configureren](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [IPsec/IKE-beleid voor Site-naar-Site VPN-verbindingen configureren](vpn-gateway-ipsecikepolicy-rm-powershell.md)

