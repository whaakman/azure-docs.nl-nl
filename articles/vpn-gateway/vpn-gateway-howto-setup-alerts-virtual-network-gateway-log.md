---
title: Waarschuwingen voor gebeurtenissen van diagnostische logboeken van Azure VPN-Gateway instellen
description: Stappen voor het configureren van waarschuwingen op VPN-Gateway diagnostische gebeurtenissen
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: alzam
ms.openlocfilehash: 4f18581b9ca5770b89be8ca37529c09d635dfb25
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607121"
---
# <a name="set-up-alerts-on-diagnostic-log-events-from-vpn-gateway"></a>Waarschuwingen voor gebeurtenissen-diagnoselogboek instellen van VPN-Gateway

Dit artikel helpt u bij het instellen van waarschuwingen op basis van diagnostische gebeurtenissen van Azure VPN-Gateway met behulp van Azure Log Analytics. 

De volgende logboeken zijn beschikbaar in Azure:

|***Name*** | ***Beschrijving*** |
|---        | ---               |
|GatewayDiagnosticLog | Diagnostische logboeken voor gateway-Configuratiegebeurtenissen, primaire wijzigingen en onderhoud voor bevat |
|TunnelDiagnosticLog | Statuswijzigingsgebeurtenissen tunnel bevat. Tunnel/verbreken gebeurtenissen hebben een samengevatte reden voor de statuswijziging heeft plaatsgevonden, indien van toepassing |
|RouteDiagnosticLog | Logboeken wijzigingen van statische routes en BGP-gebeurtenissen die op de gateway plaatsvinden |
|IKEDiagnosticLog | IKE-besturingselement berichten en gebeurtenissen in de gateway-Logboeken |
|P2SDiagnosticLog | Besturingselement voor punt-naar-site-berichten en gebeurtenissen in de gateway-Logboeken |

## <a name="setup"></a>Waarschuwingen instellen

Het volgende voorbeeld wordt maakt een waarschuwing voor een gebeurtenis verbreken dat betrekking heeft op een site-naar-site VPN-tunnel:


1. Zoek in de Azure-portal **Log Analytics** onder **alle services** en selecteer **Log Analytics-werkruimten**.

   ![Selecties voor Log Analytics-werkruimten wilt](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert0.png "maken")

2. Selecteer **maken** op de **Log Analytics** pagina.

   ![Log Analytics-pagina met de knop maken](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert1.png  "selecteren")

3. Selecteer **nieuw** en vul de gegevens.

   ![Details voor het maken van een Log Analytics-werkruimte](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert2.png  "selecteren")

4. Uw VPN-gateway vinden op de **Monitor** > **diagnostische instellingen** blade.

   ![Selecties voor het vinden van de VPN-gateway in diagnostische instellingen](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert3.png  "selecteren")

5. Als u wilt inschakelen op diagnostische gegevens, dubbelklikt u op de gateway en selecteer vervolgens **diagnostische gegevens inschakelen**.

   ![Selecties voor het inschakelen van diagnostische gegevens](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert4.png  "selecteren")

6. Vul de gegevens en zorg ervoor dat **verzenden naar Log Analytics** en **TunnelDiagnosticLog** zijn geselecteerd. Kies de Log Analytics-werkruimte die u hebt gemaakt in stap 3.

   ![Geselecteerd selectievakje](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert5.png  "selecteren")

7. Ga naar het overzicht voor resource van de virtuele netwerkgateway en selecteer **waarschuwingen** uit de **bewaking** tabblad. Vervolgens een nieuwe waarschuwingsregel maken of bewerken van een bestaande waarschuwingsregel.

   ![Selecties voor het maken van een nieuwe waarschuwingsregel](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "selecteren")

   ![punt-naar-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "selecteren")
8. Selecteer de Log Analytics-werkruimte en de resource.

   ![Selecties voor de werkruimte en resource](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert7.png  "selecteren")

9. Selecteer **zoeken in Logboeken aangepaste** als de signaallogica onder **voorwaarde toevoegen**.

   ![Selecties voor een aangepaste logboekzoekopdracht](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert8.png  "selecteren")

10. Voer in het tekstvak **Zoekquery** te volgende query in. Vervang de waarden in <> waar nodig.

     `AzureDiagnostics |
     where Category  == "TunnelDiagnosticLog" and ResourceId == toupper("<RESOURCEID OF GATEWAY>") and TimeGenerated > ago(5m) and
     remoteIP_s == "<REMOTE IP OF TUNNEL>" and status_s == "Disconnected"`

    De drempelwaarde wordt ingesteld op 0 en selecteer **gedaan**.

    ![Een query invoeren en selecteren van een drempelwaarde](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert9.png  "selecteren")

11. Op de **maken regel** weergeeft, schakelt **nieuw** onder de **ACTIEGROEPEN** sectie. Vul de details en selecteer **OK**.

    ![Details voor een nieuwe actiegroep](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert10.png  "selecteren")

12. Op de **maken regel** pagina, vul de details voor **acties aanpassen** en zorg ervoor dat de juiste naam wordt weergegeven in de **naam van de ACTIEGROEP** sectie. Selecteer **waarschuwingsregel maken** om de regel te maken.

    ![Selecties voor het maken van een regel](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert11.png  "selecteren")

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het configureren van waarschuwingen over metrische gegevens tunnel [instellen van waarschuwingen over metrische gegevens van VPN-Gateway](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).
