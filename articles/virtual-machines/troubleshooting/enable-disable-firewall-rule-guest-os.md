---
title: In- of uitschakelen van een firewallregel in een gast-OS in Azure VM | Microsoft Docs
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: willchen
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 11/22/2018
ms.author: delhan
ms.openlocfilehash: cb2c548a94a91fe9126f684e382e9626adb93dd6
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52319461"
---
# <a name="enable-or-disable-a-firewall-rule-on-a-azure-vm-guest-os"></a>In- of uitschakelen van een firewallregel in een Azure VM-Gastbesturingssysteem

Dit artikel bevat een verwijzing voor het oplossen van een situatie waarin u vermoedt dat de firewall van de Gast-besturingssysteem op een virtuele machine (VM) deel van het verkeer wordt gefilterd. Dit kan handig zijn om de volgende redenen:

*   Als een wijziging opzettelijk aan de firewall waardoor RDP-verbindingen aangebracht is mislukken, kan met de functie voor Custom Script Extension het probleem oplossen.

*   Uitschakelen van alle firewallprofielen is een meer betrouwbare manier dan de RDP-specifieke firewallregel instellen voor probleemoplossing verzameld.

## <a name="solution"></a>Oplossing

Hoe u de firewall-regels configureren, is afhankelijk van het niveau van toegang tot de virtuele machine die is vereist. De volgende voorbeelden gebruiken de RDP-regels. Dezelfde methoden kunnen echter worden toegepast op elk ander soort verkeer door die verwijst naar de juiste registersleutel.

### <a name="online-troubleshooting"></a>Online oplossen 

#### <a name="mitigation-1-custom-script-extension"></a>Risicobeperking 1: De aangepaste Scriptextensie

1.  Uw script met behulp van de volgende sjabloon maken.

    *   Een regel inschakelen:
        ```cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=yes
        ```

    *   Een regel uitschakelen:
        ```cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=no
        ```

2.  Uploaden van dit script in de Azure portal met de [Custom Script Extension](../extensions/custom-script-windows.md) functie. 

#### <a name="mitigation-2-remote-powershell"></a>Risicobeperking 2: Externe PowerShell

Als de virtuele machine online is en kan worden geopend op een andere virtuele machine in hetzelfde virtuele netwerk, kunt u de volgende oplossingen met behulp van de andere virtuele machine.

1.  Open een PowerShell-console-venster op de VM voor probleemoplossing.

2.  Voer de volgende opdrachten, waar nodig.

    *   Een regel inschakelen:
        ```powershell
        Enter-PSSession (New-PSSession -ComputerName "<HOSTNAME>" -Credential (Get-Credential) -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck)) 
        Enable-NetFirewallRule -DisplayName  "RemoteDesktop-UserMode-In-TCP"
        exit
        ```

    *   Een regel uitschakelen:
        ```powershell
        Enter-PSSession (New-PSSession -ComputerName "<HOSTNAME>" -Credential (Get-Credential) -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck)) 
        Disable-NetFirewallRule -DisplayName  "RemoteDesktop-UserMode-In-TCP"
        exit
        ```

#### <a name="mitigation-3-pstools-commands"></a>Risicobeperking 3: PSTools opdrachten

Als de virtuele machine online is en kan worden geopend op een andere virtuele machine in hetzelfde virtuele netwerk, kunt u de volgende oplossingen met behulp van de andere virtuele machine.

1.  Op de VM voor probleemoplossing, downloaden [PSTools](https://docs.microsoft.com/sysinternals/downloads/pstools).

2.  Open een CMD-exemplaar en toegang tot de virtuele machine via het interne IP (DIP). 

    * Een regel inschakelen:
        ```cmd
        psexec \\<DIP> -u <username> cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=yes
        ```

    *   Een regel uitschakelen:
        ```cmd
        psexec \\<DIP> -u <username> cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=no
        ```

#### <a name="mitigation-4-remote-registry"></a>Risicobeperking 4: De Remote Registry

Als de virtuele machine online is en kan worden geopend op een andere virtuele machine in hetzelfde virtuele netwerk, kunt u [Remote Registry](https://support.microsoft.com/help/314837/how-to-manage-remote-access-to-the-registry) op de andere virtuele machine.

1.  Op de VM voor probleemoplossing, start de Register-Editor (regedit.exe) en selecteer vervolgens **bestand** > **verbinding netwerkregister**.

2.  Open de *DOELMACHINE*\SYSTEM vertakking en geef vervolgens de volgende waarden:

    * Als u wilt inschakelen op een regel, opent u de volgende registerwaarde:
    
        *DOELCOMPUTER*\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-UserMode-In-TCP
    
        Wijzig **Active = FALSE** naar **Active = TRUE** in de tekenreeks:

        **v2.22 | Actie = toestaan | Actieve = TRUE | Dir = In | Protocol = 6 | Profiel = domein | Profiel persoonlijke = | Profiel = openbare | LPort = 3389 | App=%systemroot%\system32\svchost.exe| SVC Terminal Server = | Naam =@FirewallAPI.dll,-28775 | Desc =@FirewallAPI.dll,-28756 | EmbedCtxt =@FirewallAPI.dll,-28752 |**
    
    * Als u wilt een regel uitschakelt, opent u de volgende registerwaarde:
    
        *DOELCOMPUTER*\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-UserMode-In-TCP

        Wijzig **Active = TRUE** naar **Active = FALSE**:
        
        **v2.22 | Actie = toestaan | Actieve = FALSE | Dir = In | Protocol = 6 | Profiel = domein | Profiel persoonlijke = | Profiel = openbare | LPort = 3389 | App=%systemroot%\system32\svchost.exe| SVC Terminal Server = | Naam =@FirewallAPI.dll,-28775 | Desc =@FirewallAPI.dll,-28756 | EmbedCtxt =@FirewallAPI.dll,-28752 |**

3.  Start opnieuw op de virtuele machine om de wijzigingen te laten.

### <a name="offline-troubleshooting"></a>Het oplossen van offline 

Als u geen toegang de virtuele machine door een methode tot, met behulp van de aangepaste Scriptextensie mislukt en u moet werken in de OFFLINEMODUS bevindt door rechtstreeks uitvoeren van de schijf.

Voordat u deze stappen hebt uitgevoerd, maakt u een momentopname van de schijf van de betrokken virtuele machine als een back-up. Zie voor meer informatie, [momentopname maken van een schijf](../windows/snapshot-copy-managed-disk.md).

1.  [De schijf koppelen aan een virtuele machine voor herstel](troubleshoot-recovery-disks-portal-windows.md).

2.  Start een externe bureaubladverbinding met de virtuele machine voor herstel.

3.  Zorg ervoor dat de schijf is gemarkeerd als **Online** in de Schijfbeheer-console. Houd er rekening mee dat de stationsletter die is toegewezen aan de gekoppelde schijf.

4.  Voordat u wijzigingen aanbrengt, maakt u een kopie van de map \windows\system32\config mocht terugdraaien van de wijzigingen nodig zijn.

5.  Start de Register-Editor (regedit.exe) op de VM voor probleemoplossing.

6.  Markeer de **HKEY_LOCAL_MACHINE** sleutel, en selecteer vervolgens **bestand** > **Component laden** in het menu.

    ![Regedit](./media/enable-or-disable-firewall-rule-guest-os/load-registry-hive.png)

7.  Zoek en open vervolgens het bestand \windows\system32\config\SYSTEM. 

    > [!Note]
    > U wordt gevraagd een naam. Voer **BROKENSYSTEM**, en vouw vervolgens **HKEY_LOCAL_MACHINE**. U ziet nu een extra sleutel met de naam **BROKENSYSTEM**. Voor deze problemen oplossen, zijn we deze componenten probleem als koppelen **BROKENSYSTEM**.

8.  De volgende wijzigingen aanbrengen op de vertakking BROKENSYSTEM:

    1.  Controleren welke **ControlSet** registersleutel die de virtuele machine is uitgevoerd. Hier ziet u het belangrijkste nummer in hklm\brokensystem\select\current om.

    2.  Als u wilt inschakelen op een regel, opent u de volgende registerwaarde:
    
        HKLM\BROKENSYSTEM\ControlSet00X\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop gebruikersmodus In TCP
        
        Wijzig **Active = FALSE** naar **Active = True**.
        
        **v2.22 | Actie = toestaan | Actieve = TRUE | Dir = In | Protocol = 6 | Profiel = domein | Profiel persoonlijke = | Profiel = openbare | LPort = 3389 | App=%systemroot%\system32\svchost.exe| SVC Terminal Server = | Naam =@FirewallAPI.dll,-28775 | Desc =@FirewallAPI.dll,-28756 | EmbedCtxt =@FirewallAPI.dll,-28752 |**

    3.  Als u wilt een regel uitschakelt, opent u de volgende registersleutel:

        HKLM\BROKENSYSTEM\ControlSet00X\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop gebruikersmodus In TCP

        Wijzig **Active = True** naar **Active = FALSE**.
        
        **v2.22 | Actie = toestaan | Actieve = FALSE | Dir = In | Protocol = 6 | Profiel = domein | Profiel persoonlijke = | Profiel = openbare | LPort = 3389 | App=%systemroot%\system32\svchost.exe| SVC Terminal Server = | Naam =@FirewallAPI.dll,-28775 | Desc =@FirewallAPI.dll,-28756 | EmbedCtxt =@FirewallAPI.dll,-28752 |**

9.  Markeer **BROKENSYSTEM**, en selecteer vervolgens **bestand** > **component** in het menu.

10. [De schijf loskoppelen en opnieuw maken van de virtuele machine](troubleshoot-recovery-disks-portal-windows.md).

11. Controleer of het probleem opgelost is.