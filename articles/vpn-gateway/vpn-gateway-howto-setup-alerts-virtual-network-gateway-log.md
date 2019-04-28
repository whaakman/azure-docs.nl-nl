---
title: Over het instellen van waarschuwingen voor Azure VPN-Gateway diagnostisch logboekgebeurtenissen
description: Stappen voor het configureren van waarschuwingen op VPN-Gateway diagnostische gebeurtenissen
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: 39a6d2e6201dd0635149159cb3727fd3c40f710a
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63769734"
---
# <a name="setting-up-alerts-on-vpn-gateway-diagnostic-log-events"></a>Instellen van waarschuwingen voor VPN-Gateway diagnostisch logboekgebeurtenissen

Dit artikel helpt u bij het instellen van waarschuwingen op basis van VPN-Gateway diagnostische gebeurtenissen.


## <a name="setup"></a>Azure Monitor-waarschuwingen instellen op basis van gebeurtenissen van diagnostische logboeken met behulp van de portal

De volgende stappen uit het voorbeeld maakt u een waarschuwing voor een site-naar-site VPN-tunnel verbreken gebeurtenis



1. Zoek naar "Log Analytics" onder alle services en kies "Log Analytics-werkruimten" ![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert0.png "maken")

2. Klik op 'Maken' in de Log Analytics-pagina.
![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert1.png  "selecteren")

3. Controleer 'Nieuw' werkruimte en vul de gegevens.
![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert2.png  "selecteren")

4. Vinden van uw VPN-gateway onder het 'controleren' > 'Diagnostische instellingen' blade ![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert3.png  "selecteren")

5. Als u wilt inschakelen op diagnostische gegevens, dubbelklik op de gateway en klik vervolgens op 'Voor diagnostische gegevens inschakelen' ![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert4.png  "selecteren")

6. Vul de gegevens en zorg ervoor dat "Verzenden naar Log Analytics" en "TunnelDiagnosticLog" worden gecontroleerd. Kies de Log Analytics-werkruimte die is gemaakt in stap 3.
![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert5.png  "selecteren")

7. Navigeer naar de resource-overzicht van virtual network gateway en 'Waarschuwingen' selecteert in het tabblad bewaking en vervolgens een nieuwe waarschuwingsregel maken of bewerken van een bestaande waarschuwingsregel.
![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "selecteren")

8. Selecteer de Log Analytics-werkruimte en de resource.
![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert7.png  "selecteren")

9. Selecteer "aangepast zoeken in Logboeken" Als de signaallogica onder 'Voorwaarde toevoegen' ![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert8.png  "selecteren")

10. Voer de volgende query in het tekstvak "Zoekquery" Vervang de waarden in <> waar nodig.

    AzureDiagnostics | waar categorie == "TunnelDiagnosticLog" en ResourceId == toupper ("<RESOURCEID OF GATEWAY>') en TimeGenerated > ago(5m) en remoteIP_s =="<REMOTE IP OF TUNNEL>"en status_s =="Niet-verbonden"

    De waarde voor drempel ingesteld op 0 en klikt u op 'Gereed'

    ![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert9.png  "selecteren")

11. Klik op 'Nieuw' in de sectie ACTIEGROEPEN op de pagina "Regel maken". Vul de details en klik op OK

![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert10.png  "selecteren")

12. Vul de details voor 'Action aanpassen' en zorg ervoor dat de naam van de juiste actie wordt weergegeven in de sectie 'Naam van actiegroep' op de pagina "Regel maken". Klik op 'Maken van waarschuwingsregel' om de regel te maken.
![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert11.png  "selecteren")

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het configureren van waarschuwingen over metrische gegevens tunnel [waarschuwingen over metrische gegevens van VPN-Gateway instellen](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).
