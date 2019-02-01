---
title: 'Maken en installeren van P2S-VPN-clientconfiguratiebestanden voor Azure-certificaatverificatie: Azure | Microsoft Docs'
description: Maken en installeren van Windows, Linux, Linux (strongSwan) en Mac OS X-VPN-clientconfiguratiebestanden voor P2S-certificaatverificatie.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/18/2019
ms.author: cherylmc
ms.openlocfilehash: 0f834c88a22aca52a861309681ea0da204b2a552
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55507992"
---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-p2s-configurations"></a>Maken en VPN-clientconfiguratiebestanden voor native Azure certificate authentication P2S-configuraties installeren

VPN-clientconfiguratiebestanden zijn opgenomen in een zip-bestand. Configuratiebestanden bevatten de instellingen die vereist zijn voor een systeemeigen Windows-, Mac IKEv2 VPN- of Linux-clients verbinding maken met een VNet via punt-naar-Site-verbindingen die gebruikmaken van systeemeigen Azure certificaatverificatie.

Client-configuratiebestanden zijn specifiek voor de VPN-configuratie voor het VNet. Als er wijzigingen in de punt-naar-Site VPN-configuratie na het genereren van de VPN-client-configuratiebestanden, zoals het VPN-protocol-type of verificatie, moet u voor het genereren van nieuwe VPN-clientconfiguratiebestanden voor uw apparaten van gebruikers. 

* Voor meer informatie over point-to-site-verbindingen leest u [About Point-to-Site VPN](point-to-site-about.md) (Over point-to-site-VPN).
* Zie voor instructies OpenVPN [OpenVPN configureren voor P2S](vpn-gateway-howto-openvpn.md) en [OpenVPN configureren clients](vpn-gateway-howto-openvpn-clients.md).

>[!IMPORTANT]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="generate"></a>Configuratiebestanden voor VPN-clients genereren

Voordat u begint, zorg ervoor dat alle gebruikers die verbinding maken een geldig certificaat geïnstalleerd op het apparaat van de gebruiker. Zie voor meer informatie over het installeren van een clientcertificaat [een clientcertificaat installeren](point-to-site-how-to-vpn-client-install-azure-cert.md).

U kunt genereren met behulp van PowerShell-clientconfiguratiebestanden of met behulp van de Azure-portal. Een van beide methoden retourneert het dezelfde zip-bestand. Decomprimeer het bestand om de volgende mappen weer te geven:

  * **WindowsAmd64** en **WindowsX86**, die het installatieprogramma voor Windows 32-bits en 64-bits pakketten, respectievelijk bevatten. De **WindowsAmd64** installer-pakket is voor alle ondersteunde op 64-bits Windows-clients niet alleen Amd.
  * **Algemene**, bevat algemene informatie gebruikt voor het maken van de configuratie van uw eigen VPN-client. De algemene map is opgegeven als IKEv2 of SSTP + IKEv2 is geconfigureerd op de gateway. Als er slechts SSTP is geconfigureerd, klikt u vervolgens is de algemene map niet aanwezig.

### <a name="zipportal"></a>Genereren van bestanden met behulp van de Azure-portal

1. Navigeer naar de virtuele netwerkgateway voor het virtuele netwerk dat u verbinding wilt maken in de Azure-portal.
2. Klik op de pagina virtual network gateway **punt-naar-site-configuratie**.
3. Aan de bovenkant van de pagina punt-naar-site-configuratie, klikt u op **VPN-client downloaden**. Het duurt een paar minuten voor de client voor het genereren van het configuratiepakket.
4. Uw browser geeft aan dat het zip-bestand van een client-configuratie beschikbaar is. De naam ervan is dezelfde naam als uw gateway. Decomprimeer het bestand om de mappen weer te geven.

### <a name="zipps"></a>Genereren van bestanden met behulp van PowerShell

1. Bij genereren van het VPN-clientconfiguratie van bestanden, de waarde voor '-AuthenticationMethod' is 'EapTls'. Genereren van de configuratiebestanden van de VPN-client met de volgende opdracht:

  ```powershell
  $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls"

  $profile.VPNProfileSASUrl
  ```
2. Kopieer de URL naar uw browser naar het zip-bestand downloaden en Decomprimeer het bestand om de mappen weer te geven.

## <a name="installwin"></a>Windows

Als de versie overeenkomt met de architectuur voor de client, kunt u hetzelfde configuratiepakket voor VPN-client op elke Windows-clientcomputer. Voor een lijst van client-besturingssystemen die worden ondersteund, Zie de sectie punt-naar-Site van de [Veelgestelde vragen over VPN-Gateway](vpn-gateway-vpn-faq.md#P2S).

>[!NOTE]
>U moet beheerdersrechten hebben op de Windows-clientcomputer van waaruit u verbinding wilt maken.
>
>

Gebruik de volgende stappen uit om te configureren van de systeemeigen Windows VPN-client voor verificatie via certificaat:

1. Selecteer de VPN-clientconfiguratiebestanden die overeenkomen met de architectuur van de Windows-computer. Kies voor een architectuur met 64-bits processor het installatiepakket ‘VpnClientSetupAmd64’. Kies voor een architectuur met 32-bits processor het installatiepakket ‘VpnClientSetupAmdX86’. 
2. Dubbelklik op het pakket om het te installeren. Als u een SmartScreen-melding ziet, klikt u op **Meer info** en vervolgens op **Toch uitvoeren**.
3. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De VPN-verbinding bevat de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. 
4. Controleer of u een clientcertificaat op de clientcomputer hebt geïnstalleerd voordat u verbinding probeert te maken. Een clientcertificaat is vereist voor verificatie als u het systeemeigen verificatietype met Azure-certificaat gebruikt. Zie voor meer informatie over het genereren van certificaten, [certificaten genereren](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert). Zie voor meer informatie over het installeren van een clientcertificaat [een clientcertificaat installeren](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="installmac"></a>Mac (OS X)

 U moet de systeemeigen IKEv2 VPN-client handmatig configureren op elke Mac die verbinding met Azure zal maken. Azure biedt geen mobileconfig-bestand voor verificatie van systeemeigen Azure-certificaten. De **algemene** bevat alle informatie die u nodig hebt voor de configuratie. Als u de map Generic niet ziet in uw download, is IKEv2 waarschijnlijk niet geselecteerd als tunneltype. Genereer het zip-bestand opnieuw om de map Generic te verkrijgen nadat IKEv2 is geselecteerd.<br>De map Generic bevat de volgende bestanden:

* **VpnSettings.xml**, die belangrijke instellingen, zoals het adres en tunnel servertype bevat. 
* **VpnServerRoot.cer**, die het basiscertificaat dat is vereist voor het valideren van de Azure VPN-Gateway tijdens de installatie van de P2S-verbinding bevat.

Gebruik de volgende stappen uit het configureren van de systeemeigen VPN-client op Mac voor verificatie via certificaat. U moet deze stappen op elke Mac-computer maakt verbinding met Azure:

1. Importeren van de **VpnServerRoot** basiscertificaat op uw Mac. Dit kan worden gedaan door het bestand kopiëren naar uw Mac en erop te dubbelklikken.  
Klik op **toevoegen** om te importeren.

  ![certificaat toevoegen](./media/point-to-site-vpn-client-configuration-azure-cert/addcert.png)
  
    >[!NOTE]
    >Te dubbelklikken op het certificaat wellicht niet weergegeven de **toevoegen** dialoogvenster, maar het certificaat is geïnstalleerd in de juiste opslag. U kunt het certificaat in de sleutelketen aanmelding onder de categorie certificaten controleren.
    >
  
2. Controleer of dat u een clientcertificaat dat is uitgegeven door het basiscertificaat dat u hebt geüpload naar Azure wanneer u u P2S-instellingen geconfigureerd hebt geïnstalleerd. Dit wijkt af van de VPNServerRoot die u in de vorige stap hebt geïnstalleerd. Het clientcertificaat wordt gebruikt voor verificatie en is vereist. Zie voor meer informatie over het genereren van certificaten, [certificaten genereren](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert). Zie voor meer informatie over het installeren van een clientcertificaat [een clientcertificaat installeren](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. Open de **netwerk** dialoogvenster onder **Netwerkvoorkeuren** en klikt u op **'+'** te maken van een nieuwe VPN-client verbinding-profiel voor een P2S-verbinding met het Azure-VNet.

  De **Interface** waarde is 'VPN' en **VPN-Type** waarde is 'IKEv2'. Geef een naam voor het profiel in de **servicenaam** veld en klik vervolgens op **maken** het profiel voor VPN-client verbinding maken.

  ![netwerk](./media/point-to-site-vpn-client-configuration-azure-cert/network.png)
4. In de **algemene** map van de **VpnSettings.xml** bestand, kopieert u de **VpnServer** tagwaarde. Plak deze waarde in de **serveradres** en **externe ID** velden van het profiel.

  ![de gegevens van uw](./media/point-to-site-vpn-client-configuration-azure-cert/server.png)
5. Klik op **verificatie-instellingen** en selecteer **certificaat**. 

  ![verificatie-instellingen](./media/point-to-site-vpn-client-configuration-azure-cert/authsettings.png)
6. Klik op **selecteren...** om te kiezen het clientcertificaat dat u wilt gebruiken voor verificatie. Dit is het certificaat dat u in stap 2 hebt geïnstalleerd.

  ![certificaat](./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png)
7. **Kies een identiteit** geeft een lijst van certificaten voor u om uit te kiezen. Het juiste certificaat selecteren en klik vervolgens op **doorgaan**.

  ![identity](./media/point-to-site-vpn-client-configuration-azure-cert/identity.png)
8. In de **lokale ID** veld, geeft u de naam van het certificaat (uit stap 6). In dit voorbeeld is 'ikev2Client.com'. Klik vervolgens op **toepassen** knop de wijzigingen op te slaan.

  ![toepassen](./media/point-to-site-vpn-client-configuration-azure-cert/applyconnect.png)
9. Op de **netwerk** dialoogvenster, klikt u op **toepassen** alle wijzigingen op te slaan. Klik vervolgens op **Connect** om te beginnen de P2S-verbinding met het Azure-VNet.

## <a name="linuxgui"></a>Linux (strongSwan GUI)

### <a name="extract-the-key-and-certificate"></a>De sleutel en het certificaat ophalen

Voor strongSwan moet u de sleutel en het certificaat ophalen uit het clientcertificaat (PFX-bestand) en slaat u ze op afzonderlijke .pem-bestanden.
Volg de onderstaande stappen:

1. Download en installeer de OpenSSL van [OpenSSL](https://www.openssl.org/source/).
2. Open een opdrachtregelvenster en wijzig in de map waarin u OpenSSL, bijvoorbeeld geïnstalleerd ' c:\OpenSLL-Win64\bin\'.
3. Voer de volgende opdracht om de persoonlijke sleutel uitpakken en sla deze op een nieuw bestand met de naam 'privatekey.pem' vanuit uw clientcertificaat:

  ```
  C:\ OpenSLL-Win64\bin> openssl pkcs12 -in clientcert.pfx -nocerts -out privatekey.pem -nodes
  ```
4.  Voer nu de volgende opdracht voor het extraheren van het openbare certificaat en sla deze op een nieuw bestand:

  ```
  C:\ OpenSLL-Win64\bin> openssl pkcs12 -in clientcert.pfx -nokeys -out publiccert.pem -nodes
  ```

### <a name="install"></a>Installeren en configureren

De volgende instructies zijn gemaakt via strongSwan 5.5.1 op Ubuntu 17.0.4. Ubuntu 16.0.10 biedt geen ondersteuning voor strongSwan GUI. Als u gebruiken van Ubuntu 16.0.10 wilt, u moet gebruiken de [vanaf de opdrachtregel](#linuxinstallcli). De onderstaande voorbeelden mogelijk niet overeenkomt met de schermen die u, afhankelijk van uw versie van Linux- en strongSwan ziet.

1. Open de **Terminal** voor het installeren van **strongSwan** en de netwerkbeheerder met de opdracht in het voorbeeld. Als u een foutbericht dat gerelateerd ontvangt aan *libcharon-extra-invoegtoepassingen*, vervang deze door 'strongswan-invoegtoepassing-eap-mschapv2'.

  ```
  sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
  ```
2. Selecteer de **netwerkbeheerder** pictogram (omhoog-pijl/pijl-omlaag) Selecteer **verbindingen bewerken**.

  ![verbindingen bewerken](./media/point-to-site-vpn-client-configuration-azure-cert/editconnections.png)
3. Klik op de **toevoegen** knop om een nieuwe verbinding te maken.

  ![een verbinding toevoegen](./media/point-to-site-vpn-client-configuration-azure-cert/addconnection.png)
4. Selecteer **IPsec/IKEv2 (strongswan)** uit de vervolgkeuzelijst en klik vervolgens op **maken**. U kunt de naam van de verbinding in deze stap wijzigen.

  ![Kies een verbindingstype](./media/point-to-site-vpn-client-configuration-azure-cert/choosetype.png)
5. Open de **VpnSettings.xml** -bestand uit de **algemene** map die is opgenomen in de gedownloade client-configuratiebestanden. De code met de naam zoeken **VpnServer** en kopieert u de naam, die met 'azuregateway' beginnen en eindigen met '. cloudapp.net'.

  ![naam van de kopieerbewerking](./media/point-to-site-vpn-client-configuration-azure-cert/vpnserver.png)
6. Plak de naam in de **adres** veld van uw nieuwe VPN-verbinding in de **Gateway** sectie. Selecteer vervolgens het mappictogram aan het einde van de **certificaat** veld, Ga naar de **algemene** map en selecteer de **VpnServerRoot** bestand.
7. In de **Client** sectie van de verbinding voor **verificatie**, selecteer **certificaten/persoonlijk sleutelpaar**. Voor **certificaat** en **privésleutel**, kiest u het certificaat en de persoonlijke sleutel die eerder zijn gemaakt. In **opties**, selecteer **aanvragen van een interne IP-adres**. Klik vervolgens op **toevoegen**.

  ![een interne IP-adres aanvragen](./media/point-to-site-vpn-client-configuration-azure-cert/inneripreq.png)
8. Klik op de **netwerkbeheerder** pictogram (omhoog-pijl/pijl-omlaag) en houd de muis boven **VPN-verbindingen**. U ziet dat de VPN-verbinding die u hebt gemaakt. Klik op de verbinding tot stand brengt.

## <a name="linuxinstallcli"></a>Linux (strongSwan CLI)

### <a name="install-strongswan"></a>Install strongSwan

U kunt de volgende CLI-opdrachten gebruiken of gebruik de strongSwan-stappen in de [GUI](#install) strongSwan installeren.

1. `apt-get install strongswan-ikev2 strongswan-plugin-eap-tls`
2. `apt-get install libstrongswan-standard-plugins`

### <a name="install-and-configure"></a>Installeren en configureren

1. Download het VPNClient-pakket van Azure portal.
2. Pak het bestand.
3. Uit de **algemene** map, de VpnServerRoot.cer naar /etc/ipsec.d/cacerts verplaatsen of kopiëren.
4. Kopiëren of verplaatsen van cp client.p12 naar /etc/ipsec.d/private/. Dit bestand is een clientcertificaat voor Azure VPN-Gateway.
5. Open VpnSettings.xml bestand en kopieer de <VpnServer> waarde. U gebruikt deze waarde in de volgende stap.
6. Wijzig de waarden in het voorbeeld hieronder en klik vervolgens in het voorbeeld in de configuratie /etc/ipsec.conf toevoegen.
  
  ```
  conn azure
  keyexchange=ikev2
  type=tunnel
  leftfirewall=yes
  left=%any
  leftauth=eap-tls
  leftid=%client # use the DNS alternative name prefixed with the %
  right= Enter the VPN Server value here# Azure VPN gateway address
  rightid=% # Enter the VPN Server value here# Azure VPN gateway FQDN with %
  rightsubnet=0.0.0.0/0
  leftsourceip=%config
  auto=add
  ```
6. Het volgende toevoegen aan */etc/ipsec.secrets*.

  ```
  : P12 client.p12 'password' # key filename inside /etc/ipsec.d/private directory
  ```

7. Voer de volgende opdrachten uit:

  ```
  # ipsec restart
  # ipsec up azure
  ```

## <a name="next-steps"></a>Volgende stappen

Ga terug naar het artikel [uw P2S-configuratie voltooien](vpn-gateway-howto-point-to-site-rm-ps.md).

Zie de volgende artikelen voor het oplossen van P2S-verbindingen:

  * [Problemen met Azure point-to-site-verbindingen oplossen](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)
  * [Problemen met VPN-verbindingen van Mac OS X-VPN-clients oplossen](vpn-gateway-troubleshoot-point-to-site-osx-ikev2.md)
