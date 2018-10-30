---
title: Problemen met een algemene fout RDP naar een Windows-VM in Azure | Microsoft Docs
description: Informatie over het oplossen van een algemene fout die RDP naar een Windows-VM in Azure | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 701373efa3c3c22eb5969705927e1c0cc6e3f36b
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215791"
---
# <a name="troubleshoot-an-rdp-general-error-in-azure-vm"></a>Een algemene fout RDP in Azure VM oplossen

Dit artikel beschrijft een algemene fout die optreden kan bij het maken van een Remote Desktop Protocol (RDP)-verbinding naar een Windows virtuele Machine (VM) in Azure.

## <a name="symptoms"></a>Symptomen

Wanneer u een RDP-verbinding met een venster virtuele machine in Azure maakt, wordt de volgende algemene strekking weergegeven:

**Extern bureaublad kan geen verbinding maken met de externe computer voor een van de volgende redenen:**

1. **Externe toegang tot de server is niet ingeschakeld**

2. **De externe Computer is uitgeschakeld**

3. **De externe computer is niet beschikbaar op het netwerk**

**Zorg ervoor dat de externe computer is ingeschakeld en verbonden met het netwerk, en dat externe toegang is ingeschakeld.**

## <a name="cause"></a>Oorzaak

Dit probleem kan optreden vanwege de volgende oorzaken hebben:

### <a name="cause-1"></a>Oorzaak 1

De RDP-component is als volgt uitgeschakeld:

- Op het onderdeelniveau van het
- Op het niveau van de listener
- Op de terminal server
- Op de Remote Desktop Session Host-rol

### <a name="cause-2"></a>Oorzaak 2

Extern bureaublad-Services (Terminal Server) wordt niet uitgevoerd.

### <a name="cause-3"></a>Oorzaak 3

De RDP-listener is onjuist geconfigureerd.

## <a name="solution"></a>Oplossing

Dit probleem op te lossen [maakt u een back-up van de besturingssysteemschijf](../windows/snapshot-copy-managed-disk.md), en [de besturingssysteemschijf koppelen aan een redden VM](troubleshoot-recovery-disks-portal-windows.md), en volgt u de oplossingsopties dienovereenkomstig, of probeer de oplossingen één voor één.

### <a name="serial-console"></a>Seriële Console

#### <a name="step-1-turn-on-remote-desk"></a>Stap 1: Externe helpdesk inschakelen

1. Toegang tot de [seriële Console](serial-console-windows.md) hiervoor **ondersteuning en probleemoplossing** > **seriële console (Preview)**. Als de functie is ingeschakeld op de virtuele machine, kunt u de virtuele machine is verbinden.

2. Maak een nieuw kanaal voor een CMD-exemplaar. Type **CMD** starten van het kanaal om op te halen van de naam van het kanaal.

3. Schakel over naar het kanaal dat het exemplaar CMD uitgevoerd, in dit geval moet het kanaal 1.

   ```
   ch -si 1
   ```

4. Extern bureaublad inschakelen via de GPO-beleid door het wijzigen van het volgende beleid:

   ```
   Computer Configuration\Policies\Administrative Templates: Policy definitions\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Connections\Allow users to connect remotely by using Remote Desktop Services
   ```

5. Sommige andere registersleutels die kunnen ervoor zorgen dat dit probleem, Controleer de waarden van de registersleutels als volgt:

   1. Zorg ervoor dat het RDP-onderdeel is ingeschakeld.

      ```
      REM Get the local policy
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server " /v fDenyTSConnections

      REM Get the domain policy if any
      reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections
      ```

      Als het domeinbeleid bestaat, wordt de installatie op het lokale beleid overschreven.

         - Als het domeinbeleid dat RDP uitgeschakeld (1), wordt het beleid voor AD-update is vermeld.
         - Als het domeinbeleid dat RDP is ingeschakeld (0), is er geen update vereist.

      Als het domeinbeleid bestaat niet en het lokale beleid wordt bepaald dat RDP is uitgeschakeld (1), schakelt u RDP met behulp van de volgende opdracht uit:

         ```
         reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
         ```

   2. Controleer de huidige configuratie van de terminal server.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled
      ```

   3. Als de opdracht 0 retourneert, wordt de terminal server uitgeschakeld. Schakel de terminal server vervolgens als volgt in:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f
      ```

   4. De Terminal Server-module is ingesteld op modus leegmaken als de server zich op een terminal server-farm (extern bureaublad-services of Citrix). Controleer de huidige modus van de Terminal Server-module.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode
      ```

   5. Als de opdracht 1 retourneert, wordt de Terminal Server-module verwijderen uit de modus ingesteld. Stel vervolgens de module naar de modus werkt als volgt:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f
      ```

   6. Controleer of u verbinding met de terminal server maken kunt.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled
      ```

   7. Als de opdracht 1 retourneert, kunt u geen verbinding maken met de terminal server. Schakel vervolgens de verbinding als volgt in:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f
      ```

   8. Controleer de huidige configuratie van de RDP-listener.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation
      ```

   9. Als de opdracht 0 retourneert, wordt de RDP-listener is uitgeschakeld. Schakel de listener vervolgens als volgt in:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f
      ```

   10. Controleer of u verbinding met de RDP-listener maken kunt.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled
      ```

   11. Als de opdracht 1 retourneert, kunt u geen verbinding maken met de RDP-listener. Schakel vervolgens de verbinding als volgt in:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f
      ```

6. Start de VM opnieuw.

7. Afsluiten van het exemplaar CMD door te typen `exit`, en druk vervolgens op **Enter** twee keer.

8. Start de VM opnieuw door te typen `restart`.

Als het probleem zich nog steeds voordoet, verplaatst naar de stap 2.

#### <a name="step-2-enable-remote-desktop-services"></a>Stap 2: Extern bureaublad-services inschakelen

Zie voor meer informatie, [extern bureaublad-Services is niet gestart op een Azure VM](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>Stap 3: Opnieuw instellen RDP-listener

Zie voor meer informatie, [extern bureaublad verbinding verbreekt vaak in Azure VM](troubleshoot-rdp-intermittent-connectivity.md).

### <a name="offline-repair"></a>Offline herstellen

#### <a name="step-1-turn-on-remote-desk"></a>Stap 1: Externe helpdesk inschakelen

> [!NOTE]  
> We gaan ervan uit dat de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf F. vervangen met de juiste waarde in uw virtuele machine. De systeem- en SOFTWAREPRESTATIES componenten moeten worden ontkoppeld en is gekoppeld.

1. Open een verhoogde CMD-exemplaar en de volgende scripts uitvoeren op de redden VM:

   ```
   reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM.hiv
   reg load HKLM\BROKENSOFTWARE f:\windows\system32\config\SOFTWARE.hiv

   REM Ensure that Terminal Server is enabled
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f

   REM Ensure Terminal Service is not set to Drain mode
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f

   REM Ensure Terminal Service has logon enabled
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f

   REM Ensure the RDP Listener is not disabled
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f

   REM Ensure the RDP Listener accepts logons
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f

   REM RDP component is enabled
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
   reg add "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0 /f

   reg unload HKLM\BROKENSYSTEM
   reg unload HKLM\BROKENSOFTWARE
   ```

2. Koppel de systeem- en SOFTWARE-componenten.

   ```
   reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM
   reg load HKLM\BROKENSOFTWARE f:\windows\system32\config\SOFTWARE
   ```

3. Als de virtuele machine toegevoegd aan een domein is, kan de RDP uitschakelen op het beleidsniveau van een. Als u wilt valideren als dit het geval is, controleert u de volgende registersleutel:

   ```
   HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services\fDenyTSConnectionS
   ```

4. Als de waarde van deze sleutel is ingesteld op 1, wordt RDP is uitgeschakeld door het beleid.

5. Extern bureaublad inschakelen via de GPO-beleid door het wijzigen van het volgende beleid:

   ```
   Computer Configuration\Policies\Administrative Templates: Policy definitions\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Connections\Allow users to connect remotely by using Remote Desktop Services
   ```

6. Als deze registersleutel niet bestaat, controleert u de volgende registersleutel:

   ```
   HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections
   ```

7. Als deze sleutel is ingesteld op 1, is RDP inschakelen uitgeschakeld. Wijzig de waarde van de sleutel in 0.
8. De schijf loskoppelen van de redden VM.
9. [Maak een nieuwe virtuele machine van de schijf](../windows/create-vm-specialized.md).

Als het probleem zich nog steeds voordoet, verplaatst naar de stap 2.

#### <a name="step-2-enable-remote-desktop-services"></a>Stap 2: Extern bureaublad-services inschakelen

Zie voor meer informatie, [extern bureaublad-Services is niet gestart op een Azure VM](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>Stap 3: Opnieuw instellen RDP-listener

Zie voor meer informatie, [extern bureaublad verbinding verbreekt vaak in Azure VM](troubleshoot-rdp-intermittent-connectivity.md).

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
