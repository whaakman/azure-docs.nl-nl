---
title: Azure-VPN Gateway in azure Australië
description: Implementatie van VPN Gateway in azure Australië zodat deze voldoet aan de ISM en de bescherming van de Australische overheids instanties effectief te beschermen
author: galey801
ms.service: azure-australia
ms.topic: article
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 442ad6334a1775033018005d4a85875dbcb08ada
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571846"
---
# <a name="azure-vpn-gateway-in-azure-australia"></a>Azure-VPN Gateway in azure Australië

Een essentiële service met een open bare Cloud is de beveiligde verbinding van Cloud bronnen en-services met bestaande on-premises systemen.  De service die deze mogelijkheid biedt in azure, is Azure VPN Gateway (VPN Gateway). In dit artikel vindt u een overzicht van de belangrijkste aandachtspunten voor het configureren van de VPN Gateway om te voldoen aan de vereisten voor het [beheer van gegevens beveiliging hand matig](https://acsc.gov.au/infosec/ism/) van het Australische signalen (ISM).

Een VPN Gateway wordt gebruikt voor het verzenden van versleuteld verkeer tussen een virtueel netwerk in Azure en een ander netwerk.  Er zijn drie scenario's die worden verholpen door VPN-gateways:

- **Site-naar-site** S2S
- **Punt-naar-site** P2S
- **VNet-to-VNet**

In dit artikel wordt aandacht besteed aan S2S VPN-gateways. In diagram 1 ziet u een voor beeld van een site-naar-site-VPN-gateway configuratie.

![VPN Gateway met verbindingen met meerdere sites](media/vpngateway-multisite-connection-diagram.png)

*Diagram 1 – Azure site-naar-site-VPN Gateway*

## <a name="key-design-considerations"></a>Belangrijkste overwegingen bij het ontwerpen

Er zijn drie netwerk opties waarmee u Azure kunt verbinden met Australische overheids klanten:

- **DIAPICTOGRAM**
- **ExpressRoute**
- **Openbaar Internet**

De [Gebruikers handleiding voor](https://servicetrust.microsoft.com/viewpage/Australia) het Australische Cyber Security Center voor Azure adviseert dat VPN gateway (of een gelijkwaardige, beschermde gecertificeerde service van derden) wordt gebruikt in combi natie met de drie netwerk opties om ervoor te zorgen dat de verbindingen voldoen aan de ISM-besturings elementen voor versleuteling en integriteit.

### <a name="encryption-and-integrity"></a>Versleuteling en integriteit

Standaard onderhandelt de VPN de algoritmen voor versleuteling en integriteit tijdens de verbinding tot stand brengen als onderdeel van de IKE-Handshakes.  Tijdens de IKE-handshake zijn de configuratie en volg orde van voor keuren afhankelijk van het feit of de VPN Gateway de initiator of de responder (NB: dit wordt bepaald via het VPN-apparaat).  De uiteindelijke configuratie van de verbinding wordt bepaald door de configuratie van het VPN-apparaat.  Zie hier voor meer informatie over gevalideerde VPN-apparaten en hun configuratie: [Over VPN-apparaten](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices)

VPN-gateways kunnen versleuteling en integriteit beheren door een aangepast IPsec/IKE-beleid te configureren voor de verbinding.

### <a name="resource-operations"></a>Resource bewerkingen

VPN-gateways maken een verbinding tussen Azure-en niet-Azure-omgevingen via het open bare Internet.  De ISM heeft besturings elementen die betrekking hebben op de expliciete autorisatie van verbindingen.  Het is standaard mogelijk om VPN-gateways te gebruiken om niet-geautoriseerde tunnels te maken in beveiligde omgevingen.  Daarom is het essentieel dat organisaties op basis van Azure Role Access Control (RBAC) gebruiken om te bepalen wie VPN-gateways en hun verbindingen kan maken en wijzigen.  Azure heeft geen ingebouwde rol voor het beheren van VPN-gateways daarom is hiervoor een aangepaste rol vereist.

Toegang tot de rollen ' eigenaar ', ' Inzender ' en ' netwerk bijdrager ' wordt nauw keurig gecontroleerd.  Het wordt ook aanbevolen dat Azure AD Privileged Identity Management wordt gebruikt voor gedetailleerdere toegangs controle.

### <a name="high-availability"></a>Hoge beschikbaarheid

Azure VPN-gateways kunnen meerdere verbindingen hebben (zie diagram 1) en ondersteunen meerdere on-premises VPN-apparaten naar dezelfde on-premises omgeving.  

Virtuele netwerken in azure kunnen meerdere VPN-gateways hebben die kunnen worden geïmplementeerd in onafhankelijke, actief-passieve of actieve-actieve configuraties.

Het is raadzaam dat alle VPN-gateways worden geïmplementeerd in een [configuratie met hoge Beschik baarheid](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable): bijvoorbeeld twee on-PREMISES VPN-apparaten die zijn verbonden met twee VPN-gateways in de modus actief-passief of actief/actief (zie diagram 2).

![VPN Gateway redundante verbindingen](media/dual-redundancy.png)

*Diagram 2: actieve en actieve VPN-gateways en twee VPN-apparaten*

### <a name="forced-tunneling"></a>Geforceerde tunneling

Met geforceerde tunneling wordt alle Internet verkeer omgeleid naar de on-premises omgeving via de VPN Gateway voor inspectie en controle. Zonder geforceerde tunneling wordt het Internet-gebonden verkeer van Vm's in azure door de Azure-netwerk infrastructuur rechtstreeks naar het open bare Internet gepasseerd zonder de optie om het verkeer te controleren of te controleren.  Dit is essentieel wanneer de organisatie een beveiligde Internet gateway (SIG) voor een omgeving moet gebruiken.

## <a name="detailed-configuration"></a>Gedetailleerde configuratie

### <a name="service-attributes"></a>Service kenmerken

VPN-gateways voor S2S-verbindingen die zijn geconfigureerd voor de Australische overheid moeten de volgende kenmerken hebben:

|Kenmerk | PAS|
|--- | --- |
|Gateway type | VPN|
|

De kenmerk instellingen die vereist zijn om te voldoen aan de ISM-besturings elementen voor beveiligde zijn:

|Kenmerk | PAS|
|--- |---|
|vpnType |RouteBased|
|Verbinding/vpnClientProtocols | IkeV2|
|

Azure VPN-gateways ondersteunen een bereik van cryptografische algoritmen van de IPsec-en IKE-protocol standaarden.  De standaard beleids regels maximaliseren de interoperabiliteit met een breed scala aan VPN-apparaten van derden.  Als gevolg hiervan is het mogelijk dat tijdens de IKE-handshake een niet-compatibele configuratie wordt onderhandeld.  Het is daarom ten zeerste aanbevolen dat [aangepaste IPSec/IKE-beleids](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-ipsecikepolicy-rm-powershell) parameters worden toegepast op verbinding in VPN-gateways om ervoor te zorgen dat de verbindingen voldoen aan de ISM-besturings elementen voor on-premises omgevings verbindingen met Azure.  De belangrijkste kenmerken zijn:

|Kenmerk|WORDT|PAS|
|---|---|---|
|saLifeTimeSeconds|< 14400 sec.|> 300 seconden|
|saDataSizeKilobytes| |> 1024 KB|
|ipsecEncryption| |AES256-GCMAES256|
|ipsecIntegrity| |SHA256-GCMAES256|
|ikeEncryption| |AES256-GCMAES256|
|ikeIntegrity| |SHA256-GCMAES256|
|dhGroup|DHGroup14, DHGroup24, ECP256, ECP384|DHGroup2|
|pfsGroup|PFS2048, PFS24, ECP256, ECP384||
|

*Voor dhGroup en pfsGroup in de bovenstaande tabel zijn ECP256 en ECP384 de voor keur, zelfs als andere instellingen kunnen worden gebruikt*

### <a name="related-services"></a>Verwante services

Bij het ontwerpen en configureren van een Azure-VPN Gateway zijn er een aantal gerelateerde services die ook moeten bestaan en moeten worden geconfigureerd:

|Service | Actie vereist|
|--- | ---|
|Virtueel netwerk | VPN-gateways zijn gekoppeld aan een virtueel netwerk.  Een virtueel netwerk moet worden gemaakt voordat een nieuw VPN Gateway wordt gemaakt.|
|Openbaar IP-adres | S2S VPN-gateways hebben een openbaar IP-adres nodig om verbinding te maken tussen het on-premises VPN-apparaat en het VPN Gateway.  Een openbaar IP-adres moet worden gemaakt voordat een S2S-VPN Gateway wordt gemaakt.|
|Subnet | Er moet een subnet van het virtuele netwerk worden gemaakt voor de VPN Gateway.|
|

## <a name="implementation-steps-using-powershell"></a>Implementaties tappen met behulp van Power shell

### <a name="role-based-access-control-rbac"></a>RBAC (op rollen gebaseerd toegangsbeheer)

1. Aangepaste rol maken (bijvoorbeeld virtualNetworkGateway-bijdrager).  Maak een rol die moet worden toegewezen aan gebruikers die VPN-gateways kunnen maken en wijzigen. De aangepaste rol moet de volgende bewerkingen toestaan:

   Micro soft. Network/virtualNetworkGateways/*  
   Micro soft. Network/Connections/*  
   Micro soft. Network/localnetworkgateways/*  
   Microsoft.Network/virtualNetworks/subnets/*  
   Micro soft. Network/publicIPAddresses/*  
   Micro soft. Network/publicIPPrefixes/*  
   Microsoft.Network/routeTables/*  

2. Aangepaste rol toevoegen aan gebruikers die VPN-gateways en verbindingen met on-premises omgevingen mogen maken en beheren.

### <a name="create-vpn-gateway"></a>VPN Gateway maken

*Bij deze stappen wordt ervan uitgegaan dat er al een virtueel netwerk is gemaakt*

1. Een nieuw openbaar IP-adres maken
2. Een VPN Gateway subnet maken
3. Een VPN Gateway IP-configuratie maken
4. VPN Gateway maken
5. Een lokale netwerk gateway maken voor het on-premises VPN-apparaat
6. Een IPsec-beleid maken (uitgaande van aangepaste IPsec/IKE-beleids regels)
7. Verbinding maken tussen VPN Gateway en een lokale netwerk gateway met behulp van IPsec-beleid

### <a name="enforce-tunneling"></a>Tunneling afdwingen

Als geforceerde tunneling is vereist voordat u de VPN Gateway maakt:

1. Een route tabel en een route regel (s) maken
2. De route tabel koppelen aan de juiste subnetten

Na het maken van de VPN Gateway:

1. Stel GatewayDefaultSite in op de on-premises omgeving op de VPN Gateway

### <a name="example-powershell-script"></a>Voor beeld Power shell-script

Een voor beeld van een Power shell-script voor het maken van een aangepast IPSEC/IKE-beleid dat voldoet aan ISM-besturings elementen voor Australische beveiligde beveiligings classificatie.

Hierbij wordt ervan uitgegaan dat het virtuele netwerk, VPN Gateway en lokale gateways bestaan.

#### <a name="create-an-ipsecike-policy"></a>Een IPsec/IKE-beleid maken

Met het volgende voorbeeld script maakt u een IPsec/IKE-beleid met de volgende algoritmen en para meters:

- IKEv2: AES256, SHA256, DHGroup ECP256
- IPsec: AES256, SHA256, PFS ECP256, levens duur van SA 14.400 seconden & 102400000 KB

```powershell
$custompolicy = New-AzIpsecPolicy `
                    -IkeEncryption AES256 `
                    -IkeIntegrity SHA256 `
                    -DhGroup ECP256 `
                    -IpsecEncryption AES256 `
                    -IpsecIntegrity SHA256 `
                    -PfsGroup ECP256 `
                    -SALifeTimeSeconds 14400 `
                    -SADataSizeKilobytes 102400000
```

#### <a name="create-a-s2s-vpn-connection-with-the-custom-ipsecike-policy"></a>Een S2S-VPN-verbinding maken met het aangepaste IPsec/IKE-beleid

```powershell
$vpngw = Get-AzVirtualNetworkGateway `
                    -Name "<yourVPNGatewayName>" `
                    -ResourceGroupName "<yourResourceGroupName>"
$localgw = Get-AzLocalNetworkGateway  `
                    -Name "<yourLocalGatewayName>" `
                    -ResourceGroupName "<yourResourceGroupName>"

New-AzVirtualNetworkGatewayConnection `
                    -Name "ConnectionName" `
                    -ResourceGroupName "<yourResourceGroupName>" `
                    -VirtualNetworkGateway1 $vpngw `
                    -LocalNetworkGateway2 $localgw `
                    -Location "Australia Central" `
                    -ConnectionType IPsec `
                    -IpsecPolicies $custompolicy `
                    -SharedKey "AzureA1b2C3"
```

## <a name="next-steps"></a>Volgende stappen

Dit artikel heeft betrekking op de specifieke configuratie van VPN Gateway om te voldoen aan de vereisten die zijn opgegeven in de Information Security-hand leiding (ISM) voor het beveiligen van beveiligde gegevens van de Australische overheid tijdens de overdracht. Voor gedetailleerde stappen voor het configureren van uw VPN Gateway:

- [Overzicht van Azure Virtual Network gateway](https://docs.microsoft.com/azure/vpn-gateway/)  
- [Wat is VPN Gateway?](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)  
- [Een VNet maken met een site-naar-site-VPN-verbinding met behulp van Power shell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell)  
- [Een VPN-gateway maken en beheren](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-tutorial-create-gateway-powershell)