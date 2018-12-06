---
title: Verificatiefouten oplossen wanneer u verbinding maken met virtuele Azure-machine met RDP | Microsoft Docs
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 11/01/2018
ms.author: delhan
ms.openlocfilehash: 47d3b827099d3a4a7520ac66765d2928795b6e49
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52967977"
---
# <a name="troubleshoot-authentication-errors-when-you-use-rdp-to-connect-to-azure-vm"></a>Verificatiefouten oplossen wanneer u verbinding maken met virtuele Azure-machine met RDP

In dit artikel kunt u verificatie oplossen die zich voordoen wanneer u Remote Desktop Protocol (RDP)-verbinding om verbinding met een Azure-machine (VM) te gebruiken.

## <a name="symptoms"></a>Symptomen

Vastleggen van een schermopname van een Azure-VM die het welkomstscherm wordt weergegeven en geeft aan dat het besturingssysteem wordt uitgevoerd. Echter ontvangt, wanneer u probeert verbinding maken met de virtuele machine met behulp van de verbinding met extern bureaublad, u een van de volgende foutberichten weergegeven.

### <a name="error-message-1"></a>Foutbericht 1

**Er is een verificatiefout opgetreden. Kan geen verbinding worden gemaakt met de lokale certificeringsinstantie.**

### <a name="error-message-2"></a>Foutbericht 2

**De externe computer die u wilt verbinding maken met het netwerk Level Authentication (NLA) is vereist, maar uw Windows-domeincontroller niet bereikbaar om uit te voeren NLA. Als u een beheerder zijn op de externe computer, kunt u NLA uitschakelen met behulp van de opties op het tabblad Extern in het dialoogvenster Systeemeigenschappen.**

### <a name="error-message-3-generic-connection-error"></a>Foutbericht 3 (algemene verbindingsfout)

**Deze computer kan geen verbinding maken met de externe computer. Probeer het opnieuw, verbinding te maken als het probleem zich blijft voordoen, neem contact op met de eigenaar van de externe computer of uw netwerkbeheerder.**

## <a name="cause"></a>Oorzaak

Er zijn meerdere redenen waarom NLA de RDP-toegang tot een virtuele machine blokkeert.

### <a name="cause-1"></a>Oorzaak 1

De virtuele machine kan niet communiceren met de domeincontroller (DC). Dit probleem kan voorkomen dat een RDP-sessie openen van een virtuele machine met behulp van domeinreferenties. Echter, u zou nog steeds kunnen aanmelden met behulp van de lokale Administrator-referenties. Dit probleem kan optreden in de volgende situaties:

1. De Active Directory-beveiligingskanaal tussen deze virtuele machine en de domeincontroller is verbroken.

2. De virtuele machine heeft een oude versie van het accountwachtwoord en de domeincontroller heeft een nieuwe kopie.

3. De domeincontroller die deze virtuele machine verbinding met maakt is beschadigd.

### <a name="cause-2"></a>Oorzaak 2

Het versleutelingsniveau van de virtuele machine is hoger dan de versie die wordt gebruikt door de clientcomputer.

### <a name="cause-3"></a>Oorzaak 3

De TLS 1.0, 1.1 of 1.2 (server)-protocollen zijn uitgeschakeld op de virtuele machine.

### <a name="cause-4"></a>Oorzaak 4

De virtuele machine is ingesteld om uit te schakelen met behulp van domeinreferenties aanmelden en de Local Security Authority (LSA) is niet correct ingesteld.

### <a name="cause-5"></a>Oorzaak 5

De virtuele machine is ingesteld voor het accepteren van alleen Federal Information Processing Standard (FIPS)-compatibele algoritme verbindingen. Dit gebeurt meestal met behulp van Active Directory-beleid. Dit is een zeldzame configuratie, maar FIPS kan worden afgedwongen voor extern bureaublad-verbindingen.

## <a name="before-you-troubleshoot"></a>Voordat u oplossen

### <a name="create-a-backup-snapshot"></a>Een momentopname van een back-up maken

Volg de stappen in voor het maken van een back-upmomentopname [momentopname maken van een schijf](../windows/snapshot-copy-managed-disk.md).

### <a name="connect-to-the-vm-remotely"></a>Verbinding maken met de virtuele machine op afstand

Voor verbinding met de virtuele machine op afstand, gebruikt u een van de methoden in [externe hulpprogramma's gebruiken om problemen met virtuele Azure-machine te](remote-tools-troubleshoot-azure-vm-issues.md).

### <a name="group-policy-client-service"></a>Group policy client-service

Als dit een VM domein is, moet u eerst de Group Policy Client-service om te voorkomen dat een Active Directory-beleid van het overschrijven van de wijzigingen stoppen. U doet dit door de volgende opdracht uitvoeren:

```cmd
REM Disable the member server to retrieve the latest GPO from the domain upon start
REG add "HKLM\SYSTEM\CurrentControlSet\Services\gpsvc" /v Start /t REG_DWORD /d 4 /f
```

Nadat het probleem is opgelost, herstelt u de mogelijkheid van deze virtuele machine om contact op met het domein om op te halen van de meest recente GPO uit het domein. Voer hiervoor de volgende opdrachten:

```cmd
sc config gpsvc start= auto
sc start gpsvc

gpupdate /force
```

Als de wijziging is hersteld, betekent dit dat het probleem wordt veroorzaakt door een Active Directory-beleid. 

### <a name="workaround"></a>Tijdelijke oplossing

U kunt dit probleem omzeilen, voer de volgende opdrachten in het opdrachtvenster NLA uitschakelen:

```cmd
REM Disable the Network Level Authentication
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 0
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 0
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 0
```

Start de virtuele machine.

NLA opnieuw in te schakelen, voert u de volgende opdracht uit en start de virtuele machine opnieuw op:

```cmd
REG add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v disabledomaincreds /t REG_DWORD /d 0 /f

REG add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 1 /f
REG add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 1 /f
REG add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 1 /f
```

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="for-domain-joined-vms"></a>Voor domein-VM 's

Als u wilt dit probleem op te lossen, Controleer eerst of de virtuele machine verbinding met een domeincontroller maken kan, en of de domeincontroller de status 'gezond heeft' en kan worden verwerkt-van de virtuele machine aanvragen.

>[!Note] 
>Als u wilt testen van de DC-status, kunt u een andere virtuele machine in hetzelfde VNET en Subnet die dezelfde aanmeldingsserver delen.

Als u verbinding maken met de virtuele machine waarop het probleem met behulp van de seriële console, externe CMD of externe PowerShell, op basis van de stappen in de sectie 'Verbinding maken met de virtuele machine op afstand'.

Om te bepalen welke de virtuele machine verbinding met maakt DC, voer de volgende opdracht in de console: 

```cmd
set | find /i "LOGONSERVER"
```

Controleer de status van het beveiligde kanaal tussen de virtuele machine en de domeincontroller. U doet dit door de volgende opdracht uit in een verhoogde PowerShell-sessie worden uitgevoerd. Met deze opdracht retourneert een Booleaanse vlag die aangeeft of het beveiligde kanaal actief is:

```powershell
Test-ComputerSecureChannel -verbose
```

Nadat het kanaal verbroken is, voer de volgende opdracht uit om deze te herstellen:

```powershell
Test-ComputerSecureChannel -repair
```

Zorg ervoor dat het wachtwoord van computeraccount in Active Directory is bijgewerkt op de virtuele machine en de domeincontroller:

```powershell
Reset-ComputerMachinePassword -Server "<COMPUTERNAME>" -Credential <DOMAIN CREDENTIAL WITH DOMAIN ADMIN LEVEL>
```

Als de communicatie tussen de domeincontroller en de virtuele machine goed is, maar de domeincontroller is niet goed genoeg is om een RDP-sessie te openen, kunt u proberen om opnieuw te starten van de domeincontroller.

Als de voorgaande opdrachten niet het communicatieprobleem bij het domein opgelost is, kunt u zich weer aanmelden bij deze virtuele machine aan het domein. Voer de volgende stappen uit om dit te doen:

1. Maken van een script met de naam Unjoin.ps1 met behulp van de volgende inhoud en klikt u vervolgens het script te implementeren als een extensie voor aangepaste scripts op de Azure-portal:

    ```cmd
    cmd /c "netdom remove <<MachineName>> /domain:<<DomainName>> /userD:<<DomainAdminhere>> /passwordD:<<PasswordHere>> /reboot:10 /Force"
    ```
    
    Met dit script geforceerd wordt van de virtuele machine buiten het domein en start deze 10 seconden later opnieuw. Hebt u voor het opschonen van het computerobject aan het domein.

2.  Nadat het opschonen is voltooid, opnieuw lid worden van deze virtuele machine aan het domein. Om dit te doen, maakt u een script met de naam JoinDomain.ps1 met behulp van de volgende inhoud en klikt u vervolgens het script te implementeren als een extensie voor aangepaste scripts op de Azure-portal: 

    ```cmd
    cmd /c "netdom join <<MachineName>> /domain:<<DomainName>> /userD:<<DomainAdminhere>> /passwordD:<<PasswordHere>> /reboot:10"
    ```

    >[!Note] 
    >Dit lid wordt van de virtuele machine op het domein met behulp van de opgegeven referenties.

Als het Active Directory-kanaal in orde is, het computerwachtwoord wordt bijgewerkt en de domeincontroller naar behoren werkt, probeert u de volgende stappen uit.

Als het probleem zich blijft voordoen, controleert u of de domeinreferentie is uitgeschakeld. U doet dit door een opdrachtpromptvenster te openen en voer de volgende opdracht om te bepalen of de virtuele machine is ingesteld voor het uitschakelen van domeinaccounts voor aanmelden bij de virtuele machine:

```cmd
REG query "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v disabledomaincreds
```

Als de sleutel is ingesteld op **1**, dit betekent dat de server is ingesteld niet tot de domeinreferenties toestaan. Deze sleutel te wijzigen **0**.

### <a name="for-standalone-vms"></a>Bij zelfstandige virtuele machines

#### <a name="check-minencryptionlevel"></a>MinEncryptionLevel controleren

In een CMD-exemplaar, kunt u de volgende opdracht uitvoeren op de query de **MinEncryptionLevel** registerwaarde:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel
```

Op basis van de registerwaarde, als volgt te werk:

* 4 (FIPS): Ga naar [controleren FIPs-compatibele algoritmes verbindingen](#fips-compliant).

* 3 (128-bits codering): de ernst ingesteld op **2** door het uitvoeren van de volgende opdracht uit:

    ```cmd
    reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel /t REG_DWORD /d 2 /f
    ```

* 2 (hoogste versleuteling mogelijk, zoals bepaald door de client): U kunt proberen om in te stellen de versleuteling aan de minimale waarde van **1** door het uitvoeren van de volgende opdracht uit:

    ```cmd
    reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel /t REG_DWORD /d 1 /f
    ```
    
Start de VM opnieuw zodat de wijzigingen in het register van kracht.

#### <a name="tls-version"></a>TLS-versie

Afhankelijk van het systeem gebruikt RDP het protocol TLS 1.0, 1.1 of 1.2 (server). Open een CMD-exemplaar om te vragen hoe deze protocollen zijn ingesteld op de virtuele machine, en voer de volgende opdrachten:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled
```

Als de geretourneerde waarden niet alle **1**, dit betekent dat het protocol is uitgeschakeld. Als u deze protocollen, voert u de volgende opdrachten:

```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f
```

U kunt de volgende opdrachten uitvoeren voor andere protocolversies:

<pre lang="bat">
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS <i>x.x</i>\Server" /v Enabled
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS <i>x.x</i>\Server" /v Enabled
</pre>

> [!Note]
> De SSH/TLS-versie x.x ophalen van Logboeken voor het gastbesturingssysteem op de SCHANNEL-fouten.

#### <a name="fips-compliant"></a> FIPs-compatibele algoritmes verbindingen controleren

Extern bureaublad kan worden afgedwongen voor het gebruik van alleen FIPs-compatibele algoritme voor verbindingen. Dit kan worden ingesteld met behulp van een registersleutel. U doet dit door een opdrachtpromptvenster open en vervolgens query uitvoeren op de volgende sleutels:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy" /v Enabled
```

Als de opdracht retourneert **1**, wijzig de registerwaarde in **0**.

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy" /v Enabled /t REG_DWORD /d 0
```

Controleer welke de huidige MinEncryptionLevel op de virtuele machine is:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel
```

Als de opdracht retourneert **4**, wijzig de registerwaarde in **2**

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel /t REG_DWORD /d 2
```

Start de VM opnieuw zodat de wijzigingen in het register van kracht.

## <a name="next-steps"></a>Volgende stappen

[SetEncryptionLevel-methode van de klasse Win32_TSGeneralSetting](https://docs.microsoft.com/windows/desktop/TermServ/win32-tsgeneralsetting-setencryptionlevel)

[Server-verificatie en versleutelingsniveaus configureren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770833(v=ws.11))

[Win32_TSGeneralSetting klasse](https://docs.microsoft.com/windows/desktop/TermServ/win32-tsgeneralsetting)
