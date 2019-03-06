---
title: KRITIEKE SERVICE is mislukt bij het opstarten van een Azure-VM | Microsoft Docs
description: Informatie over het oplossen van de '0x0000005A kritieke SERVICE mislukt'-fout die optreedt bij het opstarten | Microsoft Docs
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
ms.date: 10/08/2018
ms.author: genli
ms.openlocfilehash: ffc8bd21468847a71a44be2244c72af7016cd65f
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57432019"
---
# <a name="windows-shows-critical-service-failed-on-blue-screen-when-booting-an-azure-vm"></a>Windows bevat 'Kritieke SERVICE is mislukt' in blauw scherm bij het opstarten van een Azure-VM
Dit artikel beschrijft de fout 'Kritieke SERVICE is mislukt', die optreden kunnen tijdens het opstarten van een Windows virtuele Machine (VM) in Microsoft Azure. Het biedt oplossen van problemen met stappen om u te helpen bij het oplossen van problemen. 

> [!NOTE] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel wordt beschreven met behulp van de Resource Manager-implementatiemodel, dat wordt u aangeraden voor nieuwe implementaties in plaats van het klassieke implementatiemodel.

## <a name="symptom"></a>Symptoom 

Een Windows-VM niet wordt gestart. Wanneer u de schermafbeeldingen opstarten controleren in [diagnostische gegevens over opstarten](./boot-diagnostics.md), ziet u een van de volgende foutberichten weergegeven in een blauw scherm:

- "Uw PC is bij een probleem en behoeften te starten. U kunt opnieuw. Bezoek voor meer informatie over dit probleem en mogelijke oplossingen http://windows.com/stopcode. Als u een ondersteuningsmedewerker aanroept, zodat ze deze gegevens: Stopcode: KRITIEKE-SERVICE IS MISLUKT" 
- "Uw PC is bij een probleem en behoeften te starten. Sommige foutinformatie alleen worden verzameld en start we vervolgens voor u. Als u graag meer weten, kunt u zoeken naar online later voor deze fout: CRITICAL_SERVICE_FAILED"

## <a name="cause"></a>Oorzaak

Er zijn verschillende oorzaken voor fouten stoppen. De meest voorkomende oorzaken zijn:
- Probleem met een stuurprogramma
- Beschadigde systeembestand of geheugen
- Toepassing toegang heeft tot een niet-toegestane sector van het geheugen

## <a name="solution"></a>Oplossing 

Dit probleem op te lossen [contact op met ondersteuning en het verzenden van een dumpbestand](./troubleshoot-common-blue-screen-error.md#collect-memory-dump-file), die helpt ons sneller oorzaak van het probleem of probeer de volgende Help-oplossing.

### <a name="attach-the-os-disk-to-a-recovery-vm"></a>De besturingssysteemschijf koppelen aan een virtuele machine voor herstel

1. Een momentopname van de besturingssysteemschijf van de betrokken VM als een back-up. Zie voor meer informatie, [momentopname maken van een schijf](../windows/snapshot-copy-managed-disk.md).
2. [De besturingssysteemschijf koppelen aan een virtuele machine voor herstel](./troubleshoot-recovery-disks-portal-windows.md). 
3. Tot stand brengen op een externe bureaubladverbinding met de virtuele machine voor herstel.

### <a name="enable-dump-logs-and-serial-console"></a>Dump en seriële Console inschakelen

Het logboek dump en [seriële Console](./serial-console-windows.md) kunnen we doen problemen op te lossen.

Als u wilt inschakelen en dump seriële Console, voer het volgende script.

1. Open een opdrachtprompt met verhoogde bevoegdheid-sessie (als administrator uitvoeren).
2. Voer het volgende script uit:

    In dit script, we gaan ervan uit dat de stationsletter die toegewezen aan de gekoppelde besturingssysteemschijf f kan zijn. U moet deze vervangen door de juiste waarde voor uw virtuele machine.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 10
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

### <a name="replace-the-unsigned-drivers"></a>Vervang de niet-ondertekende stuurprogramma 's

1. Op de virtuele machine voor herstel, de volgende opdracht uitvoeren vanaf een opdrachtprompt met verhoogde bevoegdheid. Met deze opdracht wordt de betreffende besturingssysteemschijf te starten in de veilige modus op de volgende keer wordt opgestart:

        bcdedit /store <OS DISK you attached>:\boot\bcd /set {default} safeboot minimal

    Als een schijf met het besturingssysteem die u hebt gekoppeld station F, bijvoorbeeld de volgende opdracht uitvoeren:

        bcdedit /store F: boot\bcd /set {default} safeboot minimal

2. [Loskoppelen van de besturingssysteemschijf en koppel de besturingssysteemschijf voor de betrokken VM vervolgens opnieuw](troubleshoot-recovery-disks-portal-windows.md). De virtuele machine wordt opgestart in de veilige modus. Als de fout zich blijft voordoen, gaat u naar de optionele stap.
3. Open de **uitvoeren** vak en voer **verifier** het hulpprogramma toepassingscontrole-stuurprogramma starten.
4. Selecteer **niet-ondertekende stuurprogramma's automatisch selecteren**, en klik vervolgens op **volgende**.
5. U krijgt de lijst met de stuurprogramma's die niet ondertekend zijn. Vergeet niet de bestandsnamen.
6. Dezelfde versies van deze bestanden van een werkende virtuele machine kopiëren en vervang deze niet-ondertekende bestanden. 

7. De instellingen voor opstarten in veilige modus verwijderen:

        bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {default} safeboot
8.  Start de VM opnieuw. 

### <a name="optional-analyze-the-dump-logs-in-dump-crash-mode"></a>Optioneel: De logboeken van de dump in de modus Crash Dump analyseren

Logboeken voor het analyseren van de dump zelf, als volgt te werk:

1. Koppel de besturingssysteemschijf aan een virtuele machine voor herstel.
2. Blader op een schijf met het besturingssysteem die u hebt gekoppeld, naar **\windows\system32\config**. Kopieer de bestanden die als een back-up als een terugdraaiactie moet uitgevoerd worden.
3. Start **Register-Editor** (regedit.exe).
4. Selecteer de **HKEY_LOCAL_MACHINE** sleutel. Selecteer in het menu **bestand** > **Component laden**.
5. Blader naar de **\windows\system32\config\SYSTEM** map op de OS-schijf die u hebt gekoppeld. Voer voor de naam van de component, **BROKENSYSTEM**. De nieuwe registercomponent wordt weergegeven onder de **HKEY_LOCAL_MACHINE** sleutel.
6. Blader naar **HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Control\CrashControl** en breng de volgende wijzigingen:

    AutoReboot = 0

    CrashDumpEnabled = 2
7.  Selecteer **BROKENSYSTEM**. Selecteer in het menu **bestand** > **component**.
8.  Wijzig de BCD-instellingen om op te starten in de foutopsporingsmodus. Voer de volgende opdrachten uit vanaf een opdrachtprompt met verhoogde bevoegdheid:

    ```cmd
    REM Setup some debugging flags on the boot manager
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {bootmgr} default {<IDENTIFIER OF THE WINDOWS BOOT LOADER>}
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {bootmgr} nointegritychecks on
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {bootmgr} integrityservices disable

    REM Setup some debugging flags on the boot loader
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} bootlog yes
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} bootstatuspolicy displayallfailures
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} nointegritychecks on
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} testsigning off
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} recoveryenabled no
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} integrityservices disable
    ```
9. [Loskoppelen van de besturingssysteemschijf en koppel de besturingssysteemschijf voor de betrokken VM vervolgens opnieuw](troubleshoot-recovery-disks-portal-windows.md).
10. Start de virtuele machine om te zien als de analyse van de dump wordt weergegeven. Zoek het bestand dat niet kan worden geladen. U moet dit bestand vervangen door een bestand van de werkende virtuele machine. 

    Hier volgt een voorbeeld van de dump analyse. U kunt zien dat de **fout** filecrypt.sys is: "FAILURE_BUCKET_ID: 0x5A_c0000428_IMAGE_filecrypt.sys".

    ```
    kd> !analyze -v 

    ******************************************************************************* 
    * * * Bugcheck Analysis * * * 

    ******************************************************************************* 
    CRITICAL_SERVICE_FAILED (5a) Arguments: Arg1: 0000000000000001 Arg2: ffffd80f4bfe7070 Arg3: ffffb00b0513d320 Arg4: ffffffffc0000428 Debugging 
    Details: ------------------ 
    DUMP_CLASS: 1 DUMP_QUALIFIER: 401 BUILD_VERSION_STRING: 10.0.14393.1770 (rs1_release.170917-1700) 
    DUMP_TYPE: 1 BUGCHECK_P1: 1 BUGCHECK_P2: ffffd80f4bfe7070 BUGCHECK_P3: ffffb00b0513d320 BUGCHECK_P4: ffffffffc0000428 BUGCHECK_STR: 0x5A_c0000428 
    CPU_COUNT: 1 CPU_MHZ: a22 CPU_VENDOR: GenuineIntel CPU_FAMILY: 6 CPU_MODEL: 3d CPU_STEPPING: 4 DEFAULT_BUCKET_ID: WIN8_DRIVER_FAULT 
    PROCESS_NAME: System CURRENT_IRQL: 0 ANALYSIS_SESSION_HOST: MININT-6RMM091 ANALYSIS_SESSION_TIME: 11-15-2017 19:32:42.0841 
    ANALYSIS_VERSION: 10.0.16361.1001 amd64fre STACK_TEXT: ffffc701`1dc74948 fffff803`b2ff4b4a : 00000000`0000005a 00000000`00000001 ffffd80f`4bfe7070 ffffb00b`0513d320 : nt!KeBugCheckEx [d:\rs1\minkernel\ntos\ke\amd64\procstat.asm @ 127] ffffc701`1dc74950 fffff803`b3205df3 : ffffd80f`4bba9f58 ffffd80f`4bba9f58 ffffc701`1dc74b80 ffffd80f`00000006 : nt!IopLoadDriver+0x19f8e6 [d:\rs1\minkernel\ntos\ke\amd64\threadbg.asm @ 81] 
    RETRACER_ANALYSIS_TAG_STATUS: DEBUG_FLR_FAULTING_IP is not found THREAD_SHA1_HASH_MOD_FUNC: eb79608c07faa1af62c0e61f25ff6bc1d6dfdb25 THREAD_SHA1_HASH_MOD_FUNC_OFFSET: 96a3a314834bb4e8443a8b7201525fc5dfc1878b THREAD_SHA1_HASH_MOD: 30a3e915496deaace47137d5b90c3ecc03746bf6 FOLLOWUP_NAME: wintriag
    MODULE_NAME: filecrypt IMAGE_NAME: filecrypt.sys DEBUG_FLR_IMAGE_TIMESTAMP: 0 IMAGE_VERSION: STACK_COMMAND: .thread ; .cxr ; kb FAILURE_BUCKET_ID: 0x5A_c0000428_IMAGE_filecrypt.sys BUCKET_ID: 0x5A_c0000428_IMAGE_filecrypt.sys PRIMARY_PROBLEM_CLASS: 0x5A_c0000428_IMAGE_filecrypt.sys TARGET_TIME: 2017-11-13T20:51:04.000Z OSBUILD: 14393 OSSERVICEPACK: 1770 SERVICEPACK_NUMBER: 0 OS_REVISION: 0 SUITE_MASK: 144 PRODUCT_TYPE: 3 OSPLATFORM_TYPE: x64 OSNAME: Windows 10 OSEDITION: Windows 10 Server TerminalServer DataCenter OS_LOCALE: USER_LCID: 0 OSBUILD_TIMESTAMP: 2017-09-17 19:16:08 BUILDDATESTAMP_STR: 170917-1700 BUILDLAB_STR: rs1_release BUILDOSVER_STR: 10.0.14393.1770 ANALYSIS_SESSION_ELAPSED_TIME: bfc ANALYSIS_SOURCE: KM FAILURE_ID_HASH_STRING: km:0x5a_c0000428_image_filecrypt.sys FAILURE_ID_HASH: {35f25777-b01e-70a1-c502-f690dab6cb3a} FAILURE_ID_REPORT_LINK: http://go.microsoft.com/fwlink/?LinkID=397724&FailureHash=35f25777-b01e-70a1-c502-f690dab6cb3a
    ```

11. Zodra de virtuele machine werkt en is niet normaal wordt opgestart, moet u de instellingen Crash Dump verwijderen:

    ```cmd
    REM Restore the boot manager to default values
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {bootmgr} nointegritychecks
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {bootmgr} integrityservices enable

    REM Restore the boot loader to default values for an azure VM
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {default} bootlog
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} bootstatuspolicy ignoreallfailures
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {default} nointegritychecks
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {default} testsigning
    bcddit /store <OS DISK LETTER>:\boot\bcd /set {default} recoveryenabled no
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} integrityservicesenable
    ```
