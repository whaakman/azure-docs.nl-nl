---
title: De seriële console van virtuele machine van Azure | Microsoft Docs
description: Bidirectionele seriële console voor Azure virtual machines.
services: virtual-machines-windows
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/05/2018
ms.author: harijay
ms.openlocfilehash: 8e9e98a0c9f1db805d62cdee038fb4754ff047e2
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960437"
---
# <a name="virtual-machine-serial-console-preview"></a>Seriële console van de virtuele machine (preview) 


De seriële console van de virtuele machine in Azure biedt toegang tot een op tekst gebaseerde console voor Linux en Windows virtuele machines. Deze seriële verbinding naar de seriële poort COM1 van de virtuele machine is en biedt toegang tot de virtuele machine en niet zijn gerelateerd aan het virtuele machinenetwerk / systeemstatus besturingssysteem. Toegang tot de seriële console voor een virtuele machine worden uitgevoerd alleen via de Azure-portal momenteel en alleen voor gebruikers die VM Inzender hebben of hoger toegang tot de virtuele machine toegestaan. 

> [!Note] 
> Voorbeelden zijn beschikbaar gemaakt voor u op voorwaarde dat u akkoord met de gebruiksvoorwaarden gaat. Zie voor meer informatie [Microsoft Azure aanvullende gebruiksvoorwaarden voor Microsoft Azure-Previews.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Deze service is momenteel **openbare preview** en toegang tot de seriële console voor virtuele machines is beschikbaar voor globale Azure-regio's. Seriële console is op dit moment niet beschikbaar Azure Government, Duitse Azure en Azure voor China cloud.

 

## <a name="prerequisites"></a>Vereisten 

* U moet gebruikmaken van de resource management-implementatiemodel. Klassieke implementaties worden niet ondersteund. 
* Virtuele machine [opstarten diagnostics](boot-diagnostics.md) ingeschakeld 
* Het account met behulp van de seriële console moet hebben [rol van Inzender](../../role-based-access-control/built-in-roles.md) voor de virtuele machine en de [opstarten diagnostics](boot-diagnostics.md) storage-account. 

## <a name="open-the-serial-console"></a>Open de seriële console
seriële console voor virtuele machines is alleen toegankelijk via [Azure-portal](https://portal.azure.com). Hieronder volgen de stappen voor toegang tot de seriële console voor virtuele machines via de portal 

  1. De Azure portal openen
  2. Selecteer de virtuele machines in het menu links.
  3. Klik op de virtuele machine in de lijst. De overzichtspagina van de virtuele machine wordt geopend.
  4. Schuif helemaal naar de ondersteuning + sectie Troubleshooting en klik op de optie voor de seriële console (Preview). Een nieuw deelvenster met de seriële console openen en de verbinding wordt gestart.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

### <a name="disable-feature"></a>Functie uitschakelen
De functionaliteit van de seriële console kan worden gedeactiveerd voor specifieke virtuele machines door die VM boot diagnostics instelling uit te schakelen.

## <a name="serial-console-security"></a>Beveiliging van de seriële console 

### <a name="access-security"></a>-Beveiliging 
Toegang tot de seriële console is beperkt tot gebruikers die hebben [VM inzenders](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) of boven de toegang tot de virtuele machine. Als uw AAD-tenant is vereist voor multi-factor Authentication wordt toegang tot de seriële console ook de MFA moet omdat de toegang [Azure-portal](https://portal.azure.com).

### <a name="channel-security"></a>Kanaal-beveiliging
Alle gegevens wordt teruggestuurd en weer op de kabel wordt versleuteld.

### <a name="audit-logs"></a>Controlelogboeken
Alle toegang tot de seriële console momenteel is aangemeld de [opstarten diagnostics](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) logboeken van de virtuele machine. Toegang tot deze logboeken zijn het eigendom en beheerd door de beheerder van de virtuele machine van Azure.  

>[!CAUTION] 
Terwijl er geen wachtwoorden voor toegang voor de console bent aangemeld als opdrachten uitvoert in de console bevat of uitvoer van wachtwoorden, geheimen, gebruikersnamen of een andere vorm van persoonsgegevens (PII), worden die naar de virtuele machine opstarten diagnostische gegevens geschreven Logboeken, samen met alle andere zichtbare tekst, als onderdeel van de implementatie van de seriële console schuiven back-functionaliteit. Deze logboeken vormen een circulaire en alleen personen met leesmachtigingen voor het opslagaccount voor diagnostische gegevens hebben toegang tot deze, maar het is raadzaam na de aanbevolen procedure van het gebruik van de extern bureaublad voor alles die u mogelijk geheimen en/of PII. 

### <a name="concurrent-usage"></a>Gelijktijdig gebruik
Als een gebruiker is verbonden met de seriële console en een andere gebruiker is om die dezelfde virtuele machine toegang vraagt, de eerste gebruiker wordt verbroken en de tweede gebruiker aangesloten op een manier die lijkt op de eerste gebruiker permanent en het verlaten van de fysieke-console en een nieuwe de gebruiker zich omlaag.

>[!CAUTION] 
Dit betekent dat de gebruiker die wordt verbroken worden niet geregistreerd af! De mogelijkheid om af te dwingen een afmelden bij het verbreken van de verbinding (via SIGHUP of een vergelijkbaar mechanisme) is nog steeds in het overzicht. Voor Windows, er is een automatische time-out ingeschakeld in SAC, maar voor Linux kunt u terminal time-outinstelling configureren. 


## <a name="accessing-serial-console-for-windows"></a>Toegang tot de seriële console van voor Windows 
Nieuwere installatiekopieën van Windows Server op Azure hebt [speciale beheerconsole](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) standaard ingeschakeld. SAC wordt ondersteund in serverversies van Windows, maar is niet beschikbaar op clientversies (bijvoorbeeld Windows 10, Windows 8 of Windows 7). Als u wilt inschakelen voor Windows virtuele machines is gemaakt met behulp van de seriële console Feb2018 of lager installatiekopieën gebruikt u de volgende stappen uit: 

1. Verbinding maken met uw Windows-machine via Extern bureaublad
2. Voer de volgende opdrachten uit een administratieve opdrachtprompt. 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. Opnieuw opstarten van het systeem voor de console SAC zijn ingeschakeld

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Als de benodigde SAC kan offline worden ingeschakeld, evenals 

1. Koppel de windows-schijf die u wilt dat SAC geconfigureerd voor als een gegevensschijf met bestaande virtuele machine. 
2. Voer de volgende opdrachten uit een administratieve opdrachtprompt. 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled-or-not"></a>Hoe weet ik of SAC of niet is ingeschakeld 

Als [SAC] (https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) is niet ingeschakeld de seriële console wordt niet weergegeven voor de SAC-prompt. Deze een VM-statusgegevens in sommige gevallen kunt weergeven of het normaal zou zijn leeg.  

### <a name="enabling-boot-menu-to-show-in-the-serial-console"></a>Opstartmenu om weer te geven in de seriële console inschakelen 

Als u wilt inschakelen, Windows-opstartlaadprogramma wordt u gevraagd om weer te geven in de seriële console kunt u de volgende aanvullende opties toevoegen aan Windows-opstartlaadprogramma.

1. Verbinding maken met uw Windows-machine via Extern bureaublad
2. Voer de volgende opdrachten uit een administratieve opdrachtprompt. 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 5`
* `bcdedit /set {bootmgr} bootems yes`
3. Opnieuw opstarten van het systeem voor het opstartmenu worden ingeschakeld

> [!NOTE] 
> Op dit punt ondersteuning voor de functie sleutels niet is ingeschakeld, als u geavanceerde opstartopties bcdedit/set {huidige} onetimeadvancedoptions gebruiken op vereist, Zie [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set) voor meer informatie

## <a name="windows-commands---cmd"></a>Opdrachten van Windows - CMD 

Deze sectie bevat een Voorbeeldopdrachten voor het uitvoeren van algemene taken in scenario's waarin u SAC moet mogelijk voor toegang tot de virtuele machine, zoals wanneer u problemen met RDP verbinding moet gebruiken.

SAC sinds Windows Server 2003 is opgenomen in alle versies van Windows, maar is standaard uitgeschakeld. SAC is afhankelijk van de `sacdrv.sys` kernelstuurprogramma de `Special Administration Console Helper` service (`sacsvr`), en de `sacsess.exe` proces. Zie voor meer informatie [Emergency Management Services Tools and Settings](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc787940(v%3dws.10)).

SAC kunt u verbinding maken met uw actieve besturingssysteem via de seriële poort. Wanneer u CMD van SAC, start `sacsess.exe` Start `cmd.exe` binnen uw actieve besturingssysteem. U kunt zien dat in de taak Manager als u RDP met uw virtuele machine op de tijd dat u verbonden bent met SAC via de seriële console-functie. De CMD u via SAC openen is hetzelfde `cmd.exe` u gebruiken wanneer verbonden via RDP. Dezelfde zijn opdrachten en hulpprogramma's beschikbaar, waaronder de mogelijkheid PowerShell uit die CMD-sessie starten. Is een belangrijk verschil tussen SAC en Windows Recovery Environment (WinRE) in die SAC is zodat u uw actieve besturingssysteem, waar WinRE wordt opgestart naar een andere, minimale besturingssysteem beheren. Terwijl Azure VM's bieden geen ondersteuning voor toegang tot WinRE, met de seriële console-functie kunnen virtuele Azure-machines worden beheerd via SAC.

Omdat SAC beperkt tot een 80 24 x schermbuffer met geen Schuif terug is, toevoegen `| more` voor opdrachten voor het weergeven van de uitvoer één pagina tegelijk. Gebruik `<spacebar>` om te zien van de volgende pagina of `<enter>` om te zien van de volgende regel.  

`SHIFT+INSERT` de snelkoppeling plakken voor de seriële console-venster is.

Vanwege SAC van de schermbuffer beperkt, langer opdrachten mogelijk beter te typen in een lokale teksteditor en vervolgens in SAC worden geplakt.

### <a name="view-and-edit-windows-registry-settings"></a>Windows-registerinstellingen weergeven en bewerken
#### <a name="verify-rdp-is-enabled"></a>Controleer of dat RDP is ingeschakeld
`reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections`

`reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections`

De tweede sleutel (onder \Policies) is alleen aanwezig als de relevante groepsbeleidsinstelling is geconfigureerd.

#### <a name="enable-rdp"></a>Schakelt u RDP
`reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0`

`reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0` 

De tweede sleutel (onder \Policies) alleen nodig als de relevante groepsbeleidinstelling had geconfigureerd. Waarde wordt op de volgende vernieuwen van Groepsbeleid worden herschreven als deze is geconfigureerd in Groepsbeleid.

### <a name="manage-windows-services"></a>Windows Services beheren

#### <a name="view-service-state"></a>Status van de service weergeven
`sc query termservice`
####  <a name="view-service-logon-account"></a>De aanmeldingsaccount voor de weergave-service
`sc qc termservice`
#### <a name="set-service-logon-account"></a>Aanmeldingsaccount service instellen 
`sc config termservice obj= "NT Authority\NetworkService"`

Een spatie is na het gelijkteken vereist.
#### <a name="set-service-start-type"></a>Opstarttype set-service
`sc config termservice start= demand` 

Een spatie is na het gelijkteken vereist. Start mogelijke waarden zijn `boot`, `system`, `auto`, `demand`, `disabled`, `delayed-auto`.
#### <a name="set-service-dependencies"></a>Set-serviceafhankelijkheden
`sc config termservice depend= RPCSS`

Een spatie is na het gelijkteken vereist.
#### <a name="start-service"></a>Service starten
`net start termservice`

of

`sc start termservice`
#### <a name="stop-service"></a>Service stoppen
`net stop termservice`

of

`sc stop termservice`
### <a name="manage-networking-features"></a>Netwerkfuncties beheren
#### <a name="show-nic-properties"></a>NIC-eigenschappen weergeven
`netsh interface show interface` 
#### <a name="show-ip-properties"></a>IP-eigenschappen weergeven
`netsh interface ip show config`
#### <a name="show-ipsec-configuration"></a>IPSec-configuratie weergeven
`netsh nap client show configuration`  
#### <a name="enable-nic"></a>NIC inschakelen
`netsh interface set interface name="<interface name>" admin=enabled`
#### <a name="set-nic-to-use-dhcp"></a>Stel NIC DHCP te gebruiken
`netsh interface ip set address name="<interface name>" source=dhcp`

Virtuele machines in Azure moeten altijd worden geconfigureerd in het gastbesturingssysteem een IP-adres verkrijgen via DHCP. De Azure statische IP-instelling wordt nog steeds DHCP gebruikt het statische IP-adres geven tot de virtuele machine.
#### <a name="ping"></a>Ping
`ping 8.8.8.8` 
#### <a name="port-ping"></a>Poortping  
De Telnet-client installeren

`dism /online /Enable-Feature /FeatureName:TelnetClient`

Connectiviteit testen

`telnet bing.com 80`

De Telnet-client verwijderen

`dism /online /Disable-Feature /FeatureName:TelnetClient`

Als beperkt tot de methoden die beschikbaar zijn in Windows standaard worden PowerShell een betere benadering voor het testen van poort connectiviteit. Zie de sectie PowerShell hieronder voor voorbeelden.
#### <a name="test-dns-name-resolution"></a>DNS-naamomzetting te testen
`nslookup bing.com`
#### <a name="show-windows-firewall-rule"></a>Windows Firewall-regel weergeven
`netsh advfirewall firewall show rule name="Remote Desktop - User Mode (TCP-In)"`
#### <a name="disable-windows-firewall"></a>Windows Firewall uitschakelen
`netsh advfirewall set allprofiles state off`

U kunt deze opdracht gebruiken bij het oplossen van tijdelijk sprake is van de Windows Firewall. Wordt niet inschakelen op het systeem opnieuw is opgestart of wanneer u enaable met behulp van de onderstaande opdracht. Stop de service Windows Firewall (MPSVC) of Base filteren Engine (BFE)-service als manier sprake is van de Windows Firewall. MPSVC of BFE stoppen leidt ertoe dat alle verbindingen worden geblokkeerd.
#### <a name="enable-windows-firewall"></a>Windows Firewall inschakelen
`netsh advfirewall set allprofiles state on`
### <a name="manage-users-and-groups"></a>Gebruikers en groepen beheren
#### <a name="create-local-user-account"></a>Lokale gebruikersaccount maken
`net user /add <username> <password>`
#### <a name="add-local-user-to-local-group"></a>Lokale gebruiker toevoegen aan de lokale groep
`net localgroup Administrators <username> /add`
#### <a name="verify-user-account-is-enabled"></a>Controleer of het gebruikersaccount is ingeschakeld
`net user <username> | find /i "active"`

Virtuele machines in Azure gemaakt op basis van algemene installatiekopie heeft het lokale beheerdersaccount gewijzigd in de naam van de opgegeven tijdens het inrichten van virtuele machine. Het wordt dus meestal niet `Administrator`.
#### <a name="enable-user-account"></a>Gebruikersaccount inschakelen
`net user <username> /active:yes`  
#### <a name="view-user-account-properties"></a>Eigenschappen van gebruikersaccount weergeven
`net user <username>`

Voorbeeldregels van belang vanuit een lokale Administrator-account:

`Account active Yes`

`Account expires Never`

`Password expires Never`

`Workstations allowed All`

`Logon hours allowed All`

`Local Group Memberships *Administrators`

#### <a name="view-local-groups"></a>Lokale groepen weergeven
`net localgroup`
### <a name="manage-the-windows-event-log"></a>Het Windows-gebeurtenislogboek beheren
#### <a name="query-event-log-errors"></a>Query gebeurtenislogboek fouten
`wevtutil qe system /c:10 /f:text /q:"Event[System[Level=2]]" | more`

Wijziging `/c:10` naar het gewenste aantal gebeurtenissen geretourneerd, of verplaats deze naar het retourneren van alle gebeurtenissen die overeenkomen met het filter.
#### <a name="query-event-log-by-event-id"></a>Gebeurtenislogboek van de query door de gebeurtenis-ID
`wevtutil qe system /c:1 /f:text /q:"Event[System[EventID=11]]" | more`
#### <a name="query-event-log-by-event-id-and-provider"></a>Gebeurtenislogboek van de query door de gebeurtenis-ID en -Provider
`wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11]]" | more`
#### <a name="query-event-log-by-event-id-and-provider-for-the-last-24-hours"></a>Gebeurtenislogboek query door de gebeurtenis-ID en -Provider voor de afgelopen 24 uur
`wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11 and TimeCreated[timediff(@SystemTime) <= 86400000]]]"`

Gebruik `604800000` om te zoeken terug 7 dagen in plaats van 24 uur.
#### <a name="query-event-log-by-event-id-provider-and-eventdata-in-the-last-7-days"></a>Gebeurtenislogboek door gebeurtenis-ID-Provider en EventData-query in de afgelopen 7 dagen
`wevtutil qe security /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Security-Auditing'] and EventID=4624 and TimeCreated[timediff(@SystemTime) <= 604800000]] and EventData[Data[@Name='TargetUserName']='<username>']]" | more`
### <a name="view-or-remove-installed-applications"></a>Weergeven of verwijderen van de geïnstalleerde toepassingen
#### <a name="list-installed-applications"></a>Lijst met geïnstalleerde toepassingen
`wmic product get Name,InstallDate | sort /r | more`

De `sort /r` sorteringen Aflopend op installatiedatum gemakkelijker om te zien wat er is onlangs hebt geïnstalleerd. Gebruik `<spacebar>` om door te gaan naar de volgende pagina van uitvoer, of `<enter>` om door te gaan van één regel.
#### <a name="uninstall-an-application"></a>Een toepassing verwijderen
`wmic path win32_product where name="<name>" call uninstall`

Vervang `<name>` met de naam die wordt geretourneerd in de bovenstaande opdracht voor de toepassing die u wilt verwijderen.

### <a name="file-system-management"></a>File System Management
#### <a name="get-file-version"></a>Bestandsversie ophalen
`wmic datafile where "drive='C:' and path='\\windows\\system32\\drivers\\' and filename like 'netvsc%'" get version /format:list`

In dit voorbeeld retourneert de versie van de virtuele NIC-stuurprogramma dat netvsc.sys, netvsc63.sys of netvsc60.sys afhankelijk van de Windows-versie.
#### <a name="scan-for-system-file-corruption"></a>Op beschadiging van systeembestanden scannen
`sfc /scannow`

Zie ook [herstellen van een Windows-installatiekopie](https://docs.microsoft.com/windows-hardware/manufacture/desktop/repair-a-windows-image).
#### <a name="scan-for-system-file-corruption"></a>Op beschadiging van systeembestanden scannen
`dism /online /cleanup-image /scanhealth`

Zie ook [herstellen van een Windows-installatiekopie](https://docs.microsoft.com/windows-hardware/manufacture/desktop/repair-a-windows-image).
#### <a name="export-file-permissions-to-text-file"></a>Bestandsmachtigingen exporteren naar bestand
`icacls %programdata%\Microsoft\Crypto\RSA\MachineKeys /t /c > %temp%\MachineKeys_permissions_before.txt`
#### <a name="save-file-permissions-to-acl-file"></a>Machtigingen voor bestanden in de ACL-bestand opslaan
`icacls %programdata%\Microsoft\Crypto\RSA\MachineKeys /save %temp%\MachineKeys_permissions_before.aclfile /t`  
#### <a name="restore-file-permissions-from-acl-file"></a>Bestandsmachtigingen van ACL-bestand terugzetten
`icacls %programdata%\Microsoft\Crypto\RSA /save %temp%\MachineKeys_permissions_before.aclfile /t`

Het pad wanneer u `/restore` moet de bovenliggende map van de map die u hebt opgegeven bij gebruik van `/save`. In dit voorbeeld `\RSA` is het bovenliggende lid van de `\MachineKeys` map die is opgegeven de `/save` in bovenstaand voorbeeld.
#### <a name="take-ntfs-ownership-of-a-folder"></a>NTFS-eigenaar van een map
`takeown /f %programdata%\Microsoft\Crypto\RSA\MachineKeys /a /r`  
#### <a name="grant-ntfs-permissions-to-a-folder-recursively"></a>NTFS-machtigingen toekennen voor een recursief map
`icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"`  
### <a name="manage-devices"></a>Apparaten beheren
#### <a name="remove-non-present-pnp-devices"></a>Verwijderen van PNP-apparaten niet aanwezig
`%windir%\System32\RUNDLL32.exe %windir%\System32\pnpclean.dll,RunDLL_PnpClean /Devices /Maxclean`
### <a name="manage-group-policy"></a>Beheren van Groepsbeleid
#### <a name="force-group-policy-update"></a>Bijwerken van groepsbeleid afdwingen
`gpupdate /force /wait:-1`
### <a name="miscellaneous-tasks"></a>Diverse taken
#### <a name="show-os-version"></a>De versie van besturingssysteem weergeven
`ver`

of 

`wmic os get caption,version,buildnumber /format:list`

of 

`systeminfo  find /i "os name"`

`systeminfo | findstr /i /r "os.*version.*build"`
#### <a name="view-os-install-date"></a>Weergave OS installatiedatum
`systeminfo | find /i "original"`

of 

`wmic os get installdate`
#### <a name="view-last-boot-time"></a>Weergave opstarttijd laatste
`systeminfo | find /i "system boot time"`
#### <a name="view-time-zone"></a>De tijdzone weergeven
`systeminfo | find /i "time zone"`

of

`wmic timezone get caption,standardname /format:list`
#### <a name="restart-windows"></a>Windows opnieuw starten
`shutdown /r /t 0`

Toe te voegen `/f` forceert waarop toepassingen worden uitgevoerd om te sluiten zonder de gebruikers worden gewaarschuwd.
#### <a name="detect-safe-mode-boot"></a>Opstarten in veilige modus detecteren
`bcdedit /enum | find /i "safeboot"` 

## <a name="windows-commands---powershell"></a>Windows - PowerShell-opdrachten

Als u wilt uitvoeren in SAC, PowerShell als u bij een opdrachtprompt, typt u:

`powershell <enter>`

>[!CAUTION]
De module PSReadLine verwijderen uit de PowerShell-sessie voordat andere PowerShell-opdrachten worden uitgevoerd. Er is een bekend probleem waarbij extra tekens kunnen worden opgenomen in de tekst van het Klembord zijn geplakt als PSReadLine wordt uitgevoerd in een PowerShell-sessie in SAC.

Controleer eerst of PSReadLine is geladen. Het wordt standaard op Windows Server 2016, Windows 10 en latere versies van Windows geladen. Deze zou alleen aanwezig zijn op eerdere versies van Windows als deze is handmatig is geïnstalleerd. 

Als deze opdracht voor een bericht waarin geen uitvoer retourneert, de module is niet geladen en u kunt doorgaan met de PowerShell-sessie in SAC als normale.

`get-module psreadline`

Als de bovenstaande opdracht geretourneerd met de versie van de module PSReadLine, voer de volgende opdracht om te verwijderen. Met deze opdracht niet verwijderen of de module verwijderen en deze alleen verwijderd uit de huidige PowerShell-sessie.

`remove-module psreadline`

### <a name="view-and-edit-windows-registry-settings"></a>Windows-registerinstellingen weergeven en bewerken
#### <a name="verify-rdp-is-enabled"></a>Controleer of dat RDP is ingeschakeld
`get-itemproperty -path 'hklm:\system\curRentcontrolset\control\terminal server' -name 'fdenytsconNections'`

`get-itemproperty -path 'hklm:\software\policies\microsoft\windows nt\terminal services' -name 'fdenytsconNections'`

De tweede sleutel (onder \Policies) is alleen aanwezig als de relevante groepsbeleidsinstelling is geconfigureerd.
#### <a name="enable-rdp"></a>Schakelt u RDP
`set-itemproperty -path 'hklm:\system\curRentcontrolset\control\terminal server' -name 'fdenytsconNections' 0 -type dword`

`set-itemproperty -path 'hklm:\software\policies\microsoft\windows nt\terminal services' -name 'fdenytsconNections' 0 -type dword`

De tweede sleutel (onder \Policies) alleen nodig als de relevante groepsbeleidinstelling had geconfigureerd. Waarde wordt op de volgende vernieuwen van Groepsbeleid worden herschreven als deze is geconfigureerd in Groepsbeleid.
### <a name="manage-windows-services"></a>Windows Services beheren
#### <a name="view-service-details"></a>Servicegegevens weergeven
`get-wmiobject win32_service -filter "name='termservice'" |  format-list Name,DisplayName,State,StartMode,StartName,PathName,ServiceType,Status,ExitCode,ServiceSpecificExitCode,ProcessId`

`Get-Service` kan worden gebruikt, maar niet het aanmeldingsaccount voor de service. `Get-WmiObject win32-service` biedt.
#### <a name="set-service-logon-account"></a>Aanmeldingsaccount service instellen
`(get-wmiobject win32_service -filter "name='termservice'").Change($null,$null,$null,$null,$null,$false,'NT Authority\NetworkService')`

Wanneer u een serviceaccount gebruikt dan `NT AUTHORITY\LocalService`, `NT AUTHORITY\NetworkService`, of `LocalSystem`, geef het wachtwoord als het laatste argument (achtste) na de accountnaam.
#### <a name="set-service-startup-type"></a>Opstarttype service instellen
`set-service termservice -startuptype Manual`

`Set-service` accepteert `Automatic`, `Manual`, of `Disabled` voor opstarttype.
#### <a name="set-service-dependencies"></a>Set-serviceafhankelijkheden
`Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\TermService' -Name DependOnService -Value @('RPCSS','TermDD')`
#### <a name="start-service"></a>Service starten
`start-service termservice`
#### <a name="stop-service"></a>Service stoppen
`stop-service termservice`
### <a name="manage-networking-features"></a>Netwerkfuncties beheren
#### <a name="show-nic-properties"></a>NIC-eigenschappen weergeven
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} |  format-list status,name,ifdesc,macadDresS,driverversion,MediaConNectState,MediaDuplexState`

of 

`get-wmiobject win32_networkadapter -filter "servicename='netvsc'" |  format-list netenabled,name,macaddress`

`Get-NetAdapter` is beschikbaar in 2012 +, 2008 R2 gebruikt `Get-WmiObject`.
#### <a name="show-ip-properties"></a>IP-eigenschappen weergeven
`get-wmiobject Win32_NetworkAdapterConfiguration -filter "ServiceName='netvsc'" |  format-list DNSHostName,IPAddress,DHCPEnabled,IPSubnet,DefaultIPGateway,MACAddress,DHCPServer,DNSServerSearchOrder`
#### <a name="enable-nic"></a>NIC inschakelen
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} | enable-netadapter`

of

`(get-wmiobject win32_networkadapter -filter "servicename='netvsc'").enable()`

`Get-NetAdapter` is beschikbaar in 2012 +, 2008 R2 gebruikt `Get-WmiObject`.
#### <a name="set-nic-to-use-dhcp"></a>Stel NIC DHCP te gebruiken
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} | Set-NetIPInterface -DHCP Enabled`

`(get-wmiobject Win32_NetworkAdapterConfiguration -filter "ServiceName='netvsc'").EnableDHCP()`

`Get-NetAdapter` is beschikbaar op 2012 +. Gebruik voor 2008 R2 `Get-WmiObject`. Virtuele machines in Azure moeten altijd worden geconfigureerd in het gastbesturingssysteem een IP-adres verkrijgen via DHCP. De Azure statische IP-instelling wordt nog steeds DHCP gebruikt om te geven van het IP-adres aan de virtuele machine.
#### <a name="ping"></a>Ping
`test-netconnection`

of

`get-wmiobject Win32_PingStatus -Filter 'Address="8.8.8.8"' | format-table -autosize IPV4Address,ReplySize,ResponseTime`

`Test-Netconnection` zonder parameters wordt geprobeerd te pingen `internetbeacon.msedge.net`. Het is beschikbaar op 2012 +. Gebruik voor 2008 R2 `Get-WmiObject` zoals in het tweede voorbeeld.
#### <a name="port-ping"></a>Ping-poort
`test-netconnection -ComputerName bing.com -Port 80`

of

`(new-object Net.Sockets.TcpClient).BeginConnect('bing.com','80',$null,$null).AsyncWaitHandle.WaitOne(300)`

`Test-NetConnection` is beschikbaar op 2012 +. Voor gebruik van 2008 R2 `Net.Sockets.TcpClient`
#### <a name="test-dns-name-resolution"></a>DNS-naamomzetting te testen
`resolve-dnsname bing.com` 

of 

`[System.Net.Dns]::GetHostAddresses('bing.com')`

`Resolve-DnsName` is beschikbaar op 2012 +. Gebruik voor 2008 R2 `System.Net.DNS`.
#### <a name="show-windows-firewall-rule-by-name"></a>Windows firewall-regel weergeven met de naam
`get-netfirewallrule -name RemoteDesktop-UserMode-In-TCP` 
#### <a name="show-windows-firewall-rule-by-port"></a>Windows firewall-regel door poort weergeven
`get-netfirewallportfilter | where {$_.localport -eq 3389} | foreach {Get-NetFirewallRule -Name $_.InstanceId} | format-list Name,Enabled,Profile,Direction,Action`

of

`(new-object -ComObject hnetcfg.fwpolicy2).rules | where {$_.localports -eq 3389 -and $_.direction -eq 1} | format-table Name,Enabled`

`Get-NetFirewallPortFilter` is beschikbaar op 2012 +. Gebruik voor 2008 R2 de `hnetcfg.fwpolicy2` COM-object. 
#### <a name="disable-windows-firewall"></a>Windows firewall uitschakelen
`Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False`

`Set-NetFirewallProfile` is beschikbaar op 2012 +. Gebruik voor 2008 R2 `netsh advfirewall` waarnaar wordt verwezen, in de bovenstaande CMD-sectie.
### <a name="manage-users-and-groups"></a>Gebruikers en groepen beheren
#### <a name="create-local-user-account"></a>Lokale gebruikersaccount maken
`new-localuser <name>`
#### <a name="verify-user-account-is-enabled"></a>Controleer of het gebruikersaccount is ingeschakeld
`(get-localuser | where {$_.SID -like "S-1-5-21-*-500"}).Enabled`

of 

`(get-wmiobject Win32_UserAccount -Namespace "root\cimv2" -Filter "SID like 'S-1-5-%-500'").Disabled`

`Get-LocalUser` is beschikbaar op 2012 +. Gebruik voor 2008 R2 `Get-WmiObject`. Dit voorbeeld ziet u de ingebouwde lokale administrator-account, die altijd SID `S-1-5-21-*-500`. Virtuele machines in Azure gemaakt op basis van algemene installatiekopie heeft het lokale beheerdersaccount gewijzigd in de naam van de opgegeven tijdens het inrichten van virtuele machine. Het wordt dus meestal niet `Administrator`.
#### <a name="add-local-user-to-local-group"></a>Lokale gebruiker toevoegen aan de lokale groep
`add-localgroupmember -group Administrators -member <username>`
#### <a name="enable-local-user-account"></a>Lokale gebruikersaccount inschakelen
`get-localuser | where {$_.SID -like "S-1-5-21-*-500"} | enable-localuser` 

In dit voorbeeld wordt het ingebouwde lokale administrator-account, die altijd SID `S-1-5-21-*-500`. Virtuele machines in Azure gemaakt op basis van algemene installatiekopie heeft het lokale beheerdersaccount gewijzigd in de naam van de opgegeven tijdens het inrichten van virtuele machine. Het wordt dus meestal niet `Administrator`.
#### <a name="view-user-account-properties"></a>Eigenschappen van gebruikersaccount weergeven
`get-localuser | where {$_.SID -like "S-1-5-21-*-500"} | format-list *`

of 

`get-wmiobject Win32_UserAccount -Namespace "root\cimv2" -Filter "SID like 'S-1-5-%-500'" |  format-list Name,Disabled,Status,Lockout,Description,SID`

`Get-LocalUser` is beschikbaar op 2012 +. Gebruik voor 2008 R2 `Get-WmiObject`. Dit voorbeeld ziet u de ingebouwde lokale administrator-account, die altijd SID `S-1-5-21-*-500`.
#### <a name="view-local-groups"></a>Lokale groepen weergeven
`(get-localgroup).name | sort` `(get-wmiobject win32_group).Name | sort`

`Get-LocalUser` is beschikbaar op 2012 +. Gebruik voor 2008 R2 `Get-WmiObject`.
### <a name="manage-the-windows-event-log"></a>Het Windows-gebeurtenislogboek beheren
#### <a name="query-event-log-errors"></a>Query gebeurtenislogboek fouten
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Level=2]]" | more`

Wijziging `/c:10` naar het gewenste aantal gebeurtenissen geretourneerd, of verplaats deze naar het retourneren van alle gebeurtenissen die overeenkomen met het filter.
#### <a name="query-event-log-by-event-id"></a>Gebeurtenislogboek van de query door de gebeurtenis-ID
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[EventID=11]]" | more`
#### <a name="query-event-log-by-event-id-and-provider"></a>Gebeurtenislogboek van de query door de gebeurtenis-ID en -Provider
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11]]" | more`
#### <a name="query-event-log-by-event-id-and-provider-for-the-last-24-hours"></a>Gebeurtenislogboek query door de gebeurtenis-ID en -Provider voor de afgelopen 24 uur
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11 and TimeCreated[timediff(@SystemTime) <= 86400000]]]"`

Gebruik `604800000` om te zoeken terug 7 dagen in plaats van 24 uur. |
#### <a name="query-event-log-by-event-id-provider-and-eventdata-in-the-last-7-days"></a>Gebeurtenislogboek door gebeurtenis-ID-Provider en EventData-query in de afgelopen 7 dagen
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Security-Auditing'] and EventID=4624 and TimeCreated[timediff(@SystemTime) <= 604800000]] and EventData[Data[@Name='TargetUserName']='<username>']]" | more`
### <a name="view-or-remove-installed-applications"></a>Weergeven of verwijderen van de geïnstalleerde toepassingen
#### <a name="list-installed-software"></a>Lijst met geïnstalleerde software
`get-wmiobject win32_product | select installdate,name | sort installdate -descending | more`
#### <a name="uninstall-software"></a>Software verwijderen
`(get-wmiobject win32_product -filter "Name='<name>'").Uninstall()`
### <a name="file-system-management"></a>File System Management
#### <a name="get-file-version"></a>Bestandsversie ophalen
`(get-childitem $env:windir\system32\drivers\netvsc*.sys).VersionInfo.FileVersion`

In dit voorbeeld retourneert de versie van het virtuele NIC-stuurprogramma met de naam netvsc.sys, netvsc63.sys of netvsc60.sys afhankelijk van de Windows-versie.
#### <a name="download-and-extract-file"></a>Downloaden en uitpakken van bestand
`$path='c:\bin';md $path;cd $path;(new-object net.webclient).downloadfile( ('htTp:/'+'/download.sysinternals.com/files/SysinternalsSuite.zip'),"$path\SysinternalsSuite.zip");(new-object -com shelL.apPlication).namespace($path).CopyHere( (new-object -com shelL.apPlication).namespace("$path\SysinternalsSuite.zip").Items(),16)`

In dit voorbeeld wordt een `c:\bin` map, klikt u vervolgens gedownload en extraheert de Sysinternals-suite met hulpprogramma's in `c:\bin`.
### <a name="miscellaneous-tasks"></a>Diverse taken
#### <a name="show-os-version"></a>De versie van besturingssysteem weergeven
`get-wmiobject win32_operatingsystem | format-list caption,version,buildnumber` 
#### <a name="view-os-install-date"></a>Weergave OS installatiedatum
`(get-wmiobject win32_operatingsystem).converttodatetime((get-wmiobject win32_operatingsystem).installdate)`
#### <a name="view-last-boot-time"></a>Weergave opstarttijd laatste
`(get-wmiobject win32_operatingsystem).lastbootuptime`
#### <a name="view-windows-uptime"></a>Windows bedrijfstijd weergeven
`"{0:dd}:{0:hh}:{0:mm}:{0:ss}.{0:ff}" -f ((get-date)-(get-wmiobject win32_operatingsystem).converttodatetime((get-wmiobject win32_operatingsystem).lastbootuptime))`

Retourneert bedrijfstijd als `<days>:<hours>:<minutes>:<seconds>:<milliseconds>`, bijvoorbeeld `49:16:48:00.00`. 
#### <a name="restart-windows"></a>Windows opnieuw starten
`restart-computer`

Toe te voegen `-force` forceert waarop toepassingen worden uitgevoerd om te sluiten zonder de gebruikers worden gewaarschuwd.
### <a name="instance-metadata"></a>Metagegevens van het exemplaar

U kunt de metagegevens van de Azure-instantie van een query in uw Azure-machine voor meer informatie zoals besturingssysteemtype, locatie, vmSize, vmId, naam, resourceGroupName, subscriptionId, privateIpAddress en publicIpAddress.

Opvragen van metagegevens van het exemplaar is vereist op de netwerkverbinding in orde Gast, omdat een REST-aanroep via de Azure-host met de service van de metagegevens exemplaar op deze manier. Dus als u de metagegevens van het exemplaar query kunnen uitvoeren, ziet die u de Gast kan communiceren via het netwerk met een Azure gehoste service.

Zie voor meer informatie [metagegevens van het exemplaar Azure service](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service).

#### <a name="instance-metadata"></a>Metagegevens van het exemplaar
`$im = invoke-restmethod -headers @{"metadata"="true"} -uri http://169.254.169.254/metadata/instance?api-version=2017-08-01 -method get`

`$im | convertto-json`
#### <a name="os-type-instance-metadata"></a>Type besturingssysteem (metagegevens van het exemplaar)
`$im.Compute.osType`
#### <a name="location-instance-metadata"></a>Locatie (metagegevens van het exemplaar)
`$im.Compute.Location`
#### <a name="size-instance-metadata"></a>Grootte (metagegevens van het exemplaar)
`$im.Compute.vmSize`
#### <a name="vm-id-instance-metadata"></a>VM-ID (metagegevens van het exemplaar)
`$im.Compute.vmId`
#### <a name="vm-name-instance-metadata"></a>VM-naam (metagegevens van het exemplaar)
`$im.Compute.name`
#### <a name="resource-group-name-instance-metadata"></a>De Resourcegroepnaam (metagegevens van het exemplaar)
`$im.Compute.resourceGroupName`
#### <a name="subscription-id-instance-metadata"></a>Abonnements-ID (metagegevens van het exemplaar)
`$im.Compute.subscriptionId`
#### <a name="tags-instance-metadata"></a>Labels (metagegevens van het exemplaar)
`$im.Compute.tags`
#### <a name="placement-group-id-instance-metadata"></a>Plaatsing groeps-ID (metagegevens van het exemplaar)
`$im.Compute.placementGroupId`
#### <a name="platform-fault-domain-instance-metadata"></a>Platformfoutdomein (metagegevens van het exemplaar)
`$im.Compute.platformFaultDomain`
#### <a name="platform-update-domain-instance-metadata"></a>Updatedomein platform (metagegevens van het exemplaar)
`$im.Compute.platformUpdateDomain`
#### <a name="ipv4-private-ip-address-instance-metadata"></a>IPv4-particulier IP-adres (metagegevens van het exemplaar)
`$im.network.interface.ipv4.ipAddress.privateIpAddress`
#### <a name="ipv4-public-ip-address-instance-metadata"></a>IPv4-openbare IP-adres (metagegevens van het exemplaar)
`$im.network.interface.ipv4.ipAddress.publicIpAddress`
#### <a name="ipv4-subnet-address--prefix-instance-metadata"></a>IPv4-subnetadres / voorvoegsel (exemplaar metagegevens)
`$im.network.interface.ipv4.subnet.address`

`$im.network.interface.ipv4.subnet.prefix`
#### <a name="ipv6-ip-address-instance-metadata"></a>IPv6-IP-adres (metagegevens van het exemplaar)
`$im.network.interface.ipv6.ipAddress`
#### <a name="mac-address-instance-metadata"></a>MAC-adres (metagegevens van het exemplaar)
`$im.network.interface.macAddress`

## <a name="errors"></a>Fouten
De meeste fouten zijn tijdelijke aard en adres verbinding deze opnieuw. Onderstaande tabel bevat een overzicht van de fouten en risicobeperking 

Fout                            |   Oplossing 
:---------------------------------|:--------------------------------------------|
Kan niet worden opgehaald van de opstartinstallatiekopie de diagnostische instellingen voor '<VMNAME>'. Zorg ervoor dat diagnostische gegevens voor opstarten is ingeschakeld voor deze virtuele machine voor het gebruik van de seriële console. | Zorg ervoor dat de virtuele machine heeft [opstarten diagnostics](boot-diagnostics.md) ingeschakeld. 
De virtuele machine is gestopt toewijzing ongedaan is gemaakt. Start de virtuele machine en probeer de seriële console-verbinding. | Virtuele machine moet zijn in een gestarte staat toegang tot de seriële console
U hebt niet de vereiste machtigingen voor het gebruik van de seriële console van deze virtuele machine. Controleer of er ten minste VM-rol bijdragerrechten.| Toegang tot de seriële console nodig bepaalde machtiging voor toegang tot. Zie [toegangsvereisten](#prerequisites) voor meer informatie
Kan niet bepalen van de resourcegroep voor het opslagaccount voor diagnostische gegevens van opstarten '<STORAGEACCOUNTNAME>'. Controleer of u diagnostische gegevens over opstarten is ingeschakeld voor deze virtuele machine en u toegang hebt tot dit opslagaccount. | Toegang tot de seriële console nodig bepaalde machtiging voor toegang tot. Zie [toegangsvereisten](#prerequisites) voor meer informatie

## <a name="known-issues"></a>Bekende problemen 
We zijn nog steeds in de preview-stadia voor toegang tot de seriële console, we werken via een aantal bekende problemen, onder de lijst is van deze voor mogelijke oplossingen 

Probleem                           |   Oplossing 
:---------------------------------|:--------------------------------------------|
Er is geen optie met de virtuele machine scale set exemplaar seriële console | Toegang tot de seriële console voor exemplaren van virtuele machine scale set wordt niet ondersteund op het moment van preview.
Roept invoeren nadat de banner verbinding wordt niet weergegeven voor een logboek in prompt | [Roept invoeren, gebeurt er niets](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)
Alleen de statusinformatie wordt weergegeven bij het verbinden met een virtuele machine van Windows| [Windows Health signalen](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md)
Kan niet naar het type SAC vragen als kernelfoutopsporing is ingeschakeld | RDP VM en voer `bcdedit /debug {current} off` vanaf een opdrachtprompt met verhoogde bevoegdheid. Als u niet kunt RDP kunt in plaats daarvan de OS-schijf koppelen aan een andere virtuele machine van Azure en wijzigt u terwijl gekoppeld als een gegevens-schijf met `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`, klikt u vervolgens de schijf weer wisselen.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen 
**Q. Hoe kan ik feedback verzenden?**

A. Feedback geven als het probleem door te gaan naar https://aka.ms/serialconsolefeedback. U kunt ook minder (voorkeur) feedback verzenden via azserialhelp@microsoft.com of in de categorie van de virtuele machine van http://feedback.azure.com 

**Q. Ophalen van een fout 'de bestaande console heeft conflicterende BS-type 'Windows' met het gevraagde type besturingssysteem van Linux?**

A. Dit is een bekend probleem te verhelpen, opent [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) in bash-modus en probeer het opnieuw.

**Q. Ik kan geen toegang tot de seriële console, waar kan ik een ondersteuningsaanvraag bestand?**

A. Deze preview-functie wordt via Azure Preview-voorwaarden voldaan. Ondersteuning voor deze het beste oplossen via kanalen hierboven vermeld. 

## <a name="next-steps"></a>Volgende stappen
* De seriële console is ook beschikbaar voor [Linux](../linux/serial-console.md) virtuele machines
* Meer informatie over [bootdiagnostics](boot-diagnostics.md)
