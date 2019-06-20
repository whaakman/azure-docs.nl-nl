---
title: Voorbereiden van een Windows-VHD te uploaden naar Azure | Microsoft Docs
description: Informatie over het voorbereiden van een Windows VHD of VHDX te uploaden naar Azure
services: virtual-machines-windows
documentationcenter: ''
author: glimoli
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2019
ms.author: genli
ms.openlocfilehash: cc942aeb34d17e8dff064c6a21a3c7b2099c742a
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67151031"
---
# <a name="prepare-a-windows-vhd-or-vhdx-to-upload-to-azure"></a>Een Windows VHD of VHDX te uploaden naar Azure voorbereiden

Voordat u een Windows virtuele machine (VM) van on-premises naar Azure uploaden, moet u de virtuele harde schijf (VHD of VHDX) voorbereiden. Azure ondersteunt zowel 1e en 2e generatie virtuele machines die in VHD-indeling zijn en die een schijf met vaste grootte. De maximale grootte van de VHD is 1023 GB. 

In een generatie 1 VM, kunt u een VHDX-bestandssysteem converteren naar VHD. U kunt ook een dynamisch uitbreidbare schijven converteren naar een schijf met vaste grootte. Maar u kunt een virtuele machine generatie niet wijzigen. Zie voor meer informatie, [maak ik een generatie 1 of 2 virtuele machine in Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v) en [ondersteuning voor Azure voor generatie 2 virtuele machines (preview)](generation-2.md).

Zie voor meer informatie over het ondersteuningsbeleid voor virtuele Azure-machines [ondersteuning van Microsoft-serversoftware voor virtuele Azure-machines](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

> [!NOTE]
> De instructies in dit artikel zijn van toepassing op de 64-bits versie van Windows Server 2008 R2 en hoger Windows Server-besturingssystemen. Zie voor meer informatie over het uitvoeren van een 32-bits besturingssysteem in Azure [ondersteuning voor 32-bits besturingssystemen in Azure VM's](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines).

## <a name="convert-the-virtual-disk-to-a-fixed-size-and-to-vhd"></a>De virtuele schijf converteren naar een vaste grootte en naar VHD 
Als u converteren van de virtuele schijf naar de vereiste indeling voor Azure wilt, kunt u een van de methoden in deze sectie gebruiken. Back-up van de virtuele machine voordat u de virtuele schijf converteren. Zorg ervoor dat de VHD met Windows correct werkt op de lokale server. Los eventuele fouten in de virtuele machine zelf vervolgens voordat u probeert te converteren of uploaden naar Azure.

Nadat u de schijf converteren, maakt u een virtuele machine die gebruikmaakt van de schijf. Start en zich aanmelden bij de virtuele machine om te voltooien voor het uploaden wordt voorbereid.

### <a name="use-hyper-v-manager-to-convert-the-disk"></a>Hyper-V Manager gebruiken voor het converteren van de schijf 
1. Open Hyper-V-beheer en selecteer uw lokale computer aan de linkerkant. Selecteer in het menu boven de lijst met computers, **actie** > **schijf bewerken**.
2. Op de **virtuele hardeschijf zoeken** pagina, selecteert u de virtuele schijf.
3. Op de **Kies actie** weergeeft, schakelt **converteren** > **volgende**.
4. Als u converteren van VHDX wilt, selecteert u **VHD** > **volgende**.
5. Als u converteren van een dynamisch uitbreidbare schijf wilt, selecteert u **vaste grootte** > **volgende**.
6. Zoek en selecteer een pad naar het nieuwe VHD-bestand in.
7. Selecteer **Finish**.

> [!NOTE]
> Een PowerShell-sessie met verhoogde bevoegdheden voor de opdrachten in dit artikel gebruiken.

### <a name="use-powershell-to-convert-the-disk"></a>Gebruik PowerShell om de schijf te converteren 
U kunt een virtuele schijf converteren met behulp van de [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) opdracht in Windows PowerShell. Selecteer **als administrator uitvoeren** wanneer u begint met PowerShell. 

De volgende opdracht converteert de schijf in VHDX naar VHD. De opdracht converteert de schijf ook vanaf een dynamisch uitbreidbare schijf naar een schijf met vaste grootte.

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

In deze opdracht, vervangt u de waarde voor `-Path` met het pad naar de virtuele harde schijf die u wilt converteren. Vervang de waarde voor `-DestinationPath` met het nieuwe pad en de naam van de schijf geconverteerd.

### <a name="convert-from-vmware-vmdk-disk-format"></a>Converteren van VMware VMDK-schijfindeling
Als u een Windows VM-installatiekopie in de [VMDK-bestandsindeling](https://en.wikipedia.org/wiki/VMDK), gebruikt u de [Microsoft Virtual Machine Converter](https://www.microsoft.com/download/details.aspx?id=42497) te converteren naar VHD-indeling. Zie voor meer informatie, [een VMware VMDK converteren naar Hyper-V-VHD](https://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx).

## <a name="set-windows-configurations-for-azure"></a>Windows-configuratie voor Azure instellen

Voer de volgende opdrachten uit op de virtuele machine die u van plan bent om te uploaden naar Azure, een [opdrachtpromptvenster](https://technet.microsoft.com/library/cc947813.aspx):

1. Een statische permanente route in de routeringstabel verwijderen:
   
   * Uitvoeren als u de routetabel, `route print` bij de opdrachtprompt.
   * Controleer de `Persistence Routes` secties. Als er een permanente route, gebruikt u de `route delete` opdracht om deze te verwijderen.
2. Verwijder de WinHTTP-proxy:
   
    ```PowerShell
    netsh winhttp reset proxy
    ```

    Als de virtuele machine werken met een specifieke proxy moet, een proxy-uitzondering toevoegen aan het Azure-IP-adres ([168.63.129.16](https://blogs.msdn.microsoft.com/mast/2015/05/18/what-is-the-ip-address-168-63-129-16/
)), zodat de virtuele machine verbinding met Azure maken kunt:
    ```
    $proxyAddress="<your proxy server>"
    $proxyBypassList="<your list of bypasses>;168.63.129.16"

    netsh winhttp set proxy $proxyAddress $proxyBypassList
    ```

3. De schijf SAN-beleid instellen op [ `Onlineall` ](https://technet.microsoft.com/library/gg252636.aspx):
   
    ```PowerShell
    diskpart 
    ```
    Typ in het venster open de opdrachtprompt de volgende opdrachten:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Tijd van Coordinated Universal Time (UTC) instellen voor Windows. Ook Stel het opstarttype van de Windows time-service (`w32time`) naar `Automatic`:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -name "RealTimeIsUniversal" -Value 1 -Type DWord -force

    Set-Service -Name w32time -StartupType Automatic
    ```
5. De power-profiel instellen op hoge prestaties:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```
6. Zorg ervoor dat de omgevingsvariabelen `TEMP` en `TMP` zijn ingesteld op hun standaardwaarden:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -name "TEMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -name "TMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -force
    ```

## <a name="check-the-windows-services"></a>De Windows-services controleren
Zorg ervoor dat elk van de volgende Windows-services is ingesteld op de Windows-standaardwaarden. Deze services zijn de minimale die moet worden ingesteld om te controleren of de VM-connectiviteit. Als u wilt herstellen van de opstartinstellingen voor, voer de volgende opdrachten:
   
```PowerShell
Set-Service -Name bfe -StartupType Automatic
Set-Service -Name dhcp -StartupType Automatic
Set-Service -Name dnscache -StartupType Automatic
Set-Service -Name IKEEXT -StartupType Automatic
Set-Service -Name iphlpsvc -StartupType Automatic
Set-Service -Name netlogon -StartupType Manual
Set-Service -Name netman -StartupType Manual
Set-Service -Name nsi -StartupType Automatic
Set-Service -Name termService -StartupType Manual
Set-Service -Name MpsSvc -StartupType Automatic
Set-Service -Name RemoteRegistry -StartupType Automatic
```

## <a name="update-remote-desktop-registry-settings"></a>Extern bureaublad-registerinstellingen bijwerken
Zorg ervoor dat de volgende instellingen correct zijn geconfigureerd voor externe toegang:

>[!NOTE] 
>Mogelijk krijgt u een foutbericht weergegeven wanneer u `Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services -name &lt;object name&gt; -value &lt;value&gt;`. U kunt dit bericht negeren. Het betekent alleen dat het domein dat de configuratie via een Group Policy Object is niet pushen.

1. Remote Desktop Protocol (RDP) is ingeschakeld:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0 -Type DWord -force

    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDenyTSConnections" -Value 0 -Type DWord -force
    ```
   
2. De RDP-poort is juist ingesteld. De standaardpoort is 3389:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "PortNumber" -Value 3389 -Type DWord -force
    ```
    Wanneer u een virtuele machine implementeert, worden de standaardregels worden gemaakt op basis van poort 3389. Als u wijzigen van het poortnummer wilt, doet u dat nadat de virtuele machine is geïmplementeerd in Azure.

3. De listener luistert in elke netwerkinterface:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "LanAdapter" -Value 0 -Type DWord -force
   ```
4. De modus voor verificatie op netwerkniveau (NLA) voor de RDP-verbindingen configureren:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1 -Type DWord -force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SecurityLayer" -Value 1 -Type DWord -force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "fAllowSecProtocolNegotiation" -Value 1 -Type DWord -force
     ```

5. Stel de keepalive waarde:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveEnable" -Value 1  -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveInterval" -Value 1  -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "KeepAliveTimeout" -Value 1 -Type DWord -force
    ```
6. Reconnect:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDisableAutoReconnect" -Value 0 -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fInheritReconnectSame" -Value 1 -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fReconnectSame" -Value 0 -Type DWord -force
    ```
7. Beperk het aantal gelijktijdige verbindingen:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "MaxInstanceCount" -Value 4294967295 -Type DWord -force
    ```
8. Geen zelfondertekende certificaten die zijn gekoppeld aan de RDP-listener verwijderen:
    
    ```PowerShell
    Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SSLCertificateSHA1Hash" -force
    ```
    Deze code zorgt ervoor dat u verbinding aan het begin maken kunt bij het implementeren van de virtuele machine. Als u dit later bekijken wilt, kunt u dit doen nadat de virtuele machine is geïmplementeerd in Azure.

9. Als de virtuele machine onderdeel van een domein is, controleert u het volgende beleid om ervoor te zorgen dat de voormalige-instellingen worden niet hersteld. 
    
    | Doel                                     | Beleid                                                                                                                                                       | Value                                                                                    |
    |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
    | RDP is ingeschakeld                           | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Components\Remote bureaublad bureaublad-sessiehost\Verbindingen         | Toestaan dat gebruikers op afstand verbinding maken met behulp van extern bureaublad                                  |
    | NLA-Groepsbeleid                         | Settings\Administrative Templates\Components\Remote Desktop-pc-sessie\Beveiliging                                                    | Gebruikersverificatie voor externe toegang met behulp van NLA vereisen |
    | Instellingen voor actief houden                      | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Windows onderdelen\Extern bureaublad bureaublad-sessiehost\Verbindingen | Interval van keepalive-verbinding configureren                                                 |
    | Opnieuw verbinding maken met instellingen                       | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Windows onderdelen\Extern bureaublad bureaublad-sessiehost\Verbindingen | Automatisch opnieuw verbinding maken                                                                   |
    | Beperkt aantal verbindingsinstellingen | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Windows onderdelen\Extern bureaublad bureaublad-sessiehost\Verbindingen | Aantal verbindingen beperken                                                              |

## <a name="configure-windows-firewall-rules"></a>Windows Firewall-regels configureren
1. Windows Firewall inschakelen op de drie profielen (domein, standard- en openbare):

   ```PowerShell
    Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True
   ```

2. Voer de volgende opdracht in PowerShell om toe te staan van WinRM via de drie firewallprofielen (domain, private en openbare) en de externe PowerShell-service inschakelen:
   
   ```PowerShell
    Enable-PSRemoting -force

    Set-NetFirewallRule -DisplayName "Windows Remote Management (HTTP-In)" -Enabled True
   ```
3. De volgende firewallregels waarmee het RDP-verkeer inschakelen:

   ```PowerShell
    Set-NetFirewallRule -DisplayGroup "Remote Desktop" -Enabled True
   ```   
4. De regel voor bestands- en printerdeling zodat de virtuele machine op een ping-opdracht in het virtuele netwerk reageren kunt inschakelen:

   ```PowerShell
   Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -Enabled True
   ``` 
5. Als de virtuele machine onderdeel van een domein is, controleert u de volgende Azure AD-beleidsregels om te controleren of dat de vorige instellingen worden niet hersteld. 

    | Doel                                 | Beleid                                                                                                                                                  | Value                                   |
    |--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
    | De Windows Firewall-profielen inschakelen | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Firewall   | Beveiligen van alle netwerkverbindingen         |
    | RDP inschakelen                           | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Firewall   | Inkomende extern bureaublad-uitzonderingen toestaan |
    |                                      | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Standard Profile\Windows Firewall | Inkomende extern bureaublad-uitzonderingen toestaan |
    | Schakel ICMP V4                       | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Firewall   | Uitzonderingen voor ICMP toestaan                   |
    |                                      | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Standard Profile\Windows Firewall | Uitzonderingen voor ICMP toestaan                   |

## <a name="verify-the-vm"></a>Controleer of de virtuele machine 

Zorg ervoor dat de virtuele machine is in orde zijn, veilige, en RDP toegankelijk is: 

1. Als u wilt controleren of de schijf is in orde en consistent is, Controleer de schijf op de volgende virtuele machine opnieuw starten:

    ```PowerShell
    Chkdsk /f
    ```
    Zorg ervoor dat het rapport bevat een schijf schone en in orde is.

2. De instellingen voor de Boot Configuration Data (BCD) ingesteld. 

    > [!NOTE]
    > Gebruik een PowerShell-venster met verhoogde bevoegdheid deze opdrachten uitvoeren.
   
   ```powershell
    cmd

    bcdedit /set {bootmgr} integrityservices enable
    bcdedit /set {default} device partition=C:
    bcdedit /set {default} integrityservices enable
    bcdedit /set {default} recoveryenabled Off
    bcdedit /set {default} osdevice partition=C:
    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures

    #Enable Serial Console Feature
    bcdedit /set {bootmgr} displaybootmenu yes
    bcdedit /set {bootmgr} timeout 5
    bcdedit /set {bootmgr} bootems yes
    bcdedit /ems {current} ON
    bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200

    exit
   ```
3. Het logboek dump kan nuttig zijn bij het oplossen van problemen voor Windows-crashes. De logboekverzamelaar dump inschakelen:

    ```powershell
    # Set up the guest OS to collect a kernel dump on an OS crash event
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name CrashDumpEnabled -Type DWord -force -Value 2
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name DumpFile -Type ExpandString -force -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name NMICrashDump -Type DWord -force -Value 1

    # Set up the guest OS to collect user mode dumps on a service crash event
    $key = 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
    if ((Test-Path -Path $key) -eq $false) {(New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting' -Name LocalDumps)}
    New-ItemProperty -Path $key -name DumpFolder -Type ExpandString -force -Value "c:\CrashDumps"
    New-ItemProperty -Path $key -name CrashCount -Type DWord -force -Value 10
    New-ItemProperty -Path $key -name DumpType -Type DWord -force -Value 2
    Set-Service -Name WerSvc -StartupType Manual
    ```
4. Controleer of de Windows Management Instrumentation (WMI)-opslagplaats consistent is:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Als de opslagplaats is beschadigd, Zie [WMI: Opslagplaats beschadigd of niet](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

5. Zorg ervoor dat er geen andere toepassing met behulp van poort 3389. Deze poort wordt gebruikt voor de RDP-service in Azure. Als u wilt zien welke poorten worden gebruikt op de virtuele machine, `netstat -anob`:

    ```PowerShell
    netstat -anob
    ```

6. Het uploaden van een Windows VHD die een domeincontroller:

   * Ga als volgt [deze extra stappen](https://support.microsoft.com/kb/2904015) voorbereiden van de schijf.

   * Zorg ervoor dat u weet dat het wachtwoord van Directory Services Restore Mode (DSRM) in het geval u moet de virtuele machine te starten in DSRM op een bepaald moment. Zie voor meer informatie, [een DSRM-wachtwoord instellen](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx).

7. Zorg ervoor dat u de ingebouwde administrator-account en wachtwoord weet. Mogelijk wilt u de huidige lokale administrator-wachtwoord opnieuw instellen en zorg ervoor dat u kunt dit account gebruiken voor aanmelding bij Windows via de RDP-verbinding. Deze machtiging wordt bepaald door de 'Aanmelden toestaan via Extern bureaublad-Services' Group Policy Object. Dit object in de Editor voor lokaal groepsbeleid hier weergeven:

    Computer\Computerconfiguratie\Windows-instellingen\Beveiligingsinstellingen\Lokaal Beleid\toewijzing

8. Controleer de volgende Azure AD-beleidsregels om te controleren of u bent niet geblokkeerd door uw RDP-toegang via RDP of via het netwerk:

    - Computer Configuration\Windows Settings\Security instellingen\Beveiligingsinstellingen\Lokaal beleid\Toewijzing weigeren toegang tot deze computer vanaf het netwerk

    - Computer Configuration\Windows Settings\Security instellingen\Beveiligingsinstellingen\Lokaal beleid\Toewijzing weigeren aanmelden via Extern bureaublad-Services


9. Controleer het volgende Azure AD-beleid om ervoor te zorgen dat u een van de accounts vereist voor toegang niet verwijderen:

   - Computer Configuration\Windows Settings\Security instellingen\Beveiligingsinstellingen\Lokaal beleid\Toewijzing rechten Assignment\Access deze computer via het netwerk

   Het beleid moet lijst met de volgende groepen:

   - Beheerders

   - Back-upoperators

   - Iedereen

   - Gebruikers

10. Start opnieuw op de virtuele machine om ervoor te zorgen dat Windows nog steeds in orde is en kan worden bereikt via de RDP-verbinding. Op dit moment kunt u een virtuele machine maken in uw lokale Hyper-V om ervoor te zorgen dat de virtuele machine volledig wordt gestart. Vervolgens testen om ervoor te zorgen dat u de virtuele machine via RDP kunt bereiken.

11. Eventuele extra Transport Driver Interface (TDI)-filters te verwijderen. Bijvoorbeeld, software verwijderen die worden geanalyseerd TCP-pakketten of extra firewalls. Als u dit later bekijken wilt, kunt u dit doen nadat de virtuele machine is geïmplementeerd in Azure.

12. Verwijder eventuele andere software van derden of het stuurprogramma dat is gerelateerd aan fysieke onderdelen of andere virtualisatietechnologie bevindt.

### <a name="install-windows-updates"></a>Windows-updates installeren
In het ideale geval moet u de machine bijgewerkt op de *patch niveau*. Als dit niet mogelijk is, zorg er dan voor dat de volgende updates zijn geïnstalleerd:

| Onderdeel               | Binair bestand         | Windows 7 SP1, Windows Server 2008 R2 SP1 | Windows 8, Windows Server 2012               | Windows 8.1, Windows Server 2012 R2 | Windows 10 v1607, Windows Server 2016 v1607 | Windows 10 v1703    | Windows 10 v1709, Windows Server 2016 v1709 | Windows 10 v1803, Windows Server 2016 v1803 |
|-------------------------|----------------|-------------------------------------------|---------------------------------------------|------------------------------------|---------------------------------------------------------|----------------------------|-------------------------------------------------|-------------------------------------------------|
| Opslag                 | disk.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17638 / 6.2.9200.21757 - KB3137061 | 6.3.9600.18203 - KB3137061         | -                                                       | -                          | -                                               | -                                               |
|                         | storport.sys   | 6.1.7601.23403 - KB3125574                | 6.2.9200.17188 / 6.2.9200.21306 - KB3018489 | 6.3.9600.18573 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.332             | -                                               | -                                               |
|                         | ntfs.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17623 / 6.2.9200.21743 - KB3121255 | 6.3.9600.18654 - KB4022726         | 10.0.14393.1198 - KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Iologmsg.dll   | 6.1.7601.23403 - KB3125574                | 6.2.9200.16384 - KB2995387                  | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | Classpnp.sys   | 6.1.7601.23403 - KB3125574                | 6.2.9200.17061 / 6.2.9200.21180 - KB2995387 | 6.3.9600.18334 - KB3172614         | 10.0.14393.953 - KB4022715                              | -                          | -                                               | -                                               |
|                         | Volsnap.sys    | 6.1.7601.23403 - KB3125574                | 6.2.9200.17047 / 6.2.9200.21165 - KB2975331 | 6.3.9600.18265 - KB3145384         | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | partmgr.sys    | 6.1.7601.23403 - KB3125574                | 6.2.9200.16681 - KB2877114                  | 6.3.9600.17401 - KB3000850         | 10.0.14393.953 - KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | volmgr.sys     |                                           |                                             |                                    |                                                         | 10.0.15063.0               | -                                               | -                                               |
|                         | Volmgrx.sys    | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | Msiscsi.sys    | 6.1.7601.23403 - KB3125574                | 6.2.9200.21006 - KB2955163                  | 6.3.9600.18624 - KB4022726         | 10.0.14393.1066 - KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Msdsm.sys      | 6.1.7601.23403 - KB3125574                | 6.2.9200.21474 - KB3046101                  | 6.3.9600.18592 - KB4022726         | -                                                       | -                          | -                                               | -                                               |
|                         | Mpio.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.21190 - KB3046101                  | 6.3.9600.18616 - KB4022726         | 10.0.14393.1198 - KB4022715                             | -                          | -                                               | -                                               |
|                         | vmstorfl.sys   | 6.3.9600.18907 - KB4072650                | 6.3.9600.18080 - KB3063109                  | 6.3.9600.18907 - KB4072650         | 10.0.14393.2007 - KB4345418                             | 10.0.15063.850 - KB4345419 | 10.0.16299.371 - KB4345420                      | -                                               |
|                         | Fveapi.dll     | 6.1.7601.23311 - KB3125574                | 6.2.9200.20930 - KB2930244                  | 6.3.9600.18294 - KB3172614         | 10.0.14393.576 - KB4022715                              | -                          | -                                               | -                                               |
|                         | Fveapibase.dll | 6.1.7601.23403 - KB3125574                | 6.2.9200.20930 - KB2930244                  | 6.3.9600.17415 - KB3172614         | 10.0.14393.206 - KB4022715                              | -                          | -                                               | -                                               |
| Netwerk                 | netvsc.sys     | -                                         | -                                           | -                                  | 10.0.14393.1198 - KB4022715                             | 10.0.15063.250 - KB4020001 | -                                               | -                                               |
|                         | mrxsmb10.sys   | 6.1.7601.23816 - KB4022722                | 6.2.9200.22108 - KB4022724                  | 6.3.9600.18603 - KB4022726         | 10.0.14393.479 - KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb20.sys   | 6.1.7601.23816 - KB4022722                | 6.2.9200.21548 - KB4022724                  | 6.3.9600.18586 - KB4022726         | 10.0.14393.953 - KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb.sys     | 6.1.7601.23816 - KB4022722                | 6.2.9200.22074 - KB4022724                  | 6.3.9600.18586 - KB4022726         | 10.0.14393.953 - KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | tcpip.sys      | 6.1.7601.23761 - KB4022722                | 6.2.9200.22070 - KB4022724                  | 6.3.9600.18478 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | http.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17285 - KB3042553                  | 6.3.9600.18574 - KB4022726         | 10.0.14393.251 - KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | vmswitch.sys   | 6.1.7601.23727 - KB4022719                | 6.2.9200.22117 - KB4022724                  | 6.3.9600.18654 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.138             | -                                               | -                                               |
| Core                    | ntoskrnl.exe   | 6.1.7601.23807 - KB4022719                | 6.2.9200.22170 - KB4022718                  | 6.3.9600.18696 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.483             | -                                               | -                                               |
| Externe bureaubladservices | rdpcorets.dll  | 6.2.9200.21506 - KB4022719                | 6.2.9200.22104 - KB4022724                  | 6.3.9600.18619 - KB4022726         | 10.0.14393.1198 - KB4022715                             | 10.0.15063.0               | -                                               | -                                               |
|                         | termsrv.dll    | 6.1.7601.23403 - KB3125574                | 6.2.9200.17048 - KB2973501                  | 6.3.9600.17415 - KB3000850         | 10.0.14393.0 - KB4022715                                | 10.0.15063.0               | -                                               | -                                               |
|                         | termdd.sys     | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | win32k.sys     | 6.1.7601.23807 - KB4022719                | 6.2.9200.22168 - KB4022718                  | 6.3.9600.18698 - KB4022726         | 10.0.14393.594 - KB4022715                              | -                          | -                                               | -                                               |
|                         | rdpdd.dll      | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | rdpwd.sys      | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
| Beveiliging                | MS17-010       | KB4012212                                 | KB4012213                                   | KB4012213                          | KB4012606                                               | KB4012606                  | -                                               | -                                               |
|                         |                |                                           | KB4012216                                   |                                    | KB4013198                                               | KB4013198                  | -                                               | -                                               |
|                         |                | KB4012215                                 | KB4012214                                   | KB4012216                          | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         |                |                                           | KB4012217                                   |                                    | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         | CVE-2018-0886  | KB4103718               | KB4103730                | KB4103725       | KB4103723                                               | KB4103731                  | KB4103727                                       | KB4103721                                       |
|                         |                | KB4103712          | KB4103726          | KB4103715|                                                         |                            |                                                 |                                                 |
       
### Bepalen wanneer Sysprep gebruiken <a id="step23"></a>    

Hulpprogramma voor systeemvoorbereiding (Sysprep) is een proces kunt u als u een Windows-installatie opnieuw wilt uitvoeren. Sysprep biedt een 'standaard'-ervaring door het verwijderen van alle persoonlijke gegevens en het opnieuw instellen van verschillende onderdelen. 

U normaal gesproken voert u Sysprep uit om te maken van een sjabloon van waaruit u verschillende andere virtuele machines waarvoor een specifieke configuratie kunt implementeren. De sjabloon heet een *gegeneraliseerde installatiekopie*.

Als u slechts één virtuele machine maken van de ene schijf wilt, hebt u geen gebruik van Sysprep. In plaats daarvan kunt u de virtuele machine uit een *gespecialiseerde installatiekopie*. Zie voor informatie over het maken van een virtuele machine van een gespecialiseerde schijf:

- [Een virtuele machine maken vanaf een gespecialiseerde schijf](create-vm-specialized.md)
- [Een virtuele machine maken vanaf een gespecialiseerde VHD-schijf](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal?branch=master)

Als u maken van een gegeneraliseerde installatiekopie wilt, moet u Sysprep uitvoeren. Zie voor meer informatie, [Sysprep gebruiken: Een inleiding](https://technet.microsoft.com/library/bb457073.aspx). 

Ondersteunt de gegeneraliseerde installatiekopieën niet elke rol of de toepassing die geïnstalleerd op een Windows-computer. Dus voordat u deze procedure uitvoert, zorg ervoor dat Sysprep biedt ondersteuning voor de rol van de computer. Zie voor meer informatie, [Sysprep-ondersteuning voor serverfuncties](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="generalize-a-vhd"></a>Generaliseren van een VHD

>[!NOTE]
> Nadat u hebt uitgevoerd `sysprep.exe` uitschakelen in de volgende stappen uit de virtuele machine. Niet weer inschakelen totdat u een installatiekopie van het maken in Azure.

1. Aanmelden bij de Windows-VM.
1. Voer **opdrachtprompt** als beheerder. 
1. Wijzig de map in `%windir%\system32\sysprep`. Voer vervolgens `sysprep.exe` uit.
1. In het dialoogvenster **Hulpprogramma voor systeemvoorbereiding** selecteert u **OOBE (Out-of-Box Experience) van systeem starten** en zorgt u dat het selectievakje **Generaliseren** is ingeschakeld.

    ![Hulpprogramma voor systeemvoorbereiding](media/prepare-for-upload-vhd-image/syspre.png)
1. In **afsluitopties**, selecteer **afsluiten**.
1. Selecteer **OK**.
1. Als Sysprep is voltooid, sluit u de virtuele machine. Gebruik geen **opnieuw** om de virtuele machine af te sluiten.

De VHD is nu gereed om te worden geüpload. Zie voor meer informatie over het maken van een virtuele machine vanaf een gegeneraliseerde schijf [een gegeneraliseerde VHD uploaden en maken van een nieuwe virtuele machine in Azure](sa-upload-generalized.md).


>[!NOTE]
> Een aangepaste *unattend.xml* bestand wordt niet ondersteund. Hoewel we bieden ondersteuning voor de `additionalUnattendContent` eigenschap, die alleen beperkte ondersteuning voor het toevoegen van biedt [microsoft-windows-shell-setup](https://docs.microsoft.com/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup) opties in de *unattend.xml* bestand dat het Azure inrichten Agent gebruikt. U kunt gebruiken, bijvoorbeeld [additionalUnattendContent](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.compute.models.additionalunattendcontent?view=azure-dotnet) FirstLogonCommands en LogonCommands toe te voegen. Zie voor meer informatie, [additionalUnattendContent FirstLogonCommands voorbeeld](https://github.com/Azure/azure-quickstart-templates/issues/1407).


## <a name="complete-the-recommended-configurations"></a>De aanbevolen configuratie-instellingen
De volgende instellingen niet van invloed op VHD uploaden. Echter, wordt aangeraden dat u deze hebt geconfigureerd.

* Installeer de [Azure Virtual Machineagent](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Vervolgens kunt u VM-extensies. De VM-extensies implementeren voor de meeste van de essentiële functionaliteit die u mogelijk wilt gebruiken met uw VM's. U moet de extensies, bijvoorbeeld: wachtwoorden opnieuw instellen of configureren van RDP. Zie voor meer informatie, [overzicht van Azure Virtual Machine Agent](../extensions/agent-windows.md).
* Nadat u de virtuele machine in Azure maakt, wordt aangeraden dat u het bestand met pagina plaatsen op de *tijdelijke stationsvolume* om prestaties te verbeteren. U kunt als volgt de bestandsplaatsing instellen:

   ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -name "PagingFiles" -Value "D:\pagefile.sys" -Type MultiString -force
   ```
  Als een gegevensschijf is gekoppeld aan de virtuele machine, de tijdelijke schijf volumeletter is doorgaans *D*. Deze aanwijzing mogelijk verschillen, afhankelijk van uw instellingen en het aantal beschikbare stations.

## <a name="next-steps"></a>Volgende stappen
* [Een Windows VM-installatiekopie uploaden naar Azure voor Resource Manager-implementaties](upload-generalized-managed.md)
* [Problemen met Azure Windows VM-activering](troubleshoot-activation-problems.md)

