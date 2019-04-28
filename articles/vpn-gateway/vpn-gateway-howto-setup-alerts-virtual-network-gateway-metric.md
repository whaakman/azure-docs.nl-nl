---
title: Over het instellen van waarschuwingen over metrische gegevens voor Azure VPN-Gateway
description: Stappen voor het configureren van waarschuwingen over metrische gegevens van VPN-Gateway
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: 890b096acba601ec20efaac21155da84e77a1f31
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63769464"
---
# <a name="setting-up-alerts-on-vpn-gateway-metrics"></a>Instellen van waarschuwingen over metrische gegevens van VPN-Gateway

Dit artikel helpt u bij het instellen van waarschuwingen voor metrische gegevens van VPN-Gateway. Azure monitor biedt de mogelijkheid voor het instellen van waarschuwingen voor Azure-resources. Waarschuwingen kunnen worden ingesteld voor virtuele netwerkgateways van het type 'VPN'.


|**Gegevens**   | **Eenheid** | **Granulariteit** | **Beschrijving** | 
|---       | ---        | ---       | ---            | ---       |
|**AverageBandwidth**| Bytes/s  | 5 minuten| Gemiddelde gebruik van de gecombineerde bandbreedte van alle site-naar-site-verbindingen op de gateway.     |
|**P2SBandwidth**| Bytes/s  | 1 minuut  | Gemiddelde gebruik van de gecombineerde bandbreedte van alle punt-naar-site-verbindingen op de gateway.    |
|**P2SConnectionCount**| Count  | 1 minuut  | Telling van P2S-verbindingen op de gateway.   |
|**TunnelAverageBandwidth** | Bytes/s    | 5 minuten  | Gemiddelde bandbreedtegebruik van tunnels die zijn gemaakt op de gateway. |
|**TunnelEgressBytes** | Bytes | 5 minuten | Uitgaand verkeer in die zijn gemaakt op de gateway-tunnels.   |
|**TunnelEgressPackets** | Count | 5 minuten | Het aantal uitgaande pakketten op tunnels die zijn gemaakt op de gateway.   |
|**TunnelEgressPacketDropTSMismatch** | Count | 5 minuten | Het aantal uitgaande pakketten verwijderd in veroorzaakt door een incompatibel TS-tunnels. |
|**TunnelIngressBytes** | Bytes | 5 minuten | Verkeer dat binnenkomt op tunnels die zijn gemaakt op de gateway.   |
|**TunnelIngressPackets** | Count | 5 minuten | Het aantal inkomende pakketten in die zijn gemaakt op de gateway-tunnels.   |
|**TunnelIngressPacketDropTSMismatch** | Count | 5 minuten | Het aantal inkomende pakketten dat in veroorzaakt door een incompatibel TS-tunnels. |


## <a name="setup"></a>Azure Monitor-waarschuwingen instellen op basis van metrische gegevens met behulp van de portal

De volgende stappen uit het voorbeeld wordt een waarschuwing maken op een gateway voor: <br>

**Metriek:** Tunnel gemiddelde bandbreedte <br>
**Voorwaarde:** bandbreedte > 10 Bytes per seconde <br>
**Venster:** 5 minuten <br>
**Actie bij waarschuwing:** Email <br>



1. Navigeer naar de resource van de virtuele netwerkgateway en 'Waarschuwingen' selecteert in het tabblad bewaking en vervolgens een nieuwe waarschuwingsregel maken of bewerken van een bestaande waarschuwingsregel.

![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert1.png "Create")

2. Selecteer uw VPN-gateway als de resource.

![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert2.png "selecteren")

3. Selecteer een metrische waarde om te configureren voor de waarschuwing ![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert3.png "selecteren")
4. De signaallogica configureren. Er zijn drie onderdelen signaallogica:

    a. Afmetingen: Als de metriek dimensies heeft, kunnen specifieke dimensiewaarden zodat de waarschuwing alleen gegevens van deze dimensie evalueert worden geselecteerd. Dit zijn optioneel.<br>
    b. Voorwaarde: De bewerking voor het evalueren van de metrische waarde.<br>
    c. Tijdstip: Geef de granulatie van de metrische gegevens en de periode voor de evaluatie van de waarschuwing op.<br>

![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert4.png "selecteren")

5. Als u wilt de geconfigureerde regels weergeven, klikt u op 'Waarschuwingsregels beheren' ![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert8.png "selecteren")

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het configureren van waarschuwingen op tunnel diagnoselogboeken [waarschuwingen instellen op VPN-Gateway logboeken met diagnostische gegevens](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md).
