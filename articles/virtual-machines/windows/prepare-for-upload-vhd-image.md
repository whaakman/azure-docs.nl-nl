---
title: Voorbereiden van een Windows-VHD te uploaden naar Azure | Microsoft Docs
description: Het voorbereiden van een Windows-VHD of VHDX voordat u uploadt naar Azure
services: virtual-machines-windows
documentationcenter: 
author: glimoli
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 08/01/2017
ms.author: genli
ms.openlocfilehash: 60da4d7f418e2aac6ed6d41092486d2ccdaf940c
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2017
---
# <a name="prepare-a-windows-vhd-or-vhdx-to-upload-to-azure"></a>Een Windows-VHD of VHDX uploaden naar Azure voorbereiden
Voordat u een virtuele Windows-machines (VM) on-premises met Microsoft Azure uploaden, moet u de virtuele harde schijf (VHD of VHDX) voorbereiden. Azure ondersteunt alleen generatie 1 virtuele machines die in de VHD-indeling en een schijf met vaste grootte hebben. De maximale grootte van de VHD is 1023 GB. U kunt een generatie 1 VM van de VHDX-bestandssysteem VHD en naar een vaste grootte en dynamisch uitbreidbare schijf converteren. Maar als u de generatie van een virtuele machine niet wijzigen. Zie voor meer informatie [maak ik een generatie 1 of 2 VM in Hyper-V](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

Zie voor meer informatie over het ondersteuningsbeleid voor virtuele machine van Azure [Microsoft server software-ondersteuning voor Microsoft Azure Virtual machines](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

> [!Note]
> De instructies in dit artikel van toepassing op de 64-bits versie van Windows Server 2008 R2 en hoger Windows server-besturingssysteem. Zie voor informatie over de uitgevoerde 32-bits versie van besturingssysteem in Azure, [ondersteuning voor 32-bits besturingssystemen in virtuele machines in Azure](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines).

## <a name="convert-the-virtual-disk-to-vhd-and-fixed-size-disk"></a>De virtuele schijf converteren naar VHD en de schijf met vaste grootte 
Als u converteren van de virtuele schijf naar de vereiste indeling voor Azure wilt, kunt u een van de methoden in deze sectie gebruiken. Maak een back-up van de virtuele machine voordat u de conversie van virtuele schijf uitvoert en zorg ervoor dat werkt de Windows-VHD correct op de lokale server. Eventuele fouten in de virtuele machine zelf oplossen voordat u probeert te converteren of uploaden naar Azure.

Nadat u de schijf converteert, maakt u een virtuele machine die gebruikmaakt van de geconverteerde schijf. Start en aanmelden met de virtuele machine bij het voltooien van de virtuele machine voorbereiden voor het uploaden.

### <a name="convert-disk-using-hyper-v-manager"></a>Converteert de schijf met behulp van Hyper-V-beheer
1. Open Hyper-V-beheer en selecteer uw lokale computer aan de linkerkant. Klik in het menu boven de computerlijst op **actie** > **schijf bewerken**.
2. Op de **virtuele hardeschijf zoeken** scherm, zoekt en selecteert u de virtuele schijf.
3. Op de **actie kiezen** scherm en selecteer vervolgens **converteren** en **volgende**.
4. Als u converteren van VHDX wilt, selecteert u **VHD** en klik vervolgens op **volgende**
5. Als u converteren van een dynamisch uitbreidbare schijf wilt, selecteert u **een vaste grootte** en klik vervolgens op **volgende**
6. Zoek en selecteer een pad naar het nieuwe VHD-bestand in.
7. Klik op **Voltooien**.

>[!NOTE]
>De opdrachten in dit artikel moeten worden uitgevoerd op een PowerShell-sessie met verhoogde bevoegdheden.

### <a name="convert-disk-by-using-powershell"></a>Converteert de schijf met behulp van PowerShell
U kunt een virtuele schijf converteren met behulp van de [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) opdracht in Windows PowerShell. Selecteer **als administrator uitvoeren** wanneer u start PowerShell. 

De volgende opdracht vanaf VHDX naar VHD en van een dynamisch uitbreidbare schijf voor een vaste grootte worden geconverteerd:

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```
In deze opdracht Vervang de waarde voor '-pad ' met het pad naar de virtuele harde schijf die u wilt converteren en de waarde voor '-doelpad ' met het nieuwe pad en de naam van de geconverteerde schijf.

### <a name="convert-from-vmware-vmdk-disk-format"></a>Converteren van VMware VMDK schijf-indeling
Als er een virtuele machine van Windows-installatiekopie in de [VMDK-indeling](https://en.wikipedia.org/wiki/VMDK), converteren naar een VHD met behulp van de [Microsoft VM Converter](https://www.microsoft.com/download/details.aspx?id=42497). Zie voor meer informatie het artikel blog [een VMware VMDK converteren naar Hyper-V-VHD](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx).

## <a name="set-windows-configurations-for-azure"></a>Windows-configuraties voor Azure instellen

Voer alle opdrachten in de volgende stappen uit op de virtuele machine die u wilt uploaden naar Azure, een [opdrachtpromptvenster](https://technet.microsoft.com/library/cc947813.aspx):

1. Statische permanente route op de routeringstabel verwijderen:
   
   * Uitvoeren als u wilt weergeven in de routetabel, `route print` bij de opdrachtprompt.
   * Controleer de **persistentie Routes** secties. Als er een permanente route is, gebruikt u [route verwijderen](https://technet.microsoft.com/library/cc739598.apx) te verwijderen.
2. Verwijder de WinHTTP-proxy:
   
    ```PowerShell
    netsh winhttp reset proxy
    ```
3. De schijf SAN-beleid instellen op [Onlineall](https://technet.microsoft.com/library/gg252636.aspx). 
   
    ```PowerShell
    diskpart 
    ```
    Typ in het venster openen opdrachtprompt de volgende opdrachten:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Tijd van Coordinated Universal Time (UTC) instellen voor Windows en het opstarttype van de service Windows Time (w32time) naar **automatisch**:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -name "RealTimeIsUniversal" 1 -Type DWord

    Set-Service -Name w32time -StartupType Auto
    ```
5. Het profiel power ingesteld op de **hoge prestaties**:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```

## <a name="check-the-windows-services"></a>Controleer de Windows-services
Zorg ervoor dat elk van de volgende Windows-services is ingesteld op de **Windows standaardwaarden**. Dit zijn de minimale nummers van services die moeten worden ingesteld om ervoor te zorgen dat de virtuele machine verbinding heeft. Als u de opstartinstellingen herstellen, voer de volgende opdrachten:
   
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
>Er wordt een foutbericht weergegeven tijdens het uitvoeren van de **Set-ItemProperty-pad ' HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services - naam &lt;objectnaam&gt; &lt;waarde&gt;** in deze stappen. Het foutbericht kan worden genegeerd. Alleen betekent dit dat het domein dat de configuratie door middel van een groepsbeleidsobject niet pusht.
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
    Wanneer u een virtuele machine implementeert, wordt de standaardregels worden gemaakt op basis van poort 3389. Als u het poortnummer wijzigt wilt, dat doen nadat de virtuele machine is geïmplementeerd in Azure.

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

5. Stel de keepalive-waarde:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveEnable" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveInterval" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "KeepAliveTimeout" 1 -Type DWord
    ```
6. Verbinding:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDisableAutoReconnect" 0 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fInheritReconnectSame" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fReconnectSame" 0 -Type DWord
    ```
7. Het aantal gelijktijdige verbindingen beperken:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "MaxInstanceCount" 4294967295 -Type DWord
    ```
8. Als er zelfondertekende certificaten die zijn gekoppeld aan de RDP-listener zijn, verwijdert u deze:
    
    ```PowerShell
    Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SSLCertificateSHA1Hash"
    ```
    Dit is om ervoor te zorgen dat u verbinding aan het begin maken kunt bij het implementeren van de virtuele machine. U kunt ook controleren dit op een later stadium nadat de virtuele machine is geïmplementeerd in Azure, indien nodig.

9. Als de virtuele machine deel van een domein uitmaken zal, controleert u de volgende instellingen om ervoor te zorgen dat de voormalige instellingen worden niet hersteld. De beleidsregels die moeten worden gecontroleerd, zijn de volgende:
    
    - RDP is ingeschakeld:

         Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\ onderdelen\Extern bureaublad bureaublad-sessiehost\Verbindingen:
         
         **Toestaan dat gebruikers extern verbinding kunnen maken met behulp van extern bureaublad**

    - NLA-Groepsbeleid:

        Settings\Administrative Templates\Components\Remote Desktop-bureaublad sessie\Beveiliging: 
        
        **Gebruiker-verificatie voor externe verbindingen met behulp van verificatie op netwerkniveau vereisen**
    
    - Houd Alive-instellingen:

        Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Windows onderdelen\Extern bureaublad bureaublad-sessiehost\Verbindingen: 
        
        **Keep-alive verbinding interval configureren**

    - Instellingen voor verbinding:

        Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Windows onderdelen\Extern bureaublad bureaublad-sessiehost\Verbindingen: 
        
        **Automatisch opnieuw verbinden**

    - Beperk het aantal verbindingsinstellingen:

        Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Windows onderdelen\Extern bureaublad bureaublad-sessiehost\Verbindingen: 
        
        **Aantal verbindingen beperken**

## <a name="configure-windows-firewall-rules"></a>Configureer Windows Firewall-regels
1. Windows Firewall inschakelen op de drie profielen (domein, Standard en openbaar):

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
3. Inschakelen van de volgende firewallregels het RDP-verkeer toestaan 

   ```PowerShell
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes
   ```   
4. Bestands- en printerdeling regel inschakelen zodat de virtuele machine op een ping-opdracht in het virtuele netwerk reageren kan:

   ```PowerShell
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes
   ``` 
5. Als de virtuele machine deel van een domein uitmaken zal, controleert u de volgende instellingen om ervoor te zorgen dat de voormalige instellingen worden niet hersteld. De AD-beleidsregels die moeten worden gecontroleerd, zijn de volgende:

    - Windows Firewall-profielen inschakelen

        Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Firewall: **alle netwerkverbindingen beveiligen**

       Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Standard Profile\Windows Firewall: **alle netwerkverbindingen beveiligen**

    - Schakelt u RDP 

        Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Firewall: **inkomende extern bureaublad-uitzonderingen toestaan**

        Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Standard Profile\Windows Firewall: **inkomende extern bureaublad-uitzonderingen toestaan**

    - ICMP-V4 inschakelen

        Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Firewall: **uitzonderingen voor ICMP toestaan**

        Computer Computerconfiguratie\Beleid\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Standard Profile\Windows Firewall: **uitzonderingen voor ICMP toestaan**

## <a name="verify-vm-is-healthy-secure-and-accessible-with-rdp"></a>Controleer of dat de VM bevindt zich in orde, veilige en toegankelijk zijn met RDP 
1. Om te controleren of dat de schijf is in orde en consistente, een selectievakje schijfbewerking worden uitgevoerd op de computer opnieuw VM:

    ```PowerShell
    Chkdsk /f
    ```
    Zorg ervoor dat het rapport wordt een schijf schoon en gezond weergegeven.

2. Stel de instellingen van de gegevens BCD (Boot Configuration). 

    > [!Note]
    > Zorg ervoor dat u deze opdrachten uitvoeren op een verhoogde CMD-venster en **niet** op PowerShell:
   
   ```CMD
   bcdedit /set {bootmgr} integrityservices enable
   
   bcdedit /set {default} device partition=C:
   
   bcdedit /set {default} integrityservices enable
   
   bcdedit /set {default} recoveryenabled Off
   
   bcdedit /set {default} osdevice partition=C:
   
   bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
   ```
3. Controleer of de Windows Management Instrumentation-opslagplaats consistent is. Als u wilt uitvoeren, moet u de volgende opdracht uitvoeren:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Als de opslagplaats is beschadigd, Zie [WMI: beschadiging van de opslagplaats of niet](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

4. Zorg ervoor dat een andere toepassing geen gebruik van de poort 3389 maakt. Deze poort wordt gebruikt voor de RDP-service in Azure. U kunt uitvoeren **netstat - anob** om te zien welke poorten worden in gebruikt op de virtuele machine:

    ```PowerShell
    netstat -anob
    ```

5. Als de Windows-VHD die u wilt uploaden een domeincontroller is, volgt u deze stappen:

    A. Ga als volgt [deze extra stappen](https://support.microsoft.com/kb/2904015) voorbereiden van de schijf.

    B. Zorg ervoor dat u het DSRM-wachtwoord kent het geval u de virtuele machine te starten in DSRM op een bepaald moment. U kunt verwijzen naar deze koppeling om in te stellen de [DSRM-wachtwoord](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx).

6. Zorg ervoor dat de ingebouwde Administrator-account en het wachtwoord bekend zijn bij u. Mogelijk wilt het huidige lokale administrator-wachtwoord opnieuw instellen en zorg ervoor dat u kunt dit account aanmeldt bij Windows via de RDP-verbinding. Deze toegangsmachtiging wordt bepaald door het groepsbeleidsobject 'Aanmelden toestaan via Extern bureaublad-Services'. U kunt dit object in de Editor voor lokaal groepsbeleid onder bekijken:

    Computer Configuration\Windows Settings\Security Beveiligingsinstellingen\Lokaal beleid\gebruikersrechten toewijzen

7. Controleer dat de volgende AD beleid om ervoor te zorgen dat u uw RDP-toegang via RDP en ook niet vanaf het netwerk niet blokkeren:

    - Computer Configuration\Windows Settings\Security Beveiligingsinstellingen\Lokaal beleid\gebruikersrechten rechten Assignment\Deny toegang tot deze computer vanaf het netwerk

    - Computer Configuration\Windows Settings\Security Beveiligingsinstellingen\Lokaal beleid\gebruikersrechten rechten Assignment\Deny aanmelden via Extern bureaublad-Services


8. Start de virtuele machine om ervoor te zorgen dat Windows nog steeds in orde is, kan worden bereikt met behulp van de RDP-verbinding. Op dit moment wilt u een virtuele machine maken in uw lokale Hyper-V Zorg ervoor dat de virtuele machine volledig wordt gestart en vervolgens controleren of het RDP bereikbaar is.

9. Verwijder eventuele extra Transport Driver Interface filters, zoals software waarmee TCP worden geanalyseerd pakketten of extra firewalls. U kunt ook controleren dit op een later stadium nadat de virtuele machine is geïmplementeerd in Azure, indien nodig.

10. Verwijder alle andere software van derden en -stuurprogramma dat is gerelateerd aan fysieke onderdelen of andere virtualisatietechnologie bevindt.

### <a name="install-windows-updates"></a>Windows-Updates installeren
De ideale configuratie is het **patch niveau van de computer op de meest recente**. Als dit niet mogelijk is, zorg ervoor dat de volgende updates zijn geïnstalleerd:

|                       |                   |           |                                       Minimale versie x64       |                                      |                                      |                            |
|-------------------------|-------------------|------------------------------------|---------------------------------------------|--------------------------------------|--------------------------------------|----------------------------|
| Onderdeel               | Binaire            | Windows 7 en Windows Server 2008 R2 | Windows 8 en WindowsServer 2012             | Windows 8.1 en Windows Server 2012 R2 | Windows 10 en Windows Server 2016 RS1 | Windows 10 RS2             |
| Storage                 | Disk.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17638 / 6.2.9200.21757 - KB3137061 | 6.3.9600.18203 - KB3137061           | -                                    | -                          |
|                         | Storport.sys      | 6.1.7601.23403 - KB3125574         | 6.2.9200.17188 / 6.2.9200.21306 - KB3018489 | 6.3.9600.18573 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.332             |
|                         | NTFS.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17623 / 6.2.9200.21743 - KB3121255 | 6.3.9600.18654 - KB4022726           | 10.0.14393.1198 - KB4022715          | 10.0.15063.447             |
|                         | Iologmsg.dll      | 6.1.7601.23403 - KB3125574         | 6.2.9200.16384 - KB2995387                  | -                                    | -                                    | -                          |
|                         | Classpnp.sys      | 6.1.7601.23403 - KB3125574         | 6.2.9200.17061 / 6.2.9200.21180 - KB2995387 | 6.3.9600.18334 - KB3172614           | 10.0.14393.953 - KB4022715           | -                          |
|                         | Volsnap.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.17047 / 6.2.9200.21165 - KB2975331 | 6.3.9600.18265 - KB3145384           | -                                    | 10.0.15063.0               |
|                         | partmgr.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.16681 - KB2877114                  | 6.3.9600.17401 - KB3000850           | 10.0.14393.953 - KB4022715           | 10.0.15063.0               |
|                         | volmgr.sys        |                                    |                                             |                                      |                                      | 10.0.15063.0               |
|                         | Volmgrx.sys       | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | 10.0.15063.0               |
|                         | Msiscsi.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.21006 - KB2955163                  | 6.3.9600.18624 - KB4022726           | 10.0.14393.1066 - KB4022715          | 10.0.15063.447             |
|                         | Msdsm.sys         | 6.1.7601.23403 - KB3125574         | 6.2.9200.21474 - KB3046101                  | 6.3.9600.18592 - KB4022726           | -                                    | -                          |
|                         | MPIO.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.21190 - KB3046101                  | 6.3.9600.18616 - KB4022726           | 10.0.14393.1198 - KB4022715          | -                          |
|                         | Fveapi.dll        | 6.1.7601.23311 - KB3125574         | 6.2.9200.20930 - KB2930244                  | 6.3.9600.18294 - KB3172614           | 10.0.14393.576 - KB4022715           | -                          |
|                         | Fveapibase.dll    | 6.1.7601.23403 - KB3125574         | 6.2.9200.20930 - KB2930244                  | 6.3.9600.17415 - KB3172614           | 10.0.14393.206 - KB4022715           | -                          |
| Netwerk                 | netvsc.sys        | -                                  | -                                           | -                                    | 10.0.14393.1198 - KB4022715          | 10.0.15063.250 - KB4020001 |
|                         | mrxsmb10.sys      | 6.1.7601.23816 - KB4022722         | 6.2.9200.22108 - KB4022724                  | 6.3.9600.18603 - KB4022726           | 10.0.14393.479 - KB4022715           | 10.0.15063.483             |
|                         | mrxsmb20.sys      | 6.1.7601.23816 - KB4022722         | 6.2.9200.21548 - KB4022724                  | 6.3.9600.18586 - KB4022726           | 10.0.14393.953 - KB4022715           | 10.0.15063.483             |
|                         | Mrxsmb.sys        | 6.1.7601.23816 - KB4022722         | 6.2.9200.22074 - KB4022724                  | 6.3.9600.18586 - KB4022726           | 10.0.14393.953 - KB4022715           | 10.0.15063.0               |
|                         | Tcpip.sys         | 6.1.7601.23761 - KB4022722         | 6.2.9200.22070 - KB4022724                  | 6.3.9600.18478 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.447             |
|                         | HTTP.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17285 - KB3042553                  | 6.3.9600.18574 - KB4022726           | 10.0.14393.251 - KB4022715           | 10.0.15063.483             |
|                         | vmswitch.sys      | 6.1.7601.23727 - KB4022719         | 6.2.9200.22117 - KB4022724                  | 6.3.9600.18654 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.138             |
| Kern                    | Ntoskrnl.exe      | 6.1.7601.23807 - KB4022719         | 6.2.9200.22170 - KB4022718                  | 6.3.9600.18696 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.483             |
| Externe bureaubladservices | rdpcorets.dll     | 6.2.9200.21506 - KB4022719         | 6.2.9200.22104 - KB4022724                  | 6.3.9600.18619 - KB4022726           | 10.0.14393.1198 - KB4022715          | 10.0.15063.0               |
|                         | TERMSRV.dll       | 6.1.7601.23403 - KB3125574         | 6.2.9200.17048 - KB2973501                  | 6.3.9600.17415 - KB3000850           | 10.0.14393.0 - KB4022715             | 10.0.15063.0               |
|                         | termdd.sys        | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
|                         | Win32k.sys        | 6.1.7601.23807 - KB4022719         | 6.2.9200.22168 - KB4022718                  | 6.3.9600.18698 - KB4022726           | 10.0.14393.594 - KB4022715           | -                          |
|                         | rdpdd.dll         | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
|                         | Rdpwd.sys         | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
| Beveiliging                | Als gevolg van WannaCrypt | KB4012212                          | KB4012213                                   | KB4012213                            | KB4012606                            | KB4012606                  |
|                         |                   |                                    | KB4012216                                   |                                      | KB4013198                            | KB4013198                  |
|                         |                   | KB4012215                          | KB4012214                                   | KB4012216                            | KB4013429                            | KB4013429                  |
|                         |                   |                                    | KB4012217                                   |                                      | KB4013429                            | KB4013429                  |
       
### Het gebruik van sysprep<a id="step23"></a>    

Sysprep is een proces dat kan worden uitgevoerd in een installatie van windows die wordt opnieuw ingesteld voor de installatie van het systeem en wordt er een 'of de box experience' door het verwijderen van alle persoonlijke gegevens en het opnieuw instellen van verschillende onderdelen. U doorgaans doen als u wilt maken van een sjabloon van waaruit u kunt verschillende andere VM's die een specifieke configuratie hebben. Dit wordt genoemd, een **gegeneraliseerd installatiekopie**.

Als u in plaats daarvan wilt slechts aan één virtuele machine maken van een schijf, er geen gebruik van sysprep. In dit geval maakt u alleen de virtuele machine uit wat wordt ook wel een **gespecialiseerde installatiekopie**.

Zie voor meer informatie over het maken van een virtuele machine vanaf een speciale schijf:

- [Een virtuele machine vanaf een speciale schijf maken](create-vm-specialized.md)
- [Een virtuele machine vanaf een speciale VHD-schijf maken](https://azure.microsoft.com/resources/templates/201-vm-specialized-vhd/)

Als u maken van een algemene installatiekopie wilt, moet u sysprep niet uitvoeren. Zie voor meer informatie over Sysprep [hoe gebruik Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx). 

Ondersteunt deze generaliseren niet elke rol of de toepassing die op een Windows-computer geïnstalleerd. Dus voordat u deze procedure uitvoert, verwijzen naar het volgende artikel om ervoor te zorgen dat de rol van die computer wordt ondersteund door sysprep. Voor meer informatie [Sysprep-ondersteuning voor serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="steps-to-generalize-a-vhd"></a>Stappen voor het generaliseren van een VHD

>[!NOTE]
> Nadat u sysprep.exe zoals opgegeven in de volgende stappen uitvoeren, uitschakelen van de virtuele machine, en kan niet deze weer inschakelen totdat u een installatiekopie van het in Azure maakt.

1. Aanmelden bij de Windows-VM.
2. Voer **opdrachtprompt** als beheerder. 
3. Wijzig de map in: **%windir%\system32\sysprep**, en voer vervolgens **sysprep.exe**.
3. In de **hulpprogramma voor systeemvoorbereiding** dialoogvenster, **System Voer Out-of-Box Experience (OOBE)**, en zorg ervoor dat de **Generalize** selectievakje is ingeschakeld.

    ![Hulpprogramma voor systeemvoorbereiding](media/prepare-for-upload-vhd-image/syspre.png)
4. In **afsluitopties**, selecteer **afsluiten**.
5. Klik op **OK**.
6. Wanneer Sysprep is voltooid, sluit u de virtuele machine. Gebruik geen **opnieuw** afsluiten van de virtuele machine.
7. De VHD is nu gereed om te worden geüpload. Zie voor meer informatie over het maken van een virtuele machine van een gegeneraliseerde schijf [een gegeneraliseerde VHD uploaden en deze gebruiken voor het maken van een nieuwe virtuele machines in Azure](sa-upload-generalized.md).


## <a name="complete-recommended-configurations"></a>Voert u de aanbevolen configuraties
De volgende instellingen hebben geen invloed op de VHD uploaden. Echter, wordt aangeraden dat u ze had geconfigureerd.

* Installeer de [virtuele machines in Azure-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Vervolgens kunt u VM-extensies inschakelen. De VM-extensies implementeren de meeste essentiële functionaliteit die u mogelijk wilt gebruiken met uw VMs zoals het opnieuw instellen van wachtwoorden, RDP configureren, enzovoort. Zie voor meer informatie:

    - [VM-Agent en -extensies: deel 1](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-1/)
    - [VM-Agent en -extensies – deel 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)
* Het logboek Dump kan helpen bij het oplossen van problemen met het vastlopen van Windows zijn. De verzameling van Dump logboek inschakelen:
  
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "CrashDumpEnable" -Value "2" -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "DumpFile" -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "AutoReboot" -Value 0 -Type DWord
    New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpFolder" -Value "c:\CrashDumps"
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpCount" -Value 10 -Type DWord
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpType" -Value 2 -Type DWord
    Set-Service -Name WerSvc -StartupType Manual
    ```
    Als u eventuele fouten tijdens de procedurele stappen in dit artikel ontvangt, betekent dit dat de registersleutels al bestaat. In dit geval gebruikt u de volgende opdrachten:

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
Als er een gegevensschijf die is gekoppeld aan de virtuele machine, is de stationsletter van het tijdelijke stationsvolume doorgaans "D" Deze aanwijzing mogelijk verschillen, afhankelijk van het aantal beschikbare stations en de instellingen die u aanbrengt.

## <a name="next-steps"></a>Volgende stappen
* [Een virtuele machine van Windows-installatiekopie uploaden naar Azure voor implementaties van Resource Manager](upload-generalized-managed.md)
* [Windows Azure virtuele machine activation problemen oplossen](troubleshoot-activation-problems.md)

