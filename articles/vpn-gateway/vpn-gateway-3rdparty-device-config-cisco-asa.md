---
title: Voorbeeldconfiguratie voor Cisco ASA-apparaten verbinden met Azure VPN-gateways | Microsoft Docs
description: Dit artikel bevat een voorbeeldconfiguratie voor Cisco ASA-apparaten verbinden met Azure VPN-gateways.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 10/19/2018
ms.author: yushwang
ms.openlocfilehash: f51a97e1493803998cfbdd81523e4e479b50346d
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55697102"
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a>Voorbeeldconfiguratie: Cisco ASA-apparaat (IKEv2/geen BGP)
Dit artikel bevat voorbeelden van configuraties voor netwerkapparaten Cisco Adaptive Security toestel (ASA) met Azure VPN-gateways. Het voorbeeld geldt voor Cisco ASA-apparaten die worden uitgevoerd zonder de Border Gateway Protocol (BGP) IKEv2. 

## <a name="device-at-a-glance"></a>Apparaat in een oogopslag

|                        |                                   |
| ---                    | ---                               |
| De leverancier van apparaat          | Cisco                             |
| Apparaatmodel           | ASA                               |
| Doelversie         | 8.4 en hoger                     |
| Geteste model           | ASA 5505                          |
| Geteste versie         | 9.2                               |
| IKE-versie            | IKEv2                             |
| BGP                    | Nee                                |
| Azure VPN gateway-type | Op route gebaseerde VPN-gateway           |
|                        |                                   |

> [!NOTE]
> De voorbeeldconfiguratie een Cisco ASA-apparaat verbindt met een Azure **op route gebaseerde** VPN-gateway. De verbinding maakt gebruik van een aangepast IPsec/IKE-beleid met de **UsePolicyBasedTrafficSelectors** optie, zoals beschreven in [in dit artikel](vpn-gateway-connect-multiple-policybased-rm-ps.md).
>
> Het voorbeeld vereist die gebruikmaken van de ASA-apparaten de **IKEv2** beleid met de configuraties op basis van access-lijst niet VTI op basis van. Raadpleeg de VPN-leverancier apparaatspecificaties om te controleren of de IKEv2 beleid wordt ondersteund op uw on-premises VPN-apparaten.


## <a name="vpn-device-requirements"></a>Vereisten voor VPN-apparaten
Azure VPN-gateways gebruiken de standaard IPsec/IKE-protocol-pakketten naar de Site-naar-Site (S2S) VPN-tunnels tot stand brengen. Zie voor de gedetailleerde parameters voor IPsec/IKE-protocol en de standaard cryptografische algoritmen voor Azure VPN-gateways, [over VPN-apparaten](vpn-gateway-about-vpn-devices.md).

> [!NOTE]
> U kunt optioneel een exact dezelfde combinatie van cryptografische algoritmen en belangrijkste sterke punten voor een specifieke verbinding opgeven zoals beschreven in [over cryptografische vereisten](vpn-gateway-about-compliance-crypto.md). Als u een exacte combinatie van algoritmen en belangrijkste sterke punten opgeeft, zorg er dan voor dat de bijbehorende specificaties gebruiken op uw VPN-apparaten.

## <a name="single-vpn-tunnel"></a>Één VPN-tunnel
Deze configuratie bestaat uit één S2S-VPN-tunnel tussen een Azure VPN-gateway en een on-premises VPN-apparaat. U kunt eventueel de [BGP via de VPN-tunnel](#bgp).

![Één S2S-VPN-tunnel](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

Zie voor stapsgewijze instructies voor het bouwen van de Azure-configuraties [één VPN-tunnel-installatie](vpn-gateway-3rdparty-device-config-overview.md#singletunnel).

### <a name="virtual-network-and-vpn-gateway-information"></a>Virtueel netwerk en gegevens van VPN-gateway
Deze sectie vindt u de parameters voor het voorbeeld.

| **Parameter**                | **Waarde**                    |
| ---                          | ---                          |
| Virtueel netwerk-adresvoorvoegsels        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure VPN-gateway-IP         | Azure_Gateway_Public_IP      |
| On-premises adresvoorvoegsels | 10.51.0.0/16<br>10.52.0.0/16 |
| On-premises VPN-apparaat IP    | OnPrem_Device_Public_IP     |
| * Virtuele netwerk ASN van BGP                | 65010                        |
| * Azure BGP-peer-IP           | 10.12.255.30                 |
| * On-premises BGP ASN         | 65050                        |
| * On-premises BGP-peer-IP     | 10.52.255.254                |
|                              |                              |

\* Optionele parameter voor BGP alleen.

### <a name="ipsecike-policy-and-parameters"></a>IPsec/IKE-beleid en parameters
De volgende tabel bevat de IPsec-/ IKE-algoritmen en parameters die worden gebruikt in het voorbeeld. Raadpleeg de specificaties van uw VPN-apparaat om te controleren of de algoritmen die worden ondersteund voor uw VPN-apparaatmodellen en firmware-versies.

| **IPsec/IKEv2**  | **Waarde**                            |
| ---              | ---                                  |
| IKEv2-versleuteling | AES256                               |
| IKEv2-integriteit  | SHA384                               |
| DH-groep         | DHGroup24                            |
| * IPsec-versleuteling | AES256                               |
| * IPsec-integriteit  | SHA1                                 |
| PFS-groep        | PFS24                                |
| QM SA-levensduur   | 7200 seconden                         |
| Verkeersselector | UsePolicyBasedTrafficSelectors $True |
| Vooraf gedeelde sleutel   | PreSharedKey                         |
|                  |                                      |

\* Op sommige apparaten moet IPsec-integriteit een null-waarde wanneer het IPsec-versleutelingsalgoritme AES-GCM is.

### <a name="asa-device-support"></a>Ondersteuning voor ASA-apparaat

* Ondersteuning voor IKEv2 vereist ASA versie 8.4 en hoger.

* Ondersteuning voor DH-groep en PFS-groep buiten groep-5 vereist ASA versie 9.x.

* Ondersteuning voor IPSec-versleuteling met AES-GCM en IPsec-integriteit met SHA-256, SHA-384 en SHA-512, vereist ASA versie 9.x. Deze vereiste ondersteuning geldt voor nieuwere ASA-apparaten. Op het moment van publicatie ondersteunen modellen ASA 5505, 5510 5520, 5540, 5550 en 5580 geen deze algoritmen. Raadpleeg de specificaties van uw VPN-apparaat om te controleren of de algoritmen die worden ondersteund voor uw VPN-apparaatmodellen en firmware-versies.


### <a name="sample-device-configuration"></a>Voorbeeldconfiguratie voor apparaat
Het script bevat een voorbeeld dat is gebaseerd op de configuratie en de parameters die worden beschreven in de vorige secties. De configuratie van de S2S VPN-tunnel bestaat uit de volgende onderdelen:

1. Interfaces en routes
2. Een lijst met toegang
3. IKE-beleid en parameters (fase 1 of hoofdmodus)
4. IPSec-beleid en parameters (fase 2 of in de snelle modus)
5. Andere parameters, zoals TCP MSS MSS

> [!IMPORTANT]
> De volgende stappen voltooien voordat u het voorbeeldscript gebruiken. Vervang de tijdelijke waarden in het script met de instellingen voor apparaten voor uw configuratie.

* Geef de configuratie van de interface voor zowel binnen als buiten-interfaces.
* Identificeer de routes voor uw binnen/persoonlijk en buiten/openbare netwerken.
* Zorg ervoor dat alle namen en beleid getallen zijn uniek op uw apparaat.
* Zorg ervoor dat de cryptografische algoritmen worden ondersteund op uw apparaat.
* Vervang de volgende **tijdelijke aanduiding voor waarden** met daadwerkelijke waarden voor uw configuratie:
  - Buiten de naam van interface: **buiten**
  - **Azure_Gateway_Public_IP**
  - **OnPrem_Device_Public_IP**
  - IKE: **Pre_Shared_Key**
  - Virtueel netwerk en de namen van de LAN-gateways: **VNetName** en **LNGName**
  - Virtueel netwerk- en on-premises netwerkadres **voorvoegsels**
  - Juiste **netmaskers**

#### <a name="sample-script"></a>Voorbeeldscript

```
! Sample ASA configuration for connecting to Azure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace the following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - the Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with the actual nexthop IP address
!
! (*) Must be unique names in the device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on the outside interface or vlan
!     > <PrivateIPAddress> on the inside interface or vlan; e.g., 10.51.0.1/24
!     > Route to connect to <Azure_Gateway_Public_IP> address
!
!     > Example:
!
!       interface Ethernet0/0
!        switchport access vlan 2
!       exit
!
!       interface vlan 1
!        nameif inside
!        security-level 100
!        ip address <PrivateIPAddress> <Netmask>
!       exit
!
!       interface vlan 2
!        nameif outside
!        security-level 0
!        ip address <OnPrem_Device_Public_IP> <Netmask>
!       exit
!
!       route outside 0.0.0.0 0.0.0.0 <NextHop IP> 1
!
! ==> Access lists
!
!     > Most firewall devices deny all traffic by default. Create access lists to
!       (1) Allow S2S VPN tunnels between the ASA and the Azure gateway public IP address
!       (2) Construct traffic selectors as part of IPsec policy or proposal
!
access-list outside_access_in extended permit ip host <Azure_Gateway_Public_IP> host <OnPrem_Device_Public_IP>
!
!     > Object group that consists of all VNet prefixes (e.g., 10.11.0.0/16 &
!       10.12.0.0/16)
!
object-group network Azure-<VNetName>
 description Azure virtual network <VNetName> prefixes
 network-object 10.11.0.0 255.255.0.0
 network-object 10.12.0.0 255.255.0.0
exit
!
!     > Object group that corresponding to the <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines the on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify the access-list between the Azure VNet and your on-premises network.
!       This access list defines the IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between the on-premises network and Azure VNet
!
nat (inside,outside) source static <LNGName> <LNGName> destination static Azure-<VNetName> Azure-<VNetName>
!
! ==> IKEv2 configuration
!
!     > General IKEv2 configuration - enable IKEv2 for VPN
!
group-policy DfltGrpPolicy attributes
 vpn-tunnel-protocol ikev1 ikev2
exit
!
crypto isakmp identity address
crypto ikev2 enable outside
!
!     > Define IKEv2 Phase 1/Main Mode policy
!       - Make sure the policy number is not used
!       - integrity and prf must be the same
!       - DH group 14 and above require ASA version 9.x.
!
crypto ikev2 policy 1
 encryption       aes-256
 integrity        sha384
 prf              sha384
 group            24
 lifetime seconds 86400
exit
!
!     > Set connection type and pre-shared key
!
tunnel-group <Azure_Gateway_Public_IP> type ipsec-l2l
tunnel-group <Azure_Gateway_Public_IP> ipsec-attributes
 ikev2 remote-authentication pre-shared-key <Pre_Shared_Key> 
 ikev2 local-authentication  pre-shared-key <Pre_Shared_Key> 
exit
!
! ==> IPsec configuration
!
!     > IKEv2 Phase 2/Quick Mode proposal
!       - AES-GCM and SHA-2 requires ASA version 9.x on newer ASA models. ASA
!         5505, 5510, 5520, 5540, 5550, 5580 are not supported.
!       - ESP integrity must be null if AES-GCM is configured as ESP encryption
!
crypto ipsec ikev2 ipsec-proposal AES-256
 protocol esp encryption aes-256
 protocol esp integrity  sha-1
exit
!
!     > Set access list & traffic selectors, PFS, IPsec proposal, SA lifetime
!       - This sample uses "Azure-<VNetName>-map" as the crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned to your outside interface, you must use
!         the same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses the access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses the proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS to 1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a>Eenvoudige foutopsporing opdrachten

Gebruik de volgende ASA-opdrachten voor foutopsporing:

* De IPSec- of IKE-beveiligingskoppelingen (SA) weergeven:
    ```
    show crypto ipsec sa
    show crypto ikev2 sa
    ```

* Voer de foutopsporingsmodus:
    ```
    debug crypto ikev2 platform <level>
    debug crypto ikev2 protocol <level>
    ```
    De `debug` opdrachten aanzienlijke uitvoer in de console kunnen genereren.

* De huidige configuraties weergeven op het apparaat:
    ```
    show run
    ```
    Gebruik `show` subopdrachten naar specifieke delen van de lijst van de configuratie van het apparaat, bijvoorbeeld:
    ```
    show run crypto
    show run access-list
    show run tunnel-group
    ```

## <a name="next-steps"></a>Volgende stappen
Voor het actief / actief cross-premises en VNet-naar-VNet-verbindingen configureren, Zie [actief / actief VPN-gateways configureren](vpn-gateway-activeactive-rm-powershell.md).
