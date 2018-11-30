---
title: Uitschakelen van de Gast OS-Firewall in virtuele Azure-machine | Microsoft Docs
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
ms.openlocfilehash: b0cd20278287b41dd953c64044b705aa2dba7557
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52319405"
---
# <a name="disable-the-guest-os-firewall-in-azure-vm"></a>De Gast OS-Firewall in virtuele Azure-machine uitschakelen

Dit artikel bevat een verwijzing voor situaties waarin u vermoedt dat de firewall van de Gast-besturingssysteem van gedeeltelijke of volledige verkeer naar een virtuele machine (VM filteren). Dit kan gebeuren als de wijzigingen opzettelijk zijn aangebracht in de firewall waardoor RDP-verbindingen mislukken.

## <a name="solution"></a>Oplossing

Het proces dat wordt beschreven in dit artikel is bedoeld om te worden gebruikt als tijdelijke oplossing, zodat u zich richten kunt op het verhelpen van uw werkelijke probleem, die over het instellen van de firewallregels correct is. It\rquote s, een Microsoft-aanbevolen om het onderdeel Windows Firewall is ingeschakeld. Hoe u de firewall-regels \cf3 configureert, is afhankelijk van het niveau van toegang tot de virtuele machine that\rquote (s) vereist.

### <a name="online-solutions"></a>Online oplossingen 

Als de virtuele machine online is en kan worden geopend op een andere virtuele machine in hetzelfde virtuele netwerk, kunt u deze oplossingen met behulp van de andere virtuele machine.

#### <a name="mitigation-1-custom-script-extension-or-run-command-feature"></a>Risicobeperking 1: Functie voor Custom Script Extension of opdracht uitvoeren

Als u een werkende Azure-agent hebt, kunt u [Custom Script Extension](../extensions/custom-script-windows.md) of de [opdrachten uitvoeren](../windows/run-command.md) functie (alleen Resource Manager-VM's) voor het extern uitvoeren van de volgende scripts.

> [!Note]
> * Als de firewall lokaal is ingesteld, voert u het volgende script uit:
>   ```
>   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile' -name "EnableFirewall" -Value 0
>   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile' -name "EnableFirewall" -Value 0
>   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\Standardprofile' -name "EnableFirewall" -Value 0 
>   Restart-Service -Name mpssvc
>   ```
> * Als de firewall door middel van een Active Directory-beleid is ingesteld, kunt u het volgende script voor tijdelijke toegang worden uitgevoerd. 
>   ```
>   Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\DomainProfile' -name "EnableFirewall" -Value 0
>   Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\PublicProfile' -name "EnableFirewall" -Value 0
>   Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\StandardProfile' name "EnableFirewall" -Value 0
>   Restart-Service -Name mpssvc
>   ```
>   Echter, zodra het beleid opnieuw wordt toegepast, moet u worden gestart af bij de externe sessie. De permanente oplossing voor dit probleem is het wijzigen van het beleid dat wordt toegepast op deze computer.

#### <a name="mitigation-2-remote-powershell"></a>Risicobeperking 2: Externe PowerShell

1.  Verbinding maken met een virtuele machine die zich in hetzelfde virtuele netwerk bevinden als de virtuele machine die u met behulp van RDP-verbinding niet kan bereiken.

2.  Open een PowerShell-consolevenster.

3.  Voer de volgende opdrachten uit:

    ```powershell
    Enter-PSSession (New-PSSession -ComputerName "<HOSTNAME>" -Credential (Get-Credential) -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck)) 
    netsh advfirewall set allprofiles state off
    Restart-Service -Name mpssvc 
    exit
    ```

> [!Note]
> Als de firewall is ingesteld via een Group Policy Object, werken deze methode niet omdat deze opdracht alleen de vermeldingen in het lokale register wijzigt. Als een beleid ingesteld is, overschrijft deze wijziging. 

#### <a name="mitigation-3-pstools-commands"></a>Risicobeperking 3: PSTools opdrachten

1.  Op de VM voor probleemoplossing, downloaden [PSTools](https://docs.microsoft.com/sysinternals/downloads/pstools).

2.  Open een CMD-exemplaar en vervolgens toegang tot de virtuele machine via de DIP.

3.  Voer de volgende opdrachten uit:

    ```cmd
    psexec \\<DIP> -u <username> cmd
    netsh advfirewall set allprofiles state off
    psservice restart mpssvc
    ```

#### <a name="mitigation-4-remote-registry"></a>Risicobeperking 4: De Remote Registry 

Volg deze stappen om het gebruik van [Remote Registry](https://support.microsoft.com/help/314837/how-to-manage-remote-access-to-the-registry).

1.  Op de VM voor probleemoplossing, start de Registereditor en ga vervolgens naar **bestand** > **verbinding netwerkregister**.

2.  Open de *DOELMACHINE*\SYSTEM vertakking en geef de volgende waarden:

    ```
    <TARGET MACHINE>\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile\EnableFirewall           -->        0 
    <TARGET MACHINE>\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile\EnableFirewall           -->        0 
    <TARGET MACHINE>\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\StandardProfile\EnableFirewall         -->        0
    ```

3.  Start de service. Omdat u niet dat doen met behulp van het externe register, moet u de Service-Console verwijderen.

4.  Open een exemplaar van **Services.msc**.

5.  Klik op **Services (lokaal)**.

6.  Selecteer **verbinding maken met een andere computer**.

7.  Voer de **privé IP-adres (DIP)** van de virtuele machine van het probleem.

8.  De lokale firewall-beleid voor opnieuw opstarten.

9.  Probeer opnieuw verbinding te maken met de virtuele machine via RDP vanaf uw lokale computer.

### <a name="offline-solutions"></a>Offline oplossingen 

Hebt u een situatie waarin u de virtuele machine niet kan door een methode bereiken, mislukt de Custom Script Extension en u moet werken in de OFFLINEMODUS bevindt door rechtstreeks uitvoeren van de schijf. Voer hiervoor de volgende stappen uit:

1.  [De schijf koppelen aan een virtuele machine voor herstel](troubleshoot-recovery-disks-portal-windows.md).

2.  Start een externe bureaubladverbinding met de virtuele machine voor herstel.

3.  Zorg ervoor dat de schijf Online is gemarkeerd in de Schijfbeheer-console. Houd er rekening mee de stationsletter die toegewezen aan de gekoppelde schijf.

4.  Voordat u wijzigingen aanbrengt, maakt u een kopie van de map \windows\system32\config mocht terugdraaien van de wijzigingen nodig zijn.

5.  Start de Register-editor (regedit.exe) op de VM voor probleemoplossing. 

6.  We zijn de componenten als BROKENSYSTEM en BROKENSOFTWARE koppelen voor deze procedure voor probleemoplossing.

7.  Markeer de sleutel HKEY_LOCAL_MACHINE en selecteer Bestand > component laden in het menu.

8.  Zoek het bestand \windows\system32\config\SYSTEM van de gekoppelde schijf.

9.  Open een verhoogde PowerShell-sessie en voer de volgende opdrachten:

    ```cmd
    # Load the hives - If your attached disk is not F, replace the letter assignment here
    reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM
    reg load HKLM\BROKENSOFTWARE f:\windows\system32\config\SOFTWARE 
    # Disable the firewall on the local policy
    $ControlSet = (get-ItemProperty -Path 'HKLM:\BROKENSYSTEM\Select' -name "Current").Current
    $key = 'BROKENSYSTEM\ControlSet00'+$ControlSet+'\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    $key = 'BROKENSYSTEM\ControlSet00'+$ControlSet+'\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    $key = 'BROKENSYSTEM\ControlSet00'+$ControlSet+'\services\SharedAccess\Parameters\FirewallPolicy\StandardProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    # To ensure the firewall is not set thru AD policy, check if the following registry entries exist and if they do, then check if the following entries exist:
    $key = 'HKLM:\BROKENSOFTWARE\Policies\Microsoft\WindowsFirewall\DomainProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    $key = 'HKLM:\BROKENSOFTWARE\Policies\Microsoft\WindowsFirewall\PublicProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    $key = 'HKLM:\BROKENSOFTWARE\Policies\Microsoft\WindowsFirewall\StandardProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    # Unload the hives
    reg unload HKLM\BROKENSYSTEM
    reg unload HKLM\BROKENSOFTWARE
    ```

10. [De schijf loskoppelen en opnieuw maken van de virtuele machine](troubleshoot-recovery-disks-portal-windows.md).

11. Controleer of het probleem opgelost is.
