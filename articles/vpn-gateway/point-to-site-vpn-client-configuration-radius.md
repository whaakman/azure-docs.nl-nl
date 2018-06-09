---
title: 'Maken en installeren van de configuratiebestanden voor VPN-clients voor RADIUS-P2S-verbindingen: PowerShell: Azure | Microsoft Docs'
description: Configuratiebestanden voor verbindingen die gebruikmaken van RADIUS-verificatie voor Windows-, Mac OS X- en Linux VPN-client maken.
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
ms.openlocfilehash: 19b1090a37ae1f97537fcabe128e7958fc26a96a
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235886"
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>Maken en configuratiebestanden van de VPN-clients voor P2S RADIUS-verificatie installeren

Als u wilt verbinden met een virtueel netwerk via de punt-naar-site (P2S), moet u het clientapparaat waarvan maakt u verbinding met configureren. U kunt P2S-VPN-verbindingen maken in Windows, Mac OS X- en Linux-clientapparaten. 

Wanneer u van RADIUS-verificatie gebruikmaakt, er zijn meerdere opties voor verificatie: verificatie van de gebruikersnaam en wachtwoord, verificatie via certificaat en andere verificatietypen. De configuratie van de VPN-client verschilt voor elk type verificatie. Voor het configureren van de VPN-client u configuratiebestanden voor clients die de vereiste instellingen bevatten. In dit artikel helpt u bij het maken en installeren van de configuratie van de VPN-client voor het RADIUS-verificatie-type dat u wilt gebruiken.

>[!IMPORTANT]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

De configuratiewerkstroom voor P2S RADIUS-verificatie is als volgt:

1. [Instellen van de Azure VPN-gateway voor P2S-connectiviteit](point-to-site-how-to-radius-ps.md).
2. [Stel uw RADIUS-server voor verificatie](point-to-site-how-to-radius-ps.md#radius). 
3. **De configuratie van de VPN-client voor de verificatieoptie van uw keuze ophalen en deze gebruiken voor het instellen van de VPN-client** (in dit artikel).
4. [Voltooi de configuratie van uw P2S en verbinding maken met](point-to-site-how-to-radius-ps.md).

>[!IMPORTANT]
>Als er wijzigingen in de punt-naar-site VPN-configuratie na het genereren van het VPN-client configuratieprofiel, zoals het VPN-protocoltype of verificatie, moet u genereren en de configuratie van een nieuwe VPN-client installeren op de apparaten van gebruikers.
>
>

Als u wilt gebruiken in de secties in dit artikel, eerst bepalen welk type verificatie u wilt gebruiken: gebruikersnaam en wachtwoord, certificaat of andere soorten authenticatie. Elke sectie bevat stappen voor Windows, Mac OS X- en Linux (beschikbaar op dit moment beperkt stappen).

## <a name="adeap"></a>Verificatie van de gebruikersnaam en wachtwoord

Verificatie van de gebruikersnaam en wachtwoord voor het gebruik van Active Directory of Active Directory niet gebruikt, kunt u configureren. Met beide scenario's, zorg ervoor dat alle gebruikers van de verbindende gebruikersnaam en wachtwoord referenties die kunnen worden geverifieerd via RADIUS.

Wanneer u een gebruikersnaam en wachtwoord authenticatie configureert, kunt u alleen een configuratie voor het verificatieprotocol EAP-MSCHAPv2-gebruikersnaam en wachtwoord. In de opdrachten `-AuthenticationMethod` is `EapMSChapv2`.

### <a name="usernamefiles"></a> 1. Configuratiebestanden voor VPN-clients genereren

Configuratiebestanden voor VPN-clients voor gebruik met verificatie van de gebruikersnaam en wachtwoord genereren. U kunt de configuratiebestanden van de VPN-clients genereren met behulp van de volgende opdracht:

```powershell 
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Met de opdracht retourneert een koppeling. Kopieer en plak de koppeling naar een webbrowser om te downloaden **VpnClientConfiguration.zip**. Pak het bestand om weer te geven van de volgende mappen: 
 
* **WindowsAmd64** en **WindowsX86**: deze mappen bevatten het installatieprogramma voor Windows 64-bits en 32-bits pakketten, respectievelijk. 
* **Algemene**: deze map bevat algemene informatie die u gebruikt om de configuratie van uw eigen VPN-client te maken. U hoeft deze map niet voor verificatie configuraties van gebruikersnaam en wachtwoord.
* **Mac**: als u IKEv2 geconfigureerd tijdens het maken van de virtuele netwerkgateway, ziet u een map met de naam **Mac** die bevat een **mobileconfig** bestand. Dit bestand kunt u Mac-clients configureren.

Als u al client configuratiebestanden gemaakt, kunt u ze ophalen met behulp van de `Get-AzureRmVpnClientConfiguration` cmdlet. Maar als u wijzigingen aanbrengt aan uw P2S-VPN-configuratie, zoals het VPN-protocoltype of verificatie, de configuratie wordt niet automatisch bijgewerkt op. U moet uitvoeren de `New-AzureRmVpnClientConfiguration` cmdlet voor het maken van het downloaden van een nieuwe configuratie.

Voor het ophalen van eerder gegenereerde client configuratiebestanden, gebruik de volgende opdracht:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```

### <a name="setupusername"></a> 2. VPN-clients configureren

U kunt de volgende VPN-clients configureren:

* [Windows](#adwincli)
* [Mac (OS X)](#admaccli)
* [Met behulp van strongSwan Linux](#adlinuxcli)
 
#### <a name="adwincli"></a>Windows VPN-client installeren

Als de versie overeenkomt met de architectuur voor de client, kunt u de dezelfde configuratiepakket voor VPN-client op elke Windows-clientcomputer. Zie voor een overzicht van client-besturingssystemen die worden ondersteund, de [Veelgestelde vragen over](vpn-gateway-vpn-faq.md#P2S).

Gebruik de volgende stappen uit om de systeemeigen Windows VPN-client voor verificatie via certificaat te configureren:

1. Selecteer de configuratiebestanden voor VPN-client die met de architectuur van de Windows-computer overeenkomen. Kies voor een 64-bits processorarchitectuur de **VpnClientSetupAmd64** installer-pakket. Kies voor een 32-bits processorarchitectuur de **VpnClientSetupX86** installer-pakket. 
2. U installeert het pakket, dubbelklik erop. Als u een pop-up SmartScreen ziet, selecteert u **meer info** > **toch uitvoeren**.
3. Blader op de clientcomputer naar **netwerkinstellingen** en selecteer **VPN**. De VPN-verbinding bevat de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. 

#### <a name="admaccli"></a>Mac (OS X) VPN-client installeren

1. Selecteer de **VpnClientSetup mobileconfig** bestands- en dit te verzenden naar elk van de gebruikers. U kunt e-mailadres of een andere methode gebruiken.

2. Zoek de **mobileconfig** -bestand op de Mac.

   ![Locatie van het bestand mobilconfig](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)
3. Dubbelklik op het profiel voor het installeren en selecteer **doorgaan**. Naam van het profiel is hetzelfde als de naam van het virtuele netwerk.

   ![Installatie-bericht](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
4. Selecteer **doorgaan** vertrouwen van de afzender van het profiel en doorgaan met de installatie.

   ![Bevestigingsbericht](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
5. Tijdens de profielinstallatie van hebt u de mogelijkheid om op te geven van de gebruikersnaam en het wachtwoord voor VPN-verificatie. Het is niet verplicht deze informatie op te geven. Als u dit doet, wordt de informatie opgeslagen en automatisch gebruikt wanneer u een verbinding tot stand brengen. Selecteer **installeren** om door te gaan.

   ![Vakken gebruikersnaam en wachtwoord voor VPN-verbinding](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
6. Voer een gebruikersnaam en wachtwoord voor de bevoegdheden die nodig zijn voor het profiel installeren op uw computer. Selecteer **OK**.

   ![Gebruikersnaam en wachtwoord in voor de profielinstallatie](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
7. Nadat het profiel is geïnstalleerd, is zichtbaar in de **profielen** in het dialoogvenster. U kunt ook openen vanuit het dialoogvenster **Systeemvoorkeuren**.

   ![Dialoogvenster 'Profielen'](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
8. Voor toegang tot de VPN-verbinding moet open de **netwerk** in het dialoogvenster van **Systeemvoorkeuren**.

   ![Pictogrammen in Systeemvoorkeuren](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
9. De VPN-verbinding wordt weergegeven als **IkeV2 VPN**. U kunt de naam wijzigen door het bijwerken van de **mobileconfig** bestand.

   ![Details voor de VPN-verbinding](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
10. Selecteer **verificatie-instellingen**. Selecteer **gebruikersnaam** in de lijst en voer uw referenties. Als u eerder hebt ingevoerd de referenties, klikt u vervolgens **gebruikersnaam** wordt automatisch gekozen in de lijst en de gebruikersnaam en wachtwoord vooraf worden ingevuld. Selecteer **OK** de instellingen op te slaan.

    ![Verificatie-instellingen](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
11. Terug in de **netwerk** dialoogvenster, **toepassen** de wijzigingen wilt opslaan. Selecteer om de verbinding tot stand brengen, **Connect**.

#### <a name="adlinuxcli"></a>Linux-VPN-client instellen via strongSwan

De volgende instructies zijn gemaakt via strongSwan 5.5.1 op Ubuntu 17.0.4. Werkelijke schermen mogelijk verschillen, afhankelijk van uw versie van Linux- en strongSwan.

1. Open de **Terminal** installeren **strongSwan** en de netwerkbeheerder met de opdracht in het voorbeeld. Als er een met betrekking foutbericht tot `libcharon-extra-plugins`, vervangen door `strongswan-plugin-eap-mschapv2`.

   ```Terminal
   sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
   ```
2. Selecteer de **netwerkbeheerder** pictogram (omhoog-pijl/pijl-omlaag) en selecteer **verbindingen bewerken**.

   !['Verbindingen bewerken' selectie in netwerkbeheer](./media/point-to-site-vpn-client-configuration-radius/EditConnection.png)
3. Selecteer de **toevoegen** knop een nieuwe verbinding maken.

   ![Knop 'Add' voor een verbinding](./media/point-to-site-vpn-client-configuration-radius/AddConnection.png)
4. Selecteer **IPsec/IKEv2 (strongswan)** uit de vervolgkeuzelijst en selecteer vervolgens **maken**. U kunt de verbinding in deze stap kunt wijzigen.

   ![Selecteren van het verbindingstype](./media/point-to-site-vpn-client-configuration-radius/AddIKEv2.png)
5. Open de **VpnSettings.xml** bestand van de **algemene** map van de configuratiebestanden van de client gedownload. De naam vinden `VpnServer` en kopieert u de naam, te beginnen met `azuregateway` en eindigend met `.cloudapp.net`.

   ![Inhoud van het bestand VpnSettings.xml](./media/point-to-site-vpn-client-configuration-radius/VpnSettings.png)
6. Plak de naam in de **adres** veld van uw nieuwe VPN-verbinding in de **Gateway** sectie. Selecteer vervolgens het pictogram van de map aan het einde van de **certificaat** veld, blader naar de **algemene** map en selecteer de **VpnServerRoot** bestand.
7. In de **Client** sectie van de verbinding, selecteer **EAP** voor **verificatie**, en voer uw gebruikersnaam en wachtwoord. Mogelijk moet selecteert het vergrendelingspictogram aan de rechterkant deze gegevens wilt opslaan. Selecteer vervolgens **Opslaan**.

   ![Verbindingsinstellingen bewerken](./media/point-to-site-vpn-client-configuration-radius/editconnectionsettings.png)
8. Selecteer de **netwerkbeheerder** pictogram (omhoog-pijl/pijl-omlaag) en houd de muis boven **VPN-verbindingen**. U ziet dat de VPN-verbinding die u hebt gemaakt. Voor het initiëren van de verbinding selecteren.

   !['VPN Radius' verbinding in netwerkbeheer](./media/point-to-site-vpn-client-configuration-radius/ConnectRADIUS.png)

## <a name="certeap"></a>Verificatie via certificaat
 
Configuratiebestanden voor RADIUS-verificatie via certificaat die het EAP-TLS-protocol gebruikt, kunt u VPN-client maken. Een onderneming uitgegeven certificaat wordt normaal gesproken een gebruiker te verifiëren voor VPN-verbinding gebruikt. Zorg ervoor dat alle gebruikers van de verbinding is een certificaat geïnstalleerd op hun apparaten en dat de RADIUS-server kan worden gebruikt voor het valideren van het certificaat.

>[!NOTE]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

In de opdrachten `-AuthenticationMethod` is `EapTls`. Tijdens de certificaatverificatie controleert de client de RADIUS-server door het certificaat te valideren. `-RadiusRootCert` is het .cer-bestand dat het basiscertificaat dat wordt gebruikt voor het valideren van de RADIUS-server bevat.

Elke client VPN-apparaat vereist een certificaat van de geïnstalleerde client. Soms heeft een Windows-apparaat meerdere clientcertificaten. Tijdens verificatie wordt kan dit resulteren in een pop-dialoogvenster met een lijst met alle certificaten. De gebruiker moet vervolgens kiezen voor het certificaat te gebruiken. Het juiste certificaat kan worden gefilterd uit door op te geven van het basiscertificaat waaraan het clientcertificaat moet aan zijn gekoppeld. 

`-ClientRootCert` is het .cer-bestand dat het basiscertificaat bevat. Het is een optionele parameter. Als het apparaat die u verbinding maken wilt vanaf slechts één clientcertificaat heeft, hebt u niet opgeven van deze parameter.

### <a name="certfiles"></a>1. Configuratiebestanden voor VPN-clients genereren

Configuratiebestanden voor VPN-clients voor gebruik met verificatie via certificaat genereren. U kunt de configuratiebestanden van de VPN-clients genereren met behulp van de volgende opdracht:
 
```powershell
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root> | fl
```

Met de opdracht retourneert een koppeling. Kopieer en plak de koppeling naar een webbrowser VpnClientConfiguration.zip downloaden. Pak het bestand om weer te geven van de volgende mappen:

* **WindowsAmd64** en **WindowsX86**: deze mappen bevatten het installatieprogramma voor Windows 64-bits en 32-bits pakketten, respectievelijk. 
* **GenericDevice**: deze map bevat algemene informatie die wordt gebruikt om de configuratie van uw eigen VPN-client te maken.

Als u al client configuratiebestanden gemaakt, kunt u ze ophalen met behulp van de `Get-AzureRmVpnClientConfiguration` cmdlet. Maar als u wijzigingen aanbrengt aan uw P2S-VPN-configuratie, zoals het VPN-protocoltype of verificatie, de configuratie wordt niet automatisch bijgewerkt op. U moet uitvoeren de `New-AzureRmVpnClientConfiguration` cmdlet voor het maken van het downloaden van een nieuwe configuratie.

Voor het ophalen van eerder gegenereerde client configuratiebestanden, gebruik de volgende opdracht:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" | fl
```
 
### <a name="setupusername"></a> 2. VPN-clients configureren

U kunt de volgende VPN-clients configureren:

* [Windows](#certwincli)
* [Mac (OS X)](#certmaccli)
* Linux (ondersteund, geen artikel stappen nog)

#### <a name="certwincli"></a>Windows VPN-client installeren

1. Selecteer een configuratiepakket en installeer deze op het clientapparaat. Kies voor een 64-bits processorarchitectuur de **VpnClientSetupAmd64** installer-pakket. Kies voor een 32-bits processorarchitectuur de **VpnClientSetupX86** installer-pakket. Als u een pop-up SmartScreen ziet, selecteert u **meer info** > **toch uitvoeren**. U kunt het pakket ook opslaan en op andere clientcomputers installeren.
2. Elke client vereist een clientcertificaat voor verificatie. Installeer het clientcertificaat. Zie voor meer informatie over clientcertificaten [clientcertificaten voor punt-naar-site](vpn-gateway-certificates-point-to-site.md). Zie het installeren van een certificaat dat is gegenereerd [een certificaat installeren op Windows-clients](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. Blader op de clientcomputer naar **netwerkinstellingen** en selecteer **VPN**. De VPN-verbinding bevat de naam van het virtuele netwerk waarmee verbinding wordt gemaakt.

#### <a name="certmaccli"></a>Mac (OS X) VPN-client installeren

U moet een afzonderlijk profiel voor alle Mac-apparaten die verbinding met het Azure-netwerk maakt maken. Dit is omdat deze apparaten het certificaat voor verificatie vereisen moet worden opgegeven in het profiel. De **algemene** map bevat alle informatie die nodig is om een profiel maken:

* **VpnSettings.xml** bevat belangrijke instellingen zoals het adres en de tunnel servertype.
* **VpnServerRoot.cer** het basiscertificaat dat u nodig hebt voor het valideren van de VPN-gateway tijdens de installatie voor P2S-verbinding bevat.
* **RadiusServerRoot.cer** bevat het basiscertificaat dat is vereist voor de validatie van de RADIUS-server tijdens de verificatie.

Gebruik de volgende stappen voor het configureren van de systeemeigen VPN-client op een Mac voor verificatie via certificaat:

1. Importeer de **VpnServerRoot** en **RadiusServerRoot** root certificaten om uw Mac. Elk bestand kopiëren naar uw Mac, dubbelklikt u op en selecteer vervolgens **toevoegen**.

   ![Het certificaat VpnServerRoot toevoegen](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

   ![Het certificaat RadiusServerRoot toevoegen](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Elke client vereist een clientcertificaat voor verificatie. Installeer het certificaat op het clientapparaat.
3. Open de **netwerk** dialoogvenster onder **Netwerkvoorkeuren**. Selecteer **+** voor het maken van een nieuwe VPN-client verbindingsprofiel voor P2S verbinding met het Azure-netwerk.

   De **Interface** waarde is **VPN**, en de **VPN-Type** waarde is **IKEv2**. Geef een naam voor het profiel in de **servicenaam** vak en selecteer vervolgens **maken** het profiel voor VPN-client verbinding maken.

   ![Interface en de naam van informatie](./media/point-to-site-vpn-client-configuration-radius/network.png)
4. In de **algemene** map van de **VpnSettings.xml** bestand, kopieert u de **VpnServer** tagwaarde. Plak deze waarde in de **serveradres** en **externe ID** vakken van het profiel. Laat de **lokale ID** vak leeg.

   ![Servergegevens](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
5. Selecteer **verificatie-instellingen**, en selecteer **certificaat**. 

   ![Verificatie-instellingen](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
6. Klik op **Selecteer** kiezen van het certificaat dat u wilt gebruiken voor verificatie.

   ![Een certificaat voor authenticatie selecteren](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
7. **Kies een identiteit** geeft een lijst met certificaten die u kunt kiezen uit. Het juiste certificaat selecteren en selecteer vervolgens **doorgaan**.

   ![Lijst 'Kies een identiteit'](./media/point-to-site-vpn-client-configuration-radius/identity.png)
8. In de **lokale ID** , geeft u de naam van het certificaat (uit stap 6). In dit voorbeeld heeft **ikev2Client.com**. Selecteer de **toepassen** knop de wijzigingen wilt opslaan.

   ![Het selectievakje 'Lokale ID'](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
9. In de **netwerk** dialoogvenster, **toepassen** alle wijzigingen op te slaan. Selecteer **Connect** starten van de P2S-verbinding met het Azure-netwerk.

## <a name="otherauth"></a>Werken met andere verificatietypen of protocollen

Een andere verificatietype (bijvoorbeeld OTP) gebruiken of een ander verificatieprotocol (zoals PEAP-MSCHAPv2 in plaats van EAP-MSCHAPv2) gebruiken, moet u uw eigen VPN-client configuratieprofiel maken. Als u wilt het profiel maakt, moet u informatie zoals het IP-adres van virtueel netwerk gateway, tunneltype en routes voor split-tunnel. U kunt deze informatie kunt krijgen met behulp van de volgende stappen uit:

1. Gebruik de `Get-AzureRmVpnClientConfiguration` cmdlet voor het genereren van de configuratie van de VPN-client voor EapMSChapv2. Zie voor instructies [in deze sectie](#ccradius) van het artikel.

2. Pak het bestand VpnClientConfiguration.zip en zoekt u de **GenenericDevice** map. De mappen met de Windows-installatieprogramma's voor 64-bits en 32-bits architectuur negeren.
 
3. De **GenenericDevice** map bevat een XML-bestand met de naam **VpnSettings**. Dit bestand bevat de vereiste gegevens:

   * **VpnServer**: FQDN-naam van de Azure VPN-gateway. Dit is het adres waarmee de client verbinding maakt.
   * **VpnType**: dat u gebruikt om verbinding te tunnelen.
   * **Routes**: Routes die u configureren in uw profiel moet zodat alleen het verkeer dat gekoppeld aan de virtuele Azure-netwerk wordt verzonden via de tunnel P2S.
   
   De **GenenericDevice** map bevat ook een cer-bestand aangeroepen **VpnServerRoot**. Dit bestand bevat het basiscertificaat dat is vereist voor de validatie van de Azure VPN-gateway tijdens de installatie voor P2S-verbinding. Het certificaat installeren op alle apparaten die met het Azure-netwerk verbinden zullen.

## <a name="next-steps"></a>Volgende stappen

Ga terug naar het artikel [uw P2S-configuratie voltooien](point-to-site-how-to-radius-ps.md).

Voor P2S probleemoplossingsinformatie, Zie [punt-naar-site-verbindingen voor probleemoplossing voor Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).