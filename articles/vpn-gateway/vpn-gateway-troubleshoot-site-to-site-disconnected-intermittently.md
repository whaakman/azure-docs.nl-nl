---
title: Problemen met Azure Site-naar-Site VPN-verbinding wordt verbroken afwisselend | Microsoft Docs
description: Informatie over het oplossen van het probleem waarin de Site-naar-Site VPN-verbinding regelmatig verbroken.
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 9bcf30fbf1febbd80aa8899253de8cb61f4ae6f8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a>Voor probleemoplossing: Azure Site-naar-Site VPN-verbinding verbreekt met tussenpozen

U kunt het probleem dat in een nieuwe of bestaande Microsoft Azure Site-naar-Site VPN-verbinding niet stabiel is of de verbinding regelmatig verbreekt ondervinden. Dit artikel bevat stappen om te identificeren en oplossen van de oorzaak van het probleem oplossen. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Stappen voor het oplossen van problemen

### <a name="prerequisite-step"></a>Vereiste stap

Controleer het type van de gateway virtuele Azure-netwerk:

1. Ga naar [Azure-portal](https://portal.azure.com).
2. Controleer de **overzicht** pagina van de virtuele netwerkgateway voor de type-informatie.
    
    ![Het overzicht van de gateway](media\vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently\gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a>Stap 1 Controleer of de on-premises VPN-apparaat wordt gevalideerd

1. Controleer of u een [gevalideerde VPN-apparaat en de versie van besturingssysteem](vpn-gateway-about-vpn-devices.md#devicetable). Als het VPN-apparaat is niet gevalideerd, moet u wellicht contact op met de fabrikant van het apparaat om te zien of er een compatibiliteitsprobleem.
2. Zorg ervoor dat het VPN-apparaat correct is geconfigureerd. Zie voor meer informatie [bewerken van apparaatconfiguraties](vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-check-the-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a>Stap 2 Controleer de instellingen van de beveiligingskoppeling (voor gateways voor virtuele Azure-netwerk op basis van beleid)

1. Zorg ervoor dat het virtuele netwerk, subnetten en bereiken in de **lokale netwerkgateway** definitie in Microsoft Azure zijn hetzelfde als de configuratie op de on-premises VPN-apparaat.
2. Controleer of de instellingen van de beveiligingskoppeling die overeenkomen met.

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a>Stap 3-controle voor de gebruiker gedefinieerde Routes of Netwerkbeveiligingsgroepen van Gatewaysubnet

Een gebruiker gedefinieerde route op het gatewaysubnet kan beperken sommige verkeer en andere verkeer toestaat. Op deze manier worden weergegeven dat de VPN-verbinding voor verkeer onbetrouwbaar en geschikt is voor anderen. 

### <a name="step-4-check-the-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a>Stap 4 controleren de 'een VPN-Tunnel per Subnet paar' (voor gateways voor virtueel netwerk op basis van beleid)

Zorg ervoor dat de on-premises VPN-apparaat is ingesteld om **een VPN-tunnel per subnet paar** voor virtuele netwerkgateways op basis van beleid.

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a>Stap 5-controle voor beveiliging koppeling beperking (voor gateways voor virtueel netwerk op basis van beleid)

De op beleid gebaseerde virtuele netwerkgateway heeft de limiet van 200 subnet beveiligingskoppeling paren. Als het aantal virtuele Azure-netwerksubnetten vermenigvuldigd tijden door het nummer van lokale subnetten is groter dan 200, er sporadisch subnetten verbinding wordt verbroken.

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a>Stap 6 Controleer on-premises VPN-apparaat externe Interfaceadres

- Als de IP-adres van het VPN-apparaat voor Internetgericht is opgenomen in de **lokale netwerkgateway** definitie in Azure, treden sporadisch worden verbroken.
- De externe interface van het apparaat moet rechtstreeks op het Internet. Er zijn geen NAT (Network Address Translation) of de firewall tussen het Internet en het apparaat.
-  Als u Firewall Clustering als u wilt dat een virtueel IP-adres configureert, moet u het cluster opsplitsen en weergeven van de VPN-apparaat rechtstreeks naar een openbare interface die de gateway kan verbinding maken met.

### <a name="step-7-check-whether-the-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a>Stap 7 Controleer of de on-premises VPN-apparaat Perfect Forward Secrecy ingeschakeld is

De **Perfect Forward Secrecy** functie kan leiden tot problemen met de verbinding wordt verbroken. Als het VPN-apparaat heeft **Perfect forward Secrecy** ingeschakeld, schakelt u de functie. Vervolgens [bijwerken van de virtuele netwerkgateway IPSec-beleid](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).

## <a name="next-steps"></a>Volgende stappen

- [Een Site-naar-Site-verbinding met een virtueel netwerk configureren](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Beleid voor IPsec/IKE voor Site-naar-Site VPN-verbindingen configureren](vpn-gateway-ipsecikepolicy-rm-powershell.md)

