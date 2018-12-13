---
title: Kan geen verbinding maken naar Azure Virtual Machines op afstand omdat de virtuele machine wordt opgestart in de veilige modus | Microsoft Docs
description: Meer informatie over het oplossen van problemen waarbij niet RDP-verbinding met een virtuele machine omdat de virtuele machine wordt opgestart in de veilige modus. | Microsoft Docs
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
ms.date: 11/13/2018
ms.author: genli
ms.openlocfilehash: 0ef4aa988f4adc855051b213013636b4a04f1cca
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316968"
---
#  <a name="cannot-rdp-to-a-vm-because-the-vm-boots-into-safe-mode"></a>Niet van RDP-verbinding met een virtuele machine omdat de virtuele machine wordt opgestart in de veilige modus

In dit artikel laat zien hoe het oplossen van een probleem waarbij u geen verbinding naar Azure Windows Virtual Machines (VM's maken) omdat de virtuele machine is geconfigureerd om op te starten in de veilige modus.

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over het Resource Manager-implementatiemodel, dat wordt u aangeraden voor nieuwe implementaties in plaats van het klassieke implementatiemodel.

## <a name="symptoms"></a>Symptomen

Kunt u niet een RDP-verbinding of andere verbindingen (zoals HTTP) aan een virtuele machine in Azure omdat de virtuele machine is geconfigureerd om op te starten in de veilige modus. Wanneer u de schermafbeelding controleren in de [diagnostische gegevens over opstarten](../troubleshooting/boot-diagnostics.md) in Azure portal, ziet u mogelijk dat de virtuele machine normaal worden opgestart, maar de netwerkinterface niet beschikbaar is:

![Afbeelding van netwerk inferce in de veilige modus](./media/troubleshoot-rdp-safe-mode/network-safe-mode.png)

## <a name="cause"></a>Oorzaak

De RDP-service is niet beschikbaar in de veilige modus. Alleen zijn essentiële systeem programma's en services geladen wanneer de virtuele machine wordt opgestart in de veilige modus. Dit geldt voor de twee verschillende versies van de veilige modus zijn "Opstarten in veilige modus minimale" en "Veilig opstarten met de connectiviteit".


## <a name="solution"></a>Oplossing

Voordat u deze stappen hebt uitgevoerd, maakt u een momentopname van de besturingssysteemschijf van de betrokken virtuele machine als een back-up. Zie voor meer informatie, [momentopname maken van een schijf](../windows/snapshot-copy-managed-disk.md).

U lost dit probleem, kunt u seriële besturingselement gebruiken om te configureren van de virtuele machine te starten in de normale modus of [herstel de virtuele machine offline](#repair-the-vm-offline) met behulp van een virtuele machine voor herstel.

### <a name="use-serial-control"></a>Seriële besturingselement gebruiken

1. Verbinding maken met [seriële Console- en CMD-instantie openen](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). Als de seriële Console is niet ingeschakeld op de virtuele machine, Zie [herstel de virtuele machine offline](#repair-the-vm-offline).
2. Controleer de opstartconfiguratiegegevens:

        bcdedit /enum

    Als de virtuele machine is geconfigureerd om op te starten in de veilige modus, ziet u een extra vlag onder de **Windows-opstartlaadprogramma** sectie met de naam **veilig**. Als u niet ziet de **veilig** vlag, de virtuele machine is niet in de veilige modus. In dit artikel geldt niet voor uw scenario.

    De **veilig** vlag kan worden weergegeven met de volgende waarden:
    - Minimaal
    - Netwerk

    RDP wordt in een van deze twee modi, niet worden gestart. De oplossing blijft daarom ongewijzigd.

    ![Afbeelding van de markering voor de veilige modus](./media/troubleshoot-rdp-safe-mode/safe-mode-tag.png)

3. Verwijder de **safemoade** markeren, zodat de virtuele machine opnieuw in de normale modus opgestart:

        bcdedit /deletevalue {current} safeboot

4. Controleer de opstartconfiguratiegegevens om ervoor te zorgen dat de **veilig** markering wordt verwijderd:

        bcdedit /enum

5. Start de VM opnieuw en controleer vervolgens of het probleem opgelost is.

### <a name="repair-the-vm-offline"></a>Herstel de virtuele machine offline

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>De besturingssysteemschijf koppelen aan een virtuele machine voor herstel

1. [De besturingssysteemschijf koppelen aan een virtuele machine voor herstel](../windows/troubleshoot-recovery-disks-portal.md).
2. Start een externe bureaubladverbinding met de virtuele machine voor herstel.
3. Zorg ervoor dat de schijf is gemarkeerd als **Online** in de Schijfbeheer-console. Houd er rekening mee de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf.

#### <a name="enable-dump-log-and-serial-console-optional"></a>Inschakelen van de dump logboek- en seriële Console (optioneel)

De dump logboek- en seriële Console kunnen we doen problemen op te lossen als het probleem niet kan worden omgezet door de oplossing in dit artikel.

Om in te schakelen dump logboek- en seriële Console, voer het volgende script.

1. Open een opdrachtprompt met verhoogde bevoegdheid-sessie (**als administrator uitvoeren**).
2. Voer het volgende script uit:

    In dit script, we gaan ervan uit dat de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf F. vervangen deze stationsletter door de juiste waarde voor uw virtuele machine.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

#### <a name="configure-the-windows-to-boot-into-normal-mode"></a>Configureren van de Windows om op te starten in de normale modus

1. Open een opdrachtprompt met verhoogde bevoegdheid-sessie (**als administrator uitvoeren**).
2. Controleer de opstartconfiguratiegegevens. In de volgende opdrachten, we gaan ervan uit dat de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf F. vervangen deze stationsletter door de juiste waarde voor uw virtuele machine.

        bcdedit /store F:\boot\bcd /enum
    Noteer de id-naam van de partitie met de **\windows** map. Standaard is de id-naam 'Standaard'.

    Als de virtuele machine is geconfigureerd om op te starten in de veilige modus, ziet u een extra vlag onder de **Windows-opstartlaadprogramma** sectie met de naam **veilig**. Als u niet ziet de **veilig** vlag, in dit artikel is niet van toepassing op uw scenario.

    ![De afbeelding over opstarten id](./media/troubleshoot-rdp-safe-mode/boot-id.png)

3. Verwijder de **veilig** markeren, zodat de virtuele machine opnieuw in de normale modus opgestart:

        bcdedit /store F:\boot\bcd /deletevalue {Default} safeboot
4. Controleer de opstartconfiguratiegegevens om ervoor te zorgen dat de **veilig** markering wordt verwijderd:

        bcdedit /store F:\boot\bcd /enum
5. [De OS-schijf loskoppelen en opnieuw maken van de virtuele machine](../windows/troubleshoot-recovery-disks-portal.md). Controleer vervolgens of het probleem is opgelost.
