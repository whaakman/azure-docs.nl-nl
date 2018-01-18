---
title: 'Maken en installeren van de configuratiebestanden voor VPN-clients voor RADIUS-P2S-verbindingen: PowerShell: Azure | Microsoft Docs'
description: In dit artikel helpt u bij het maken van VPN-client-configuratiebestand voor punt-naar-Site-verbindingen die gebruikmaken van RADIUS-verificatie.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2018
ms.author: cherylmc
ms.openlocfilehash: 37951a04bbfd266717490dd1752d0be04d2231a5
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>Maken en configuratiebestanden van de VPN-clients voor P2S RADIUS-verificatie installeren

Configuratiebestanden voor VPN-clients bevinden zich in een zip-bestand. Configuratiebestanden bevatten de instellingen die vereist zijn voor een native client voor Windows of Mac IKEv2 VPN-verbinding maken met een VNet via punt-naar-Site. De RADIUS-server biedt verschillende opties voor verificatie en zo de configuratie van de VPN-client varieert voor elke optie.

### <a name="workflow"></a>Werkstroom

  1. [Instellen van de Azure VPN-gateway voor P2S-connectiviteit](point-to-site-how-to-radius-ps.md).
  2. [Stel uw RADIUS-server voor verificatie](point-to-site-how-to-radius-ps.md#radius). 
  3. (Dit artikel) - verkrijgen van de configuratie van de VPN-client voor de verificatieoptie van uw keuze en deze gebruiken voor het instellen van de VPN-client op uw Windows-apparaat. Hiermee kunt u verbinding maken met Azure VNets via een P2S-verbinding.
  4. [Voltooi de configuratie van uw P2S en verbinding maken met](point-to-site-how-to-radius-ps.md).

>[!IMPORTANT]
>Als er wijzigingen in de punt-naar-Site VPN-configuratie na het genereren van het VPN-client configuratieprofiel, zoals het VPN-protocoltype of verificatie, moet u genereren en de configuratie van een nieuwe VPN-client installeren op uw apparaten van gebruikers.
>
>

## <a name="adeap"></a>Over verificatie met gebruikersnaam en wachtwoord

* **AD-verificatie:** een populaire scenario is verificatie van AD-domeinen. In dit scenario gebruikers hun domeinreferenties gebruiken verbinding maken met Azure VNets. U kunt de configuratiebestanden voor VPN-clients voor AD RADIUS-verificatie maken.

* **Authenticatie zonder AD:** kunt u ook het scenario met gebruikersnaam/wachtwoord RADIUS-verificatie zonder AD configureren.

Zorg ervoor dat alle gebruikers van de verbindende gebruikersnaam en wachtwoord referenties die kunnen worden geverifieerd via RADIUS. U kunt alleen een configuratie voor het protocol voor verificatie van EAP-MSCHAPv2-gebruikersnaam en wachtwoord maken. '-AuthenticationMethod' is opgegeven als 'EapMSChapv2'.

## <a name="usernamefiles"></a> 1. Configuratiebestanden voor VPN-clients genereren

Configuratiebestanden voor VPN-clients voor gebruik met verificatie van de gebruikersnaam en wachtwoord genereren. U kunt de configuratiebestanden van de VPN-client met de volgende opdracht genereren:

```powershell 
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Met de opdracht retourneert een koppeling. Kopieer en plak de koppeling naar een webbrowser om te downloaden van 'VpnClientConfiguration.zip'. Pak het bestand om weer te geven van de volgende mappen: 
 
* **WindowsAmd64** en **WindowsX86** -deze mappen bevatten het installatieprogramma voor Windows 64-bits en 32-bits pakketten, respectievelijk. 
* **GenericDevice** -deze map bevat algemene informatie gebruikt om de configuratie van uw eigen VPN-client te maken. Deze map is niet nodig voor de gebruikersnaam en wachtwoord verificatie configuraties.
* **Mac** -als IKEv2 tijdens het maken van de virtuele netwerkgateway is geconfigureerd, ziet u een map met de naam 'Mac' met een **mobileconfig** bestand. Dit bestand wordt gebruikt voor het configureren van Mac-clients.

Als u al client configuratiebestanden gemaakt, kunt u ze kunt ophalen met de cmdlet 'Get-AzureRmVpnClientConfiguration'. Echter, als u wijzigingen aanbrengt aan uw P2S-VPN-configuratie, zoals het Protocol van de VPN-type of verificatie, de configuratie niet automatisch bijgewerkt. U moet de cmdlet, New-AzureRmVpnClientConfiguration' voor het maken van het downloaden van een nieuwe configuratie uitvoeren.

Voor het ophalen van eerder gegenereerde client configuratiebestanden, gebruik de volgende opdracht:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```

## <a name="setupusername"></a> 2. Windows en Mac-VPN-clients configureren
 
### <a name="adwincli"></a>Windows VPN-client installeren

Als de versie overeenkomt met de architectuur voor de client, kunt u de dezelfde configuratiepakket voor VPN-client op elke Windows-clientcomputer. Voor de lijst met client-besturingssystemen die worden ondersteund, Zie de sectie punt-naar-Site van de [Veelgestelde vragen over](vpn-gateway-vpn-faq.md#P2S).

Gebruik de volgende stappen uit om de systeemeigen Windows VPN-client voor verificatie via certificaat te configureren:

1. Selecteer de configuratiebestanden voor VPN-client die met de architectuur van de Windows-computer overeenkomen. Kies het 'VpnClientSetupAmd64' installer-pakket voor een 64-bits processorarchitectuur. Kies het 'VpnClientSetupX86' installer-pakket voor een 32-bits processorarchitectuur. 
2. Dubbelklik op het pakket om het te installeren. Als u een SmartScreen-melding ziet, klikt u op **Meer info** en vervolgens op **Toch uitvoeren**.
3. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De VPN-verbinding bevat de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. 

### <a name="admaccli"></a>Mac (OSX) VPN-client installeren

1. Selecteer de **VpnClientSetup mobileconfig** bestands- en dit te verzenden naar elk van de gebruikers. U kunt e-mailadres of een andere methode gebruiken om dit te doen.

2. Zoek de **mobileconfig** -bestand op de Mac.

  ![Ga naar mobilconfig-bestand](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)
3. Dubbelklik op het profiel installeren, klikt u op **doorgaan**. Naam van het profiel is hetzelfde als de naam van uw VNet.

  ![Installeren](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
4. Klik op **doorgaan** vertrouwen van de afzender van het profiel en doorgaan met de installatie.

  ![doorgaan](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
5. Tijdens de profielinstallatie van krijgt u de mogelijkheid te geven van de gebruikersnaam en het wachtwoord dat wordt gebruikt voor VPN-verificatie. Het is niet verplicht deze informatie op te geven. Als u opgeeft, wordt de informatie opgeslagen en automatisch gebruikt wanneer u een verbinding tot stand brengen. Klik op **installeren** om door te gaan.

  ![instellingen](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
6. Voer een gebruikersnaam en wachtwoord voor de benodigde bevoegdheden vereist het profiel te installeren op uw computer. Klik op **OK**.

  ![gebruikersnaam en wachtwoord](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
7. Na de installatie, het profiel is zichtbaar in de **profielen** in het dialoogvenster. Dit dialoogvenster kan ook later geopend vanuit **Systeemvoorkeuren**.

  ![Systeemvoorkeuren](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
8. Voor toegang tot de VPN-verbinding moet open de **netwerk** dialoogvenster van **Systeemvoorkeuren**.

  ![Netwerk](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
9. De VPN-verbinding wordt weergegeven als **IkeV2 VPN**. De naam kan worden gewijzigd door het bijwerken van de **mobileconfig** bestand.

  ![verbinding](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
10. Klik op **verificatie-instellingen**. Kies **gebruikersnaam** in de vervolgkeuzelijst en voer uw referenties. Als u eerder hebt ingevoerd de referenties, klikt u vervolgens **gebruikersnaam** wordt automatisch gekozen in de vervolgkeuzelijst en de gebruikersnaam en het wachtwoord zijn al ingevuld. Klik op **OK** om de instellingen op te slaan. Hiermee gaat u terug naar het dialoogvenster netwerk.

  ![Verifiëren](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
11. Klik op **toepassen** de wijzigingen wilt opslaan. Voor het initiëren van de verbinding, klikt u op **Connect**.

## <a name="certeap"></a>Over verificatie via certificaat
 
Configuratiebestanden voor RADIUS-verificatie via certificaat die het EAP-TLS-protocol gebruikt, kunt u VPN-client maken. Een onderneming uitgegeven certificaat wordt normaal gesproken een gebruiker te verifiëren voor VPN-verbinding gebruikt. Zorg ervoor dat alle gebruikers van de verbinding een certificaat geïnstalleerd op apparaten van de gebruikers, en dat het certificaat kan worden gevalideerd door de RADIUS-server.
 
* '-AuthenticationMethod' is 'EapTls'.
* Tijdens de certificaatverificatie controleert de client de RADIUS-server door het certificaat te valideren. -RadiusRootCert is het cer-bestand met het root-certificaat dat wordt gebruikt voor het valideren van de RADIUS-server.  
* Soms heeft een Windows-apparaat meerdere clientcertificaten. Tijdens verificatie wordt kan dit resulteren in een pop-dialoogvenster met alle certificaten. De gebruiker moet vervolgens kiezen voor het certificaat te gebruiken. Het juiste certificaat kan worden gefilterd uit door op te geven van het basiscertificaat waarnaar het certificaat van de client moet zijn gekoppeld. '-ClientRootCert' is het .cer-bestand dat het root-certificaat bevat. Het is een optionele parameter. Als het apparaat van waaruit u verbinding wilt slechts één clientcertificaat deze parameter niet hoeft heeft te worden opgegeven.

## <a name="certfiles"></a>1. Configuratiebestanden voor VPN-clients genereren

Configuratiebestanden voor VPN-clients voor gebruik met verificatie via certificaat genereren. U kunt de configuratiebestanden van de VPN-client met de volgende opdracht genereren:
 
```powershell
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root>
```

Met de opdracht retourneert een koppeling. Kopieer en plak de koppeling naar een webbrowser om te downloaden van 'VpnClientConfiguration.zip'. Pak het bestand om weer te geven van de volgende mappen:

* **WindowsAmd64** en **WindowsX86** -deze mappen bevatten het installatieprogramma voor Windows 64-bits en 32-bits pakketten, respectievelijk. 
* **GenericDevice** -deze map bevat algemene informatie gebruikt om de configuratie van uw eigen VPN-client te maken.

Als u al client configuratiebestanden gemaakt, kunt u ze kunt ophalen met de cmdlet 'Get-AzureRmVpnClientConfiguration'. Echter, als u wijzigingen aanbrengt aan uw P2S-VPN-configuratie, zoals het Protocol van de VPN-type of verificatie, de configuratie niet automatisch bijgewerkt. U moet de cmdlet, New-AzureRmVpnClientConfiguration' voor het maken van het downloaden van een nieuwe configuratie uitvoeren.

Voor het ophalen van eerder gegenereerde client configuratiebestanden, gebruik de volgende opdracht:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```
 
## <a name="setupusername"></a> 2. Windows en Mac-VPN-clients configureren

### <a name="certwincli"></a>Windows VPN-client installeren

1. Selecteer een configuratiepakket en installeer deze op het clientapparaat. Kies het 'VpnClientSetupAmd64' installer-pakket voor een 64-bits processorarchitectuur. Kies het 'VpnClientSetupX86' installer-pakket voor een 32-bits processorarchitectuur. Als u een SmartScreen-melding ziet, klikt u op **Meer info** en vervolgens op **Toch uitvoeren**. U kunt het pakket ook opslaan en op andere clientcomputers installeren.
2. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De VPN-verbinding bevat de naam van het virtuele netwerk waarmee verbinding wordt gemaakt.

### <a name="certmaccli"></a>Mac (OSX) VPN-client installeren

Een afzonderlijk profiel moet worden gemaakt voor alle Mac-apparaten die verbinding met Azure VNet maakt. Dit is omdat deze apparaten het certificaat voor verificatie vereisen moet worden opgegeven in het profiel. De **algemene** map bevat alle gegevens die vereist zijn om een profiel te maken.

  * **VpnSettings.xml** bevat belangrijke instellingen zoals het adres en de tunnel servertype.
  * **VpnServerRoot.cer** het basiscertificaat dat is vereist voor de validatie van de VPN-gateway tijdens de installatie voor P2S-verbinding bevat.
  * **RadiusServerRoot.cer** bevat het basiscertificaat dat is vereist voor de validatie van de RADIUS-server tijdens de verificatie.

Gebruik de volgende stappen voor het configureren van de systeemeigen VPN-client op Mac voor verificatie via certificaat:

1. Importeer de **VpnServerRoot** en de **RadiusServerRoot** root certificaten om uw Mac. Dit kan worden gedaan door het bestand kopiëren via uw Mac en erop te dubbelklikken.  
Klik op **toevoegen** om te importeren.

  **VpnServerRoot toevoegen**

  ![certificaat toevoegen](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

  **RadiusServerRoot toevoegen**

  ![certificaat toevoegen](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Open de **netwerk** dialoogvenster onder **Netwerkvoorkeuren** en klik op **'+'** voor het maken van een nieuwe VPN-client verbindingsprofiel voor P2S verbinding met het Azure VNet.

  De **Interface** waarde is 'VPN' en **VPN-Type** waarde is 'IKEv2'. Geef een naam voor het profiel in de **servicenaam** veld en klik vervolgens op **maken** het profiel voor VPN-client verbinding maken.

  ![Netwerk](./media/point-to-site-vpn-client-configuration-radius/network.png)
3. In de **algemene** map van de **VpnSettings.xml** bestand, kopieert u de **VpnServer** tagwaarde. Plak deze waarde in de **serveradres** en **externe ID** velden van het profiel. Laat de **lokale ID** veld leeg.

  ![Server-info](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
4. Klik op **verificatie-instellingen** en selecteer **certificaat**. 

  ![verificatie-instellingen](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
5. Klik op **selecteren...** om te kiezen in het certificaat dat u wilt gebruiken voor verificatie.

  ![certificaat](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
6. **Kies een identiteit** geeft een lijst met certificaten die u kunt kiezen uit. Het juiste certificaat selecteren en klik vervolgens op **doorgaan**.

  ![identity](./media/point-to-site-vpn-client-configuration-radius/identity.png)
7. In de **lokale ID** veld, geef de naam van het certificaat (uit stap 5). In dit voorbeeld is het 'ikev2Client.com'. Klik vervolgens op **toepassen** knop de wijzigingen wilt opslaan.

  ![toepassen](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
8. Op de **netwerk** dialoogvenster, klikt u op **toepassen** alle wijzigingen op te slaan. Klik vervolgens op **Connect** starten van de P2S-verbinding met het Azure VNet.

## <a name="otherauth"></a>Werken met andere verificatietypen of protocollen

Verschillende verificatietype (bijvoorbeeld OTP), en geen gebruikersnaam en wachtwoord of certificaten wilt gebruiken of een andere verificatieprotocol (zoals PEAP-MSCHAPv2, in plaats van EAP-MSCHAPv2) te gebruiken, moet u uw eigen VPN-client configuratieprofiel maken. Als u wilt het profiel maakt, moet u informatie zoals het IP-adres van virtueel netwerk gateway, tunneltype en routes voor split-tunnel. U kunt deze informatie kunt krijgen met behulp van de volgende stappen uit:

1. Gebruik de cmdlet 'Get-AzureRmVpnClientConfiguration' voor het genereren van de configuratie van de VPN-client voor EapMSChapv2. Zie voor instructies [in deze sectie](#ccradius) van het artikel.

2. Pak het bestand VpnClientConfiguration.zip en zoek naar de map GenenericDevice. De mappen met de Windows-installatieprogramma's voor 64-bits en 32-bits architectuur negeren.
 
3. De map GenenericDevice bevat een XML-bestand VpnSettings aangeroepen. Dit bestand bevat de vereiste informatie.

  * VpnServer - FQDN-naam van de Azure VPN-Gateway. Dit is het adres waarmee de client verbinding maakt.
  * VpnType - de tunneltype waarmee u verbinding maakt.
  * Routes - Routes die u hebt in uw profiel zodanig configureren dat alleen Azure VNet verkeer gebonden wordt verzonden via de tunnel P2S.
  * De map GenenericDevice bevat ook een cer-bestand 'VpnServerRoot' genoemd. Dit bestand bevat het basiscertificaat dat is vereist voor de validatie van de Azure VPN-Gateway tijdens de installatie voor P2S-verbinding. Het certificaat installeren op alle apparaten die verbinding met het Azure VNet maakt. 
 
## <a name="next-steps"></a>Volgende stappen

Ga terug naar het artikel [uw P2S-configuratie voltooien](point-to-site-how-to-radius-ps.md).