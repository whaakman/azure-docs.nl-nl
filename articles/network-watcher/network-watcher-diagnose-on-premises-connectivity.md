---
title: Spoor On-Premises connectiviteit via VPN-gateway met Azure-netwerk-Watcher | Microsoft Docs
description: In dit artikel wordt beschreven hoe op te sporen lokale connectiviteit via VPN-gateway met Azure-netwerk-Watcher resource probleemoplossing.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: aeffbf3d-fd19-4d61-831d-a7114f7534f9
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: e51d31035a8b05238ef0f8d13dd6b6c3f9ad02e8
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="diagnose-on-premises-connectivity-via-vpn-gateways"></a>Diagnose van lokale connectiviteit via VPN-gateways

Azure VPN-Gateway kunt u hybride oplossing waarmee de noodzaak van een beveiligde verbinding tussen uw on-premises netwerk en uw virtuele Azure-netwerk maken. Als uw vereisten uniek zijn, is de keuze van on-premises VPN-apparaat. Azure ondersteunt momenteel [meerdere VPN-apparaten](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable) die voortdurend worden gevalideerd samen met de Apparaatleveranciers. Bekijk de apparaat-specifieke configuratie-instellingen voordat u uw on-premises VPN-apparaat configureert. Op deze manier Azure VPN-Gateway is geconfigureerd met een reeks [IPSec-parameters ondersteund](../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) die worden gebruikt voor het tot stand brengen van verbindingen. Er is momenteel geen manier voor u opgeven of Selecteer een specifieke combinatie van IPSec-parameters in de Azure VPN-Gateway. Voor het tot stand brengen van een verbinding tussen on-premises en Azure, moet de instellingen van het lokale VPN-apparaat in overeenstemming met de IPSec-parameters voorgeschreven Azure VPN-Gateway. Als de instellingen correct zijn, moet er een verlies van verbinding is en tot op heden het oplossen van deze problemen is niet trivial en meestal duurde uren om te bepalen en corrigeer het probleem.

Met de Azure-netwerk-Watcher oplossen functie, kunnen analyseren van problemen met uw Gateway en verbindingen en binnen enkele minuten zijn onvoldoende gegevens om een gefundeerde beslissing nemen op te lossen van het probleem.

## <a name="scenario"></a>Scenario

U wilt configureren, een site-naar-site-verbinding tussen Azure en on-premises FortiGate gebruiken als de lokale VPN-Gateway. Voor dit scenario, moet u de volgende instellingen:

1. Virtuele netwerkgateway - de VPN-Gateway in Azure
1. Lokale netwerkgateway - de [lokale (FortiGate) VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) weergave in Azure-cloud
1. Site-naar-site-verbinding (route gebaseerd) - [verbinding tussen de VPN-Gateway en de lokale router](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#createconnection)
1. [FortiGate configureren](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/Site-to-Site_VPN_using_FortiGate.md)

Gedetailleerde stapsgewijze instructies voor het configureren van een Site-naar-Site-configuratie kunt u vinden op: [maken van een VNet met een Site-naar-Site-verbinding met de Azure portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

Een van de kritieke configuratiestappen is het configureren van de IPSec-communicatie-parameters, een onjuiste configuratie leidt tot verlies van verbinding tussen de on-premises netwerk en Azure. Azure VPN-Gateways zijn momenteel geconfigureerd voor ondersteuning van de volgende IPsec-parameters voor fase 1. Houd er rekening mee, zoals eerder gezegd dat deze instellingen kunnen niet worden gewijzigd.  Zoals u in de onderstaande tabel ziet, zijn de versleutelingsalgoritmen ondersteund door Azure VPN-Gateway AES256 en AES128 3DES.

### <a name="ike-phase-1-setup"></a>Configuratie IKE fase 1 setup

| **Eigenschap** | **PolicyBased** | **RouteBased en Standard of High-Performance VPN-gateway** |
| --- | --- | --- |
| IKE-versie |IKEv1 |IKEv2 |
| Diffie-Hellman-groep |Groep 2 (1024 bits) |Groep 2 (1024 bits) |
| Verificatiemethode |Vooraf gedeelde sleutel |Vooraf gedeelde sleutel |
| Versleutelingsalgoritmen |AES256 AES128 3DES |AES256 3DES |
| Hash-algoritme |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| Levensduur (tijd) van beveiligingskoppeling (SA) fase 1 |28.800 seconden |10.800 seconden |

Als een gebruiker is vereist voor de configuratie van uw FortiGate, een voorbeeldconfiguratie vindt u op [GitHub](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/fortigate_show%20full-configuration.txt). U hebt geconfigureerd onbewust uw FortiGate voor het gebruik van SHA-512 als hash-algoritme. Als deze algoritme geen ondersteunde algoritme voor verbindingen op basis van beleid is, werkt uw VPN-verbinding.

Deze problemen zijn moeilijk om op te lossen en hoofdoorzaken zijn vaak onduidelijke. In dit geval kunt u een ondersteuningsticket voor informatie over het oplossen van het probleem openen. Maar oplossen met Azure-netwerk-Watcher API, kunt u deze problemen op uw eigen identificeren.

## <a name="troubleshooting-using-azure-network-watcher"></a>Problemen oplossen met behulp van Azure-netwerk-Watcher

Verbinding maken met Azure PowerShell om op te sporen uw verbinding, en start de `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet. U vindt de informatie over het gebruik van deze cmdlet op [virtuele netwerkgateway oplossen en verbindingen - PowerShell](network-watcher-troubleshoot-manage-powershell.md). Deze cmdlet kan enkele minuten duren om te voltooien.

Zodra de cmdlet is voltooid, kunt u navigeren naar de opslaglocatie die is opgegeven in de cmdlet voor gedetailleerde informatie op over het probleem en de logboeken. Azure-netwerk-Watcher maakt een zip-map met de volgende logboekbestanden:

![1][1]

Open het bestand IKEErrors.txt aangeroepen en wordt de volgende fout, die wijzen op een probleem met on-premises onjuiste configuratie IKE-instelling.

```
Error: On-premises device rejected Quick Mode settings. Check values.
     based on log : Peer sent NO_PROPOSAL_CHOSEN notify
```

U kunt gedetailleerde informatie ophalen uit de Scrubbed-wfpdiag.txt over de fout zoals in dit geval er vermeld dat er was `ERROR_IPSEC_IKE_POLICY_MATCH` die ertoe leiden dat connection is niet goed werkt.

Een andere veelvoorkomende is configuratiefout de opgeven onjuist gedeelde sleutels. Als in het vorige voorbeeld u verschillende gedeelde sleutels opgegeven had, wordt de IKEErrors.txt op de volgende fout: `Error: Authentication failed. Check shared key`.

Azure-netwerk-Watcher oplossen functie kunt u vaststellen en oplossen van uw VPN-Gateway en de verbinding met het gemak van een eenvoudig PowerShell-cmdlet. Momenteel ondersteuning voor het onderzoeken van de volgende voorwaarden en we naar meer voorwaarde toe te voegen.

### <a name="gateway"></a>Gateway

| Fouttype | Reden | Logboek|
|---|---|---|
| NoFault | Wanneer er geen fout wordt gedetecteerd. |Ja|
| GatewayNotFound | Kan de Gateway of Gateway niet is ingericht niet vinden. |Nee|
| PlannedMaintenance |  Gateway-instantie is in onderhoud.  |Nee|
| UserDrivenUpdate | Wanneer een gebruiker wordt bijgewerkt. Dit wordt mogelijk een bewerking formaat wijzigen. | Nee |
| VipUnResponsive | Kan het primaire exemplaar van de Gateway niet bereiken. Dit gebeurt wanneer de health-test is mislukt. | Nee |
| PlatformInActive | Er is een probleem met het platform. | Nee|
| ServiceNotRunning | De onderliggende service is niet uitgevoerd. | Nee|
| NoConnectionsFoundForGateway | Er bestaat geen verbindingen op de gateway. Dit is alleen een waarschuwing.| Nee|
| ConnectionsNotConnected | Geen van de verbindingen zijn verbonden. Dit is alleen een waarschuwing.| Ja|
| GatewayCPUUsageExceeded | Het huidige CPU-gebruik van de Gateway-gebruik is > 95%. | Ja |

### <a name="connection"></a>Verbinding

| Fouttype | Reden | Logboek|
|---|---|---|
| NoFault | Wanneer er geen fout wordt gedetecteerd. |Ja|
| GatewayNotFound | Kan de Gateway of Gateway niet is ingericht niet vinden. |Nee|
| PlannedMaintenance | Gateway-instantie is in onderhoud.  |Nee|
| UserDrivenUpdate | Wanneer een gebruiker wordt bijgewerkt. Dit wordt mogelijk een bewerking formaat wijzigen.  | Nee |
| VipUnResponsive | Kan het primaire exemplaar van de Gateway niet bereiken. Fout treedt op wanneer de health-test is mislukt. | Nee |
| ConnectionEntityNotFound | Verbindingsconfiguratie ontbreekt. | Nee |
| ConnectionIsMarkedDisconnected | De verbinding is gemarkeerd als 'verbinding verbroken.' |Nee|
| ConnectionNotConfiguredOnGateway | De onderliggende service heeft niet de verbinding geconfigureerd. | Ja |
| ConnectionMarkedStandy | De onderliggende service is gemarkeerd als stand-by.| Ja|
| Authentication | Vooraf gedeelde sleutel komt niet overeen. | Ja|
| PeerReachability | De peer-gateway is niet bereikbaar. | Ja|
| IkePolicyMismatch | De gateway van de peer heeft IKE-beleidsregels die worden niet ondersteund door Azure. | Ja|
| WfpParse fout | Er is een fout opgetreden bij het parseren van het WPF-logboek. |Ja|

## <a name="next-steps"></a>Volgende stappen

Meer informatie over VPN-Gateway-connectiviteit met PowerShell en Azure Automation controleren in via [Monitor VPN-gateways bij het oplossen van Azure-netwerk-Watcher](network-watcher-monitor-with-azure-automation.md)

[1]: ./media/network-watcher-diagnose-on-premises-connectivity/figure1.png
