---
title: Blauw scherm fouten bij het opstarten van een Azure-VM | Microsoft Docs
description: Informatie over het oplossen van het probleem dat de fout blauw scherm is ontvangen bij het opstarten | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
authors: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 00/19/2018
ms.author: genli
ms.openlocfilehash: 05529b1d9397fb14e4a0eece5587023321b955b7
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586896"
---
# <a name="windows-shows-blue-screen-error-when-booting-an-azure-vm"></a>Windows wordt een blauw scherm fout weergegeven wanneer een Azure-VM wordt opgestart
Dit artikel wordt beschreven blauw scherm fouten die optreden kunnen tijdens het opstarten van een Windows virtuele Machine (VM) in Microsoft Azure. Het bevat stappen voor het verzamelen van gegevens voor een ondersteuningsticket. 

> [!NOTE] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel wordt beschreven met behulp van de Resource Manager-implementatiemodel, dat wordt u aangeraden voor nieuwe implementaties in plaats van het klassieke implementatiemodel.

## <a name="symptom"></a>Symptoom 

Een Windows-VM niet wordt gestart. Wanneer u de schermafbeeldingen opstarten controleren in [diagnostische gegevens over opstarten](./boot-diagnostics.md), ziet u een van de volgende foutberichten weergegeven in een blauw scherm:

- onze PC is een probleem en behoeften te starten. Sommige foutinformatie alleen worden verzameld en u kunt vervolgens opnieuw.
- Uw PC is een probleem en behoeften te starten.

Deze sectie vindt u de algemene foutberichten die optreden kunnen bij het beheren van virtuele machines:

## <a name="cause"></a>Oorzaak

Er zijn mogelijk meerdere redenen als waarom u een fout stoppen krijgt. De meest voorkomende oorzaken zijn:

- Probleem met een stuurprogramma
- Beschadigde systeembestand of geheugen
- Een toepassing toegang heeft tot een niet-toegestane sector van het geheugen

## <a name="collect-memory-dump-file"></a>Een geheugendumpbestand verzamelen

U lost dit probleem, moet u eerst om te verzamelen dumpbestand voor de crash en neem contact op met de ondersteuning met het dumpbestand. Volg deze stappen voor het verzamelen van het dumpbestand:

### <a name="attach-the-os-disk-to-a-recovery-vm"></a>De besturingssysteemschijf koppelen aan een virtuele machine voor herstel

1. Een momentopname van de besturingssysteemschijf van de betrokken VM als een back-up. Zie voor meer informatie, [momentopname maken van een schijf](../windows/snapshot-copy-managed-disk.md).
2. [De besturingssysteemschijf koppelen aan een virtuele machine voor herstel](../windows/troubleshoot-recovery-disks-portal.md). 
3. Extern bureaublad op de virtuele machine voor herstel.

### <a name="locate-dump-file-and-submit-a-support-ticket"></a>Zoek dumpbestand en verzend een ondersteuningsticket

1. Op de virtuele machine voor herstel, gaat u naar de map windows in de gekoppelde besturingssysteemschijf. Als de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf F is, moet u gaat u naar F:\Windows.
2. Zoek het dumpbestand th, en vervolgens [verzend een ondersteuningsticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) met het dumpbestand. 

Als u het bestand niet vinden, verplaatst u de volgende stap om in te schakelen dump logboek- en seriële Console.

### <a name="enable-dump-log-and-serial-console"></a>Dump logboek- en seriële Console inschakelen

Om in te schakelen dump logboek- en seriële Console, voer het volgende script.

1. Open de opdrachtprompt met verhoogde bevoegdheid sessie (als administrator uitvoeren).
2. Voer het volgende script uit:

    In dit script, we gaan ervan uit dat de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf f kan zijn.  Vervang deze door de juiste waarde in uw virtuele machine.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

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

    1. Zorg ervoor dat er voldoende ruimte beschikbaar is op de schijf zo veel geheugen toewijzen als de hoeveelheid RAM, afhankelijk van de grootte die u voor deze virtuele machine selecteert.
    2. Als er onvoldoende ruimte is of dit een grote omvang virtuele machine (G, GS of E-serie is), kunt u de locatie waar dit bestand wordt gemaakt en die verwijzen naar een andere gegevensschijf die is gekoppeld aan de virtuele machine wijzigen. Om dit te doen, moet u de volgende sleutel wijzigen:

            reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

            REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
            REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f

            reg unload HKLM\BROKENSYSTEM

3. [Loskoppelen van de besturingssysteemschijf en koppel de besturingssysteemschijf voor de betrokken VM vervolgens opnieuw](../windows/troubleshoot-recovery-disks-portal.md).
4. Start de virtuele machine om het probleem te reproduceren en vervolgens een dumpbestand wordt gegenereerd.
5. De besturingssysteemschijf koppelen aan de herstel-VM, verzamelen dumpbestand, en vervolgens [verzend een ondersteuningsticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) met het dumpbestand.



