---
title: Extern verbinding maken met uw StorSimple-apparaat | Microsoft Docs
description: Wordt uitgelegd hoe u uw apparaat voor beheer op afstand configureert en hoe u verbinding maakt met Windows PowerShell voor StorSimple via HTTP of HTTPS.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/02/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05bec60f4c56c98e9b910b50e858656a2e5554b2
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55816489"
---
# <a name="connect-remotely-to-your-storsimple-8000-series-device"></a>Extern verbinding maken met de StorSimple 8000-apparaat

## <a name="overview"></a>Overzicht

U kunt op afstand verbinding met uw apparaat via Windows PowerShell. Wanneer u op deze manier verbinding maakt, ziet u niet een menu. (Ziet u een menu alleen als u de seriële console op het apparaat om verbinding te maken.) Met Windows PowerShell voor externe toegang verbinding u met een specifieke runspace. U kunt ook de weergavetaal opgeven.

Voor meer informatie over het gebruik van Windows PowerShell voor externe toegang om uw apparaat te beheren, gaat u naar [gebruik Windows PowerShell voor StorSimple voor het beheren van uw StorSimple-apparaat](storsimple-8000-windows-powershell-administration.md).

In deze zelfstudie wordt uitgelegd hoe u uw apparaat voor beheer op afstand configureert en hoe u verbinding maakt met Windows PowerShell voor StorSimple. U kunt HTTP of HTTPS gebruiken op afstand verbinding maken via Windows PowerShell. Echter, wanneer u hoe u verbinding maakt met Windows PowerShell voor StorSimple beslist, houd rekening met de volgende informatie:

* Directe verbinding met de seriële console van het apparaat is beveiligd, maar geen verbinding maken met de seriële console via netwerkswitches is. Wees voorzichtig met van de beveiligingsrisico's het verbinding maken met de seriële console van het apparaat via netwerkswitches.
* Verbinding maken via een HTTP-sessie biedt meer beveiliging dan verbinding maken via de seriële console via het netwerk mogelijk. Hoewel dit niet de meest beveiligde methode is, is het acceptabel in vertrouwde netwerken.
* Verbinding maken via een HTTPS-sessie met een zelfondertekend certificaat is het veiligste niveau en de aanbevolen optie.

U kunt extern verbinding maken met de Windows PowerShell-interface. Externe toegang tot uw StorSimple-apparaat via de Windows PowerShell-interface is echter niet standaard ingeschakeld. U moet eerst extern beheer op het apparaat inschakelen en klik vervolgens op de client die wordt gebruikt voor toegang tot uw apparaat.

De stappen in dit artikel zijn uitgevoerd op een hostsysteem met Windows Server 2012 R2.

## <a name="connect-through-http"></a>Verbinding maken via HTTP

Verbinding maken met Windows PowerShell voor StorSimple via een HTTP-sessie biedt meer beveiliging dan verbinding te maken via de seriële console van uw StorSimple-apparaat. Hoewel dit niet de meest beveiligde methode is, is het acceptabel in vertrouwde netwerken.

U kunt de Azure-portal of de seriële console extern beheer configureren. Selecteer in de volgende procedures:

* De Azure portal gebruiken voor het inschakelen van extern beheer via HTTP
* [Gebruik de seriële console extern beheer inschakelen via HTTP](#use-the-serial-console-to-enable-remote-management-over-http)

Nadat u extern beheer inschakelen, gebruik de volgende procedure de client voorbereiden voor een externe verbinding.

* [De client voorbereiden voor externe verbinding](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-portal-to-enable-remote-management-over-http"></a>De Azure portal gebruiken voor het inschakelen van extern beheer via HTTP

Voer de volgende stappen uit in de Azure portal voor het inschakelen van extern beheer via HTTP.

#### <a name="to-enable-remote-management-through-the-azure-portal"></a>Extern beheer via de Azure-portal inschakelen

1. Ga naar uw StorSimple-apparaatbeheerservice. Selecteer **apparaten** en selecteer en klik op het apparaat dat u wilt configureren voor extern beheer. Ga naar **apparaatinstellingen > beveiliging**.
2. In de **beveiligingsinstellingen** blade, klikt u op **extern beheer**.
3. In de **extern beheer** blade ingesteld **extern beheer inschakelen** naar **Ja**.
4. U kunt nu verbinding maken via HTTP. (De standaardinstelling is verbinding maken via HTTPS.) Zorg ervoor dat HTTP is geselecteerd.
   
   > [!NOTE]
   > Verbinding maken via HTTP is alleen acceptabel in vertrouwde netwerken.
   
5. Klik op **opslaan** en selecteer wanneer hierom wordt gevraagd om bevestiging, **Ja**.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>Gebruik de seriële console extern beheer inschakelen via HTTP
Voer de volgende stappen uit op de seriële console van het apparaat extern beheer inschakelen.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Extern beheer via de seriële console van het apparaat inschakelen
1. Selecteer op het menu van de seriële console optie 1. Voor meer informatie over het gebruik van de seriële console op het apparaat, gaat u naar [verbinding maken met Windows PowerShell voor StorSimple via de seriële console apparaat](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. Bij de opdrachtprompt, typt u: `Enable-HcsRemoteManagement –AllowHttp`
3. U krijgt een melding over de beveiligingsproblemen van het gebruik van HTTP verbinding maken met het apparaat. Wanneer u wordt gevraagd, bevestigt u door te typen **Y**.
4. Controleer of HTTP is ingeschakeld door te typen: `Get-HcsSystem`
5. Controleer de **RemoteManagementMode** veld bevat **HttpsAndHttpEnabled**. De volgende afbeelding ziet deze instellingen in de PuTTY.
   
     ![Seriële HTTPS en HTTP-ingeschakeld](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>De client voorbereiden voor externe verbinding
Voer de volgende stappen uit op de client extern beheer inschakelen.

#### <a name="to-prepare-the-client-for-remote-connection"></a>De client voorbereiden voor externe verbinding
1. Start een Windows PowerShell-sessie als beheerder. Als u een Windows 10-client wordt standaard gebruikt, wordt de Windows Remote Management-service is ingesteld op handmatig. Mogelijk moet u start de service door te typen:

    `Start-Service WinRM`
    
2. Typ de volgende opdracht om het IP-adres van de StorSimple-apparaat toevoegen aan lijst met vertrouwde hosts van de client:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     Vervang <*device_ip*> met het IP-adres van uw apparaat; bijvoorbeeld: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Typ de volgende opdracht voor het opslaan van referenties van het apparaat in een variabele: 
   
    ```
    $cred = Get-Credential
    ```
    
4. In het dialoogvenster dat wordt weergegeven:
   
   1. Typ de naam van de gebruiker in deze indeling: *device_ip\SSAdmin*.
   2. Typ het beheerderswachtwoord voor het apparaat dat is ingesteld wanneer het apparaat is geconfigureerd met de wizard setup. Is het standaardwachtwoord *Wachtwoord1*.
5. Start een Windows PowerShell-sessie op het apparaat door deze opdracht te typen:
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > Toevoegen voor het maken van een Windows PowerShell-sessie voor gebruik met de StorSimple-apparaat, de `–Port` parameter en geeft u de openbare poort die u hebt geconfigureerd in externe toegang voor StorSimple Virtual Appliance.
   
   
Op dit moment moet u een actieve externe Windows PowerShell-sessie op het apparaat hebben.
   
![PowerShell voor externe toegang met behulp van HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>Verbinding maken via HTTPS

Verbinding maken met Windows PowerShell voor StorSimple via een HTTPS-sessie is de veiligste en aanbevolen methode voor externe verbinding maken met uw Microsoft Azure StorSimple-apparaat. De volgende procedures wordt uitgelegd over het instellen van de seriële console- en client-computers, zodat u HTTPS verbinding maken met Windows PowerShell voor StorSimple kunt gebruiken.

U kunt de Azure-portal of de seriële console extern beheer configureren. Selecteer in de volgende procedures:

* De Azure portal gebruiken voor het inschakelen van extern beheer via HTTPS
* [Gebruik de seriële console extern beheer via HTTPS inschakelen](#use-the-serial-console-to-enable-remote-management-over-https)

Nadat u extern beheer inschakelen, gebruikt u de volgende procedures voor het voorbereiden van de host voor een extern beheer en verbinding maken met het apparaat van de externe host.

* [De host voorbereiden voor extern beheer](#prepare-the-host-for-remote-management)
* [Verbinding maken met het apparaat van de externe host](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-portal-to-enable-remote-management-over-https"></a>De Azure portal gebruiken voor het inschakelen van extern beheer via HTTPS

Voer de volgende stappen uit in de Azure portal voor het inschakelen van extern beheer via HTTPS.

#### <a name="to-enable-remote-management-over-https-from-the-azure-portal"></a>Extern beheer via HTTPS inschakelen vanuit de Azure-portal

1. Ga naar uw StorSimple-apparaatbeheerservice. Selecteer **apparaten** en selecteer en klik op het apparaat dat u wilt configureren voor extern beheer. Ga naar **apparaatinstellingen > beveiliging**.
2. In de **beveiligingsinstellingen** blade, klikt u op **extern beheer**.
3. Stel **Extern beheer inschakelen** in op **Ja**.
4. U kunt nu verbinding maken met behulp van HTTPS. (De standaardinstelling is verbinding maken via HTTPS.) Zorg ervoor dat HTTPS is geselecteerd.
5. Klik op... en klik vervolgens op **certificaat voor extern beheer downloaden**. Geef een locatie als dit bestand wilt opslaan. U moet dit certificaat op de client of hostcomputer computer die u gebruiken wilt om verbinding met het apparaat te installeren.
6. Klik op **opslaan** en klik vervolgens op **Ja** wanneer hierom wordt gevraagd om bevestiging.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>Gebruik de seriële console extern beheer via HTTPS inschakelen

Voer de volgende stappen uit op de seriële console van het apparaat extern beheer inschakelen.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Extern beheer via de seriële console van het apparaat inschakelen
1. Selecteer op het menu van de seriële console optie 1. Voor meer informatie over het gebruik van de seriële console op het apparaat, gaat u naar [verbinding maken met Windows PowerShell voor StorSimple via de seriële console apparaat](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. Bij de opdrachtprompt, typt u:
   
     `Enable-HcsRemoteManagement`
   
    Dit moet HTTPS inschakelen op uw apparaat.
3. Controleer of dat HTTPS is ingeschakeld door te typen: 
   
     `Get-HcsSystem`
   
    Zorg ervoor dat de **RemoteManagementMode** veld bevat **HttpsEnabled**. De volgende afbeelding ziet deze instellingen in de PuTTY.
   
     ![Seriële HTTPS-functionaliteit](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. Uit de uitvoer van `Get-HcsSystem`, Kopieer het serienummer van het apparaat en bewaar het voor later gebruik.
   
   > [!NOTE]
   > Het serienummer wordt toegewezen aan de CN-naam in het certificaat.
   
5. Een certificaat voor extern beheer downloaden door te typen: 
   
     `Get-HcsRemoteManagementCert`
   
    Een certificaat met de volgende strekking weergegeven.
   
    ![Certificaat voor extern beheer ophalen](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. Kopieer de gegevens in het certificaat van **---BEGIN CERTIFICATE---** naar **---END CERTIFICATE---** in een teksteditor zoals Kladblok en sla deze op te geven als een cer-bestand. (U wordt dit bestand kopiëren naar uw externe host bij het voorbereiden van de host.)
   
   > [!NOTE]
   > Voor het genereren van een nieuw certificaat, gebruikt u de `Set-HcsRemoteManagementCert` cmdlet.
   
### <a name="prepare-the-host-for-remote-management"></a>De host voorbereiden voor extern beheer

Als u wilt de hostcomputer voorbereiden op een externe verbinding die gebruikmaakt van een HTTPS-sessie, voert u de volgende procedures:

* [Het cer-bestand importeren in het basisarchief van de client of de externe host](#to-import-the-certificate-on-the-remote-host).
* [De serienummers van apparaten toevoegen aan het hosts-bestand op de externe host](#to-add-device-serial-numbers-to-the-remote-host).

De bovenstaande procedures worden hieronder beschreven.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Het certificaat op de externe host importeren
1. Klik met de rechtermuisknop op het CER-bestand en selecteer **Certificaat installeren**. Hiermee start u de wizard Certificaat importeren.
   
    ![Wizard Certificaat importeren 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. Selecteer bij **Archieflocatie** de optie **Lokale computer** en klik op **Volgende**.
3. Selecteer **Alle certificaten in het onderstaande archief opslaan** en klik op **Bladeren**. Navigeer naar het basisarchief van de externe host en klik vervolgens op **Volgende**.
   
    ![Wizard Certificaat importeren 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. Klik op **Voltooien**. Er wordt een bericht weergegeven dat het importeren is geslaagd.
   
    ![Wizard Certificaat importeren 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Serienummers van apparaten toevoegen aan de externe host
1. Start Kladblok als administrator en open vervolgens het Hostsbestand dat zich bevindt in \Windows\System32\Drivers\etc.
2. Voeg de volgende drie items toe aan het hosts-bestand: **DATA 0 IP-adres**, **vaste IP-adres van Controller 0**, en **Controller 1 vaste IP-adres**.
3. Voer het serienummer van het apparaat dat u eerder hebt opgeslagen. Wijs dit toe aan het IP-adres zoals wordt weergegeven in de volgende afbeelding. Voor Controller 0 en Controller 1 append **Controller0** en **Controller1** aan het einde van het serienummer (CN-naam).
   
    ![De CN-naam toe te voegen aan hosts-bestand](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. Sla het hosts-bestand.

### <a name="connect-to-the-device-from-the-remote-host"></a>Verbinding maken met het apparaat van de externe host

Gebruik Windows PowerShell en SSL in te voeren van een sessie SSAdmin op uw apparaat vanaf een externe host of -client. De sessie SSAdmin wordt toegewezen aan de optie 1 in de [seriële console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menu van uw apparaat.

De volgende procedure uitvoeren op de computer van waaruit u wilt maken van de externe Windows PowerShell-verbinding.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Om in te voeren van een sessie SSAdmin op het apparaat met behulp van Windows PowerShell en SSL
1. Start een Windows PowerShell-sessie als beheerder. Als u een Windows 10-client wordt standaard gebruikt, wordt de Windows Remote Management-service is ingesteld op handmatig. Mogelijk moet u start de service door te typen:

    `Start-Service WinRM`

2. Het IP-adres van het apparaat toevoegen aan de vertrouwde hosts van de client door te typen:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    Waarbij <*device_ip*> is het IP-adres van uw apparaat, bijvoorbeeld: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Als u wilt een nieuwe referentie maken, typt u:
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    Waarbij <*IP-adres van het doelapparaat*> is het IP-adres van de DATA 0 voor uw apparaat; bijvoorbeeld, **10.126.173.90** zoals wordt weergegeven in de voorgaande afbeelding van het hosts-bestand. Ook, geef het beheerderswachtwoord voor uw apparaat.
4. Geen sessie maken door te typen:
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    Voor de parameter - ComputerName in de cmdlet, geeft u de <*serienummer van het doelapparaat*>. Dit serienummer is toegewezen aan het IP-adres van de DATA 0 in het hosts-bestand op de externe host; bijvoorbeeld, **SHX0991003G44MT** zoals wordt weergegeven in de volgende afbeelding.
5. Type:
   
     `Enter-PSSession $session`
6. U moet over een paar minuten en vervolgens wordt u verbonden met het apparaat via HTTPS via SSL. U ziet het bericht weergegeven dat u bent verbonden met uw apparaat.
   
    ![PowerShell voor externe toegang met behulp van HTTPS en SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [met behulp van Windows PowerShell voor het beheren van uw StorSimple-apparaat](storsimple-8000-windows-powershell-administration.md).
* Meer informatie over [met de StorSimple Device Manager-service voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

