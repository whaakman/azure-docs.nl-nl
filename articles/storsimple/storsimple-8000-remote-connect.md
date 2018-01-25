---
title: Extern verbinding maken met uw StorSimple-apparaat | Microsoft Docs
description: Legt uit hoe u uw apparaat voor extern beheer configureren en verbinding maken met Windows PowerShell voor StorSimple via HTTP of HTTPS.
services: storsimple
documentationcenter: 
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/02/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9414d9c93fe463910ffa6fce72aada6a0d720464
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="connect-remotely-to-your-storsimple-8000-series-device"></a>Extern verbinding maken met uw StorSimple 8000 series apparaat

## <a name="overview"></a>Overzicht

U kunt op afstand verbinding met uw apparaat via Windows PowerShell. Als u op deze manier verbinding maakt, ziet u niet een menu. (Ziet u een menu alleen als u de seriële console op het apparaat om verbinding te gebruiken.) Met Windows PowerShell op afstand verbinding u met een specifieke runspace. U kunt ook de weergavetaal opgeven.

Voor meer informatie over het beheren van uw apparaat met Windows PowerShell op afstand, gaat u naar [gebruik Windows PowerShell voor StorSimple voor het beheren van uw StorSimple-apparaat](storsimple-8000-windows-powershell-administration.md).

Deze zelfstudie wordt uitgelegd hoe u uw apparaat voor extern beheer configureren en vervolgens verbinding maken met Windows PowerShell voor StorSimple. U kunt HTTP of HTTPS gebruiken om te verbinden via Windows PowerShell. Echter, wanneer u verbinding maken met Windows PowerShell voor StorSimple bepaalt, houd rekening met de volgende informatie:

* Rechtstreeks verbinding maken met de seriële console van het apparaat is beveiligd, maar geen verbinding maken met de seriële console via netwerkswitches is. Wees voorzichtig met over de beveiligingsrisico's het verbinding maken met de seriële console van het apparaat via netwerkswitches.
* Verbinding maken via een HTTP-sessie, is het mogelijk bieden meer beveiliging dan verbinding maken via de seriële console via het netwerk. Hoewel dit niet de meest beveiligde methode, is het is acceptabel in vertrouwde netwerken.
* Verbinding maken via een HTTPS-sessie met een zelfondertekend certificaat is de veiligste en de aanbevolen optie.

U kunt op afstand verbinden met de Windows PowerShell-interface. Externe toegang tot uw StorSimple-apparaat via de Windows PowerShell-interface wordt echter niet standaard ingeschakeld. U moet eerst extern beheer op het apparaat inschakelen en klik vervolgens op de client die wordt gebruikt voor toegang tot uw apparaat.

De stappen in dit artikel zijn uitgevoerd op een hostsysteem met Windows Server 2012 R2.

## <a name="connect-through-http"></a>Verbinding maken via HTTP

Verbinding maken met Windows PowerShell voor StorSimple via een HTTP-sessie, biedt betere beveiliging dan verbinding maken via de seriële console van uw StorSimple-apparaat. Hoewel dit niet de meest beveiligde methode, is het is acceptabel in vertrouwde netwerken.

U kunt de Azure portal of de seriële console extern beheer configureren. Selecteer in de volgende procedures:

* [De Azure portal gebruiken voor het inschakelen van extern beheer via HTTP](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [Gebruik de seriële console extern beheer inschakelen via HTTP](#use-the-serial-console-to-enable-remote-management-over-http)

Nadat u extern beheer inschakelen, gebruik de volgende procedure voor het voorbereiden van de client voor een externe verbinding.

* [De client voorbereiden voor externe verbinding](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-portal-to-enable-remote-management-over-http"></a>De Azure portal gebruiken voor het inschakelen van extern beheer via HTTP

Voer de volgende stappen uit in de Azure portal extern beheer inschakelen via HTTP.

#### <a name="to-enable-remote-management-through-the-azure-portal"></a>Extern beheer via de Azure portal inschakelen

1. Ga naar uw StorSimple-apparaatbeheerservice. Selecteer **apparaten** en selecteer en klik op het apparaat dat u wilt configureren voor extern beheer. Ga naar **apparaatinstellingen > beveiliging**.
2. In de **beveiligingsinstellingen** blade, klikt u op **extern beheer**.
3. In de **extern beheer** blade ingesteld **extern beheer inschakelen** naar **Ja**.
4. U kunt nu verbinding maken via HTTP. (De standaardinstelling is verbinding maken via HTTPS.) Zorg ervoor dat HTTP is geselecteerd.
   
   > [!NOTE]
   > Verbinding maken via HTTP is alleen acceptabel in vertrouwde netwerken.
   
5. Klik op **opslaan** en wanneer u om bevestiging gevraagd, selecteert u **Ja**.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>Gebruik de seriële console extern beheer inschakelen via HTTP
De volgende stappen uitvoeren op de seriële console van het apparaat extern beheer inschakelen.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Extern beheer via de seriële console van het apparaat inschakelen
1. Selecteer in het menu seriële console optie 1. Voor meer informatie over het gebruik van de seriële console op het apparaat, gaat u naar [verbinding maken met Windows PowerShell voor StorSimple via de seriële console apparaat](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. Typ het volgende bij de opdrachtprompt:`Enable-HcsRemoteManagement –AllowHttp`
3. U wordt geïnformeerd over de beveiligingsproblemen van het gebruik van HTTP verbinding maken met het apparaat. Bevestig door op te geven wanneer u wordt gevraagd, **Y**.
4. Controleer of HTTP is ingeschakeld door te typen:`Get-HcsSystem`
5. Controleer de **RemoteManagementMode** veld bevat **HttpsAndHttpEnabled**. De volgende afbeelding ziet deze instellingen in de PuTTY.
   
     ![Seriële HTTPS en HTTP-ingeschakeld](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>De client voorbereiden voor externe verbinding
Voer de volgende stappen uit op de client extern beheer inschakelen.

#### <a name="to-prepare-the-client-for-remote-connection"></a>De client voorbereiden voor externe verbinding
1. Start een Windows PowerShell-sessie als administrator. Als u een Windows 10-client standaard, wordt de Windows Remote Management-service is ingesteld op handmatig. Mogelijk moet u de service starten door in te voeren:

    `Start-Service WinRM`
    
2. Typ de volgende opdracht het IP-adres van het StorSimple-apparaat toevoegen aan lijst met vertrouwde hosts van de client:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     Vervang <*device_ip*> met het IP-adres van uw apparaat; bijvoorbeeld: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Typ de volgende opdracht voor het opslaan van de referenties van het apparaat in een variabele: 
   
    ```
    $cred = Get-Credential
    ```
    
4. In het dialoogvenster dat wordt weergegeven:
   
   1. Typ de gebruikersnaam in deze indeling: *device_ip\SSAdmin*.
   2. Typ het administrator-wachtwoord voor het apparaat dat is ingesteld wanneer het apparaat is geconfigureerd met de wizard setup. Is het standaardwachtwoord *Wachtwoord1*.
5. Start een Windows PowerShell-sessie op het apparaat door deze opdracht te typen:
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > Toevoegen als u wilt maken voor gebruik van een Windows PowerShell-sessie met het virtuele StorSimple-apparaat, de `–Port` parameter en geeft u de openbare poort die u hebt geconfigureerd in externe toegang voor virtueel StorSimple-apparaat.
   
   
U hebt op dit moment een actieve externe Windows PowerShell-sessie op het apparaat.
   
![Externe communicatie via HTTP met PowerShell](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>Verbinding maken via HTTPS

Verbinding maken met Windows PowerShell voor StorSimple via een HTTPS-sessie, is de veiligste en aanbevolen methode externe verbinding maken met uw Microsoft Azure StorSimple-apparaat. De volgende procedures wordt uitgelegd hoe voor het instellen van de seriële console en client-computers, zodat u HTTPS kunt verbinding maken met Windows PowerShell voor StorSimple.

U kunt de Azure portal of de seriële console extern beheer configureren. Selecteer in de volgende procedures:

* [De Azure portal gebruiken voor het inschakelen van extern beheer via HTTPS](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [Gebruik de seriële console extern beheer inschakelen via HTTPS](#use-the-serial-console-to-enable-remote-management-over-https)

Nadat u extern beheer inschakelen, gebruikt u de volgende procedures voor het voorbereiden van de host voor een extern beheer en verbinding maken met het apparaat van de externe host.

* [De host voorbereiden voor extern beheer](#prepare-the-host-for-remote-management)
* [Verbinding maken met het apparaat van de externe host](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-portal-to-enable-remote-management-over-https"></a>De Azure portal gebruiken voor het inschakelen van extern beheer via HTTPS

Voer de volgende stappen uit in de Azure portal extern beheer inschakelen via HTTPS.

#### <a name="to-enable-remote-management-over-https-from-the-azure-portal"></a>Extern beheer inschakelen via HTTPS vanaf de Azure portal

1. Ga naar uw StorSimple-apparaatbeheerservice. Selecteer **apparaten** en selecteer en klik op het apparaat dat u wilt configureren voor extern beheer. Ga naar **apparaatinstellingen > beveiliging**.
2. In de **beveiligingsinstellingen** blade, klikt u op **extern beheer**.
3. Stel **Extern beheer inschakelen** in op **Ja**.
4. U kunt nu verbinding maken via HTTPS. (De standaardinstelling is verbinding maken via HTTPS.) Zorg ervoor dat HTTPS is geselecteerd.
5. Klik op... en klik vervolgens op **certificaat voor extern beheer downloaden**. Geef een locatie voor dit bestand opslaan. U moet dit certificaat installeren op de client of hostcomputer computer die u gebruiken wilt voor het verbinding maken met het apparaat.
6. Klik op **opslaan** en klik vervolgens op **Ja** wanneer u om bevestiging wordt gevraagd.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>Gebruik de seriële console extern beheer inschakelen via HTTPS

De volgende stappen uitvoeren op de seriële console van het apparaat extern beheer inschakelen.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Extern beheer via de seriële console van het apparaat inschakelen
1. Selecteer in het menu seriële console optie 1. Voor meer informatie over het gebruik van de seriële console op het apparaat, gaat u naar [verbinding maken met Windows PowerShell voor StorSimple via de seriële console apparaat](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. Typ het volgende bij de opdrachtprompt:
   
     `Enable-HcsRemoteManagement`
   
    Dit moet HTTPS inschakelen op uw apparaat.
3. Controleer of HTTPS is ingeschakeld door te typen: 
   
     `Get-HcsSystem`
   
    Zorg ervoor dat de **RemoteManagementMode** veld bevat **HttpsEnabled**. De volgende afbeelding ziet deze instellingen in de PuTTY.
   
     ![Seriële HTTPS-functionaliteit](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. Vanuit de uitvoer van `Get-HcsSystem`, Kopieer het serienummer van het apparaat en opslaan voor later gebruik.
   
   > [!NOTE]
   > Het serienummer wordt toegewezen aan de CN-naam in het certificaat.
   
5. Een certificaat voor extern beheer verkrijgen door te typen: 
   
     `Get-HcsRemoteManagementCert`
   
    Een certificaat voor de volgende strekking weergegeven.
   
    ![Extern beheercertificaat ophalen](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. Kopieer de gegevens in het certificaat van **---BEGIN CERTIFICATE---** naar **---EINDCERTIFICAAT---** in een teksteditor zoals Kladblok en sla deze op te geven als een .cer-bestand. (Kopieert u dit bestand met de externe host bij het voorbereiden van de host.)
   
   > [!NOTE]
   > Als een nieuw certificaat worden gegenereerd, gebruiken de `Set-HcsRemoteManagementCert` cmdlet.
   
### <a name="prepare-the-host-for-remote-management"></a>De host voorbereiden voor extern beheer

Als u de computer voorbereiden voor een externe verbinding die gebruikmaakt van een HTTPS-sessie, kunt u de volgende procedures uitvoeren:

* [Het cer-bestand importeren in het basisarchief van de client of de externe host](#to-import-the-certificate-on-the-remote-host).
* [De serienummers van het apparaat toevoegen aan het hosts-bestand op de externe host](#to-add-device-serial-numbers-to-the-remote-host).

Elk van de voorgaande procedures wordt hieronder beschreven.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Het certificaat op de externe host importeren
1. Met de rechtermuisknop op het cer-bestand en selecteer **installeren certificaat**. Hiermee start u de Wizard Certificaat importeren.
   
    ![Wizard Certificaat importeren 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. Voor **archieflocatie**, selecteer **lokale Machine**, en klik vervolgens op **volgende**.
3. Selecteer **alle certificaten in het onderstaande archief plaatsen**, en klik vervolgens op **Bladeren**. Navigeer naar het basisarchief van de externe host, en klik vervolgens op **volgende**.
   
    ![Wizard Certificaat importeren 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. Klik op **Voltooien**. Er verschijnt een bericht dat u het importeren is geslaagd.
   
    ![Wizard Certificaat importeren 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Serienummers toevoegen aan de externe host
1. Kladblok start als beheerder en open vervolgens het hosts-bestand dat zich bevindt op \Windows\System32\Drivers\etc.
2. De volgende drie items toevoegen aan het hosts-bestand: **DATA 0-IP-adres**, **Controller 0 vast IP-adres**, en **Controller 1 vast IP-adres**.
3. Voer het serienummer van het apparaat dat u eerder hebt opgeslagen. Toewijzing van het IP-adres zoals in de volgende afbeelding. Voor Controller 0 en Controller 1 toevoegen **Controller0** en **Controller1** aan het einde van het serienummer (CN-naam).
   
    ![CN-naam aan het hosts-bestand toe te voegen](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. Sla het hosts-bestand.

### <a name="connect-to-the-device-from-the-remote-host"></a>Verbinding maken met het apparaat van de externe host

Gebruik Windows PowerShell- en SSL voor een sessie SSAdmin op uw apparaat vanaf een externe host of de client. De sessie SSAdmin wordt toegewezen aan de optie 1 in de [seriële console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menu van uw apparaat.

De volgende procedure uitvoeren op de computer van waaruit u wilt maken van de externe verbinding met Windows PowerShell.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Een sessie SSAdmin invoeren op het apparaat met behulp van Windows PowerShell- en SSL
1. Start een Windows PowerShell-sessie als administrator. Als u een Windows 10-client standaard, wordt de Windows Remote Management-service is ingesteld op handmatig. Mogelijk moet u de service starten door in te voeren:

    `Start-Service WinRM`

2. Het IP-adres van het apparaat toevoegen aan de vertrouwde hosts van de client door te typen:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    Waarbij <*device_ip*> het IP-adres van uw apparaat is; bijvoorbeeld: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Als u wilt een nieuwe referentie maken, typt u:
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    Waarbij <*IP-adres van het doelapparaat*> het IP-adres van de DATA 0 voor uw apparaat; bijvoorbeeld **10.126.173.90** zoals weergegeven in de voorgaande afbeelding van het hosts-bestand. Geef ook het administrator-wachtwoord voor uw apparaat.
4. Een sessie maken door te typen:
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    Voor de parameter - ComputerName in de cmdlet, geeft u de <*serienummer van het doelapparaat*>. Dit serienummer is toegewezen aan het IP-adres van de DATA 0 in het bestand hosts op de externe host; bijvoorbeeld: **SHX0991003G44MT** zoals weergegeven in de volgende afbeelding.
5. Type:
   
     `Enter-PSSession $session`
6. U moet wacht een paar minuten en vervolgens u worden verbonden met uw apparaat via HTTPS via SSL. U ziet een bericht dat u bent verbonden met uw apparaat aangeeft.
   
    ![PowerShell voor externe toegang met behulp van HTTPS- en SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Windows PowerShell gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-8000-windows-powershell-administration.md).
* Meer informatie over [de service Manager voor StorSimple-apparaat gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

