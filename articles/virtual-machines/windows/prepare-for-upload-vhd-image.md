---
title: Voorbereiden van een Windows-VHD te uploaden naar Azure | Microsoft Docs
description: Een Windows VHD of VHDX voorbereiden voordat u uploadt naar Azure
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
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 9eb9984d99b907cd73f5f667cca41496127744e9
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263511"
---
# <a name="prepare-a-windows-vhd-or-vhdx-to-upload-to-azure"></a>Een Windows VHD of VHDX te uploaden naar Azure voorbereiden
Voordat u een Windows virtuele machines (VM van on-premises met Microsoft Azure) uploadt, moet u de virtuele harde schijf (VHD of VHDX) voorbereiden. Azure ondersteunt alleen generation 1 VM's die in de VHD-indeling en hebben een schijf met vaste grootte. De maximale grootte van de VHD is 1023 GB. U kunt een generatie 1 VM op basis van de VHDX-bestandssysteem en naar een dynamisch uitbreidbare schijf naar vaste VHD converteren. Maar u kunt een virtuele machine generatie niet wijzigen. Zie voor meer informatie, [maak ik een generatie 1 of 2 virtuele machine in Hyper-V](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

Zie voor meer informatie over het ondersteuningsbeleid voor voor Azure-VM [ondersteuning voor Microsoft Azure-VM's van Microsoft-serversoftware](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

> [!Note]
> De instructies in dit artikel zijn van toepassing op de 64-bits versie van Windows Server 2008 R2 en hoger Windows server-besturingssysteem. Zie voor meer informatie over de 32-bits versie van besturingssysteem in Azure [ondersteuning voor 32-bits besturingssystemen in virtuele Azure-machines](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines).

## <a name="convert-the-virtual-disk-to-vhd-and-fixed-size-disk"></a>De virtuele schijf converteren naar VHD en schijf met vaste grootte 
Als u converteren van de virtuele schijf naar de vereiste indeling voor Azure wilt, kunt u een van de methoden in deze sectie gebruiken. Maak een back-up van de virtuele machine voordat u de conversie van virtuele schijf uitvoert en zorg ervoor dat werkt de VHD met Windows correct op de lokale server. Los eventuele fouten in de virtuele machine zelf voordat u probeert te converteren of uploaden naar Azure.

Nadat u de schijf converteren, moet u een virtuele machine die gebruikmaakt van de geconverteerde schijf maken. Start en zich aanmelden bij de virtuele machine om te voltooien voor het uploaden van de virtuele machine wordt voorbereid.

### <a name="convert-disk-using-hyper-v-manager"></a>Converteren van de schijf met behulp van Hyper-V-beheer
1. Open Hyper-V-beheer en selecteer uw lokale computer aan de linkerkant. Klik in het menu boven de lijst met computers op **actie** > **schijf bewerken**.
2. Op de **virtuele hardeschijf zoeken** scherm, zoekt en selecteert u de virtuele schijf.
3. Op de **Kies actie** scherm en selecteer vervolgens **converteren** en **volgende**.
4. Als u converteren van VHDX wilt, selecteert u **VHD** en klik vervolgens op **volgende**
5. Als u converteren van een dynamisch uitbreidbare schijf wilt, selecteert u **vaste grootte** en klik vervolgens op **volgende**
6. Zoek en selecteer een pad naar het nieuwe VHD-bestand in.
7. Klik op **Voltooien**.

>[!NOTE]
>De opdrachten in dit artikel moeten worden uitgevoerd op een PowerShell-sessie met verhoogde bevoegdheden.

### <a name="convert-disk-by-using-powershell"></a>Schijf converteren met behulp van PowerShell
U kunt een virtuele schijf converteren met behulp van de [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) opdracht in Windows PowerShell. Selecteer **als administrator uitvoeren** wanneer u begint met PowerShell. 

De volgende voorbeeldopdracht wordt geconverteerd van VHDX, zodat de VHD en van een dynamisch uitbreidbare schijf naar een vaste grootte:

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```
In deze opdracht, vervangt u de waarde voor '-pad ' door het pad naar de virtuele harde schijf die u wilt converteren en de waarde voor '-doelpad ' met het nieuwe pad en de naam van de schijf geconverteerd.

### <a name="convert-from-vmware-vmdk-disk-format"></a>Converteren van VMware VMDK-schijfindeling
Als u een Windows VM-installatiekopie in de [VMDK-bestandsindeling](https://en.wikipedia.org/wiki/VMDK), converteren naar een VHD met behulp van de [Microsoft VM Converter](https://www.microsoft.com/download/details.aspx?id=42497). Zie voor meer informatie het blogartikel [een VMware VMDK converteren naar Hyper-V-VHD](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx).

## <a name="set-windows-configurations-for-azure"></a>Windows-configuratie voor Azure instellen

Voer alle opdrachten in de volgende stappen uit op de virtuele machine die u van plan bent om te uploaden naar Azure, een [opdrachtprompt met verhoogde bevoegdheid](https://technet.microsoft.com/library/cc947813.aspx):

1. Een statische permanente route in de routeringstabel verwijderen:
   
   * Uitvoeren als u de routetabel, `route print` bij de opdrachtprompt.
   * Controleer de **persistentie Routes** secties. Als er een permanente route, gebruikt u [route delete](https://technet.microsoft.com/library/cc739598.apx) om deze te verwijderen.
2. Verwijder de WinHTTP-proxy:
   
    ```PowerShell
    netsh winhttp reset proxy
    ```
3. De schijf SAN-beleid instellen op [Onlineall](https://technet.microsoft.com/library/gg252636.aspx). 
   
    ```PowerShell
    diskpart 
    ```
    Typ in het venster open Command Prompt de volgende opdrachten:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Tijd van Coordinated Universal Time (UTC) instellen voor Windows en het opstarttype van de Windows Time (w32time)-service **automatisch**:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -name "RealTimeIsUniversal" 1 -Type DWord

    Set-Service -Name w32time -StartupType Auto
    ```
5. Het profiel power ingesteld op de **hoge prestaties**:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```

## <a name="check-the-windows-services"></a>De Windows-services controleren
Zorg ervoor dat elk van de volgende Windows-services is ingesteld op de **Windows standaardwaarden**. Dit zijn de minimale nummers van de services die moeten worden ingesteld om ervoor te zorgen dat de VM verbinding heeft. Als u wilt herstellen van de opstartinstellingen voor, voer de volgende opdrachten:
   
```PowerShell
Set-Service -Name bfe -StartupType Auto
Set-Service -Name dhcp -StartupType Auto
Set-Service -Name dnscache -StartupType Auto
Set-Service -Name IKEEXT -StartupType Auto
Set-Service -Name iphlpsvc -StartupType Auto
Set-Service -Name netlogon -StartupType Manual
Set-Service -Name netman -StartupType Manual
Set-Service -Name nsi -StartupType Auto
Set-Service -Name termService -StartupType Manual
Set-Service -Name MpsSvc -StartupType Auto
Set-Service -Name RemoteRegistry -StartupType Auto
```

## <a name="update-remote-desktop-registry-settings"></a>Instellingen voor extern bureaublad-register bijwerken
Zorg ervoor dat de volgende instellingen correct zijn geconfigureerd voor verbinding met extern bureaublad:

>[!Note] 
>U wordt een foutbericht weergegeven wanneer u de **Set-ItemProperty-pad ' HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services - naam &lt;objectnaam&gt; &lt;waarde&gt;** in deze stappen. Het foutbericht kan veilig worden genegeerd. Het betekent alleen dat het domein dat de configuratie via een Group Policy object niet pusht.
>
>

1. Remote Desktop Protocol (RDP) is ingeschakeld:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDenyTSConnections" -Value 0 -Type DWord
    ```
   
2. De RDP-poort juist is ingesteld (standaard poort 3389):
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "PortNumber" 3389 -Type DWord
    ```
    Wanneer u een virtuele machine implementeert, worden de standaardregels worden gemaakt op basis van poort 3389. Als u wijzigen van het poortnummer wilt, doet u dat nadat de virtuele machine is geïmplementeerd in Azure.

3. De listener luistert in elke netwerkinterface:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "LanAdapter" 0 -Type DWord
   ```
4. De modus voor verificatie op netwerkniveau voor de RDP-verbindingen configureren:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" 1 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SecurityLayer" 1 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "fAllowSecProtocolNegotiation" 1 -Type DWord
     ```

5. Stel de keepalive waarde:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveEnable" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveInterval" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "KeepAliveTimeout" 1 -Type DWord
    ```
6. Reconnect：
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDisableAutoReconnect" 0 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fInheritReconnectSame" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fReconnectSame" 0 -Type DWord
    ```
7. Beperk het aantal gelijktijdige verbindingen:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "MaxInstanceCount" 4294967295 -Type DWord
    ```
8. Als er geen zelfondertekende certificaten die zijn gekoppeld aan de RDP-listener, deze verwijderen:
    
    ```PowerShell
    Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SSLCertificateSHA1Hash"
    ```
    Dit is om ervoor te zorgen dat u verbinding aan het begin maken kunt bij het implementeren van de virtuele machine. U kunt ook bekijkt u deze op een later stadium nadat de virtuele machine is geïmplementeerd in Azure, indien nodig.

9. Als de virtuele machine onderdeel van een domein is, controleert u de volgende instellingen om ervoor te zorgen dat de voormalige instellingen worden niet hersteld. Het beleid dat moeten worden gecontroleerd op zijn de volgende:
    
    | Doel                                     | Beleid                                                                                                                                                       | Waarde                                                                                    |
    |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
    | RDP is ingeschakeld                           | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Components\Remote bureaublad bureaublad-sessiehost\Verbindingen         | Toestaan dat gebruikers op afstand verbinding maken met behulp van extern bureaublad                                  |
    | NLA-Groepsbeleid                         | Settings\Administrative Templates\Components\Remote Desktop-pc-sessie\Beveiliging                                                    | Gebruiker-verificatie voor externe verbindingen met behulp van verificatie op netwerkniveau vereisen |
    | Instellingen voor actief houden                      | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Windows onderdelen\Extern bureaublad bureaublad-sessiehost\Verbindingen | Interval van keepalive-verbinding configureren                                                 |
    | Opnieuw verbinding maken met instellingen                       | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Windows onderdelen\Extern bureaublad bureaublad-sessiehost\Verbindingen | Automatisch opnieuw verbinden                                                                   |
    | Beperk het aantal verbindingsinstellingen | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Windows onderdelen\Extern bureaublad bureaublad-sessiehost\Verbindingen | Aantal verbindingen beperken                                                              |

## <a name="configure-windows-firewall-rules"></a>Windows Firewall-regels configureren
1. Windows Firewall inschakelen op de drie profielen (domein-, Standard- en openbare):

   ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile' -name "EnableFirewall" -Value 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile' -name "EnableFirewall" -Value 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\Standardprofile' -name "EnableFirewall" -Value 1 -Type DWord
   ```

2. Voer de volgende opdracht in PowerShell WinRM toestaan via de drie firewallprofielen (Domain, Private en openbare) en de externe PowerShell-service inschakelen:
   
   ```PowerShell
    Enable-PSRemoting -force
    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
   ```
3. De volgende firewallregels waarmee het RDP-verkeer inschakelen 

   ```PowerShell
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes
   ```   
4. Bestands- en printerdeling regel inschakelen zodat de virtuele machine op een ping-opdracht in het Virtueelnetwerk reageren kan:

   ```PowerShell
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes
   ``` 
5. Als de virtuele machine onderdeel van een domein is, controleert u de volgende instellingen om ervoor te zorgen dat de voormalige instellingen worden niet hersteld. De AD-beleidsregels die moeten worden gecontroleerd op zijn de volgende:

    | Doel                                 | Beleid                                                                                                                                                  | Waarde                                   |
    |--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
    | De Windows Firewall-profielen inschakelen | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Firewall   | Beveiligen van alle netwerkverbindingen         |
    | RDP inschakelen                           | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Firewall   | Inkomende extern bureaublad-uitzonderingen toestaan |
    |                                      | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Standard Profile\Windows Firewall | Inkomende extern bureaublad-uitzonderingen toestaan |
    | Schakel ICMP V4                       | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Firewall   | Uitzonderingen voor ICMP toestaan                   |
    |                                      | Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Standard Profile\Windows Firewall | Uitzonderingen voor ICMP toestaan                   |

## <a name="verify-vm-is-healthy-secure-and-accessible-with-rdp"></a>Controleer of dat de VM bevindt zich in orde, veilige en toegankelijk zijn met RDP 
1. Om te controleren of dat de schijf is in orde en consistente, een schijfbewerking controle worden uitgevoerd op de volgende virtuele machine opnieuw op te starten:

    ```PowerShell
    Chkdsk /f
    ```
    Zorg ervoor dat het rapport een schijf schone en in orde is bevat.

2. De instellingen voor de Boot Configuration Data (BCD) ingesteld. 

    > [!Note]
    > Zorg ervoor dat u deze opdrachten uitvoeren in een verhoogde CMD-venster en **niet** op PowerShell:
   
   ```CMD
   bcdedit /set {bootmgr} integrityservices enable
   
   bcdedit /set {default} device partition=C:
   
   bcdedit /set {default} integrityservices enable
   
   bcdedit /set {default} recoveryenabled Off
   
   bcdedit /set {default} osdevice partition=C:
   
   bcdedit /set {default} bootstatuspolicy IgnoreAllFailures

   #Enable Serial Console Feature

    bcdedit /set {bootmgr} displaybootmenu yes

    bcdedit /set {bootmgr} timeout 10

    bcdedit /set {bootmgr} bootems yes

    bcdedit /ems {<<BOOT LOADER IDENTIFIER>>} ON

    bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200

    #Setup the Guest OS to collect a kernel dump on an OS crash event

    REG ADD "HKLM\SYSTEM\ControlSet00x\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2

    REG ADD "HKLM\SYSTEM\ControlSet00x\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP"

    REG ADD "HKLM\SYSTEM\ControlSet00x\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1
   ```
3. Controleer of de Windows Management Instrumentation-opslagplaats consistent is. Als u wilt uitvoeren, moet u de volgende opdracht uitvoeren:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Als de opslagplaats is beschadigd, Zie [WMI: beschadiging van de opslagplaats, of niet](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

4. Zorg ervoor dat alle andere toepassingen geen gebruik van de poort 3389 maakt. Deze poort wordt gebruikt voor de RDP-service in Azure. U kunt uitvoeren **netstat - anob** om te zien welke poorten zijn in gebruikt op de virtuele machine:

    ```PowerShell
    netstat -anob
    ```

5. Als de Windows-VHD die u wilt uploaden een domeincontroller is, volgt u deze stappen:

    A. Ga als volgt [deze extra stappen](https://support.microsoft.com/kb/2904015) voorbereiden van de schijf.

    B. Zorg ervoor dat u kent het DSRM-wachtwoord in het geval u moet de virtuele machine te starten in DSRM op een bepaald moment. U kunt om te verwijzen naar deze koppeling om in te stellen de [DSRM-wachtwoord](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx).

6. Zorg ervoor dat de ingebouwde Administrator-account en het wachtwoord bekend zijn voor u. Mogelijk wilt de huidige lokale administrator-wachtwoord opnieuw instellen en zorg ervoor dat u dit account gebruiken kunt voor aanmelding bij Windows via de RDP-verbinding. Deze machtiging wordt bepaald door het groepsbeleidsobject 'Aanmelden toestaan via Extern bureaublad-Services'. U kunt dit object in de Editor voor lokaal groepsbeleid onder bekijken:

    Computer\Computerconfiguratie\Windows-instellingen\Beveiligingsinstellingen\Lokaal Beleid\toewijzing

7. Controleer dat de volgende AD-beleidsregels om ervoor te zorgen dat u niet uw RDP-toegang via RDP, en niet in het netwerk blokkeert:

    - Computer Configuration\Windows Settings\Security instellingen\Beveiligingsinstellingen\Lokaal beleid\Toewijzing weigeren toegang tot deze computer vanaf het netwerk

    - Computer Configuration\Windows Settings\Security instellingen\Beveiligingsinstellingen\Lokaal beleid\Toewijzing weigeren aanmelden via Extern bureaublad-Services


8. Opnieuw opstarten van de virtuele machine om ervoor te zorgen dat Windows nog steeds in orde is, kan worden bereikt met behulp van de RDP-verbinding. Op dit moment kunt u een virtuele machine maken in uw lokale Hyper-V Zorg ervoor dat de virtuele machine volledig is gestart en vervolgens controleren of het RDP-bereikbaar is.

9. Verwijder eventuele extra filters voor het Transport Driver Interface, zoals software waarmee worden geanalyseerd TCP-pakketten of extra firewalls. U kunt ook bekijkt u deze op een later stadium nadat de virtuele machine is geïmplementeerd in Azure, indien nodig.

10. Verwijder eventuele andere software van derden en stuurprogramma's met betrekking tot fysieke onderdelen of andere virtualisatietechnologie bevindt.

### <a name="install-windows-updates"></a>Windows-Updates installeren
De ideale configuratie is het **hebben van de patch-niveau van de machine op de meest recente**. Als dit niet mogelijk is, zorg ervoor dat de volgende updates zijn geïnstalleerd:

| Onderdeel               | Binair bestand         | Windows 7 SP1, Windows Server 2008 R2 SP1 | Windows 8, WindowsServer 2012               | Windows 8.1, Windows Server 2012 R2 | Windows 10 versie 1607 voor 1607Windows Server 2016-versie | Windows 10 versie 1703    | Windows 10 1709 Windows Server 2016-versie 1709 | Windows 10 1803Windows Server 2016, versie 1803 |
|-------------------------|----------------|-------------------------------------------|---------------------------------------------|------------------------------------|---------------------------------------------------------|----------------------------|-------------------------------------------------|-------------------------------------------------|
| Storage                 | Disk.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17638 / 6.2.9200.21757 - KB3137061 | 6.3.9600.18203 - KB3137061         | -                                                       | -                          | -                                               | -                                               |
|                         | Storport.sys   | 6.1.7601.23403 - KB3125574                | 6.2.9200.17188 / 6.2.9200.21306 - KB3018489 | 6.3.9600.18573 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.332             | -                                               | -                                               |
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
| Kern                    | ntoskrnl.exe   | 6.1.7601.23807 - KB4022719                | 6.2.9200.22170 - KB4022718                  | 6.3.9600.18696 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.483             | -                                               | -                                               |
| Externe bureaubladservices | rdpcorets.dll  | 6.2.9200.21506 - KB4022719                | 6.2.9200.22104 - KB4022724                  | 6.3.9600.18619 - KB4022726         | 10.0.14393.1198 - KB4022715                             | 10.0.15063.0               | -                                               | -                                               |
|                         | termsrv.dll    | 6.1.7601.23403 - KB3125574                | 6.2.9200.17048 - KB2973501                  | 6.3.9600.17415 - KB3000850         | 10.0.14393.0 - KB4022715                                | 10.0.15063.0               | -                                               | -                                               |
|                         | termdd.sys     | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | Win32k.sys     | 6.1.7601.23807 - KB4022719                | 6.2.9200.22168 - KB4022718                  | 6.3.9600.18698 - KB4022726         | 10.0.14393.594 - KB4022715                              | -                          | -                                               | -                                               |
|                         | rdpdd.dll      | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | rdpwd.sys      | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
| Beveiliging                | MS17-010       | KB4012212                                 | KB4012213                                   | KB4012213                          | KB4012606                                               | KB4012606                  | -                                               | -                                               |
|                         |                |                                           | KB4012216                                   |                                    | KB4013198                                               | KB4013198                  | -                                               | -                                               |
|                         |                | KB4012215                                 | KB4012214                                   | KB4012216                          | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         |                |                                           | KB4012217                                   |                                    | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         | CVE-2018-0886  | KB4103718               | KB4103730                | KB4103725       | KB4103723                                               | KB4103731                  | KB4103727                                       | KB4103721                                       |
|                         |                | KB4103712          | KB4103726          | KB4103715|                                                         |                            |                                                 |                                                 |
       
### Wanneer u sysprep <a id="step23"></a>    

Sysprep is een proces dat u in een windows-installatie die wordt opnieuw ingesteld voor de installatie van het systeem en biedt een uitvoeren kunt ' de box experience' door het verwijderen van alle persoonlijke gegevens en het opnieuw instellen van verschillende onderdelen. U doen dit meestal als u wilt maken van een sjabloon van waaruit u verschillende andere virtuele machines waarvoor een specifieke configuratie kunt implementeren. Dit heet een **gegeneraliseerde installatiekopie**.

Als u in plaats daarvan wilt slechts aan één virtuele machine maken van één schijf, moet u geen sysprep te gebruiken. In dit geval kunt u alleen de virtuele machine maken van wat wordt ook wel een **gespecialiseerde installatiekopie**.

Zie voor meer informatie over het maken van een virtuele machine van een gespecialiseerde schijf:

- [Een virtuele machine maken vanaf een gespecialiseerde schijf](create-vm-specialized.md)
- [Een virtuele machine maken vanaf een gespecialiseerde VHD-schijf](https://review.docs.microsoft.com/en-us/azure/virtual-machines/windows/create-vm-specialized-portal?branch=master)

Als u maken van een gegeneraliseerde installatiekopie wilt, moet u sysprep uitvoeren. Zie voor meer informatie over Sysprep [Sysprep gebruiken: een inleiding](http://technet.microsoft.com/library/bb457073.aspx). 

Biedt ondersteuning voor deze generalisatie niet elke rol of de toepassing die geïnstalleerd op een Windows-computer. Dus voordat u deze procedure uitvoert, Zie het volgende artikel om ervoor te zorgen dat de rol van die computer wordt ondersteund door sysprep. Voor meer informatie, [Sysprep-ondersteuning voor serverfuncties](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="steps-to-generalize-a-vhd"></a>Stappen voor het generaliseren van een VHD

>[!NOTE]
> Na het uitvoeren van sysprep.exe zoals opgegeven in de volgende stappen uit, de virtuele machine uitschakelen en kan niet weer inschakelen totdat u een installatiekopie van het maken in Azure.

1. Aanmelden bij de Windows-VM.
2. Voer **opdrachtprompt** als beheerder. 
3. Wijzig de map in: **%windir%\system32\sysprep**, en voer **sysprep.exe**.
3. In het dialoogvenster **Hulpprogramma voor systeemvoorbereiding** selecteert u **OOBE (Out-of-Box Experience) van systeem starten** en zorgt u dat het selectievakje **Generaliseren** is ingeschakeld.

    ![Hulpprogramma voor systeemvoorbereiding](media/prepare-for-upload-vhd-image/syspre.png)
4. In **afsluitopties**, selecteer **afsluiten**.
5. Klik op **OK**.
6. Als Sysprep is voltooid, sluit u de virtuele machine. Gebruik geen **opnieuw** om de virtuele machine af te sluiten.
7. De VHD is nu gereed om te worden geüpload. Zie voor meer informatie over het maken van een virtuele machine vanaf een gegeneraliseerde schijf [een gegeneraliseerde VHD uploaden en maken van een nieuwe virtuele machines in Azure](sa-upload-generalized.md).


## <a name="complete-recommended-configurations"></a>Voert u de aanbevolen configuraties
De volgende instellingen hebben geen invloed op de VHD uploaden. Echter, wordt aangeraden dat u deze hebt geconfigureerd.

* Installeer de [Azure VM's Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Vervolgens kunt u VM-extensies. De VM-extensies implementeren de meeste van de essentiële functionaliteit die u mogelijk wilt gebruiken met uw VM opnieuw instellen van wachtwoorden, zoals het configureren van RDP's, enzovoort. Zie voor meer informatie:

    - [VM-Agent en -extensies – deel 1](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-1/)
    - [VM-Agent en -extensies – deel 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)
* Het logboek Dump kan nuttig zijn bij het oplossen van problemen voor Windows-crashes. De logboekverzamelaar Dump inschakelen:
  
    ```cmd
    md c:\CrashDumps
    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f
    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f
    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f
    sc config WerSvc start= demand
    ```
    Als u eventuele fouten tijdens de procedure stappen in dit artikel ontvangt, betekent dit dat de registersleutels al bestaat. In dit geval gebruikt u de volgende opdrachten:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "CrashDumpEnable" -Value "2" -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "DumpFile" -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpFolder" -Value "c:\CrashDumps"
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpCount" -Value 10 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpType" -Value 2 -Type DWord
    Set-Service -Name WerSvc -StartupType Manual
    ```
*  Nadat de virtuele machine is gemaakt in Azure, wordt u aangeraden dat u het wisselbestand op het stationsvolume 'Tijdelijke plaatsen' om prestaties te verbeteren. U kunt instellen dit als volgt:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -name "PagingFiles" -Value "D:\pagefile"
    ```
Als er een gegevensschijf die is gekoppeld aan de virtuele machine, is de stationsletter van het volume van de tijdelijke schijf doorgaans "D." Deze aanwijzing mogelijk verschillen, afhankelijk van het aantal beschikbare stations en de instellingen die u aanbrengt.

## <a name="next-steps"></a>Volgende stappen
* [Een Windows VM-installatiekopie uploaden naar Azure voor Resource Manager-implementaties](upload-generalized-managed.md)
* [Problemen met Windows Azure virtuele machine-activering](troubleshoot-activation-problems.md)

