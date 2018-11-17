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
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 25e70b3a685f3b777a74c4cc6bf0e56dd37741a7
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51821511"
---
# <a name="troubleshoot-an-rdp-general-error-in-azure-vm"></a>Een algemene fout RDP in Azure VM oplossen

Dit artikel beschrijft een algemene fout die optreden kan bij het maken van een Remote Desktop Protocol (RDP)-verbinding naar een Windows virtuele Machine (VM) in Azure.

## <a name="symptom"></a>Symptoom

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

Dit probleem op te lossen [maakt u een back-up van de besturingssysteemschijf](../windows/snapshot-copy-managed-disk.md), en [de besturingssysteemschijf koppelen aan een redden VM](troubleshoot-recovery-disks-portal-windows.md), en volg de stappen.

### <a name="serial-console"></a>Seriële Console

#### <a name="step-1-open-cmd-instance-in-serial-console"></a>Stap 1: Open CMD-exemplaar in de seriële console

1. Toegang tot de [seriële Console](serial-console-windows.md) hiervoor **ondersteuning en probleemoplossing** > **seriële console (Preview)**. Als de functie is ingeschakeld op de virtuele machine, kunt u de virtuele machine is verbinden.

2. Maak een nieuw kanaal voor een CMD-exemplaar. Type **CMD** starten van het kanaal om op te halen van de naam van het kanaal.

3. Schakel over naar het kanaal dat het exemplaar CMD uitgevoerd, in dit geval moet het kanaal 1.

   ```
   ch -si 1
   ```

#### <a name="step-2-check-the-values-of-rdp-registry-keys"></a>Stap 2: Controleer de waarden van registersleutels voor RDP:

1. Controleer of het RDP-protocol is uitgeschakeld door beleid.

      ```
      REM Get the local policy 
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server " /v fDenyTSConnections

      REM Get the domain policy if any
      reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections
      ```

      - Als het domeinbeleid bestaat, wordt de installatie op het lokale beleid overschreven.
      - Als het domeinbeleid dat RDP is uitgeschakeld (1) en vervolgens de AD-updatebeleid van de domeincontroller wordt vermeld.
      - Als het domeinbeleid dat RDP is ingeschakeld (0), is er geen update vereist.
      - Als het domeinbeleid bestaat niet en het lokale beleid wordt bepaald dat RDP is uitgeschakeld (1), schakelt u RDP met behulp van de volgende opdracht uit: 
      
            reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
                  

2. Controleer de huidige configuratie van de terminal server.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled
      ```

      Als de opdracht 0 retourneert, wordt de terminal server uitgeschakeld. Schakel de terminal server vervolgens als volgt in:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f
      ```

3. De Terminal Server-module is ingesteld op modus leegmaken als de server zich op een terminal server-farm (extern bureaublad-services of Citrix). Controleer de huidige modus van de Terminal Server-module.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode
      ```

      Als de opdracht 1 retourneert, wordt de Terminal Server-module verwijderen uit de modus ingesteld. Stel vervolgens de module naar de modus werkt als volgt:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f
      ```

4. Controleer of u verbinding met de terminal server maken kunt.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled
      ```

      Als de opdracht 1 retourneert, kunt u geen verbinding maken met de terminal server. Schakel vervolgens de verbinding als volgt in:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f
      ```
5. Controleer de huidige configuratie van de RDP-listener.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation
      ```

      Als de opdracht 0 retourneert, wordt de RDP-listener is uitgeschakeld. Schakel de listener vervolgens als volgt in:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f
      ```

6. Controleer of u verbinding met de RDP-listener maken kunt.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled
      ```

   Als de opdracht 1 retourneert, kunt u geen verbinding maken met de RDP-listener. Schakel vervolgens de verbinding als volgt in:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f
      ```

7. Start de VM opnieuw.

8. Afsluiten van het exemplaar CMD door te typen `exit`, en druk vervolgens op **Enter** twee keer.

9. Start de VM opnieuw door te typen `restart`, en maak verbinding met de virtuele machine.

Als het probleem zich nog steeds voordoet, verplaatst naar de stap 2.

#### <a name="step-2-enable-remote-desktop-services"></a>Stap 2: Extern bureaublad-services inschakelen

Zie voor meer informatie, [extern bureaublad-Services is niet gestart op een Azure VM](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>Stap 3: Opnieuw instellen RDP-listener

Zie voor meer informatie, [extern bureaublad verbinding verbreekt vaak in Azure VM](troubleshoot-rdp-intermittent-connectivity.md).

### <a name="offline-repair"></a>Offline herstellen

#### <a name="step-1-turn-on-remote-desktop"></a>Stap 1: Extern bureaublad inschakelen

1. [De besturingssysteemschijf koppelen aan een virtuele machine voor herstel](../windows/troubleshoot-recovery-disks-portal.md).
2. Start een externe bureaubladverbinding met de virtuele machine voor herstel.
3. Zorg ervoor dat de schijf is gemarkeerd als **Online** in de Schijfbeheer-console. Houd er rekening mee de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf.
4. Start een externe bureaubladverbinding met de virtuele machine voor herstel.
5. Open een opdrachtprompt met verhoogde bevoegdheid-sessie (**als administrator uitvoeren**). Voer de volgende scripts. In dit script, we gaan ervan uit dat de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf F. vervangen deze stationsletter door de juiste waarde voor uw virtuele machine.

      ```
      reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv 
      reg load HKLM\BROKENSOFTWARE F:\windows\system32\config\SOFTWARE.hiv 
 
      REM Ensure that Terminal Server is enabled 

      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f 

      REM Ensure Terminal Service is not set to Drain mode 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f 

      REM Ensure Terminal Service has logon enabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f 

      REM Ensure the RDP Listener is not disabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f 

      REM Ensure the RDP Listener accepts logons 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f 

      REM RDP component is enabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0 /f 

      reg unload HKLM\BROKENSYSTEM 
      reg unload HKLM\BROKENSOFTWARE 
      ```

6. Als de virtuele machine toegevoegd aan een domein is, controleert u de volgende registersleutel om te zien of er een Groepsbeleid die RDP wordt uitgeschakeld. 

      ```
      HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services\fDenyTSConnectionS
      ```

      Als de waarde van deze sleutel is ingesteld op 1 betekent dit dat RDP is uitgeschakeld door het beleid. Om extern bureaublad inschakelen via de GPO-beleid, wijzigt u het volgende beleid van de domeincontroller:

   
      **Computer Configuration\Policies\Administrative sjablonen:**

      Beleid definitions\Windows onderdelen\Extern Desktop Services\Remote bureaublad-sessie Host\Connections\Allow gebruikers op afstand verbinding kunnen maken met behulp van extern bureaublad-Services
  
7. De schijf loskoppelen van de redden VM.
8. [Maak een nieuwe virtuele machine van de schijf](../windows/create-vm-specialized.md).

Als het probleem zich nog steeds voordoet, verplaatst naar de stap 2.

#### <a name="step-2-enable-remote-desktop-services"></a>Stap 2: Extern bureaublad-services inschakelen

Zie voor meer informatie, [extern bureaublad-Services is niet gestart op een Azure VM](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>Stap 3: Opnieuw instellen RDP-listener

Zie voor meer informatie, [extern bureaublad verbinding verbreekt vaak in Azure VM](troubleshoot-rdp-intermittent-connectivity.md).

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
