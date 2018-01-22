---
title: 'Via een computer verbinding maken met een virtueel Azure-netwerk met behulp van een point-to-site-verbinding en systeemeigen Azure-certificaatverificatie: PowerShell | Microsoft Docs'
description: U kunt een computer op een veilige manier verbinden met uw virtuele netwerk door een point-to-site-VPN-gatewayverbinding te maken met behulp van systeemeigen VPN-gateway-certificaatverificatie van Azure. Dit artikel is van toepassing op het Resource Manager-implementatiemodel en maakt gebruik van PowerShell.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3eddadf6-2e96-48c4-87c6-52a146faeec6
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2018
ms.author: cherylmc
ms.openlocfilehash: bbaa5a6bbc01af4529c657aee3b2916942b4269f
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-native-azure-certificate-authentication-powershell"></a>Een point-to-site-verbinding naar een VNet configureren met behulp van systeemeigen Azure-certificaatverificatie: PowerShell

In dit artikel wordt beschreven hoe u een VNet met een punt-naar-site-verbinding maakt in het Resource Manager-implementatiemodel met behulp van PowerShell. Bij deze configuratie wordt gebruikgemaakt van certificaten voor verificatie. In deze configuratie voert de Azure VPN-gateway de validatie van het certificaat uit in plaats van een RADIUS-server. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure Portal (klassiek)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Met een punt-naar-site-VPN-gateway (P2S) kunt u vanaf een afzonderlijke clientcomputer een beveiligde verbinding maken met uw virtuele netwerk. P2S-verbindingen zijn nuttig als u verbinding wilt maken met uw VNet vanaf een externe locatie, bijvoorbeeld als u ook thuis werkt of op een congres verbinding wilt maken. Een P2S-VPN is ook een uitstekende oplossing in plaats van een site-naar-site-VPN wanneer u maar een paar clients hebt die verbinding moeten maken met een VNet. Er wordt een P2S-VPN-verbinding gestart vanaf Windows en Mac-apparaten. 

Clients die verbinding maken, kunnen de volgende verificatiemethoden gebruiken:

* RADIUS-server
* Systeemeigen Azure-certificaatverificatie van VPN Gateway

Dit artikel helpt u bij het configureren van een P2S-configuratie met verificatie met behulp van de systeemeigen Azure-certificaatverificatie. Zie [P2S using RADIUS authentication](point-to-site-how-to-radius-ps.md) (P2S met behulp van RADIUS-verificatie) als u RADIUS wilt gebruiken om gebruikers die verbinding maken, te verifiëren.

![Diagram: een computer verbinden met een Azure VNet-punt-naar-site-verbinding](./media/vpn-gateway-howto-point-to-site-rm-ps/p2snativeps.png)

Punt-naar-site-verbindingen hebben geen VPN-apparaat of openbaar IP-adres nodig. P2S maakt de VPN-verbinding via SSTP (Secure Socket Tunneling Protocol) of IKEv2.

* SSTP is een op SSL gebaseerde VPN-tunnel die alleen wordt ondersteund op Windows-clientplatforms. Omdat met SSTP firewalls kunnen worden gepasseerd, is dit een ideale optie om vanaf elke locatie verbinding te maken met Azure. De SSTP-versies 1.0, 1.1 en 1.2 aan de serverzijde worden ondersteund. De client besluit welke versie moet worden gebruikt. Voor Windows 8.1 en hoger, gebruikt SSTP standaard 1.2.

* IKEv2 VPN, een op standaarden gebaseerde IPsec VPN-oplossing. IKEv2 VPN kan worden gebruikt om verbinding te maken vanaf Mac-apparaten (OSX-versie 10.11 en hoger).

Voor point-to-site-verbindingen met systeemeigen Azure-certificaatverificatie is het volgende vereist:

* Een RouteBased VPN-gateway.
* De openbare sleutel (CER-bestand) voor een basiscertificaat dat is geüpload naar Azure. Nadat het certificaat is geüpload, wordt het beschouwd als een vertrouwd certificaat en wordt het gebruikt voor verificatie.
* Een clientcertificaat dat wordt gegenereerd op basis van het basiscertificaat en dat wordt geïnstalleerd op elke clientcomputer die verbinding gaat maken met het VNet. Dit certificaat wordt gebruikt voor clientverificatie.
* Een VPN-clientconfiguratie. De configuratiebestanden voor de VPN-client bevatten de gegevens die nodig zijn om de client te verbinden met het VNet. Met de bestanden wordt de bestaande VPN-client geconfigureerd die is ingebouwd in het besturingssysteem. Elke client die verbinding maakt, moet worden geconfigureerd met behulp van de instellingen in de configuratiebestanden.

Voor meer informatie over point-to-site-verbindingen leest u [About Point-to-Site connections](point-to-site-about.md) (Over point-to-site-verbindingen).

## <a name="before-you-begin"></a>Voordat u begint

* Controleer of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial).
* Installeer de meest recente versie van de PowerShell-cmdlets van Resource Manager. Zie [How to install and configure Azure PowerShell](/powershell/azure/overview) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="example"></a>Voorbeeldwaarden

U kunt de volgende voorbeeldwaarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel. De variabelen worden ingesteld in sectie [1](#declare) van dit artikel. U kunt de stappen gebruiken als een overzicht en de waarden ongewijzigd gebruiken, of u kunt ze wijzigen zodat ze overeenkomen met uw omgeving.

* **Naam: VNet1**
* **Adresruimte: 192.168.0.0/16** en **10.254.0.0/16**<br>In dit voorbeeld wordt meer dan één adresruimte gebruikt om te laten zien dat deze configuratie met meerdere adresruimten werkt. Meerdere adresruimten zijn echter niet vereist voor deze configuratie.
* **Subnetnaam: FrontEnd**
  * **Subnetadresbereik: 192.168.1.0/24**
* **Subnetnaam: BackEnd**
  * **Subnetadresbereik: 10.254.1.0/24**
* **Subnetnaam: GatewaySubnet**<br>De naam van het subnet *GatewaySubnet* is verplicht voor een goede werking van de VPN-gateway.
  * **Adresbereik GatewaySubnet: 192.168.200.0/24** 
* **VPN-clientadresgroep: 172.16.201.0/24**<br>VPN-clients die verbinding maken met het VNet via deze punt-naar-site-verbinding, ontvangen een IP-adres van de VPN-clientadresgroep.
* **Abonnement:** controleer of u het juiste abonnement hebt in het geval u er meer dan één hebt.
* **Resourcegroep: TestRG**
* **Locatie: VS - oost**
* **DNS-server: IP-adres** van de DNS-server die u wilt gebruiken voor naamomzetting. (optioneel)
* **Gatewaynaam: Vnet1GW**
* **Openbare IP-naam: VNet1GWPIP**
* **VPNType: op route gebaseerd** 

## <a name="declare"></a>1. Aanmelden en variabelen instellen

In deze sectie meldt u zich aan en declareert u de waarden die voor deze configuratie worden gebruikt. De opgegeven waarden worden in de voorbeeldscripts gebruikt. Wijzig de waarden zodat ze overeenkomen met uw omgeving. U kunt ook de gedeclareerde waarden gebruiken en de stappen bij wijze van oefening doorlopen.

1. Open de PowerShell-console met verhoogde bevoegdheden en meld u aan bij uw Azure-account. Deze cmdlet vraagt u om de aanmeldingsreferenties. Na het aanmelden, worden de instellingen van uw account gedownload zodat ze beschikbaar zijn voor Azure PowerShell.

  ```powershell
  Login-AzureRmAccount
  ```
2. Haal een lijst met uw Azure-abonnementen op.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Geef het abonnement op dat u wilt gebruiken.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
4. Declareer de waarden die u wilt gebruiken. Gebruik het volgende voorbeeld, en vervang zo nodig de waarden door uw eigen waarden.

  ```powershell
  $VNetName  = "VNet1"
  $FESubName = "FrontEnd"
  $BESubName = "Backend"
  $GWSubName = "GatewaySubnet"
  $VNetPrefix1 = "192.168.0.0/16"
  $VNetPrefix2 = "10.254.0.0/16"
  $FESubPrefix = "192.168.1.0/24"
  $BESubPrefix = "10.254.1.0/24"
  $GWSubPrefix = "192.168.200.0/26"
  $VPNClientAddressPool = "172.16.201.0/24"
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## <a name="ConfigureVNet"></a>2. Een VNet configureren

1. Maak een resourcegroep.

  ```powershell
  New-AzureRmResourceGroup -Name $RG -Location $Location
  ```
2. Maak de subnetconfiguraties voor het virtuele netwerk, noem deze *FrontEnd*, *BackEnd* en *GatewaySubnet*. Deze voorvoegsels moeten deel uitmaken van de VNet-adresruimte die u hebt opgegeven.

  ```powershell
  $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
  $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
  $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
  ```
3. Maak het virtuele netwerk.

  In dit voorbeeld is de serverparameter-DnsServer optioneel. Het opgeven van een waarde betekent niet dat er een nieuwe DNS-server wordt gemaakt. Het IP-adres van de DNS-server dat u opgeeft, moet het adres zijn van een DNS-server die de namen kan omzetten voor de resources waarmee u verbinding maakt vanuit uw VNet. In dit voorbeeld wordt een privé-IP-adres gebruikt, maar het is zeer onwaarschijnlijk dat dit het IP-adres van uw DNS-server is. Zorg ervoor dat u uw eigen waarden gebruikt. De waarde die u opgeeft, wordt gebruikt door de bronnen die u in het VNet implementeert, niet door de P2S-verbinding of de VPN-client.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
  ```
4. Geef de variabelen op voor het virtuele netwerk dat u hebt gemaakt.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  ```
5. Een VPN Gateway moet een openbaar IP-adres hebben. U vraagt eerst de resource van het IP-adres aan en verwijst hier vervolgens naar bij het maken van uw virtuele netwerkgateway. Het IP-adres wordt dynamisch aan de resource toegewezen wanneer de VPN Gateway wordt gemaakt. VPN Gateway ondersteunt momenteel alleen *dynamische* toewijzing van openbare IP-adressen. U kunt geen toewijzing van een statisch openbaar IP-adres aanvragen. Dit betekent echter niet dat het IP-adres verandert nadat het aan uw VPN-gateway is toegewezen. Het openbare IP-adres verandert alleen wanneer de gateway wordt verwijderd en opnieuw wordt gemaakt. Het verandert niet wanneer de grootte van uw VPN Gateway verandert, wanneer deze gateway opnieuw wordt ingesteld of wanneer andere interne onderhoudswerkzaamheden of upgrades worden uitgevoerd.

  Vraag een dynamisch toegewezen openbaar IP-adres aan.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```

## <a name="creategateway"></a>3. De VPN-gateway maken

Configureer en maak de virtuele netwerkgateway voor uw VNet.

* -GatewayType moet **Vpn** zijn en -VpnType moet **RouteBased** zijn.
* -VpnClientProtocol wordt gebruikt om de soorten tunnels op te geven die u wilt inschakelen. De twee tunnelopties zijn **SSTP** en **IKEv2**. U kunt een van beide of beide inschakelen. Als u ze beide wilt inschakelen, geeft u beide namen op, gescheiden door een komma. De Strongswan-client op Android en Linux en de systeemeigen IKEv2 VPN-client op iOS en OS x gebruiken alleen de IKEv2-tunnel om verbinding te maken. Windows-clients proberen eerst IKEv2. Als daarmee geen verbinding kan worden gemaakt, vallen ze terug op SSTP.
* Een VPN-gateway wordt binnen maximaal 45 minuten voltooid. De daadwerkelijke instelduur hangt af van de [gateway-SKU](vpn-gateway-about-vpn-gateway-settings.md) die u selecteert. In dit voorbeeld wordt IKEv2 gebruikt.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocol "IKEv2"
```

## <a name="addresspool"></a>4. De VPN-clientadrespool toevoegen

Nadat de VPN-gateway is gemaakt, kunt u de VPN-clientadrespool toevoegen. De VPN-clientadrespool is het bereik vanwaaruit de VPN-clients een IP-adres ontvangen wanneer er verbinding wordt gemaakt. Gebruik een privé-IP-adresbereik dat niet overlapt met de on-premises locatie waarvanaf u verbinding maakt of met het VNet waarmee u verbinding wilt maken. In dit voorbeeld wordt de VPN-clientadrespool in stap 1 opgegeven als [variabele](#declare).

```powershell
$Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
```

## <a name="Certificates"></a>5. Certificaten genereren

Certificaten worden door Azure gebruikt om VPN-clients voor punt-naar-site-VPN's te verifiëren. U uploadt de informatie van de openbare sleutel over het basiscertificaat naar Azure. De openbare sleutel wordt dan beschouwd als 'vertrouwd'. Clientcertificaten moeten worden gegenereerd op basis van het vertrouwde basiscertificaat en geïnstalleerd op elke clientcomputer. Dit gebeurt in het certificaatarchief Certificates-Current user/Personal. Het certificaat wordt gebruikt om de client te verifiëren bij het maken van verbinding met het VNet. 

Als u zelfondertekende certificaten gebruikt, moeten ze worden gemaakt met behulp van specifieke parameters. U kunt een zelfondertekend certificaat maken met behulp van de instructies voor [PowerShell en Windows 10](vpn-gateway-certificates-point-to-site.md). Als u niet beschikt over Windows 10, kunt u ook [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) gebruiken. Het is belangrijk dat u de stappen in de instructies opvolgt bij het maken van zelfondertekende basiscertificaten en clientcertificaten. Anders zijn de certificaten die u maakt, niet compatibel met P2S-verbindingen en treedt er een verbindingsfout op.

### <a name="cer"></a>1. Het CER-bestand voor het basiscertificaat verkrijgen

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]


### <a name="generate"></a>2. Een clientcertificaat genereren

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>6. Gegevens van openbare sleutel van basiscertificaat uploaden

Controleer of het maken van de VPN-gateway is voltooid. Als dat het geval is, uploadt u het CER-bestand (met de gegevens van de openbare sleutel) voor een vertrouwd basiscertificaat naar Azure. Nadat het CER-bestand is geüpload, kan Azure daarmee clients met een geïnstalleerd clientcertificaat (gemaakt op basis van het vertrouwde basiscertificaat) verifiëren. Indien nodig kunt u later aanvullende vertrouwde basiscertificaatbestanden uploaden (maximaal 20).

1. Declareer de variabele voor uw certificaatnaam, waarbij u de waarde vervangt door uw eigen waarde.

  ```powershell
  $P2SRootCertName = "P2SRootCert.cer"
  ```
2. Vervang het pad naar het bestand door uw eigen pad en voer vervolgens de cmdlets uit.

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
  ```
3. Upload de gegevens van de openbare sleutel naar Azure. Nadat de certificaatgegevens zijn geüpload, wordt het certificaat als een vertrouwd basiscertificaat beschouwd in Azure.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
  ```

## <a name="clientcertificate"></a>7. Een geëxporteerd clientcertificaat installeren

Als u een P2S-verbinding wilt maken vanaf een andere clientcomputer dan de computer die u gebruikt om de clientcertificaten te genereren, moet u een clientcertificaat installeren. Wanneer u een clientcertificaat installeert, hebt u het wachtwoord nodig dat is gemaakt tijdens het exporteren van het clientcertificaat.

Zorg ervoor dat het certificaat is geëxporteerd als een PFX-bestand, samen met de volledige certificaatketen (dit is de standaardinstelling). Anders zijn de gegevens van het basiscertificaat niet aanwezig op de clientcomputer en kan de client niet worden geverifieerd. 

Zie voor de installatiestappen [Install a client certificate](point-to-site-how-to-vpn-client-install-azure-cert.md) (Een clientcertificaat installeren).

## <a name="clientconfig"></a>8. De systeemeigen VPN-client configureren

De configuratiebestanden van de VPN-clients bevatten de instellingen voor het configureren van apparaten om verbinding te maken met een VNet via een P2S-verbinding. Zie voor instructies voor het genereren en installeren van VPN-clientconfiguratiebestanden [Create and install VPN client configuration files for native Azure certificate authentication P2S configurations](point-to-site-vpn-client-configuration-azure-cert.md) (VPN-clientconfiguratiebestanden voor P2S-configuraties voor systeemeigen Azure-certificaatverificatie maken en installeren).

## <a name="connect"></a>9. Verbinding maken met Azure

### <a name="to-connect-from-a-windows-vpn-client"></a>Verbinding maken vanaf een Windows-VPN-client

1. Als u met uw VNet wilt verbinden, gaat u op de clientcomputer naar de VPN-verbindingen en zoekt u de VPN-verbinding die u hebt gemaakt. Deze heeft dezelfde naam als het virtuele netwerk. Klik op **Verbinden**. Er verschijnt mogelijk een pop-upbericht dat verwijst naar het certificaat. Klik op **Doorgaan** om verhoogde bevoegdheden te gebruiken. 
2. Klik op de pagina **Verbindingsstatus** op **Verbinden** om de verbinding te starten. Als het scherm **Certificaat selecteren** wordt geopend, controleert u of het weergegeven clientcertificaat het certificaat is dat u voor de verbinding wilt gebruiken. Als dat niet het geval is, gebruikt u de pijl-omlaag om het juiste certificaat te selecteren en klikt u op **OK**.

  ![VPN-client maakt verbinding met Azure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. De verbinding is tot stand gebracht.

  ![Verbinding tot stand gebracht](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

#### <a name="troubleshooting-windows-client-p2s-connections"></a>Problemen met P2S-verbindingen in Windows-clients oplossen

[!INCLUDE [client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="to-connect-from-a-mac-vpn-client"></a>Verbinding maken vanaf een Mac-VPN-client

Zoek in het dialoogvenster Netwerk het clientprofiel dat u wilt gebruiken en klik op **Verbinding maken**.

  ![Mac-verbinding](./media/vpn-gateway-howto-point-to-site-rm-ps/applyconnect.png)

## <a name="verify"></a>De verbinding verifiëren

Deze instructies zijn van toepassing op Windows-clients.

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

## <a name="connectVM"></a>Verbinding maken met een virtuele machine

Deze instructies zijn van toepassing op Windows-clients.

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="addremovecert"></a>Een basiscertificaat toevoegen of verwijderen

U kunt vertrouwde basiscertificaat toevoegen in en verwijderen uit Azure. Wanneer u een basiscertificaat verwijdert, kunnen clients met een certificaat dat is gegenereerd op basis van dat basiscertificaat niet worden geverifieerd, en kunnen ze dus geen verbinding maken. Als u wilt dat clients kunnen worden geverifieerd en verbinding kunnen maken, moet u een nieuw clientcertificaat installeren dat is gegenereerd op basis van een basiscertificaat dat wordt vertrouwd (is geüpload) in Azure.

### <a name="addtrustedroot"></a>Een vertrouwd basiscertificaat toevoegen

U kunt maximaal 20 CER-basiscertificaatbestanden toevoegen aan Azure. Volg de volgende stappen om een basiscertificaat toe te voegen:

#### <a name="certmethod1"></a>Methode 1

Dit is de meest efficiënte methode voor het uploaden van een basiscertificaat.

1. Bereid het CER-bestand voor dat u wilt uploaden:

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert3.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64_3 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64_3
  ```
2. Upload het bestand. U kunt slechts één bestand tegelijk uploaden.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64_3
  ```

3. Controleren of het certificaatbestand is geüpload:

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

#### <a name="certmethod2"></a>Methode 2

Deze methode heeft meer stappen dan methode 1, maar levert hetzelfde resultaat. Gebruik deze methode als u de gegevens van het certificaat wilt weergeven.

1. Maak en bereid het nieuwe basiscertificaat voor dat u aan Azure gaat toevoegen. Exporteer de openbare sleutel als een met Base-64 gecodeerd X.509-certificaat (.CER) en open het bestand met een teksteditor. Kopieer de waarden, zoals weergegeven in het volgende voorbeeld:

  ![certificaat](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

  > [!NOTE]
  > Zorg er bij het kopiëren van de certificaatgegevens voor dat u de tekst als één ononderbroken regel kopieert zonder regelterugloop of regelinvoer. Mogelijk moet u de weergave in de teksteditor wijzigen naar Symbool weergeven/Alle tekens weergeven om de regelterugloop en regelinvoer te zien.
  >
  >

2. Geef de certificaatnaam en sleutelgegevens op als een variabele. Vervang de gegevens door uw eigen gegevens, zoals weergegeven in het volgende voorbeeld:

  ```powershell
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
3. Voeg het nieuwe basiscertificaat toe. U kunt slechts één certificaat tegelijk toevoegen.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
4. U kunt controleren of het nieuwe certificaat correct is toegevoegd met behulp van het volgende voorbeeld:

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

### <a name="removerootcert"></a>Een basiscertificaat verwijderen

1. Declareer de variabelen.

  ```powershell
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
2. Verwijder het certificaat.

  ```powershell
  Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
3. Gebruik het volgende voorbeeld om te controleren of het certificaat is verwijderd.

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

## <a name="revoke"></a>Een clientcertificaat intrekken

U kunt clientcertificaten intrekken. Met de certificaatintrekkingslijst kunt u selectief punt-naar-site-verbinding weigeren op basis van afzonderlijke clientcertificaten. Dit is anders van het verwijderen van een vertrouwd basiscertificaat. Als u een vertrouwd .cer-basiscertificaat uit Azure verwijdert, wordt de toegang ingetrokken voor alle clientcertificaten die zijn gegenereerd/ondertekend door het ingetrokken basiscertificaat. Als u in plaats van een basiscertificaat een clientcertificaat intrekt, kunt u de certificaten die zijn gegenereerd op basis van het basiscertificaat, blijven gebruiken voor verificatie.

De algemene procedure is het basiscertificaat te gebruiken om de toegang te beheren op het team- of organisatieniveau, terwijl u ingetrokken clientcertificaten gebruikt voor nauwkeuriger toegangsbeheer bij afzonderlijke gebruikers.

### <a name="revokeclientcert"></a>Een clientcertificaat intrekken

1. Haal de vingerafdruk voor het clientcertificaat op. Zie voor meer informatie [De vingerafdruk van een certificaat ophalen](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopieer de gegevens naar een teksteditor en verwijder alle spaties, zodat u een doorlopende tekenreeks overhoudt. Deze reeks wordt in de volgende stap gedeclareerd als een variabele.
3. Declareer de variabelen. Zorg ervoor dat u de vingerafdruk declareert die u in de vorige stap hebt opgehaald.

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
4. Voeg de vingerafdruk toe aan de lijst met ingetrokken certificaten. U ziet Geslaagd als de vingerafdruk is toegevoegd.

  ```powershell
  Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
  -Thumbprint $RevokedThumbprint1
  ```
5. Controleer of de vingerafdruk is toegevoegd aan de certificaatintrekkingslijst.

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```
6. Nadat de vingerafdruk is toegevoegd, kan het certificaat niet langer worden gebruikt om te verbinden. Clients die verbinding proberen te maken met het certificaat, ontvangen een bericht waarin wordt gemeld dat het certificaat niet meer geldig is.

### <a name="reinstateclientcert"></a>Een clientcertificaat opnieuw activeren

U kunt een clientcertificaat opnieuw activeren door de vingerafdruk te verwijderen uit de lijst met ingetrokken clientcertificaten.

1. Declareer de variabelen. Zorg ervoor dat u de juiste vingerafdruk declareert voor het certificaat dat u opnieuw wilt activeren.

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
2. Verwijder de vingerafdruk van het certificaat uit de lijst met ingetrokken certificaten.

  ```powershell
  Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
  ```
3. Controleer of de vingerafdruk is verwijderd uit de lijst met ingetrokken vingerafdrukken.

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```

## <a name="faq"></a>Veelgestelde vragen over punt-naar-site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie. Zie [Azure and Linux VM Network Overview](../virtual-machines/linux/azure-vm-network-overview.md) (Overzicht van Azure- en Linux-VM-netwerken) voor meer informatie over netwerken en virtuele machines.
