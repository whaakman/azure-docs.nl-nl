---
title: 'Maken en VPN-clientconfiguratiebestanden voor P2S-RADIUS-verbindingen te installeren: PowerShell: Azure | Microsoft Docs'
description: Configuratiebestanden voor verbindingen die gebruikmaken van RADIUS-verificatie voor Windows, Mac OS X en Linux-VPN-client maken.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/07/2018
ms.author: cherylmc
ms.openlocfilehash: 6d21a5bceab2d5dada79ec4c694cdf12f0acac48
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56329636"
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>Maken en VPN-clientconfiguratiebestanden voor P2S-RADIUS-verificatie installeren

Als u wilt verbinding maken met een virtueel netwerk via punt-naar-site (P2S), moet u de client-apparaat waarmee u verbinding van maken kunt configureren. U kunt P2S-VPN-verbindingen maken van Windows, Mac OS X en Linux-clientapparaten. 

Als u RADIUS-verificatie, er zijn meerdere verificatieopties: verificatie voor gebruikersnaam en wachtwoord, verificatie via certificaat en andere verificatietypen. De configuratie van de VPN-client verschilt voor elk type verificatie. Voor het configureren van de VPN-client, u configuratiebestanden voor clients die de vereiste instellingen bevatten. Dit artikel helpt u bij het maken en installeren van de configuratie van de VPN-client voor het type van de RADIUS-verificatie die u wilt gebruiken.

>[!IMPORTANT]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

De configuratiewerkstroom voor P2S-RADIUS-verificatie is als volgt:

1. [Instellen van de Azure VPN-gateway voor P2S-connectiviteit](point-to-site-how-to-radius-ps.md).
2. [Instellen van uw RADIUS-server voor verificatie](point-to-site-how-to-radius-ps.md#radius). 
3. **De configuratie van de VPN-client voor de optie voor verificatie van uw keuze downloaden en deze gebruiken voor het instellen van de VPN-client** (in dit artikel).
4. [Voltooi uw P2S-configuratie en verbinding maken met](point-to-site-how-to-radius-ps.md).

>[!IMPORTANT]
>Als er wijzigingen in de configuratie van de punt-naar-site-VPN na het genereren van het VPN-client configuratieprofiel, zoals het VPN-protocol-type of verificatie, moet u genereren en installeren van de configuratie van een nieuwe VPN-client op apparaten van uw gebruikers.
>
>

Als u wilt gebruiken in de secties in dit artikel, eerst bepalen welk type verificatie dat u wilt gebruiken: gebruikersnaam en wachtwoord, certificaat of andere typen verificatie. Elke sectie bevat stappen voor Windows, Mac OS X en Linux (beschikbaar op dit moment beperkt stappen).

## <a name="adeap"></a>Verificatie voor gebruikersnaam en wachtwoord

Verificatie van de gebruikersnaam en wachtwoord voor het gebruik van Active Directory of Active Directory niet gebruikt, kunt u configureren. Met beide scenario's, zorg ervoor dat alle gebruikers die verbinding maken gebruikersnaam en wachtwoord-referenties die kunnen worden geverifieerd via RADIUS.

Als u verificatie voor gebruikersnaam en wachtwoord configureert, kunt u alleen een configuratie voor het EAP-MSCHAPv2 gebruikersnaam en wachtwoord-verificatieprotocol. In de opdrachten `-AuthenticationMethod` is `EapMSChapv2`.

### <a name="usernamefiles"></a> 1. De configuratiebestanden voor de VPN-client genereren

VPN-clientconfiguratiebestanden voor gebruik met verificatie voor gebruikersnaam en wachtwoord genereren. U kunt de configuratiebestanden van de VPN-client genereren met behulp van de volgende opdracht uit:

```powershell 
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Met de opdracht retourneert een koppeling. Kopieer en plak de koppeling naar een webbrowser om te downloaden **VpnClientConfiguration.zip**. Decomprimeer het bestand om de volgende mappen weer te geven: 
 
* **WindowsAmd64** en **WindowsX86**: Deze mappen bevatten de pakketten van het installatieprogramma voor Windows 64-bits en 32-bits, respectievelijk. 
* **Algemene**: Deze map bevat algemene informatie die u gebruiken voor het maken van de configuratie van uw eigen VPN-client. U hoeft deze map geen verificatie voor gebruikersnaam en wachtwoord.
* **Mac**: Als u tijdens het maken van de virtuele netwerkgateway IKEv2 hebt geconfigureerd, ziet u een map met de naam **Mac** die bevat een **mobileconfig** bestand. Dit bestand kunt u Mac-clients configureren.

Als u al hebt gemaakt client-configuratiebestanden, kunt u ze wel ophalen met behulp van de `Get-AzureRmVpnClientConfiguration` cmdlet. Maar als u wijzigingen aanbrengt aan uw P2S-VPN-configuratie, zoals het VPN-protocol-type of verificatietype, de configuratie automatisch wordt niet bijgewerkt. U moet uitvoeren de `New-AzureRmVpnClientConfiguration` cmdlet voor het maken van het downloaden van een nieuwe configuratie.

Als u wilt ophalen van eerder gegenereerde client-configuratiebestanden, gebruik de volgende opdracht:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```

### <a name="setupusername"></a> 2. VPN-clients configureren

U kunt de volgende VPN-clients configureren:

* [Windows](#adwincli)
* [Mac (OS X)](#admaccli)
* [Linux met behulp van strongSwan](#adlinuxcli)
 
#### <a name="adwincli"></a>Windows VPN-client installeren

Als de versie overeenkomt met de architectuur voor de client, kunt u hetzelfde configuratiepakket voor VPN-client op elke Windows-clientcomputer. Zie voor de lijst met client-besturingssystemen die worden ondersteund, de [Veelgestelde vragen over](vpn-gateway-vpn-faq.md#P2S).

Gebruik de volgende stappen uit om te configureren van de systeemeigen Windows VPN-client voor verificatie via certificaat:

1. Selecteer de VPN-clientconfiguratiebestanden die overeenkomen met de architectuur van de Windows-computer. Kies voor een 64-bits processor-architectuur, de **VpnClientSetupAmd64** installer-pakket. Kies voor een 32-bits processorarchitectuur de **VpnClientSetupX86** installer-pakket. 
2. Voor het installeren van het pakket, door erop te dubbelklikken. Als u een SmartScreen-pop-upvenster ziet, selecteert u **meer info** > **toch uitvoeren**.
3. Blader op de clientcomputer naar **netwerkinstellingen** en selecteer **VPN**. De VPN-verbinding bevat de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. 

#### <a name="admaccli"></a>Mac (OS X) VPN-client installeren

1. Selecteer de **VpnClientSetup mobileconfig** -bestand en dit te verzenden naar elk van de gebruikers. U kunt e-mailadres of een andere methode gebruiken.

2. Zoek de **mobileconfig** -bestand op de Mac.

   ![Locatie van het bestand mobileconfig](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)

3. Optionele stap - als u wilt opgeven van een aangepaste DNS-server, voeg de volgende regels aan de **mobileconfig** bestand:
```xml
    <key>DNS</key>
    <dict>
      <key>ServerAddresses</key>
        <array>
            <string>10.0.0.132</string>
        <array>
      <key>SupplementalMatchDomains</key>
        <array>
            <string>TestDomain.com</string>
        </array>
    </dict> 
```
4. Dubbelklik op het profiel wilt installeren, en selecteer **doorgaan**. Naam van het profiel is hetzelfde als de naam van het virtuele netwerk.

   ![Installatie van bericht](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
5. Selecteer **doorgaan** vertrouwen van de afzender van het profiel en doorgaan met de installatie.

   ![Bevestigingsbericht](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
6. Tijdens de profielinstallatie van hebt u de optie om op te geven van de gebruikersnaam en het wachtwoord voor de VPN-verificatie. Het is niet verplicht om in te voeren van deze informatie. Als u dit doet, wordt de informatie opgeslagen en automatisch gebruikt wanneer u een verbinding tot stand brengen. Selecteer **installeren** om door te gaan.

   ![Gebruikersnaam en wachtwoord in voor VPN](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
7. Voer een gebruikersnaam en wachtwoord voor de bevoegdheden die nodig zijn voor het profiel installeren op uw computer. Selecteer **OK**.

   ![Gebruikersnaam en wachtwoord in voor de profielinstallatie](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
8. Nadat het profiel is geïnstalleerd, wordt het weergegeven in de **profielen** in het dialoogvenster. U kunt ook openen vanuit het dialoogvenster **Systeemvoorkeuren**.

   ![In het dialoogvenster van '-mailprofielen'](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
9. Voor toegang tot de VPN-verbinding, opent u de **netwerk** in het dialoogvenster van **Systeemvoorkeuren**.

   ![De pictogrammen in Systeemvoorkeuren](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
10. De VPN-verbinding wordt weergegeven als **IkeV2-VPN-**. U kunt de naam wijzigen door het bijwerken van de **mobileconfig** bestand.

   ![Details voor de VPN-verbinding](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
11. Selecteer **verificatie-instellingen**. Selecteer **gebruikersnaam** in de lijst en voer uw referenties. Als u eerder hebt ingevoerd de referenties, klikt u vervolgens **gebruikersnaam** wordt automatisch gekozen in de lijst en de gebruikersnaam en wachtwoord worden vooraf ingevuld. Selecteer **OK** de instellingen op te slaan.

    ![Verificatie-instellingen](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
12. Klik in de **netwerk** in het dialoogvenster, selecteer **toepassen** de wijzigingen op te slaan. Als u wilt de verbinding tot stand brengen, selecteer **Connect**.

#### <a name="adlinuxcli"></a>Linux-VPN-client instellen via strongSwan

De volgende instructies zijn gemaakt via strongSwan 5.5.1 op Ubuntu 17.0.4. Werkelijke schermen mogelijk verschillen, afhankelijk van uw versie van Linux- en strongSwan.

1. Open de **Terminal** voor het installeren van **strongSwan** en de netwerkbeheerder met de opdracht in het voorbeeld. Als u een foutbericht dat gerelateerd ontvangt aan `libcharon-extra-plugins`, vervang deze door `strongswan-plugin-eap-mschapv2`.

   ```Terminal
   sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
   ```
2. Selecteer de **netwerkbeheerder** pictogram (omhoog-pijl/pijl-omlaag) en selecteer **verbindingen bewerken**.

   !['Verbindingen bewerken' selectie in Network Manager](./media/point-to-site-vpn-client-configuration-radius/EditConnection.png)
3. Selecteer de **toevoegen** knop om een nieuwe verbinding te maken.

   ![Knop 'Toevoegen' voor een verbinding](./media/point-to-site-vpn-client-configuration-radius/AddConnection.png)
4. Selecteer **IPsec/IKEv2 (strongswan)** uit de vervolgkeuzelijst en selecteer vervolgens **maken**. U kunt de naam van de verbinding in deze stap wijzigen.

   ![Het verbindingstype selecteren](./media/point-to-site-vpn-client-configuration-radius/AddIKEv2.png)
5. Open de **VpnSettings.xml** -bestand uit de **algemene** map van de gedownloade client-configuratiebestanden. De code met de naam zoeken `VpnServer` en kopieert u de naam, beginnend met `azuregateway` en eindigend met `.cloudapp.net`.

   ![Inhoud van het bestand VpnSettings.xml](./media/point-to-site-vpn-client-configuration-radius/VpnSettings.png)
6. Plak de naam in de **adres** veld van uw nieuwe VPN-verbinding in de **Gateway** sectie. Selecteer vervolgens het mappictogram aan het einde van de **certificaat** veld, Ga naar de **algemene** map en selecteer de **VpnServerRoot** bestand.
7. In de **Client** sectie van de verbinding, selecteer **EAP** voor **verificatie**, en voer uw gebruikersnaam en wachtwoord. Mogelijk hebt om te selecteren van het vergrendelingspictogram aan de rechterkant deze gegevens op te slaan. Selecteer vervolgens **Opslaan**.

   ![Instellingen van de verbinding bewerken](./media/point-to-site-vpn-client-configuration-radius/editconnectionsettings.png)
8. Selecteer de **netwerkbeheerder** pictogram (omhoog-pijl/pijl-omlaag) en houd de muis boven **VPN-verbindingen**. U ziet dat de VPN-verbinding die u hebt gemaakt. Als u wilt de verbinding tot stand brengen, selecteert u deze.

   !['VPN Radius' verbinding in Network Manager](./media/point-to-site-vpn-client-configuration-radius/ConnectRADIUS.png)

## <a name="certeap"></a>Verificatie via certificaat
 
Configuratiebestanden voor RADIUS-verificatie via certificaat dat het EAP-TLS-protocol gebruikt, kunt u VPN-client maken. Normaal gesproken wordt een enterprise-uitgegeven certificaat gebruikt voor het verifiëren van een gebruiker voor VPN-verbinding. Zorg ervoor dat alle gebruikers die verbinding maken een certificaat geïnstalleerd op hun apparaten hebben en dat de RADIUS-server kan worden gebruikt voor het valideren van het certificaat.

>[!NOTE]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

In de opdrachten `-AuthenticationMethod` is `EapTls`. Tijdens de verificatie via certificaat controleert de client de RADIUS-server door het certificaat te valideren. `-RadiusRootCert` is het cer-bestand dat het basiscertificaat dat wordt gebruikt voor het valideren van de RADIUS-server bevat.

Elk VPN-client-apparaat vereist een geïnstalleerd clientcertificaat. Een Windows-apparaat heeft soms meerdere clientcertificaten. Tijdens de verificatie, kan dit resulteren in een pop-updialoogvenster met een lijst met alle certificaten. De gebruiker moet vervolgens kiest u het certificaat te gebruiken. Het juiste certificaat kan worden gefilterd door op het basiscertificaat dat het clientcertificaat dat moet worden gekoppeld aan te geven. 

`-ClientRootCert` is het cer-bestand dat het basiscertificaat bevat. Er is een optionele parameter. Als het apparaat dat u verbinding maken wilt vanaf slechts één clientcertificaat heeft, moet u niet deze parameter opgeeft.

### <a name="certfiles"></a>1. De configuratiebestanden voor de VPN-client genereren

VPN-clientconfiguratiebestanden voor gebruik met verificatie via certificaat genereren. U kunt de configuratiebestanden van de VPN-client genereren met behulp van de volgende opdracht uit:
 
```powershell
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root> | fl
```

Met de opdracht retourneert een koppeling. Kopieer en plak de koppeling naar een webbrowser om te downloaden van VpnClientConfiguration.zip. Decomprimeer het bestand om de volgende mappen weer te geven:

* **WindowsAmd64** en **WindowsX86**: Deze mappen bevatten de pakketten van het installatieprogramma voor Windows 64-bits en 32-bits, respectievelijk. 
* **GenericDevice**: Deze map bevat algemene informatie die wordt gebruikt voor het maken van de configuratie van uw eigen VPN-client.

Als u al hebt gemaakt client-configuratiebestanden, kunt u ze wel ophalen met behulp van de `Get-AzureRmVpnClientConfiguration` cmdlet. Maar als u wijzigingen aanbrengt aan uw P2S-VPN-configuratie, zoals het VPN-protocol-type of verificatietype, de configuratie automatisch wordt niet bijgewerkt. U moet uitvoeren de `New-AzureRmVpnClientConfiguration` cmdlet voor het maken van het downloaden van een nieuwe configuratie.

Als u wilt ophalen van eerder gegenereerde client-configuratiebestanden, gebruik de volgende opdracht:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" | fl
```
 
### <a name="setupusername"></a> 2. VPN-clients configureren

U kunt de volgende VPN-clients configureren:

* [Windows](#certwincli)
* [Mac (OS X)](#certmaccli)
* Linux (ondersteund, geen artikel stappen nog)

#### <a name="certwincli"></a>Windows VPN-client installeren

1. Selecteer een configuratiepakket en installeer deze op het clientapparaat. Kies voor een 64-bits processor-architectuur, de **VpnClientSetupAmd64** installer-pakket. Kies voor een 32-bits processorarchitectuur de **VpnClientSetupX86** installer-pakket. Als u een SmartScreen-pop-upvenster ziet, selecteert u **meer info** > **toch uitvoeren**. U kunt het pakket ook opslaan en op andere clientcomputers installeren.
2. Elke client vereist een clientcertificaat voor verificatie. Het clientcertificaat installeren. Zie voor meer informatie over clientcertificaten [Client-certificaten voor punt-naar-site](vpn-gateway-certificates-point-to-site.md). Zie voor het installeren van een certificaat dat is gegenereerd, [een certificaat installeren op Windows-clients](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. Blader op de clientcomputer naar **netwerkinstellingen** en selecteer **VPN**. De VPN-verbinding bevat de naam van het virtuele netwerk waarmee verbinding wordt gemaakt.

#### <a name="certmaccli"></a>Mac (OS X) VPN-client installeren

U moet voor elk Mac-apparaat dat is verbonden met het Azure-netwerk een afzonderlijk profiel maken. Dit is omdat deze apparaten vereisen het certificaat voor verificatie dat moet worden opgegeven in het profiel. De **algemene** map bevat alle informatie die vereist zijn om een profiel te maken:

* **VpnSettings.xml** bevat belangrijke instellingen zoals servertype adres en -tunnel.
* **VpnServerRoot.cer** bevat het basiscertificaat dat is vereist voor het valideren van de VPN-gateway tijdens de installatie van de P2S-verbinding.
* **RadiusServerRoot.cer** bevat het basiscertificaat dat is vereist voor het valideren van de RADIUS-server tijdens de verificatie.

Gebruik de volgende stappen uit om te configureren van de systeemeigen VPN-client op een Mac voor verificatie via certificaat:

1. Importeren van de **VpnServerRoot** en **RadiusServerRoot** basiscertificaten op uw Mac. Elk bestand kopiëren naar uw Mac, dubbelklikt u op en selecteer vervolgens **toevoegen**.

   ![Het certificaat VpnServerRoot toe te voegen](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

   ![Het certificaat RadiusServerRoot toe te voegen](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Elke client vereist een clientcertificaat voor verificatie. Het clientcertificaat installeren op het clientapparaat.
3. Open de **netwerk** in het dialoogvenster onder **Netwerkvoorkeuren**. Selecteer **+** te maken van een nieuwe VPN-client verbinding-profiel voor een P2S-verbinding met de Azure-netwerk.

   De **Interface** waarde **VPN**, en de **VPN-Type** waarde **IKEv2**. Geef een naam voor het profiel in de **servicenaam** vak en selecteer vervolgens **maken** het profiel voor VPN-client verbinding maken.

   ![-Interface en de naam van informatie](./media/point-to-site-vpn-client-configuration-radius/network.png)
4. In de **algemene** map van de **VpnSettings.xml** bestand, kopieert u de **VpnServer** tagwaarde. Plak deze waarde in de **serveradres** en **externe ID** selectievakjes in van het profiel. Laat de **lokale ID** vak leeg.

   ![Servergegevens](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
5. Selecteer **verificatie-instellingen**, en selecteer **certificaat**. 

   ![Verificatie-instellingen](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
6. Klik op **Selecteer** kiezen van het certificaat dat u wilt gebruiken voor verificatie.

   ![Een certificaat voor verificatie selecteren](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
7. **Kies een identiteit** geeft een lijst van certificaten voor u om uit te kiezen. Het juiste certificaat selecteren en selecteer vervolgens **doorgaan**.

   !['Kies een identiteit' lijst](./media/point-to-site-vpn-client-configuration-radius/identity.png)
8. In de **lokale ID** , geeft u de naam van het certificaat (uit stap 6). In dit voorbeeld heeft **ikev2Client.com**. Selecteer vervolgens de **toepassen** knop de wijzigingen op te slaan.

   !['Lokale ID' in](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
9. In de **netwerk** in het dialoogvenster, selecteer **toepassen** alle wijzigingen op te slaan. Selecteer **Connect** om te beginnen de P2S-verbinding met de Azure-netwerk.

## <a name="otherauth"></a>Werken met andere verificatietypen of protocollen

Gebruik een ander verificatietype (bijvoorbeeld voor eenmalig gebruik), of een andere verificatieprotocol (zoals PEAP-MSCHAPv2 in plaats van EAP-MSCHAPv2) te gebruiken, moet u uw eigen VPN-client-configuratieprofiel maken. Als u wilt het profiel maakt, moet u informatie zoals het IP-adres voor gateway van virtueel netwerk, tunneltype en routes voor split-tunnel. U kunt deze informatie krijgen door de volgende stappen uit:

1. Gebruik de `Get-AzureRmVpnClientConfiguration` cmdlet voor het genereren van de configuratie van de VPN-client voor EapMSChapv2.

2. Pak het bestand VpnClientConfiguration.zip uit en zoek naar de **GenericDevice** map. De mappen met de Windows-installatieprogramma's voor 64-bits en 32-bits architectuur negeren.
 
3. De **GenericDevice** map bevat een XML-bestand met de naam **VpnSettings**. Dit bestand bevat de vereiste gegevens:

   * **VpnServer**: FQDN-naam van de Azure VPN-gateway. Dit is het adres waarmee de client verbinding maakt.
   * **VpnType**: Tunneltype die u gebruikt om verbinding te maken.
   * **Routes**: Routes die u configureren in uw profiel moet zodat alleen het verkeer dat gekoppeld aan het Azure-netwerk wordt verzonden via de P2S-tunnel.
   
   De **GenericDevice** map bevat ook een cer-bestand met de naam **VpnServerRoot**. Dit bestand bevat het basiscertificaat dat is vereist voor het valideren van de Azure VPN-gateway tijdens de installatie van de P2S-verbinding. Het certificaat installeren op alle apparaten die verbinding met de Azure-netwerk maken.

## <a name="next-steps"></a>Volgende stappen

Ga terug naar het artikel [uw P2S-configuratie voltooien](point-to-site-how-to-radius-ps.md).

Voor P2S probleemoplossingsinformatie, Zie [punt-naar-site-verbindingen van Azure voor het oplossen van](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
