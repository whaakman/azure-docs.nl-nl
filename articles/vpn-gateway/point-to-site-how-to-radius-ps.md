---
title: 'Een computer verbinden met een virtueel netwerk met behulp van punt-naar-Site en RADIUS-verificatie: PowerShell | Azure'
description: Windows en Mac OS X-clients veilig verbinden met een virtueel netwerk met behulp van P2S en RADIUS-verificatie.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: cherylmc
ms.openlocfilehash: 0aea797458649af3f839b7b8ae9b5cea384fe3b0
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56865141"
---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-radius-authentication-powershell"></a>Een punt-naar-Site-verbinding met een VNet met behulp van RADIUS-verificatie configureren: PowerShell

Dit artikel ziet u hoe u een VNet maakt met een punt-naar-Site-verbinding die gebruikmaakt van RADIUS-verificatie. Deze configuratie is alleen beschikbaar voor de Resource Manager-implementatiemodel.

Met een punt-naar-site-VPN-gateway (P2S) kunt u vanaf een afzonderlijke clientcomputer een beveiligde verbinding maken met uw virtuele netwerk. Punt-naar-Site VPN-verbindingen zijn nuttig wanneer u verbinding maken met uw VNet vanaf een externe locatie wilt, zoals wanneer u vanaf thuis of een conferentie teleforenzen. Een P2S-VPN is ook een uitstekende oplossing in plaats van een site-naar-site-VPN wanneer u maar een paar clients hebt die verbinding moeten maken met een VNet.

Er wordt een P2S-VPN-verbinding gestart vanaf Windows en Mac-apparaten. Clients die verbinding maken, kunnen de volgende verificatiemethoden gebruiken:

* RADIUS-server
* Systeemeigen VPN-Gateway-certificaatverificatie

Dit artikel helpt u bij het configureren van een P2S-configuratie met verificatie met behulp van RADIUS-server. Als u wilt verifiëren met behulp van gegenereerde certificaten en systeemeigen certificaatverificatie van VPN-gateway in plaats daarvan, Zie [een punt-naar-Site-verbinding met een VNet met behulp van systeemeigen certificaatverificatie van VPN-gateway configureren](vpn-gateway-howto-point-to-site-rm-ps.md).

![Verbindingsdiagram - RADIUS](./media/point-to-site-how-to-radius-ps/p2sradius.png)

Punt-naar-site-verbindingen hebben geen VPN-apparaat of openbaar IP-adres nodig. P2S maakt de VPN-verbinding via SSTP (Secure Socket Tunneling Protocol) of IKEv2.

* SSTP is een op SSL gebaseerde VPN-tunnel die alleen wordt ondersteund op Windows-clientplatforms. Omdat met SSTP firewalls kunnen worden gepasseerd, is dit een ideale optie om vanaf elke locatie verbinding te maken met Azure. Wij ondersteunen SSTP versies 1.0, 1.1 en 1.2 aan de serverzijde. De client besluit welke versie moet worden gebruikt. Voor Windows 8.1 en hoger, gebruikt SSTP standaard 1.2.

* IKEv2 VPN, een op standaarden gebaseerde IPsec VPN-oplossing. IKEv2 VPN kan worden gebruikt om verbinding te maken vanaf Mac-apparaten (OSX-versie 10.11 en hoger).

Voor P2S-verbindingen is het volgende vereist:

* Een RouteBased VPN-gateway. 
* Een RADIUS-server voor het afhandelen van verificatie van de gebruiker. De RADIUS-server kan worden on-premises geïmplementeerd, of in het Azure-VNet.
* Een VPN-clientconfiguratiepakket voor de Windows-apparaten die verbinding met het VNet maken. Een VPN-clientconfiguratiepakket bevat de instellingen die vereist zijn voor een VPN-client verbinding maken via P2S.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="aboutad"></a>Over Active Directory (AD)-domeinverificatie voor P2S VPN 's

AD-domeinverificatie kan gebruikers zich aanmelden bij Azure met de domeinreferenties van hun organisatie. Hiervoor een RADIUS-server die is geïntegreerd met de AD-server. Organisaties kunnen ook gebruikmaken van hun bestaande RADIUS-implementatie.
 
De RADIUS-server kan zich on-premises bevinden of in uw Azure VNet. Tijdens de verificatie, wordt de VPN-gateway fungeert als een Pass Through-query en stuurt verificatieberichten heen en weer tussen de RADIUS-server en het verbindende apparaat. Het is belangrijk voor de VPN-gateway te kunnen bereiken van de RADIUS-server. Als de RADIUS-server zich op locatie is, klikt u vervolgens is een Site-naar-Site VPN-verbinding van Azure met de on-premises site vereist.

Naast de Active Directory, kan een RADIUS-server ook worden geïntegreerd met andere systemen externe id. Hiermee opent u tal van verificatieopties voor punt-naar-Site-VPN's, met inbegrip van opties voor MFA. Controleer uw leveranciersdocumentatie van RADIUS-server om de lijst kan worden geïntegreerd met identiteitssystemen.

![Verbindingsdiagram - RADIUS](./media/point-to-site-how-to-radius-ps/radiusimage.png)

> [!IMPORTANT]
>Alleen een Site-naar-Site VPN-verbinding kan worden gebruikt voor het verbinden met een RADIUS-server on-premises. Een ExpressRoute-verbinding kan niet worden gebruikt.
>
>

## <a name="before"></a>Voordat u begint

Controleer of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial).

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="signin"></a>Aanmelden

[!INCLUDE [sign in](../../includes/vpn-gateway-cloud-shell-ps-login.md)]

### <a name="example"></a>Voorbeeldwaarden

U kunt de volgende voorbeeldwaarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel. U kunt de stappen gebruiken als een overzicht en de waarden ongewijzigd gebruiken, of u kunt ze wijzigen zodat ze overeenkomen met uw omgeving.

* **Naam: VNet1**
* **Adresruimte: 192.168.0.0/16** en **10.254.0.0/16**<br>Voor dit voorbeeld gebruiken we meer dan één adresruimte om te verduidelijken dat deze configuratie met meerdere adresruimten werkt. Meerdere adresruimten zijn echter niet vereist voor deze configuratie.
* **Subnetnaam: FrontEnd**
  * **Subnetadresbereik: 192.168.1.0/24**
* **Subnetnaam: BackEnd**
  * **Subnetadresbereik: 10.254.1.0/24**
* **Subnetnaam: GatewaySubnet**<br>De naam van het subnet *GatewaySubnet* is verplicht voor een goede werking van de VPN-gateway.
  * **Adresbereik GatewaySubnet: 192.168.200.0/24** 
* **VPN-clientadresgroep: 172.16.201.0/24**<br>VPN-clients die verbinding maken met het VNet via deze punt-naar-site-verbinding, ontvangen een IP-adres van de VPN-clientadresgroep.
* **Abonnement:** Als u meer dan één abonnement hebt, controleert u of dat u van het juiste is gebruikmaakt.
* **Resourcegroep: TestRG**
* **Locatie: VS-Oost**
* **DNS-Server: IP-adres** van de DNS-server die u wilt gebruiken voor naamomzetting voor uw VNet. (optioneel)
* **Naam van GW: Vnet1GW**
* **Openbare IP-naam: VNet1GWPIP**
* **VpnType: RouteBased**

## 1. <a name="vnet"></a>Maken van de resourcegroep, VNet en openbare IP-adres

De volgende stappen maakt een resourcegroep en een virtueel netwerk in de resourcegroep met drie subnetten. Bij het vervangen van waarden, is het belangrijk dat u altijd de naam het gatewaysubnet bijzonder GatewaySubnet. Als u een andere naam kiest, mislukt het maken van de gateway.

1. Maak een resourcegroep.

  ```azurepowershell-interactive
  New-AzResourceGroup -Name "TestRG" -Location "East US"
  ```
2. Maak de subnetconfiguraties voor het virtuele netwerk, noem deze *FrontEnd*, *BackEnd* en *GatewaySubnet*. Deze voorvoegsels moeten deel uitmaken van de VNet-adresruimte die u hebt opgegeven.

  ```azurepowershell-interactive
  $fesub = New-AzVirtualNetworkSubnetConfig -Name "FrontEnd" -AddressPrefix "192.168.1.0/24"  
  $besub = New-AzVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.254.1.0/24"  
  $gwsub = New-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "192.168.200.0/24"
  ```
3. Maak het virtuele netwerk.

  In dit voorbeeld is de serverparameter-DnsServer optioneel. Het opgeven van een waarde betekent niet dat er een nieuwe DNS-server wordt gemaakt. Het IP-adres van de DNS-server dat u opgeeft, moet het adres zijn van een DNS-server die de namen kan omzetten voor de resources waarmee u verbinding maakt vanuit uw VNet. Voor dit voorbeeld hebben we een privé-IP-adres gebruikt, maar het is zeer onwaarschijnlijk dat dit het IP-adres van uw DNS-server is. Zorg ervoor dat u uw eigen waarden gebruikt. De waarde die u opgeeft wordt gebruikt door de resources die u met het VNet, niet door de P2S-verbinding implementeert.

  ```azurepowershell-interactive
  New-AzVirtualNetwork -Name "VNet1" -ResourceGroupName "TestRG" -Location "East US" -AddressPrefix "192.168.0.0/16","10.254.0.0/16" -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
  ```
4. Een VPN Gateway moet een openbaar IP-adres hebben. U vraagt eerst de resource van het IP-adres aan en verwijst hier vervolgens naar bij het maken van uw virtuele netwerkgateway. Het IP-adres wordt dynamisch aan de resource toegewezen wanneer de VPN Gateway wordt gemaakt. VPN Gateway ondersteunt momenteel alleen *dynamische* toewijzing van openbare IP-adressen. U kunt geen toewijzing van een statisch openbaar IP-adres aanvragen. Dit betekent echter niet dat het IP-adres wordt gewijzigd nadat het aan uw VPN Gateway is toegewezen. Het openbare IP-adres verandert alleen wanneer de gateway wordt verwijderd en opnieuw wordt gemaakt. Het verandert niet wanneer de grootte van uw VPN Gateway verandert, wanneer deze gateway opnieuw wordt ingesteld of wanneer andere interne onderhoudswerkzaamheden of upgrades worden uitgevoerd.

  Geef de variabelen voor het aanvragen van een dynamisch toegewezen openbare IP-adres.

  ```azurepowershell-interactive
  $vnet = Get-AzVirtualNetwork -Name "VNet1" -ResourceGroupName "TestRG"  
  $subnet = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet 
  $pip = New-AzPublicIpAddress -Name "VNet1GWPIP" -ResourceGroupName "TestRG" -Location "East US" -AllocationMethod Dynamic 
  $ipconf = New-AzVirtualNetworkGatewayIpConfig -Name "gwipconf" -Subnet $subnet -PublicIpAddress $pip
  ```

## 2. <a name="radius"></a>Instellen van uw RADIUS-server

Voordat u het maken en configureren van de virtuele netwerkgateway, moet uw RADIUS-server correct worden geconfigureerd voor verificatie.

1. Als u geen een RADIUS-server die is geïmplementeerd, implementeert u een. Raadpleeg de installatiehandleiding geleverd door de leverancier van de RADIUS-voor de implementatiestappen.  
2. Configureer de VPN-gateway als een RADIUS-client op de RADIUS. Geef het virtuele netwerk GatewaySubnet dat u hebt gemaakt bij het toevoegen van deze RADIUS-client. 
3. Zodra de RADIUS-server is ingesteld, IP-adres van de RADIUS-server en het gedeelde geheim dat RADIUS-clients gebruiken moeten om te communiceren met de RADIUS-server ophalen. Als de RADIUS-server in de Azure-VNet, gebruikt u de CA-IP-adres van de virtuele machine van de RADIUS-server.

De [Network Policy Server (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) artikel bevat richtlijnen over het configureren van een Windows-RADIUS-server (NPS) voor de verificatie van AD-domeinen.

## 3. <a name="creategw"></a>De VPN-gateway maken

Configureer en maak de VPN-gateway voor uw VNet.

* Het GatewayType 'Vpn' moet zijn en - VpnType moet 'RouteBased'.
* Een VPN-gateway kan tot 45 minuten duren om uit te voeren, afhankelijk van de [gateway-SKU](vpn-gateway-about-vpn-gateway-settings.md#gwsku) u selecteert.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1
```

## 4. <a name="addradius"></a>De RADIUS-server en client-adresgroep toevoegen
 
* De RadiusServer - kan worden opgegeven door de naam of IP-adres. Als u de naam en de server bevindt zich on-premises, kan de VPN-gateway kan niet worden de naam kunnen omzetten. Als dit het geval is, is het beter om op te geven van het IP-adres van de server. 
* De RadiusSecret - moet overeenkomen met wat er op uw RADIUS-server is geconfigureerd.
* Het VpnClientAddressPool - is het bereik van waaruit de verbindende VPN-clients een IP-adres ontvangen. Gebruik een privé-IP-adresbereik dat niet overlapt met de on-premises locatie waarvanaf u verbinding maakt of met het VNet waarmee u verbinding wilt maken. Zorg ervoor dat u een groot genoeg adresgroep geconfigureerd hebt.  

1. Maak een beveiligde tekenreeks voor de RADIUS geheim.

  ```azurepowershell-interactive
  $Secure_Secret=Read-Host -AsSecureString -Prompt "RadiusSecret"
  ```

2. U wordt gevraagd naar de RADIUS-geheim invoeren. De tekens die u invoert worden niet weergegeven en in plaats daarvan wordt vervangen door de "*" teken.

  ```azurepowershell-interactive
  RadiusSecret:***
  ```
3. De adresgroep van de VPN-client en de informatie van RADIUS-server toevoegen.

  Voor SSTP-configuraties:

    ```azurepowershell-interactive
    $Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol "SSTP" `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```

  Voor IKEv2-configuraties:

    ```azurepowershell-interactive
    $Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol "IKEv2" `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```

  Voor SSTP en IKEv2

    ```azurepowershell-interactive
    $Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol @( "SSTP", "IkeV2" ) `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```

## 5. <a name="vpnclient"></a>Het configuratiepakket voor VPN-client downloaden en de VPN-client instellen

De configuratie van de VPN-client kunt apparaten verbinding maken met een VNet via een P2S-verbinding. Als u wilt een VPN-clientconfiguratiepakket genereren en instellen van de VPN-client, Zie [maken van een VPN-clientconfiguratie voor RADIUS-verificatie](point-to-site-vpn-client-configuration-radius.md).

## <a name="connect"></a>6. Verbinding maken met Azure

### <a name="to-connect-from-a-windows-vpn-client"></a>Verbinding maken vanaf een Windows-VPN-client

1. Als u met uw VNet wilt verbinden, gaat u op de clientcomputer naar de VPN-verbindingen en zoekt u de VPN-verbinding die u hebt gemaakt. Deze heeft dezelfde naam als het virtuele netwerk. Voer de domeinreferenties van uw en klik op 'Connect'. Een pop-upbericht aanvragen verhoogde rechten wordt weergegeven. Accepteren en voer de referenties in.

  ![VPN-client maakt verbinding met Azure](./media/point-to-site-how-to-radius-ps/client.png)
2. De verbinding is tot stand gebracht.

  ![Verbinding tot stand gebracht](./media/point-to-site-how-to-radius-ps/connected.png)

### <a name="connect-from-a-mac-vpn-client"></a>Verbinding maken vanaf een Mac-VPN-client

Zoek in het dialoogvenster Netwerk het clientprofiel dat u wilt gebruiken en klik op **Verbinding maken**.

  ![Mac-verbinding](./media/vpn-gateway-howto-point-to-site-rm-ps/applyconnect.png)

## <a name="verify"></a>De verbinding verifiëren

1. Als u wilt controleren of uw VPN-verbinding actief is, opent u een opdrachtprompt met verhoogde bevoegdheid en voert u *ipconfig/all* in.
2. Bekijk de resultaten. Het IP-adres dat u hebt ontvangen is een van de adressen binnen het adresbereik van de punt-naar-site-VPN-clientadresgroep die u hebt opgegeven in uw configuratie. De resultaten zijn vergelijkbaar met het volgende voorbeeld:

  ```
  PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
  ```

Zie voor het oplossen van een P2S-verbinding, [punt-naar-site-verbindingen van Azure voor het oplossen van](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).

## <a name="connectVM"></a>Verbinding maken met een virtuele machine

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="faq"></a>Veelgestelde vragen

Deze Veelgestelde vragen is van toepassing op P2S met behulp van RADIUS-verificatie

[!INCLUDE [Point-to-Site RADIUS FAQ](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Volgende stappen

Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/) voor meer informatie. Zie [Azure and Linux VM Network Overview](../virtual-machines/linux/azure-vm-network-overview.md) (Overzicht van Azure- en Linux-VM-netwerken) voor meer informatie over netwerken en virtuele machines.
