---
title: Problemen met Azure VM RDP-verbinding oplossen op gebeurtenis-ID | Microsoft Docs
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
ms.openlocfilehash: 4c783c70217a84bbe5ccf15accc4a2bec0b7cca8
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52959679"
---
# <a name="troubleshoot-azure-vm-rdp-connection-issues-by-event-id"></a>Problemen met Azure VM RDP-verbinding oplossen op basis van gebeurtenis-id 

In dit artikel wordt uitgelegd hoe u gebeurtenis-id's gebruiken voor het oplossen van problemen die verhinderen een verbinding met extern bureaublad-protocol (RDP) naar een Azure-virtuele Machine (VM dat).

## <a name="symptoms"></a>Symptomen

U probeert te gebruiken van een sessie van extern bureaublad (RDP)-protocol verbinding maken met een Azure-VM. Nadat u uw referenties hebt ingevoerd, mislukt de verbinding en ontvangt u de volgende strekking weergegeven:

**Deze computer kan geen verbinding maken met de externe computer. Probeer het opnieuw, verbinding te maken als het probleem zich blijft voordoen, neem contact op met de eigenaar van de externe computer of uw netwerkbeheerder.**

Om dit probleem wilt oplossen, Controleer de gebeurtenislogboeken op de virtuele machine en Raadpleeg de volgende scenario's.

## <a name="before-you-troubleshoot"></a>Voordat u oplossen

### <a name="create-a-backup-snapshot"></a>Een momentopname van een back-up maken

Volg de stappen in voor het maken van een back-upmomentopname [momentopname maken van een schijf](../windows/snapshot-copy-managed-disk.md).

### <a name="connect-to-the-vm-remotely"></a>Verbinding maken met de virtuele machine op afstand

Voor verbinding met de virtuele machine op afstand, gebruikt u een van de methoden in [externe hulpprogramma's gebruiken om problemen met virtuele Azure-machine te](remote-tools-troubleshoot-azure-vm-issues.md).

## <a name="scenario-1"></a>Scenario 1

### <a name="event-logs"></a>Gebeurtenislogboeken

Voer de volgende opdrachten om te controleren of gebeurtenis 1058 of 1057 is vastgelegd in het systeemlogboek die in de afgelopen 24 uur in een CMD-exemplaar:

```cmd
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-TerminalServices-RemoteConnectionManager'] and EventID=1058 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-TerminalServices-RemoteConnectionManager'] and EventID=1057 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

**Logboeknaam:** System <br />
**Bron:** Microsoft-Windows-TerminalServices-RemoteConnectionManager <br />
**Datum:***tijd* <br />
**Gebeurtenis-ID:** 1058 <br />
**Taakcategorie:** None <br />
**Niveau:** fout <br />
**Trefwoorden:** klassieke <br />
**Gebruiker:** N.V.T. <br />
**Computer:***computer* <br />
**Beschrijving:** de RD Session Host-Server om te verlopen zelfondertekend certificaat gebruikt voor verificatie van de RD Session Host-Server op SSL-verbindingen vervangen is mislukt. De relevante statuscode is dat de toegang is geweigerd.

**Logboeknaam:** System <br />
**Bron:** Microsoft-Windows-TerminalServices-RemoteConnectionManager <br />
**Datum:***tijd* <br />
**Gebeurtenis-ID:** 1058 <br />
**Taakcategorie:** None <br />
**Niveau:** fout <br />
**Trefwoorden:** klassieke <br />
**Gebruiker:** N.V.T. <br />
**Computer:***computer* <br />
**Beschrijving:** RD Session host-server te maken van een nieuw zelfondertekend certificaat moet worden gebruikt voor RD Session host-serververificatie op SSL-verbindingen is mislukt, de relevante statuscode is het object bestaat al.

**Logboeknaam:** System <br />
**Bron:** Microsoft-Windows-TerminalServices-RemoteConnectionManager <br />
**Datum:***tijd* <br />
**Gebeurtenis-ID:** 1057 <br />
**Taakcategorie:** None <br />
**Niveau:** fout <br />
**Trefwoorden:** klassieke <br />
**Gebruiker:** N.V.T. <br />
**Computer:***computer* <br />
**Beschrijving:** de RD Session Host-Server is mislukt voor het maken een nieuw zelfondertekend certificaat moet worden gebruikt voor verificatie van de RD Session Host-Server op SSL-verbindingen. De relevante statuscode is dat sleutelpaar bestaat niet

U kunt ook controleren voor SCHANNEL foutgebeurtenissen 36872 en 36870 door het uitvoeren van de volgende opdrachten:

```cmd
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Schannel'] and EventID=36870 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Schannel'] and EventID=36872 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

**Logboeknaam:** System <br />
**Bron:** Schannel <br />
**Datum:** : <br />
**Gebeurtenis-ID:** 36870 <br />
**Taakcategorie:** None <br />
**Niveau:** fout <br />
**Trefwoorden:**       <br />
**Gebruiker:** SYSTEM <br />
**Computer:***computer* <br />
**Beschrijving:** er is een onherstelbare fout opgetreden bij het toegang tot de persoonlijke sleutel voor SSL server referentie. De cryptografische module geretourneerde foutcode is 0x8009030D.  <br />
De status van de interne fout is 10001.

### <a name="cause"></a>Oorzaak
Dit probleem treedt op omdat de lokale RSA coderingssleutels opgeslagen in de map MachineKeys op de virtuele machine kunnen niet worden geopend. Dit probleem kan optreden voor een van de volgende redenen:

1. De configuratie van de verkeerde machtigingen op de map Machinekeys of de RSA-bestanden.

2. RSA-sleutel, beschadigd of ontbreekt.

### <a name="resolution"></a>Oplossing

Om dit probleem wilt oplossen, moet u de juiste machtigingen voor het RDP-certificaat instellen met behulp van deze stappen.

#### <a name="grant-permission-to-the-machinekeys-folder"></a>Machtiging verlenen aan de map MachineKeys

1. Maken van een script met behulp van de volgende inhoud:

   ```powershell
   remove-module psreadline 
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\BeforeScript_permissions.txt
   takeown /f "C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys" /a /r
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\System:(F)"
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\NETWORK SERVICE:(R)"
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\AfterScript_permissions.txt
   Restart-Service TermService -Force
   ```

2.  Voer dit script opnieuw instellen van de machtigingen van de MachineKey-map en in te stellen de RSA-bestanden op de standaardwaarden.

3.  Probeer opnieuw toegang krijgen tot de virtuele machine.

Nadat het script is uitgevoerd, kunt u de volgende bestanden die u problemen met machtigingen ondervindt controleren:

* c:\temp\BeforeScript_permissions.txt
* c:\temp\AfterScript_permissions.txt

#### <a name="renew-rdp-self-signed-certificate"></a>RDP zelf-ondertekend certificaat vernieuwen

Als het probleem zich blijft voordoen, moet u het volgende script om ervoor te zorgen dat het RDP-zelf-ondertekend certificaat wordt verlengd uitvoeren:

```powershell
Import-Module PKI
Set-Location Cert:\LocalMachine
$RdpCertThumbprint = 'Cert:\LocalMachine\Remote Desktop\'+((Get-ChildItem -Path 'Cert:\LocalMachine\Remote Desktop\').thumbprint)
Remove-Item -Path $RdpCertThumbprint
Stop-Service -Name "SessionEnv"
Start-Service -Name "SessionEnv"
```

Als u het certificaat kan niet worden vernieuwd, volg deze stappen om te proberen om het certificaat te verwijderen:

1. Open op een andere virtuele machine in hetzelfde VNET, de **uitvoeren** in het vak **mmc**, en druk vervolgens op **OK**. 

2. Op de **bestand** in het menu **module toevoegen/verwijderen**.

3. In de **beschikbare modules** in de lijst met **certificaten**, en selecteer vervolgens **toevoegen**.

4. Selecteer **computeraccount**, en selecteer vervolgens **volgende**.

5. Selecteer **een andere computer**, en voeg het IP-adres van de virtuele machine die problemen heeft.
   >[!Note]
   >Probeer het interne netwerk gebruiken om te voorkomen dat met behulp van een virtueel IP-adres.

6. Selecteer **voltooien**, en selecteer vervolgens **OK**.

   ![Selecteer Computer](./media/event-id-troubleshoot-vm-rdp-connecton/select-computer.png)

7. Vouw de certificaten, gaat u naar de map Externe Desktop\Certificates, met de rechtermuisknop op het certificaat en selecteer vervolgens **verwijderen**.

8. De configuratie voor extern bureaublad-service opnieuw starten:

   ```cmd
   net stop SessionEnv
   net start SessionEnv
   ```

   >[!Note]
   >Op dit moment als u de store van mmc vernieuwt, het certificaat wordt opnieuw weergegeven. 

Probeert te krijgen tot de virtuele machine met behulp van RDP opnieuw.

#### <a name="update-secure-sockets-layer-ssl-certificate"></a>Secure Sockets Layer (SSL)-certificaat bijwerken

Als u de virtuele machine ingesteld om te gebruiken een SSL-certificaat, voer de volgende opdracht om op te halen van de vingerafdruk. Controleer of het is hetzelfde als de vingerafdruk van het certificaat:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SSLCertificateSHA1Hash
```

Als dit niet is, wijzigt u de vingerafdruk van het:

```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SSLCertificateSHA1Hash /t REG_BINARY /d <CERTIFICATE THUMBPRINT>
```

U kunt ook proberen de sleutel verwijderen zodat het RDP-protocol het zelfondertekende certificaat voor RDP gebruikt:

```cmd
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SSLCertificateSHA1Hash
```

## <a name="scenario-2"></a>Scenario 2

### <a name="event-log"></a>Gebeurtenislogboek

Voer de volgende opdrachten om te controleren of SCHANNEL is een foutgebeurtenis 36871 wordt geregistreerd in het systeemlogboek in de afgelopen 24 uur in een CMD-exemplaar:

```cmd
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Schannel'] and EventID=36871 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

**Logboeknaam:** System <br />
**Bron:** Schannel <br />
**Datum:** : <br />
**Gebeurtenis-ID:** 36871 <br />
**Taakcategorie:** None <br />
**Niveau:** fout <br />
**Trefwoorden:**       <br />
**Gebruiker:** SYSTEM <br />
**Computer:***computer* <br />
**Beschrijving:** er is een onherstelbare fout opgetreden tijdens het maken van een referentie TLS-server. De status van de interne fout is 10013.
 
### <a name="cause"></a>Oorzaak

Dit probleem wordt veroorzaakt door het beveiligingsbeleid. Bij oudere versies van TLS (zoals 1.0) zijn uitgeschakeld, mislukt voor RDP-toegang.

### <a name="resolution"></a>Oplossing

RDP maakt gebruik van TLS 1.0 als het standaard-protocol. Het protocol kan echter worden gewijzigd naar TLS 1.1, dit is de nieuwe standaard.

Om dit probleem wilt oplossen, Zie [verificatiefouten oplossen wanneer u verbinding maken met virtuele Azure-machine met RDP](troubleshoot-authentication-error-rdp-vm.md#tls-version).

## <a name="scenario-3"></a>Scenario 3

Als u hebt geïnstalleerd het **Remote Desktop Connection Broker** rol op de virtuele machine, Controleer of er gebeurtenis 2056 of 1296 binnen de afgelopen 24 uur. Voer de volgende opdrachten in een CMD-exemplaar: 

```cmd
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name=' Microsoft-Windows-TerminalServices-SessionBroker '] and EventID=2056 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name=' Microsoft-Windows-TerminalServices-SessionBroker-Client '] and EventID=1296 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

**Logboeknaam:** Microsoft-Windows-TerminalServices-SessionBroker/operationeel <br />
**Bron:** Microsoft-Windows-TerminalServices-SessionBroker <br />
**Datum:***tijd* <br />
**Gebeurtenis-ID:** 2056 <br />
**Taakcategorie:** (109) <br />
**Niveau:** fout <br />
**Trefwoorden:**       <br />
**Gebruiker:** netwerkservice <br />
**Computer:***computer FQDN-naam* <br />
**Beschrijving:** de beschrijving van gebeurtenis-ID 2056 van Microsoft-Windows-TerminalServices-SessionBroker bron kan niet worden gevonden. Het onderdeel dat deze gebeurtenis is niet geïnstalleerd op uw lokale computer of de installatie is beschadigd. U kunt installeren of herstellen van het onderdeel op de lokale computer. <br />
Als de gebeurtenis is opgetreden op een andere computer, wordt de weergave-informatie moest worden opgeslagen met de gebeurtenis. <br />
De volgende informatie is opgenomen in de gebeurtenis: <br />
NULL <br />
NULL <br />
Meld u aan de database is mislukt.

**Logboeknaam:** Microsoft-Windows-TerminalServices-SessionBroker-Client/operationeel <br />
**Bron:** Microsoft-Windows-TerminalServices-SessionBroker-Client <br />
**Datum:***tijd* <br />
**Gebeurtenis-ID:** 1296 <br />
**Taakcategorie:** (104) <br />
**Niveau:** fout <br />
**Trefwoorden:**       <br />
**Gebruiker:** netwerkservice <br />
**Computer:***computer FQDN-naam* <br />
**Beschrijving:** de beschrijving van gebeurtenis-ID 1296 van bron Microsoft-Windows-TerminalServices-SessionBroker-Client kan niet worden gevonden. Het onderdeel dat deze gebeurtenis is niet geïnstalleerd op uw lokale computer of de installatie is beschadigd. U kunt installeren of herstellen van het onderdeel op de lokale computer.
Als de gebeurtenis is opgetreden op een andere computer, wordt de weergave-informatie moest worden opgeslagen met de gebeurtenis.
De volgende informatie is opgenomen in de gebeurtenis:  <br />
*Tekst* <br />
*Tekst* <br />
Extern bureaublad Connection Broker is niet gereed voor RPC-communicatie.

### <a name="cause"></a>Oorzaak

Dit probleem treedt op omdat de hostnaam van de extern bureaublad Connection Broker-server wordt gewijzigd, dit is geen ondersteunde wijziging. 

De hostnaam heeft vermeldingen en afhankelijkheden voor de Windows Internal Database, die nodig is voor extern bureaublad-services-farm om te kunnen werken. De hostnaam te wijzigen nadat de farm is al ingebouwd veroorzaakt veel fouten en kan leiden tot de broker-server niet meer werken.

### <a name="resolution"></a>Oplossing 

U kunt dit probleem oplossen, de Remote Desktop Connection Broker-functie en de Windows Internal Database opnieuw moet worden geïnstalleerd.

## <a name="next-steps"></a>Volgende stappen

[Schannel-gebeurtenissen](https://technet.microsoft.com/library/dn786445(v=ws.11).aspx)

[Technisch overzicht van Schannel-SSP](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx)

[RDP is mislukt met de gebeurtenis-ID 1058 & gebeurtenis 36870 met extern bureaublad-sessiehost Hostcertificaat & SSL-communicatie](https://blogs.technet.microsoft.com/askperf/2014/10/22/rdp-fails-with-event-id-1058-event-36870-with-remote-desktop-session-host-certificate-ssl-communication/)

[Schannel 36872 of Schannel 36870 op een domeincontroller](https://blogs.technet.microsoft.com/instan/2009/01/05/schannel-36872-or-schannel-36870-on-a-domain-controller/)

[Gebeurtenis-ID 1058: Extern bureaublad-Services verificatie en versleuteling](https://technet.microsoft.com/library/ee890862(v=ws.10).aspx)

