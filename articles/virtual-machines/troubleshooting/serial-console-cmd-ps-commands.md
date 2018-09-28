---
title: CMD en PowerShell in Windows Azure VM's | Microsoft Docs
description: Het gebruik van CMD en PowerShell-opdrachten binnen SAC in Azure Windows VM 's
services: virtual-machines-windows
documentationcenter: ''
author: alsin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/14/2018
ms.author: alsin
ms.openlocfilehash: 6eb0ce9098b5cc5f66035851c6837e9e46fcf2df
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47413731"
---
# <a name="windows-commands---cmd-and-powershell"></a>Windows-opdrachten - CMD en PowerShell

Deze sectie bevat een van de Voorbeeldopdrachten voor het uitvoeren van algemene taken in scenario's waarbij u wellicht SAC voor toegang tot uw Windows-VM, zoals wanneer u problemen met RDP-verbinding moet gebruiken.

SAC sinds Windows Server 2003 is opgenomen in alle versies van Windows, maar is standaard uitgeschakeld. SAC is afhankelijk van de `sacdrv.sys` kernelstuurprogramma, de `Special Administration Console Helper` service (`sacsvr`), en de `sacsess.exe` proces. Zie voor meer informatie, [Emergency Management Services Tools and Settings](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc787940(v%3dws.10)).

SAC kunt u verbinding maken met uw actieve besturingssysteem via de seriële poort. Wanneer u CMD van SAC, start `sacsess.exe` Start `cmd.exe` binnen uw actieve besturingssysteem. U kunt zien dat in de taak Manager als u RDP met uw virtuele machine op hetzelfde wanneer u verbonden bent met SAC via de seriële console-functie. De CMD u toegang via SAC hebt is hetzelfde `cmd.exe` u gebruiken wanneer verbonden via RDP. Dezelfde zijn opdrachten en hulpprogramma's beschikbaar, waaronder de mogelijkheid om PowerShell uit die CMD-sessie starten. Dat is een belangrijk verschil tussen SAC en de Windows Recovery Environment (WinRE) in die SAC is zodat u uw actieve besturingssysteem, waar WinRE wordt opnieuw naar een andere, minimale OS opgestart beheren. Terwijl de mogelijkheid tot WinRE, met de seriële console-functie, bieden geen ondersteuning voor Azure-VM's kunnen Azure-VM's worden beheerd via SAC.

Omdat de SAC is beperkt tot een 80 24 x schermbuffer met weer geen schuifbalken, toevoegen `| more` opdrachten om de uitvoer een pagina op een tijdstip weer te geven. Gebruik `<spacebar>` om te zien van de volgende pagina of `<enter>` om te zien van de volgende regel.  

`SHIFT+INSERT` is de snelkoppeling plakken voor de seriële console-venster.

Vanwege de SAC beperkt schermbuffer, meer opdrachten mogelijk beter te typen in een lokale teksteditor en vervolgens in SAC geplakt.

## <a name="view-and-edit-windows-registry-settings"></a>Windows-registerinstellingen weergeven en bewerken
### <a name="verify-rdp-is-enabled"></a>Controleer of dat RDP is ingeschakeld
`reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections`

`reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections`

De tweede sleutel (onder \Policies) is alleen aanwezig als de desbetreffende instelling voor Groepsbeleid is geconfigureerd.

### <a name="enable-rdp"></a>RDP inschakelen
`reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0`

`reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0` 

De tweede sleutel (onder \Policies) alleen nodig als de desbetreffende instelling voor Groepsbeleid heeft is geconfigureerd. Waarde wordt op de volgende vernieuwen van Groepsbeleid worden herschreven als deze is geconfigureerd in Groepsbeleid.

## <a name="manage-windows-services"></a>Windows-Services beheren

### <a name="view-service-state"></a>Servicestatus weergeven
`sc query termservice`
###  <a name="view-service-logon-account"></a>Serviceaccount voor aanmelding weergeven
`sc qc termservice`
### <a name="set-service-logon-account"></a>Serviceaccount voor aanmelding instellen 
`sc config termservice obj= "NT Authority\NetworkService"`

Na het gelijkteken is een ruimte nodig.
### <a name="set-service-start-type"></a>Set-service starttype
`sc config termservice start= demand` 

Na het gelijkteken is een ruimte nodig. Start mogelijke waarden zijn onder andere `boot`, `system`, `auto`, `demand`, `disabled`, `delayed-auto`.
### <a name="set-service-dependencies"></a>Set-serviceafhankelijkheden
`sc config termservice depend= RPCSS`

Na het gelijkteken is een ruimte nodig.
### <a name="start-service"></a>Service starten
`net start termservice`

of

`sc start termservice`
### <a name="stop-service"></a>Service stoppen
`net stop termservice`

of

`sc stop termservice`
## <a name="manage-networking-features"></a>Netwerkfuncties beheren
### <a name="show-nic-properties"></a>NIC-eigenschappen weergeven
`netsh interface show interface` 
### <a name="show-ip-properties"></a>IP-eigenschappen weergeven
`netsh interface ip show config`
### <a name="show-ipsec-configuration"></a>IPSec-configuratie weergeven
`netsh nap client show configuration`  
### <a name="enable-nic"></a>NIC inschakelen
`netsh interface set interface name="<interface name>" admin=enabled`
### <a name="set-nic-to-use-dhcp"></a>Instellen van de NIC in op het gebruik van DHCP
`netsh interface ip set address name="<interface name>" source=dhcp`

Voor meer informatie over `netsh`, [Klik hier](https://docs.microsoft.com/windows-server/networking/technologies/netsh/netsh-contexts).

Azure-VM's moeten altijd worden geconfigureerd in het gastbesturingssysteem te installeren met DHCP een IP-adres. De instelling voor de Azure statische IP gebruikt nog steeds DHCP te geven van het statische IP-adres aan de virtuele machine.
### <a name="ping"></a>Ping
`ping 8.8.8.8` 
### <a name="port-ping"></a>Poortping  
De Telnet-client installeren

`dism /online /Enable-Feature /FeatureName:TelnetClient`

Connectiviteit testen

`telnet bing.com 80`

De telnet-client verwijderen

`dism /online /Disable-Feature /FeatureName:TelnetClient`

Als beperkt tot de methoden die beschikbaar zijn in Windows standaard worden PowerShell een betere benadering voor het testen van de poortconnectiviteit. Zie de sectie PowerShell hieronder voor meer voorbeelden.
### <a name="test-dns-name-resolution"></a>Test de DNS-naamomzetting
`nslookup bing.com`
### <a name="show-windows-firewall-rule"></a>Windows Firewall-regel weergeven
`netsh advfirewall firewall show rule name="Remote Desktop - User Mode (TCP-In)"`
### <a name="disable-windows-firewall"></a>Windows Firewall uitschakelen
`netsh advfirewall set allprofiles state off`

U kunt deze opdracht gebruiken bij het oplossen van om tijdelijk uit de Windows-Firewall. Het zal worden inschakelen op het systeem opnieuw is opgestart of wanneer u enaable met behulp van de onderstaande opdracht. Het Windows Firewall-service (MPSVC) of de Base Filtering Engine (BFE)-service niet worden gestopt als manier om uit de Windows Firewall. MPSSVC of BFE stoppen resulteert in alle verbindingen worden geblokkeerd.
### <a name="enable-windows-firewall"></a>Windows Firewall inschakelen
`netsh advfirewall set allprofiles state on`
## <a name="manage-users-and-groups"></a>Gebruikers en groepen beheren
### <a name="create-local-user-account"></a>Lokale gebruikersaccounts maken
`net user /add <username> <password>`
### <a name="add-local-user-to-local-group"></a>Lokale gebruiker toevoegen aan de lokale groep
`net localgroup Administrators <username> /add`
### <a name="verify-user-account-is-enabled"></a>Controleer of het gebruikersaccount dat is ingeschakeld
`net user <username> | find /i "active"`

Azure VM's die zijn gemaakt op basis van gegeneraliseerde installatiekopie heeft het lokale administrator-account gewijzigd in de naam die is opgegeven tijdens het inrichten van VM's. Het wordt dus meestal niet `Administrator`.
### <a name="enable-user-account"></a>Gebruikersaccount inschakelen
`net user <username> /active:yes`  
### <a name="view-user-account-properties"></a>Eigenschappen van gebruikersaccount weergeven
`net user <username>`

Van de voorbeeldregels van de interesses, van een lokaal Administrator-account:

`Account active Yes`

`Account expires Never`

`Password expires Never`

`Workstations allowed All`

`Logon hours allowed All`

`Local Group Memberships *Administrators`

### <a name="view-local-groups"></a>Lokale groepen weergeven
`net localgroup`
## <a name="manage-the-windows-event-log"></a>Beheren van het Windows-gebeurtenislogboek
### <a name="query-event-log-errors"></a>Query uitvoeren op gebeurtenislogboek
`wevtutil qe system /c:10 /f:text /q:"Event[System[Level=2]]" | more`

Wijziging `/c:10` naar het gewenste aantal gebeurtenissen die moeten worden geretourneerd, of verplaats het retourneren van alle gebeurtenissen die overeenkomen met het filter.
### <a name="query-event-log-by-event-id"></a>Query-gebeurtenislogboek op gebeurtenis-ID
`wevtutil qe system /c:1 /f:text /q:"Event[System[EventID=11]]" | more`
### <a name="query-event-log-by-event-id-and-provider"></a>Gebeurtenislogboek van de query door de gebeurtenis-ID en -Provider
`wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11]]" | more`
### <a name="query-event-log-by-event-id-and-provider-for-the-last-24-hours"></a>Gebeurtenislogboek van de gebeurtenis-ID en -Provider op te vragen voor de afgelopen 24 uur
`wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11 and TimeCreated[timediff(@SystemTime) <= 86400000]]]"`

Gebruik `604800000` om te controleren of terug 7 dagen in plaats van 24 uur.
### <a name="query-event-log-by-event-id-provider-and-eventdata-in-the-last-7-days"></a>Gebeurtenislogboek van de gebeurtenis-ID-Provider en EventData-query in de afgelopen 7 dagen
`wevtutil qe security /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Security-Auditing'] and EventID=4624 and TimeCreated[timediff(@SystemTime) <= 604800000]] and EventData[Data[@Name='TargetUserName']='<username>']]" | more`
## <a name="view-or-remove-installed-applications"></a>Weergeven of verwijderen van de geïnstalleerde toepassingen
### <a name="list-installed-applications"></a>Lijst met geïnstalleerde toepassingen
`wmic product get Name,InstallDate | sort /r | more`

De `sort /r` sorteren op aflopende installatiedatum gemakkelijker om te zien wat er onlangs is geïnstalleerd. Gebruik `<spacebar>` om door te gaan naar de volgende pagina van de uitvoer, of `<enter>` om door te gaan een regel.
### <a name="uninstall-an-application"></a>Een toepassing verwijderen
`wmic path win32_product where name="<name>" call uninstall`

Vervang `<name>` met de naam die is geretourneerd in de bovenstaande opdracht voor de toepassing die u wilt verwijderen.

## <a name="file-system-management"></a>Bestand System Management
### <a name="get-file-version"></a>Bestandsversie ophalen
`wmic datafile where "drive='C:' and path='\\windows\\system32\\drivers\\' and filename like 'netvsc%'" get version /format:list`

In dit voorbeeld retourneert de versie van het virtuele NIC-stuurprogramma netvsc.sys, netvsc63.sys of netvsc60.sys afhankelijk van de Windows-versie.
### <a name="scan-for-system-file-corruption"></a>Beschadiging van systeembestanden scannen
`sfc /scannow`

Zie ook [herstellen van een Windows-installatiekopie](https://docs.microsoft.com/windows-hardware/manufacture/desktop/repair-a-windows-image).
### <a name="scan-for-system-file-corruption"></a>Beschadiging van systeembestanden scannen
`dism /online /cleanup-image /scanhealth`

Zie ook [herstellen van een Windows-installatiekopie](https://docs.microsoft.com/windows-hardware/manufacture/desktop/repair-a-windows-image).
### <a name="export-file-permissions-to-text-file"></a>Bestandsmachtigingen exporteren naar bestand
`icacls %programdata%\Microsoft\Crypto\RSA\MachineKeys /t /c > %temp%\MachineKeys_permissions_before.txt`
### <a name="save-file-permissions-to-acl-file"></a>Machtigingen voor bestanden in de ACL-bestand opslaan
`icacls %programdata%\Microsoft\Crypto\RSA\MachineKeys /save %temp%\MachineKeys_permissions_before.aclfile /t`  
### <a name="restore-file-permissions-from-acl-file"></a>Machtigingen voor bestanden herstellen van ACL-bestand
`icacls %programdata%\Microsoft\Crypto\RSA /save %temp%\MachineKeys_permissions_before.aclfile /t`

Het pad wanneer u `/restore` moet de bovenliggende map van de map die u hebt opgegeven bij het gebruik van `/save`. In dit voorbeeld `\RSA` is het bovenliggende lid van de `\MachineKeys` map die is opgegeven de `/save` bovenstaand voorbeeld.
### <a name="take-ntfs-ownership-of-a-folder"></a>NTFS-eigenaar van een map
`takeown /f %programdata%\Microsoft\Crypto\RSA\MachineKeys /a /r`  
### <a name="grant-ntfs-permissions-to-a-folder-recursively"></a>NTFS-machtigingen verlenen aan een map recursief
`icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"`  
## <a name="manage-devices"></a>Apparaten beheren
### <a name="remove-non-present-pnp-devices"></a>Niet aanwezig Plug en Play-apparaten verwijderen
`%windir%\System32\RUNDLL32.exe %windir%\System32\pnpclean.dll,RunDLL_PnpClean /Devices /Maxclean`
## <a name="manage-group-policy"></a>Beheren van Groepsbeleid
### <a name="force-group-policy-update"></a>Bijwerken van groepsbeleid afdwingen
`gpupdate /force /wait:-1`
## <a name="miscellaneous-tasks"></a>Diverse taken
### <a name="show-os-version"></a>Versie van het besturingssysteem weergeven
`ver`

of 

`wmic os get caption,version,buildnumber /format:list`

of 

`systeminfo  find /i "os name"`

`systeminfo | findstr /i /r "os.*version.*build"`
### <a name="view-os-install-date"></a>Installatiedatum OS weergeven
`systeminfo | find /i "original"`

of 

`wmic os get installdate`
### <a name="view-last-boot-time"></a>Laatste opstarttijd weergeven
`systeminfo | find /i "system boot time"`
### <a name="view-time-zone"></a>Tijdzone weergeven
`systeminfo | find /i "time zone"`

of

`wmic timezone get caption,standardname /format:list`
### <a name="restart-windows"></a>Opnieuw opstarten van Windows
`shutdown /r /t 0`

Toe te voegen `/f` forceert een actieve toepassingen te sluiten zonder waarschuwing gebruikers.
### <a name="detect-safe-mode-boot"></a>Opstarten in veilige modus detecteren
`bcdedit /enum | find /i "safeboot"` 

# <a name="windows-commands---powershell"></a>Windows-opdrachten - PowerShell

Om uit te voeren PowerShell in SAC, als u bij een opdrachtprompt, typt u:

`powershell <enter>`

>[!CAUTION]
Verwijder de module PSReadLine vanuit de PowerShell-sessie voordat u andere PowerShell-opdrachten uitvoert. Er is een bekend probleem waarbij extra tekens in de tekst van het Klembord zijn geplakt als PSReadLine wordt uitgevoerd in een PowerShell-sessie in SAC kunnen worden ingevoerd.

Controleer eerst of PSReadLine is geladen. Het wordt standaard op Windows Server 2016, Windows 10 en latere versies van Windows geladen. Deze zouden alleen aanwezig zijn in eerdere versies van Windows als u deze handmatig is geïnstalleerd. 

Als deze opdracht wordt geretourneerd naar de prompt met geen uitvoer, klikt u vervolgens de module niet is geladen en u kunt doorgaan met het gebruik van de PowerShell-sessie in SAC als normaal.

`get-module psreadline`

Als de bovenstaande opdracht de versie van de module PSReadLine retourneert, voer de volgende opdracht om te verwijderen. Met deze opdracht niet verwijderen of de module verwijderen, deze alleen verwijderd uit de huidige PowerShell-sessie.

`remove-module psreadline`

## <a name="view-and-edit-windows-registry-settings"></a>Windows-registerinstellingen weergeven en bewerken
### <a name="verify-rdp-is-enabled"></a>Controleer of dat RDP is ingeschakeld
`get-itemproperty -path 'hklm:\system\curRentcontrolset\control\terminal server' -name 'fdenytsconNections'`

`get-itemproperty -path 'hklm:\software\policies\microsoft\windows nt\terminal services' -name 'fdenytsconNections'`

De tweede sleutel (onder \Policies) is alleen aanwezig als de desbetreffende instelling voor Groepsbeleid is geconfigureerd.
### <a name="enable-rdp"></a>RDP inschakelen
`set-itemproperty -path 'hklm:\system\curRentcontrolset\control\terminal server' -name 'fdenytsconNections' 0 -type dword`

`set-itemproperty -path 'hklm:\software\policies\microsoft\windows nt\terminal services' -name 'fdenytsconNections' 0 -type dword`

De tweede sleutel (onder \Policies) alleen nodig als de desbetreffende instelling voor Groepsbeleid heeft is geconfigureerd. Waarde wordt op de volgende vernieuwen van Groepsbeleid worden herschreven als deze is geconfigureerd in Groepsbeleid.
## <a name="manage-windows-services"></a>Windows-Services beheren
### <a name="view-service-details"></a>Details van de service weergeven
`get-wmiobject win32_service -filter "name='termservice'" |  format-list Name,DisplayName,State,StartMode,StartName,PathName,ServiceType,Status,ExitCode,ServiceSpecificExitCode,ProcessId`

`Get-Service` kan worden gebruikt, maar niet het serviceaccount voor aanmelding. `Get-WmiObject win32-service` biedt.
### <a name="set-service-logon-account"></a>Serviceaccount voor aanmelding instellen
`(get-wmiobject win32_service -filter "name='termservice'").Change($null,$null,$null,$null,$null,$false,'NT Authority\NetworkService')`

Bij het gebruik van een service-account dan `NT AUTHORITY\LocalService`, `NT AUTHORITY\NetworkService`, of `LocalSystem`, geef het wachtwoord voor het als het laatste argument (achtste) na de accountnaam.
### <a name="set-service-startup-type"></a>Opstarttype service instellen
`set-service termservice -startuptype Manual`

`Set-service` accepteert `Automatic`, `Manual`, of `Disabled` voor opstarttype.
### <a name="set-service-dependencies"></a>Set-serviceafhankelijkheden
`Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\TermService' -Name DependOnService -Value @('RPCSS','TermDD')`
### <a name="start-service"></a>Service starten
`start-service termservice`
### <a name="stop-service"></a>Service stoppen
`stop-service termservice`
## <a name="manage-networking-features"></a>Netwerkfuncties beheren
### <a name="show-nic-properties"></a>NIC-eigenschappen weergeven
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} |  format-list status,name,ifdesc,macadDresS,driverversion,MediaConNectState,MediaDuplexState`

of 

`get-wmiobject win32_networkadapter -filter "servicename='netvsc'" |  format-list netenabled,name,macaddress`

`Get-NetAdapter` is beschikbaar in 2012 +, voor gebruik van 2008R2 `Get-WmiObject`.
### <a name="show-ip-properties"></a>IP-eigenschappen weergeven
`get-wmiobject Win32_NetworkAdapterConfiguration -filter "ServiceName='netvsc'" |  format-list DNSHostName,IPAddress,DHCPEnabled,IPSubnet,DefaultIPGateway,MACAddress,DHCPServer,DNSServerSearchOrder`
### <a name="enable-nic"></a>NIC inschakelen
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} | enable-netadapter`

of

`(get-wmiobject win32_networkadapter -filter "servicename='netvsc'").enable()`

`Get-NetAdapter` is beschikbaar in 2012 +, voor gebruik van 2008R2 `Get-WmiObject`.
### <a name="set-nic-to-use-dhcp"></a>Instellen van de NIC in op het gebruik van DHCP
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} | Set-NetIPInterface -DHCP Enabled`

`(get-wmiobject Win32_NetworkAdapterConfiguration -filter "ServiceName='netvsc'").EnableDHCP()`

`Get-NetAdapter` is beschikbaar op 2012 +. Gebruik voor 2008R2 `Get-WmiObject`. Azure-VM's moeten altijd worden geconfigureerd in het gastbesturingssysteem te installeren met DHCP een IP-adres. De instelling voor de Azure statische IP gebruikt nog steeds DHCP te geven van het IP-adres aan de virtuele machine.
### <a name="ping"></a>Ping
`test-netconnection`

of

`get-wmiobject Win32_PingStatus -Filter 'Address="8.8.8.8"' | format-table -autosize IPV4Address,ReplySize,ResponseTime`

`Test-Netconnection` zonder parameters wordt geprobeerd om te pingen `internetbeacon.msedge.net`. Het is beschikbaar op 2012 +. Gebruik voor 2008R2 `Get-WmiObject` zoals in het tweede voorbeeld.
### <a name="port-ping"></a>Ping-poort
`test-netconnection -ComputerName bing.com -Port 80`

of

`(new-object Net.Sockets.TcpClient).BeginConnect('bing.com','80',$null,$null).AsyncWaitHandle.WaitOne(300)`

`Test-NetConnection` is beschikbaar op 2012 +. Voor 2008R2 gebruik `Net.Sockets.TcpClient`
### <a name="test-dns-name-resolution"></a>Test de DNS-naamomzetting
`resolve-dnsname bing.com` 

of 

`[System.Net.Dns]::GetHostAddresses('bing.com')`

`Resolve-DnsName` is beschikbaar op 2012 +. Gebruik voor 2008R2 `System.Net.DNS`.
### <a name="show-windows-firewall-rule-by-name"></a>Windows firewall-regel met de naam weergeven
`get-netfirewallrule -name RemoteDesktop-UserMode-In-TCP` 
### <a name="show-windows-firewall-rule-by-port"></a>Windows firewall-regel door poort weergeven
`get-netfirewallportfilter | where {$_.localport -eq 3389} | foreach {Get-NetFirewallRule -Name $_.InstanceId} | format-list Name,Enabled,Profile,Direction,Action`

of

`(new-object -ComObject hnetcfg.fwpolicy2).rules | where {$_.localports -eq 3389 -and $_.direction -eq 1} | format-table Name,Enabled`

`Get-NetFirewallPortFilter` is beschikbaar op 2012 +. Gebruik voor 2008R2 de `hnetcfg.fwpolicy2` COM-object. 
### <a name="disable-windows-firewall"></a>Windows firewall uitschakelen
`Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False`

`Set-NetFirewallProfile` is beschikbaar op 2012 +. Gebruik voor 2008R2 `netsh advfirewall` als waarnaar wordt verwezen in de bovenstaande CMD-sectie.
## <a name="manage-users-and-groups"></a>Gebruikers en groepen beheren
### <a name="create-local-user-account"></a>Lokale gebruikersaccounts maken
`new-localuser <name>`
### <a name="verify-user-account-is-enabled"></a>Controleer of het gebruikersaccount dat is ingeschakeld
`(get-localuser | where {$_.SID -like "S-1-5-21-*-500"}).Enabled`

of 

`(get-wmiobject Win32_UserAccount -Namespace "root\cimv2" -Filter "SID like 'S-1-5-%-500'").Disabled`

`Get-LocalUser` is beschikbaar op 2012 +. Gebruik voor 2008R2 `Get-WmiObject`. In dit voorbeeld ziet u de ingebouwde lokale administrator-account, die altijd SID `S-1-5-21-*-500`. Azure VM's die zijn gemaakt op basis van gegeneraliseerde installatiekopie heeft het lokale administrator-account gewijzigd in de naam die is opgegeven tijdens het inrichten van VM's. Het wordt dus meestal niet `Administrator`.
### <a name="add-local-user-to-local-group"></a>Lokale gebruiker toevoegen aan de lokale groep
`add-localgroupmember -group Administrators -member <username>`
### <a name="enable-local-user-account"></a>Lokaal gebruikersaccount inschakelen
`get-localuser | where {$_.SID -like "S-1-5-21-*-500"} | enable-localuser` 

In dit voorbeeld wordt de ingebouwde lokale administrator-account, die altijd SID `S-1-5-21-*-500`. Azure VM's die zijn gemaakt op basis van gegeneraliseerde installatiekopie heeft het lokale administrator-account gewijzigd in de naam die is opgegeven tijdens het inrichten van VM's. Het wordt dus meestal niet `Administrator`.
### <a name="view-user-account-properties"></a>Eigenschappen van gebruikersaccount weergeven
`get-localuser | where {$_.SID -like "S-1-5-21-*-500"} | format-list *`

of 

`get-wmiobject Win32_UserAccount -Namespace "root\cimv2" -Filter "SID like 'S-1-5-%-500'" |  format-list Name,Disabled,Status,Lockout,Description,SID`

`Get-LocalUser` is beschikbaar op 2012 +. Gebruik voor 2008R2 `Get-WmiObject`. In dit voorbeeld ziet u de ingebouwde lokale administrator-account, die altijd SID `S-1-5-21-*-500`.
### <a name="view-local-groups"></a>Lokale groepen weergeven
`(get-localgroup).name | sort` `(get-wmiobject win32_group).Name | sort`

`Get-LocalUser` is beschikbaar op 2012 +. Gebruik voor 2008R2 `Get-WmiObject`.
## <a name="manage-the-windows-event-log"></a>Beheren van het Windows-gebeurtenislogboek
### <a name="query-event-log-errors"></a>Query uitvoeren op gebeurtenislogboek
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Level=2]]" | more`

Wijziging `/c:10` naar het gewenste aantal gebeurtenissen die moeten worden geretourneerd, of verplaats het retourneren van alle gebeurtenissen die overeenkomen met het filter.
### <a name="query-event-log-by-event-id"></a>Query-gebeurtenislogboek op gebeurtenis-ID
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[EventID=11]]" | more`
### <a name="query-event-log-by-event-id-and-provider"></a>Gebeurtenislogboek van de query door de gebeurtenis-ID en -Provider
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11]]" | more`
### <a name="query-event-log-by-event-id-and-provider-for-the-last-24-hours"></a>Gebeurtenislogboek van de gebeurtenis-ID en -Provider op te vragen voor de afgelopen 24 uur
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11 and TimeCreated[timediff(@SystemTime) <= 86400000]]]"`

Gebruik `604800000` om te controleren of terug 7 dagen in plaats van 24 uur. |
### <a name="query-event-log-by-event-id-provider-and-eventdata-in-the-last-7-days"></a>Gebeurtenislogboek van de gebeurtenis-ID-Provider en EventData-query in de afgelopen 7 dagen
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Security-Auditing'] and EventID=4624 and TimeCreated[timediff(@SystemTime) <= 604800000]] and EventData[Data[@Name='TargetUserName']='<username>']]" | more`
## <a name="view-or-remove-installed-applications"></a>Weergeven of verwijderen van de geïnstalleerde toepassingen
### <a name="list-installed-software"></a>Lijst met geïnstalleerde software
`get-wmiobject win32_product | select installdate,name | sort installdate -descending | more`
### <a name="uninstall-software"></a>Software verwijderen
`(get-wmiobject win32_product -filter "Name='<name>'").Uninstall()`
## <a name="file-system-management"></a>Bestand System Management
### <a name="get-file-version"></a>Bestandsversie ophalen
`(get-childitem $env:windir\system32\drivers\netvsc*.sys).VersionInfo.FileVersion`

In dit voorbeeld retourneert de versie van de virtuele NIC-stuurprogramma, met de naam netvsc.sys, netvsc63.sys of netvsc60.sys afhankelijk van de Windows-versie.
### <a name="download-and-extract-file"></a>Downloaden en uitpakken van bestand
`$path='c:\bin';md $path;cd $path;(new-object net.webclient).downloadfile( ('htTp:/'+'/download.sysinternals.com/files/SysinternalsSuite.zip'),"$path\SysinternalsSuite.zip");(new-object -com shelL.apPlication).namespace($path).CopyHere( (new-object -com shelL.apPlication).namespace("$path\SysinternalsSuite.zip").Items(),16)`

Dit voorbeeld maakt u een `c:\bin` map, klikt u vervolgens downloadt en extraheert de Sysinternals-hulpprogramma's in `c:\bin`.
## <a name="miscellaneous-tasks"></a>Diverse taken
### <a name="show-os-version"></a>Versie van het besturingssysteem weergeven
`get-wmiobject win32_operatingsystem | format-list caption,version,buildnumber` 
### <a name="view-os-install-date"></a>Installatiedatum OS weergeven
`(get-wmiobject win32_operatingsystem).converttodatetime((get-wmiobject win32_operatingsystem).installdate)`
### <a name="view-last-boot-time"></a>Laatste opstarttijd weergeven
`(get-wmiobject win32_operatingsystem).lastbootuptime`
### <a name="view-windows-uptime"></a>Actieve tijdsduur van Windows weergeven
`"{0:dd}:{0:hh}:{0:mm}:{0:ss}.{0:ff}" -f ((get-date)-(get-wmiobject win32_operatingsystem).converttodatetime((get-wmiobject win32_operatingsystem).lastbootuptime))`

Retourneert de actieve tijdsduur als `<days>:<hours>:<minutes>:<seconds>:<milliseconds>`, bijvoorbeeld `49:16:48:00.00`. 
### <a name="restart-windows"></a>Opnieuw opstarten van Windows
`restart-computer`

Toe te voegen `-force` forceert een actieve toepassingen te sluiten zonder waarschuwing gebruikers.
## <a name="instance-metadata"></a>Metagegevens van het exemplaar

U kunt de metagegevens van de Azure-instantie van een query binnen uw Azure-VM voor meer informatie, zoals osType, locatie, vmSize, vmId, naam, resourceGroupName, abonnements-id, privateIpAddress en openbare IP-adres.

Opvragen van metagegevens van het exemplaar is vereist in orde Gast netwerkverbinding, omdat het een REST-aanroep via de Azure-host naar de instance metadata-service maakt. Dus als u op metagegevens van het exemplaar een query uitgevoerd, staat waarin dat de Gast kan communiceren via het netwerk aan een service wordt gehost op Azure.

Zie voor meer informatie, [Azure Instance Metadata service](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service).

### <a name="instance-metadata"></a>Metagegevens van het exemplaar
`$im = invoke-restmethod -headers @{"metadata"="true"} -uri http://169.254.169.254/metadata/instance?api-version=2017-08-01 -method get`

`$im | convertto-json`
### <a name="os-type-instance-metadata"></a>Type besturingssysteem (metagegevens van het exemplaar)
`$im.Compute.osType`
### <a name="location-instance-metadata"></a>Locatie (metagegevens van het exemplaar)
`$im.Compute.Location`
### <a name="size-instance-metadata"></a>Grootte (metagegevens van het exemplaar)
`$im.Compute.vmSize`
### <a name="vm-id-instance-metadata"></a>VM-ID (metagegevens van het exemplaar)
`$im.Compute.vmId`
### <a name="vm-name-instance-metadata"></a>VM-naam (metagegevens van het exemplaar)
`$im.Compute.name`
### <a name="resource-group-name-instance-metadata"></a>Naam van de resourcegroep (metagegevens van het exemplaar)
`$im.Compute.resourceGroupName`
### <a name="subscription-id-instance-metadata"></a>Abonnements-ID (metagegevens van het exemplaar)
`$im.Compute.subscriptionId`
### <a name="tags-instance-metadata"></a>Tags (metagegevens van het exemplaar)
`$im.Compute.tags`
### <a name="placement-group-id-instance-metadata"></a>Plaatsing groeps-ID (metagegevens van het exemplaar)
`$im.Compute.placementGroupId`
### <a name="platform-fault-domain-instance-metadata"></a>Platformfoutdomein (metagegevens van het exemplaar)
`$im.Compute.platformFaultDomain`
### <a name="platform-update-domain-instance-metadata"></a>Updateplatform (metagegevens van het exemplaar)
`$im.Compute.platformUpdateDomain`
### <a name="ipv4-private-ip-address-instance-metadata"></a>IPv4-particulier IP-adres (metagegevens van het exemplaar)
`$im.network.interface.ipv4.ipAddress.privateIpAddress`
### <a name="ipv4-public-ip-address-instance-metadata"></a>IPv4-openbare IP-adres (metagegevens van het exemplaar)
`$im.network.interface.ipv4.ipAddress.publicIpAddress`
### <a name="ipv4-subnet-address--prefix-instance-metadata"></a>IPv4-subnetadres / voorvoegsel (metagegevens exemplaar)
`$im.network.interface.ipv4.subnet.address`

`$im.network.interface.ipv4.subnet.prefix`
### <a name="ipv6-ip-address-instance-metadata"></a>IPv6-IP-adres (metagegevens van het exemplaar)
`$im.network.interface.ipv6.ipAddress`
### <a name="mac-address-instance-metadata"></a>MAC-adres (metagegevens van het exemplaar)
`$im.network.interface.macAddress`

## <a name="next-steps"></a>Volgende stappen
* De belangrijkste seriële console van het Windows-documentatiepagina bevindt [hier](serial-console-windows.md).
* Seriële console van het is ook beschikbaar voor [Linux](serial-console-linux.md) VM's.
* Meer informatie over [diagnostische gegevens over opstarten](boot-diagnostics.md).
