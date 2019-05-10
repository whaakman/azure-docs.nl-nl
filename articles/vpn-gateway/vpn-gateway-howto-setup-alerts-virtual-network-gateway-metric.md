---
title: Waarschuwingen over metrische gegevens voor Azure VPN-Gateway instellen
description: Stappen voor het configureren van waarschuwingen over metrische gegevens van VPN-Gateway
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: e54dadbda0582095e8152ea30376d369177bfd86
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65509895"
---
# <a name="set-up-alerts-on-vpn-gateway-metrics"></a>Waarschuwingen over metrische gegevens van VPN-Gateway instellen

Dit artikel helpt u bij het instellen van waarschuwingen over metrische gegevens voor Azure VPN-Gateway. Azure Monitor biedt de mogelijkheid voor het instellen van waarschuwingen voor Azure-resources. U kunt waarschuwingen voor virtuele netwerkgateways van het type 'VPN' instellen.


|**Gegevens**   | **Eenheid** | **Granulariteit** | **Beschrijving** | 
|---       | ---        | ---       | ---            | ---       |
|**AverageBandwidth**| Bytes/s  | 5 minuten| Gemiddelde gebruik van de gecombineerde bandbreedte van alle site-naar-site-verbindingen op de gateway.     |
|**P2SBandwidth**| Bytes/s  | 1 minuut  | Gemiddelde gebruik van de gecombineerde bandbreedte van alle punt-naar-site-verbindingen op de gateway.    |
|**P2SConnectionCount**| Count  | 1 minuut  | Telling van punt-naar-site-verbindingen op de gateway.   |
|**TunnelAverageBandwidth** | Bytes/s    | 5 minuten  | Gemiddelde bandbreedtegebruik van tunnels die zijn gemaakt op de gateway. |
|**TunnelEgressBytes** | Bytes | 5 minuten | Uitgaand verkeer in die zijn gemaakt op de gateway-tunnels.   |
|**TunnelEgressPackets** | Count | 5 minuten | Het aantal uitgaande pakketten op tunnels die zijn gemaakt op de gateway.   |
|**TunnelEgressPacketDropTSMismatch** | Count | 5 minuten | Het aantal uitgaande pakketten verwijderd op tunnels veroorzaakt door verkeersselector probleem. |
|**TunnelIngressBytes** | Bytes | 5 minuten | Verkeer dat binnenkomt op tunnels die zijn gemaakt op de gateway.   |
|**TunnelIngressPackets** | Count | 5 minuten | Het aantal inkomende pakketten in die zijn gemaakt op de gateway-tunnels.   |
|**TunnelIngressPacketDropTSMismatch** | Count | 5 minuten | Het aantal inkomende pakketten dat in veroorzaakt door verkeersselector verschil-tunnels. |


## <a name="setup"></a>Instellen van Azure Monitor-waarschuwingen op basis van metrische gegevens met behulp van Azure portal

Het volgende voorbeeld wordt maakt een waarschuwing op een gateway voor:

- **Metriek:** TunnelAverageBandwidth
- **Voorwaarde:** Bandbreedte > 10 bytes per seconde
- **Venster:** 5 minuten
- **Actie bij waarschuwing:** E-mailen



1. Ga naar de resource van de virtuele netwerkgateway en selecteer **waarschuwingen** uit de **bewaking** tabblad. Vervolgens een nieuwe waarschuwingsregel maken of bewerken van een bestaande waarschuwingsregel.

   ![Selecties voor het maken van een waarschuwingsregel](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert1.png "maken")

2. Selecteer uw VPN-gateway als de resource.

   ![De knop selecteren en de VPN-gateway in de lijst met resources](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert2.png "selecteren")

3. Selecteer een metrische waarde voor het configureren van de waarschuwing.

   ![Metrische gegevens in de lijst met metrische gegevens geselecteerd](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert3.png "selecteren")
4. De signaallogica configureren. Er zijn drie onderdelen:

    a. **Dimensies**: Als de metriek dimensies heeft, kunt u specifieke dimensiewaarden selecteren zodat de waarschuwing alleen gegevens van deze dimensie evalueert. Dit zijn optioneel.

    b. **Voorwaarde**: Dit is de bewerking voor het evalueren van de metrische waarde.

    c. **Tijd**: Geef de granulatie van de metrische gegevens en de periode voor de evaluatie van de waarschuwing.

   ![Details voor het configureren van logische signaal](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert4.png "selecteren")

5. Als u wilt de geconfigureerde regels weergeven, selecteert u **Waarschuwingsregels beheren**.

   ![Knop voor het beheren van waarschuwingsregels](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert8.png "selecteren")

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het configureren van waarschuwingen op tunnel diagnostische logboeken [instellen van waarschuwingen voor VPN-Gateway logboeken met diagnostische gegevens](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md).
