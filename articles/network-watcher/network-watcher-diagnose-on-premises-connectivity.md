---
title: Diagnose On-Premises connectiviteit via VPN-gateway met Azure Network Watcher | Microsoft Docs
description: Dit artikel wordt beschreven hoe u on-premises connectiviteit via VPN-gateway bij het oplossen van Azure Network Watcher-resource vaststelt.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: aeffbf3d-fd19-4d61-831d-a7114f7534f9
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: f5c4f8d2c9cec4372ef5de70485d45ab33e022de
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55099393"
---
# <a name="diagnose-on-premises-connectivity-via-vpn-gateways"></a>Diagnose van on-premises connectiviteit via VPN-gateways

Met Azure VPN Gateway kunt u hybride oplossing waarmee de noodzaak van een beveiligde verbinding tussen uw on-premises netwerk en uw Azure-netwerk maken. Als uw vereisten uniek zijn, zodat u de keuze van de on-premises VPN-apparaat is. Azure ondersteunt momenteel [verschillende VPN-apparaten](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable) die voortdurend worden gevalideerd in samenwerking met de Apparaatleveranciers. Bekijk de apparaat-specifieke configuratie-instellingen voordat u uw on-premises VPN-apparaat configureert. Op deze manier Azure VPN-Gateway is geconfigureerd met een set [IPsec-parameters ondersteund](../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) die worden gebruikt voor het tot stand brengen van verbindingen. Er is momenteel geen enkele manier waarop u kunt opgeven of Selecteer een specifieke combinatie van IPsec-parameters in de Azure VPN-Gateway. Voor een geslaagde verbinding maken tussen on-premises en Azure, moet on-premises VPN-apparaatinstellingen in overeenstemming met de IPsec-parameters voorgeschreven door Azure VPN-Gateway. Als de instellingen correct zijn, moet er een verlies van verbinding is en tot op heden deze problemen oplossen van problemen is niet eenvoudig en meestal duurde het uren om te bepalen en corrigeer het probleem.

Problemen met de Azure Network Watcher functie, kunt u problemen ondervindt met uw Gateway en verbindingen vaststellen en binnen enkele minuten er zijn onvoldoende gegevens naar een gefundeerde beslissing nemen om rectificatie van het probleem.

## <a name="scenario"></a>Scenario

U wilt een site-naar-site-verbinding tussen Azure en on-premises configureren met behulp van FortiGate als de on-premises VPN-Gateway. Voor het bereiken van dit scenario, moet u de volgende instellingen:

1. Virtuele netwerkgateway - de VPN-Gateway op Azure
1. Lokale netwerkgateway - de [on-premises VPN-Gateway (FortiGate)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) weergeven in Azure-cloud
1. Site-naar-site-verbinding (op basis van route) - [verbinding tussen de VPN-Gateway en de on-premises router](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#createconnection)
1. [FortiGate configureren](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/Site-to-Site_VPN_using_FortiGate.md)

Gedetailleerde stapsgewijze instructies voor het configureren van een Site-naar-Site-configuratie kan worden gevonden door naar de pagina: [Een VNet maken met een Site-naar-Site-verbinding met de Azure-portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

Een van de kritieke configuratiestappen configureert u de communicatie IPSec-parameters, een onjuiste configuratie leidt tot verlies van verbinding tussen de on-premises netwerk en Azure. Azure VPN-Gateways zijn momenteel geconfigureerd ter ondersteuning van de volgende IPsec-parameters voor fase 1. Houd er rekening mee, zoals eerder vermeld dat deze instellingen kunnen niet worden gewijzigd.  Zoals u in de onderstaande tabel zien kunt, zijn de ondersteund door Azure VPN-Gateway versleutelingsalgoritmen AES256, AES128 en 3DES.

### <a name="ike-phase-1-setup"></a>IKE phase 1 instellen

| **Eigenschap** | **PolicyBased** | **RouteBased en standaard of High-Performance VPN-gateway** |
| --- | --- | --- |
| IKE-versie |IKEv1 |IKEv2 |
| Diffie-Hellman-groep |Groep 2 (1024 bits) |Groep 2 (1024 bits) |
| Verificatiemethode |Vooraf gedeelde sleutel |Vooraf gedeelde sleutel |
| Versleutelingsalgoritmen |AES256 AES128 3DES |AES256 3DES |
| Hash-algoritme |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| Levensduur (tijd) van beveiligingskoppeling (SA) fase 1 |28.800 seconden |10.800 seconden |

Als een gebruiker is vereist voor het configureren van uw FortiGate, een voorbeeldconfiguratie kan worden gevonden op [GitHub](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/fortigate_show%20full-configuration.txt). U hebt geconfigureerd onbewust uw FortiGate voor het gebruik van SHA-512 als hash-algoritme. Als dit algoritme niet een ondersteunde algoritme voor verbindingen op basis van beleid is, worden uw VPN-verbinding werkt.

Deze problemen zijn moeilijk is om op te lossen en belangrijkste oorzaken zijn vaak niet-intuïtieve. In dit geval kunt u een ondersteuningsticket voor hulp bij het oplossen van het probleem openen. Maar met Azure Network Watcher oplossen API, kunt u deze problemen op uw eigen identificeren.

## <a name="troubleshooting-using-azure-network-watcher"></a>Problemen oplossen met behulp van Azure Network Watcher

Verbinding maken met Azure PowerShell voor het bepalen van uw verbinding en start de `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet. Lees de informatie over het gebruik van deze cmdlet op [virtuele netwerkgateway oplossen en verbindingen - PowerShell](network-watcher-troubleshoot-manage-powershell.md). Deze cmdlet kan enkele minuten duren om uit te voeren.

Nadat de cmdlet is voltooid, kunt u navigeren naar de opslaglocatie die is opgegeven in de cmdlet voor gedetailleerde informatie op over het probleem en Logboeken. Azure Network Watcher maakt u een zip-map met de volgende logboekbestanden:

![1][1]

Open het bestand met de naam IKEErrors.txt en wordt de pagina de volgende fout, die wijzen op een probleem met on-premises onjuiste configuratie IKE-instelling.

```
Error: On-premises device rejected Quick Mode settings. Check values.
     based on log : Peer sent NO_PROPOSAL_CHOSEN notify
```

U kunt gedetailleerde informatie opvragen uit de Scrubbed-wfpdiag.txt over de fout, zoals in dit geval wordt deze vermeld dat er `ERROR_IPSEC_IKE_POLICY_MATCH` die leiden tot een verbinding niet goed werkt.

Een andere veelvoorkomende is configuratiefout de opgeven onjuist gedeelde sleutels. Als u in het vorige voorbeeld had u verschillende gedeelde sleutels opgeeft, de IKEErrors.txt ziet u de volgende fout: `Error: Authentication failed. Check shared key`.

Azure Network Watcher oplossen functie kunt u problemen vaststellen en oplossen van uw VPN-Gateway en de verbinding met het gemak van een eenvoudige PowerShell-cmdlet. Op dit moment we ondersteuning voor het vaststellen van de volgende voorwaarden en worden gewerkt aan het toevoegen van meer voorwaarde.

### <a name="gateway"></a>Gateway

| Type probleem | Reden | Logboek|
|---|---|---|
| NoFault | Als er geen fout is gedetecteerd. |Ja|
| GatewayNotFound | Kan de Gateway of Gateway nog niet is ingericht niet vinden. |Nee|
| PlannedMaintenance |  Toepassingsgateway-exemplaar is in onderhoud.  |Nee|
| UserDrivenUpdate | Wanneer een update wordt uitgevoerd. Dit wordt mogelijk een bewerking formaat wijzigen. | Nee |
| VipUnResponsive | Kan het primaire exemplaar van de Gateway niet bereiken. Dit gebeurt wanneer de statustest is mislukt. | Nee |
| PlatformInActive | Er is een probleem met het platform. | Nee|
| ServiceNotRunning | De onderliggende service wordt niet uitgevoerd. | Nee|
| NoConnectionsFoundForGateway | Er bestaat geen verbindingen op de gateway. Dit is alleen een waarschuwing.| Nee|
| ConnectionsNotConnected | Geen van de verbindingen zijn verbonden. Dit is alleen een waarschuwing.| Ja|
| GatewayCPUUsageExceeded | Het huidige CPU-gebruik van de Gateway-gebruik is > 95%. | Ja |

### <a name="connection"></a>Verbinding

| Type probleem | Reden | Logboek|
|---|---|---|
| NoFault | Als er geen fout is gedetecteerd. |Ja|
| GatewayNotFound | Kan de Gateway of Gateway nog niet is ingericht niet vinden. |Nee|
| PlannedMaintenance | Toepassingsgateway-exemplaar is in onderhoud.  |Nee|
| UserDrivenUpdate | Wanneer een update wordt uitgevoerd. Dit wordt mogelijk een bewerking formaat wijzigen.  | Nee |
| VipUnResponsive | Kan het primaire exemplaar van de Gateway niet bereiken. Dit gebeurt wanneer de statustest mislukt. | Nee |
| ConnectionEntityNotFound | Verbindingsconfiguratie ontbreekt. | Nee |
| ConnectionIsMarkedDisconnected | De verbinding is gemarkeerd als "niet-verbonden." |Nee|
| ConnectionNotConfiguredOnGateway | De onderliggende service beschikt niet over de verbinding is geconfigureerd. | Ja |
| ConnectionMarkedStandby | De onderliggende service is gemarkeerd als stand-by.| Ja|
| Verificatie | Vooraf gedeelde sleutel komt niet overeen. | Ja|
| PeerReachability | De peer-gateway is niet bereikbaar. | Ja|
| IkePolicyMismatch | De peer-gateway heeft IKE-beleid die niet worden ondersteund door Azure. | Ja|
| WfpParse fout | Er is een fout opgetreden bij het parseren van het logboek WPF. |Ja|

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het VPN-gatewayverbinding met PowerShell en Azure Automation controleren door naar de pagina [Monitor VPN-gateways bij het oplossen van Azure Network Watcher](network-watcher-monitor-with-azure-automation.md)

[1]: ./media/network-watcher-diagnose-on-premises-connectivity/figure1.png
