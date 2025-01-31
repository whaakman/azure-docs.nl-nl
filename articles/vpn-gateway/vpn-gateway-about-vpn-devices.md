---
title: Over VPN-apparaten voor cross-premises Azure-verbindingen | Microsoft Docs
description: Dit artikel gaat over VPN-apparaten en IPSec-parameters voor cross-premises site-naar-site-VPN-gateway-verbindingen. Het artikel bevat koppelingen naar configuratie-instructies en voorbeelden.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 07/05/2019
ms.author: yushwang
ms.openlocfilehash: 8301594f63efaa5c6484a4dfd640aafa96cf15a0
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67666259"
---
# <a name="about-vpn-devices-and-ipsecike-parameters-for-site-to-site-vpn-gateway-connections"></a>VPN-apparaten en IPSec-/IKE-parameters voor site-naar-site-VPN-gateway-verbindingen

U hebt een VPN-apparaat nodig om een cross-premises site-naar-site-VPN-verbinding te configureren. Site-naar-site-verbindingen kunnen worden gebruikt om een hybride oplossing te maken of wanneer u beveiligde verbindingen wilt maken tussen uw on-premises netwerken en virtuele netwerken. Dit artikel bevat een lijst met gevalideerde VPN-apparaten en een lijst met IPSec-/IKE-parameters voor VPN-gateways.

> [!IMPORTANT]
> Raadpleeg [Bekende compatibiliteitsproblemen](#known) als u problemen ondervindt met de connectiviteit tussen uw lokale VPN-apparaten en VPN-gateways.
>

### <a name="items-to-note-when-viewing-the-tables"></a>Waar u op moet letten wanneer u de tabellen bekijkt:

* Er is een terminologiewijziging voor Azure VPN-gateways. Alleen de namen zijn gewijzigd. Er is geen wijziging in de functionaliteit.
  * Statische routering = PolicyBased
  * Dynamische routering = RouteBased
* De specificaties voor een HighPerformance-VPN-gateway en een RouteBased VPN-gateway zijn hetzelfde, tenzij anders wordt vermeld. Zo zijn de gevalideerde VPN-apparaten die compatibel zijn met RouteBased VPN-gateways, ook compatibel met de HighPerformance VPN-gateway.

## <a name="devicetable"></a>Gevalideerde VPN-apparaten en apparaatconfiguratiehandleidingen

We hebben samen met apparaatleveranciers een reeks standaard VPN-apparaten gevalideerd. Alle apparaten in de apparaatfamilies in de volgende lijst kunnen met VPN-gateways worden gebruikt. Zie [Over VPN-gatewayinstellingen](vpn-gateway-about-vpn-gateway-settings.md#vpntype) voor informatie over welk VPN-type u moet gebruiken (PolicyBased of RouteBased) voor de VPN-gatewayoplossing die u wilt configureren.

Om te helpen uw VPN-apparaat configureren, raadpleegt u de koppelingen die overeenkomen met de betreffende apparaatstuurprogrammafamilie. De koppelingen naar configuratie-instructies worden naar beste vermogen geleverd. Voor ondersteuning van VPN-apparaten neemt u contact op met de fabrikant van uw apparaat.

|**Leverancier**          |**Apparaatfamilie**     |**Minimale versie van het besturingssysteem** |**PolicyBased configuratie-instructies** |**RouteBased configuratie-instructies** |
| ---                | ---                  | ---                   | ---            | ---           |
| A10 Networks, Inc. |Thunder CFW           |ACOS 4.1.1             |Niet compatibel  |[Configuratiehandleiding](https://www.a10networks.com/resources/deployment-guides/a10-thunder-cfw-ipsec-vpn-interoperability-azure-vpn-gateways)|
| Allied Telesis     |VPN-routers uit AR-serie |5\.4.7+ AR-serie               |Binnenkort beschikbaar     |[Configuratiehandleiding](https://www.alliedtelesis.com/documents/how-to/configure/site-to-site-vpn-between-azure-and-ar-series-router)|
| Barracuda Networks, Inc. |Barracuda NextGen Firewall F-serie |PolicyBased: 5.4.3<br>Op route gebaseerd: 6.2.0 |[Configuratiehandleiding](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) |[Configuratiehandleiding](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall X-serie |Barracuda Firewall 6.5 |[Configuratiehandleiding](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) |Niet compatibel |
| Check Point |Security Gateway |R80.10 |[Configuratiehandleiding](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |[Configuratiehandleiding](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco              |ASA       |8.3<br>8.4+ (IKEv2*) |Ondersteund |[Configuratiehandleiding*](https://www.cisco.com/c/en/us/support/docs/security/adaptive-security-appliance-asa-software/214109-configure-asa-ipsec-vti-connection-to-az.html) |
| Cisco |ASR |PolicyBased: IOS 15.1<br>Op route gebaseerd: IOS 15.2 |Ondersteund |Ondersteund |
| Cisco | CSR | Op route gebaseerd: IOS-XE 16.10 | | [Het script voor configuratie](vpn-gateway-download-vpndevicescript.md) |
| Cisco |ISR |PolicyBased: IOS 15.0<br>Op route gebaseerd *: IOS 15.1 |Ondersteund |Ondersteund |
| Cisco |Meraki |N/A |Niet compatibel |Niet compatibel |
| Citrix |NetScaler MPX, SDX, VPX |10.1 en hoger |[Configuratiehandleiding](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) |Niet compatibel |
| F5 |BIG-IP-serie |12.0 |[Configuratiehandleiding](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) |[Configuratiehandleiding](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling) |
| Fortinet |FortiGate |FortiOS 5.6 |  |[Configuratiehandleiding](https://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-56/) |
| Internet Initiative Japan (IIJ) |SEIL-serie |SEIL/X 4.60<br>SEIL/B1 4.60<br>SEIL/x86 3.20 |[Configuratiehandleiding](https://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) |Niet compatibel |
| Juniper |SRX |PolicyBased: JunOS 10.2<br>Op route gebaseerd: JunOS 11.4 |Ondersteund |[Het script voor configuratie](vpn-gateway-download-vpndevicescript.md) |
| Juniper |J-serie |PolicyBased: JunOS 10.4r9<br>Op route gebaseerd: JunOS 11.4 |Ondersteund |[Het script voor configuratie](vpn-gateway-download-vpndevicescript.md) |
| Juniper |ISG |ScreenOS 6.3 |Ondersteund |[Het script voor configuratie](vpn-gateway-download-vpndevicescript.md) |
| Juniper |SSG |ScreenOS 6.2 |Ondersteund |[Het script voor configuratie](vpn-gateway-download-vpndevicescript.md) |
| Juniper |MX |JunOS 12.x|Ondersteund |[Het script voor configuratie](vpn-gateway-download-vpndevicescript.md) |
| Microsoft |Routering en Remote Access-Service |Windows Server 2012 |Niet compatibel |Ondersteund |
| Open Systems AG |Mission Control Security Gateway |N/A |[Configuratiehandleiding](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |Niet compatibel |
| Palo Alto Networks |Alle apparaten waarop PAN-OS wordt uitgevoerd |PAN-OS<br>PolicyBased: 6.1.5 of hoger<br>Op route gebaseerd: 7.1.4 |[Configuratiehandleiding](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) |[Configuratiehandleiding](https://knowledgebase.paloaltonetworks.com/KCSArticleDetail?id=kA10g000000Cm6WCAS) |
| ShareTech | Next Generation UTM (NU-serie) | 9.0.1.3 | Niet compatibel | [Configuratiehandleiding](http://www.sharetech.com.tw/images/file/Solution/NU_UTM/S2S_VPN_with_Azure_Route_Based_en.pdf) |
| SonicWall |TZ-serie, NSA-serie<br>SuperMassive-serie<br>E-Class NSA-serie |SonicOS 5.8.x<br>SonicOS 5.9.x<br>SonicOS 6.x |Niet compatibel |[Configuratiehandleiding](https://www.sonicwall.com/support/knowledge-base/170505320011694) |
| Sophos | XG Firewall van de volgende generatie | XG v17 | | [Configuratiehandleiding](https://community.sophos.com/kb/127546)<br><br>[Handleiding voor de configuratie - meerdere SAs](https://community.sophos.com/kb/en-us/133154) |
| Synology | MR2200ac <br>RT2600ac <br>RT1900ac | SRM1.1.5/VpnPlusServer-1.2.0 |  | [Configuratiehandleiding](https://www.synology.com/en-global/knowledgebase/SRM/tutorial/VPN/How_to_set_up_Site_to_Site_VPN_between_Synology_Router_and_MS_Azure) |
| Ubiquiti | EdgeRouter | EdgeOS v1.10 |  | [BGP via IKEv2/IPsec](https://help.ubnt.com/hc/en-us/articles/115012374708)<br><br>[VTI via IKEv2/IPsec](https://help.ubnt.com/hc/en-us/articles/115012305347)
| WatchGuard |Alle |Fireware XTM<br> PolicyBased: v11.11.x<br>RouteBased: v11.12.x |[Configuratiehandleiding](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA2F00000000LI7KAM&lang=en_US) |[Configuratiehandleiding](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA22A000000XZogSAG&lang=en_US)|
| Zyxel |Reeks ZyWALL USG<br>ZyWALL ATP-serie<br>ZyWALL VPN series | ZLD v4.32 + | | [VTI via IKEv2/IPsec](https://businessforum.zyxel.com/discussion/2648/)<br>[BGP via IKEv2/IPsec](https://businessforum.zyxel.com/discussion/2650/)|

> [!NOTE]
>
> (*) Met Cisco ASA versie 8.4 en hoger wordt IKEv2-ondersteuning toegevoegd en kan verbinding worden gemaakt met Azure VPN-gateways met behulp van een aangepast IPsec/IKE-beleid met de optie UsePolicyBasedTrafficSelectors. Raadpleeg dit [artikel met instructies](vpn-gateway-connect-multiple-policybased-rm-ps.md).
>
> (\*\*) Routers uit de ISR 7200-serie bieden alleen ondersteuning voor PolicyBased VPN's.

## <a name="configscripts"></a>Configuratiescripts voor VPN-apparaten downloaden van Azure

Voor bepaalde apparaten, kunt u downloaden configuratiescripts rechtstreeks vanuit Azure. Zie voor meer informatie en instructies [configuratiescripts voor VPN downloaden apparaat](vpn-gateway-download-vpndevicescript.md).

### <a name="devices-with-available-configuration-scripts"></a>Apparaten met een configuratie met scripts

[!INCLUDE [scripts](../../includes/vpn-gateway-device-configuration-scripts.md)]

## <a name="additionaldevices"></a>Niet-gevalideerde VPN-apparaten

Als uw apparaat niet in de tabel met gevalideerde VPN-apparaten wordt vermeld, werkt het misschien toch met een site-naar-site-verbinding. Neem contact op met de fabrikant van uw apparaat voor aanvullende ondersteuning en configuratie-instructies.

## <a name="editing"></a>Voorbeelden van het bewerken van apparaatconfiguraties

Nadat u het bij het VPN-apparaat meegeleverde configuratievoorbeeld hebt gedownload, moet u enkele waarden veranderen zodat ze overeenkomen met de instellingen voor uw omgeving.

### <a name="to-edit-a-sample"></a>U bewerkt een voorbeeld als volgt:

1. Open het voorbeeld met Kladblok.
2. Zoek alle <*tekst*>-tekenreeksen en vervang ze door de waarden die betrekking hebben op uw omgeving. Zorg dat u < en > opneemt. Als u een naam opgeeft, moet deze uniek zijn. Als een opdracht niet werkt, raadpleeg dan de documentatie van de fabrikant van uw apparaat.

| **Voorbeeldtekst** | **Wijzig in** |
| --- | --- |
| &lt;RP_OnPremisesNetwork&gt; |De naam die u voor dit object hebt gekozen. Voorbeeld: myOnPremisesNetwork |
| &lt;RP_AzureNetwork&gt; |De naam die u voor dit object hebt gekozen. Voorbeeld: myAzureNetwork |
| &lt;RP_AccessList&gt; |De naam die u voor dit object hebt gekozen. Voorbeeld: myAzureAccessList |
| &lt;RP_IPSecTransformSet&gt; |De naam die u voor dit object hebt gekozen. Voorbeeld: myIPSecTransformSet |
| &lt;RP_IPSecCryptoMap&gt; |De naam die u voor dit object hebt gekozen. Voorbeeld: myIPSecCryptoMap |
| &lt;SP_AzureNetworkIpRange&gt; |Geef het bereik op. Voorbeeld: 192.168.0.0 |
| &lt;SP_AzureNetworkSubnetMask&gt; |Geef het subnetmasker op. Voorbeeld: 255.255.0.0 |
| &lt;SP_OnPremisesNetworkIpRange&gt; |Geef het on-premises bereik op. Voorbeeld: 10.2.1.0 |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; |Geef het on-premises subnetmasker op. Voorbeeld: 255.255.255.0 |
| &lt;SP_AzureGatewayIpAddress&gt; |Deze informatie is specifiek voor uw virtuele netwerk en u vindt deze in de beheerportal als **IP-adres van gateway**. |
| &lt;SP_PresharedKey&gt; |Deze informatie is specifiek voor uw virtuele netwerk en u vindt deze in de beheerportal onder Sleutel beheren. |

## <a name="ipsec"></a>IPSec-/IKE-parameters

> [!IMPORTANT]
> 1. De onderstaande tabellen bevatten de combinaties van algoritmen en parameters die in de standaardconfiguratie voor Azure VPN-gateways worden gebruikt. Voor op routes gebaseerde VPN-gateways die zijn gemaakt met het Azure Resource Management-implementatiemodel, kunt u voor elke afzonderlijke verbinding een aangepast beleid opgeven. Raadpleeg [Configure IPsec/IKE policy](vpn-gateway-ipsecikepolicy-rm-powershell.md) (IPsec/IKE-beleid configureren) voor gedetailleerde instructies.
>
> 2. Bovendien moet u TCP **MSS** vastzetten op **1350**. Als uw VPN-apparaten het vastzetten van MSS niet ondersteunen, kunt u in plaats daarvan ook de **MTU** op de tunnelinterface **1400** instellen op bytes.
>

In de volgende tabellen:

* SA = Security Association
* IKE Phase 1 wordt ook 'Main Mode' genoemd
* IKE Phase 2 wordt ook 'Quick Mode' genoemd

### <a name="ike-phase-1-main-mode-parameters"></a>Parameters voor IKE Phase 1 (Main Mode)

| **Eigenschap**          |**PolicyBased**    | **RouteBased**    |
| ---                   | ---               | ---               |
| IKE-versie           |IKEv1              |IKEv2              |
| Diffie-Hellman-groep  |Groep 2 (1024 bits) |Groep 2 (1024 bits) |
| Verificatiemethode |Vooraf gedeelde sleutel     |Vooraf gedeelde sleutel     |
| Versleutelings- en hash-algoritmen |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |1. AES256, SHA1<br>2. AES256, SHA256<br>3. AES128, SHA1<br>4. AES128, SHA256<br>5. 3DES, SHA1<br>6. 3DES, SHA256 |
| SA-levensduur           |28.800 seconden     |28.800 seconden     |

### <a name="ike-phase-2-quick-mode-parameters"></a>Parameters voor IKE Phase 2 (Quick Mode)

| **Eigenschap**                  |**PolicyBased**| **RouteBased**                              |
| ---                           | ---           | ---                                         |
| IKE-versie                   |IKEv1          |IKEv2                                        |
| Versleutelings- en hash-algoritmen |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |[RouteBased QM SA-aanbiedingen](#RouteBasedOffers) |
| SA-levensduur (tijd)            |3\.600 seconden  |27.000 seconden                                |
| SA-levensduur (bytes)           |102.400.000 kB | -                                           |
| Perfect Forward Secrecy (PFS) |Nee             |[RouteBased QM SA-aanbiedingen](#RouteBasedOffers) |
| Dead Peer Detection (DPD)     |Niet ondersteund  |Ondersteund                                    |


### <a name ="RouteBasedOffers"></a>Aanbiedingen RouteBased VPN IPsec Security Association (IKE Quick Mode SA)

De volgende tabel bevat aanbiedingen van IPSec-SA (IKE Quick Mode). De aanbiedingen staan in volgorde van voorkeur waarin de aanbieding is gepresenteerd of geaccepteerd.

#### <a name="azure-gateway-as-initiator"></a>Azure-gateway als initiator

|-  |**Versleuteling**|**Verificatie**|**PFS-groep**|
|---| ---          |---               |---          |
| 1 |GCM AES256    |GCM (AES256)      |Geen         |
| 2 |AES256        |SHA1              |Geen         |
| 3 |3DES          |SHA1              |Geen         |
| 4 |AES256        |SHA256            |Geen         |
| 5 |AES128        |SHA1              |Geen         |
| 6 |3DES          |SHA256            |Geen         |

#### <a name="azure-gateway-as-responder"></a>Azure-Gateway als antwoorder

|-  |**Versleuteling**|**Verificatie**|**PFS-groep**|
|---| ---          | ---              |---          |
| 1 |GCM AES256    |GCM (AES256)      |Geen         |
| 2 |AES256        |SHA1              |Geen         |
| 3 |3DES          |SHA1              |Geen         |
| 4 |AES256        |SHA256            |Geen         |
| 5 |AES128        |SHA1              |Geen         |
| 6 |3DES          |SHA256            |Geen         |
| 7 |DES           |SHA1              |Geen         |
| 8 |AES256        |SHA1              |1            |
| 9 |AES256        |SHA1              |2            |
| 10|AES256        |SHA1              |14           |
| 11|AES128        |SHA1              |1            |
| 12|AES128        |SHA1              |2            |
| 13|AES128        |SHA1              |14           |
| 14|3DES          |SHA1              |1            |
| 15|3DES          |SHA1              |2            |
| 16|3DES          |SHA256            |2            |
| 17|AES256        |SHA256            |1            |
| 18|AES256        |SHA256            |2            |
| 19|AES256        |SHA256            |14           |
| 20|AES256        |SHA1              |24           |
| 21|AES256        |SHA256            |24           |
| 22|AES128        |SHA256            |Geen         |
| 23|AES128        |SHA256            |1            |
| 24|AES128        |SHA256            |2            |
| 25|AES128        |SHA256            |14           |
| 26|3DES          |SHA1              |14           |

* U kunt IPsec ESP NULL-versleuteling opgeven met RouteBased VPN-gateways en HighPerformance VPN-gateways. Op null gebaseerde versleuteling biedt geen beveiliging voor gegevens tijdens de overdracht. Dit mag alleen worden gebruikt wanneer maximale doorvoer en minimale latentie zijn vereist. Clients kunnen ervoor kiezen dit te gebruiken voor communicatie tussen VNET's of wanneer elders in de oplossing versleuteling wordt toegepast.
* Gebruik voor cross-premises connectiviteit via internet de standaardinstellingen voor Azure VPN-gateways met versleuteling en hash-algoritmen die in de tabel hierboven worden vermeld, om beveiliging van uw kritieke communicatie te waarborgen.

## <a name="known"></a>Bekende compatibiliteitsproblemen

> [!IMPORTANT]
> Dit zijn de bekende compatibiliteitsproblemen tussen VPN-apparaten van derden en Azure VPN-gateways. Het team van Azure werkt samen met de leveranciers aan een oplossing voor de hier vermelde problemen. Zodra de problemen zijn opgelost, wordt deze pagina bijgewerkt met de meest actuele informatie. Bekijk deze pagina daarom regelmatig.
>
>

### <a name="feb-16-2017"></a>16 februari 2017

**Palo Alto Networks-apparaten met oudere versie dan 7.1.4** voor route gebaseerde Azure VPN: Als u VPN-apparaten van Palo Alto Networks met een PAN-OS-versie ouder is dan 7.1.4 gebruikt en u met Azure route gebaseerde VPN-gateways connectiviteitsproblemen, voert u de volgende stappen uit:

1. Controleer de firmwareversie van uw Palo Alto Networks-apparaat. Als de PAN-OS-versie ouder is dan 7.1.4, voert u een upgrade uit naar 7.1.4.
2. Op het Palo Alto Networks-apparaat wijzigt u de levensduur van de beveiligingskoppeling fase 2 (of de beveiligingskoppeling in snelle modus) in 28.800 seconden (8 uur) wanneer er verbinding met de Azure VPN-gateway wordt gemaakt.
3. Als het connectiviteitsprobleem zich nog steeds voordoet, opent u een ondersteuningsaanvraag via de Azure-portal.
